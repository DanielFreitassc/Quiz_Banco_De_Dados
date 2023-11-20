# Quiz Banco De Dados
> Paulo Vinícius, Daniel Freitas, Rhyan, Nathan, Pablo

# Criar o banco de dados de um quiz 

# Modelo ER Conceitual

![modeloConceitual](https://github.com/DanielFreitassc/Quiz_Banco_De_Dados/assets/133172885/6afbd005-9e17-488f-b021-055b5e6be523)

# Modelo ER Físico
....
# Dicionário de Dados
...
# Script dos comandos para criação do Banco de dados (DDL)
```
-- Tabela cidade
CREATE TABLE cidade (
    cd_cidade INT IDENTITY NOT NULL,
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
    dt_inicio DATETIME NOT NULL DEFAULT GETDATE(), -- Adiciona a data de inicio da resposta, se necessário
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
INSERT INTO Usuario (id, nome, email, senha) VALUES (1, 'João', 'joao@email.com', '123456');
INSERT INTO Usuario (id, nome, email, senha) VALUES (2, 'Maria', 'maria@email.com', 'abcdef');
INSERT INTO Usuario (id, nome, email, senha) VALUES (3, 'Pedro', 'pedro@email.com', 'qwerty');

INSERT INTO Quiz (id, título, descrição) VALUES (1, 'Quiz de Programação', 'Teste seu conhecimento em programação.');
INSERT INTO Quiz (id, título, descrição) VALUES (2, 'Quiz de Matemática', 'Teste seu conhecimento em matemática.');
INSERT INTO Quiz (id, título, descrição) VALUES (3, 'Quiz de Geografia', 'Teste seu conhecimento em geografia.');

INSERT INTO Rel (id, título, descrição) VALUES (1, 'Relacionamento 1', 'Relacionamento 1.');
INSERT INTO Rel (id, título, descrição) VALUES (2, 'Relacionamento 2', 'Relacionamento 2.');
INSERT INTO Rel (id, título, descrição) VALUES (3, 'Relacionamento 3', 'Relacionamento 3.');

INSERT INTO Rel_Quiz (id, quiz_id, rel_id) VALUES (1, 1, 1);
INSERT INTO Rel_Quiz (id, quiz_id, rel_id) VALUES (2, 2, 2);
INSERT INTO Rel_Quiz (id, quiz_id, rel_id) VALUES (3, 3, 3);

INSERT INTO Rel_Usuario (id, rel_id, usuario_id) VALUES (1, 1, 1);
INSERT INTO Rel_Usuario (id, rel_id, usuario_id) VALUES (2, 2, 2);
INSERT INTO Rel_Usuario (id, rel_
```
# Principais consultas mapeadas baseadas em regras de negócio (DQL) (mínimo 6)
```
SELECT * FROM Usuario
JOIN Rel_Usuario ON Usuario.id = Rel_Usuario.usuario_id
JOIN Rel ON Rel_Usuario.rel_id = Rel.id
JOIN Quiz ON Rel_Quiz.quiz_id = Quiz.id;

SELECT * FROM Quiz
JOIN Rel_Quiz ON Quiz.id = Rel_Quiz.quiz_id
JOIN Rel ON Rel_Quiz.rel_id = Rel.id;

SELECT * FROM Rel_Usuario
JOIN Rel ON Rel_Usuario.rel_id = Rel.id;
```
![bd](https://github.com/DanielFreitassc/Quiz_Banco_De_Dados/assets/129224303/e1e608cd-36c2-467b-83f9-84cb012435ef)
