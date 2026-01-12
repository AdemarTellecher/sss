# sss (Shamir's Secret Sharing)

[![Build Status](https://travis-ci.org/codahale/sss.png?branch=master)](https://travis-ci.org/codahale/sss)

A pure Go implementation of
[Shamir's Secret Sharing algorithm](http://en.wikipedia.org/wiki/Shamir's_Secret_Sharing)
over GF(2^8).

Inspired by @hbs's [Python implementation](https://github.com/hbs/PySSSS).

For documentation, check [godoc](http://godoc.org/github.com/codahale/sss).


## Da Wikipédia, a enciclopédia livre
(Redirecionado de Shamir's Secret Sharing)
O compartilhamento de segredos de Shamir (SSS) é um algoritmo eficiente de compartilhamento de segredos para distribuir informações privadas (o "segredo") entre um grupo, desenvolvido pela primeira vez por Adi Shamir em 1979. O segredo não pode ser revelado a menos que um número mínimo de membros do grupo aja junto para unir seu conhecimento. Para isso, o segredo é matematicamente dividido em partes (as "ações") das quais o segredo só pode ser remontado quando um número suficiente de ações for combinado. O SSS possui a propriedade de segurança baseada na teoria da informação, o que significa que, mesmo que um atacante roube algumas ações, é impossível para ele reconstruir o segredo a menos que tenha roubado um número suficiente de compartilhamentos.

O compartilhamento de segredos do Shamir é usado em alguns aplicativos para compartilhar as chaves de acesso a um segredo mestre.


## Explicação de alto nível 
O SSS é usado para proteger um segredo de forma distribuída, na maioria das vezes para proteger chaves de criptografia . O segredo é dividido em vários compartilhamentos, que individualmente não fornecem nenhuma informação sobre o segredo.

Para reconstruir um segredo protegido pelo SSS, são necessários vários compartilhamentos, chamados de limite . Nenhuma informação sobre o segredo pode ser obtida de qualquer número de compartilhamentos abaixo do limite (uma propriedade chamada sigilo perfeito ). Neste sentido, SSS é uma generalização do bloco único (que pode ser visto como SSS com limite de duas ações e duas ações no total). [ 1]

## Exemplo de aplicação 
Uma empresa precisa proteger seu cofre. Se um solteiro Se uma pessoa souber o código do cofre, o código poderá ser perdido ou indisponível quando o cofre precisar ser aberto. Se houver diversos pessoas que conhecem o código, podem não confiar umas nas outras para sempre agirem honestamente.

O SSS pode ser utilizado nesta situação para gerar compartilhamentos do código do cofre que são distribuídos a pessoas autorizadas na empresa. O limite mínimo e o número de ações dadas a cada indivíduo podem ser selecionados de forma que o cofre seja acessível apenas por (grupos de) indivíduos autorizados. Se forem apresentadas menos ações do que o limite, o cofre não poderá ser aberto.

Por acidente, coação ou como ato de oposição, alguns indivíduos podem apresentar informações incorretas sobre suas ações. Se o total de ações corretas não atingir o limite mínimo, o cofre permanecerá bloqueado.

## Casos de uso 
O compartilhamento secreto de Shamir pode ser usado para

* compartilhar uma chave para descriptografar a chave raiz de um gerenciador de senhas , [ 2]
* recuperar uma chave de usuário para e-mail criptografado acesso [ 3] e
* compartilhe o senha usado para recriar um segredo mestre, que por sua vez é usado para acessar um carteira de criptomoeda . [ 4]


## Propriedades e fraquezas 
* SSS tem propriedades úteis, mas também pontos fracos [ 5] isso significa que não é adequado para alguns usos.

### Propriedades úteis incluem:

1. Seguro : O esquema tem segurança teórica da informação .
2. Mínimo : O tamanho de cada peça não excede o tamanho dos dados originais.
3. Extensível : para qualquer limite, os compartilhamentos podem ser adicionados ou excluídos dinamicamente sem afetar os compartilhamentos existentes
4. Dinâmico : A segurança pode ser aprimorada sem alterar o segredo, mas alterando o polinômio ocasionalmente (mantendo o mesmo termo livre ) e construindo uma nova parcela para cada um dos participantes.
5. Flexível : Em organizações onde a hierarquia é importante, cada participante pode receber diferentes números de ações de acordo com sua importância dentro da organização. Por exemplo, com um limite de 3, o presidente poderia desbloquear o cofre sozinho se recebesse três ações, enquanto três secretários com uma ação cada devem combinar suas ações para desbloquear o cofre.
### As fraquezas incluem:

* No compartilhamento secreto verificável : 
    Durante o processo de remontagem de compartilhamento, o SSS não fornece uma maneira de verificar a exatidão de cada compartilhamento em uso. O compartilhamento secreto verificável visa verificar se os acionistas são honestos e não enviam ações falsas.
    Ponto único de falha : o segredo deve existir em um local quando for dividido em compartilhamentos e novamente em um local quando for remontado. Estes são pontos de ataque e outros esquemas, incluindo multiassinatura elimine pelo menos um desses pontos únicos de falha .
