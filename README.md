# Quiz Banco De Dados
> Paulo Vinícius, Daniel Freitas, Rhyan, Nathan, Pablo

# Criar o banco de dados de um quiz 

# Modelo ER Conceitual(5 Entidades)

![bd](https://github.com/DanielFreitassc/Quiz_Banco_De_Dados/assets/129224303/e1e608cd-36c2-467b-83f9-84cb012435ef)


# Modelo ER Físico(5 Entidades)

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
    cd_bairro INT  NOT NULL,
    nm_bairro VARCHAR(50) NOT NULL,
    cd_cidade INT NOT NULL,

	CONSTRAINT pk_bairro PRIMARY KEY (cd_bairro),

	CONSTRAINT fk_bairro__cidade FOREIGN KEY (cd_cidade)
	REFERENCES cidade (cd_cidade)

);

-- Tabela usuario
CREATE TABLE usuario (
    cd_usuario INT  NOT NULL,
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
    cd_quiz INT  NOT NULL,
    nm_quiz VARCHAR(100) NOT NULL,
    ds_quiz VARCHAR(200) NOT NULL,
    tema VARCHAR(50) NOT NULL,

	CONSTRAINT pk_quiz PRIMARY KEY (cd_quiz)

);

-- Tabela questao
CREATE TABLE questao (
    cd_questao INT  NOT NULL,
    cd_quiz INT NOT NULL,
    valor_questao SMALLINT NOT NULL,
    ds_questao VARCHAR(400) NOT NULL,

	CONSTRAINT pk_questao PRIMARY KEY (cd_questao),

	CONSTRAINT fk_questao__quiz FOREIGN KEY (cd_quiz)
	REFERENCES quiz (cd_quiz)

);

-- Tabela opcao_questao
CREATE TABLE opcao_questao (
    cd_opcao_questao INT  NOT NULL,
    cd_questao INT NOT NULL,
    ds_opcao VARCHAR(200) NOT NULL,
    is_correta BIT NOT NULL,

	CONSTRAINT pk_opcao_questao PRIMARY KEY (cd_opcao_questao),

	CONSTRAINT fk_opcao_questao__questao FOREIGN KEY (cd_questao)
	REFERENCES questao (cd_questao)

);

-- Tabela resposta_usuario
CREATE TABLE resposta_usuario (
    cd_resposta_usuario INT  NOT NULL,
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
# Script que popula as tabelas do Banco de dados (DML) (7 Linhas de dados)
```
-- Inserir dados na tabela cidade
INSERT INTO cidade (cd_cidade, nm_cidade, UF) VALUES
    (1, 'São Paulo', 'SP'),
    (2, 'Rio de Janeiro', 'RJ'),
    (3, 'Belo Horizonte', 'MG'),
    (4, 'Brasília', 'DF'),
    (5, 'Fortaleza', 'CE'),
    (6, 'Manaus', 'AM'),
    (7, 'Santos', 'SP');

-- Inserir dados na tabela bairro
INSERT INTO bairro (cd_bairro, nm_bairro, cd_cidade) VALUES
    (1, 'Centro', 1),
    (2, 'Copacabana', 2),
    (3, 'Savassi', 3),
    (4, 'Asa Sul', 4),
    (5, 'Barra', 5),
    (6, 'Batel', 6),
    (7, 'Boa Viagem', 7);

-- Inserir dados na tabela usuario
INSERT INTO usuario (cd_usuario, nm_usuario, idade, endereco, numero, cep, complemento, referencia, cd_bairro, cd_cidade, UF, telefone, e_mail) VALUES
    (1, 'João Silva', 30, 'Rua A', 123, '12345-678', 'Apto 101', 'Próximo ao mercado', 1, 1, 'SP', '123456789', 'joao@email.com'),
    (2, 'Maria Oliveira', 25, 'Rua B', 456, '54321-876', 'Casa 202', 'Próximo à escola', 2, 2, 'RJ', '987654321', 'maria@email.com'),
    (3, 'Carlos Santos', 35, 'Av. C', 789, '98765-432', 'Sala 101', 'Ao lado do hospital', 3, 3, 'MG', '654321987', 'carlos@email.com'),
    (4, 'Ana Souza', 28, 'Rua D', 101, '12345-678', 'Apto 303', 'Próximo ao parque', 4, 4, 'DF', '123456789', 'ana@email.com'),
    (5, 'Felipe Lima', 22, 'Av. E', 111, '87654-321', 'Casa 404', 'Próximo à estação', 5, 5, 'BA', '987654321', 'felipe@email.com'),
    (6, 'José Oliveira', 40, 'Rua F', 789, '54321-876', 'Apto 505', 'Próximo ao shopping', 6, 6, 'PR', '987654321', 'jose@email.com'),
    (7, 'Fernanda Silva', 33, 'Av. G', 222, '98765-432', 'Casa 606', 'Próximo ao parque', 7, 7, 'PE', '123456789', 'fernanda@email.com');

-- Inserir dados na tabela quiz
INSERT INTO quiz (cd_quiz, nm_quiz, ds_quiz, tema) VALUES
    (1, 'Quiz de Matemática', 'Teste seus conhecimentos em matemática.', 'Matemática'),
    (2, 'Quiz de História', 'Teste seus conhecimentos em história mundial.', 'História'),
    (3, 'Quiz de Geografia', 'Teste seus conhecimentos em geografia mundial.', 'Geografia'),
    (4, 'Quiz de Ciências', 'Teste seus conhecimentos em ciências naturais.', 'Ciências'),
    (5, 'Quiz de Literatura', 'Teste seus conhecimentos em literatura mundial.', 'Literatura'),
    (6, 'Quiz de Tecnologia', 'Teste seus conhecimentos em tecnologia.', 'Tecnologia'),
    (7, 'Quiz de Esportes', 'Teste seus conhecimentos em esportes.', 'Esportes');

-- Inserir dados na tabela questao
INSERT INTO questao (cd_questao, cd_quiz, valor_questao, ds_questao) VALUES
    (1, 1, 10, 'Qual é a fórmula da área de um triângulo?'),
    (2, 2, 15, 'Quem foi o primeiro presidente do Brasil?'),
    (3, 3, 20, 'Qual é a capital da França?'),
    (4, 4, 25, 'Qual é o componente mais abundante na atmosfera da Terra?'),
    (5, 5, 30, 'Quem escreveu "Dom Quixote"?'),
    (6, 6, 10, 'O que significa a sigla HTML?'),
    (7, 7, 15, 'Qual esporte é conhecido como "esporte bretão"?');

-- Inserir dados na tabela opcao_questao
INSERT INTO opcao_questao (cd_opcao_questao, cd_questao, ds_opcao, is_correta) VALUES
    (1, 1, 'A = bh/2', 1),
    (2, 1, 'A = πr²', 0),
    (3, 2, 'Getúlio Vargas', 1),
    (4, 2, 'Juscelino Kubitschek', 0),
    (5, 3, 'Paris', 1),
    (6, 3, 'Londres', 0),
    (7, 4, 'Nitrogênio', 1),
    (8, 4, 'Oxigênio', 0),
    (9, 5, 'Miguel de Cervantes', 1),
    (10, 5, 'William Shakespeare', 0),
    (11, 6, 'Hypertext Markup Language', 1),
    (12, 6, 'High-Level Programming Language', 0),
    (13, 7, 'Futebol', 1),
    (14, 7, 'Críquete', 0);

-- Inserir dados na tabela resposta_usuario
INSERT INTO resposta_usuario (cd_resposta_usuario, cd_usuario, cd_questao, cd_opcao_questao, dt_fim) VALUES
    (1, 1, 1, 1, '2023-11-20 12:30:00'),
    (2, 2, 2, 3, '2023-11-21 14:45:00'),
    (3, 3, 3, 5, '2023-11-22 10:30:00'),
    (4, 4, 4, 7, '2023-11-23 18:15:00'),
    (5, 5, 5, 9, '2023-11-24 22:00:00'),
    (6, 6, 6, 2, '2023-11-25 16:30:00'),
    (7, 7, 7, 4, '2023-11-26 14:00:00');

```
# Principais consultas mapeadas baseadas em regras de negócio (DQL) (mínimo 6)
```
SELECT b.cd_bairro, b.nm_bairro, c.nm_cidade, c.UF
FROM bairro b
INNER JOIN cidade c ON b.cd_cidade = c.cd_cidade;

```
```
SELECT u.cd_usuario, u.nm_usuario, u.idade, u.endereco, u.numero, u.cep, u.complemento, u.referencia,
       b.nm_bairro, c.nm_cidade, c.UF, u.telefone, u.e_mail
FROM usuario u
INNER JOIN bairro b ON u.cd_bairro = b.cd_bairro
INNER JOIN cidade c ON u.cd_cidade = c.cd_cidade;

```
