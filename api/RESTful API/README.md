# RESTful API

Este documento tem como objetivo documentar os testes que realizei na API pública conhecida como RESTful API, criada para testes e prototipagem, para treinar minhas habilidades de testes de API e adicionar ao meu portfólio como demonstração de experiência.

O link da documentação e site oficial da API é https://restful-api.dev/.

A API é focada em um CRUD básico com 6 endpoints públicos e 9 endpoints com autenticação necessária.

Todos os endpoints foram testados e documentados em pastas separadas para os públicos e outra para os autenticados.

Os testes foram conduzidos utilizando o aplicativo Postman para desktop em sua última versão disponibilizada.

## Estrutura de pastas

```text
/RESTful API
    README.MD
    /postman-collection
        collection.json
    /public -> Endpoints públicos (sem autenticação necessária)
        /test-plan
        /test-cases
    /authenticated -> Endpoints com autenticação obrigatória
        /test-plan
        /test-cases
```




