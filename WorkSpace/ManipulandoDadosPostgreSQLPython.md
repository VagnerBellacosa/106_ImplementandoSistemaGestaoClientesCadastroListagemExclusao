

# Manipulando Dados em PostgreSQL com Python

[On 20 de maio de 2021](https://dadosaocubo.com/2021/05/20/), by [Tiago Dias](https://dadosaocubo.com/author/tiago/)

![img](https://i1.wp.com/dadosaocubo.com/wp-content/uploads/2021/05/CapaManipulandoDados-1.png?fit=1024%2C512&ssl=1)

Alguém aí está curioso para saber como vamos finalizar a nossa trilogia **API com Python, o céu é o limite**. Então vamos lá, após [consumir os dados da API](https://dadosaocubo.com/ingestao-de-dados-via-api-com-python/), realizar o [processamento paralelo com Python](https://dadosaocubo.com/processamento-paralelo-com-python/) para otimizar o tempo, então temos o seguinte problema, onde vamos guardar os dados da API? Sendo assim o dados ao cubo vai mostrar para vocês como persistir os dados em um banco de dados relacional, o PostgreSQL, utilizando a linguagem Python.

Para isso conheceremos a biblioteca **Psycopg2**, com ela é possível trabalhar com Python e PostgreSQL. E então, vamos para prática e ver toda essa teoria acontecendo.

## **Bancos de Dados Relacionais e PostgreSQL**

![img](https://lh5.googleusercontent.com/72_sufWNA_REMSd7d9Q5iUStQOewBX37vFgaTD5anj_HnQnXCNANMejjk_P00GW5XzdM7uw2it0z2tDqQZDv8ICl7WwIDRbI-aBB-DO4KwjmgHjpLKVKQg5YGrMfDQ)

Os bancos de dados relacionais, já vimos em outro post ([Linguagem SQL e os Bancos de Dados Relacionais](https://dadosaocubo.com/linguagem-sql-e-os-bancos-de-dados-relacionais/)) e temos a seguinte definição:

“Falando apenas dos bancos relacionais, são dados estruturados em linhas e colunas. O armazenamento é feito através de tabelas com relacionamentos entre si. Essa estrutura é formalizada pelos termos: relação (tabela), tupla (linhas) e atributo (coluna).”

E o PostgreSQL é um sistema gerenciador de banco de dados (SGDB) de código aberto. Como o PostgreSQL, existem muitos outros SGBD, seja também de código aberto ou proprietário, então para nossa prática utilizaremos o PostgreSQL, mas antes precisamos ver como se comunicar com ele através do Python.

## **A Biblioteca Psycopg2**

![img](https://lh6.googleusercontent.com/mLZScJ9YatzNBsCtzezPppOPblDs7BogNVcfyV1HsclYVx1f-CwgTSinGmiacxSgFBjf-Mjeb3NW6Tr9TY25O_-3zQWH5I_h0mqqVOhorOm2d89qcDbxHRn7V4RBVQ)

Esse pacote, faz a comunicação do Python com banco de dados PostgreSQL. Dessa forma é possível realizar operações no banco de dados via scripts Python. Estas funcionalidades facilitam e muito a manipulação de dados na linguagem Python e os bancos de dados PostgreSQL. A biblioteca pode ser instalada utilizando o comando abaixo:

```
pip install psycopg2
```

Agora já sabemos as informações básicas para iniciar a parte prática, onde persistiremos os dados no banco PostgreSQL utilizando a linguagem Python através do pacote psycopg2.

## **Manipulando Dados em PostgreSQL com o D[3]**

Iniciamos, importando as bibliotecas necessárias para o desenvolvimento do nosso notebook.

```
import requests
import json
import pandas as pd
import psycopg2
```

As bibliotecas **requests** e **json** vão auxiliar na extração dos dados da API. O pacote **pandas** vai realizar a estruturação de dados. E a biblioteca **psycopg2** vai fazer a comunicação com o PostgreSQL.

Vamos relembrar a requisição dos dados na API, utilizando a função **request** do pacote **requests**, passando o método (GET), a url (https://dadosabertos.camara.leg.br/api/v2/deputados). Com o resultado da requisição utilizaremos a função **loads** da biblioteca **json** para obter os dados em um formato de dicionário. 

```
# Requisição dos dados dos Deputados
url        = 'https://dadosabertos.camara.leg.br/api/v2/deputados'
parametros = {}
resposta   = requests.request("GET", url, params=parametros)
objetos    = json.loads(resposta.text)
dados      = objetos['dados']
```

Faremos a estruturação dos dados, utilizaremos a função **DataFrame** do **pandas**

```
df = pd.DataFrame(dados)
```

Em seguida deixaremos todas as colunas com o formato de string, através da função **apply** do **pandas**.

```
for col in df.columns:
  df[col] = df[col].apply(str)
```

Agora que já temos os dados da API, podemos começar a se comunicar com o PostgreSQL.

### **Conexão do Python com o PostgreSQL**

Primeiro, criamos uma função para a conexão com o banco de dados, chamaremos de **conecta_db**. Para isso, utilizamos a função **connect** do pacote **psycopg2**, informamos o host, database, user e password.

```
# Função para criar conexão no banco
def conecta_db():
  con = psycopg2.connect(host='localhost', 
                         database='db_deputados',
                         user='postgres', 
                         password='postgres')
  return con
```

Chamamos a função que tem a conexão com o banco de dados do PostgreSQL.

### **Dropando e Criando Tabela no PostgreSQL**

Em seguida, estruturamos a função **criar_db** que cria a tabela no banco de dados. Aqui utilizaremos as funções **cursor**, para abrir uma sessão para o usuário, a **execute** que roda o sql no banco, o **commit** finaliza a transação e a **close** finaliza a conexão com o banco. Todas essas funções do pacote **psycopg2**.

```
# Função para criar tabela no banco
def criar_db(sql):
  con = conecta_db()
  cur = con.cursor()
  cur.execute(sql)
  con.commit()
  con.close()
```

Agora, chamamos a função que criamos acima, informando o comando SQL para executar no banco. Primeiro deletamos a tabela do banco caso ela exista e em seguida usamos a mesma função **criar_db** para criar a tabela de **deputados**.

```
# Dropando a tabela caso ela já exista
sql = 'DROP TABLE IF EXISTS public.deputados'
criar_db(sql)
# Criando a tabela dos deputados
sql = '''CREATE TABLE public.deputados 
      ( id            character varying(10), 
        uri           character varying(100), 
        nome          character varying(500), 
        siglaPartido  character varying(50), 
        uriPartido    character varying(200), 
        siglaUf       character varying(10), 
        idLegislatura character varying(10), 
        urlFoto       character varying(100), 
        email         character varying(100) 
      )'''
criar_db(sql)
```

Com a tabela criada, podemos começar a inserir os dados.

### **Inserindo Dados no PostgreSQL**

A gravação dos dados no banco, envolve criar a função **inserir_db**. Aqui conectamos ao banco com nossa função **conecta_db**, depois abriremos uma sessão com o **cursor**. Utilizaremos o **try** e **except** para verificar a ocorrência de erros na inserção. Caso a função **execute** não apresente erro, realizamos o **commit** para finalizar a transação. Já em caso de erro usamos o **rollback** para manter a integridade do banco e exibimos o erro. Por fim, aplicamos a função **close** para encerrar a conexão.

```
# Função para inserir dados no banco
def inserir_db(sql):
    con = conecta_db()
    cur = con.cursor()
    try:
        cur.execute(sql)
        con.commit()
    except (Exception, psycopg2.DatabaseError) as error:
        print("Error: %s" % error)
        con.rollback()
        cur.close()
        return 1
    cur.close()
```

Vamos montar um loop com **for**, para inserir cada linha do DataFrame. Passamos no SQL as colunas do banco e as colunas do DataFrame relacionadas. Em cada iteração do loop usamos a função **inserir_db** para inserir o registro.

```
# Inserindo cada registro do DataFrame
for i in df.index:
    sql = """
    INSERT into public.deputados (id,uri,nome,siglaPartido,uriPartido,siglaUf,idLegislatura,urlFoto,email) 
    values('%s','%s','%s','%s','%s','%s','%s','%s','%s');
    """ % (df['id'][i], df['uri'][i], df['nome'][i], df['siglaPartido'][i], df['uriPartido'][i], df['siglaUf'][i], df['idLegislatura'][i], df['urlFoto'][i], df['email'][i])
    inserir_db(sql)
 
Error: syntax error at or near "Angelo"
LINE 3: ...s.camara.leg.br/api/v2/deputados/141439','Chico D'Angelo','P...
```

Apresentou o erro em apenas um registro, deixei para exemplificar nossa função. Aproveita e comenta como você corrigiu esse erro?

Agora para finalizar faremos uma consulta no banco de dados PostgreSQL.

### **Consultando Dados no PostgreSQL**

A função **consultar_db**, irá retornar os registros da consulta SQL que passaremos. Aqui vamos mais uma vez, conectar ao banco com nossa função **conecta_db**, depois abrimos uma sessão com o **cursor**. Em seguida aplicamos o **execute** para rodar o SQL e o **fetchall** vai retornar os dados e então usamos um loop **for** para colocar os dados em uma lista. Para finalizar, encerramos a conexão com o **close**.

```
# Função para consultas no banco
def consultar_db(sql):
  con = conecta_db()
  cur = con.cursor()
  cur.execute(sql)
  recset = cur.fetchall()
  registros = []
  for rec in recset:
    registros.append(rec)
  con.close()
  return registros
```

Faremos um simples select na tabela de deputados.

```
# Realizando a consulta no PostegreSQL
reg = consulta_db('select * from public.deputados')
```

Com os dados na variável **reg**, vamos transformar em um **DataFrame**, passando a variável **reg** e o nome das colunas dos dados.

```
# Tranformando os dados da consulta no PostegreSQL em DataFrame
df_bd = pd.DataFrame(reg, columns=['id','uri','nome','siglaPartido','uriPartido','siglaUf','idLegislatura','urlFoto','email'])
df_bd.head()
```

A função **head** exibirá os primeiros registros da consulta.

![img](https://lh6.googleusercontent.com/VxzZ3POi8Focy5w6r-KXF-bQkBlJqeq3BQjiVrrhB3prZBYmXMlbX8Y0eGxcgYQtk1TMhxIuM632l8OLOcO5TgDBtEp2aICxioQP5WDH9DLKGuVvw1FAz2E9UvkYkw)

Persistir os dados em um banco de dados, ou apenas consultar para fazer uma análise, são tarefas muito comuns no trabalho dos profissionais de dados.

## **Conclusões**

Enfim, chegamos ao último artigo da nossa trilogia **API com Python, o céu é o limite**. O objetivo foi mostrar como persistir dados com Python em um banco PostgreSQL, depois de uma ingestão de dados de uma API.

O código completo você pode conferir no [GitHub do Dados ao Cubo](https://github.com/dadosaocubo/api/blob/master/Manipulando Dados em PostgreSQL com Python.ipynb). Vimos como fazer a ingestão de dados de uma API, como realizar processamento paralelo com Python e fechamos persistindo esses dados em um banco relacional. 

Espero que tenha curtido nosso conteúdo sobre API, se gostou compartilhe para fortalecer a comunidade de dados aqui no Brasil. Não esqueça também de compartilhar seu *feedback* conosco, um grande abraço e até a próxima.

## **Conteúdos ao Cubo**

Por fim, deixo algumas sugestões de conteúdos que você pode encontrar no Dados ao Cubo, sempre falando sobre o mundo dos dados.

- [Boas Práticas de Visualização de Dados Parte I](https://dadosaocubo.com/boas-praticas-de-visualizacao-de-dados-parte-i/)
- [Boas Práticas de Visualização de Dados Parte II](https://dadosaocubo.com/boas-praticas-de-visualizacao-de-dados-parte-ii/)
- [Regressão com scikit-learn](https://dadosaocubo.com/regressao-com-scikit-learn/)
- [Engenharia de Atributos AKA Feature Engineering Parte I](https://dadosaocubo.com/engenharia-de-atributos-aka-feature-engineering-parte-i/)
- [Engenharia de Atributos AKA Feature Engineering Parte II](https://dadosaocubo.com/engenharia-de-atributos-aka-feature-engineering-parte-ii/)

#### VOCÊ PODE GOSTAR:

[![img](https://i1.wp.com/dadosaocubo.com/wp-content/uploads/2021/09/RasaAoCubov2.png?resize=448%2C316&ssl=1)](https://dadosaocubo.com/como-criar-um-chatbot-com-rasa-open-source/)

### [Como Criar um Chatbot com Rasa Open Source](https://dadosaocubo.com/como-criar-um-chatbot-com-rasa-open-source/)



[![img](https://i0.wp.com/dadosaocubo.com/wp-content/uploads/2021/08/AnalisandoLinkedIn.png?resize=448%2C316&ssl=1)](https://dadosaocubo.com/analisando-dados-do-linkedin/)

### [Analisando Dados do LinkedIn](https://dadosaocubo.com/analisando-dados-do-linkedin/)



[![img](https://i1.wp.com/dadosaocubo.com/wp-content/uploads/2021/08/DadosSpeedTest.png?resize=448%2C316&ssl=1)](https://dadosaocubo.com/velocidade-da-internet-com-a-biblioteca-speedtest-python/)

### [Velocidade da Internet com a Biblioteca SpeedTest Python](https://dadosaocubo.com/velocidade-da-internet-com-a-biblioteca-speedtest-python/)

<iframe name="like-comment-frame-179541360-71-61b25466c3550" src="https://widgets.wp.com/likes/#blog_id=179541360&amp;comment_id=71&amp;origin=dadosaocubo.com&amp;obj_id=179541360-71-61b25466c3550" height="18px" width="100%" frameborder="0" scrolling="no" style="margin: 0px; padding: 0px;"></iframe>

#### PUBLICAÇÕES MAIS LIDAS

[![EDA_D3](https://i0.wp.com/dadosaocubo.com/wp-content/uploads/2021/07/EDA_D3.jpeg?resize=448%2C316&ssl=1)](https://dadosaocubo.com/analise-exploratoria-de-dados-com-python-parte-i/)

## [Análise Exploratória de Dados com Python Parte I](https://dadosaocubo.com/analise-exploratoria-de-dados-com-python-parte-i/)

[27 de julho de 2020](https://dadosaocubo.com/2020/07/27/)

[![max-bohme-VvdiUHMxZzQ-unsplash](https://i1.wp.com/dadosaocubo.com/wp-content/uploads/2020/08/max-bohme-VvdiUHMxZzQ-unsplash-scaled.jpg?resize=448%2C316&ssl=1)](https://dadosaocubo.com/modelos-em-producao-com-streamlit/)

## [Modelos em Produção com Streamlit](https://dadosaocubo.com/modelos-em-producao-com-streamlit/)

[12 de agosto de 2020](https://dadosaocubo.com/2020/08/12/)

[![acoes](https://i0.wp.com/dadosaocubo.com/wp-content/uploads/2020/08/acoes-scaled.jpg?resize=448%2C316&ssl=1)](https://dadosaocubo.com/ciencia-de-dados-para-mercado-de-acoes-parte-i/)

## [Ciência de Dados para Mercado de Ações Parte I](https://dadosaocubo.com/ciencia-de-dados-para-mercado-de-acoes-parte-i/)

[3 de agosto de 2020](https://dadosaocubo.com/2020/08/03/)

#### NOVAS PUBLICAÇÕES

[![RasaAoCubov2](https://i1.wp.com/dadosaocubo.com/wp-content/uploads/2021/09/RasaAoCubov2.png?resize=448%2C316&ssl=1)](https://dadosaocubo.com/como-criar-um-chatbot-com-rasa-open-source/)

## [Como Criar um Chatbot com Rasa Open Source](https://dadosaocubo.com/como-criar-um-chatbot-com-rasa-open-source/)

*[2 de setembro de 2021](https://dadosaocubo.com/2021/09/02/)*

[![AnalisandoLinkedIn](https://i0.wp.com/dadosaocubo.com/wp-content/uploads/2021/08/AnalisandoLinkedIn.png?resize=448%2C316&ssl=1)](https://dadosaocubo.com/analisando-dados-do-linkedin/)

## [Analisando Dados do LinkedIn](https://dadosaocubo.com/analisando-dados-do-linkedin/)

*[26 de agosto de 2021](https://dadosaocubo.com/2021/08/26/)*

[![DadosSpeedTest](https://i1.wp.com/dadosaocubo.com/wp-content/uploads/2021/08/DadosSpeedTest.png?resize=448%2C316&ssl=1)](https://dadosaocubo.com/velocidade-da-internet-com-a-biblioteca-speedtest-python/)

## [Velocidade da Internet com a Biblioteca SpeedTest Python](https://dadosaocubo.com/velocidade-da-internet-com-a-biblioteca-speedtest-python/)

*[12 de agosto de 2021](https://dadosaocubo.com/2021/08/12/)*

[![DadosSpeechRecognition](https://i2.wp.com/dadosaocubo.com/wp-content/uploads/2021/08/DadosSpeechRecognition.png?resize=448%2C316&ssl=1)](https://dadosaocubo.com/reconhecimento-de-voz-com-a-biblioteca-speechrecognition-python/)

## [Reconhecimento de Voz com a Biblioteca SpeechRecognition Python](https://dadosaocubo.com/reconhecimento-de-voz-com-a-biblioteca-speechrecognition-python/)

*[5 de agosto de 2021](https://dadosaocubo.com/2021/08/05/)*

[![capa_analise_br](https://i0.wp.com/dadosaocubo.com/wp-content/uploads/2021/07/capa_analise_br.png?resize=448%2C316&ssl=1)](https://dadosaocubo.com/analisando-dados-do-brasileirao-serie-a/)

## [Analisando Dados do Brasileirão Série A](https://dadosaocubo.com/analisando-dados-do-brasileirao-serie-a/)

*[29 de julho de 2021](https://dadosaocubo.com/2021/07/29/)*

[Política de Privacidade](https://dadosaocubo.com/politica-de-privacidade/)

![Tudo sobre o mundo de dados!](https://dadosaocubo.com/wp-content/uploads/2021/03/NvLogoDadosAoCubo.png)

