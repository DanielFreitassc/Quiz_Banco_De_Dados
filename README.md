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
-- Populando a tabela cidade
INSERT INTO cidade (nm_cidade, UF) VALUES 
    ('São Paulo', 'SP'),
    ('Rio de Janeiro', 'RJ'),
    ('Belo Horizonte', 'MG'),
    ('Porto Alegre', 'RS'),
    ('Salvador', 'BA'),
    ('Brasília', 'DF'),
    ('Fortaleza', 'CE');

-- Populando a tabela bairro
INSERT INTO bairro (nm_bairro, cd_cidade) VALUES
    ('Centro', 1),
    ('Copacabana', 2),
    ('Savassi', 3),
    ('Moinhos de Vento', 4),
    ('Barra', 5),
    ('Asa Sul', 6),
    ('Aldeota', 7);

-- Populando a tabela usuario
INSERT INTO usuario (nm_usuario, idade, endereco, numero, cep, complemento, referencia, cd_bairro, cd_cidade, UF, telefone, e_mail) VALUES
    ('João Silva', 30, 'Rua A', 123, '12345678', 'Apto 101', 'Próximo ao mercado', 1, 1, 'SP', '1234-5678', 'joao@email.com'),
    ('Maria Santos', 25, 'Av. B', 456, '54321876', 'Casa 202', 'Próximo à escola', 2, 2, 'RJ', '9876-5432', 'maria@email.com'),
    ('Carlos Oliveira', 40, 'Rua C', 789, '98765432', NULL, 'Próximo ao parque', 3, 3, 'MG', '8765-4321', 'carlos@email.com'),
    ('Ana Pereira', 35, 'Av. D', 101, '13579246', 'Bloco B', 'Próximo à praia', 4, 4, 'RS', '7654-3210', 'ana@email.com'),
    ('Lucas Santos', 28, 'Rua E', 222, '24680135', 'Casa 303', 'Próximo à academia', 5, 5, 'BA', '4321-0987', 'lucas@email.com'),
    ('Mariana Oliveira', 32, 'Av. F', 333, '36985741', 'Apto 404', 'Próximo ao shopping', 6, 6, 'DF', '9876-1234', 'mariana@email.com'),
    ('Rafael Lima', 27, 'Rua G', 444, '14785369', 'Casa 505', 'Próximo ao teatro', 7, 7, 'CE', '1234-5678', 'rafael@email.com');

-- Populando a tabela quiz
INSERT INTO quiz (nm_quiz, ds_quiz, tema) VALUES
    ('Quiz de Matemática', 'Teste seus conhecimentos em matemática.', 'Matemática'),
    ('Quiz de História', 'Descubra o quanto você sabe sobre história.', 'História'),
    ('Quiz de Ciências', 'Teste seus conhecimentos em ciências.', 'Ciências'),
    ('Quiz de Geografia', 'Veja o quanto você conhece sobre geografia.', 'Geografia'),
    ('Quiz de Tecnologia', 'Teste seus conhecimentos em tecnologia.', 'Tecnologia'),
    ('Quiz de Entretenimento', 'Divirta-se com perguntas sobre entretenimento.', 'Entretenimento'),
    ('Quiz de Esportes', 'Descubra o quanto você sabe sobre esportes.', 'Esportes');

-- Populando a tabela questao
INSERT INTO questao (cd_quiz, valor_questao, ds_questao) VALUES
    (1, 5, 'Quanto é 2 + 3?'),
    (1, 3, 'Qual é a raiz quadrada de 9?'),
    (2, 4, 'Quem foi o primeiro presidente do Brasil?'),
    (2, 2, 'Em que ano ocorreu a Independência do Brasil?'),
    (3, 5, 'Qual é a fórmula química da água?'),
    (3, 3, 'Quem desenvolveu a teoria da relatividade?'),
    (4, 4, 'Qual é a capital da Austrália?'),
    (4, 2, 'Qual é o maior rio do mundo?'),
    (5, 5, 'Quem é o fundador da Microsoft?'),
    (5, 3, 'Qual é o sistema operacional mais utilizado no mundo?'),
    (6, 4, 'Quem é o autor de Harry Potter?'),
    (6, 2, 'Qual é o filme mais assistido de todos os tempos?'),
    (7, 5, 'Em que esporte Michael Phelps se destacou nas Olimpíadas?'),
    (7, 3, 'Qual é o esporte mais popular do mundo?');

-- Populando a tabela opcao_questao
INSERT INTO opcao_questao (cd_questao, ds_opcao, is_correta) VALUES
    (1, '5', 1),
    (1, '6', 0),
    (2, '3', 1),
    (2, '4', 0),
    (3, 'Getúlio Vargas', 0),
    (3, 'Juscelino Kubitschek', 0),
    (3, 'Marechal Deodoro', 1),
    (3, 'Tancredo Neves', 0),
    (4, '1822', 1),
    (4, '1888', 0),
    (5, 'H2O', 1),
    (5, 'CO2', 0),
    (6, 'Isaac Newton', 0),
    (6, 'Albert Einstein', 1),
    (7, 'Sydney', 1),
    (7, 'Canberra', 0),
    (8, 'Amazonas', 1),
    (8, 'Nilo', 0),
    (9, 'Bill Gates', 1),
    (9, 'Steve Jobs', 0),
    (10, 'Windows', 1),
    (10, 'iOS', 0),
    (11, 'J.K. Rowling', 1),
    (11, 'George R.R. Martin', 0),
    (12, 'Avatar', 0),
    (12, 'Vingadores: Ultimato', 1),
    (13, 'Natação', 1),
    (13, 'Futebol', 0),
    (14, 'Futebol', 1),
    (14, 'Críquete', 0)

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
# Deletar todas tableas
```
-- Desativar temporariamente as restrições de chave estrangeira
EXEC sp_MSforeachtable 'ALTER TABLE ? NOCHECK CONSTRAINT ALL';

-- Excluir as tabelas em ordem inversa de dependência

-- Tabela resposta_usuario
DROP TABLE IF EXISTS resposta_usuario;

-- Tabela opcao_questao
DROP TABLE IF EXISTS opcao_questao;

-- Tabela questao
DROP TABLE IF EXISTS questao;

-- Tabela quiz
DROP TABLE IF EXISTS quiz;

-- Tabela usuario
DROP TABLE IF EXISTS usuario;

-- Tabela bairro
DROP TABLE IF EXISTS bairro;

-- Tabela cidade
DROP TABLE IF EXISTS cidade;

-- Ativar novamente as restrições de chave estrangeira
EXEC sp_MSforeachtable 'ALTER TABLE ? WITH CHECK CHECK CONSTRAINT ALL';
```
