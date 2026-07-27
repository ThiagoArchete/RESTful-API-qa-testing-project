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

Os casos de teste foram levantados a partir da documentação oficial de cada endpoint, cobrindo as principais variações esperadas de request body, response body e HTTP status code.

Para validar os casos de teste e critérios de aceite, criei scripts automatizados no Postman para cada endpoint, testando-os individualmente antes de prosseguir. Alguns scripts apresentaram falhas na primeira execução, causadas por erros de sintaxe ou lógica de validação — identifiquei a causa e corrigi cada um até refletirem corretamente os critérios de aceite definidos.

Todas as requisições da coleção são validadas quanto a tempo de resposta < 1000ms via script de nível de coleção.

Durante a execução dos casos de teste, o caso CT-API-004 previamente foi criado para veriricar a resposta da API quanto a um formato de ID inesperado, com caracteres especiais, porém ele foi substituído por 004a–004c após identificação de que 'ID em formato inesperado' cobria categorias de entrada com semânticas de URL diferentes

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
- Limiar de 1000ms para todos os endpoints é uma referência de SLA aproximada; variações podem ocorrer por fatores de rede alheios à aplicação.