# Plano de Testes - Endpoints Públicos

## 1. Objetivo

Esse plano de testes foi desenvolvido para analisar e documentar o escopo do que e como serão realizados os testes da API nos endpoints públicos. Esses testes foram realizados com o objetivo de treinar minhas capacidades de testes de API com Postman, além de documentar o processo de forma clara e organizada, com versionamento no GitHub.

## 2. Escopo

Neste plano, todos os 6 endpoints disponibilizados na URL https://api.restful-api.dev foram testados, sendo eles:

- GET /objects -> Lista todos os objetos.
- GET /objects/{id} -> Lista um objeto pelo seu respectivo id.
- POST /objects -> Cria um objeto.
- PUT /objects/{id} -> Atualiza completamente um objeto pelo seu respectivo id.
- PATCH /objects/{id} -> Atualiza parcialmente um objeto pelo seu respectivo id.
- DELETE /objects/{id} -> Apaga um objeto pelo seu respectivo id.

## 3. Estratégia de teste

Os testes foram realizados após um levantamento dos casos de teste baseados nos resultados de cada endpoint e nas possíveis variações do request body, response body e HTTP status code.

## 4. Ambiente e ferramentas

- Postman | Última versão disponibilizada
- OS Windows 10

## 5. Critérios de entrada/saída

Critérios de entrada: disponibilidade da API, acesso ao endpoint e uso de dados válidos ou inválidos de acordo com o cenário a ser testado.

Critérios de saída: o teste é considerado concluído quando todos os cenários planejados foram executados, os resultados foram verificados e o comportamento esperado foi comparado com o retorno da API, incluindo status code, body e mensagens de erro, quando aplicável.

## 6. Riscos e premissas

- A API pública pode ficar instável durante os testes, por ser um ambiente de prática e uso compartilhado.
- As premissas foram definidas com base nas regras esperadas da API e nos comportamentos observados durante a execução dos testes.
- Quando alguma regra não estava totalmente clara, foi adotada uma premissa para fins de documentação e execução dos testes.