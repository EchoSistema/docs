# Endpoints do Footprints

Este diretório contém documentação detalhada para todos os endpoints do domínio Footprints.

## Lista de Endpoints

| Endpoint | Método | Descrição |
| -------- | ------ | ----------- |
| [FootprintsIndex](./FootprintsIndex.md) | GET | Listar todas as pegadas com opções de filtragem |
| [FootprintsShow](./FootprintsShow.md) | GET | Recuperar detalhes de uma pegada específica |
| [FootprintsStore](./FootprintsStore.md) | POST | Criar um novo registro de pegada |

## Características Comuns

### Autenticação

A maioria dos endpoints requer autenticação:
- `Authorization: Bearer {token}`
- `X-PUBLIC-KEY: {chave_plataforma}`

O endpoint de criação pode ser usado sem autenticação para fins de rastreamento.

### Filtragem

O endpoint de índice suporta:
- Filtragem por intervalo de datas, usuário, plataforma
- Paginação com tamanho de página configurável
- Ocultação de pegadas sensíveis (apenas masters)

## Relacionados

- [Domínio Footprints](../README.md)
