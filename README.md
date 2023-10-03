# Banco-de-Dados
-- Comando para seleção
SELECT * FROM produtos ORDER BY id DESC;
-- COMMANDS DDL  linguagem de definição de dados (Interagir com a tabela inteira)
-- Criação de tabela
CREATE TABLE produtos(
	id INT PRIMARY KEY NOT NULL,
	nome VARCHAR (255),
	preco DECIMAL(10,2)
);
-- Excluir uma tabela 
DROP TABLE produtos;
-- alterar tabela, adicionar coluna
ALTER TABLE produtos ADD descricao TEXT;
--excluir coluna
ALTER TABLE produtos DROP descricao;
--DML - Linguagem de manipulaçaõ de dados(utilizar a linha)
--Inserir dado na tabela
INSERT INTO produtos(id, nome, preco) VALUES
(1, 'Nescau Radical', 8.00),
(2, 'Yakult', 4.00),
(3, 'Alpino amargo', 5.00),
(4, 'Miojo Monica', 2.00);
--Alterar dado da tabela 
UPDATE produtos SET preco = 15.00 WHERE id=1;
UPDATE produtos SET nome = 'Toddy' WHERE nome LIKE 'Nescau Radical';
UPDATE produtos SET descricao = 'Armazenar em ambiente frio'
WHERE id=2 or id=3;
UPDATE produtos SET descricao = 'Promoção'
WHERE  preco <5.00;
--Deletar dado na tabela
DELETE FROM produtos WHERE nome LIKE 'Alpino amargo';
DELETE FROM produtos WHERE id=1;
--Comandos DQL (Linguagem de consulta de dados)
--Seleccionar todos "*" os produtos, ordene ascendente
SELECT * FROM produtos ORDER BY id DESC;
--Ordenar pelo preço crescente(Do menor para o maior)
SELECT * FROM produtos ORDER BY preco ASC;
--Ordenar pelo preço decrecente(Do maior pra o menor)
SELECT * FROM produtos ORDER BY preco DESC;
--Selecionar Colunas
SELECT nome,preco FROM produtos ORDER BY id;
--Selecionar produtos maiores que R$4,00
SELECT preco,nome FROM produtos WHERE preco > 4.00;
--Selecionar produto mais caro
SELECT MAX(preco) AS maior_valor FROM produtos;
SELECT preco, nome FROM produtos ORDER BY  preco DESC limit 1;
--Simulando procura por nome exato
SELECT * FROM produtos WHERE nome LIKE 'Nescau Radical';
--Procura pelos primeiros caracteres, o resto não importa
SELECT * FROM produtos WHERE nome LIKE 'Ne%';
--A parte anterior não importa, encontra o ultimo caracter
SELECT * FROM produtos WHERE nome LIKE '%Fit';
--Procura por qualquer parte do nome
SELECT * FROM produtos WHERE nome LIKE '%Nike%';

-- LEVEL 2 --------------------------------------
--Criando tabela Funcionário---------------------
CREATE TABLE funcionario( 
	id  SERIAL PRIMARY KEY,
	nome VARCHAR(255) NOT NULL,
	data_nasc DATE,
	sexo CHAR(1),
	salario DECIMAL(10,2),
	ativo BOOLEAN
);
SELECT * FROM funcionario;
INSERT INTO funcionario(nome, data_nasc,sexo,salario,ativo) VALUES
('Bob', '1990-05-15','M', 2000.00, true),
('Augusto', '1970-04-17', 'M', 1500.00, false),
('Joanir', '2000-01-01', 'F', 1800.00, false),
('Elisa', '1995-10-02', 'F', 1900.00, true);
SELECT * FROM funcionario;
--Seleciona funcionários que nasceram após 01-01-1992 
SELECT * FROM funcionario WHERE data_nasc > '1992-01-01';
--Selciona funcionários em um período
SELECT nome, data_nasc FROM funcionario
WHERE data_nasc BETWEEN '1990-01-01' AND '1997-01-01';
--Contabilizar
SELECT sexo, COUNT(*) AS total_pessoas,   --Contar
AVG(salario) AS media_salarial,           --Média
FROM funcionario GROUP BY sexo;
--Selciona funcionário inativo
SELECT * FROM funcionario WHERE ativo = false;
--1.Criar uma tabela "Clientes" com campos para id, nome e email.
--2.Adicionar uma coluna "Telefone" à tabela "Clientes" usando o comando ALTER TABLE.
--3.Remover a coluna "Email" da tabela "Clientes" usando ALTER TABLE.
--4.Criar uma tabela "itens" com campos para id,nome e preço.
--5.Inserir um novo cliente na tabela "Clientes" INSERT.
--6.Inserir 3 novos itens na tabela "itens" INSERT.
--7.Atualizar o nome de um cliente na tabela "Clientes" UPDATE.
--8.Atualizar o preço de um item na tabela "itens" UPDATE.
--9.Excluir um cliente da tabela de "Clientes" DELETE.
--10.Excluir um item de tabela "itens" DELETE.
SELECT * FROM Clientes ORDER BY id ASC;
CREATE TABLE Clientes(
id INT PRIMARY KEY NOT NULL,
	nome VARCHAR(255),
	email VARCHAR(255)
);
ALTER TABLE Clientes ADD Telefone VARCHAR(20);
ALTER TABLE Clientes DROP email;
SELECT * FROM itens ORDER BY id ASC;
CREATE TABLE itens(
id INT PRIMARY KEY NOT NULL,
	nome VARCHAR(60),
	preco DECIMAL(10,2)
);
INSERT INTO Clientes(id,nome,Telefone) VALUES
(1,'Kimahri','(44)9978-07749');
UPDATE Clientes SET nome = 'Tidus' WHERE nome LIKE 'Kimahri';
INSERT INTO itens(id,nome,preco) VALUES
(1,'Mandioca',10.00),
(2,'Batata',6.00 ),
(3,'Alface',5.00 );
UPDATE itens SET preco = 10.00  WHERE id=3;
DELETE FROM Clientes WHERE nome LIKE 'Kimahri';
DELETE FROM itens WHERE nome LIKE 'Batata';
--Entendo relacionamentos entre tabelas no banco de dados
CREATE TABLE estados(
id_estado SERIAL PRIMARY KEY,
	nome_estado  VARCHAR(50) NOT NULL,
	sigla CHAR(2)
);
CREATE TABLE cidades(
	id_cidade SERIAL PRIMARY KEY,
	nome_cidade VARCHAR(50),
	cep VARCHAR(50),
	id_estado INT REFERENCES estados(id_estado)
);
--Consultador de tabela
SELECT * FROM estados;
SELECT * FROM cidades;
--INSERINDO DADOS NAS TABELAS
INSERT INTO estados(id_estado,nome_estado, sigla) VALUES
(1,'Paraná','PR'),
(2,'São Paulo','SP'),
(3,'Minas Gerais','MG');
INSERT INTO cidades(id_cidade, nome_cidade, cep, id_estado) VALUES
(10,'NOVA LONDRINA','87970-000',1 ),
(11, 'MARILENA','87960-000',1),
(12,'ITAÚNA','87980-000',1),
(13,'PRIMAVERA','19273-000',2),
(14,'ROSANA','19274-000',2),
(15,'CACHOEIRA DA PRATA','35765-000',3);
--TRABALHANDO COM RELACIONAMENTOS NO SQL
--SELECIONAR TODAS AS CIDADES E SEUS ESTADOS 
SELECT cidades.nome_cidade,estados.nome_estado
FROM estados INNER JOIN cidades
ON cidades.id_estado = estados.id_estado;
--SELECIONA TODAS AS CIDADES DO PARANÁ
SELECT cidades.nome_cidade, estados.sigla
FROM estados INNER JOIN cidades
ON cidades.id_estado = estados.id_estado
WHERE cidades.sigla LIKE 'PR'
ORDER BY cidades.nome_cidade ASC;
--SELECIONA OS ESTADOS COM MAIS CIDADES
SELECT estados.nome_estado, COUNT(cidades.id_cidade) AS total_cidades
FROM estados INNER JOIN 	cidades
ON estados.id_estado = cidades.id_estado
GROUP BY estados.nome_estado
ORDER BY total_cidades DESC;
--CRIANDO MAIS UMA TABELA PARA O RELACIONAMENTO DE CIDADE-ESTADO
CREATE TABLE pessoa(
	id_pessoa SERIAL PRIMARY KEY,
	nome_pessoa VARCHAR(60),
	data_nascimento DATE,
	sexo CHAR(1),
	estado_civil VARCHAR(60),
	profissao VARCHAR(60),
	id_cidade INT REFERENCES cidades(id_cidade)
);
SELECT * FROM pessoa;
INSERT INTO pessoa
(id_pessoa, nome_pessoa, data_nascimento, sexo, estado_civil, profissao, id_cidade)
VALUES
(1, 'Marcele', '2000-01-01','F','Solteira','Arquiteta',10),
(2, 'Ananias', '1980-02-20','M','Casado','Carpinteiro',10),
(3, 'Silvio', '1950-10-10','M','Casado', 'Dublador',11),
(4, 'Léo', '2001-01-02','M','Casado', 'Entregador',11),
(5, 'Chris', '1990-05-05','M','Solteiro', 'Fisiculturista',12),
(6, 'Ana', '1997-04-01','F','Solteira', 'Cantora',13),
(7, 'Jaime', '1970-03-01','M','Casado', 'Carteiro',14),
(8, 'ALice', '2002-01-10','F','Solteira', 'Advogada',15),
(9, 'Valentina', '19995-05-03','F','Solteira', 'Zootecnista',15),
(10, 'Laura', '1998-05-09','F','Solteira', 'Balconista',15);
--Selecionar pessoa com sua cidade (relacionamento tabela pessoa com cidade)
SELECT pessoa.nome_pessoa, cidades.nome_cidade
FROM pessoa INNER JOIN cidades
ON pessoa.id_cidade = cidades.id_cidade
--Seleciona Estado, Cidade, Pessoa
SELECT pessoa.nome_pessoa, cidades.nome_cidade, estados.nome_estado
FROM pessoa INNER JOIN cidades
ON pessoa.id_cidade = cidades.id_cidade
INNER JOIN estados
ON cidades.id_estado = estados.id_estado;
--Seleciona pessoas do Estado do Paraná
SELECT pesssoa.nome_pessoa, estados.nome_estado, cidades.nome_cidade
FROM estados INNER  JOIN cidades
ON estados.id_estado = cidades.id_estado
INNER JOIN cidades.id_cidade = pessoa.id_cidade
WHERE estados.nome_estado LIKE 'Paraná';
--Selecionar o nome da pessoa, profisssão e qual cidade pertence
SELECT pessoa.nome_pessoa, cidades.nome_cidade, pessoa.profissao
FROM pessoa INNER JOIN cidades
ON pessoa.id_cidade = cidades.id_cidade
--Selecionar cidade com mais mulher/homem
SELECT cidades.nome_cidade, COUNT(*) AS Quantidade
FROM cidades INNER JOIN pessoa
ON cidades.id_cidade = pessoa.id_cidade
WHERE pessoa.sexo = 'M'
GROUP BY cidades.id_cidade
ORDER BY Quantidade desc;
--1 - Seleciona todas as pessoas
SELECT pessoa.nome_pessoa FROM pessoa;
--2 - Selecione todas as pessoas do sexo Masculino "m"
SELECT pessoa.nome_pessoa FROM pessoa
WHERE pessoa.sexo LIKE 'M';
--3 - Selecione todas as pessoas com estado civil solteiro
SELECT pessoa.nome_pessoa, pessoa.estado_civil FROM pessoa
WHERE pessoa.estado_civil LIKE 'solteiro'
OR pessoa.estado_civil LIKE 'solteira';
--4 - Selecione todas as pessoas e sua profissão
SELECT pessoa.nome_pessoa, pessoa.profissao FROM pessoa;
--5 - Selecione as pessoas que nasceram entre 19080-01-01 e 2000-01-01
SELECT pessoa.nome_pessoa, pessoa.data_nascimento
FROM pessoa WHERE data_nascimento  BETWEEN '1980-01-01' AND '2000-01-01';
--6 - Selecione as pessoas do estado de São Paulo
SELECT pessoa.nome_pessoa, estados.nome_estado 
FROM pessoa INNER JOIN cidades
ON pessoa.id_cidade = cidades.id_cidade
INNER JOIN estados
ON cidades.id_estado = estados.id_estado
WHERE estados.nome_estado LIKE 'São Paulo';
