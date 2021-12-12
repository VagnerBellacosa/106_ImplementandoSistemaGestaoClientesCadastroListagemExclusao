# Início Rápido: Usar o Python para se conectar e consultar dados no Banco de Dados do Azure para PostgreSQL – Servidor Flexível

- Artigo
- 13/05/2021
- 5 minutos para o fim da leitura
- - [![img](https://github.com/sunilagarwal.png?size=32)](https://github.com/sunilagarwal)
  - [![img](https://github.com/olprod.png?size=32)](https://github.com/olprod)

Esta página é útil?

 Importante

O Servidor Flexível do Banco de Dados do Azure para PostgreSQL está em versão prévia

Neste guia de início rápido, será possível se conectar a um Servidor Flexível do Banco de Dados do Azure para PostgreSQL usando o Python. Você usará instruções SQL para consultar, inserir, atualizar e excluir dados no banco de dados de plataformas Windows, Ubuntu Linux e Mac.

Este artigo pressupõe que você está familiarizado com o desenvolvimento usando Python, mas começou recentemente a trabalhar com o Banco de Dados do Azure para PostgreSQL.

## Pré-requisitos

- Uma conta do Azure com uma assinatura ativa. [Crie uma conta gratuitamente](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).
- Um Banco de Dados do Azure para PostgreSQL – Servidor Flexível. Para criar um servidor flexível, confira [Criar um Banco de Dados do Azure para PostgreSQL – Servidor Flexível usando o portal do Azure](https://docs.microsoft.com/pt-br/azure/postgresql/flexible-server/quickstart-create-server-portal).
- [Python](https://www.python.org/downloads/) 2.7 ou 3.6+.
- Instalador do pacote [pip](https://pip.pypa.io/en/stable/installing/) mais recente.

## Como preparar sua estação de trabalho cliente

- Caso tenha criado um servidor flexível com *acesso privado (Integração VNet)* , será necessário se conectar ao servidor de um recurso na mesma VNet do servidor. Crie uma máquina virtual e adicione-a à VNET criada com o servidor flexível. Confira [Criar e gerenciar a rede virtual do Banco de Dados do Azure para PostgreSQL – Servidor Flexível usando a CLI do Azure](https://docs.microsoft.com/pt-br/azure/postgresql/flexible-server/how-to-manage-virtual-network-cli).
- Caso tenha criado um servidor flexível com *acesso público (endereços IP permitidos)* , será possível adicionar seu endereço IP local à lista de regras de firewall no servidor. Confira [Criar e gerenciar regras de firewall do Banco de Dados do Azure para PostgreSQL – Servidor Flexível usando a CLI do Azure](https://docs.microsoft.com/pt-br/azure/postgresql/flexible-server/how-to-manage-firewall-cli).

## Instalar as bibliotecas do Python para PostgreSQL

O módulo [psycopg2](https://pypi.python.org/pypi/psycopg2/) permite estabelecer conexão e consultar um banco de dados PostgreSQL e está disponível como um pacote [indicador](https://pythonwheels.com/) do Linux, macOS ou Windows. Instale a versão binária do módulo, incluindo todas as dependências. Para saber mais sobre instalação e requisitos de `psycopg2`, confira [Instalação](http://initd.org/psycopg/docs/install.html).

Para instalar `psycopg2`, abra um terminal ou um prompt de comando e execute o comando `pip install psycopg2`.

## Obter informações da conexão de banco de dados

A conexão a Banco de Dados do Azure para PostgreSQL – Servidor Flexível requer o nome do servidor totalmente qualificado e as credenciais de logon. Você pode obter essas informações no portal do Azure.

1. No [portal do Azure](https://portal.azure.com/), pesquise e selecione o nome do servidor flexível.

2. Na página **Visão Geral** do servidor, copie o **Nome do servidor** totalmente qualificado e o **Nome de usuário do administrador**. O **Nome do servidor** totalmente qualificado está sempre no formato *<my-server-name>.postgres.database.azure.com*.

   Você também precisa da sua senha de administrador. Se você se esquecer dele, poderá redefini-lo na página de visão geral.

## Como executar os exemplos de Python

Para cada exemplo de código neste artigo:

1. Crie um arquivo em um editor de texto.
2. Adicione o exemplo de código ao arquivo. No código, substitua:
   - `<server-name>` e `<admin-username>` pelos valores copiados do portal do Azure.
   - `<admin-password>` pela sua senha de servidor.
   - `<database-name>` pelo nome do banco de dados referente ao Banco de Dados do Azure para PostgreSQL – Servidor Flexível. Um banco de dados padrão chamado *postgres* foi criado automaticamente quando você criou seu servidor. É possível renomear esse banco de dados ou criar outro usando os comandos SQL.
3. Salve o arquivo na pasta do seu projeto com uma extensão *.py*, como *postgres-insert.py*. Para Windows, verifique se a codificação UTF-8 está selecionada quando você salvar o arquivo.
4. Para executar o arquivo, altere a pasta do projeto em um interface de linha de comando e digite `python` seguido pelo nome do arquivo, por exemplo `python postgres-insert.py`.

## Criar uma tabela e inserir dados

O exemplo de código a seguir conecta-se ao seu banco de dados referente ao Banco de Dados do Azure para PostgreSQL – Servidor Flexível usando a função [psycopg2.connect](http://initd.org/psycopg/docs/connection.html) e carrega os dados com uma instrução **INSERT**. A função [cursor.execute](http://initd.org/psycopg/docs/cursor.html#execute) executa a consulta SQL no banco de dados.

PythonCopiar

```Python
import psycopg2
# Update connection string information 
host = "<server-name>"
dbname = "<database-name>"
user = "<admin-username>"
password = "<admin-password>"
sslmode = "require"
# Construct connection string
conn_string = "host={0} user={1} dbname={2} password={3} sslmode={4}".format(host, user, dbname, password, sslmode)
conn = psycopg2.connect(conn_string) 
print("Connection established")
cursor = conn.cursor()
# Drop previous table of same name if one exists
cursor.execute("DROP TABLE IF EXISTS inventory;")
print("Finished dropping table (if existed)")
# Create a table
cursor.execute("CREATE TABLE inventory (id serial PRIMARY KEY, name VARCHAR(50), quantity INTEGER);")
print("Finished creating table")
# Insert some data into the table
cursor.execute("INSERT INTO inventory (name, quantity) VALUES (%s, %s);", ("banana", 150))
cursor.execute("INSERT INTO inventory (name, quantity) VALUES (%s, %s);", ("orange", 154))
cursor.execute("INSERT INTO inventory (name, quantity) VALUES (%s, %s);", ("apple", 100))
print("Inserted 3 rows of data")
# Clean up
conn.commit()
cursor.close()
conn.close()
```

Quando o código é executado com êxito, ele produz a seguinte saída:

![Saída de linha de comando](https://docs.microsoft.com/pt-br/azure/postgresql/flexible-server/media/connect-python/2-example-python-output.png)

## Ler dados

O exemplo de código a seguir conecta-se ao seu banco de dados referente ao Banco de Dados do Azure para PostgreSQL – Servidor Flexível e usa [cursor.execute](http://initd.org/psycopg/docs/cursor.html#execute) com a instrução **SELECT** do SQL para leitura de dados. Essa função aceita uma consulta e retorna um conjunto de resultados a ser iterado usando [cursor.fetchall()](http://initd.org/psycopg/docs/cursor.html#cursor.fetchall).

PythonCopiar

```Python
import psycopg2
# Update connection string information
host = "<server-name>"
dbname = "<database-name>"
user = "<admin-username>"
password = "<admin-password>"
sslmode = "require"
# Construct connection string
conn_string = "host={0} user={1} dbname={2} password={3} sslmode={4}".format(host, user, dbname, password, sslmode)
conn = psycopg2.connect(conn_string) 
print("Connection established")
cursor = conn.cursor()
# Fetch all rows from table
cursor.execute("SELECT * FROM inventory;")
rows = cursor.fetchall()
# Print all rows
for row in rows:
    print("Data row = (%s, %s, %s)" %(str(row[0]), str(row[1]), str(row[2])))
# Cleanup
conn.commit()
cursor.close()
conn.close()
```

## Atualizar dados

O exemplo de código a seguir conecta-se ao seu banco de dados referente ao Banco de Dados do Azure para PostgreSQL – Servidor Flexível e usa [cursor.execute](http://initd.org/psycopg/docs/cursor.html#execute) com a declaração de **UPDATE** do SQL para atualização de dados.

PythonCopiar

```Python
import psycopg2
# Update connection string information
host = "<server-name>"
dbname = "<database-name>"
user = "<admin-username>"
password = "<admin-password>"
sslmode = "require"
# Construct connection string
conn_string = "host={0} user={1} dbname={2} password={3} sslmode={4}".format(host, user, dbname, password, sslmode)
conn = psycopg2.connect(conn_string) 
print("Connection established")
cursor = conn.cursor()
# Update a data row in the table
cursor.execute("UPDATE inventory SET quantity = %s WHERE name = %s;", (200, "banana"))
print("Updated 1 row of data")
# Cleanup
conn.commit()
cursor.close()
conn.close()
```

## Excluir dados

O exemplo de código a seguir conecta-se ao seu banco de dados referente ao Banco de Dados do Azure para PostgreSQL – Servidor Flexível e usa [cursor.execute](http://initd.org/psycopg/docs/cursor.html#execute) com a instrução **DELETE** do SQL para exclusão de um item de inventário que você inseriu anteriormente.

PythonCopiar

```Python
import psycopg2
# Update connection string information
host = "<server-name>"
dbname = "<database-name>"
user = "<admin-username>"
password = "<admin-password>"
sslmode = "require"
# Construct connection string
conn_string = "host={0} user={1} dbname={2} password={3} sslmode={4}".format(host, user, dbname, password, sslmode)
conn = psycopg2.connect(conn_string) 
print("Connection established")
cursor = conn.cursor()
# Delete data row from table
cursor.execute("DELETE FROM inventory WHERE name = %s;", ("orange",))
print("Deleted 1 row of data")
# Cleanup
conn.commit()
cursor.close()
conn.close()
```

## Próximas etapas

[Como migrar seu banco de dados usando o despejo e a restauração](https://docs.microsoft.com/pt-br/azure/postgresql/howto-migrate-using-dump-and-restore)