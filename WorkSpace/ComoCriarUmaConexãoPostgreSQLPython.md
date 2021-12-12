# Como criar uma conexão em PostgreSQL com Python

## Veja nesse artigo como trabalhar criar e modularizar conexões com o banco de dados PostgreSQL através de dois adaptadores: PSYCOPG e PY-POSTGRESQL.

- [Artigos](https://www.devmedia.com.br/artigos/)[Canal Mais](https://www.devmedia.com.br/artigos/outros)Como criar uma conexão em PostgreSQL com Python

Nesse artigo serão apresentados dois tipos de configuração para **conexão com o banco de dados PostgreSQL com Python**, muito úteis para agilizar o desenvolvimento: utilizando o PSYCOPG, que é considerado o adaptador mais utilizado, e também o PY-POSTGRESQL, que é considerado um provedor de interface mais otimizado.

Estamos usando para rodar os exemplos deste artigo a interface de desenvolvimento integrado LiClipse, baixando do seu site (vide seção **Links**) e escolhendo a opção “Get it Download”.

Após a instalação do mesmo, execute-o e aparecerá uma tela como a da **Figura 1**.

![Seleção do workspace](https://arquivo.devmedia.com.br/ARTIGOS/Igor_Estevao/python_oo/image004.jpg)**Figura 1**. Seleção do workspace

Esta janela se refere à localização do workspace, ou seja, o local no disco onde vão ficar os projetos. Para esse artigo vamos usar o valor padrão dos documentos do usuário logado. Depois de selecionado, basta escolher qual o tema desejado, como na **Figura 2**. Para esse artigo usaremos o primeiro tema.

![Escolha do tema](https://arquivo.devmedia.com.br/ARTIGOS/Igor_Estevao/python_oo/image005.jpg)**Figura 2**. Escolha do tema

Com isso o LiClipse estará pronto para ser utilizado, como mostra a **Figura 3**.

![Tela que ilustra como é o menu principal “File” com as opções para se trabalhar](https://arquivo.devmedia.com.br/ARTIGOS/Igor_Estevao/python_oo/image006.jpg)**Figura 3**. Tela que ilustra como é o menu principal “File” com as opções para se trabalhar

### PSYCOPG

Uma das maneiras da **linguagem Python reconhecer o banco de dados PostgreSQL** é através do adaptador PSYCOPG, que é considerado o mais utilizado.

Saiba mais: [Curso de Python](https://www.devmedia.com.br/curso/curso-python/2280)

Existem duas maneiras de instalá-lo:

1. Pelo instalador Windows, que usaremos nesse artigo ou pelo instalador MAC: baixe a versão apropriada do PSYCOPG no site (seção **Links**) e execute o instalador na pasta site-packages do Python;
2. Instalação Manual: baixe o pacote tar.gz em http://initd.org/psycopg/tarballs/PSYCOPG-2-6/psycopg2-2.6.tar.gz e descompacte o arquivo em alguma pasta conhecida. Em seguida, execute o prompt de comando, entre na pasta que você descompactou o pacote e acesse a pasta. Rode o seguinte comando para instalar:

```
c:\Python34\python setup.py install
```

### Configurando A Conexão Pelo PSYCOPG

Veja na **Listagem 1** como configurar a conexão com o Banco de Dados PostgreSQL utilizando o PSYCOPG. Lembre-se de criar um banco de dados “regiao” num projeto do [Liclipse](https://www.devmedia.com.br/python-orientado-a-objetos-com-o-framework-cherrypy/33489).

```
import psycopg2
con = psycopg2.connect(host='localhost', database='regiao',
user='postgres', password='postgres123')
cur = con.cursor()
sql = 'create table cidade (id serial primary key, nome varchar(100), uf varchar(2))'
cur.execute(sql)
sql = "insert into cidade values (default,'São Paulo,'SP')"
cur.execute(sql)
con.commit()
cur.execute('select * from cidade')
recset = cur.fetchall()
for rec in recset:
print (rec)
con.close()
```

**Listagem 1**. Configurando a Conexão

Veja que o código é responsável por criar rotinas de conexão, tabelas, inserção de dados e transações. Na linha 1 importamos a biblioteca PSYCOPG para que seus recursos possam ser utilizados durante a programação do módulo. Nas linhas 2 e 3 configuramos a conexão local ao banco de dados 'regiao'. Já a linha 4 recupera o objeto 'cur' para acesso ao banco. Em seguida montamos um SQL para criar a tabela, que é executado a seguir. Na linha 7 criamos um SQL para inserir uma linha na tabela e a executamos em seguida. A linha 9 força a execução das transações pendentes e na linha 10 executamos um SQL de consulta. Da linha 11 até a 13 recuperamos e exibimos os resultados e ao final fechamos a conexão.

O retorno será: (1, 'São Paulo', 'SP'), como mostra na **Figura 4.**

![Código sendo executado e exibido o resultado no Console do LiClipse](https://arquivo.devmedia.com.br/artigos/Igor_Estevao/postgre_pyton/image001.jpg)**Figura 4**. Código sendo executado e exibido o resultado no Console do LiClipse

O rec tem finalidade em retornar: print rec[0] print rec[1] print rec[2] ... print rec [n], ou seja, todos os registros buscados da tabela.

### Modularizando Classe Conexão

Vamos criar agora uma classe denominada “Conexao”, localizada na **Listagem 2,** cujo objetivo é modularizar o código criado na **Listagem 1** para que a classe se adapte de um modo geral e todo o seu comportamento fique mais rápido com relação ao tempo de resposta, melhorando o seu desempenho. Note que já foi criada a tabela ‘cidade’ anteriormente.

```
class Conexao(object):
   _db=None
   def __init__(self, mhost, db, usr, pwd):
    self._db = psycopg2.connect(host=mhost, database=db, user=usr,  password=pwd)
   def manipular(self, sql):
       try:
           cur=self._db.cursor()
           cur.execute(sql)
           cur.close();
         self._db.commit()
     except:
         return False;
     return True;
 def consultar(self, sql):
     rs=None
     try:
         cur=self._db.cursor()
         cur.execute(sql)
         rs=cur.fetchall();
     except:
         return None
     return rs
def proximaPK(self, tabela, chave):
     sql='select max('+chave+') from '+tabela
     rs = self.consultar(sql)
     pk = rs[0][0]
     return pk+1
def fechar(self):
     self._db.close()
```

**Listagem 2**. Código responsável por modularizar através da criação da classe Conexao

Na linha 3 definimos o método __init__() para iniciação com a passagem de parâmetros. Em seguida criamos a conexão com os objetos para o banco de dados: “db”, usuário “usr” e senha “pwd”. Além disso criamos um método responsável por manipular todas as funcionalidades. A linha 7 faz a varredura com o cursor, pesquisando todos os registros do banco de dados para, em seguida, executarmos o SQL.

Nas linhas 9 e 10 fechamos a conexão e encerramos as transações, respectivamente.

A partir da linha 14 é criado o método para consulta: o objeto ‘rs’ recebe um valor vazio. Na linha 19 esse mesmo objeto recebe o valor total da consulta existente. Na linha 22, se existir algum registro, então esse é retornado para o objeto ‘rs’. Já na linha 23 é criado o método proximaPK, que é responsável por retornar o maior índice da chave primária do registro inserido. Depois de executarmos o comando SQL de consulta na linha 24, a linha 26 recupera a primeira coluna da primeira linha, que é retornado para o objeto pk que será exibido. A linha 28 cria o método fechar para encerrar a conexão com o banco de dados.

Para complementar o código da **Listagem 2** vamos implementar a seguir o código da **Listagem 3**, podendo ser em um outro módulo, cuja finalidade é para que se assumam todos os valores e comportamentos de conexão, inserção, consulta e transação de um modo breve e simples.

```
con=Conexao('localhost','regiao','postgres','postgres123')
sql = "insert into cidade values (default,'Rio de Janeiro','RJ')"
if con.manipular(sql):
  print('inserido com sucesso!')
print (con.proximaPK('cidade', 'id'))
rs=con.consultar("select * from cidade")
for linha in rs:
  print (linha)
con.fechar()
```

**Listagem 3**. Código responsável por definir os valores dos parâmetros para a conexão e sua manipulação

O código começa com a criação do objeto ‘con’, onde é definido o local e nome para o banco de dados, usuário e senha respectivamente. Na linha 2 é criado o comando SQL para inserir novos valores (cidade e estado) para a tabela cidade (já criada anteriormente) Em seguida verificamos se foi inserido o registro anterior. Já a linha 5 imprime a cidade inserida. Na linha 6 é criado o objeto ‘rs’ para recuperar a consulta, que tem os registros impressos na linha 8. Ao final fechamos a conexão com o banco de dados. O resultado a ser exibido no Console será de acordo com a **Figura 5**, que mostra uma mensagem que foi inserido o registro com sucesso e abaixo os registros que foram inseridos até o momento.

![Tela do Console mostrando uma mensagem que foi inserido o registro com sucesso e abaixo os registros que foram inseridos até o momento](https://arquivo.devmedia.com.br/artigos/Igor_Estevao/postgre_pyton/image002.jpg)**Figura 5**. Tela do Console mostrando uma mensagem que foi inserido o registro com sucesso e abaixo os registros que foram inseridos até o momento

### PY-POSTGRESQL

Agora será apresentado o Py-Postgresql, que é um provedor de interface otimizado para conexão com o banco de dados PostgreSQL e o Python. Para começarmos precisamos primeiro baixar o pacote tar.gz no link da seção **Links.**

Após descompactar o arquivo em alguma pasta conhecida, execute o prompt do DOS e acesse a pasta que você descompactou o pacote e rode o seguinte comando:

```
c:\Python34\python setup.py install
```

Assim, o Py-postgresql será instalado na pasta site-packages do Python.

### Configurando a Conexão pelo Py-Postgresql

Veja na **Listagem 4** como configurar a conexão com o banco de dados PostgreSQL utilizando o adaptador Py-Postgresql. Nesse exemplo será possível abrir a conexão, criar uma tabela, inserir linha de registro e executar as transações em um módulo Python. Para esse exemplo precisamos já ter criado o banco de dados “teste” anteriormente.

```
import postgresql
 db = postgresql.open("pq://postgres:postgres123@localhost/teste")
 sql = 'create table cidade (id serial primary key, nome varchar(100), uf varchar(2))'
 db.execute(sql)
 sql = "insert into cidade values (default,'Curitiba,'PR')"
 try:
  db.execute(sql)
 except:
  print ("erro")
sql = "select * from cidade“
rs=db.prepare(sql)
for linha in rs:
  print (linha)
db.close()
```

**Listagem 4**. Código responsável por fazer a configuração e realizar alguns comportamentos

Começamos o código fazendo a importação do driver postgresql para utilizarmos os seus recursos durante a implementação. Em seguida abrimos a conexão local no banco de dados teste e montamos um SQL para criar a tabela e executamos. Na linha 5 criamos um SQL para inserir uma linha nessa tabela criada. Da linha 6 até a linha 9 executamos o bloco try except.

Na linha 10 executamos um SQL de consulta de seleção e em seguida preparamos outro comando SQL para ser executado, que por sua vez imprime a consulta, retornando todos os registros. Ao final, na linha 14 fecha a conexão.

O retorno será: (1, 'Curitiba', 'PR'), conforme mostra a **Figura 6**.

![Registro buscado da tabela “teste”](https://arquivo.devmedia.com.br/artigos/Igor_Estevao/postgre_pyton/image003.jpg)**Figura 6**. Registro buscado da tabela “teste”

### Modularizando a Classe Conexão

Na **Listagem 5** criamos a classe denominada Conexao, cujo fundamento é modularizar o código criado anteriormente na **Listagem 4**. Para que sua funcionalidade de conexão fique mais modular precisamos melhorar o desempenho da aplicação e torna-la mais rápida com relação ao tempo de resposta. Note que já foi criada a tabela ‘cidade’ anteriormente.

```
import postgresql
class Conexao(object):
   _db=None;
   def __init__(self, banco):
       self._db = postgresql.open(banco)
   def manipular(self, sql):
       try:
           self._db.execute(sql)
       except:
          return False;
      return True;
  def consultar(self, sql):
      rs=None
      try:
          rs=self._db.prepare(sql)
      except:
          return None
      return rs
  def proximaPK(self, tabela, chave):
      sql='select max('+chave+') from '+tabela
      rs = self.consultar(sql)
      pk = rs.first()
      return pk+1
  def fechar(self):
      self._db.close()
```

**Listagem 5**. Código responsável por modularizar a configuração com o banco de dados

Começamos criando a classe Conexao na linha 2. Já na linha 4 é criada o método de iniciação __init__ com o parâmetro do banco. Em seguida é criada o objeto que recebe o método para abrir o banco, além do método ‘manipular’, que efetua as transações para executar o SQL. Na linha 12 é definido o método de consulta e o objeto ‘rs’ é criado na linha 14. A partir da linha 15 esse ‘rs’ prepara o SQL para ser executado e, na linha 18, esse mesmo objeto retorna a consulta.

Na linha 19 é criado o método proximaPK, que verifica se a chave primária existe e, em seguida, na linha 20, temos o SQL que faz a consulta do maior índice, criado através do registro inserido. Na linha 21 temos o objeto ‘rs’ recebendo o valor da consulta por SQL. Já na linha 22 temos o objeto pk recebendo o primeiro registro da tabela e em seguida o seu retorn do objeto ‘pk’. As linhas 24 e 25 criam o método fechar e encerram a conexão.

Com o objetivo de integrar o código anterior da **Listagem 5** vamos implementar a seguir o código da **Listagem 6**, podendo ser implementado em um outro módulo, para que assuma todos os comportamentos e parâmetros de conexão, inserção, consulta e transação.

```
import postgresql
from Conexao import *
con=Conexao("pq://postgres:postgres123@localhost/teste")
sql = "insert into cidade values (default,'Bahia','BA')"
if con.manipular(sql):
  print('inserido com sucesso!')
print (con.proximaPK('cidade', 'id'))
rs=con.consultar("select * from cidade")
for linha in rs:
print (linha)
con.fechar()
```

**Listagem 6**. Passagem de parâmetros

Esse código efetua a passagem de parâmetros para conexão, realiza inserção na tabela e retorna seus registros, começando pela linha 2, que faz a importação da classe Conexao. Em seguida é criado o objeto ‘con' para conexão com suas diretrizes e nome do banco. Na linha 4 é criado o comando para inserir valores na tabela cidade e na linha 5 verifica-se se foi inserido com sucesso. A linha 7 imprime o maior registro inserido. O objeto rs recebe a consulta da tabela cidade pelo código da linha 8 e na linha 10 imprime-se os registros da tabela. A conexão é fechada na linha 11.

Veja na **Figura 7** o resultado da inserção e busca dos registros da tabela “teste” utilizando modularização.

![Console exibindo uma mensagem de inserção com sucesso e a retorno de todos os registros armazenados](https://arquivo.devmedia.com.br/artigos/Igor_Estevao/postgre_pyton/image004.jpg)**Figura 7**. Console exibindo uma mensagem de inserção com sucesso e a retorno de todos os registros armazenados

Vimos nesse artigo como é fácil instalar e configurar o Psycopg para implementar a API Python DB 2.0, que é usada para conectar a aplicação com o banco PostgreSQL. Com esse adaptador conseguimos que diversas extensões permitam acesso a muitos dos recursos oferecidos por esse banco, como o thread-safe para usar conexões diferentes ou compartilhar a mesma configuração.

Já o py- postgresql nos fornece um conjunto de módulos em Python para nos ajudar a conversar com o banco por meio de interfaces. Para usar este é interessante ter o Psycopg instalado, pois proporciona uma maior utilidade.

Espero que tenham gostado!

**Links**:

- [PostgreSQL](https://www.postgresql.org/)
- [Psycopg](http://initd.org/psycopg/)
- [Py-postgresql](https://pypi.org/project/py-postgresql/)
- [PSYCOPG para Windows](http://www.stickpeople.com/projects/python/win-psycopg/)

Tecnologias:

- [Banco de Dados](https://www.devmedia.com.br/banco-de-dados/)
-  

- [PostgreSQL](https://www.devmedia.com.br/postgresql/)
-  

- [Python](https://www.devmedia.com.br/python/)
-  

- [SQL](https://www.devmedia.com.br/sql/)