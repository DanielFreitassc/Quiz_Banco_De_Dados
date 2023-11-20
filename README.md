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
# Script que popula as tabelas do Banco de dados (DML)
```
-- Inserir mais dados na tabela cidade
INSERT INTO cidade (nm_cidade, UF) VALUES ('Belo Horizonte', 'MG');
INSERT INTO cidade (nm_cidade, UF) VALUES ('Brasília', 'DF');
INSERT INTO cidade (nm_cidade, UF) VALUES ('Recife', 'PE');
INSERT INTO cidade (nm_cidade, UF) VALUES ('Curitiba', 'PR');
INSERT INTO cidade (nm_cidade, UF) VALUES ('Salvador', 'BA');

-- Inserir mais dados na tabela bairro
INSERT INTO bairro (nm_bairro, cd_cidade) VALUES ('Savassi', 1);
INSERT INTO bairro (nm_bairro, cd_cidade) VALUES ('Asa Sul', 2);
INSERT INTO bairro (nm_bairro, cd_cidade) VALUES ('Boa Viagem', 3);
INSERT INTO bairro (nm_bairro, cd_cidade) VALUES ('Batel', 4);
INSERT INTO bairro (nm_bairro, cd_cidade) VALUES ('Barra', 5);

-- Inserir mais dados na tabela usuario
INSERT INTO usuario (nm_usuario, idade, endereco, numero, cep, complemento, referencia, cd_bairro, cd_cidade, UF, telefone, e_mail) 
VALUES ('Maria Oliveira', 25, 'Rua B', 456, '54321-876', 'Casa 202', 'Próximo à escola', 2, 2, 'DF', '987654321', 'maria@email.com');

INSERT INTO usuario (nm_usuario, idade, endereco, numero, cep, complemento, referencia, cd_bairro, cd_cidade, UF, telefone, e_mail) 
VALUES ('Carlos Santos', 35, 'Av. C', 789, '98765-432', 'Sala 101', 'Ao lado do hospital', 3, 3, 'PE', '654321987', 'carlos@email.com');

INSERT INTO usuario (nm_usuario, idade, endereco, numero, cep, complemento, referencia, cd_bairro, cd_cidade, UF, telefone, e_mail) 
VALUES ('Ana Souza', 28, 'Rua D', 101, '12345-678', 'Apto 303', 'Próximo ao parque', 4, 4, 'PR', '123456789', 'ana@email.com');

INSERT INTO usuario (nm_usuario, idade, endereco, numero, cep, complemento, referencia, cd_bairro, cd_cidade, UF, telefone, e_mail) 
VALUES ('Felipe Lima', 22, 'Av. E', 111, '87654-321', 'Casa 404', 'Próximo à estação', 5, 5, 'BA', '987654321', 'felipe@email.com');

-- Inserir mais dados na tabela quiz
INSERT INTO quiz (nm_quiz, ds_quiz, tema) VALUES ('Quiz de Geografia', 'Teste seus conhecimentos em geografia mundial.', 'Geografia');
INSERT INTO quiz (nm_quiz, ds_quiz, tema) VALUES ('Quiz de Ciências', 'Teste seus conhecimentos em ciências naturais.', 'Ciências');
INSERT INTO quiz (nm_quiz, ds_quiz, tema) VALUES ('Quiz de Literatura', 'Teste seus conhecimentos em literatura mundial.', 'Literatura');
INSERT INTO quiz (nm_quiz, ds_quiz, tema) VALUES ('Quiz de Inglês', 'Teste seus conhecimentos em língua inglesa.', 'Inglês');
INSERT INTO quiz (nm_quiz, ds_quiz, tema) VALUES ('Quiz de Arte', 'Teste seus conhecimentos em arte e cultura.', 'Arte');

-- Inserir mais dados na tabela questao
INSERT INTO questao (cd_quiz, valor_questao, ds_questao) VALUES (3, 20, 'Quem é o autor da obra "Dom Casmurro"?');
INSERT INTO questao (cd_quiz, valor_questao, ds_questao) VALUES (4, 15, 'Qual é o verbo auxiliar em uma frase afirmativa em inglês?');
INSERT INTO questao (cd_quiz, valor_questao, ds_questao) VALUES (5, 10, 'Quem pintou a Mona Lisa?');
INSERT INTO questao (cd_quiz, valor_questao, ds_questao) VALUES (3, 25, 'Qual é a obra mais conhecida de William Shakespeare?');
INSERT INTO questao (cd_quiz, valor_questao, ds_questao) VALUES (4, 12, 'O que é uma célula eucariótica?');

-- Inserir mais dados na tabela opcao_questao
INSERT INTO opcao_questao (cd_questao, ds_opcao, is_correta) VALUES (6, 'Machado de Assis', 1);
INSERT INTO opcao_questao (cd_questao, ds_opcao, is_correta) VALUES (6, 'Carlos Drummond de Andrade', 0);
INSERT INTO opcao_questao (cd_questao, ds_opcao, is_correta) VALUES (7, 'am', 1);
INSERT INTO opcao_questao (cd_questao, ds_opcao, is_correta) VALUES (7, 'have', 0);
INSERT INTO opcao_questao (cd_questao, ds_opcao, is_correta) VALUES (8, 'Leonardo da Vinci', 1);
INSERT INTO opcao_questao (cd_questao, ds_opcao, is_correta) VALUES (8, 'Pablo Picasso', 0);
INSERT INTO opcao_questao (cd_questao, ds_opcao, is_correta) VALUES (9, 'Hamlet', 1);
INSERT INTO opcao_questao (cd_questao, ds_opcao, is_correta) VALUES (9, 'Romeu e Julieta', 0);
INSERT INTO opcao_questao (cd_questao, ds_opcao, is_correta) VALUES (10, 'Célula com núcleo definido', 1);
INSERT INTO opcao_questao (cd_questao, ds_opcao, is_correta) VALUES (10, 'Célula procarionte', 0);

-- Inserir mais dados na tabela resposta_usuario
INSERT INTO resposta_usuario (cd_usuario, cd_questao, cd_opcao_questao, dt_fim) VALUES (2, 6, 1, '2023-11-21 14:45:00');
INSERT INTO resposta_usuario (cd_usuario, cd_questao, cd_opcao_questao, dt_fim) VALUES (3, 7, 3, '2023-11-22 10:30:00');
INSERT INTO resposta_usuario (cd_usuario, cd_questao, cd_opcao_questao, dt_fim) VALUES (4, 8, 5, '2023-11-23 18:15:00');
INSERT INTO resposta_usuario (cd_usuario, cd_questao, cd_opcao_questao, dt_fim) VALUES (5, 9, 7, '2023-11-24 22:00:00');
INSERT INTO resposta_usuario (cd_usuario, cd_questao, cd_opcao_questao, dt_fim) VALUES (1, 10, 9, '2023-11-25 16:30:00');

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
