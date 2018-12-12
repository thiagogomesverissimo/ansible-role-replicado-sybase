# Ansible Role: SAP ATE

Esta *role* tenta encapsular toda lógica para criação de um servidor
*SAP Sybase ASE Express Edition for Linux* e foi baseada em [ansible-sap-iq](https://github.com/andrewrothstein/ansible-sap-iq).

## Recursos 

 - “collation” do banco é case-sensitive
 - separação de log
 - Sort order tem que ser case-sensitive, compatível com ISO 8859-1(Latin-1)
 - Language = US English
 - Aumentar número de conexões
 - Aumentar o número de locks (valor mínimo sugerido: 30.000)
 - criar um usuário com nome dbmaint, com privilégio de criar tabelas no Banco de Dados (alias de dbo)
 - Deve-se configurar uma profile para que o Banco possa ser aberto e utilizado. Sugere-se colocar o Banco de Dados recém-criado como default, no statement de inicialização do Servidor.
 - ativar a opção de truncate log on checkpoint
 - manter uma política de ‘backup’ diário do Banco de Dados
 - um ‘backup’ de transações (dump tran). O backup do transaction log não é essencial p/ replicação.

## Role Variables

See [meta/main.yml](meta/main.yml)

## Example Playbook

```yml
- hosts: servers
  roles:
    - andrewrothstein.sap-iq
```
