# PRD-001: PrecioJusto — historial de precios de súper con comparador en góndola

> PrecioJusto es una app web responsive que lee tickets fotografiados y fotos de precios en góndola, guarda el historial de precios por producto y súper con el tipo de cambio del día, y permite consultar en el momento de compra si el precio actual es conveniente o no.

## Contexto y Problema
Compro en varios supermercados y no tengo registro de qué compré, a qué precio ni dónde. Cuando estoy parado frente a una góndola no sé si el precio que veo es caro o barato respecto a lo que pagué antes. Tampoco sé qué productos consumo más ni cómo evolucionan los precios en el tiempo. La inflación hace que comparar precios históricos en pesos sea impreciso, por lo que necesito normalizarlos. Hoy no tengo ninguna herramienta que me ayude con esto: los tickets quedan en el bolsillo y los tiro, y en la góndola no tengo forma de consultar rápido si el precio es conveniente.

Personas:
- Usuario único: persona que compra en varios súpers, usa el celular en la góndola y quiere saber si el precio que ve es conveniente antes de poner el producto en el carrito.

## Objetivos
Que el usuario pueda fotografiar un ticket o el precio en góndola y que la app lo lea automáticamente, acumule el historial de precios por producto y súper normalizado por tipo de cambio, y le diga en el momento de compra si el precio actual está por encima o por debajo de lo que pagó antes.

## Requerimientos Funcionales
- RF-01: El sistema debe permitir fotografiar un ticket y extraer automáticamente los productos, precios, fecha y nombre del súper mediante OCR.
- RF-02: El sistema debe permitir al usuario corregir los productos y precios extraídos antes de confirmar el guardado.
- RF-03: El sistema debe permitir cargar productos y precios manualmente cuando el OCR no pueda leer el ticket.
- RF-04: El sistema debe almacenar cada compra con fecha, súper, lista de productos y precios.
- RF-05: El sistema debe mostrar el historial de precios de un producto.
- RF-06: El sistema debe indicar si el precio obtenido en góndola (por foto o ingreso manual) es conveniente respecto al promedio histórico del producto en ese súper.
- RF-07: El sistema debe mostrar métricas de consumo: productos más comprados, gasto total por mes y gasto por supermercado.
- RF-08: El sistema debe funcionar de forma responsive para su uso desde el celular.
- RF-09: El sistema debe permitir fotografiar el precio en góndola y extraer automáticamente el nombre del producto y precio para consultar el historial al instante.
- RF-10: El sistema debe permitir ingresar el tipo de cambio del dólar al momento de cada compra, almacenar el equivalente en dólares, y mostrar los precios históricos convertidos a pesos al tipo de cambio actual para que sean comparables en el tiempo.

## Requerimientos No Funcionales
- RNF-01: El OCR debe extraer los productos con una precisión ≥ 85% sobre tickets legibles.
- RNF-02: El resultado del OCR debe estar disponible en < 5 s (p95) tras subir la foto.
- RNF-03: La consulta de precio en góndola debe responder en < 2 s (p95).

## Criterios de Aceptación
- AC-01 (RF-01): Dado un ticket fotografiado con 10 productos legibles, cuando se procesa, entonces la app muestra al menos 9 productos extraídos correctamente.
- AC-02 (RF-01): Dado un ticket con imagen borrosa o muy arrugada, cuando se procesa, entonces la app informa que no pudo leerlo en lugar de mostrar datos incorrectos.
- AC-03 (RF-02): Dado un producto extraído con precio incorrecto, cuando el usuario lo edita y confirma, entonces el precio corregido queda guardado y el original descartado.
- AC-04 (RF-03): Dado un ticket ilegible, cuando el OCR no puede procesarlo, entonces la app informa el error y habilita la carga manual.
- AC-05 (RF-04): Dado un ticket procesado con 5 productos confirmados, cuando el usuario guarda, entonces la compra queda registrada con fecha, nombre del súper y los 5 productos con sus precios.
- AC-06 (RF-05): Dado un producto comprado en 3 súpers distintos, cuando el usuario lo consulta, entonces ve el historial de precios separado por súper.
- AC-07 (RF-06): Dado un producto con umbral configurado en 10% y un precio actual 15% mayor al promedio histórico, cuando el usuario consulta en góndola, entonces la app lo marca como "por encima del promedio".
- AC-08 (RF-07): Dado un historial de 3 meses, cuando el usuario abre las métricas, entonces ve los 5 productos más comprados y el gasto total por mes.
- AC-09 (RF-07): Dado un historial de 3 meses con compras en 2 súpers distintos, cuando el usuario abre las métricas, entonces ve el gasto total separado por supermercado y por mes.
- AC-10 (RF-08): Dado un usuario accediendo desde un celular con pantalla de 390px de ancho, cuando abre la app, entonces todos los elementos son visibles y operables sin scroll horizontal.
- AC-11 (RF-09): Dada una foto del precio en góndola con producto y precio legibles, cuando se procesa, entonces la app muestra el nombre del producto, el precio extraído y el historial de precios anteriores de ese producto.
- AC-12 (RF-10): Dado un producto comprado a $1.000 con tipo de cambio $1.000/USD (= U$S 1) y un tipo de cambio actual de $1.500/USD, cuando el usuario consulta el historial, entonces el precio histórico se muestra como $1.500 (convertido a pesos de hoy).

## Fuera de Alcance
- Integración con apps de supermercados o bases de precios online.
- Compartir historial con otras personas.
- Presupuesto o planificación de compras.
- Autenticación y multi-usuario.
- Comparación de precios entre súpers en tiempo real.
- Paginación del historial (en v1 se muestran los últimos 100 registros).
- Validación de formato de precio y tipo de cambio (queda para v2).

## Riesgos y Dependencias
- Riesgo: el OCR falla con tickets arrugados, mal iluminados o con fuente muy pequeña → mitigación: RF-03 permite carga manual como alternativa siempre disponible.
- Riesgo: distintos súpers usan nombres distintos para el mismo producto (ej. "leche entera 1L" vs "leche SL 1000cc") → mitigación: el usuario puede renombrar productos manualmente para unificarlos en v1.
- Dependencia: API de OCR o modelo de visión para lectura de tickets y fotos de góndola.
- Dependencia: almacenamiento local o base de datos para el historial de precios.
