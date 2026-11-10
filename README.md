# 🎵 Sistema Musical — Backend em PHP & MySQL (Fase 2)

Este repositório contém a versão completa do backend do **Sistema Musical**. O sistema foi convertido de protótipos estáticos para uma aplicação PHP funcional integrada ao MySQL, com autenticação de usuários, gerenciamento (CRUD) de artistas e músicas, upload/redimensionamento de imagens e biblioteca com busca dinâmica.

---

## 📂 Estrutura de Arquivos

```text
sistema-musical/
├── styles.css               # Folha de estilos centralizada
├── bd.sql                   # Script de criação do banco e tabelas
├── conexao.php              # Conexão MySQLi com o banco de dados
├── geraSenha.php            # Utilitário para gerar hashes de senha (Bcrypt)
├── valida_sessao.php        # Middleware de proteção de acesso
├── login.php                # Autenticação de usuários
├── logout.php               # Encerramento de sessão
├── index.php                # Menu principal do sistema
├── cadastro_artista.php     # CRUD de Artistas + Upload de Foto
├── cadastro_musica.php      # CRUD de Músicas + Capa de Álbum + YouTube
├── listagem_musicas.php     # Biblioteca musical com busca e filtros
└── img/                     # Diretório de armazenamento das mídias
```

---

## ⚙️ Requisitos e Configuração

- **Ambiente Web**: Apache/Nginx com **PHP 7.4+** (XAMPP, Laragon, WAMP ou Docker).
- **Extensão PHP**: Extension `gd` ativada no `php.ini` (para manipulação/redimensionamento de imagens).
- **Banco de Dados**: MySQL 5.7+ / MariaDB.

---

## 🛢️ Configuração do Banco de Dados (`bd.sql`)

Crie o banco de dados `sistema_musical_v2` e execute o script SQL abaixo:

```sql
CREATE DATABASE IF NOT EXISTS sistema_musical_v2;
USE sistema_musical_v2;

-- Tabela de usuários
CREATE TABLE IF NOT EXISTS usuarios (
    id INT AUTO_INCREMENT PRIMARY KEY,
    usuario VARCHAR(50) NOT NULL UNIQUE,
    senha VARCHAR(255) NOT NULL
);

-- Tabela de artistas
CREATE TABLE IF NOT EXISTS artistas (
    id INT AUTO_INCREMENT PRIMARY KEY,
    nome VARCHAR(100) NOT NULL,
    genero VARCHAR(50),
    pais VARCHAR(50),
    imagem VARCHAR(255)
);

-- Tabela de músicas
CREATE TABLE IF NOT EXISTS musicas (
    id INT AUTO_INCREMENT PRIMARY KEY,
    artista_id INT NOT NULL,
    titulo VARCHAR(100) NOT NULL,
    album VARCHAR(100),
    genero VARCHAR(50),
    youtube_link VARCHAR(255),
    letra TEXT,
    imagem_capa VARCHAR(255),
    FOREIGN KEY (artista_id) REFERENCES artistas(id) ON DELETE RESTRICT
);

-- Usuário padrão (admin / 123)
INSERT INTO usuarios (usuario, senha) 
VALUES ('admin', '$2y$12$zw1c2aDgt4HHnAzXc3g3dOV6Mn96Dm2UaSIketIhYe1xmtuaJG6qa')
ON DUPLICATE KEY UPDATE id=id;
```

---

## 🔑 Credenciais Padrão de Teste

- **Usuário**: `admin`
- **Senha**: `123`

---

## 🚀 Como Executar o Projeto

1. Coloque a pasta do projeto no diretório do seu servidor local (`htdocs`, `www` ou similar).
2. Verifique e ajuste as credenciais em `conexao.php` se a sua senha do MySQL for diferente (`$password = "Senai@118";`).
3. Importe o arquivo `bd.sql` no MySQL / phpMyAdmin.
4. Acesse no navegador: `http://localhost/sistema-musical/login.php`.

---

## 🔒 Recursos de Segurança Implementados

- **Proteção contra SQL Injection**: Uso de *Prepared Statements* em todas as queries SQL.
- **Criptografia de Senhas**: Armazenamento com algoritmos hash Bcrypt (`password_hash` / `password_verify`).
- **Proteção XSS**: Sanitização de saídas no HTML com `htmlspecialchars()`.
- **Validação de Uploads**: Checagem de tipo MIME, extensão (`.jpg`, `.png`) e limite de tamanho (5 MB).
- **Controle de Acesso**: Proteção de rotas através de controle de sessões em PHP (`valida_sessao.php`).
