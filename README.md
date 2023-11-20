# Quiz Banco De Dados
> Paulo Vinícius, Daniel Freitas, Rhyan, Nathan, Pablo

# Criar o banco de dados de um quiz 

# Modelo ER Conceitual

![bd](https://github.com/DanielFreitassc/Quiz_Banco_De_Dados/assets/129224303/e1e608cd-36c2-467b-83f9-84cb012435ef)


# Modelo ER Físico

[MODELO FISICO.pdf](https://github.com/DanielFreitassc/Quiz_Banco_De_Dados/files/13420037/MODELO.FISICO.pdf)




....
# Dicionário de Dados
...
# Script dos comandos para criação do Banco de dados (DDL)
```
-- Tabela cidade
CREATE TABLE cidade (
    cd_cidade INT NOT NULL,
    nm_cidade VARCHAR(50) NOT NULL,
    UF CHAR(2) NOT NULL,

	CONSTRAINT pk_cidade PRIMARY KEY (cd_cidade)

);

-- Tabela bairro
CREATE TABLE bairro (
    cd_bairro INT IDENTITY NOT NULL,
    nm_bairro VARCHAR(50) NOT NULL,
    cd_cidade INT NOT NULL,

	CONSTRAINT pk_bairro PRIMARY KEY (cd_bairro),

	CONSTRAINT fk_bairro__cidade FOREIGN KEY (cd_cidade)
	REFERENCES cidade (cd_cidade)

);

-- Tabela usuario
CREATE TABLE usuario (
    cd_usuario INT IDENTITY NOT NULL,
    nm_usuario VARCHAR(100) NOT NULL,
    idade SMALLINT NOT NULL,
    endereco VARCHAR(70) NOT NULL,
    numero INT NULL,
    cep CHAR(8) NOT NULL,
    complemento VARCHAR(20) NULL,
    referencia VARCHAR(50) NULL,
    cd_bairro INT NOT NULL,
    cd_cidade INT NOT NULL,
    UF CHAR(2) NOT NULL,
    telefone VARCHAR(15) NULL,
    e_mail VARCHAR(40) NULL,

	CONSTRAINT pk_usuario PRIMARY KEY (cd_usuario),

	CONSTRAINT fk_usuario__bairro FOREIGN KEY (cd_bairro)
	REFERENCES bairro (cd_bairro),

	CONSTRAINT fk_usuario__cidade FOREIGN KEY (cd_cidade)
	REFERENCES cidade (cd_cidade)
);

-- Tabela quiz
CREATE TABLE quiz (
    cd_quiz INT IDENTITY NOT NULL,
    nm_quiz VARCHAR(100) NOT NULL,
    ds_quiz VARCHAR(200) NOT NULL,
    tema VARCHAR(50) NOT NULL,

	CONSTRAINT pk_quiz PRIMARY KEY (cd_quiz)

);

-- Tabela questao
CREATE TABLE questao (
    cd_questao INT IDENTITY NOT NULL,
    cd_quiz INT NOT NULL,
    valor_questao SMALLINT NOT NULL,
    ds_questao VARCHAR(400) NOT NULL,

	CONSTRAINT pk_questao PRIMARY KEY (cd_questao),

	CONSTRAINT fk_questao__quiz FOREIGN KEY (cd_quiz)
	REFERENCES quiz (cd_quiz)

);

-- Tabela opcao_questao
CREATE TABLE opcao_questao (
    cd_opcao_questao INT IDENTITY NOT NULL,
    cd_questao INT NOT NULL,
    ds_opcao VARCHAR(200) NOT NULL,
    is_correta BIT NOT NULL,

	CONSTRAINT pk_opcao_questao PRIMARY KEY (cd_opcao_questao),

	CONSTRAINT fk_opcao_questao__questao FOREIGN KEY (cd_questao)
	REFERENCES questao (cd_questao)

);

-- Tabela resposta_usuario
CREATE TABLE resposta_usuario (
    cd_resposta_usuario INT IDENTITY NOT NULL,
    cd_usuario INT NOT NULL,
    cd_questao INT NOT NULL,
    cd_opcao_questao INT NOT NULL,
    dt_inicio DATETIME NOT NULL DEFAULT GETDATE(), -- Adiciona a data de inicio da resposta, se necess�rio
	dt_fim DATETIME NULL,

    CONSTRAINT pk_resposta_usuario PRIMARY KEY (cd_resposta_usuario),
    
	CONSTRAINT fk_resposta_usuario__usuario FOREIGN KEY (cd_usuario)
	REFERENCES usuario (cd_usuario),
	
	CONSTRAINT fk_resposta_usuario__questao FOREIGN KEY (cd_questao)
	REFERENCES questao (cd_questao),
	
	CONSTRAINT fk_resposta_usuario__opcao_questao FOREIGN KEY (cd_opcao_questao)
	REFERENCES opcao_questao (cd_opcao_questao)
);
```
# Script que popula as tabelas do Banco de dados (DML)
```
INSERT INTO cidade (cd_cidade,nm_cidade, UF) VALUES
    (1,'São Paulo', 'SP'),
    (2,'Rio de Janeiro', 'RJ'),
    (3,'Belo Horizonte', 'MG'),
    (4,'Brasília', 'DF'),
    (5,'Salvador', 'BA'),
    (6,'Curitiba', 'PR'),
    (7,'Recife', 'PE');


```
# Principais consultas mapeadas baseadas em regras de negócio (DQL) (mínimo 6)
```
Tabela cidade - Consulta para Obter Todas as Cidades:


SELECT * FROM cidade;
Tabela bairro - Consulta para Obter Bairros em uma Cidade Específica:


SELECT * FROM bairro WHERE cd_cidade = 1; -- Substitua 1 pelo ID da cidade desejada
Tabela usuario - Consulta para Obter Informações de Usuários em uma Cidade Específica:


SELECT * FROM usuario
WHERE cd_cidade = 1; -- Substitua 1 pelo ID da cidade desejada
Tabela quiz - Consulta para Obter Todos os Quizzes:



SELECT * FROM quiz;
Tabela questao - Consulta para Obter Questões de um Quiz Específico:


SELECT * FROM questao WHERE cd_quiz = 1; -- Substitua 1 pelo ID do quiz desejado
Tabela opcao_questao - Consulta para Obter Opções de uma Questão Específica:


SELECT * FROM opcao_questao WHERE cd_questao = 1; -- Substitua 1 pelo ID da questão desejada
Tabela resposta_usuario - Consulta para Obter Respostas de um Usuário Específico:


SELECT * FROM resposta_usuario WHERE cd_usuario = 1; -- Substitua 1 pelo ID do usuário desejado


SELECT count(*) FROM Usuario

WHERE UF = "SP"
```
