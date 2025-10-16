# Ecommerce Endpoints

This directory contains detailed documentation for all Ecommerce domain endpoints.

## Endpoint List

| Endpoint | Method | Description |
| -------- | ------ | ----------- |
| [ProductStockIndex](./ProductStockIndex.md) | GET | ProductStockIndex endpoint |
| [ProductStockShow](./ProductStockShow.md) | GET | ProductStockShow endpoint |
| [ProductResalableIndex](./ProductResalableIndex.md) | GET | ProductResalableIndex endpoint |
| [ProductResale](./ProductResale.md) | POST | ProductResale endpoint |
| [AdminProductIndex](./AdminProductIndex.md) | GET | AdminProductIndex endpoint |
| [AdminProductShow](./AdminProductShow.md) | GET | AdminProductShow endpoint |
| [AdminProductStore](./AdminProductStore.md) | POST | AdminProductStore endpoint |
| [AdminProductUpdate](./AdminProductUpdate.md) | PUT | AdminProductUpdate endpoint |
| [AdminProductShelve](./AdminProductShelve.md) | PATCH | AdminProductShelve endpoint |
| [AdminProductDestroy](./AdminProductDestroy.md) | DELETE | AdminProductDestroy endpoint |

## Common Features

### Autenticación

Requerida: `Authorization: Bearer {token}` and `X-PUBLIC-KEY: {key}`

## Relacionados

- [Ecommerce Dominio](../README.md)
