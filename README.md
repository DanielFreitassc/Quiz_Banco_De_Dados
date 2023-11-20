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
    cd_cidade INTEGER PRIMARY KEY AUTOINCREMENT,
    nm_cidade VARCHAR(50) NOT NULL,
    UF CHAR(2) NOT NULL
);

-- Tabela bairro
CREATE TABLE bairro (
    cd_bairro INTEGER PRIMARY KEY AUTOINCREMENT,
    nm_bairro VARCHAR(50) NOT NULL,
    cd_cidade INTEGER NOT NULL,
    CONSTRAINT fk_bairro__cidade FOREIGN KEY (cd_cidade) REFERENCES cidade(cd_cidade)
);

-- Tabela usuario
CREATE TABLE usuario (
    cd_usuario INTEGER PRIMARY KEY AUTOINCREMENT,
    nm_usuario VARCHAR(100) NOT NULL,
    idade SMALLINT NOT NULL,
    endereco VARCHAR(70) NOT NULL,
    numero INTEGER NULL,
    cep CHAR(8) NOT NULL,
    complemento VARCHAR(20) NULL,
    referencia VARCHAR(50) NULL,
    cd_bairro INTEGER NOT NULL,
    cd_cidade INTEGER NOT NULL,
    UF CHAR(2) NOT NULL,
    telefone VARCHAR(15) NULL,
    e_mail VARCHAR(40) NULL,
    CONSTRAINT fk_usuario__bairro FOREIGN KEY (cd_bairro) REFERENCES bairro(cd_bairro),
    CONSTRAINT fk_usuario__cidade FOREIGN KEY (cd_cidade) REFERENCES cidade(cd_cidade)
);

-- Tabela quiz
CREATE TABLE quiz (
    cd_quiz INTEGER PRIMARY KEY AUTOINCREMENT,
    nm_quiz VARCHAR(100) NOT NULL,
    ds_quiz VARCHAR(200) NOT NULL,
    tema VARCHAR(50) NOT NULL
);

-- Tabela questao
CREATE TABLE questao (
    cd_questao INTEGER PRIMARY KEY AUTOINCREMENT,
    cd_quiz INTEGER NOT NULL,
    valor_questao SMALLINT NOT NULL,
    ds_questao VARCHAR(400) NOT NULL,
    CONSTRAINT fk_questao__quiz FOREIGN KEY (cd_quiz) REFERENCES quiz(cd_quiz)
);

-- Tabela opcao_questao
CREATE TABLE opcao_questao (
    cd_opcao_questao INTEGER PRIMARY KEY AUTOINCREMENT,
    cd_questao INTEGER NOT NULL,
    ds_opcao VARCHAR(200) NOT NULL,
    is_correta INTEGER NOT NULL,
    CONSTRAINT fk_opcao_questao__questao FOREIGN KEY (cd_questao) REFERENCES questao(cd_questao)
);

-- Tabela resposta_usuario
CREATE TABLE resposta_usuario (
    cd_resposta_usuario INTEGER PRIMARY KEY AUTOINCREMENT,
    cd_usuario INTEGER NOT NULL,
    cd_questao INTEGER NOT NULL,
    cd_opcao_questao INTEGER NOT NULL,
    dt_inicio DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP, -- Adiciona a data de inicio da resposta, se necessário
    dt_fim DATETIME NULL,
    CONSTRAINT fk_resposta_usuario__usuario FOREIGN KEY (cd_usuario) REFERENCES usuario(cd_usuario),
    CONSTRAINT fk_resposta_usuario__questao FOREIGN KEY (cd_questao) REFERENCES questao(cd_questao),
    CONSTRAINT fk_resposta_usuario__opcao_questao FOREIGN KEY (cd_opcao_questao) REFERENCES opcao_questao(cd_opcao_questao)
);

```
# Script que popula as tabelas do Banco de dados (DML)
```

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
