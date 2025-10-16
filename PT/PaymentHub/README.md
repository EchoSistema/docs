# Domínio PaymentHub

O domínio PaymentHub gerencia o gerenciamento e recuperação de ordens de pagamento para a plataforma.

## Visão Geral

Este domínio fornece endpoints para:
- Listar ordens de pagamento associadas a uma plataforma
- Recuperar informações detalhadas sobre ordens de pagamento específicas
- Gerenciar transações de pagamento e parcelas

## Autenticação

Todos os endpoints requerem autenticação via token Bearer com o cabeçalho `X-PUBLIC-KEY`.

## Caminho Base

```
/api/v1/payment-hub
```

## Endpoints Disponíveis

- [Índice de Ordens de Pagamento](./Endpoints/PaymentOrderIndex.md) - Listar todas as ordens de pagamento da plataforma
- [Exibir Ordem de Pagamento](./Endpoints/PaymentOrderShow.md) - Obter detalhes de uma ordem de pagamento específica

## Recursos

### PaymentOrder

Uma ordem de pagamento representa uma transação ou intenção de pagamento criada por um usuário da plataforma.

**Atributos principais:**
- `uuid`: Identificador único
- `status`: Status atual do pagamento
- `amount`: Valor do pagamento
- `currency`: Moeda do pagamento
- `installments`: Detalhes de parcelas do pagamento (quando aplicável)

## Domínios Relacionados

- **Shared**: Recursos comuns e autenticação
- **Ecommerce**: Compras de produtos que geram ordens de pagamento

## Notas

- As ordens de pagamento são delimitadas por plataformas
- Os dados de parcelas só são carregados ao visualizar uma ordem de pagamento específica
- A paginação está disponível com tamanho de página configurável (padrão: 10)

## Suporte

Para dúvidas ou problemas relacionados aos endpoints do PaymentHub, consulte a documentação individual de cada endpoint ou entre em contato com a equipe de desenvolvimento.
