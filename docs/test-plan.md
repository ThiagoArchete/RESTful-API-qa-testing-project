# Plano de Testes — Endpoints Públicos

## 1. Objetivo

Este plano de testes descreve o escopo e a abordagem utilizada para testar os endpoints públicos da API disponível em https://api.restful-api.dev, com o objetivo de aplicar e documentar de forma estruturada práticas de teste de API com Postman, incluindo versionamento no GitHub.

## 2. Escopo

Os 6 endpoints públicos disponibilizados em https://api.restful-api.dev foram testados:

- `GET /objects` -> Lista todos os objetos.
- `GET /objects/{id}` -> Lista um objeto pelo seu respectivo id.
- `POST /objects` -> Cria um objeto.
- `PUT /objects/{id}` -> Atualiza completamente um objeto pelo seu respectivo id.
- `PATCH /objects/{id}` -> Atualiza parcialmente um objeto pelo seu respectivo id.
- `DELETE /objects/{id}` -> Apaga um objeto pelo seu respectivo id.

### Fora de escopo

- **Testes de segurança** (autenticação, autorização, injeção, etc.) — não se aplicam aos endpoints públicos.
- **Testes de performance/carga** — este plano valida tempo de resposta individual por requisição (limiar de 1000ms, ver seção 6), mas não cobre testes de carga, stress ou concorrência.

## 3. Estratégia de teste

Os casos de teste foram levantados a partir da documentação oficial de cada endpoint, cobrindo cenários de sucesso, erros esperados (dados inválidos, IDs inexistentes, formatos de entrada inesperados) e validação de contrato de resposta (schema, tipos de campo, status code).

O design dos casos foi conduzido de forma exploratória, sem aplicação formal de técnicas como partição de equivalência ou análise de valor limite.

Para validar os casos de teste e critérios de aceite, foram criados scripts automatizados no Postman para cada endpoint, testados individualmente antes da execução da collection completa. Alguns scripts apresentaram falhas na primeira execução, causadas por erros de sintaxe ou lógica de validação — a causa foi identificada e cada script corrigido até refletir corretamente os critérios de aceite definidos.

Todas as requisições da coleção são validadas quanto a tempo de resposta < 1000ms via script de nível de coleção (justificativa do limiar na seção 6).

## 4. Ambiente e ferramentas

- Postman Desktop v12.21.2
- OS Windows 10

## 5. Critérios de entrada/saída

**Critérios de entrada:** disponibilidade da API, acesso ao endpoint e uso de dados válidos ou inválidos de acordo com o cenário a ser testado.

**Critérios de saída:** o teste é considerado concluído quando todos os cenários planejados foram executados, os resultados verificados e o comportamento esperado comparado com o retorno da API, incluindo status code, body e mensagens de erro quando aplicável.

## 6. Riscos e premissas

- A API pública pode ficar instável durante os testes, por ser um ambiente de prática e uso compartilhado.
- Por ser um ambiente compartilhado, objetos criados ou alterados por outros usuários podem, em tese, interferir nos dados observados durante a execução. Os casos de teste foram desenhados para criar e manipular seus próprios dados dentro do mesmo fluxo de execução, reduzindo essa dependência.
- Quando alguma regra de negócio não estava totalmente clara na documentação oficial, foi adotada uma premissa razoável para fins de documentação e execução, registrada no caso de teste correspondente.
- O limiar de 1000ms para tempo de resposta não é um SLA documentado pela API — foi definido como uma referência prática e conservadora para testes básicos, na ausência de um valor oficial publicado. Variações por fatores de rede alheios à aplicação podem ocorrer.