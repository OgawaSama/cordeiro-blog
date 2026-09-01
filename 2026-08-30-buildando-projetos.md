# Foi decidido algo?
**Tl;dr: sim, GIMP.**  
Passei esta segunda semana consertando meu computador apos uma instalacao falha de [Arch](https://archlinux.org/) (me perdoem inclusive pela falta de acentuacao, nao estou com um teclado abnt~).   
Assim que postei (ou ao menos tentei postar multiplas vezes, ate desistir e enviar o post via email para o professor), salvei um backup dos meus arquivos mais importantes, fui para a [aba de Downloads da wiki do Arch](https://archlinux.org/download/) e instalei — pelo que deve ser minha decima vez em um ano — a ISO mais recente e coloquei em um pendrive.  

A instalacao em si foi facil: sofri tanto no passado que ja tenho um papel na minha escrivaninha com as instrucoes especificas para minha maquina.   
O problema mesmo foi comecar a usar Arch e Hyprland sem um template como das outras vezes. Meu deus como eh dificil configurar o proprio setup!! Nunca imaginei que eu ficaria acordado ate as 6 da manha programando um script de Python para simplesmente mudar meus wallpapers dinamicamente (Em minha defesa, eu nao sabia muito alem de "Hello, world!" em Python antes de tentar isso).  

O resultado foi que atualmente eu tenho um setup decentemente completo, com apenas algumas pequenas coisas nao-essenciais me restando (auto-boot do hyprland, barra de tarefas, etc).  

Agora com um setup bom, eu pude dedicar mais tempo para o projeto da disciplina!  
Ja que minhas opcoes, segundo meu ultimo post, eram entre um programa que precisava de Windows para contribuir (Dalamut) e outro que nao (e eu **acabei** de instalar meu Linux), escolhi a segunda opcao: GIMP!  

## GIMP
"Como eu posso ajudar no GIMP?", de long, eh o que eu mais me preocupo neste momento. Como ja expliquei (cansativamente), eu nao tenho experiencia na area! Felizmente, GIMP oferece a [lista de Issues abertos para iniciantes](https://gitlab.gnome.org/GNOME/gimp/-/work_items?label_name%5B%5D=4.%20Newcomers&state=opened).   

Mesmo com ela em maos, eu senti certa ansiedade na hora de escolher qual topico contribuir. A maioria dos que eu chequei estao abertos ha quase um ano! Como que *eu* vou resolver isso, sem experiencia alguma? Caramba!  
Mesmo assim, eu preciso tentar.  

## Inicializando meu ambiente de trabalho
Antes de tudo, eu preciso preparar meu computador com as dependencias e pastas necessarias.  
Assustadoramente, GIMP nao eh complexo para montar a Source. Basta seguir o [guia dedicado](https://developer.gimp.org/core/setup/) na wiki, que consiste em (basicamente):
* Clonar os repositorios (3 deles);
* Instalar [Meson](https://mesonbuild.com/) e [Ninja](https://ninja-build.org/) para seu sistema;
* rodar `ninja install` nas pastas `_build` respectivas.

So isso? Sim! Tambem me assustei com o quao facil foi fazer isso!  
Com isso feito, decidi ir dormir. Afinal, tinha sido um dia produtivo: decidi qual projeto usar, preparei as dependencias e deixei o setup pronto. Basta encontrar uma Issue que me interesse e que seja do meu nivel e comecar a trabalhar nela. ***Nada pode dar errado!***  

## Tudo deu errado
No dia seguinte, eu decidi seguir um pouco do meu setup do computador. Salvei meus `dotfiles` em um repositorio e achei que estava pronto para continuar.  
A proxima etapa que eu queria fazer era instalar Windows 11 no meu SSD de 256GB (optaria pelo 10, mas a [Microsoft encerrou o suporte para ele](https://support.microsoft.com/en-US/Windows/Deployment/Updates-Lifecycle/windows-10-support-has-ended-on-october-14-2025) e eu prefiro evitar virus quando possivel).  

Simples o suficiente, imaginei. *Ah, como eu estava errado.*  
Primeiro sofri para conseguir instalar a ISO em um pendrive. Os malditos da Microsoft decidiram infernizar minha vida e nao basta a ISO pesar 8GB, ela nao queria ser utilizavel no meu pendrive de forma alguma!! Eu tentei seguir o que a [wiki do Arch diz](https://wiki.archlinux.org/title/USB_flash_installation_medium), usando `cp` ou `dd`, mas alem de ser terrivelmente demorado (gracas ao meu pendrive velho), nao funcionou. Por alguma razao divina ou infernal, quando eu tentava iniciar o computador pelo pendrive, a tela de instalacao do Windows aparecia, mas completamente vazia e sem opcoes.  

Foi **sofrido** resolver isso. Minha primeira ideia foi bootar meu Arch e arrumar o pendrive. Mas quando eu fui instalar o windows, eu desconectei meus drives para, caso o Windows decidisse formatar meu sistema durante a instalacao, eu nao perdesse tudo que fiz.  
Assim que eu reconectei os drives para usar o Arch, nao havia sistema operacional...? Por algum motivo, os caminhos de boot que o GRUB usava sumiram. Passei umas boas 6 horas tentando de tudo (varias tentativas incluiram eu esquecer uma linha ou outra e ter que reiniciar a instalacao do Arch de novo), ate que enfim, consegui acessar o Arch.  

Mas, mesmo com o Arch funcionando normalmente, eu nao conseguia resolver a ISO. Repeti o processo de *desconectarDrive-tentarWindows-reconectarDrive-repararArch-repararISO* varias e varias vezes, ate que eu desisti e procurei pro algum laptop com Windows na minha casa e usei ele pra instalar Ventoy e a ISO. No fim, deu certo.  

Conectei o pendrive e instalei Windows 11. Ah, que sistema **ruim**.  

Como que toda opcao que eles oferecem parece ou predatoria ou simplesmente ruim? Nao, eu nao quero usar Copilot na minha pasta Home. Nao, eu nao quero assinar o Office365 por apenas R$509,00/ano. Nao, eu nao quero ativar as dicas da Microsoft. Por favor, me deixem instalar o sistema vazio!!  

Depois de clicar em "nao" varias vezes, rodar [script para debloat](https://github.com/Raphire/Win11Debloat), ativar Windows via powershell e customizar algumas configuracoes (voce acredita que trocar a saida de audio padrao eh presa atras da paywall da Microsoft?? Eu nao sabia! Meu controle conectou ao computador e eu nao conseguia ouvir nada pois o Win11 decidiu que minha saida padrao deveria ser pelo minusculo speaker do controle)  

Enfim, Windows 11 baixado. Apenas me levou dois dias inteiros!

## E o projeto?
Nao me sobrou tempo para comecar o projeto efetivamente, mas durante as minhas pausas para desestressar dos erros de instalacao do arch e do processo normal de instalacao da Praga 11, chequei [o guia de Estilo de Codigo](https://developer.gimp.org/core/coding_style/). Varias das regras sao boas e comuns, mas algumas delas me doem de usar. Como assim "espaco apos o nome da funcao"? Fica tao feio chamar as funcoes! `function (args)`? Que horror?   
Bom, eu nao tenho muita escolha, entao vamos so aceitar.  

Para facilitar minha vida, editei meu NeoVim para usar essas configuracoes como default e encerrei pela semana.  

Pretendo comecar a programar (ou ao menos a ler a documentacao relacionada de) algum issue especifico naquela lista.  

**Tl;dr:** Instalei GIMP, sofri com instalacao de Arch e Windows 11, li as regras de codigo do GIMP.
