# aula14
Nesta aula introduzimos Triggers, blocos de código em PL/SQL que são executados de forma automática na ocorrência de um evento. Introduzimos ainda cláusulas que permitem o tratamento de erros em SQL para definição de comportamento na ocorrência de erros durante a execução de blocos de código.
Bom trabalho!

[0. Requisitos](#0-requisitos)

[1. Triggers](#1-triggers)

[2. Error Handling](#2-error-handling)

[3. Resoluções](#3-resoluções)

[Bibliografia e Referências](#bibliografia-e-referências)

[Outros](#outros)

## 0. Requisitos
Requisitos: Para esta aula, precisa de ter o ambiente de trabalho configurado ([Docker](https://www.docker.com/products/docker-desktop/) com [base de dados HR](https://github.com/ULHT-BD/aula03/blob/main/docker_db_aula03.zip) e [DBeaver](https://dbeaver.io/download/)). Caso ainda não o tenha feito, veja como fazer seguindo o passo 1 da [aula03](https://github.com/ULHT-BD/aula03/edit/main/README.md#1-prepare-o-seu-ambiente-de-trabalho).

Caso já tenha o docker pode iniciá-lo usando o comando ```docker start mysgbd``` no terminal, ou através do interface gráfico do docker-desktop:
<img width="1305" alt="image" src="https://user-images.githubusercontent.com/32137262/194916340-13af4c85-c282-4d98-a571-9c4f7b468bbb.png">

Deve também ter o cliente DBeaver.

## 1. Triggers
Enquanto Stored Procedure são blocos de código em SQL que podem ser invocados explicitamente quando se pretende a sua execução, Triggers são blocos de código igualmente armazenados como objetos de código pré-compilado na base de dados que são executados automaticamente quando um dado evento ocorre na base de dados (por exemplo uma inserção de um tuplo, atualização, etc).

A sintaxe para criação de um Trigger é:
``` sql
CREATE TRIGGER trigger_name
    trigger_time trigger_event
    ON tbl_name FOR EACH ROW
    [trigger_order]
    trigger_body
```

onde trigger_time indica se deve ser executado antes ```BEFORE``` ou após ```AFTER``` o evento trigger_event (```INSERT```, ```UPDATE```, ```DELETE```). trigger_order permite definir ordem (```FOLLOWS```, ```PRECEDES```) de sequência de triggers quando necessário.

Por exemplo
``` sql
DELIMITER \\

CREATE TRIGGER inscreveBD AFTER INSERT ON Aluno
       FOR EACH ROW
       INSERT INTO inscricoes VALUES (NEW.numero, ‘BD’);\\
      
DELIMITER ;
```

```NEW``` e ```OLD``` permitem manipular tuplos antes ou depois de alteracao ou insercao. No exemplo ```NEW``` representa o tuplo que acaba de ser inserido.


## 2. Error Handling
Quando executamos blocos de código e importante definir o comportamento do codigo no caso da ocorrencia de erros, por exemplo, o que devera acontecer se na execucao de um procedimento uma restricao de integridade for infringida ou se uma transacao falhar? Conseguimos definir o comportamento atraves da definicao de handlers que definem o comportamento para um dado tipo de erros.

A sintaxe e:
``` sql
DECLARE action
HANDLER FOR 
    condition_value
    statement;
```

* onde action define o comportamento no caso da ocorrencia do erro. Pode ser ```CONTINUE``` para continuar a execucao ou ```EXIT``` para interromper a execucao
* condition_value e a condição que provoca activação do handler. Pode ser um MySQL error code, SQLSTATE (SQLWARNING , NOTFOUND, SQLEXCEPTION)
* statement e a instrucao ou bloco de instrucoes a executar em caso e erro

exemplo:
``` sql
DELIMITER $$

CREATE PROCEDURE exemplo2 (IN param1 INT, IN param2 INT)
BEGIN

	DECLARE EXIT HANDLER FOR 1062 
	BEGIN
		SELECT ’encontrado duplicado na inserção’ AS message;
		RESIGNAL;
	END;

	-- insert tuple
	INSERT INTO tabela VALUES (param1, param2);
END$$

DELIMITER ;
```

### Exercícios
1. Escreva o código PL/SQL que permite criar o procedimento que cria um novo job (job_id, job_title, min_salary, max_salary) e que caso a inserção seja duplicada interrompe execução com mensagem de erro

2. Escreva o código PL/SQL que permite criar o procedimento para proceder a aumentos salariais tal que, numa transação, managers são aumentados 5% enquanto que trabalhadores são aumentados 10%. No caso de erro na transação deve ser feito rollback de devolvida uma mensagem de erro

3. Escreva o código PL/SQL que permite criar um trigger tal que sempre que um novo funcionario e adicionado, os valores de salario minimo e maximo para essa funcao sao atualizados;


## 3. Resoluções
[Resolução dos exercícios em aula](https://github.com/ULHT-BD/aula13/blob/main/aula13_resolucao.sql)


## Bibliografia e Referências
* [Slides aula](https://github.com/ULHT-BD/aula14/blob/main/Aula14.pdf) 
* [Documentação Error Handling - tutorialspoint](https://www.mysqltutorial.org/mysql-error-handling-in-stored-procedures/)
* [Documentação Error Handling - MySQL](https://dev.mysql.com/doc/refman/8.0/en/declare-handler.html)
* [Triggers - MySQL](https://dev.mysql.com/doc/refman/8.0/en/trigger-syntax.html)


## Outros
Para dúvidas e discussões pode juntar-se ao grupo slack da turma através do [link](
https://join.slack.com/t/ulht-bd/shared_invite/zt-1iyiki38n-ObLCdokAGUG5uLQAaJ1~fA) (atualizado 2022-11-03)
