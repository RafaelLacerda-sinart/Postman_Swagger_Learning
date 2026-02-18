# 📘 GUIA COMPLETO — POSTMAN E SWAGGER (SMARTBEAR)

Este README reúne conceitos, organização, boas práticas e instruções práticas para uso do Postman e do Swagger no desenvolvimento de APIs REST.

--------------------------------------------------------------------
🔷 1. POSTMAN — TESTES, ORGANIZAÇÃO E AUTOMAÇÃO DE APIs
--------------------------------------------------------------------

O Postman é uma plataforma completa para testar, validar, automatizar e documentar APIs.

Principais funcionalidades:
- Criar requisições HTTP
- Organizar endpoints
- Trabalhar com variáveis
- Automatizar testes
- Versionar collections
- Compartilhar com a equipe

--------------------------------------------------------------------
1.1 WORKSPACE (ESPAÇO DE TRABALHO)
--------------------------------------------------------------------

O Workspace é o ambiente onde a equipe trabalha.

Tipos:
- Personal → Uso individual
- Team → Compartilhado com equipe
- Public → APIs públicas

Boa prática:
Criar um Workspace por projeto.

--------------------------------------------------------------------
1.2 COLLECTIONS
--------------------------------------------------------------------

Collections organizam as requisições da API.

Exemplo de estrutura:
```
API Sistema
├── Usuários
│   ├── GET - Listar Usuários
│   ├── POST - Criar Usuário
│   ├── PUT - Atualizar Usuário
│   ├── DELETE - Remover Usuário
└── Produtos
    ├── GET - Listar Produtos
    ├── POST - Criar Produto
```
Boas práticas:
- Separar por domínio
- Nomear métodos claramente
- Adicionar descrição explicativa
- Criar pastas para organização

--------------------------------------------------------------------
1.3 MÉTODOS HTTP
--------------------------------------------------------------------

GET     → Buscar dados                   
POST    → Criar recurso                                
PUT     → Atualizar totalmente                         
PATCH   → Atualizar parcialmente                         
DELETE  → Remover recurso                                 

--------------------------------------------------------------------
1.4 BODY — JSON E RAW
--------------------------------------------------------------------

Para enviar dados (POST, PUT, PATCH):

1. Ir em Body
2. Selecionar raw
3. Escolher JSON

Exemplo:
```
{
  "nome": "João Silva",
  "email": "joao@email.com",
  "senha": "123456"
}
```
Sempre verificar:
- Header: Content-Type = application/json
- JSON válido
- Campos obrigatórios

--------------------------------------------------------------------
1.5 BASEURL E ENVIRONMENTS
--------------------------------------------------------------------

Evita repetir endereço da API.

Exemplo errado:
```http://localhost:8080/api/usuarios```

Exemplo correto:
```{{baseURL}}/usuarios```

Criar Environments:
- Dev
- Homologação
- Produção

Exemplo de variáveis:

```baseURL = http://localhost:8080/api```              
```token = eyJhbGciOiJIUzI1NiIsInR...```

Benefícios:
- Alternar ambiente rapidamente
- Segurança
- Organização

--------------------------------------------------------------------
1.6 AUTENTICAÇÃO
--------------------------------------------------------------------

Tipos comuns:
- Bearer Token
- Basic Auth
- API Key

Exemplo Header:

```Authorization: Bearer {{token}}```

Boa prática:
Salvar token como variável de ambiente.

--------------------------------------------------------------------
1.7 SCRIPTS E TESTES AUTOMATIZADOS
--------------------------------------------------------------------

Aba "Tests" permite escrever scripts em JavaScript.

Exemplo:
```
pm.test("Status code é 200", function () {
    pm.response.to.have.status(200);
});

pm.test("Resposta contém nome", function () {
    var jsonData = pm.response.json();
    pm.expect(jsonData.nome).to.eql("João Silva");
});
```
Utilidades:
- Validar resposta
- Automatizar testes
- Garantir qualidade da API

--------------------------------------------------------------------
1.8 PRE-REQUEST SCRIPT
--------------------------------------------------------------------

Executado antes da requisição.

Usado para:
- Gerar token automático
- Criar ID dinâmico
- Gerar datas

--------------------------------------------------------------------
1.9 DOCUMENTAÇÃO E COMENTÁRIOS
--------------------------------------------------------------------

Na descrição da Collection incluir:
- Objetivo da API
- Regras de negócio
- Tipo de autenticação
- Como executar

Registrar resolução de erros:

Exemplo:
```Erro 401 resolvido adicionando Authorization Bearer.```         
Campo email tornou-se obrigatório após ajuste no backend.

Isso evita retrabalho da equipe.

--------------------------------------------------------------------
1.10 VERSIONAMENTO
--------------------------------------------------------------------

Boas práticas:
- Usar padrão semântico (v1.0.0)
- Documentar mudanças
- Exportar Collection em JSON
- Versionar junto ao Git

--------------------------------------------------------------------
🔷 2. SWAGGER (SMARTBEAR) — DOCUMENTAÇÃO E PADRONIZAÇÃO
--------------------------------------------------------------------

O Swagger é um conjunto de ferramentas mantido pela SmartBear para documentação de APIs usando o padrão OpenAPI.

Função principal:
Padronizar e documentar APIs REST.

--------------------------------------------------------------------
2.1 COMO FUNCIONA
--------------------------------------------------------------------

Baseado em um arquivo OpenAPI (YAML ou JSON) que descreve:

- Endpoints
- Métodos HTTP
- Parâmetros
- Respostas
- Modelos de dados
- Códigos de status

Exemplo simplificado:
```
paths:
  /usuarios:
    get:
      summary: Lista usuários
      responses:
        200:
          description: Lista retornada com sucesso
```

--------------------------------------------------------------------
2.2 SWAGGER UI
--------------------------------------------------------------------

Interface visual gerada automaticamente.

Permite:
- Visualizar endpoints
- Ver parâmetros
- Testar requisições
- Ver exemplos de resposta

Muito usado por:
- Frontend
- QA
- Documentação oficial

--------------------------------------------------------------------
2.3 IMPLEMENTAÇÃO
--------------------------------------------------------------------

Passos gerais:

1. Instalar dependência Swagger/OpenAPI
2. Configurar anotações no backend
3. Gerar documentação automaticamente
4. Acessar rota:

```http://localhost:8080/swagger-ui.html```

--------------------------------------------------------------------
2.4 BOAS PRÁTICAS NO SWAGGER
--------------------------------------------------------------------

- Documentar todos os endpoints
- Explicar códigos de erro (400, 401, 404, 500)
- Adicionar exemplos de request/response
- Versionar API (v1, v2)
- Documentar autenticação

--------------------------------------------------------------------
🔷 3. POSTMAN + SWAGGER JUNTOS
--------------------------------------------------------------------

Fluxo ideal:

1. Criar API
2. Documentar no Swagger
3. Importar OpenAPI no Postman
4. Criar testes automatizados
5. Versionar
6. Compartilhar com equipe

Comparação:

Swagger → Define estrutura oficial                
Postman → Testa e automatiza

--------------------------------------------------------------------


# 📘 COMO UTILIZAR NA PRATICA DO ZERO — FLUXO COM SWAGGER + POSTMAN

Este documento explica o fluxo real de uso do Swagger e do Postman
em um projeto grande, desde a documentação até os testes completos da API.

--------------------------------------------------------------------
🔷 1. PAPEL DE CADA FERRAMENTA
--------------------------------------------------------------------

Ferramenta     | Função Principal                  | Objetivo
---------------|----------------------------------|------------------------------
Swagger        | Documentação oficial da API      | Definir contrato da API
Swagger UI     | Teste rápido via navegador       | Validar estrutura
Postman        | Testes completos e automatizados | Validar comportamento real

O Swagger define como a API deve funcionar.
O Postman valida se ela realmente está funcionando corretamente.

--------------------------------------------------------------------
🔷 2. EXEMPLO DE API
--------------------------------------------------------------------

Base URL:
```http://localhost:3000```

Endpoints:
```
GET  /usuarios 
POST /usuarios
```

--------------------------------------------------------------------
🔷 3. IMPLEMENTAÇÃO DO SWAGGER (EXEMPLO EXPLICATIVO)
--------------------------------------------------------------------

Swagger é mantido pela SmartBear e segue o padrão OpenAPI.

Exemplo de configuração:
```
import swaggerUi from "swagger-ui-express";
import swaggerJsdoc from "swagger-jsdoc";

const options = {
  definition: {
    openapi: "3.0.0",
    info: {
      title: "API Sistema",
      version: "1.0.0",
      description: "Documentação da API de usuários"
    },
    servers: [
      {
        url: "http://localhost:3000"
      }
    ]
  },
  apis: ["./src/routes/*.ts"],
};

const specs = swaggerJsdoc(options);
app.use("/docs", swaggerUi.serve, swaggerUi.setup(specs));
```
Acessar:
```http://localhost:3000/docs```

--------------------------------------------------------------------

Exemplo documentando um endpoint:
```
/**
 * @swagger
 * /usuarios:
 *   get:
 *     summary: Lista todos usuários
 *     responses:
 *       200:
 *         description: Lista retornada com sucesso
 */
```
Isso gera automaticamente:
- Método HTTP
- Descrição
- Código de resposta
- Interface visual para teste

--------------------------------------------------------------------
🔷 4. TESTANDO PELO SWAGGER UI
--------------------------------------------------------------------

No navegador:

1. Abrir /docs
2. Selecionar GET /usuarios
3. Clicar em "Try it out"
4. Clicar em "Execute"

Swagger executa a requisição real:

Request URL:
```http://localhost:3000/usuarios```

Resposta exibida:

Campo          | Exemplo
---------------|-----------------------
Status         | 200 OK
Response Body  | JSON com usuários
Content-Type   | application/json

IMPORTANTE:
A URL mostrada no Swagger é a mesma usada no Postman.

--------------------------------------------------------------------
🔷 5. LEVANDO PARA O POSTMAN
--------------------------------------------------------------------

Criar Workspace do projeto.

Criar Collection organizada:
```
API Sistema v1
├── Auth
│   └── POST Login
├── Usuários
│   ├── GET - Listar
│   ├── POST - Criar
│   ├── PUT - Atualizar
│   └── DELETE - Remover
```
--------------------------------------------------------------------
🔷 6. CRIAR ENVIRONMENT NO POSTMAN
--------------------------------------------------------------------

Variáveis recomendadas:

Variável   | Valor
-----------|-------------------------
baseURL    | http://localhost:3000
token      | (gerado no login)

Usar nas requisições:

```{{baseURL}}/usuarios```

Isso permite trocar ambiente sem alterar todas as rotas.

--------------------------------------------------------------------
🔷 7. TESTE GET NO POSTMAN
--------------------------------------------------------------------

Configuração:

Método: ```GET```            
URL: ```{{baseURL}}/usuarios```

Verificações:

Verificação        | Esperado
-------------------|----------
Status             | 200
Content-Type       | application/json
Estrutura JSON     | Array de usuários

--------------------------------------------------------------------
🔷 8. TESTE POST NO POSTMAN
--------------------------------------------------------------------

Body → raw → JSON
```
{
  "nome": "Carlos",
  "email": "carlos@email.com"
}
```
Verificações:

Verificação        | Esperado
-------------------|----------
Status             | 201
Objeto retornado   | Sim
ID gerado          | Sim

--------------------------------------------------------------------
🔷 9. TESTES NEGATIVOS (FUNDAMENTAL)
--------------------------------------------------------------------

Cenário                        | Código Esperado
--------------------------------|----------------
Campo obrigatório ausente      | 400
Token inválido                 | 401
ID inexistente                 | 404
Erro interno inesperado        | 500

Isso garante robustez da API.

--------------------------------------------------------------------
🔷 10. COMPARAÇÃO SWAGGER VS POSTMAN
--------------------------------------------------------------------

Situação                          | Swagger | Postman
-----------------------------------|----------|----------
Ver estrutura da API              | Sim      | Não
Teste rápido manual               | Sim      | Sim
Teste automatizado                | Não      | Sim
Separação por ambientes           | Limitado | Sim
Testar fluxo completo (CRUD)      | Parcial  | Completo

--------------------------------------------------------------------
🔷 11. FLUXO PROFISSIONAL COMPLETO
--------------------------------------------------------------------

Etapa                              | Ferramenta
------------------------------------|--------------
Criar endpoint                     | Backend
Documentar                         | Swagger
Validar estrutura                  | Swagger UI
Criar collection organizada        | Postman
Criar environments                 | Postman
Testar cenários positivos          | Postman
Testar cenários negativos          | Postman
Automatizar validações             | Postman
Versionar API                      | Swagger + Postman

-------------------------------------------------------------------
