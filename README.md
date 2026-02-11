
# 🚀 Desafio de Automação de Testes de API  
Banco Carrefour – Automação com Postman + Newman + GitHub Actions  

---

## 📌 Objetivo

Automatizar os testes da API REST de gerenciamento de usuários, garantindo cobertura completa dos fluxos funcionais e cenários negativos, incluindo autenticação JWT, validação de regras de negócio e tratamento de erros.

API utilizada:  
https://serverest.dev/

---

## 🛠 Tecnologias Utilizadas

- Postman
- Newman
- Node.js 18+
- GitHub Actions
- newman-reporter-html

---

## 📂 Estrutura da Collection

```
📁 API_carrefour
│
├── 📁 Regressao
│   │
│   ├── 📁 Login
│   │   └── 📄 POST Gera token - Login
│   │
│   ├── 📁 Cadastro
│   │   ├── 📄 POST /usuarios - cadastro
│   │   └── 📄 GET /id - validar usuario cadastrado
│   │
│   ├── 📁 Listagem
│   │   └── 📄 GET /usuarios
│   │
│   ├── 📁 Deletar
│   │   └── 📄 DELETE /users
│   │
│   ├── 📁 Alterar
│   │   ├── 📄 PUT /cadastro
│   │   ├── 📄 PUT /alterar
│   │   └── 📄 GET Valida usuario alterado com sucesso
│   │
│   └── 📁 Negativos
│       ├── 📄 POST Login invalido - email
│       ├── 📄 POST Login invalido - password
│       ├── 📄 POST /usuario - ja existe
│       ├── 📄 POST /token ausente
│       ├── 📄 POST /token invalido
│       ├── 📄 POST /campos obrigatorios
│       ├── 📄 GET Buscar usuario inexistente
│       ├── 📄 PUT /campos obrigatorios
│       ├── 📄 PUT Email ja cadastrado
│       └── 📄 DELETE Nao existe id para deletar
│
└── 📁 Rate Limit
    └── 📄 GET Validando Rate Limit

```

---

## 🔐 Autenticação

A API utiliza autenticação via JWT.

O token é obtido através do endpoint:

POST /login

Após login com sucesso, o token é salvo automaticamente em variável de ambiente:

```javascript
pm.environment.set("token", jsonData.authorization);
```

As requisições protegidas utilizam o header:

Authorization: {{token}}

---

## ✅ Cobertura de Testes

### ✔️ Cenários Positivos

- Login com sucesso
- Cadastro de usuário
- Validação do usuário criado
- Listagem de usuários
- Atualização de usuário
- Validação da atualização
- Exclusão de usuário

### ❌ Cenários Negativos

- Login com email inválido
- Login com password inválido
- Cadastro com email já existente
- Requisição sem token
- Requisição com token inválido
- Campos obrigatórios ausentes
- Busca de usuário inexistente
- Atualização com email duplicado
- Exclusão de ID inexistente
- Validação de Rate Limit (100 requisições/minuto)

---

## ⚙️ Configuração do Ambiente

### 1️⃣ Instalar Node.js

Requer Node 18 ou superior.

Verificar instalação:

node -v

---

### 2️⃣ Instalar Newman

npm install -g newman newman-reporter-html

---

## ▶️ Executar Testes Localmente

newman run API_carrefour.json -e ambiente.json --folder "Regressao" -r html --reporter-html-export regressao-report.html
newman run API_carrefour.json -e ambiente.json --folder "Rate Limit" --iteration-count 110 -r html --reporter-html-export rate-limit-report.html

Após a execução será gerado:

regressao-report
rate-limit-report.html

Abrir o arquivo no navegador para visualizar o relatório completo.

---

## 🔄 Integração Contínua (CI)

A automação está integrada ao GitHub Actions.

Arquivo de configuração:

.github/workflows/pipeline.yml

A pipeline executa automaticamente:

- A cada push na branch `main`
- Instala Node.js
- Instala Newman
- Executa os testes
- Gera relatório HTML
- Publica o relatório como artefato da execução

---

## 📊 Relatório de Execução

O relatório é gerado utilizando:

newman-reporter-html

Na pipeline:

Actions → Selecionar execução → Artifacts → Download api-report

---

## 🚦 Limitação da API

A API possui limitação de:

100 requisições por minuto

Foi criado um cenário específico para validação de Rate Limit.

---

## 👨‍💻 Autor

Thiago Augusto  
QA Automation Engineer
