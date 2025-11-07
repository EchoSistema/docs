# API de Inteligencia Artificial - EchoIntel

Este documento proporciona una visión completa de todos los endpoints de inteligencia artificial disponibles a través de la integración con la plataforma EchoIntel.

## Visión General

La API de Inteligencia Artificial proporciona **41 endpoints** organizados en **7 categorías principales**, ofreciendo soluciones de machine learning, análisis predictivo, optimización y procesamiento de lenguaje natural para diversos casos de uso empresariales.

### URL Base

```
https://your-domain.com/api/v1/ai/echointel
```

## Autenticación

Todos los endpoints requieren autenticación vía **Bearer token** (Sanctum) y pueden requerir headers adicionales:

| Header             | Requerido   | Descripción |
| ------------------ | ----------- | ----------- |
| Authorization      | Sí          | `Bearer {token}` |
| X-Customer-Api-Id  | Condicional | UUID del tenant (v4). |
| X-Secret           | Condicional | Secret de 64 caracteres. Debe rotarse cada 90 días. |
| Accept-Language    | No          | Idioma (`en`, `es`, `pt`). Por defecto: `en`. |
| Content-Type       | Sí          | `application/json` |

## Categorías de Endpoints

Ver documentación completa en inglés (EN) o portugués (PT) para lista detallada de los 41 endpoints organizados en 7 categorías:
- Customer Intelligence (14 endpoints)
- Propensity (3 endpoints)
- Recommendations (6 endpoints)
- Forecast (6 endpoints)
- Inventory (6 endpoints)
- Risk (5 endpoints)
- Analytics (5 endpoints)

## Referencias

- **Documentación EchoIntel:** https://github.com/EchoSistema/abintel-documentation
- **Controller:** `src/Domain/ArtificialIntelligence/Http/Controllers/EchoIntelProxyController.php`
- **Rutas:** `src/Domain/ArtificialIntelligence/routes/api.php`

**Última actualización:** 2025-01-07
