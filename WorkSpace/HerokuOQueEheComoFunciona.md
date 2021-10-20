- [Full stack](https://blog.geekhunter.com.br/category/full-stack/)

# Heroku: o que é e como funciona

- fevereiro 28, 2020
- [4 Comments](https://blog.geekhunter.com.br/heroku/#disqus_thread)

Heroku: o que é? onde vive? do que se alimenta?

Nesse artigo você vai descobrir, de uma vez por todas, o que é essa plataforma!

Criei um passo a passo para você criar uma api básica para servir de exemplo prático! Bora? 😉

**Conteúdo** [ocultar](https://blog.geekhunter.com.br/heroku/#) 

[1 O que é Heroku?](https://blog.geekhunter.com.br/heroku/#O_que_e_Heroku)

[1.1 Serviços suportados e hospedagem](https://blog.geekhunter.com.br/heroku/#Servicos_suportados_e_hospedagem)

[2 Heroku cli](https://blog.geekhunter.com.br/heroku/#Heroku_cli)

[2.1 Hora do deploy](https://blog.geekhunter.com.br/heroku/#Hora_do_deploy)

[2.2 Uma api básica](https://blog.geekhunter.com.br/heroku/#Uma_api_basica)

[2.3 Não se esqueça de fazer o login](https://blog.geekhunter.com.br/heroku/#Nao_se_esqueca_de_fazer_o_login)

[2.4 Quase lá, hein!](https://blog.geekhunter.com.br/heroku/#Quase_la_hein)

[3 Vantagens e desvantagens da Heroku](https://blog.geekhunter.com.br/heroku/#Vantagens_e_desvantagens_da_Heroku)

[3.1 Vantagens](https://blog.geekhunter.com.br/heroku/#Vantagens)

[3.2 Desvantagens](https://blog.geekhunter.com.br/heroku/#Desvantagens)

[4 Continue estudando a Heroku!](https://blog.geekhunter.com.br/heroku/#Continue_estudando_a_Heroku)

## **O que é Heroku**?

A Heroku é uma plataforma nuvem que faz deploy de várias aplicações back-end seja para hospedagem, testes em produção ou escalar as suas aplicações.

Também tem integração com o GitHub, deixando o uso mais fácil e com containers denominados [Dyno](https://www.heroku.com/dynos).

### **Serviços suportados e hospedagem**

Muitos preferem utilizar essa plataforma pela quantidade de serviços que a heroku suporta. Como as aplicações feitas em:

- [Node.js](https://blog.geekhunter.com.br/por-que-usar-node-js-uma-justificativa-passo-a-passo/);
- [Ruby](https://blog.geekhunter.com.br/linguagem-ruby/);
- [Java](https://blog.geekhunter.com.br/tudo-sobre-java/);
- [Php](https://blog.geekhunter.com.br/boas-praticas-de-programacao-em-php/);
- [Python](https://blog.geekhunter.com.br/fundamentos-de-python-para-analise-de-dados/);
- [Go](https://blog.geekhunter.com.br/golang/);
- Scala;
- Clojure.

Uma das formas para hospedar uma aplicação na Heroku é por um repositório no GitHub, uma plataforma de hospedagem de código fonte.

Se você não usa ainda, corre!

Primeiro você precisa criar o repositório e colocar todo o código da aplicação lá.

Você também pode usar o DropBox, com todas as pastas e arquivos pré-definidos.

![Exemplo de criação de um repositório no GitHub](https://blog-geek-midia.s3.amazonaws.com/wp-content/uploads/2020/01/04150230/untitled.png)Exemplo de criação de um repositório no GitHub

## **Heroku cli**

Para hospedar você precisa do heroku CLI.

Baixe se você tiver [windows 64-bit](https://cli-assets.heroku.com/heroku-x64.exe) ou o[ windows 32-bit](https://cli-assets.heroku.com/heroku-x86.exe).

No Linux, use o comando `sudo snap install --classic heroku`.

Para MacOS você pode tanto[ baixar o instalador](https://cli-assets.heroku.com/heroku.pkg) ou instalar com o comando `brew tap heroku/brew && brew install heroku`.

###  **Hora do deploy**

Vamos iniciar o procedimento de deploy!

Caso a aplicação que você irá hospedar seja [node.js](https://blog.geekhunter.com.br/por-que-usar-node-js-uma-justificativa-passo-a-passo/), você começa com `npm init` no terminal ou no cmd com o node e o npm instalado.

Para verificar se o node e o npm está instalado, insira no terminal, `npm -v` e `node -v`, se retornar um output com a versão, está instalado!

![instalações importantes para usar a plataforma ](https://blog-geek-midia.s3.amazonaws.com/wp-content/uploads/2020/01/04151522/untitled-1.png)Node e o Npm instalados? Hands ON!

Se ainda não tiver instalado, [instale ](https://cli-assets.heroku.com/heroku.pkg)agora.

Após isso, você pode criar uma pasta `src `para guardar todo o código da aplicação ou criar o` index.js` na raiz.

### Uma api básica

Neste passo a passo, irei criar uma api básica com express para servir de exemplo prático, primeiro precisamos instalar o express com o npm usando `npm install express`.

![instalando express e npm para usar a heroku ](https://blog-geek-midia.s3.amazonaws.com/wp-content/uploads/2020/01/04152734/untitled-2.png)Instalando o express com o npm

Para uma request GET básica na raiz “/”.

O código ficará mais ou menos deste jeito:

![request na plataforma ](https://blog-geek-midia.s3.amazonaws.com/wp-content/uploads/2020/01/04152902/untitled-3.png)

Apenas mais um detalhe, vamos criar um arquivo chamado “procfile”, e lá iremos inserir web `node caminho/para/sua/index.js` como este exemplo:

![arquivo criado](data:image/svg+xml,%3Csvg%20xmlns='http://www.w3.org/2000/svg'%20viewBox='0%200%20252%2085'%3E%3C/svg%3E)Arquivo criado!

Eu subi o código para o GitHub para facilitar o entendimento, caso queiram ver se estiverem com dúvidas ou se deu algo de errado, você irá ser redirecionado para o [link do repositório](https://cli-assets.heroku.com/heroku.pkg).

Após fazermos nossa api, é hora de dar deploy no Heroku, iremos usar o comando `heroku create` para criar um Dyno e gerar um nome para o app.

![ deploy no Heroku](https://blog-geek-midia.s3.amazonaws.com/wp-content/uploads/2020/01/04153203/untitled-5.png)Calma, estamos quase lá!

### Não se esqueça de fazer o login

No primeiro uso do heroku cli, ele vai pedir para você fazer login.

Aperte qualquer tecla e ele irá abrir uma nova aba no seu navegador padrão, você vai fazer login ou criar uma conta nova.

A criação da conta não é difícil, apenas preencha os campos obrigatórios de acordo com o que está sendo requisitado.

Após fazer login volte para o terminal e o seu app estará criado, algo parecido com a imagem a seguir.

![criação de app para dar deploy](https://blog-geek-midia.s3.amazonaws.com/wp-content/uploads/2020/01/04153353/untitled-6.png)Ainda não está pronto para o deploy

Ele irá ter um nome gerado, o meu foi `mysterious-harbor-03490`, o seu provavelmente irá ser diferente.

Mas não se preocupe, se quiser escolher um nome específico para sua aplicação de uma olhada em [como renomear](https://devcenter.heroku.com/articles/renaming-apps).

Agora só falta mais um comando para você dar deploy em sua aplicação!

### Quase lá, hein!

Vamos lá, iremos usar o comando `git push heroku master `e pronto!

Conseguimos dar deploy em nosso app!

Para abrir-lo em uma nova aba do navegador, digite `heroku open` no terminal.

Observa a imagem a seguir de exemplo da nossa api sendo hospedada:

![criação de deploy na plataforma heroku ](https://blog-geek-midia.s3.amazonaws.com/wp-content/uploads/2020/01/04153716/untitled-7.png)YAY!

 Fácil, né? 😀

## Vantagens e desvantagens da Heroku

###  **Vantagens**

As vantagens de usar o heroku é a produtividade!

Porque possui uma integração com o GitHub que deixa o deploy e o controle mais fácil.

Também possui um mercado de add-ons interessante!

Facilita quando você quer, por exemplo, adicionar uma database, ou como um novo plug-in em um site WordPress, apenas tendo que [acessar o Heroku ](https://devcenter.heroku.com/articles/renaming-apps)e apenas selecionar o addon que você quer instalar.

Além dele prezar pela atenção focada no desenvolvimento do núcleo da aplicaçã! E não em serviços externos como hospedagem, testes, ou infraestrutura.

Assim deixa bem mais simples de usar, aumentando muito a produtividade do programador salvando umas horas importantes quando se está desenvolvendo.

### **Desvantagens**

Não tem muitas desvantagens mas acontece que se você hospedar sua aplicação na versão gratuita e ela ficar inativa por 30 minutos, irá adormecer.

A Heroku tem 512 mb de ram para hospedar a aplicação, que não é muito ideal para aplicações muito grandes com alto tráfego e requests.

Você pode ver mais informações sobre os [planos de hospedagem](https://www.heroku.com/pricing).

## **Continue estudando a Heroku!**

Se você tiver mais dúvidas e se quiser aprofundar, dê uma olhada no [podcast oficial da Heroku](https://www.heroku.com/podcasts/codeish) e também leia a [documentação completa](https://devcenter.heroku.com/categories/reference).