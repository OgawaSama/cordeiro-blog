# Próximo Projeto  
Agora que consegui enviar um PR, para onde vou? Simples! Crio outro PR! 

Claro que não vai ser para o mesmo problema, mas um novo PR vai ser necessário
caso eu pretenda fazer a matéria de verdade e não só fingir que completei o
**semestre** em uma semana de trabalho (por mais legal que isso seria...)  

Mas que PR eu faço? Por enquanto, **acho que vou continuar no GIMP**.  

Minha autoestima em relação à esses trabalhos subiu bastante quando eu consegui
resolver um problema e publicar o PR, especialmente com o quão simples foi! Mas,
mesmo assim, ainda acho fora do meu nível tentar alguns outros projetos que eu
havia separado no início desses posts.  

> Se tudo der certo, vou tentar algum patch para Bloodborne ou Final Fantasy XIV  

## Buscando mais Issues fazíveis  
Voltamos, então, à [aba de Issues Abertos](https://gitlab.gnome.org/GNOME/gimp/-/work_items).  
Grande problema é o mesmo que havia semana retrasada: Qual issue aqui é fácil
suficiente para mim?  

Apesar de eu estar mais confiante, ainda estou ciente do fato que alguns issues
são definitivamente difíceis demais.  

Um que me chamou atenção foi um relacionado à ferramenta Select, mais
especificadamente, quando no formato de elipse.  

**O issue pode ser encontrado
[aqui](https://gitlab.gnome.org/GNOME/gimp/-/work_items/16565)**.  

## Especificando o Issue 
O problema descrito é bem simples:  
* No Windows 11,
* Quando o Select Ellipse é usado e SHIFT/CONTROL é segurado,
* As proporções podem ficar 1px erradas (e.g. 450x451)

Olhando os comentários do issue, também é informado que o problema ocorre quando
a opção "Fixed Aspect ratio" é ativada (e então SHIFT/CONTROL podem serem soltos). 
Além disso, o problema também existe em Linux Mint.  

Bem simples!  

Com alguns testes básicos na versão mais recente do GIMP em meu desktop (Arch),
pude perceber que o issue ainda existe e é bem o que disseram: 1px de diferença
*às vezes*.  

Também testei o Select retangular e o mesmo erro existe, então o problema deve
estar em alguma função em comum.  

## Começando a trabalhar 
Felizmente, meu projeto anterior também se tratava de um bug nas ferramentas,
então estou um pouco acostumado com como elas funcionam no GIMP.  
Minha estratégia para esse novo issue consiste em:  
1. Checar os códigos das ferramentas em `app/tools`;  
2. Checar as outras funções que são chamadas;  
3. Testar algumas pequenas mudanças na minha versão local;  
4. Se der errado, voltar ao 1.

## Iterações da resolução 
Como expliquei acima, o problema provavelmente está em alguma função mais
abrangente do que simplesmente "ellpise-select-tool.c". O que eu não esperava,
entretanto, é que o problema não é local somente aos Selects.  

Passei um bom tempo explorando todas as funções relacionadas a uma possível
aparição de "Fixed aspect ratio" nos comentários ou alguma linha que indicasse
isso, mas não encontrei nada. Foi aí que me veio a ideia de que possivelmente o
mesmo erro ocorre em outras ferramentas.  

Meu primeiro chute foi alguma ferramenta de criar figuras, como círculos e
quadrados, mas o GIMP não possui isso nativo. Meu próximo chute foi na
ferramenta de Crop, e voilà! Fixed Aspect Ratio existe como opção e o mesmo erro
ocorre!  

Isso significa duas coisas:
1. Estou mais perto de resolver o problema;
2. Ele é maior do que eu imaginava.

## Código suspeito!
*ACHO* que encontrei onde preciso modificar para arrumar o erro.  
No arquivo `app/tools/gimptoolrectangle.c`, dentro da função
`gimp_tool_rectangle_apply_fixed_rule()`, temos a seguinte seção:  

```C
// ...
if (private->fixed_rule_active &&
      private->fixed_rule == GIMP_RECTANGLE_FIXED_ASPECT)
    {
      gdouble aspect;

      aspect = CLAMP (private->aspect_numerator /
                      private->aspect_denominator,
                      1.0 / gimp_image_get_height (image),
                      gimp_image_get_width (image));

// ...
```  

Fiz um pequeno teste adicionando `aspect = 2.0;` após essa primeira definição
apenas para checar se de fato era esta linha que afetava tanto o Select quanto o
Crop e... sim! É aqui mesmo.  

O problema agora é descobrir como arrumar essa definição SEM quebrar outras
funcionalidades e SEM ser muito feio (adicionar um "se aspect ratio for 1:1, não
errar por 1px" é sacanagem)  

Não consegui encontrar a definição da função `CLAMP()` aqui usada, mas estou
assumindo ser a mesma que aquela definida em std do C++, [disponível aqui](https://en.cppreference.com/cpp/algorithm/clamp).  

Se esse for o caso, meu problema provavelmente se encontra na divisão de 1.0 por
image_height... o que é um problemão. Se resolver essas divisões de float fosse
fácil, milhões de problemas de computação nem existiriam.  

Aqui eu não posso editar a função CLAMP (obviamente) e eu não quero um fix tosco
como o exemplo que eu dei um pouco acima. Eu preciso pensar em algo mais sólido
e elegante... tarefa que acaba ficando para o próximo post.

Mas acredito que esse é o caminho certo! A parte boa desse problema é que ele é
bem desconectado do restante do código do GIMP, então não preciso ficar no
computador o tempo inteiro para resolvê-lo; posso ter ideias no papel e longe de
casa.  

Vamos ver o que acontece daqui pra frente.

# E o PR antigo? Como está?
Por enquanto, nada.  

Os moderadores do gitlab do GIMP perceberam meu PR,
comentaram um pequeno fix (como mencionei no post anterior) e ficou por aí. Eles
alteraram a milestone relacionada ao meu PR, então imagino que esteja na mesa de
trabalho de alguém esperando para ser checado.  

Se esse for o caso, estou feliz! Mesmo que não seja aprovado, foi algo! (￣▽￣)  

Quando o PR for aprovado ou rejeitado, eu escreverei no blog aqui. Enquanto
isso, assuma que nada mudou.


