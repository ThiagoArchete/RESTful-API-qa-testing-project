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
| **Evidências** | [Request/Response](../public/evidences/test-cases/CT-API-001-request-response.png) · [Test Results](../public/evidences/test-cases/CT-API-001-test-results.png)|
 
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
| **Evidências** | [Request/Response](../public/evidences/test-cases/CT-API-002-request-response.png) · [Test Results](../public/evidences/test-cases/CT-API-002-test-results.png)|

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
| **Evidências** | [Request/Response](../public/evidences/test-cases/CT-API-003-request-response.png) · [Test Results](../public/evidences/test-cases/CT-API-003-test-results.png)|

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
| **Evidências** | [Request/Response](../public/evidences/test-cases/CT-API-004a-request-response.png) · [Test Results](../public/evidences/test-cases/CT-API-004a-test-results.png)|

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
| **Evidências** | [Request/Response](../public/evidences/test-cases/CT-API-004b-request-response.png) · [Test Results](../public/evidences/test-cases/CT-API-004b-test-results.png)|

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
| **Evidências** | [Request/Response](../public/evidences/test-cases/CT-API-004c-request-response.png) · [Test Results](../public/evidences/test-cases/CT-API-004c-test-results.png)|

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
| **Critérios de Aceite / Validações** | `id` gerado é string não vazia; `name` e `data` no response são idênticos ao request; `createdAt` é um timestamp válido; tempo de resposta deve ser menor que 1000ms |
| **Resultado Atual** | Quatro testes passaram e um foi rejeitado (4/5) - Os válidos foram: tempo de execução < 1000ms, `id` gerado é uma string não vazia, `name` e `data` no response são idênticos ao request e `createdAt` é um timestamp válido. O teste inválido foi o status code, o esperado seria `201 Created` por ter criado um objeto na base de dados.|
| **Evidências** | [Request/Response](../public/evidences/test-cases/CT-API-005-request-response.png) · [Test Results](../public/evidences/test-cases/CT-API-005-test-results.png)|

---
 
### CT-API-006 — Rejeitar criação sem o campo `name`
 
| Campo | Detalhes |
| :--- | :--- |
| **Endpoint** | `POST` `/objects` |
| **Pré-condições** | Nenhuma |
| **Request Headers** | `Content-Type: application/json` |
| **Request Body** | <pre>{<br>&nbsp;&nbsp;"data": {<br>&nbsp;&nbsp;&nbsp;&nbsp;"year": 2019<br>&nbsp;&nbsp;}<br>}</pre> |
| **Status Code Esperado** | `400 Bad Request` — **atenção:** a documentação fornecida não declara `name` como obrigatório nem descreve regras de validação. Esta é uma expectativa assumida, não confirmada. Ao executar, registrar o status real (esta API sandbox é conhecida por ser permissiva e pode aceitar/gerar `name` vazio com `201`) |
| **Response Body Esperada** | A confirmar na execução |
| **Critérios de Aceite / Validações** | Se a API validar: mensagem de erro clara referenciando o campo `name`. Se a API não validar: registrar como gap de validação a ser reportado, não como teste "passou"; tempo de resposta deve ser menor que 1000ms |
| **Resultado Atual** | |

---
 
### CT-API-007 — Criar objeto com payload de `data` grande e aninhado
 
| Campo | Detalhes |
| :--- | :--- |
| **Endpoint** | `POST` `/objects` |
| **Pré-condições** | Nenhuma |
| **Request Headers** | `Content-Type: application/json` |
| **Request Body** | <pre>{<br>&nbsp;&nbsp;"name": "Stress Test Object",<br>&nbsp;&nbsp;"data": {<br>&nbsp;&nbsp;&nbsp;&nbsp;"specs": {<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"level1": {<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"level2": {<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"level3": {<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"value": "teste de aninhamento"<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}<br>&nbsp;&nbsp;&nbsp;&nbsp;},<br>&nbsp;&nbsp;&nbsp;&nbsp;"tags": ["a", "b", "c", "... (100+ itens)"]<br>&nbsp;&nbsp;}<br>}</pre> |
| **Status Code Esperado** | `201 Created` (a confirmar se há limite de tamanho de payload documentado) |
| **Response Body Esperada** | Objeto criado com `data` preservando integralmente a estrutura enviada |
| **Critérios de Aceite / Validações** | Estrutura aninhada não é truncada nem corrompida no retorno; se houver limite de tamanho, API deve responder `413 Payload Too Large` de forma explícita, não com erro genérico `500`; tempo de resposta deve ser menor que 1000ms |
| **Resultado Atual** | |
 
---
 
## 4. PUT /objects/{id}
 
### CT-API-008 — Substituir integralmente um objeto existente
 
| Campo | Detalhes |
| :--- | :--- |
| **Endpoint** | `PUT` `/objects/7` |
| **Pré-condições** | Objeto com `id = 7` existente no banco |
| **Request Headers** | `Content-Type: application/json` |
| **Request Body** | <pre>{<br>&nbsp;&nbsp;"name": "Apple MacBook Pro 16",<br>&nbsp;&nbsp;"data": {<br>&nbsp;&nbsp;&nbsp;&nbsp;"year": 2019,<br>&nbsp;&nbsp;&nbsp;&nbsp;"price": 2049.99,<br>&nbsp;&nbsp;&nbsp;&nbsp;"CPU model": "Intel Core i9",<br>&nbsp;&nbsp;&nbsp;&nbsp;"Hard disk size": "1 TB",<br>&nbsp;&nbsp;&nbsp;&nbsp;"color": "silver"<br>&nbsp;&nbsp;}<br>}</pre> |
| **Status Code Esperado** | `200 OK` |
| **Response Body Esperada** | <pre>{<br>&nbsp;&nbsp;"id": "7",<br>&nbsp;&nbsp;"name": "Apple MacBook Pro 16",<br>&nbsp;&nbsp;"data": {<br>&nbsp;&nbsp;&nbsp;&nbsp;"year": 2019,<br>&nbsp;&nbsp;&nbsp;&nbsp;"price": 2049.99,<br>&nbsp;&nbsp;&nbsp;&nbsp;"CPU model": "Intel Core i9",<br>&nbsp;&nbsp;&nbsp;&nbsp;"Hard disk size": "1 TB",<br>&nbsp;&nbsp;&nbsp;&nbsp;"color": "silver"<br>&nbsp;&nbsp;},<br>&nbsp;&nbsp;"updatedAt": "&lt;dinâmico&gt;"<br>}</pre> |
| **Critérios de Aceite / Validações** | `id` inalterado; todos os campos de `data` refletem exatamente o que foi enviado; `updatedAt` atualizado; tempo de resposta deve ser menor que 1000ms |
| **Resultado Atual** | |

---
 
### CT-API-009 — Tentar atualizar objeto com ID inexistente
 
| Campo | Detalhes |
| :--- | :--- |
| **Endpoint** | `PUT` `/objects/999999999` |
| **Pré-condições** | ID `999999999` garantidamente não cadastrado |
| **Request Headers** | `Content-Type: application/json` |
| **Request Body** | <pre>{<br>&nbsp;&nbsp;"name": "Objeto Fantasma",<br>&nbsp;&nbsp;"data": {<br>&nbsp;&nbsp;&nbsp;&nbsp;"year": 2024<br>&nbsp;&nbsp;}<br>}</pre> |
| **Status Code Esperado** | `404 Not Found` — a confirmar se a API cria o recurso via *upsert* em vez de retornar erro (comportamento não documentado) |
| **Response Body Esperada** | A confirmar na execução |
| **Critérios de Aceite / Validações** | API não deve criar silenciosamente um novo recurso sob um `PUT` de ID inexistente sem deixar isso explícito no contrato; se o comportamento for *upsert*, isso deve ser documentado como regra de negócio, não assumido; tempo de resposta deve ser menor que 1000ms |
| **Resultado Atual** | |

---
 
### CT-API-010 — Verificar se `PUT` com body parcial remove os campos omitidos
 
| Campo | Detalhes |
| :--- | :--- |
| **Endpoint** | `PUT` `/objects/7` |
| **Pré-condições** | Objeto com `id = 7` existente e com múltiplos campos em `data` |
| **Request Headers** | `Content-Type: application/json` |
| **Request Body** | <pre>{<br>&nbsp;&nbsp;"name": "Apple MacBook Pro 16"<br>}</pre> |
| **Status Code Esperado** | `200 OK` |
| **Response Body Esperada** | Objeto com `data` ausente ou `null` (comportamento esperado de um `PUT` semanticamente correto, que substitui o recurso por completo) |
| **Critérios de Aceite / Validações** | Confirma se a API trata `PUT` como substituição total (RFC 7231) — campos de `data` não enviados devem desaparecer, diferente do comportamento esperado em `PATCH`; se a API mantiver os campos antigos, isso indica que `PUT` e `PATCH` estão implementados de forma equivalente, o que é uma inconsistência a ser reportada; tempo de resposta deve ser menor que 1000ms |
| **Resultado Atual** | |
 
---
 
## 5. PATCH /objects/{id}
 
### CT-API-011 — Atualizar somente o campo `name`, preservando `data`
 
| Campo | Detalhes |
| :--- | :--- |
| **Endpoint** | `PATCH` `/objects/7` |
| **Pré-condições** | Objeto com `id = 7` existente no banco |
| **Request Headers** | `Content-Type: application/json` |
| **Request Body** | <pre>{<br>&nbsp;&nbsp;"name": "Apple MacBook Pro 16 (Updated Name)"<br>}</pre> |
| **Status Code Esperado** | `200 OK` |
| **Response Body Esperada** | <pre>{<br>&nbsp;&nbsp;"id": "7",<br>&nbsp;&nbsp;"name": "Apple MacBook Pro 16 (Updated Name)",<br>&nbsp;&nbsp;"data": {<br>&nbsp;&nbsp;&nbsp;&nbsp;"year": 2019,<br>&nbsp;&nbsp;&nbsp;&nbsp;"price": 1849.99,<br>&nbsp;&nbsp;&nbsp;&nbsp;"CPU model": "Intel Core i9",<br>&nbsp;&nbsp;&nbsp;&nbsp;"Hard disk size": "1 TB"<br>&nbsp;&nbsp;},<br>&nbsp;&nbsp;"updatedAt": "&lt;dinâmico&gt;"<br>}</pre> |
| **Critérios de Aceite / Validações** | Apenas `name` muda; `data` permanece idêntico ao estado anterior; `updatedAt` é atualizado; tempo de resposta deve ser menor que 1000ms |
| **Resultado Atual** | |

---
 
### CT-API-012 — Tentar atualização parcial em ID inexistente
 
| Campo | Detalhes |
| :--- | :--- |
| **Endpoint** | `PATCH` `/objects/999999999` |
| **Pré-condições** | ID `999999999` garantidamente não cadastrado |
| **Request Headers** | `Content-Type: application/json` |
| **Request Body** | <pre>{<br>&nbsp;&nbsp;"name": "Objeto Fantasma"<br>}</pre> |
| **Status Code Esperado** | `404 Not Found` |
| **Response Body Esperada** | <pre>{<br>&nbsp;&nbsp;"error": "Object with id=999999999 was not found."<br>}</pre> |
| **Critérios de Aceite / Validações** | Status `404`; mensagem de erro consistente com a de `GET`/`PUT` para o mesmo cenário (checa padronização entre endpoints); tempo de resposta deve ser menor que 1000ms |
| **Resultado Atual** | |

---
 
### CT-API-013 — Enviar `PATCH` sem nenhum campo no body
 
| Campo | Detalhes |
| :--- | :--- |
| **Endpoint** | `PATCH` `/objects/7` |
| **Pré-condições** | Objeto com `id = 7` existente no banco |
| **Request Headers** | `Content-Type: application/json` |
| **Request Body** | <pre>{}</pre> |
| **Status Code Esperado** | `200 OK` (a confirmar se a API rejeita body vazio com `400`) |
| **Response Body Esperada** | Objeto retornado idêntico ao estado anterior, exceto possivelmente `updatedAt` |
| **Critérios de Aceite / Validações** | Nenhum campo do objeto é alterado ou apagado; se `updatedAt` mudar mesmo sem alteração real de dado, isso é um comportamento a ser questionado; tempo de resposta deve ser menor que 1000ms |
| **Resultado Atual** | |
 
---
 
## 6. DELETE /objects/{id}
 
### CT-API-014 — Remover um objeto existente com sucesso
 
| Campo | Detalhes |
| :--- | :--- |
| **Endpoint** | `DELETE` `/objects/6` |
| **Pré-condições** | Objeto com `id = 6` existente no banco |
| **Request Headers** | Nenhum obrigatório |
| **Request Body** | N/A |
| **Status Code Esperado** | `200 OK` |
| **Response Body Esperada** | <pre>{<br>&nbsp;&nbsp;"message": "Object with id = 6, has been deleted."<br>}</pre> |
| **Critérios de Aceite / Validações** | Mensagem confirma o `id` deletado; um `GET /objects/6` subsequente deve retornar `404` (checagem cruzada entre endpoints); tempo de resposta deve ser menor que 1000ms |
| **Resultado Atual** | |

---
 
### CT-API-015 — Tentar deletar objeto que não existe
 
| Campo | Detalhes |
| :--- | :--- |
| **Endpoint** | `DELETE` `/objects/999999999` |
| **Pré-condições** | ID `999999999` garantidamente não cadastrado |
| **Request Headers** | Nenhum obrigatório |
| **Request Body** | N/A |
| **Status Code Esperado** | `404 Not Found` |
| **Response Body Esperada** | <pre>{<br>&nbsp;&nbsp;"error": "Object with id=999999999 was not found."<br>}</pre> |
| **Critérios de Aceite / Validações** | Status `404`; API não retorna `200` com mensagem de sucesso para um recurso que nunca existiu; tempo de resposta deve ser menor que 1000ms |
| **Resultado Atual** | |

---
 
### CT-API-016 — Deletar o mesmo objeto duas vezes em sequência rápida
 
| Campo | Detalhes |
| :--- | :--- |
| **Endpoint** | `DELETE` `/objects/8` (executado duas vezes, a segunda imediatamente após a primeira) |
| **Pré-condições** | Objeto com `id = 8` existente no banco antes da primeira chamada |
| **Request Headers** | Nenhum obrigatório |
| **Request Body** | N/A |
| **Status Code Esperado** | 1ª chamada: `200 OK`. 2ª chamada: `404 Not Found` |
| **Response Body Esperada** | 1ª: mensagem de sucesso. 2ª: mensagem de "não encontrado" |
| **Critérios de Aceite / Validações** | A segunda chamada não deve retornar `200` novamente (idempotência mal implementada seria um bug); nota: como esta é uma API pública compartilhada, o teste real de concorrência (2 requisições simultâneas) é mais confiável em uma instância dedicada/mockada do que neste sandbox público; tempo de resposta deve ser menor que 1000ms |
| **Resultado Atual** | |