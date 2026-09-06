# Onde está o erro?
Essa é a primeira semana em que eu consegui, de fato, começar a trabalhar em
resolver algum Issue e abrir arquivos de código.  
Como mencionei em meu último post, decidi trabalhar com o
[GIMP](https://www.gimp.org/).  

Apesar de existir a aba de [Issues para
Iniciantes](https://gitlab.gnome.org/GNOME/gimp/-/work_items?label_name%5B%5D=4.%20Newcomers&state=opened), todos os issues que encontrei eram complicados!  
Minha suspeita é que os issues que restam ainda abertos são aqueles que ninguém
conseguiu resolver. Meu melhor exemplo é
[este](https://gitlab.gnome.org/GNOME/gimp/-/work_items?sort=created_asc&state=opened&label_name%5B%5D=4.+Newcomers&first_page_size=20&show=eyJpaWQiOiI5NSIsImZ1bGxfcGF0aCI6IkdOT01FL2dpbXAiLCJpZCI6MzY1NDN9)
que foi criado em 2004. Se não foi resolvido em mais tempo do que eu existo, duvido muito que eu vá
conseguir resolver esse enigma em 4 meses.  
Com essa quebra de expectativas, eu decidi usar a [lista de issues
geral](https://gitlab.gnome.org/GNOME/gimp/-/work_items/?sort=created_asc&state=opened&first_page_size=20)
e procurar algum interessante e não tão complexo.  

Minha primeira ideia foi checar os issues abertos recentemente, assim não teria
como eu pegar um aberto há uma década e ainda não resolvido.  
Encontrei rapidamente este: [Bucket fill on a layer with an un-merged filter
displays incorrectly](https://gitlab.gnome.org/GNOME/gimp/-/work_items/16718).  
O issue consiste em aplicar um filtro como "Colorize" a uma layer e depois usar
o balde. A preview da layer demonstra o resultado correto enquanto a imagem
principal não. Recarregar a imagem principal ao desligar e ligar sua
visibilidade conserta o problema.  

Parece simples! Fiquei animado em encontrar algo que eu consiga fazer e criei
minha branch local.  
Agora vem um pequeno (grande) problema: **onde está o erro?**

# Hipóteses
Primeiro, preciso pensar em qual etapa do processo isso ocorre.  
Seria na geração de thumbnail? Na aplicação do filtro? No próprio balde?  
Minha ideia principal é a seguinte:  
* Ao aplicar o balde, a imagem principal gera o resultado **antes** de verificar
  por filtros un-merged. Enquanto isso, a thumbnail segue o processo esperado e
  gera o resultado correto.  

Mas seria **só** o balde que causa isso? Será que outros efeitos ou tipos de
baldes causam o mesmo erro ou é apenas com os valores *default* que isso ocorre?  
Uma testagem rápida revelou que:
* Apenas o balde causa o erro; Ferramentas como lápis, estampa, pincel não causam.  
* Quase todos os modos do balde causam o erro; Exceção é o modo "Luminance".  
* Todos os filtros do balde causam o erro.  
* Ao escolher a área afetada como "Fill whole selection", o erro desaparece.  
* Nenhuma das opções em "Finding similar colors" corrige o erro.  
* Ao marcar "Merge layers" antes de aplicar o filtro, o erro não ocorre.  

Agora, com isso em mente, preciso descobrir como achar o código culpado pelo crime.  
Como não tenho experiência em editar o código fonte do GIMP (quem diria?),
muitos `CTRL+SHIFT+F` e visitas às [wikis](https://developer.gimp.org/api/3.0/)
serão feitos.  

# Busca iterativa
## GEGL 
[GEGL](https://www.gegl.org/) é o framework que o GIMP usa para processamento de
imagem e, segundo o
[guia](https://docs.gimp.org/3.2/en/gimp-colors-menu.html#colors-common-features):
>  # Merge filter
>    By default, GEGL filters are applied non-destructively as layer effects, which means they can still be changed at a later time. When you want to apply the filter immediately to the layer itself, you can enable this option.   

Logo, imagino que a biblioteca talvez tenha culpa nesse bug.  
Por sorte, o código fonte de GEGL também é editável, então se esse for o caso,
ainda posso contribuir.  

## GIMP
Dentro do GIMP, meu alvo de suspeita são dois:
* Como ele processa/chama os filtros antes de dar merge;
* Como o balde aplica seus efeitos.  

Eu fiz uma busca atrás de como ele chama os filtros, mas infelizmente não
descobri muito. Após exaustar minhas outras alternativas, planejo voltar aqui.  
Meu foco está no Balde. Tudo me indica que ele é o maior culpado: seja se for no
método em que ele é chamado, ou se for em como aplica ou até mesmo em como gera
a thumbnail.  

## Bucket-tool
Eu acho que encontrei a solução!! No arquivo `gimpbucketfilltool.c`, a função
```c
static gboolean
gimp_bucket_fill_tool_coords_in_active_pickable (GimpBucketFillTool *tool,
                                                 GimpDisplay        *display,
                                                 const GimpCoords   *coords)
{
    //...
    switch (options->fill_area)
    {
    case GIMP_BUCKET_FILL_SELECTION:
      break;

    case GIMP_BUCKET_FILL_SIMILAR_COLORS:
      sample_merged = options->sample_merged;
      break;

    case GIMP_BUCKET_FILL_LINE_ART:
      sample_merged = options->line_art_source ==
                      GIMP_LINE_ART_SOURCE_SAMPLE_MERGED;
      break;
    }
    //...
``` 

Tem esse *switch* que, apenas no caso "Fill Selection", **não** é colocado algo
em "sample_merged" e, por acaso, "Fill Selection" é o único modo em que o erro
não acontece. Minha suspeita é que esse sample_merged é o problema.

--- 
Infelizmente, não era :(  
Eu testei simplesmente comentar as seções "sample_merged" para ver se o erro
sumiria (em tese, agiria igual o FILL_SELECTION). Nada aconteceu. Hora de
procurar outra seção de código 

--- 
**ENCONTREI!!!**
No mesmo arquivo, dentro da função `gimp_bucket_fill_tool_button_press()`, há a
seção:
```C
    //...
    if (options->fill_area == GIMP_BUCKET_FILL_SELECTION)
    {
      gimp_drawable_edit_fill (drawable, fill_options, NULL);
      gimp_image_flush (image);
    }
    else /* GIMP_BUCKET_FILL_SIMILAR_COLORS || GIMP_BUCKET_FILL_LINE_ART */
    {
      gimp_bucket_fill_tool_start (bucket_tool, coords, display);
      gimp_bucket_fill_tool_preview (bucket_tool, coords, display,
                                         fill_options);
    }
    //...
```

Ao substituir a seção do `else` pelo conteúdo da seção `if`, os três modos
(Selection, Similar Colors, Line Art) agem corretamente! (Fora, claro, as
funcionalidades que eu removi...)  
Okay! Sabemos um ponto crítico do código! **É por aqui que o erro acontece.**  

# Resolvendo o issue
## Investingando as funções
Primeiro, fui investigar como a função `gimp_drawable_edit_fill` age.  
Ao acessar o arquivo `app/core/gimpdrawable-edit.c`, a função faz o seguinte
(linhas 159~253):  
* Checa diversos pontos de falha e encerra preventivamente;
* Checa se a região a ser preenchida está dentro da máscara;  
* Checa se a camada está no mode Alpha Only;
* Se puder preencher diretamente, atualiza a bounding box e chama
  `gimp_drawable_update`;  
* Senão, gera um novo filtro, aplica ele com `gimp_drawable_filter_apply` e atualiza o drawable via
  `gimp_drawable_commit`.  

Okay. As três primeiras etapas me parecem um pouco triviais para o problema que
enfrentamos aqui, mas a etapa final de chamar `gimp_drawable_update` ou
`_commit` me chamou atenção. Parece exatamente o que eu pensei ser: atualização
esquecida antes de encerrar o fill, que então só ocorre após atualizar
manualmente a camada.  

Agora vamos analizar o que acontece nas funções dentro do else,
`gimp_bucket_fill_tool_start` e `gimp_bucket_fill_tool_preview`.  
Um skim rápido por `_start` foi suficiente para perceber que ele apenas
inicializa algumas variáveis e nada mais. Enquanto isso, `_preview` tinha algo
interessantíssimo:
```c
//...
// linhas 484~495
    if (fill)
        {
        gegl_node_set (tool->priv->fill_node,
                     "buffer", fill,
                     NULL);
        gegl_node_set (tool->priv->offset_node,
                     "x", x,
                     "y", y,
                     NULL);
        gimp_drawable_filter_apply (tool->priv->filter, NULL);
        g_object_unref (fill);
        }
//...
```

Você conseguiu pereceber algo ali? Isso mesmo! **Não há `gimp_drawable_commit`
após o `_apply`!**  
Será que vai ser fácil assim mesmo de resolver esse bug? Só há um jeito de
saber: testando.  

## Testando soluções
Meu primeiro teste foi simples: adicionar o `_commit` após o `_apply`, exatamente
idêntico àquele dentro de `gimp_drawable_edit_fill`.  
Funcionou! ...Quase. Alguns pequenos testes manuais revelaram alguns pequenos
bugs como o balde não sendo sempre consistente, então resolvi copiar mais
algumas linhas do outro arquivo, i.e., adicionei uma variável `filter_stack` e uma simples *if* para
reordenar a stack dos filtros (mover o novo para o fundo) antes do `_commit`.  

Se tudo isso ficou confuso, não se preocupe. Não é você, sou eu.  
Mas ao ler o código abaixo, você deve entender um pouco melhor do que foi dito:  
```c 
//... 
//nova versão
if (fill)
    {
//// VARIÁVEL NOVA
      GimpContainer          *filter_stack;
////
      
      gegl_node_set (tool->priv->fill_node,
                     "buffer", fill,
                     NULL);
      gegl_node_set (tool->priv->offset_node,
                     "x", x,
                     "y", y,
                     NULL);
//// SEÇÃO NOVA
      gimp_drawable_filter_apply (tool->priv->filter, NULL);
      /* Move to bottom of filter stack */
      filter_stack = gimp_drawable_get_filters (drawable);
      if (filter_stack)
        gimp_container_reorder (filter_stack, GIMP_OBJECT (tool->priv->filter),
                                gimp_container_get_n_children (filter_stack) - 1);

      gimp_drawable_filter_commit (tool->priv->filter, FALSE, NULL, FALSE);
////
      g_object_unref (fill);
    }
//...
```

## E aí?
Deu certo? Não deu?  
...  
...  
...  
**Deu tudo certo!**  
Mal consigo acreditar que consegui resolver um bug tão rapidamente e sem me
estressar (tanto)! Definitivamente ainda preciso melhorar muito no meu processo
de debugging, mas baixar algumas ferramentas e customizar melhor meu
[NeoVim](https://neovim.io/) e meu [VSCodium](https://vscodium.com/) deve ser
suficiente para consertar esses atrasos.  

Num geral, estou muito feliz!  
O que falta agora é meu Merge Request ser aceito!  

# Pushing upstream 
Essa é a parte mais crucial de todas as partes cruciais já cruciadas: enviar o
commit para o repositório oficial.  
Novamente, a wiki vem para salvar com um [guia de como submeter
patches](https://developer.gimp.org/core/submit-patch/). Super simples e nada
desafiador, até mesmo para mim :p  
Na realidade, foi TÃO simples, que eu dei o "Create merge request" final antes
do que eu imaginava e *boom*, meu pedido estava lá.  

Você pode acessar meu pedido
[aqui](https://gitlab.gnome.org/GNOME/gimp/-/merge_requests/2990). Ele está sob
um pseudônimo meu.  

Espero que seja aprovado rapidamente! Apesar de não ser um bug crucial, seria
legal ser resolvido rápido :v  
Ah, e por enquanto ele está marcado com um erro: "*commit message subject must
not exceed 80 characters*". Eu renomeei o commit e separei o texto em linhas de
<80 chars, mas não parece ter resolvido? Vai saber.

Enfim, esse foi meu primeiro Merge Request em um projeto Open Source!  
