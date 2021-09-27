[![Django](https://kenzie.com.br/blog/wp-content/uploads/2021/01/kenzie_blog_django-820x431.jpg)](https://kenzie.com.br/blog/django/)

# O que é Django, para que serve e como usar este framework

![Ugo Roveda](https://kenzie.com.br/blog/wp-content/uploads/2020/10/image-9-150x150.png)[Ugo Roveda](https://kenzie.com.br/blog/author/ugo-roveda/) - jan 20 - 8 min leitura

A vida de um profissional de programação não é fácil, isso todo mundo sabe.

Apesar de toda gratificação que sentimos ao ver uma aplicação desenvolvida funcionando perfeitamente, muitas vezes problemas pequenos atravancam o avanço de um projeto, trazendo muitos transtornos não só para o profissional responsável, mas para toda a equipe.

O que algumas pessoas talvez não saibam é que existem hoje, no mercado, diversas ferramentas voltadas à otimização de processos, como frameworks e bibliotecas.

No conteúdo de hoje, falaremos mais sobre o **framework Django**, para que serve, como funciona e como utilizá-lo.

Vamos lá?

## O que é Django?

O Django é um framework web [full stack](https://kenzie.com.br/blog/full-stack-o-que-e/) *open source* (código aberto) baseado em [Python](https://kenzie.com.br/blog/o-que-e-python/), gratuito e de alto nível.

Este framework foi criado com o objetivo de resolver todos os problemas mais comuns do processo de desenvolvimento de aplicações web, como por exemplo autenticação, rotas, *object relational mapper* (ORM) e até *migrations*.

Além disso, ele ainda suporta projetos escaláveis e conta com um eficiente sistema de segurança.

## Como surgiu o Django?

O Django foi criado em 2005 no estado do Kansas, nos EUA.

Ele foi desenvolvido pelos programadores Adrian Holovaty e Simon Willison que, na época, trabalhavam em um jornal local e precisavam criar uma solução que permitisse que o site deste jornal pudesse ser atualizado com mais frequência, mais rapidamente e sem enrolação.

Inclusive, é exatamente o que motivou a sua criação que sustenta seu slogan: “**Um framework para perfeccionistas que possuem prazos.**“

Seu nome foi dado em homenagem ao guitarrista de jazz Django Reinhardt.

## Como funciona o Django?

O Django fornece aos seus programadores e programadoras uma construção de sites rápida e com o uso de pouco código.

De modo geral, um projeto desenvolvido com o Django costuma ser dividido em pequenas aplicações compostas por um pacote Python que, por sua vez, resolve porções específicas de requisições relativas àquela aplicação.

Cada uma destas pequenas aplicações baseia-se no modelo **MVT** — ou *Model-View-Template*, os três aspectos de uma aplicação web trabalhados pelo Django.

Mais detalhadamente, são eles:

- **Model**: o modelo é a estrutura que representa os dados dessa aplicação, ou seja, possui ligação direta com um banco de dados. Esta é a camada de abstração feita para manipular, incluir ou excluir dados. Sempre que um *model* é criado, o Django fornece uma API para esta manipulação.

- **View**: o view é uma função Python feita para receber uma requisição e enviar uma resposta em retorno. É aqui onde os dados são extraídos e produzem uma resposta.

- **Template**: esta é a camada onde o template da aplicação é produzido através de artefatos compreendidos pelo navegador web. Esta é a parte que diz respeito a tudo que o usuário final é capaz de visualizar em seu dispositivo.

## O que é um Framework?

Um **framework**, assim como uma **biblioteca**, é como uma caixa de ferramentas: um conjunto de recursos e facilidades que auxiliam um desenvolvedor ou desenvolvedora na hora de construir um projeto.

A utilização de um framework traz inúmeros benefícios porque, além de colaborar significativamente com a produtividade dos profissionais de programação, também reduz custos para as empresas otimizando processos e encurtando o tempo de entrega de um produto.

## O framework é opinativo ou não opinativo?

Os frameworks classificam-se, entre outras categorias, em **opinativos** e **não-opinativos**.

Os frameworks **opinativos** são aqueles menos flexíveis com relação à utilização de seus componentes. 

Em outras palavras, podemos dizer que os frameworks opinativos já possuem uma *maneira correta* de lidar com os problemas que eles se propõem a resolver, não deixando muito margem para que o desenvolvedor(a) utilize seus elementos de formas distintas.

Já os frameworks **não-opinativos** são aqueles menos restritivos quanto ao modo como os seus recursos internos devem ser utilizados, dando mais liberdade ao profissional que enxerga diferentes caminhos para a resolução de seus problemas.

O Django é um framework **moderadamente opinativo**. Isso significa que, apesar de oferecer um conjunto de soluções prontas para a maioria das atividades, o(a) programador(a) que está no comando também pode usufruir de uma arquitetura desacoplada que o dá certa liberdade de escolha.

## Para que serve o Django?

Como explicado algumas linhas acima, o Django serve para facilitar e otimizar o tempo de desenvolvimento de projetos web por meio da linguagem de programação Python.

Abaixo, detalharei melhor cada uma das aplicações deste framework que é um dos mais queridinho do mercado de tecnologia da informação!

### Aplicações Web

A criação de **aplicações web** é uma das mais populares utilizações do Django.

Neste framework, as aplicações, em seu desenvolvimento, são divididas em pequenas outras aplicações. 

Estas pequenas aplicações são responsáveis por diferentes módulos do projeto como um todo e, deste modo, toda a compreensão e visualização do projeto ficam mais organizadas.

### Mapeamento de URL

Com o modo URLconf é possível mapear as suas URLs para que elas não contenham nenhum item não amigável, como final **.php** ou **.asp**.

### Views

As **views**, que citamos acima, são funções Python que recebem uma requisição e enviam uma resposta em retorno.

Ela funciona como um intermediário entre as camadas *model* e o *template*, acessando os dados da primeira camada e transmitindo para a segunda.

### Modelos

Os **modelos**, anteriores às views, são a camada da nossa aplicação relacionada ao armazenamento de dados, ou ao **banco de dados**.

O banco de dados padrão do Django é o SQL e é nesta camada que ocorre a manipulação dos dados deste banco.

### Formulários

O Django também permite a criação de **formulários** de maneira automática dentro da camada *templates*.

Para criar estes formulários, é necessário antes criar os arquivos model.py e views.py e, então, nosso forms.py, que será responsável por determinar todos campos do formulário a ser visualizado pelo usuário.

### Autenticação de usuários

O Django oferece um sistema de **autenticação de usuários** — ou o processo de permitir que usuários façam login em seu site em suas próprias contas — extremamente funcional, seguro e rápido de ser criado.

O framework possui modelos internos estruturados para dar a essa autenticação o melhor desempenho possível.

### Caching

Quando você acessa, diariamente, um site em que precisa preencher um pequeno formulário de login, por exemplo, o seu navegador, para não precisar processar todos os dias os dados de uma mesma página, a armazena em **cache**.

Deste modo, ao fazer a requisição para acessá-la, ela carregará mais rápido, eliminando o tempo de resposta que haveria caso seus dados precisassem ser extraídos novamente.

Em suma, armazenar em cache é registrar uma operação para que ela não precise ser recarregada toda vez que for requisitada.

Para sites de grande porte, este é um recurso fundamental, e o Django possui um sistema bastante eficiente de cache que permite o registro de páginas dinâmicas.

Com isso, os projetos desenvolvidos com este framework que contêm páginas dinâmicas podem dispor desta facilidade com uma simples configuração.

### Interface de administração

Quando navegamos pela internet, na maioria massiva das vezes transitamos somente pelas partes *externas* de um site, ou seja, sua interface visível.

Mas, por trás desta interface, existe o que o administrador daquele site vê, a sua **interface de administração**.

 Com o Django, você tem acesso a um modelo padrão de ambiente de admin muito eficiente e intuitivo.

### Serialização de objetos

**Serializar** **objetos**, ou **dados**, significa traduzi-los de formatos como texto, por exemplo, para formatos suportados por um ambiente de desenvolvimento.


Você pode instalar o pacote Django Rest Framework para trabalhar facilmente com a conversão de dados para JSON. Isso é algo bastante comum quando trabalhamos com APIs web.

## Quais as vantagens de usar Django?

Como você já deve ter percebido ao longo deste conteúdo, o Django é um framework muito atrativo para iniciantes em programação ou estudantes que ainda estão buscando especialização antes de ingressar no mercado de trabalho.

Abaixo, explicarei melhor as principais vantagens do uso deste framework.

### Completo

O Django é uma ferramenta extremamente **completa**, contendo soluções para problemas que vão desde os mais simples aos mais complexos, porque serve tanto a profissionais novatos quanto aos mais experientes.

### Versátil

Este é um dos frameworks mais **versáteis** do mercado, podendo ser utilizado para o desenvolvimento de aplicações de diversos tipos, além de ser **multiplataforma** como a linguagem em que é baseado, o Python.

### Seguro

**Segurança** foi uma preocupação bastante importante para os programadores que desenvolveram o Django.

Com este framework a sua aplicação já é desenvolvida com proteção contra os mais comuns ataques da web, como por exemplo o Cross Site Scripting (XSS), injeção SQL, Cross Site Request Forgery (CSRF) e o Clickjacking.

### Escalável

O Django foi especialmente desenvolvido para auxiliar a criação de **projetos escaláveis**, e por isso ele é utilizado por plataformas mundialmente famosas, como as redes sociais Instagram e Pinterest, além do navegador Mozilla.

### Sustentável

Baseado no princípio **DRY** — *Don’t Repeat Yourself* –, o Django possui códigos altamente sustentáveis, ou seja: de fácil manutenção e reutilizáveis graças ao agrupamento de funcionalidades.

Este é, inclusive, um dos maiores atrativos deste framework: a capacidade de criar aplicações limpas cujos códigos não precisam se repetir, ainda que a função precise.

### Portável

Assim como a linguagem em que é fundamentado, o Python, o Django também é multiplataforma e portável.

Isso significa que para utilizá-lo não é necessário estar preso a nenhum sistema operacional ou plataforma, basta ter seu interpretador instalado na máquina.

## Como usar o Django?

Enfim, para utilizar o Django é muito simples.

Em linhas gerais, basta ter o Python instalado em sua máquina e, a partir de seu interpretador, o **pip**, instalar o Django.

Siga os passos descritos abaixo:

### Baixando o Phyton

Acesse o site https://www.python.org/, vá até a seção **Downloads** e baixe a última versão do Python (hoje a 3.9.1).

### Instalando o Python

Após a conclusão do download, siga as instruções do instalador e certifique-se de que o **pip** já está pronto para ser usado na sua máquina.

Para instalar o Django, basta ir até o pip e utilizar o seguinte comando:

pip install Django

### Usando um editor de texto

O editor de texto padrão do Python é o IDLE, mas se você tiver alguma preferência por outro editor, não há problemas em utilizá-lo.

Por ser uma IDE muito simples, o IDLE geralmente é substituído por opções como:

- PyCharm
- Atom
- Sublime
- VSCode
- Notepad++
- Vim, e etc.

### Escolhendo um banco de dados

Por padrão, o Django suporta os bancos de dados SQLite (já vem junto com a aplicação), Oracle, PostreSQL e MariaDB, mas, se você não tem familiaridade ou não gosta de trabalhar com nenhum deles, outras opções são:

- CockroachDB
- Firebird
- Microsoft SQL Server

### Fazendo o Deployment da aplicação Web

Hospedar suas aplicações desenvolvidas com o Django é muito simples.

Há, hoje, muitas opções no mercado, sendo as mais populares:

- Heroku
- PythonAnywhere
- DigitalOcean
- AWS

## Onde posso aprender mais sobre Django?

Com o curso de programação full stack da Kenzie Academy Brasil, você tem acesso a um dos melhores conteúdos de especialização de programadores do país e aprende, além de como utilizar o Django, as mais importantes linguagens do mercado.

No 4o módulo de nosso currículo, **Django Framework** entra como uma disciplina complementar ao aprendizado de Python.

Na Kenzie, você se capacita como um programador ou programadora full stack em apenas 12 meses, podendo executar funções voltadas tanto a back quanto ao front-end.

## Conclusão

Se você está começando a estudar programação, saiba que este framework serve a você e as suas primeiras necessidades.

Na hora de montar seu portfólio, use e abuse da utilização destas ferramentas e ingresse no mercado de trabalho com o pé direito!

Conte conosco!