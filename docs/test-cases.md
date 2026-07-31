# Casos de Teste — API `https://api.restful-api.dev`
 
> Nota de execução: para os campos `id`, `createdAt` e `updatedAt`, a API gera esses valores dinamicamente no momento da requisição. Por isso, nas respostas esperadas, esses campos aparecem marcados como `<dinâmico>` em vez de um valor fixo — não é possível prever o valor exato antes de executar o teste. O critério de aceite correspondente descreve o que checar em vez do valor exato.
 
---
 
## 1. GET /objects
 
### CT-API-001 — Listar todos os objetos cadastrados com sucesso
 
| Campo | Detalhes |
| :--- | :--- |
| **Endpoint** | `GET` `/objects` |
| **Pré-condições** | Existem pelo menos 2 ou mais objetos cadastrados no banco |
| **Request Headers** | Nenhum obrigatório |
| **Request Body** | N/A |
| **Status Code Esperado** | `200 OK` |
| **Response Body Esperada** | <pre>[<br>&nbsp;&nbsp;{<br>&nbsp;&nbsp;&nbsp;&nbsp;"id": "1",<br>&nbsp;&nbsp;&nbsp;&nbsp;"name": "Google Pixel 6 Pro",<br>&nbsp;&nbsp;&nbsp;&nbsp;"data": {<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"color": "Cloudy White",<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"capacity": "128 GB"<br>&nbsp;&nbsp;&nbsp;&nbsp;}<br>&nbsp;&nbsp;}<br>]</pre> |
| **Critérios de Aceite / Validações** | Response é um array não vazio; cada objeto possui `id` (string) e `name` (string); `data` pode ser objeto ou `null`; tempo de resposta deve ser menor que 1000ms |
| **Resultado Atual** | Todos os testes automatizados foram aprovados (5/5): tempo de execução < 1000ms, status code 200, array não vazio, e cada objeto validado com `id`/`name` como string e `data` como objeto ou `null`|
| **Evidências** | [Request/Response](/evidences/test-cases/CT-API-001-request-response.png) · [Test Results](/evidences/test-cases/CT-API-001-test-results.png)|
 
---
 
## 2. GET /objects/{id}
 
### CT-API-002 — Retornar detalhes de um objeto a partir de um ID válido
 
| Campo | Detalhes |
| :--- | :--- |
| **Endpoint** | `GET` `/objects/7` |
| **Pré-condições** | Objeto com `id = 7` existente no banco |
| **Request Headers** | Nenhum obrigatório |
| **Request Body** | N/A |
| **Status Code Esperado** | `200 OK` |
| **Response Body Esperada** | <pre>{<br>&nbsp;&nbsp;"id": "7",<br>&nbsp;&nbsp;"name": "Apple MacBook Pro 16",<br>&nbsp;&nbsp;"data": {<br>&nbsp;&nbsp;&nbsp;&nbsp;"year": 2019,<br>&nbsp;&nbsp;&nbsp;&nbsp;"price": 1849.99,<br>&nbsp;&nbsp;&nbsp;&nbsp;"CPU model": "Intel Core i9",<br>&nbsp;&nbsp;&nbsp;&nbsp;"Hard disk size": "1 TB"<br>&nbsp;&nbsp;}<br>}</pre> |
| **Critérios de Aceite / Validações** | `id` retornado é igual ao solicitado; `name` é string não vazia; estrutura do objeto corresponde ao que foi cadastrado; tempo de resposta deve ser menor que 1000ms |
| **Resultado Atual** | Todos os testes automatizados foram aprovados (5/5): tempo de execução < 1000ms, status code 200, ID retornado igual ao solicitado, `name` do objeto validado como string não vazia e a estrutura do objeto condiz com o que foi criado e cada objeto validado|
| **Evidências** | [Request/Response](/evidences/test-cases/CT-API-002-request-response.png) · [Test Results](/evidences/test-cases/CT-API-002-test-results.png)|

 ---

### CT-API-003 — Retornar erro ao buscar ID que não existe no banco
 
| Campo | Detalhes |
| :--- | :--- |
| **Endpoint** | `GET` `/objects/999999999` |
| **Pré-condições** | ID `999999999` garantidamente não cadastrado |
| **Request Headers** | Nenhum obrigatório |
| **Request Body** | N/A |
| **Status Code Esperado** | `404 Not Found` |
| **Response Body Esperada** | <pre>{<br>&nbsp;&nbsp;"error": "Object with id=999999999 was not found."<br>}</pre> |
| **Critérios de Aceite / Validações** | Status `404`; mensagem de erro identifica o ID buscado; nenhum dado sensível de outro objeto vaza na resposta; tempo de resposta deve ser menor que 1000ms |
| **Resultado Atual** | Todos os testes automatizados foram aprovados (3/3): tempo de execução < 1000ms, status code `404 Not Found`, mensagem de erro identifica o `id` buscado e não contém dados sensíveis|
| **Evidências** | [Request/Response](/evidences/test-cases/CT-API-003-request-response.png) · [Test Results](/evidences/test-cases/CT-API-003-test-results.png)|

---

### CT-API-004a — Busca com ID contendo caractere especial não reservado

| Campo | Detalhes |
| :--- | :--- |
| **Endpoint** | `GET` `/objects/!` |
| **Pré-condições** | Nenhuma |
| **Request Headers** | Nenhum obrigatório |
| **Request Body** | N/A |
| **Status Code Esperado** | `404 Not Found` (hipótese baseada no padrão observado de busca sem validação prévia de formato — a confirmar na execução) |
| **Response Body Esperada** | <pre>{<br>&nbsp;&nbsp;"error": "Object with id=! was not found."<br>}</pre> |
| **Critérios de Aceite / Validações** | mensagem de erro identifica o ID buscado; nenhum dado sensível de outro objeto vaza na resposta; tempo de resposta deve ser menor que 1000ms |
| **Resultado Atual** | Todos os testes automatizados foram aprovados (3/3): tempo de execução < 1000ms, status code `404 Not Found`, mensagem de erro identifica o `id` buscado e não contém dados sensíveis|
| **Evidências** | [Request/Response](/evidences/test-cases/CT-API-004a-request-response.png) · [Test Results](/evidences/test-cases/CT-API-004a-test-results.png)|

### CT-API-004b — Busca com ID contendo caractere reservado da URL, devidamente codificado

| Campo | Detalhes |
| :--- | :--- |
| **Endpoint** | `GET` `/objects/%23` |
| **Pré-condições** | Nenhuma |
| **Request Headers** | Nenhum obrigatório |
| **Request Body** | N/A |
| **Status Code Esperado** | `404 Not Found` (hipótese — a confirmar na execução) |
| **Response Body Esperada** | <pre>{<br>&nbsp;&nbsp;"error": "Object with id=# was not found."<br>}</pre> |
| **Critérios de Aceite / Validações** | mensagem de erro identifica o ID codificado; nenhum dado sensível de outro objeto vaza na resposta; tempo de resposta deve ser menor que 1000ms |
| **Resultado Atual** | Todos os testes automatizados foram aprovados (3/3): tempo de execução < 1000ms, status code `404 Not Found`, mensagem de erro identifica o `id` codificado e não contém dados sensíveis|
| **Evidências** | [Request/Response](/evidences/test-cases/CT-API-004b-request-response.png) · [Test Results](/evidences/test-cases/CT-API-004b-test-results.png)|

### CT-API-004c — Busca com percent-encoding malformado na URL

| Campo | Detalhes |
| :--- | :--- |
| **Endpoint** | `GET` `/objects/%` |
| **Pré-condições** | Nenhuma |
| **Request Headers** | Nenhum obrigatório |
| **Request Body** | N/A |
| **Status Code Esperado** | `400 Bad Request` |
| **Response Body Esperada** | A confirmar na execução |
| **Critérios de Aceite / Validações** | mensagem de erro indica requisição malformada; tempo de resposta deve ser menor que 1000ms |
| **Resultado Atual** | Todos os testes automatizados foram aprovados (3/3): tempo de execução < 1000ms, status code `400 Bad Request`, mensagem de erro indica requisição malformada|
| **Evidências** | [Request/Response](/evidences/test-cases/CT-API-004c-request-response.png) · [Test Results](/evidences/test-cases/CT-API-004c-test-results.png)|

---
 
## 3. POST /objects
 
### CT-API-005 — Criar novo objeto com `name` e `data` válidos
 
| Campo | Detalhes |
| :--- | :--- |
| **Endpoint** | `POST` `/objects` |
| **Pré-condições** | Nenhuma |
| **Request Headers** | `Content-Type: application/json` |
| **Request Body** | <pre>{<br>&nbsp;&nbsp;"name": "Apple MacBook Pro 16",<br>&nbsp;&nbsp;"data": {<br>&nbsp;&nbsp;&nbsp;&nbsp;"year": 2019,<br>&nbsp;&nbsp;&nbsp;&nbsp;"price": 1849.99,<br>&nbsp;&nbsp;&nbsp;&nbsp;"CPU model": "Intel Core i9",<br>&nbsp;&nbsp;&nbsp;&nbsp;"Hard disk size": "1 TB"<br>&nbsp;&nbsp;}<br>}</pre> |
| **Status Code Esperado** | `201 Created` |
| **Response Body Esperada** | <pre>{<br>&nbsp;&nbsp;"id": "&lt;dinâmico&gt;",<br>&nbsp;&nbsp;"name": "Apple MacBook Pro 16",<br>&nbsp;&nbsp;"data": {<br>&nbsp;&nbsp;&nbsp;&nbsp;"year": 2019,<br>&nbsp;&nbsp;&nbsp;&nbsp;"price": 1849.99,<br>&nbsp;&nbsp;&nbsp;&nbsp;"CPU model": "Intel Core i9",<br>&nbsp;&nbsp;&nbsp;&nbsp;"Hard disk size": "1 TB"<br>&nbsp;&nbsp;},<br>&nbsp;&nbsp;"createdAt": "&lt;dinâmico&gt;"<br>}</pre> |
| **Critérios de Aceite / Validações** | `id` gerado é string não vazia; `name` e `data` no response são idênticos ao request; `createdAt` é uma string em formato ISO 8601 válida; tempo de resposta deve ser menor que 1000ms |
| **Resultado Atual** | Três testes passaram e dois foram rejeitados (3/5) - Os válidos foram: tempo de execução < 1000ms, `id` gerado é uma string não vazia e `name` e `data` no response são idênticos ao request. Os testes inválidos foram: status code, o esperado seria `201 Created` por ter criado um objeto na base de dados, mas o do sistema foi `200 OK`; O segundo teste inválidado foi o `createdAt` deveria ser uma string no formato ISO 8601, conforme está na documentação, porém a API retorna um formato diferente númerico. |
| **Evidências** | [Request/Response](/evidences/test-cases/CT-API-005-request-response.png) · [Test Results](/evidences/test-cases/CT-API-005-test-results.png)|

---
 
### CT-API-006 — Rejeitar criação sem o campo `name`
 
| Campo | Detalhes |
| :--- | :--- |
| **Endpoint** | `POST` `/objects` |
| **Pré-condições** | Nenhuma |
| **Request Headers** | `Content-Type: application/json` |
| **Request Body** | <pre>{<br>&nbsp;&nbsp;"data": {<br>&nbsp;&nbsp;&nbsp;&nbsp;"year": 2019<br>&nbsp;&nbsp;}<br>}</pre> |
| **Status Code Esperado** | `400 Bad Request` |
| **Response Body Esperada** | A verificar  |
| **Critérios de Aceite / Validações** | status code deveria ser `400 Bad Request`, dado que name é o campo identificador do recurso — documentação não especifica obrigatoriedade |
| **Resultado Atual** | Um teste automatizado passou e um falhou (1/2) - O tempo de execução < 1000ms foi validado corretamente, enquanto o status code retornado foi `200 OK` ao invés de `400 Bad Request` e o objeto foi criado com o campo `name` como `null`|
| **Evidências** | [Request/Response](/evidences/test-cases/CT-API-006-request-response.png) · [Test Results](/evidences/test-cases/CT-API-006-test-results.png)|

---
 
## 4. PUT /objects/{id}
 
### CT-API-007 — Substituir inteiramente um objeto existente
 
| Campo | Detalhes |
| :--- | :--- |
| **Endpoint** | `PUT` `/objects/{id de um objeto criado}` |
| **Pré-condições** | Possuir um objeto cadastrado no banco, que não seja um objeto padrão que já vem cadastrado |
| **Request Headers** | `Content-Type: application/json` |
| **Request Body** | <pre>{<br>&nbsp;&nbsp;"name": "Iphone 17 Pro Max",<br>&nbsp;&nbsp;"data": {<br>&nbsp;&nbsp;&nbsp;&nbsp;"year": 2025,<br>&nbsp;&nbsp;&nbsp;&nbsp;"price": 12.500,<br>&nbsp;&nbsp;&nbsp;&nbsp;"CPU model": "Apple A19 Pro",<br>&nbsp;&nbsp;&nbsp;&nbsp;"Hard disk size": "256 GB",<br>&nbsp;&nbsp;&nbsp;&nbsp;"color": "silver"<br>&nbsp;&nbsp;}<br>}</pre> |
| **Status Code Esperado** | `200 OK` |
| **Response Body Esperada** | <pre>{<br>&nbsp;&nbsp;"id": "{dinâmico}",<br>&nbsp;&nbsp;"name": "Iphone 17 Pro Max",<br>&nbsp;&nbsp;"data": {<br>&nbsp;&nbsp;&nbsp;&nbsp;"year": 2025,<br>&nbsp;&nbsp;&nbsp;&nbsp;"price": 12.500,<br>&nbsp;&nbsp;&nbsp;&nbsp;"CPU model": "Apple A19 Pro",<br>&nbsp;&nbsp;&nbsp;&nbsp;"Hard disk size": "256 GB",<br>&nbsp;&nbsp;&nbsp;&nbsp;"color": "silver"<br>&nbsp;&nbsp;},<br>&nbsp;&nbsp;"updatedAt": "&lt;dinâmico&gt;"<br>}</pre> |
| **Critérios de Aceite / Validações** | `id` inalterado; todos os campos de `data` refletem exatamente o que foi enviado; `updatedAt` atualizado e difere do tempo do `createdAt` quado o objeto foi criado; tempo de resposta deve ser menor que 1000ms |
| **Resultado Atual** | Todos os testes automatizados passaram (5/5) - tempo de execução < 1000ms, status code `200 OK`, `id` do objeto permanece inalterado, os campos de `data` refletem o que foi enviado e o campo `updatedAt` é maior do que o valor de `createdAt` |
| **Evidências** | [Request/Response](/evidences/test-cases/CT-API-007-request-response.png) · [Test Results](/evidences/test-cases/CT-API-007-test-results.png)|

---
 
### CT-API-008 — Tentar atualizar objeto com ID inexistente
 
| Campo | Detalhes |
| :--- | :--- |
| **Endpoint** | `PUT` `/objects/999999999` |
| **Pré-condições** | ID `999999999` garantidamente não cadastrado |
| **Request Headers** | `Content-Type: application/json` |
| **Request Body** | <pre>{<br>&nbsp;&nbsp;"name": "Objeto Fantasma",<br>&nbsp;&nbsp;"data": {<br>&nbsp;&nbsp;&nbsp;&nbsp;"year": 2024<br>&nbsp;&nbsp;}<br>}</pre> |
| **Status Code Esperado** | `404 Not Found`|
| **Response Body Esperada** | Erro informando que o objeto não existe. |
| **Critérios de Aceite / Validações** | API retorna erro informando que o objeto com o `id` inserido não existe; tempo de resposta deve ser menor que 1000ms |
| **Resultado Atual** | Todos os testes automatizados passaram (3/3) - tempo de execução < 1000ms, status code `404 Not Found` e a response body retorna mensagem de erro identificando o ID correto buscado e informando que o mesmo não existe|
| **Evidências** | [Request/Response](/evidences/test-cases/CT-API-008-request-response.png) · [Test Results](/evidences/test-cases/CT-API-008-test-results.png)|

---
 
### CT-API-009 — Verificar se `PUT` com body parcial remove os campos omitidos
 
| Campo | Detalhes |
| :--- | :--- |
| **Endpoint** | `PUT` `/objects/{id de um objeto criado}` |
| **Pré-condições** | Possuir um objeto cadastrado no banco, que não seja um objeto padrão que já vem cadastrado, com múltiplos campos em `data` |
| **Request Headers** | `Content-Type: application/json` |
| **Request Body** | <pre>{<br>&nbsp;&nbsp;"name": "Apple MacBook Pro 16"<br>}</pre> |
| **Status Code Esperado** | `200 OK` |
| **Response Body Esperada** | Objeto com `data` ausente ou `null` (comportamento esperado de um `PUT` semanticamente correto, que substitui o recurso por completo) |
| **Critérios de Aceite / Validações** | Campos de `data` não enviados devem desaparecer; campo `name` deve permanecer inalterado; response body deve identificar o `id` atualizado; tempo de resposta deve ser menor que 1000ms |
| **Resultado Atual** | Todos os testes automatizados passaram (5/5) - tempo de execução < 1000ms, status code `200 OK`, campo `data` se tornou `null`, campo `name` permaneceu inalterado e o `id` do objeto atualizado na response body é o mesmo inserido na URL da requisição |
| **Evidências** | [Request/Response](/evidences/test-cases/CT-API-009-request-response.png) · [Test Results](/evidences/test-cases/CT-API-009-test-results.png)|

---
 
## 5. PATCH /objects/{id}
 
### CT-API-010 — Atualizar somente o campo `name`, preservando `data`
 
| Campo | Detalhes |
| :--- | :--- |
| **Endpoint** | `PATCH` `/objects/{id de um objeto criado}` |
| **Pré-condições** | Possuir um objeto cadastrado no banco, que não seja um objeto padrão que já vem cadastrado |
| **Request Headers** | `Content-Type: application/json` |
| **Request Body** | <pre>{<br>&nbsp;&nbsp;"name": "Apple MacBook Pro 16 (Updated Name)"<br>}</pre> |
| **Status Code Esperado** | `200 OK` |
| **Response Body Esperada** | <pre>{<br>&nbsp;&nbsp;"id": "{dinâmico}",<br>&nbsp;&nbsp;"name": "Apple MacBook Pro 16 (Updated Name)",<br>&nbsp;&nbsp;"data": {<br>&nbsp;&nbsp;&nbsp;&nbsp;"year": 2019,<br>&nbsp;&nbsp;&nbsp;&nbsp;"price": 1849.99,<br>&nbsp;&nbsp;&nbsp;&nbsp;"CPU model": "Intel Core i9",<br>&nbsp;&nbsp;&nbsp;&nbsp;"Hard disk size": "1 TB"<br>&nbsp;&nbsp;},<br>&nbsp;&nbsp;"updatedAt": "&lt;dinâmico&gt;"<br>}</pre> |
| **Critérios de Aceite / Validações** | Apenas `name` muda e `data` permanece idêntico ao estado anterior; `id` identificado na response é o mesmo na URL da request; `updatedAt` é atualizado; tempo de resposta deve ser menor que 1000ms |
| **Resultado Atual** | Todos os testes automatizados passaram (5/5) - tempo de execução < 1000ms, status code `200 OK`, apenas o campo `name` é alterado e a `data` se mantém, o `id` do objeto atualizado na response body é o mesmo inserido na URL da requisição e o campo `updatedAt` é maior do que o valor de `createdAt`|
| **Evidências** | [Request/Response](/evidences/test-cases/CT-API-010-request-response.png) · [Test Results](/evidences/test-cases/CT-API-010-test-results.png) · [Pre-request Script](/evidences/test-cases/CT-API-010-pre-request-script.png)|

---
 
### CT-API-011 — Tentar atualização parcial em ID inexistente
 
| Campo | Detalhes |
| :--- | :--- |
| **Endpoint** | `PATCH` `/objects/999999999` |
| **Pré-condições** | ID `999999999` garantidamente não cadastrado |
| **Request Headers** | `Content-Type: application/json` |
| **Request Body** | <pre>{<br>&nbsp;&nbsp;"name": "Objeto Fantasma"<br>}</pre> |
| **Status Code Esperado** | `404 Not Found` |
| **Response Body Esperada** | <pre>{<br>&nbsp;&nbsp;"error": "Object with id=999999999 was not found."<br>}</pre> |
| **Critérios de Aceite / Validações** | Status `404`; mensagem de erro consistente com a de `GET`/`PUT` para o mesmo cenário (checa padronização entre endpoints); tempo de resposta deve ser menor que 1000ms |
| **Resultado Atual** | Todos os testes automatizados passaram (3/3) - tempo de execução < 1000ms, status code `404 Not Found` e a response body retorna mensagem de erro identificando o ID correto buscado e informando que o mesmo não existe, mesmo padrão das rotas `GET` e `PUT`.|
| **Evidências** | [Request/Response](/evidences/test-cases/CT-API-011-request-response.png) · [Test Results](/evidences/test-cases/CT-API-011-test-results.png)|

---
 
### CT-API-012 — Enviar `PATCH` sem nenhum campo no body
 
| Campo | Detalhes |
| :--- | :--- |
| **Endpoint** | `PATCH` `/objects/{id de um objeto criado}` |
| **Pré-condições** | Possuir um objeto cadastrado no banco, que não seja um objeto padrão que já vem cadastrado |
| **Request Headers** | `Content-Type: application/json` |
| **Request Body** | <pre>{}</pre> |
| **Status Code Esperado** | `400 Bad Request` |
| **Response Body Esperada** | <pre>{<br>&nbsp;&nbsp;"error": "No valid field(s) to update have been passed as part of a request body."<br>}</pre> |
| **Critérios de Aceite / Validações** | Response retorna erro informando que nenhum campo válido para atualizar foi passado na request body; tempo de resposta deve ser menor que 1000ms |
| **Resultado Atual** | Três testes automatizados passaram e um falhou (3/4) - Os válidos foram: tempo de execução < 1000ms, response body possui somente o campo `error` e a mensagem de erro é a response esperada. O teste que falhou foi o do status code esperado, onde foi retornado o código `404 Not Found`, o que não faz sentido porque o `id` inserido na URL da request existe na base de dados, um bug será documentado sobre esse caso.|
| **Evidências** | [Request/Response](/evidences/test-cases/CT-API-012-request-response.png) · [Test Results](/evidences/test-cases/CT-API-012-test-results.png)|

---
 
## 6. DELETE /objects/{id}
 
### CT-API-013 — Remover um objeto existente com sucesso
 
| Campo | Detalhes |
| :--- | :--- |
| **Endpoint** | `DELETE` `/objects/{id de um objeto criado}` |
| **Pré-condições** | Possuir um objeto cadastrado no banco, que não seja um objeto padrão que já vem cadastrado |
| **Request Headers** | Nenhum obrigatório |
| **Request Body** | N/A |
| **Status Code Esperado** | `200 OK` |
| **Response Body Esperada** | <pre>{<br>&nbsp;&nbsp;"message": "Object with id = {id}, has been deleted."<br>}</pre> |
| **Critérios de Aceite / Validações** | Response confirma o `id` deletado; um `GET /objects/{id}` subsequente deve retornar `404`; tempo de resposta deve ser menor que 1000ms |
| **Resultado Atual** | Todos os testes automatizados passaram (4/4) - tempo de execução < 1000ms, status code `200 OK`, a `responde body` confirma o `id` deletado igual ao inserido na URL da request, e o último teste automatizado foi inserido em uma requisição `GET` em sequência ao `DELETE` com uma automação para verificar se o status code é `404 Not Found`, foi feito dessa forma para automatizar no momento da collection run. |
| **Evidências** | [Request/Response (DELETE)](/evidences/test-cases/CT-API-013-request-response.png) · [Test Results (DELETE)](/evidences/test-cases/CT-API-013-test-results.png) · [Request/Response (GET)](/evidences/test-cases/CT-API-013b-request-response.png) · [Test Result (GET)](/evidences/test-cases/CT-API-013b-test-results.png)| 

---
 
### CT-API-014 — Tentar deletar objeto que não existe
 
| Campo | Detalhes |
| :--- | :--- |
| **Endpoint** | `DELETE` `/objects/999999999` |
| **Pré-condições** | ID `999999999` garantidamente não cadastrado |
| **Request Headers** | Nenhum obrigatório |
| **Request Body** | N/A |
| **Status Code Esperado** | `404 Not Found` |
| **Response Body Esperada** | <pre>{<br>&nbsp;&nbsp;"error": "Object with id=999999999 was not found."<br>}</pre> |
| **Critérios de Aceite / Validações** | Status `404`; response identifica que o `id` buscado não existe; tempo de resposta deve ser menor que 1000ms |
| **Resultado Atual** | Todos os testes automatizados passaram (3/3) - tempo de execução < 1000ms, status code `404 Not Found` e a response body retorna mensagem de erro identificando o ID correto buscado e informando que o mesmo não existe, mesmo padrão das rotas `GET`, `PUT` e `PATCH`. |
| **Evidências** | [Request/Response](/evidences/test-cases/CT-API-014-request-response.png) · [Test Results](/evidences/test-cases/CT-API-014-test-results.png)| 

---
 
### CT-API-015 — Tentar deletar sem inserir um ID
 
| Campo | Detalhes |
| :--- | :--- |
| **Endpoint** | `DELETE` `/objects/` |
| **Pré-condições** | Nenhuma obrigatória  |
| **Request Headers** | Nenhum obrigatório |
| **Request Body** | N/A |
| **Status Code Esperado** | `405 Method Not Allowed` |
| **Response Body Esperada** | À confirmar na execução |
| **Critérios de Aceite / Validações** | A response informa que o método não é permitido; tempo de resposta deve ser menor que 1000ms |
| **Resultado Atual** | Todos os testes automatizados passaram (3/3) - tempo de execução < 1000ms, status code `405 Method Not Allowed` e a response body retorna mensagem de erro informando que o método não é permitido.|
| **Evidências** | [Request/Response](/evidences/test-cases/CT-API-015-request-response.png) · [Test Results](/evidences/test-cases/CT-API-015-test-results.png)| 