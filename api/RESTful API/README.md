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
    /public     -> Endpoints públicos (sem autenticação necessária)
        /evidences      -> Pasta com evidências dos testes (prints)
        /bug-reports    -> Bugs/Defeitos encontrados durante os testes
        /suggestions    -> Sugestões de alterações/inclusões na API baseadas nas execuções dos casos de teste
        /test-plan      -> Plano de testes completo
        /test-cases     -> Casos de teste estruturados e executados com evidências
    /authenticated  -> Endpoints com autenticação obrigatória
        /evidences
        /bug-reports
        /suggestions
        /test-plan
        /test-cases
```




