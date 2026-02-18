# 📘 GUIA COMPLETO — POSTMAN E SWAGGER (SMARTBEAR)

Este README reúne conceitos, organização, boas práticas e instruções práticas para uso do Postman e do Swagger no desenvolvimento de APIs REST.

====================================================================
🔷 1. POSTMAN — TESTES, ORGANIZAÇÃO E AUTOMAÇÃO DE APIs
====================================================================

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

API Sistema
├── Usuários
│   ├── GET - Listar Usuários
│   ├── POST - Criar Usuário
│   ├── PUT - Atualizar Usuário
│   ├── DELETE - Remover Usuário
└── Produtos
    ├── GET - Listar Produtos
    ├── POST - Criar Produto

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

{
  "nome": "João Silva",
  "email": "joao@email.com",
  "senha": "123456"
}

Sempre verificar:
- Header: Content-Type = application/json
- JSON válido
- Campos obrigatórios

--------------------------------------------------------------------
1.5 BASEURL E ENVIRONMENTS
--------------------------------------------------------------------

Evita repetir endereço da API.

Exemplo errado:
http://localhost:8080/api/usuarios

Exemplo correto:
{{baseURL}}/usuarios

Criar Environments:
- Dev
- Homologação
- Produção

Exemplo de variáveis:

baseURL = http://localhost:8080/api
token   = eyJhbGciOiJIUzI1NiIsInR...

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

Authorization: Bearer {{token}}

Boa prática:
Salvar token como variável de ambiente.

--------------------------------------------------------------------
1.7 SCRIPTS E TESTES AUTOMATIZADOS
--------------------------------------------------------------------

Aba "Tests" permite escrever scripts em JavaScript.

Exemplo:

pm.test("Status code é 200", function () {
    pm.response.to.have.status(200);
});

pm.test("Resposta contém nome", function () {
    var jsonData = pm.response.json();
    pm.expect(jsonData.nome).to.eql("João Silva");
});

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
Erro 401 resolvido adicionando Authorization Bearer.
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

====================================================================
🔷 2. SWAGGER (SMARTBEAR) — DOCUMENTAÇÃO E PADRONIZAÇÃO
====================================================================

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

paths:
  /usuarios:
    get:
      summary: Lista usuários
      responses:
        200:
          description: Lista retornada com sucesso

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

http://localhost:8080/swagger-ui.html

--------------------------------------------------------------------
2.4 BOAS PRÁTICAS NO SWAGGER
--------------------------------------------------------------------

- Documentar todos os endpoints
- Explicar códigos de erro (400, 401, 404, 500)
- Adicionar exemplos de request/response
- Versionar API (v1, v2)
- Documentar autenticação

====================================================================
🔷 3. POSTMAN + SWAGGER JUNTOS
====================================================================

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
