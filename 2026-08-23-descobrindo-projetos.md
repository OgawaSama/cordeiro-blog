# Introdução
Nesta primeira semana, foquei em tentar descobrir com quais projetos eu realmente conseguiria contribuir algo. Dada a minha inexperiência em projetos fora daqueles EPs da faculdade, acredito que seja melhor evitar projetos muito complexos como a Kernel do Linux ou aqueles de áreas impossíveis para mim, como cripto-segurança. A última coisa que queremos é ficar enviando PR's inúteis e tomando tempo dos mantedores dos projetos. O objetivo aqui é contribuir.

Com isso, vamos lá com os meus
# Critérios para Projetos
* 7 meses de existência + atividade recente (queremos passar na matéria né);
* C++ ou C (nada de Java. Odeio Java);
* Algum uso para mim (eu gostaria de contribuir para algo que eu conseguisse usar! Ou que eu tenha o mínimo de interesse, claro)
* Comunidade não-tóxica (já que sou iniciante, vamos evitar grupos elitistas, né?)
* Não é muito complexo (senão eu vou passar o semestre todo tentando entender a documentação e não conseguir progresso algum).

Acho que não são critérios muito exigentes e facilitam um pouco na hora de decidir se o projeto que eu encontrar é viável para mim ou não.  
Eu encontrei alguns que me pareceram legais de trabalhar, ou ao menos considerar:

* [XIVLauncher](https://github.com/goatcorp/FFXIVQuickLauncher) — Launcher customizado para [Final Fantasy XIV](https://store.steampowered.com/app/39211/FINAL_FANTASY_XIV_Online/);
* [Dalamud](https://github.com/goatcorp/Dalamud)  — Framework para desenvolvimento de plugins para Final Fantasy XIV/
* [GIMP](https://github.com/GNOME/gimp) — É o GIMP.


Primeiro vamos ""adressar"" o elefante na sala: sim, ⅔ dos projetos são relacionados a FFXIV. Esse jogo tomou completamente minha vida nos últimos meses e acho que seria legal contribuir para a comunidade, especialmente em projetos que eu já uso há algum tempo. Além disso, a comunidade é super amigável e então imagino que os devs desses projetos também serão!

*Did you know that the critically acclaimed MMORPG Final Fantasy XIV has a free trial, and includes the entirety of A Realm Reborn, Heavensward, Stormblood AND the award-winning Shadowbringers expansion up to level 80 with no restrictions on playtime? Sign up, and enjoy Eorzea today! https://secure.square-enix.com/account/app/svc/ffxivregister?lng=en-gb*

Agora vamos analisar melhor cada um dos projetos que temos.
# Projetos encontrados
## XIVLauncher
[XIVLauncher](https://goatcorp.github.io/) é um launcher customizado de FFXIV com o objetivo de facilitar e agilizar o login no jogo (que por algum motivo requer sua senha toda vez que o launcher oficial é aberto). Além de acelerar esse processo, ele também perimite o uso de plugins e addons para o jogo base!  
XIVLauncher está disponível para Windows, Linux e Steam Deck! Sem ele, eu não conseguiria jogar esse jogo lindo no meu novo laptop com linux. Devo muito à ele.  
Infelizmente, não parece ser tão fácil de contribuir para esse projeto, então talvez não seja essa a minha escolha principal.  
## Dalamud
[Dalamud](https://dalamud.dev/) é uma framework para desenvolvimento de plugins no FFXIV. Desenvolvido pelo mesmo grupo que o XIVLauncher, Dalamud pode ser instalado ao optar por "sim" durante a instalação inicial do launcher.  
Diferente do launcher, aqui parece ser super simples de começar a contribuir! A própria [wiki](https://dalamud.dev/building/)  fornece um guia de como buildar o projeto e começar a desenvolver. Além disso, o discord deles parece ser bem organizado e fácil para pedir ajuda (muito importante.)  
Entretanto, a wiki recomenda usar Visual Studio 2027 (o roxo) e Windows (que será desinstalado logo após publicar esse diário :p ). Então, se eu for seguir por esse caminho, alguns sacrifícios (usar Windows 11) terão de ser feitos...  
## GIMP
[GIMP](https://www.gimp.org/) é o GIMP. Qualquer um que já teve um amigo usuário de Linux já ouviu sobre o GIMP e como ele é uma alternativa melhor (e gratuita!) ao Photoshop, além de ser Open-Source, disponível em várias plataformas, mais ferramen- Okay, deu pra entender. GNU Image Manipulation Program é exatamente isso que seus fanboys dizem ser: Photoshop mas open-source e melhor.  
GIMP está disponível para Linux, Windows e macOS. Mesmo para mim, com [Clip Studio Paint](https://www.clipstudio.net/en/) comprado e usado frequentemente, GIMP é uma maravilha. Seus comandos são fáceis de aprender, a UI é bonita (mas teve uma época que ela não era tão boa assim) e ele sempre salva minha vida quando eu estou no Linux e não no Windows (o que vai acontecer com cada vez mais frequência).  
Além de todas essas perfeições que descrevi, GIMP também tem um [guia de como desenvolver patches](https://developer.gimp.org/core/submit-patch/) e uma [lista de issues abertos e beginner-friendly](https://gitlab.gnome.org/GNOME/gimp/-/work_items?label_name%6B%5D=4.%20Newcomers&state=opened)! De longe, me parece um dos melhores projetos para um completo iniciante (eu) nesse tipo de tarefa.  

## Menções Honrosas: BB-Launcher e ReShade
[BB-Launcher](https://github.com/rainmakerv4/BB_Launcher) é um launcher customizado de [shadPS4](https://shadps4.net/) (emulador de Playstation 4) com foco inteiramente em [Bloodborne](https://en.wikipedia.org/wiki/Bloodborne), um dos meus jogos favoritos. Infelizmente, mesmo sendo lançado ONZE ANOS ATRÁS, Bloodborne ainda é um exclusivo do PS4 e jogar no PC (oficialmente) é impossível. A comunidade do jogo tem tentado desenvolver uma versão jogável e estável há um longo tempo já e BB-Launcher parece ter resolvido vários dos problemas.  
Entretanto, não acho que eu esteja tão pronto para ajudar como num projeto desses... ainda.  

[ReShade](https://reshade.me/) é um injetor de post-processing para jogos. Serve para deixar jogos bem mais bonitos ao aplicar efeitos diferentes na pipeline gráfica. Eu o uso bastante quando jogo FFXIV e por isso chamou minha atenção. Infelizmente, acho que é complexo demais para mim agora, mas espero conseguir algum dia contribuir em algo ou ao menos desenvolver meus próprios addons :)  

# Veredito
XIVLauncher não parece tão fácil de contribuir, Dalamud tem uma comunidade amigável e uma wiki dedicada e GIMP tem... tudo que eu poderia sonhar para um projeto desses. Devido ao meu leve viés para FFXIV, Dalamud ainda é uma opção para mim.   
Nesta próxima semana vou explorar melhor as documentações dessas duas opções e tentar começar algum projeto. Se tudo der certo, tudo dará certo :)  

Infelizmente (eu realmente preciso de algum novo sinônimo né?)  
Desventuradamente, não consegui muito progresso na disciplina nessa semana inicial. Estive ocupado com outros projetos pessoais (que vão ajudar nessa disciplina, já que são relacionados a aprender C++), além de estar finalmente (!!!) trocando para uma setup com Linux na partição primária e Windows em um drive isolado (o oposto do setup que uso atualmente). Não só vou ter que me acostumar com o dia a dia somente em Linux, mas já que decidi usar Hyprland e NÃO usar o mesmo [projeto de setup que eu usei anteriormente](https://hydeproject.pages.dev/), vou precisar de muita paciência e madrugadas para isso.  
Ah e se você estiver se perguntando o porquê de não usar Clip Studio Paint no Linux, se eu já até mesmo paguei pelo programa, é bem simples: Não tem versão oficial para Linux e as alternativas que eu encontrei sempre funcionavam só parcialmente.  

PS: Se alguém estiver lendo isso e quiser alguma sugestão de projeto para contribuir, arrume o Moodle, por favor. Tive que reescrever este texto duas vezes porque ele decidiu simplesmente deletar tudo :)   
PPS: Duas semanas depois, ainda não consigo postar no Moodle. Tentei via Windows, Linux, Android, VPN, 5G, com/sem hyperlink, nada funciona. Agora esse post vai estar em uma página do meu github em formato `.md` e, se eu conseguir o que planejo, também no meu próprio blog!  
