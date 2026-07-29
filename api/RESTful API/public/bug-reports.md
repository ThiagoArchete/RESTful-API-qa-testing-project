# Bug Reports — API `https://api.restful-api.dev`

## Modelo
| Campo | Detalhes |
| :--- | :--- |
| **Título** |  |
| **Casos de Teste Relacionados** |  |
| **Endpoint** |  |
| **Severidade** |  |
| **Prioridade** |  |
| **Ambiente** |  |
| **Pré-condições** |  |
| **Passo a passo** |  |
| **Resultado Esperado** |  |
| **Resultado Obtido** |  |
| **Evidências** |  |

## BUG-001

| Campo | Detalhes |
| :--- | :--- |
| **Título** | `createdAt` e `updatedAt` retornam em formato de número em vez de string ISO 8601 documentada |
| **Casos de Teste Relacionados** | CT-API-005, CT-API-007 |
| **Endpoint** | `POST` `/objects` |
| **Severidade** | Baixa |
| **Prioridade** | Baixa-Média |
| **Ambiente** | API pública `restful-api.dev`, testado em [28/07/2026] |
| **Pré-condições** | Nenhuma |
| **Passo a passo** | Realizar `POST /objects` com um corpo válido (`name` e `data`) e inspecionar o campo `createdAt` na resposta *E/OU* realizar `PUT /objects/{id}` e inspecionar o campo `updatedAt` na resposta|
| **Resultado Esperado** | String no formato ISO 8601 (ex: `2022-11-21T20:06:23.986Z`), conforme documentado no endpoint |
| **Resultado Obtido** | Número inteiro representando epoch em milissegundos (ex: `1785191908251`) — tipo e formato divergentes do documentado |
| **Evidências** | [Request/Response - POST](../public/evidences/bug-reports/BUG-001-request-response.png) · [Exemplo de response - POST](../public/evidences/bug-reports/BUG-001-response-example.png)|