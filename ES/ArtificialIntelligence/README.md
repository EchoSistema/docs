# API de Inteligencia Artificial - EchoIntel

Este documento proporciona una visión completa de todos los endpoints de inteligencia artificial disponibles a través de la integración con la plataforma EchoIntel.

## 📚 Documentación Completa de Endpoints

La documentación detallada de cada endpoint está disponible en tres idiomas, con información completa sobre parámetros, respuestas, algoritmos y flujos de trabajo:

### Español (ES)
📄 **[Documentación Completa en Español](Endpoints/EchoIntel/)** - 46 endpoints documentados

### English (EN)
📄 **[Complete English Documentation](../EN/ArtificialIntelligence/Endpoints/EchoIntel/)** - 46 endpoints documented

### Português (PT)
📄 **[Documentação Completa em Português](../PT/ArtificialIntelligence/Endpoints/EchoIntel/)** - 46 endpoints documentados

Cada documento de endpoint incluye:
- ✅ Autenticación y headers necesarios
- ✅ Parámetros completos (tipos, requisitos, descripciones)
- ✅ Ejemplos de solicitud (curl, JavaScript, PHP)
- ✅ Estructura de respuesta detallada
- ✅ Códigos de estado HTTP
- ✅ Manejo de errores
- ✅ **Cómo se Calcula** - Explicaciones de algoritmos IA/ML
- ✅ **Flujo de Trabajo Típico** - Guía práctica de uso (endpoints principales)
- ✅ Enlaces a endpoints relacionados
- ✅ Referencias al controlador

---

## Visión General

La API de Inteligencia Artificial proporciona **41 endpoints** organizados en **7 categorías principales**, ofreciendo soluciones de machine learning, análisis predictivo, optimización y procesamiento de lenguaje natural para diversos casos de uso empresariales.

### URL Base

```
https://echosistema.online/api/v1/ai/echointel
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

La API ofrece **46 endpoints** organizados en **7 categorías principales**:

- **Customer Intelligence** (14 endpoints) - Análisis comportamental y segmentación de clientes
- **Propensity** (3 endpoints) - Modelos de propensión de compra y respuesta
- **Recommendations** (6 endpoints) - Sistemas de recomendación personalizados
- **Forecast** (6 endpoints) - Previsiones de demanda, ingresos y costos
- **Inventory** (6 endpoints) - Optimización de inventario y análisis NLP
- **Risk** (5 endpoints) - Evaluación de riesgo crediticio y detección de anomalías
- **Analytics** (5 endpoints) - Análisis de sentimiento y trayectorias de clientes

Para documentación detallada de cada endpoint, consulte los archivos individuales en [Endpoints/EchoIntel/](Endpoints/EchoIntel/).

## Referencias

- **Controller:** `src/Domain/ArtificialIntelligence/Http/Controllers/EchoIntelProxyController.php`
- **Rutas:** `src/Domain/ArtificialIntelligence/routes/api.php`
- **Logs:** `storage/logs/ia.log`

**Última actualización:** 2025-01-09
