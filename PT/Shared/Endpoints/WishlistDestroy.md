# Shared – Remover Item da Lista de Desejos

## Endpoint

```
DELETE /api/v1/wishlists/{wishlist}
```

## Autenticação

Obrigatória – Bearer {token} com habilidade `backoffice`

## Cabeçalhos

| Cabeçalho        | Tipo   | Obrigatório | Descrição |
| ---------------- | ------ | ----------- | --------- |
| Authorization    | string | Sim         | `Bearer {token}`. |
| X-PUBLIC-KEY     | string | Sim         | Chave pública da plataforma. |
| Accept-Language  | string | Não         | Locale IETF (ex.: `pt-BR`, `en`, `es`). |

## Parâmetros

### Parâmetros de rota

| Parâmetro | Tipo   | Obrigatório | Descrição |
| --------- | ------ | ----------- | --------- |
| wishlist  | string | Sim         | UUID da entrada da lista de desejos a remover |

## Exemplos

### Exemplo de requisição (curl)

```bash
curl -X DELETE \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <chave>" \
  -H "Accept-Language: pt-BR" \
  "https://sandbox.seu-dominio.com/api/v1/wishlists/550e8400-e29b-41d4-a716-446655440000"
```

### Exemplo de resposta

```json
{
  "message": "Excluído com sucesso."
}
```

## Estrutura JSON Explicada

| Campo   | Tipo   | Descrição |
| ------- | ------ | --------- |
| message | string | Mensagem de sucesso confirmando a exclusão |

## Status HTTP

- 200: Sucesso - Item removido da lista de desejos com sucesso
- 401: Não autenticado - Token inválido ou ausente
- 403: Proibido - Usuário não é proprietário desta entrada
- 404: Não encontrado - Entrada da lista de desejos não encontrada
- 500: Erro interno do servidor

## Erros

### 401 Não autenticado
```json
{
  "message": "Não autenticado."
}
```

### 403 Proibido
```json
{
  "message": "Esta ação não está autorizada."
}
```

### 404 Não encontrado
```json
{
  "message": "Nenhum resultado encontrado para o modelo [Domain\\Shared\\Models\\Wishlist]."
}
```

## Notas

- Apenas o proprietário da entrada pode excluí-la
- O endpoint usa UUID para identificação (não ID interno)
- A exclusão é soft delete - o registro é marcado como excluído mas permanece no banco de dados
- Após a exclusão bem-sucedida, o item não aparecerá mais na lista de desejos do usuário
- Se a entrada não existir, um erro 404 é retornado

## Relacionados

- [Listar Lista de Desejos do Usuário](./WishlistIndex.md)
- [Adicionar Item à Lista de Desejos](./WishlistStore.md)

## Changelog

- 2025-10-17: Versão inicial - Remover item da lista de desejos com validação de proprietário
