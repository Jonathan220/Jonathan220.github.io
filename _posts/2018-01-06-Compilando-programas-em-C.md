---
layout: post
title: "Compilando programas em C"
author: "Jonathan Abreu"
categories: journal
tags: [programação, linguagemC]
image: https://source.unsplash.com/user/golakka/likes
---
Uma dúvida que assola a muitos estudantes da linguagem C nos seus primeiros contatos com a linguagem, é quando precisam compilar um programa em C sem a utilização de uma IDE, principalmente em sistemas Linux.

Hoje eu vou explicar como fazer isso no Windows e no Linux, utilizando o **compilador gcc**.

## Compilando em sistemas Unix

Feito o programa em linguagem C, através de um editor, o próximo passo é verificar se o código está escrito da maneira correta. A este trabalho se dá o compilador, ele verifica se a sintaxe do programa está correta (ele não verifica erros de lógica). Caso haja erro na sintaxe dos comandos o processo de compilação é abortado.

O compilador também pode enviar mensagens de aviso(Warning), casos em que não há erro de sintaxe, mas pode haver alguma suspeita de erro ou uso de comandos antigos. Isso não impede a compilação.

Por padrão na maioria dos sistemas Unix(se não todos), há um compilador da linguagem C, no meu caso que estou usando **Fedora**, o compilador é o **gcc**(GNU Compiler Collection). 

Para compilar um programa em C (arquivo com extensão .c), primeiro você deve acessar a pasta onde o arquivo se encontra no terminal através do comando: 

{% highlight bash %}
$ cd nome da pasta.
{% endhighlight %}

Depois de acessada a pasta o próximo passo é a compilação do arquivo.
{% highlight bash %}
$gcc programa.c -o programa
{% endhighlight %}

O **Programa.c** é o arquivo que você editou em linguagem C e quer compilar. **gcc** é o nome do comando para compilar usando o **compilador gcc**. O parâmetro **-o** é para indicar construção do executável com o nome programa. Se ocorrer tudo corretamente, o terminal não lhe enviará nenhuma mensagem.

Se você não indicar o parâmetro **-o nomedoexecutável**, o terminal criará um arquivo **a.out** por padrão como o executável. E a cada nova compilação vai fazer com que apague o executável do projeto anterior.

Também é possível compilar através do utilitário **Make**, evitando escritas de comando muito grande. Entrarei em detalhes sobre este comando no próximo post.

Para executar o programa basta executar no terminal:
{% highlight bash %}
$./programa
{% endhighlight %}

E para terminar, alguns parâmetros que podem ser útil são: 
	*-wall* - mostra alguns avisos e informações que podem ser útil na detecção de algum problema de compilação.

{% highlight bash %}
$gcc --version 
{% endhighlight %}
	
Caso não saiba que compilador seu computador possui.

{% highlight bash %}
$gcc -c prog.c
{% endhighlight %}
	
Caso queira criar apenas um arquivo objeto. Arquivo criado antes do processo de "linkagem".

O desenvolvimento de um programa engloba quatro fases distintas:
<ol>
	<li>Edição do código fonte.</li>
	<li>
		Compilação do programa.
		<ul>
			<li>correção dos erros de sintaxe, cria o arquivo objeto (em linux com extensão .o em windows .OBJ).</li>
		</ul>
	</li>
	<li>
		"Linkagem" dos objetos.
		<ul>
			<li>Adiciona bibliotecas necessárias para a execução do programa.</li>
		</ul>
	</li>
	<li>execução do programa.</li>
</ol>

## Compilando em ambiente Windows

Em ambientes Windows o mais recomendado é a instalação de uma IDE para a compilação. As duas mais comuns são [Dev-c++](http://www.bloodshed.net/devcpp.html) e [codeBlocks](http://www.codeblocks.org/home). O gcc possui uma versão chamada [MinGW](http://www.mingw.org/), e normalmente vem instalada junto das IDE's.

Para executar o compilador por linhas de comando será necessário adicioná-lo ao caminho de busca de executáveis. Isso é feito adicionando o caminho da sua instalação ao final da variável de ambiente PATH.

Para compilar o comando é o mesmo do Linux, lembrando que deve estar dentro da pasta do arquivo a ser compilado.
Na hora de executar o programa basta digitar o nome do executável(sem o ./ e a extensão).