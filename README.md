# Ansible Role: SAP ASE

Esta *role* tenta encapsular toda lógica para criação de um servidor
*SAP Sybase ASE Express Edition for Linux* e foi baseada em [ansible-sap-iq](https://github.com/andrewrothstein/ansible-sap-iq).

## Recursos

## Role Variables

See [meta/main.yml](meta/main.yml)

## Example Playbook

```yml
- hosts: servers
  roles:
    - fflch.sap-ase
```

## Comandos do isql

Conectar no servidor estando no mesmo, o -w1000 torna os outputs 
mas *bonitos*:

    isql -Usa -PSUA_SENHA -w1000

Conectar no servidor remotamente:

    isql -Usa -PSUA_SENHA -S192.168.100.14 -w1000

Ver e alterar o número de locks:

    1> sp_configure 'number of locks'
    2> go
    1> sp_configure 'number of locks', 100000 
    2> go
    
Ver o alterar o número de conexões que o servidor recebe:

    1> sp_configure 'number of user connections'
    2> go
    1> sp_configure 'number of user connections', 150
    2> go

Criar usuário dbmaint. Lista todos usuários:

    1> use master
    2> go
    1> sp_addlogin "dbmaint", "SuaSuperSenha123"
    2> go
    1> select name from syslogins
    2> go

Criar um banco chamado fflch_dbc. Lista databases:

    1> use master
    2> go
    1> create database fflch_dbc
    2> go
    1> sp_helpdb	
    2> go

Ativar a opção de truncate log on checkpoint para o banco de dados fflch_dbc:

    1> use master
    2> go
    1> sp_dboption fflch_dbc, "trunc log on chkpt", true
    2> go
    
Usuário dbmaint com privilégio de criar tabelas no Banco de Dados (alias de dbo):

    1> use fflch_dbc
    2> go
    1> sp_addalias dbmaint,dbo
    2> go
    1> sp_helpuser
    2> go

Mostrar qual banco de dados que estou connectado no momento:

    1> select db_name()
    2> go

Criar uma tabela no banco de dados fflch_dbc:
    
    1> use fflch_dbc
    2> go

    1>create table pagamentos (
     id int not null,
     nome varchar(160) not null,
     recebimento_em timestamp null,
    )
    6>go

Mostrar tabelas do banco de dados fflch_dbc:

    1> use fflch_dbc
    2> go
    1> exec sp_tables '%', '%', 'fflch_dbc',"'TABLE'"
    2> go

Você pode carregar um arquivo com seu sql, como por exemplo,
SeuArquivoComSql.sql assim:

    isql -Usa -PSuaSenha -iSeuArquivoComSql.sql

Conferir se o sort order é case-sensitive, compatível com ISO 8859-1(Latin-1)

    1> use master
    2> go
    1> sp_helpsort
    2> go

Pendẽncias:

 - Collation do banco é case-sensitive
 - Language = US English
 - 
 - Deve-se configurar uma profile para que o Banco possa ser aberto e utilizado. Sugere-se colocar o Banco de Dados recém-criado como default, no statement de inicialização do Servidor.
 - manter uma política de ‘backup’ diário do Banco de Dados
 - um ‘backup’ de transações (dump tran). O backup do transaction log não é essencial p/ replicação.
