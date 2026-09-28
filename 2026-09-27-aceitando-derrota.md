# Como assim "Aceitando Derrota"???
É, infelizmente esse é o título do post mesmo.  
Tentei aprofundar mais nesse PR mas cheguei a conclusão que seria melhor deixar
esse bug pequeno existindo do que alterar algo que quebre muitas outras funções.  

O porquê? Bem simples: A função que causa o erro de arredondamento na divisão
por float é uma função fundamental do GIMP (para cálculo de retângulos) e o
único *fix* que eu consegui pensar seria como colocar um band-aid em uma
represa.  

Basicamente, minha ideia seria um "if aspect-ratio == 1:1, não errar divisão",
mas isso deixaria o código horrivelmente feio (com esse if aleatório no meio de
código complexo e muito mais profissional) e seria uma solução muito temporária
para realmente ser implementada no GIMP real.  

Seria quase como fazer um PR só para dizer que fiz; sem resolver nada de
verdade.  

## E para onde vamos então?
Temos dois caminhos aqui:  
* Procurar outro problema no GIMP;
* Procurar outro problema em algum outro lugar.

Minha prioridade, claro, é o GIMP.  
Manter nesse projeto por pelo menos mais um PR.  

Mas, outros projetos também são interessantes! Principalmente algo relacionado a
Bloodborne ou FFXIV (*talvez* sejam meus favoritos no momento :p )!  

Ou, talvez, [LibreOffice](https://www.libreoffice.org/)!  
Eu fiz alguns slides essa semana para minha apresentação no
[Siicusp](https://prpi.usp.br/siicusp/) (inclusive que será logo logo!) e
percebi um pequeno bug na hora de apresentar os slides e achei interessante
aprofundar nele.  

Basicamente, um dos itens dos slides (o contador de slides) deslocava para fora
de posição quando entrava em tela cheia. Eu imagino que isso esteja relacionado
ao meu monitor ser Ultrawide e o código não contabilizar isso.  

Se for o caso, teremos um PR legal aí! :)))  

## Então essa semana ficou curta assim?
**Poisé, ficou.**  

Não vou incluir aqui o aprofundamento que eu fiz no PR dos pixels errados porque
não levou a lugar nenhum, então seria uma perda de tempo (acredito eu).  

Não foi tão interessante também; Foi basicamente explorar mais a fundo as
funções e tentar bolar algum plano elegante para resolver e... não foi
resolvido, então não houve final feliz :(  

---
## Vamos ver até onde chegamos no próximo post então
até~  
