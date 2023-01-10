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
Stored Procedure são procedimentos em SQL armazenados como objetos de código pré-compilado na base de dados e que podem ser executados diretamente no SGBD. Permitem assegurar eficiência e segurança.

A sintaxe para criação de um SP é:

``` sql
CREATE PROCEDURE <proc-name> 
		(param_spec1, param_spec2, …, param_specn ) 
BEGIN
	-- codigo a executar	
END;
```

onde os parâmetros podem ser definidos como de entrada ```IN```, saída ```OUT``` ou entrada e saída ```INOUT```.

Por exemplo
``` sql
DELIMITER $$

CREATE PROCEDURE sp_exemplo (IN param1 INT, OUT param2 INT)
BEGIN
	SELECT COUNT(*) INTO param2 
	FROM tabela
	WHERE num=param1;
END$$

DELIMITER ;
```

Neste exemplo a cláusula ```DELIMITER``` permite alterar o caracter delimitador de instruções para que este possa ser usado no corpo do bloco de código. No final é importante repôr o caracter ```;```.

Chamamos o procedimento usando a cláusula ```CALL```, por exemplo:
``` sql
CALL sp_exemplo(1, @a);
```

Podemos destruir um stored procedure usando a cláusula ```DROP```:
``` sql
DROP PROCEDURE sp_exemplo;
```

## 2. Error Handling
À semelhança dos Stored Procedures, as Stored Functions são um tipo de objeto constituído por um bloco de código précompilado armazenado na base de dados e que pode ser executado em qualquer momento. Uma função pode ser criada através da sintaxe:



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
