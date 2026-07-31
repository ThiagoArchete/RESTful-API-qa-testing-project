# Bug Reports — API `https://api.restful-api.dev`

## BUG-001

| Campo | Detalhes |
| :--- | :--- |
| **Título** | `createdAt` e `updatedAt` retornam em formato de número em vez de string ISO 8601 documentada |
| **Casos de Teste Relacionados** | CT-API-005, CT-API-007, CT-API-010 |
| **Endpoint** | `POST, PUT e PATCH` `/objects` |
| **Severidade** | Baixa |
| **Prioridade** | Baixa-Média |
| **Ambiente** | API pública `restful-api.dev`, testado em [28/07/2026] |
| **Pré-condições** | Nenhuma |
| **Passo a passo** | Realizar `POST /objects` com um corpo válido (`name` e `data`) e inspecionar o campo `createdAt` na resposta *E/OU* realizar `PUT /objects/{id}` e inspecionar o campo `updatedAt` na resposta|
| **Resultado Esperado** | String no formato ISO 8601 (ex: `2022-11-21T20:06:23.986Z`), conforme documentado no endpoint |
| **Resultado Obtido** | Número inteiro representando epoch em milissegundos (ex: `1785191908251`) — tipo e formato divergentes do documentado |
| **Evidências** | [Request/Response - POST](/evidences/bug-reports/BUG-001-request-response.png) · [Exemplo de response - POST](/evidences/bug-reports/BUG-001-response-example.png)| [Request/Response - PUT](/evidences/bug-reports/BUG-001-request-response-PUT.png) · [Exemplo de response - PUT](/evidences/bug-reports/BUG-001-response-example-PUT.png)|

---

## BUG-002

| Campo | Detalhes |
| :--- | :--- |
| **Título** | Rota `PATCH` com corpo vazio retorna `404 Not Found` com mensagem de erro referente a corpo inválido |
| **Casos de Teste Relacionados** | CT-API-012 |
| **Endpoint** | `PATCH` `/objects/{id}` |
| **Severidade** | Média |
| **Prioridade** | Média |
| **Ambiente** | API pública `restful-api.dev`, testado em [30/07/2026] |
| **Pré-condições** | Objeto existente com ID válido |
| **Passo a passo** | Realizar a requisição `PATCH /objects/{id}` com ID existente com um body request vazio `{}` |
| **Resultado Esperado** | `400 Bad Request` — o erro é de requisição malformada (corpo sem campos válidos) |
| **Resultado Obtido** | `404 Not Found`, com corpo: `{"error": "No valid field(s) to update have been passed as part of a request body."}` — status code inconsistente com a própria mensagem de erro retornada |
| **Evidências** | [Request/Response](/evidences/bug-reports/BUG-002-request-response.png)|