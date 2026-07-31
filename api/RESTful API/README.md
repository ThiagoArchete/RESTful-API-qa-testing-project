# RESTful API — Testes de API Pública (restful-api.dev)

## Sobre o projeto

Este repositório documenta a suíte de testes funcionais construída para a API pública [restful-api.dev](https://restful-api.dev/), cobrindo os 6 endpoints públicos com 15 casos de teste e 65 asserções automatizadas em Postman. O objetivo é demonstrar, na prática, planejamento de testes, design de casos, execução com evidências e reporte estruturado de defeitos.

A documentação completa (plano de testes, casos de teste, evidências e relatórios de bugs) está organizada nas pastas listadas em [Estrutura do repositório](#estrutura-do-repositório).

## Status

| Bloco | Status |
|---|---|
| Endpoints públicos (6) | ✅ Concluído — 15 casos de teste, 65 testes automatizados |
| Endpoints autenticados (9) | ⏳ Em andamento |

## Resultados até o momento

**2 bugs documentados** ([bug-reports](./docs/bug-reports)):
- Campos `createdAt` / `updatedAt` retornados em formato epoch numérico, divergindo do formato ISO 8601 documentado pela API.
- `PATCH` com corpo vazio retorna `404 Not Found` com mensagem referente a corpo inválido — o esperado seria `400 Bad Request`.

**2 itens registrados em sugestões/gaps** ([suggestions](./docs/suggestions)):
- `POST /objects` retorna `200 OK` em vez de `201 Created`. Não é tratado como bug porque a documentação da API não especifica explicitamente o status code esperado para criação.
- Campo `name` é aceito como `null` sem validação de contrato.

Das 65 asserções automatizadas executadas na collection completa, **4 falharam** na primeira execução — todas investigadas e mapeadas para os itens acima (2 bugs + 2 sugestões), sem falhas atribuídas a erro de script.

## Ferramentas

- Postman Desktop v12.21.2
- Windows 10

## Estrutura do repositório

```
/RESTful API
├── README.md
├── /postman-collection
│   └── collection.json
└── /docs
    ├── /test-plan       -> Plano de testes completo
    ├── /test-cases      -> Casos de teste estruturados e executados, com evidências
    ├── /bug-reports     -> Bugs/defeitos encontrados durante os testes
    ├── /suggestions     -> Sugestões de melhoria e gaps identificados (não são bugs formais)
    └── /evidences       -> Prints e evidências de execução
```

## Como rodar os testes

1. Importe `postman-collection/collection.json` no Postman.
2. Os endpoints públicos (pasta `Public`) não exigem configuração adicional — a collection já valida tempo de resposta (< 1000ms) e contrato de resposta automaticamente.
3. Os endpoints autenticados (pasta `Authenticated`) ainda estão em construção. Quando finalizados, será necessário importar um environment e executar a requisição de login previamente para popular o token de acesso — instruções detalhadas serão adicionadas neste README nessa etapa.

## Decisões de escopo e design

- Testes de segurança e de performance/carga estão fora do escopo deste projeto (detalhes em [`docs/test-plan`](./docs/test-plan)).
- Os casos de teste foram levantados a partir da documentação oficial da API, priorizando cenários de sucesso, erro esperado e variações relevantes de entrada. A definição dos casos foi conduzida de forma exploratória, sem aplicação formal de técnicas de design como partição de equivalência ou análise de valor limite.
- O caso CT-API-004 (ID em formato inesperado) foi desmembrado em CT-API-004a–004c após identificar que a categoria original cobria variações de entrada com semânticas de URL distintas. Detalhes em [`docs/test-cases`](./docs/test-cases).