# Endpoints do PaymentHub

Este diretório contém documentação detalhada para todos os endpoints do domínio PaymentHub.

## Lista de Endpoints

### Ordens de Pagamento

| Endpoint | Método | Descrição |
| -------- | ------ | ----------- |
| [PaymentOrderIndex](./PaymentOrderIndex.md) | GET | Listar todas as ordens de pagamento da plataforma autenticada |
| [PaymentOrderShow](./PaymentOrderShow.md) | GET | Recuperar informações detalhadas sobre uma ordem de pagamento específica |

## Características Comuns

### Autenticação

Todos os endpoints requerem:
- `Authorization: Bearer {token}`
- `X-PUBLIC-KEY: {chave_plataforma}`

### Paginação

Os endpoints de listagem suportam paginação:
- `per_page`: Número de resultados por página (padrão: 10)

### Formato de Resposta

Todas as respostas seguem o formato JSON padrão com wrapper `data` e metadados de paginação quando aplicável.

## Documentação Relacionada

- [Visão Geral do Domínio PaymentHub](../README.md)
- [Domínio Ecommerce](../../Ecommerce/README.md)
