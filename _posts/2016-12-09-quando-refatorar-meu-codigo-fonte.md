---
layout: post
id: 1
title:  "Quando refatorar meu código-fonte?"
subtitle:  "4 maneiras de descobrir o momento exato para refatorar"
date:   2016-12-09 00:00:00
author: Thomas Alvarenga
categories: Refactoring
tags:	refactoring
cover:  "/assets/instacode.png"
---

>_"A arte de programar consiste na arte de organizar e dominar a complexidade."_ - Dijkstra

Era uma vez, um simples programador que estava trabalhando tranquilo no seu computador
quando de repente, assustado com o que vê na tela, para tudo o que esta fazendo e pronuncia mentalmente 
a ilustre sentença: - Que merda que tá esse código!

![programador assustado com o código](/assets/programador-assustado.gif)

Eu tenho certeza que em alguns momentos da sua jornada como desenvolvedor essa história se tornou realidade,
você se vê num beco sem saída, sabe que aquele código está ruim, sabe que precisa refatorar, mas ao mesmo
tempo não tem ideia do que fazer exatamente para melhorar e até tem medo de mexer em um lugar e afetar outras partes do software.

Esse é o famoso **código espaguete**!

Nesse momento nós constumamos pensar o seguinte: "Se está funcionando, deixa quieto!", mas esse pensamento é uma auto sabotagem,
quando pensamos dessa maneira estamos vendo apenas o lado do cliente que vai utilizar o software, não paramos pra pensar em
quem dará manutenção nesse código terrível, agindo assim você vai criar um problema pra si mesmo no futuro e também para seus colegas de trabalho.

Sem contar que códigos espaguetes tendem a gerar softwares bugados com o passar do tempo!

Nossa mentalidade como desenvolvedores tem que mudar, nós temos que nos importar com o código que produzimos para que ele
mesmo não seja um fardo no futuro para darmos manutenção. Na minha carreira como desenvolvedor encontrei poucos profissionais
que realmente se importavam com o código e ao mesmo tempo tinham coragem para refatorá-lo.

>_"Qualquer um pode escrever um código que o computador entenda. Bons programadores escrevem códigos que os humanos entendem."_ - Martin Fowler

Se você quer melhorar a qualidade do código que produz mas se sente inseguro quando vai refatorar, esse post é pra você, vou mostrar
quatro maneiras de descobrir o momento exato de refatorar seu código e ter confiança no software que você está produzindo.

**Então vamos ao que interessa!**

Quando você deve refatorar seu código?

### 1. Quando não segue o Princípio da Responsabilidade Única (SRP)

O Princípio da Responsabilidade Única (SRP - Single Responsibility Principle) é um dos princípios de desenvolvimento do SOLID
que diz o seguinte:

>_"Uma classe deve ter um, e apenas um, motivo para ser modificada."_

Isso quer dizer que se uma classe ou mesmo um arquivo do seu código tiver mais de uma responsabilidade você deve separá-lo em quantas
classes ou arquivos forem necessários para que cada um tenha apenas um motivo específico para ser alterado.

Vamos detalhar melhor com um exemplo, digamos que você tem essa classe no seu código:

```javascript
class Product {
  add(params) {
    //Logic to insert the product in database
  }
  addToShoppingCart(params) {
    //Logic to add the product to shopping cart
  }
  updateStockQuantity(params) {
    //Logic to update the stock count of the product
  }
}
```
Essa classe Product é responsável por inserir um novo produto no banco de dados, adicionar um produto no carrinho de compras
e ainda atualizar a quantidade do produto em estoque, ou seja, essa classe tem três responsabilidades, três motivos para ser
alterada.

Agora vamos imaginar que temos que implementar as seguintes funcionalidades:

* Incluir o campo "validade" na tabela de produtos;
* Modificar a maneira como o estoque é contabilizado;
* Adicionar a opção de cupom de desconto no carrinho de compras;

Todas essas alterações seriam feitas no mesmo arquivo, bagunçando ainda mais o código e até inserindo mais possíveis causas
para bugs na aplicação.

Então como devemos refatorar esse código e aplicar o Princípio da Responsabilidade Única?

Basta separarmos em três arquivos diferentes, um para cada resposabilidade, por exemplo:

```javascript
class Product {
  add(params) {
    //Logic to insert the product in database
  }
}
```
```javascript
class ShoppingCart {
  addProduct(params) {
    //Logic to add the product to shopping cart
  }
}
```
```javascript
class Stock {
  updateQuantity(params) {
    //Logic to update the stock quantity of the product
  }
}
```
É lógico que esse exemplo é muito simples e fácil, mas o conceito por trás é poderoso e pouco aplicado pelos desenvolvedores.
De todos os projetos que já trabalhei, poucos aplicavam esse princípio de verdade.

Então, não se esqueça, se identificar mais de uma responsabilidade, separe-as!

### 2. Quando um arquivo tiver mais de cem linhas

Essa é uma maneira muito simples de identificar um local para refatorar no seu código, normalmente arquivos
que possuem mais de cem linhas não seguem o Princípio de Responsabilidade Única, portanto devem ser separados
em quantos arquivos forem necessários.

Um outro motivo muito importante para não termos arquivos com mais de cem linhas no nosso código é que nosso
cérebro tem dificuldade em assimilar grandes porções de lógica, você já deve ter percebido isso quando passa
um tempo longe de um código e depois volta a trabalhar com ele, o cérebro demora um pouco pra "pegar no tranco".

![cerebro tentando entender a lógica](/assets/cerebro-lendo.gif)

Sendo assim, o melhor a se fazer é criar arquivos, classes e métodos o menor possível para ajudar seu cérebro
a compreender aquela lógica mais facilmente.  

### 3. Quando os nomes não expressarem o verdadeiro sentido

"Quem nunca criou uma variável com nome 'i' pra usar no FOR que atire a primeira pedra"

Dar nomes é uma das coisas mais importantes na hora que você está desenvolvendo, porque praticamente tudo
que fazemos precisa de nomes, isso pode parecer simples para alguns, mas pode se tornar uma tarefa árdua
quando a equipe não tem um padrão de nomenclatura definida ou quando você está sem criatividade para dar nomes.

Vamos dar uma olhada nesse código:

```javascript
for (int i = 0; i < list.length; i++) {
  t += list[i].count;
}
```
Está bem claro que isso é um FOR que vai percorrer o array 'list' e somar todas as propriedades 'count'
na variável 't', mas você conseguiu identificar o verdadeiro sentido desse FOR estar no código?

É bem provável que não, porque os nomes das variáveis, arrays e propriedades não expressam o verdadeiro
sentido desse código.

Então vamos dar nome aos bois:

```javascript
for (int index = 0; index < productList.length; index++) {
  stockTotal += productList[index].count;
}
```
Com esses nomes o código fala por si só, não é necessário comentário nem documentação pra mostrar o que
esse FOR está fazendo, ele faz a contagem da quantidade total de produtos em estoque, simples assim.

Nesses casos, para refatorar o código você pode:

* Usar nomes claros e que expressem o verdadeiro sentido daquela variável, método ou classe;
* Definir um padrão de nomenclatura para sua equipe, por exemplo, camelCase para métodos e snake_case
para variáveis;
* Utilizar um style guide e lint para que todos na equipe escrevam o código no mesmo padrão, se você
trabalha com Javascript eu recomendo usar o [style guide do Airbnb](https://github.com/airbnb/javascript){:target="_blank"},
parece estranho no começo mas com o tempo você acostuma e seu código fica lindo e padronizado;
* Seguir o paradigma da [Convenção sobre Configuração](https://en.wikipedia.org/wiki/Convention_over_configuration){:target="_blank"}
que define, por exemplo, uma nomenclatura padrão a ser seguida para que os desenvolvedores não precisem decidir cada nome na sua aplicação,
basta seguir a convenção, o que é muito difundido no Ruby on Rails.

### 4. Quando a lógica está dificil de entender

Você já se deparou com um código que é dificil de entender a lógica?

Você não sabe o porque, mas toda vez que bate o olho naquela parte do codigo você perde alguns segundos até entender o que está
acontecendo, as vezes ele até segue todas as boas práticas que citamos aqui, como o Princípio da Responsabilidade Única, arquivo com 
menos de cem linhas e nomes significativos, porém aquela parte específica é confusa e parece meio bagunçada.

Isso normalmente acontece quando há muita lógica acontecendo no mesmo lugar, na mesma linha ou no mesmo método.

Com um exemplo sempre fica melhor de entender, digamos que você tenha um método para calcular quantos produtos da categoria de 'eletrônicos'
foram vendidos na sua loja virtual:

```javascript
class ShoppingCart {
  getEletronicProductSales(shoppingCarts) {
    var eletronicSalesCount = 0;

    for (int i = 0; i < shoppingCarts.length; i++) {
      for (int x = 0; shoppingCarts[i].products.length; x++) {
        if (shoppingCarts[i].products[x].category === 'eletronic' && shoppingCarts[i].paid) {
          eletronicSalesCount += shoppingCarts[i].products[x].quantity;
        }
      }
    }

    return eletronicSalesCount;
  }
}
```

Você consegue perceber como esse código é confuso mesmo sendo pequeno?

Há muita informação, muita lógica em pouco espaço, ele está percorrendo o array shoppingCarts, dentro de cada carrinho de compras ele 
percorre os produtos, dentro de cada produto ele verifica se a categoria é 'eletrônicos' e ainda verifica se a compra foi paga,
pra só então somar no total.

Até que dá pra entender o que está acontecendo, mas demora um certo tempo, é bagunçado e toda vez que você dar manutenção nesse código
você vai perder um tempo para entender, a razão disso acontecer são lógicas aninhadas e isso causa uma grande confusão no nosso cérebro.

Podemos resolver isso separando cada lógica em diferentes métodos e classes:

```javascript
class Category {
  isEletronic() {
    return this.value === 'eletronic';
  }
}
```

```javascript
class Product {
  this.category = new Category();
}
```

```javascript
class ShoppingCart {
  
  getEletronicSales(shoppingCarts) {
    var eletronicSalesCount = 0;

    foreach (cart in shoppingCarts) {
      eletronicSalesCount += sumEletronicSales(cart);
    }

    return eletronicSalesCount;
  }

  sumEletronicSales(cart) {
    var quantity = 0;
    
    if (!this.itsPaid(cart)) {
      return 0;
    }

    foreach (product in cart.products) {
      if (product.category.isEletronic()) {
         quantity += product.quantity;
      }
    }

    return quantity;
  }

  itsPaid(cart) {
    return cart.paid;
  }
  
}
```

Viu como esse código ficou mais organizado e mais fácil de entender?

Mais uma vez, esse é um exemplo pequeno e simplificado só para entendimento do conceito, o código verdadeiro talvez não caberia nesse post,
até usei um foreach pra ficar mais bonito!

Parece que estamos escrevendo mais linhas para fazer a mesma coisa, mais um código melhor estruturado se paga com o passar do tempo.

### Agora é a sua vez!

Se você chegou até aqui é porque realmente quer aprender a refatorar seu código-fonte e mudar a maneira como escreve software, entretanto, de acordo
com essa [pesquisa](http://blog.mundopm.com.br/2013/03/15/o-ser-humano-e-a-absorcao-de-conhecimentos/){:target="_blank"}, nós armazenamos 
apenas 10% do conteúdo que lemos, mas se você praticar o que leu esse valor sobe para 75%.

Pensando nisso, eu preparei uma série de exercícios que você pode ter acesso inserindo seu email no campo abaixo:

<link href="//cdn-images.mailchimp.com/embedcode/horizontal-slim-10_7.css" rel="stylesheet" type="text/css">
<style type="text/css">
	#mc_embed_signup{background:#fff; clear:left; font:14px Helvetica,Arial,sans-serif; width:100%;}
</style>
<div id="mc_embed_signup">
<form action="//plenitudesoftware.us14.list-manage.com/subscribe/post?u=e145542d2457225f21795e468&amp;id=2b41336108" method="post" id="mc-embedded-subscribe-form" name="mc-embedded-subscribe-form" class="validate" target="_blank" novalidate>
    <div id="mc_embed_signup_scroll">
	<input type="email" placeholder="Insira seu melhor email aqui..." value="" name="EMAIL" class="email" id="mce-EMAIL" required>
    <div style="position: absolute; left: -5000px;" aria-hidden="true"><input type="text" name="b_e145542d2457225f21795e468_2b41336108" tabindex="-1" value=""></div>
    <div class="clear"><input type="submit" value="EU QUERO APRENDER MAIS COM OS EXERCÍCIOS" name="subscribe" id="mc-embedded-subscribe" class="button"></div>
    </div>
</form>
</div>
<br/>
No final da série de exercícios eu coloquei um link para um repositório no Github que está bem bagunçado,
só te esperando para ser refatorado. Faz um fork lá e aplica o que aprendeu nesse post na prática, 
depois só mandar um PR que agente faz uma avaliação.

Ahh não esquece de deixar seu comentário aqui embaixo e compartilhar com os desenvolvedores que você conhece,
eles devem estar precisando desse conteúdo também!

Então é isso, um abraço e até o próximo post!
