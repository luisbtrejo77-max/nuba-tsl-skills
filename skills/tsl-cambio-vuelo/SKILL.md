---
name: "tsl-cambio-vuelo"
description: "Crea el PDF de Cambio de Vuelo de The Suite Life: vuelos cancelados con el precio pagado, la protección de la aerolínea y nuevas opciones con precio, en una tabla. Úsala ante cancelaciones o cambios de horario."
---

# TSL · Cambio de Vuelo

PDF de una hoja (carta) para avisar al cliente que una aerolínea canceló o cambió vuelos. En una sola tabla muestra:
1. Los vuelos originales cancelados (en gris, número de vuelo tachado) con el precio pagado por boleto.
2. La protección que da la aerolínea.
3. Una o más opciones nuevas para comprar, con su precio.

Usa el mismo estilo de la Cotización Vuelos de TSL: franja de foto del destino, tarjeta beige con logo, títulos en serif delgada, Karla para el texto, firma de Mariana al pie. Es documento de The Suite Life: lleva la firma de Mariana y no se adapta a NUBA. El ejemplo aprobado es el de Roberto Segovia y Sofía Torre (Seychelles – París, mayo 2027, cancelación de Qatar).

## Materiales (proyecto Nuba, carpeta `claude/plantillas/`)

- `tsl_cambio_vuelo.py`: el generador. No se edita por caso; todo va en el JSON.
- `tsl_cambio_vuelo_ejemplo_segovia.json`: ejemplo completo de datos. Cópialo y cambia los valores.
- `tsl_logo_card.svg` y `tsl_logo_foot.svg`: el logo de The Suite Life en vector (con lema y sin lema).

Bájalos con `project_read` y guárdalos juntos en una carpeta de trabajo. No dependas de la laptop de Mariana: así funciona en cualquier computadora.

## Tipografías

IvyOra no está disponible completa fuera de la carpeta `TSL - Brand Files/Fonts`. La plantilla usa Karla (la de la marca) y Cormorant Garamond como sustituta de IvyOra para títulos y precios grandes. Se bajan de npm:

```
mkdir fonts && cd fonts && npm pack @fontsource/karla @fontsource/cormorant-garamond
for f in *.tgz; do tar xzf $f --one-top-level; done
cp */package/files/karla-latin-{300,400,500,700}-normal.woff2 */package/files/cormorant-garamond-latin-{300,400}-normal.woff2 */package/files/cormorant-garamond-latin-300-italic.woff2 .
```

La carpeta `fonts/` tiene que quedar junto a `tsl_cambio_vuelo.py`.

## Flujo

1. **Datos.** Toma los vuelos del PNR (`*I`/`*IA`), del boleto (`WETR`) y del Price Quote (`*PQ`) que mande Luis. Segmentos `UN`/`HX` = cancelados; `TK` = protección o cambio de horario de la aerolínea; los nuevos salen del `*PQ`. No inventes precios, horarios ni reglas.
   - Precio pagado: el TOTAL en MXN del `WETR`. Si solo hay un boleto a la vista, pregunta si los demás pasajeros pagaron lo mismo antes de multiplicar.
   - Precio nuevo: el TOTAL del `*PQ` por adulto.
   - Clase: la letra de reservación según la cabina de esa aerolínea (D/J/C = Business normalmente). Si dudas, pregunta.
   - Equipaje: `2PC` = 2 maletas; en Etihad, `55K` = hasta 55 kg por persona en dos maletas. Si no sabes leer el código, pregunta; no adivines.
   - Duraciones y escalas: calcúlalas con las zonas horarias de cada aeropuerto y comprueba las cuentas.
2. **Formato de datos.**
   - Fechas `22 may`, horas en 24 h, `+1` en el campo `mas` cuando se llega al día siguiente.
   - Ciudades como `Mahé (SEZ)`, `Abu Dabi (AUH)`, `París (CDG)`.
   - Precios `MX$67,986.00` (o la moneda que pida Luis), en el primer tramo de cada grupo. Los demás tramos dicen `Incluido`. La protección sin costo dice `Sin costo`.
   - El grupo cancelado lleva `"cancelado": true`.
   - Nombres de pasajeros con mayúscula inicial, nunca en mayúsculas como vienen de Sabre.
3. **Notas.** Una nota corta por grupo (3 columnas): cabina, escala, llegada, si es el mismo boleto o uno nuevo, reglas de la tarifa y equipaje. La nota final lleva los totales por todos los pasajeros, la vigencia o "sujeta a disponibilidad y no garantizada hasta la emisión", "Horarios locales; +1 indica llegada al día siguiente" y el crédito de la foto.
4. **Foto.** Del destino al que vuela el cliente, de Unsplash (gratis, no Unsplash+). Busca con WebFetch en `unsplash.com/s/photos/...`, abre la página de la foto para sacar la URL `images.unsplash.com/photo-...` y el fotógrafo, y bájala con curl (`?w=2400&q=85&fm=jpg`). Recórtala a proporción 612:234 de modo que el motivo quede a la derecha (la tarjeta tapa la mitad izquierda) y apágala un poco con PIL: Color 0.72, Contraste 0.92 y mezcla de 8 % con #dfd3c5. Revisa el recorte en imagen antes de seguir.
5. **Generar.** `python3 tsl_cambio_vuelo.py datos.json salida.pdf` (necesita google-chrome headless).
6. **Revisar.** Debe salir de una hoja (`pdfinfo`). Renderízala a PNG y mírala: nada encimado sobre la firma, precios sin chocar con la duración. Compara cifras, vuelos, fechas y horas contra Sabre con `pdftotext -layout`. Si el texto no cabe, recorta palabras de las notas; no achiques la letra.
7. **Entregar.** Manda el PDF con `SendUserFile`, con nombre `Cambio_Vuelo_<Apellido>_<Ruta>_<MesAño>.pdf`. Di qué falta o quedó pendiente (por ejemplo, la vigencia de la tarifa). Nunca se envía al cliente sin que Mariana lo confirme.

## Sabre

Solo consulta. Nunca reserves, aceptes cambios de horario, guardes PQ ni emitas sin confirmación de Mariana en el chat.