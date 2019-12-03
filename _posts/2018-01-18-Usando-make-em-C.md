---
layout: post
tittle: "Usando make em C"
author: "Jonathan Abreu"
tags: [geral, programação]
image: https://source.unsplash.com/user/lucabravo/XJXWbfSo2f0
---

O utilitário **make** é util para a compilação de muitos arquivos de maneira automática, facilitando casos em que um executável consiste em muitos arquivos para sua criação.

Por exemplo, um usuário precisa compilar muitos arquivos de um programa em C, para isso ele utiliza o seguinte comando:

{% highlight bash %}
$ gcc prog1.c prog2.c prog3.c -o executavel
{% endhighlight %}

Mas se o usuário precisar modificar apenas um dos arquivos ele terá que compilar tudo de novo. Uma alternativa seria somente compilar o arquivo que você alterou com o seguinte comando:

{% highlight bash %}
$ gcc -c prog1.c
{% endhighlight %}

Este comando só compila o programa, não realizando a linkedição do arquivo, e como resultado cria um arquivo objeto com extensão .o. Dessa forma vc detecta erros de sintaxe sem ter que compilar muitos arquivos. Para mais detalhes sobre as fases de compilação dê uma olhada na matéria [Compilando programas em C]({{ site.github.url }}{ post_url % 2018-01-06-Compilando-programas-em-C %})
Em seguida o usuário terá apenas que montar os arquivos objetos:

{% highlight bash %}
$ gcc -o executavel prog1.o prog2.o prog3.o
{% endhighlight %}

Mas a medida que os programas forem ficando maiores e com muitos arquivos o make se torna uma opção melhor e mais prática.

O make usa um arquivo chamado **makefile**(este arquivo pode também ser denominado GNUmakefile ou Makefile), que contém regras de como compilar e criar um programa. Ao executar o comando make no terminal, sem a especificação de um arquivo ele executará o primeiro arquivo makefile que encontrar.

O arquivo makefile(sem extensão) possui regras dentro dele, estas regras são denominadas por alvos(cada alvo é um conjunto de instruções e dependências). O arquivo possui o seguinte formato:

{% highlight make %}
nome_do_alvo : [dependências]
comandos
{% endhighlight %}

O arquivo tem um nome alvo seguido por arquivos o qual o alvo irá necessitar para sua execução(suas dependências). Abaixo segue os comandos a serem executados. Os comandos devem vir identados por **TAB**, nada de espaços em branco. As dependẽncias podem ser arquivos necessários para a execução do alvo ou uma lista de outros alvos. Se as dependências são uma lista de arquivos, estes devem ser encontrados para a execução do alvo. Se a lista de dependências são outros alvos, estes devem ser executados antes.

### Exemplos:

{% highlight make %}
Alvo1: exemplo.c
	gcc -c exemplo.c
{% endhighlight %}

Neste exemplo o arquivo **exemplo.c** é procurado dentro do diretório em que se encontra o arquivo makefile, ao ser encontrado é executado a compilação do arquivo, gerando no fim o arquivo **exemplo.o**.

{% highlight make %}
alvo1: exemplo
	./exemplo

exemplo: exemplo.c exemplo.o
	gcc -o exemplo exemplo.o

exemplo.o: exemplo.c
	gcc -c exemplo.c
{% endhighlight %}

Neste exemplo o **alvo1** é primeiro executado, a sua dependẽncia é o alvo **exemplo**. Então o alvo exemplo é executado, suas dependências são **exemplo.c** e **exemplo.o**. **Exemplo.c** é um arquivo e sua existencia é verificada no diretório local, já **exemplo.o** não existe. O alvo **exemplo.o** é executado, suas dependẽncias resolvida e o comando abaixo executado, gerando o arquivo **exemplo.o**. Em seguida o alvo **exemplo** tem seu comando executado, e finalmente o **alvo1** tem seu comando executado também.

Aqui ele executou o primeiro alvo que encontrou no arquivo, mas esta ordem pode ser alterada. Ao executar **make** no terminal o usuário pode definir um ou mais alvos, caso não especifique o primeiro alvo do arquivo é executado.

### Outras funcionalidades do makefile

É possível também colocar comentários no makefile através do caractere **#**. O comentários se extenderá até o fim da linha.

Existe alguns nomes de alvos que são comumente utilizados:

* **all**: a regra global, é o primeiro alvo a ser executado pelo comando make, quando este não especifica um alvo em sua execução.

* **clean**: usado para para apagar arquivos-objeto e outros arquivos temporários.

* **install**: usado para instalar programas. 

Podemos também criar variáveis no arquivo makefile. Por exemplo:

{% highlight make %}
variavel = teste

alvo1:
	@echo $(variavel)
{% endhighlight %}

Iniciamos uma variável com o valor **teste** e em seguida no primeiro alvo imprimimos a variável com o valor dela. O comando **echo** imprime a variável, o simbolo **@** que o precede é para que o comando não seja impresso. O **$** é para indicar que a palavra entre parênteses deve ser substituída por seu valor correspondente. A variável deve ser colocada entre chaves{} ou parênteses().

Além de declarar suas próprias variáveis é possível utilizar variáveis de forma automática, variáveis mantidas internamente pelo make.

* **$*** nome dos arquivos dependentes sem extensão.

* **$@** nome do arquivo alvo

* **$<** nome dos arquivos dependentes com extensão.

{% highlight make %}
all: exemplo

exemplo: exemplo.o
	gcc -o $@ $<

exemplo.o: exemplo.c
	gcc -c $<
{% endhighlight %}

Uma linha longa pode ser quebrada em várias linhas, sem interromper seu valor, utilizando uma barra invertida(\\).

Esta é uma introdução básica ao funcionamento do makefile. Este utilitário é muito mais complexo, e possui funções poderosas para a compilação e instalação de programas. Para mais informação o usuário pode acessar a documentação(em inglês) pelo terminal:

{% highlight bash %}
$ info make
{% endhighlight %}