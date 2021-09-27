# Django: o que é e como começar a usar este framework

- junho 25, 2021 - [0 Comments](https://blog.geekhunter.com.br/django-introducao-ao-framework/#disqus_thread)

Neste artigo, vamos falar sobre um framework para aplicações escrito em Python. Ele é muito popular entre os desenvolvedores e bastante requisitado entre os recrutadores. É o tal do Django.

Usada em grandes empresas como o Instagram, Mozilla e o Pinterest, o Django Framework atrai atenção dos desenvolvedores de python porque permite a criação de aplicações web com processos muito otimizados!

Mas por que “Django”? Criado em 2003, a ideia para o nome do framework, segundo seus desenvolvedores, surgiu em homenagem a Jean Reinhardt, um famoso guitarrista que era conhecido popularmente como Django, seu nome [Romani](https://en.wikipedia.org/wiki/Romani_people).

Vamos ao conteúdo? Tenho certeza que você vai curtir! 😁

**Conteúdo** [ocultar](https://blog.geekhunter.com.br/django-introducao-ao-framework/#) 

[1 O que é Django](https://blog.geekhunter.com.br/django-introducao-ao-framework/#O_que_e_Django)

[2 Como funciona o Django](https://blog.geekhunter.com.br/django-introducao-ao-framework/#Como_funciona_o_Django)

[2.1 Models](https://blog.geekhunter.com.br/django-introducao-ao-framework/#Models)

[2.2 View](https://blog.geekhunter.com.br/django-introducao-ao-framework/#View)

[2.3 Templates](https://blog.geekhunter.com.br/django-introducao-ao-framework/#Templates)

[3 Para que serve o Django](https://blog.geekhunter.com.br/django-introducao-ao-framework/#Para_que_serve_o_Django)

[3.1 Construção rápida de aplicações](https://blog.geekhunter.com.br/django-introducao-ao-framework/#Construcao_rapida_de_aplicacoes)

[3.2 Disponibilizar o máximo de ferramentas](https://blog.geekhunter.com.br/django-introducao-ao-framework/#Disponibilizar_o_maximo_de_ferramentas)

[3.3 Garantir a segurança da sua aplicação](https://blog.geekhunter.com.br/django-introducao-ao-framework/#Garantir_a_seguranca_da_sua_aplicacao)

[3.4 Django é um framework moderadamente opinativo](https://blog.geekhunter.com.br/django-introducao-ao-framework/#Django_e_um_framework_moderadamente_opinativo)

[4 O que é um Framework](https://blog.geekhunter.com.br/django-introducao-ao-framework/#O_que_e_um_Framework)

[5 Como obter o Python](https://blog.geekhunter.com.br/django-introducao-ao-framework/#Como_obter_o_Python)

[6 Como baixar o Python na sua máquina](https://blog.geekhunter.com.br/django-introducao-ao-framework/#Como_baixar_o_Python_na_sua_maquina)

[7 Como instalar o Python na sua máquina](https://blog.geekhunter.com.br/django-introducao-ao-framework/#Como_instalar_o_Python_na_sua_maquina)

[8 Qual IDE usar para escrever códigos Django](https://blog.geekhunter.com.br/django-introducao-ao-framework/#Qual_IDE_usar_para_escrever_codigos_Django)

[9 Quais bancos de dados posso utilizar com Django](https://blog.geekhunter.com.br/django-introducao-ao-framework/#Quais_bancos_de_dados_posso_utilizar_com_Django)

[10 Onde posso fazer o deployment da minha aplicação Web com Django](https://blog.geekhunter.com.br/django-introducao-ao-framework/#Onde_posso_fazer_o_deployment_da_minha_aplicacao_Web_com_Django)

## O que é Django

O Django é um framework de aplicativos web gratuito e de código aberto escrito em Python.

No [site oficial do framework Django](https://djangoproject.com/), ele é descrito como uma estrutura da Web Python de alto nível que incentiva o desenvolvimento rápido e o design limpo e pragmático.

Construído por desenvolvedores experientes, o framework cuida de grande parte do aborrecimento do desenvolvimento da Web, para que você possa se concentrar em escrever seu aplicativo sem precisar reinventar a roda.

Como desenvolvedor Django que sou, não poderia concordar mais com essa descrição.

## Como funciona o Django

O Django separa o processo em duas fases **Request Phase** e **Response Phase.**

Podemos conferir na imagem sobre o ciclo completo entre as duas fases.

![como-funciona-django](https://blog-geek-midia.s3.amazonaws.com/wp-content/uploads/2020/08/13100920/como-funciona-o-django.jpg)Como funciona o Django

O Django utiliza uma arquitetura nomeada Model View Template, que é bem semelhante da já conhecida MVC. A Mode View Controller você encontra em linguagens de programação como Java, C#, Delphi, Ruby, PHP, JS.

Consiste em um objetic-relational mapper (ORM) que faz a mediação entre os modelos de dados (definidos como classes de Python) e um banco de dados relacional (“**M**odel”), um sistema para processamento de solicitações HTTP com um sistema de modelos da web (“**V**iew”) e um despachante de URL baseado em expressão regular (“**C**ontroller”).

### Models

Um “Model” é a fonte única e definitiva de informações sobre seus dados. Ele contém os campos e comportamentos essenciais dos dados que você está armazenando. É a camada relacionada ao banco de dados.

Esse exemplo de model define uma `**Person**`, que tem um `**first_name**` e um `**last_name**`:

```
from django.db import models

class Person(models.Model):
    first_name = models.CharField(max_length=30)
    last_name = models.CharField(max_length=30)
```

**`\**first_name\**`** e ***\**\*`\*\*last_name\*\*`\*\**\*** são campos do model. Cada campo é especificado como um atributo de classe e cada atributo é mapeado para uma coluna do banco de dados.

O model **Person** acima criaria uma tabela de banco de dados como esta:

```
CREATE TABLE myapp_person (
    "id" serial NOT NULL PRIMARY KEY,
    "first_name" varchar(30) NOT NULL,
    "last_name" varchar(30) NOT NULL
);
```

> Veja os [10 livros de Python](https://blog.geekhunter.com.br/10-livros-de-python-que-todo-dev-especialista-deve-ler/) que todo dev especialista deve ler

### View

É uma função Python que recebe uma solicitação da Web e retorna uma resposta da Web. Essa resposta pode ser o conteúdo HTML de uma página da Web, ou um redirecionamento, ou um erro 404, ou um documento XML, ou uma imagem… ou qualquer coisa, realmente.

Esta é uma visualização que retorna a data e hora atuais, como um documento HTML:

```
from django.http import HttpResponse
import datetime

def current_datetime(request):
    now = datetime.datetime.now()
    html = "<html><body>It is now %s.</body></html>" % now
    return HttpResponse(html)
```

As views usam os models para acessar os dados necessários às solicitações. Depois, levam essas informações para os…

### Templates

Um tempalte contém as partes estáticas da saída HTML desejada, bem como alguma sintaxe especial que descreve como o conteúdo dinâmico será inserido.

É aqui que a randerização dos dados acontece, vindo das views. É como o Django mostra as solicitações para os usuários.

Olhando de novo o esquema de Request-Response Cycle acima, você consegue ver essa sequência mais facilmente.

## Para que serve o Django

![Começando a usar o Framework Django](https://blog-geek-midia.s3.amazonaws.com/wp-content/uploads/2021/06/25153128/Get-Started-With-Django_Watermarked.15a1e05597bc1-1024x576.jpg)Utilidades do Framework Django. Foto reproduzida do blog Real Python.

Entendido como ele funciona, vamos partir para sua aplicações práticas. O que o Django é capaz de fazer?

Como um otimizador de desenvolvimento de projetos web, o Django é capaz de construir melhores aplicações web, de forma mais rápida e com menos código. Vou te explicar ponto por ponto:

### Construção rápida de aplicações

O framework Django foi construído para ajudar as pessoas desenvolvedoras a sair do conceito da aplicação para a produção o mais rápido possível.

Em poucos passos, você já consegue instalar esse framework python e sair usando. Vou te mostrar isso mais pra frente, então continua comigo!

Na documentação oficial há um [tutorial rápido](https://docs.djangoproject.com/en/3.2/intro/tutorial01/) que você já pode implementar, criando a sua primeira aplicação Django!

### Disponibilizar o máximo de ferramentas

Django inclui dezenas de extras que você pode usar para lidar com tarefas comuns de desenvolvimento da Web. Django cuida da autenticação do usuário, administração de conteúdo, mapas do site, feeds RSS e muitas outras tarefas.

Ele trabalha com qualquer framework do lado do cliente, entregando em vários formatos aqueles conteúdos que vocÊ já conhece (HTML, JSON, XML…)

### Garantir a segurança da sua aplicação

O Django leva a segurança a sério e ajuda os desenvolvedores a evitar muitos erros comuns de segurança, como injeção de SQL, script entre sites, falsificação de solicitações entre sites e clickjacking.

Seu sistema de autenticação de usuário fornece uma maneira segura de gerenciar contas e senhas de usuários.

Você pode ler em detalhes todos esses [erros de segurança na documentação oficial](https://docs.djangoproject.com/en/3.2/topics/security/) do framework django!

### Django é um framework moderadamente opinativo

Além disso, existe um ponto legal de comentar: o Django é um framework moderadamente opinativo. Você sabe o que é isso?

Basicamente, é a flexibilidade que o framework dá às pessoas desenvolvedoras para a resolução dos problemas. Se é opinativo, já possuem uma maneira correta de resolver os problemas, csem margens. Se é não-opinativo, não possui essas “regras” e deixa o dev livre para solucionar os problemas como quiser.

Por estar no meio dessas duas extremidades, ele apresenta o melhor dos dois mundos. O usuário pode usufruir de soluções prontas e também usar a arquitetura desacoplada para ter liberdade de escolha na hora de reseolver problemas com o framework Django.

## O que é um Framework

Só para você ter mais claro, um framework nada mais é do que um conjunto de módulos que facilitam o desenvolvimento e, no caso do Django, esses módulos são utilizados em conjunto, permitindo assim a criação de aplicações ou páginas web a partir de um código existente, ao invés de começar tudo do zero.

> Os 5 [melhores frameworks de Python](https://blog.geekhunter.com.br/os-5-melhores-frameworks-de-python/)

## Como obter o Python

![Página de download no site oficial do Python](https://blog-geek-midia.s3.amazonaws.com/wp-content/uploads/2021/06/25132856/baixar-python-1024x440.png)Página de download no site oficial do Python

Eu sei que você não vai acreditar no que vou te dizer agora, mas para utilizar um framework Python como o Django, você precisa ter a linguagem Python instalada na sua máquina…

E já que estamos falando de Python, que tal dar uma olhada nos [passos para iniciar a sua carreira de pessoa desenvolvedora Python](https://blog.geekhunter.com.br/passos-para-iniciar-a-sua-carreira-de-desenvolvedor-na-linguagem-python/)?

Então, para a instalação da linguagem Python, faça o seguinte:

## Como baixar o Python na sua máquina

Vá para a [página de download da linguagem Python](https://www.python.org/downloads/) e escolha o sistema operacional que a sua máquina possui.

Vamos considerar o processo para uma máquina com Sistema Operacional Windows, para facilitar o exercício.

Logo abaixo do cabeçalho, onde diz “Looking for a specific release?”, clique no botão da coluna Release Version escrito “Python 3.9.5” (atenção porque pode ser que quando você estiver lendo esse artigo, o número da versão pode estar bem maior).

![Releases do Python para download - Framework Django](https://blog-geek-midia.s3.amazonaws.com/wp-content/uploads/2021/06/25134024/versoes-python-para-download-1024x431.png)Releases do Python para download

Depois, vá até o fim da página e escolha **Windows x86-64** executable installer, se a sua máquina for 64 bits ou **Windows x86** executable installer no caso de sua máquina for 32 bits.

**Observação:** se por acaso você escolher a versão “errada” do Python, exemplo, sua máquina é de 64 bits, mas você escolheu a de 32 bits, você terá trabalho para corrigir. Para instalar a versão “correta”, você vai precisar desinstalar a versão “errada”, baixar o outro instalador e proceder com uma nova instalação.

## Como instalar o Python na sua máquina

Instalando a release oficial do Django:

Instale pip – essa parte é bem fácil porque se você já instalou a linguagem Python na sua máquina, o pip veio junto no pacote.

Depois, vá para o terminal e escreva o seguinte comando:

```
C:> python -m pip install Django
```

## Qual IDE usar para escrever códigos Django

Geralmente, IDLE é a IDE que vem junto com a instalação do Python. Ou seja, instalou o Python, a IDE vem junto no pacote.

O problema é que essa IDE não possui um monte de facilidades que certamente vão aumentar a velocidade de geração de código, consequentemente, turbinando a sua produtividade.

A lista é grande e vou citar apenas alguns, sem nenhuma ordem de preferência. Na dúvida, faça um teste com umas duas ou três e escolha aquela que melhor se adapta ao seu estilo.

Pois bem, vamos a lista:

- **PyCharm** – Essa é a minha IDE favorita para escrever código Python;
- **VS Code** – Quando comecei a desenvolver para a Web, essa foi a minha primeira escolha. Achava sensacional, até o dia que eu encontrei o PyCharm;
- **Atom** – é um excelente editor de texto de código aberto, desenvolvido pelo GitHub.
- **Brackets** – também já escrevi código com esse editor, desenvolvido pela Adobe Systems. Interessante;
- **Notepad++** – editor também da Microsoft que já também cativou e continua cativando o coração de muito desenvolvedor;
- **Vim** – acho que esse editor veio para o pessoal que veio do mundo Linux, mas isso é só uma opinião. Já trabalhei com o editor vi, provavelmente neto do Vim, e o negócio é pesado…

A lista segue, mas já deu para se ter uma ideia de que a escolha pela IDE/Editor de texto vai mais da questão de gosto de cada desenvolvedor e, certamente, tem uma opção para cada um.

> 5 [melhores IDEs e Editores de Código em Python](https://blog.geekhunter.com.br/ides-e-editores-de-codigo-em-python-para-2020/) em 2020

## Quais bancos de dados posso utilizar com Django

Oficialmente, o framework Django suporta os seguintes bancos de dados:

- PostgreSQL;
- MariaDB;
- Oracle;
- SQLite;

Extraoficialmente, existem outros bancos de dados que também podem ser usados com o Django, segundo a documentação oficial:

- CockroachDB – confesso que esse nunca tinha ouvido falar, mas está na documentação oficial;
- Firebird;
- Microsoft SQL Server.

Quando você instala o Django na sua máquina, ele já vem com o SQLite instalado e você não precisa se preocupar com configuração de qualquer outro banco.

Naturalmente, quando for fazer o *deploy* para um ambiente de produção, aí você escolhe o banco de dados que cabe no seu bolso. Enquanto estiver na fase de desenvolvimento, o SQLite dá conta do recado direitinho.

## Onde posso fazer o deployment da minha aplicação Web com Django

As opções são muitas e só esse tópico, por si só, já valeria um artigo para se falar onde se pode hospedar sua aplicação desenvolvida com Django.

A minha opção foi hospedar minhas aplicações Django no site Heroku, e confesso que foi uma das minhas primeiras opções de deployment.

Como ainda não tive tempo para avaliar outras opções que certamente valem a pena, acabei ficando por lá mesmo, até porque me adaptei facilmente com o processo.

Apenas para citar algumas, sem nenhuma ordem específica, apenas as primeiras que vem na minha cabeça:

- **[Heroku](https://blog.geekhunter.com.br/heroku/)** – naturalmente a primeira que veio à mente, por questões práticas. Para mim é perfeita, ou quase perfeita, se sua aplicação for pequena ou de médio porte, caso contrário, o preço pode ser um impeditivo. Pode hospedar outras linguagens além do Python como Java, Ruby e PHP. Possui uma perfeita integração com o Github, mas isso também é assunto para um outro artigo;
- **PythonAnywhere** – Apesar de nunca ter utilizado, parece ser de fácil configuração, mas como o próprio nome sugere, só hospeda aplicação com a linguagem Python;
- **AWS** – A quantidade de opções é tão grande que as vezes acaba complicando, principalmente se você estiver iniciando a carreira de desenvolvedor;
- **DigitalOcean** – Parece ser uma das mais populares, baseado nas reviews que vejo a respeito. Fácil de configurar e escalar.

Nessa pequena lista que fiz, não existe melhor ou pior e, mais uma vez, você precisa fazer sua própria avaliação e ver aquele que se encaixa melhor com o seu estilo e responde as suas demandas.

Se chegou à decisão de que Django é o ideal para resolver os seus problemas de negócio, você já sabe onde pode hospedar sua aplicação.

Importante: antes de assinar com qualquer um, certifique-se que o suporte ao framework Django é oferecido, para evitar surpresas futuras.

E lembre-se, por mais que uma nova tecnologia possa parecer desafiadora, lembre-se que:

> “Quanto piores eles são, maior é a recompensa”
>
> Jamie Foxx como Django, no filme “Django livre’.

Um abraço e até a próxima! 😄