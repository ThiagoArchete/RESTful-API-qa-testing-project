# Sugestões de Implantação na API baseadas nas execuções dos Casos de Teste

## OBS-001 — Status code de criação não segue convenção REST comum

| Campo | Detalhes |
| :--- | :--- |
| **Endpoint:** | `POST /objects`|
| **Observação:** | A API retorna `200 OK` para criação bem-sucedida de recurso. A documentação do endpoint não especifica o status code esperado, portanto isso não constitui divergência de contrato — porém diverge da convenção REST amplamente adotada, que recomenda `201 Created` para esse cenário (novo recurso criado em uma coleção).|
|**Impacto:** | Baixo — o cliente ainda identifica sucesso via faixa 2xx e pelo corpo da resposta.|
| **Tipo:** | Sugestão de melhoria.|
| **Evidência** | [Request/Response](../public/evidences/test-cases/CT-API-005-request-response.png)|

---

## OBS-002 — API permite a criação de objetos com o nome nulo

| Campo | Detalhes |
| :--- | :--- |
| **Endpoint:** | `POST /objects`|
| **Observação:** | A API retorna `200 OK` para criação bem-sucedida de recurso. A documentação do endpoint não especifica que o campo `name` é obrigatório, portanto isso não constitui divergência de contrato — porém para uma integridade correta dos dados da API, não faz sentido manter o cadastro de um objeto que não tenha nome e tenha dados complementares e o ano.|
|**Impacto:** | Médio |
| **Tipo:** | Sugestão de melhoria.|
| **Evidência** | [Request/Response](../public/evidences/test-cases/CT-API-006-request-response.png)|