# Skills NUBA / The Suite Life

Skills de Claude para el flujo de trabajo de NUBA y The Suite Life (TSL): cotizaciones, documentos para cliente, ofertas, anuncios y registro en NUBA Core.

Última actualización: 23 nov 2026.

## Estructura

```
skills/
  <nombre-de-la-skill>/
    SKILL.md
```

Cada carpeta es una skill independiente. El archivo `SKILL.md` lleva el nombre y la descripción en el encabezado y las instrucciones debajo.

## Skills incluidas

### NUBA Core

| Skill | Para qué sirve |
|---|---|
| `nuba-core-expediente` | Abrir un expediente en NUBA Core a nombre de Mariana Medina y registrar servicios de proveedores con su pago y comisión (prepago neto, prepago comisionable o pago directo cliente). |
| `nuba-core-cobro` | Crear un cobro en NUBA Core asociado a un expediente; Core manda la solicitud de cobro al email capturado. |

### Cotizar (resultados en el chat)

| Skill | Para qué sirve |
|---|---|
| `tsl-cotizar-hoteles-sabre` | Cotizar y comparar hoteles y vuelos en Sabre (tarifas NEM/API), Expedia y sitios directos; incluye comandos Sabre. |
| `tsl-cotizar-traslados-rolzo` | Cotizar traslados privados y chofer por horas con el conector Rolzo. |
| `tsl-buscar-ofertas-hoteles` | Buscar noches gratis y promociones en Virtuoso y Four Seasons Preferred Partner. |

### Documentos para cliente (formato nuevo, nov 2026)

| Skill | Para qué sirve |
|---|---|
| `tsl-cotizacion-completa` | Cotización Completa: portada, índice, resumen con mapa, vuelos, hoteles, itinerario, hoja por hotel, cotización, notas y cierre. |
| `tsl-cotizacion-rapida` | Cotización Rápida de 2 hojas: vuelos, hoteles y servicios adicionales con total. |
| `tsl-cotizacion-vuelos` | Comparativa de opciones de vuelo (2 hojas). |
| `tsl-cotizacion-hoteles` | Cotización solo de hoteles, una o varias ciudades. |
| `tsl-entrega-final-formato-nuevo` | Entrega Final: vuelos por pasajero, hoteles, itinerario, confirmaciones y cierre. |
| `tsl-guia-viaje-formato-nuevo` | Guía de Viaje: clima, qué empacar, outfits, maletas, propinas y esenciales. |

### Marketing

| Skill | Para qué sirve |
|---|---|
| `tsl-ads-instagram` | Anuncios de Instagram con la marca TSL: carrusel, anuncio individual, Story y series de ofertas. |

## Cómo instalar una skill en Claude

1. Comprime la carpeta de la skill (por ejemplo `skills/tsl-cotizacion-rapida/`) en un `.zip`.
2. En claude.ai ve a **Configuración → Capacidades → Skills** y sube el `.zip`.

## Cómo actualizar este repositorio

Cuando se cambie una skill en Claude, reemplaza su `SKILL.md` en la carpeta correspondiente y haz commit con una nota de qué cambió.

## Nota

Las skills contienen datos de contacto de The Suite Life y nombres de proveedores. Mantén este repositorio **privado**.
