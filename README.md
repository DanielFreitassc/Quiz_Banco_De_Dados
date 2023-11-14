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
CREATE TABLE Usuario (
 id INT PRIMARY KEY,
 nome VARCHAR(255),
 email VARCHAR(255),
 senha VARCHAR(255)
);

CREATE TABLE Quiz (
 id INT PRIMARY KEY,
 título VARCHAR(255),
 descrição TEXT
);

CREATE TABLE Rel (
 id INT PRIMARY KEY,
 título VARCHAR(255),
 descrição TEXT
);

CREATE TABLE Rel_Quiz (
 id INT PRIMARY KEY,
 quiz_id INT,
 rel_id INT,
 FOREIGN KEY (quiz_id) REFERENCES Quiz(id),
 FOREIGN KEY (rel_id) REFERENCES Rel(id)
);

CREATE TABLE Rel_Usuario (
 id INT PRIMARY KEY,
 rel_id INT,
 usuario_id INT,
 FOREIGN KEY (rel_id) REFERENCES Rel(id),
 FOREIGN KEY (usuario_id) REFERENCES Usuario(id)
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
