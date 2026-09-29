---
title: "Caso práctico — Planner Comercial (versión corta)"
subtitle: "90 minutos · mismo archivo de datos"
lang: es
---

# Contexto

Supermercados del Istmo (cadena ficticia) tiene 93 tiendas en Costa Rica en tres formatos: Hiper, Súper y Bodega (descuento). Entre el 5 de enero y el 29 de junio de 2026 movió precios y corrió promociones en Gaseosas, Café, Aceites y Detergentes. La Gerencia Comercial quiere saber qué funcionó.

Recibes `caso_rgm_datos.xlsx` con las hojas `Ventas`, `Maestro_Productos`, `Tiendas` y `Diccionario`. Montos en colones (₡). Nadie revisó la extracción.

# Reglas

- 90 minutos, Excel o Google Sheets, sin inteligencia artificial.
- Si una instrucción no alcanza para decidir, escribe tu supuesto y sigue.

# Tareas

1. **Limpieza.** Crea `Ventas_Limpias` sin tocar `Ventas`. En una hoja `Bitácora` anota cada problema, cuántas filas afectó y qué hiciste.
2. **Formatos.** ¿Qué formato de tienda rinde más? Respalda con cifras.
3. **Elasticidad.** Cola Tropical 2L tuvo 25 % de descuento en dos semanas distintas. Calcula la elasticidad con cada una y explica si coinciden.
4. **Promoción.** Detergente Blanco Max estuvo 30 % más barato cuatro semanas. ¿Conviene repetirla? Usa el costo del maestro.
5. **Faltantes.** Nombra tres datos que no están en el archivo y qué decisión cambiaría con cada uno.

# Entregable

Un Excel con `Ventas`, `Ventas_Limpias`, `Bitácora` y `Resumen` (media página: una respuesta con su cifra por tarea).

# Referencia

Elasticidad = % cambio en unidades ÷ % cambio en precio. Tú eliges el periodo de comparación y lo justificas.

Margen bruto = Venta − (Unidades × Costo_Unitario).
