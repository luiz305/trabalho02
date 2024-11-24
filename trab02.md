# trabalho02CREATE DATABASE sistema_academico;

USE sistema_academico;

CREATE TABLE estudantes (
    id INT AUTO_INCREMENT PRIMARY KEY,
    nome VARCHAR(100) NOT NULL,
    rg VARCHAR(20) NOT NULL,
    cpf VARCHAR(14) NOT NULL,
    data_nascimento DATE NOT NULL,
    telefone1 VARCHAR(15) NOT NULL,
    telefone2 VARCHAR(15),
    nome_mae VARCHAR(100),
    nome_pai VARCHAR(100)
);

CREATE TABLE enderecos (
    id INT AUTO_INCREMENT PRIMARY KEY,
    estudante_id INT NOT NULL,
    logradouro VARCHAR(100) NOT NULL,
    numero VARCHAR(10) NOT NULL,
    bairro VARCHAR(50) NOT NULL,
    cidade VARCHAR(50) NOT NULL,
    estado VARCHAR(2) NOT NULL,
    cep VARCHAR(10),
    FOREIGN KEY (estudante_id) REFERENCES estudantes(id)
);

CREATE TABLE cursos (
    id INT AUTO_INCREMENT PRIMARY KEY,
    nome VARCHAR(100) NOT NULL,
    descricao TEXT,
    carga_horaria INT NOT NULL,
    preco DECIMAL(10, 2) NOT NULL,
    data_inicio DATE NOT NULL
);

CREATE TABLE estudantes_cursos (
    estudante_id INT NOT NULL,
    curso_id INT NOT NULL,
    PRIMARY KEY (estudante_id, curso_id),
    FOREIGN KEY (estudante_id) REFERENCES estudantes(id),
    FOREIGN KEY (curso_id) REFERENCES cursos(id)
);
<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0-alpha1/dist/css/bootstrap.min.css" rel="stylesheet">
    <title>Cadastro de Estudante</title>
</head>
<body>
<div class="container mt-5">
    <h2>Cadastro de Estudante</h2>
    <form action="processa_estudante.php" method="POST">
        <div class="mb-3">
            <label for="nome" class="form-label">Nome</label>
            <input type="text" class="form-control" id="nome" name="nome" required>
        </div>
        <div class="mb-3">
            <label for="rg" class="form-label">RG</label>
            <input type="text" class="form-control" id="rg" name="rg" required>
        </div>
        <div class="mb-3">
            <label for="cpf" class="form-label">CPF</label>
            <input type="text" class="form-control" id="cpf" name="cpf" required>
        </div>
        <div class="mb-3">
            <label for="data_nascimento" class="form-label">Data de Nascimento</label>
            <input type="date" class="form-control" id="data_nascimento" name="data_nascimento" required>
        </div>
        <div class="mb-3">
            <label for="telefone1" class="form-label">Telefone 1</label>
            <input type="text" class="form-control" id="telefone1" name="telefone1" required>
        </div>
        <div class="mb-3">
            <label for="telefone2" class="form-label">Telefone 2</label>
            <input type="text" class="form-control" id="telefone2" name="telefone2">
        </div>
        <div class="mb-3">
            <label for="nome_mae" class="form-label">Nome da Mãe</label>
            <input type="text" class="form-control" id="nome_mae" name="nome_mae">
        </div>
        <div class="mb-3">
            <label for="nome_pai" class="form-label">Nome do Pai</label>
            <input type="text" class="form-control" id="nome_pai" name="nome_pai">
        </div>
        <h3>Endereço</h3>
        <div class="mb-3">
            <label for="logradouro" class="form-label">Logradouro</label>
            <input type="text" class="form-control" id="logradouro" name="logradouro" required>
        </div>
        <div class="mb-3">
            <label for="numero" class="form-label">Número</label>
            <input type="text" class="form-control" id="numero" name="numero" required>
        </div>
        <div class="mb-3">
            <label for="bairro" class="form-label">Bairro</label>
            <input type="text" class="form-control" id="bairro" name="bairro" required>
        </div>
        <div class="mb-3">
            <label for="cidade" class="form-label">Cidade</label>
            <input type="text" class="form-control" id="cidade" name="cidade" required>
        </div>
        <div class="mb-3">
            <label for="estado" class="form-label">Estado</label>
            <input type="text" class="form-control" id="estado" name="estado" maxlength="2" required>
        </div>
        <div class="mb-3">
            <label for="cep" class="form-label">CEP</label>
            <input type="text" class="form-control" id="cep" name="cep">
        </div>
        <button type="submit" class="btn btn-primary">Cadastrar</button>
    </form>
</div>
</body>
</html>
<?php
// Conexão com o banco de dados
$conn = new mysqli("localhost", "root", "", "sistema_academico");

if ($conn->connect_error) {
    die("Falha na conexão: " . $conn->connect_error);
}

// Dados do estudante
$nome = $_POST['nome'];
$rg = $_POST['rg'];
$cpf = $_POST['cpf'];
$data_nascimento = $_POST['data_nascimento'];
$telefone1 = $_POST['telefone1'];
$telefone2 = $_POST['telefone2'];
$nome_mae = $_POST['nome_mae'];
$nome_pai = $_POST['nome_pai'];

// Endereço
$logradouro = $_POST['logradouro'];
$numero = $_POST['numero'];
$bairro = $_POST['bairro'];
$cidade = $_POST['cidade'];
$estado = $_POST['estado'];
$cep = $_POST['cep'];

// Inserir estudante
$sql_estudante = "INSERT INTO estudantes (nome, rg, cpf, data_nascimento, telefone1, telefone2, nome_mae, nome_pai) 
                  VALUES ('$nome', '$rg', '$cpf', '$data_nascimento', '$telefone1', '$telefone2', '$nome_mae', '$nome_pai')";
$conn->query($sql_estudante);

// Pegar ID do estudante inserido
$estudante_id = $conn->insert_id;

// Inserir endereço
$sql_endereco = "INSERT INTO enderecos (estudante_id, logradouro, numero, bairro, cidade, estado, cep) 
                 VALUES ('$estudante_id', '$logradouro', '$numero', '$bairro', '$cidade', '$estado', '$cep')";
$conn->query($sql_endereco);

echo "Estudante cadastrado com sucesso!";
$conn->close();
?>
<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0-alpha1/dist/css/bootstrap.min.css" rel="stylesheet">
    <title>Cadastro de Curso</title>
</head>
<body>
<div class="container mt-5">
    <h2>Cadastro de Curso</h2>
    <form action="processa_curso.php" method="POST">
        <div class="mb-3">
            <label for="nome" class="form-label">Nome do Curso</label>
            <input type="text" class="form-control" id="nome" name="nome" required>
        </div>
        <div class="mb-3">
            <label for="descricao" class="form-label">Descrição</label>
            <textarea class="form-control" id="descricao" name="descricao" rows="4"></textarea>
        </div>
        <div class="mb-3">
            <label for="carga_horaria" class="form-label">Carga Horária (horas)</label>
            <input type="number" class="form-control" id="carga_horaria" name="carga_horaria" min="1" required>
        </div>
        <div class="mb-3">
            <label for="preco" class="form-label">Preço (R$)</label>
            <input type="number" class="form-control" id="preco" name="preco" step="0.01" required>
        </div>
        <div class="mb-3">
            <label for="data_inicio" class="form-label">Data de Início</label>
            <input type="date" class="form-control" id="data_inicio" name="data_inicio" required>
        </div>
        <button type="submit" class="btn btn-primary">Cadastrar Curso</button>
    </form>
</div>
</body>
</html>
<?php
// Conexão com o banco de dados
$conn = new mysqli("localhost", "root", "", "sistema_academico");

if ($conn->connect_error) {
    die("Falha na conexão: " . $conn->connect_error);
}

// Dados do curso
$nome = $_POST['nome'];
$descricao = $_POST['descricao'];
$carga_horaria = $_POST['carga_horaria'];
$preco = $_POST['preco'];
$data_inicio = $_POST['data_inicio'];

// Inserir curso no banco de dados
$sql = "INSERT INTO cursos (nome, descricao, carga_horaria, preco, data_inicio) 
        VALUES ('$nome', '$descricao', '$carga_horaria', '$preco', '$data_inicio')";

if ($conn->query($sql) === TRUE) {
    echo "Curso cadastrado com sucesso!";
} else {
    echo "Erro ao cadastrar o curso: " . $conn->error;
}

$conn->close();
?>
