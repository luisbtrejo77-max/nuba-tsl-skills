---
name: "nuba-core-expediente"
description: "Crear un expediente nuevo en NUBA Core a nombre de Mariana Medina y registrar servicios de proveedores con su pago y comisión (prepago neto, prepago comisionable o pago directo cliente). Usar cuando Mariana pida abrir un expediente, registrar un servicio, pago a proveedor o comisión."
---

# Expedientes y servicios en NUBA Core

Todos los expedientes son a nombre de **Mariana Medina**.

## Orden de trabajo
1. Crear o localizar el expediente.
2. Registrar el cobro al cliente con la skill `nuba-core-cobro`.
3. Registrar cada servicio de proveedor con su pago y/o comisión (sección 2). La comisión se captura dentro del mismo servicio, según el **Tipo de pago**.

En cada caso, pedir a Mariana los datos que falten, agrupados en forma de formulario. NUBA Core es producción: si un dato es de prueba, marcarlo como "PRUEBA" o "NO PAGAR" (en Nombre viaje y en Notas) y avisar antes de guardar.

Ir describiendo en el chat cada paso que se hace y lo que se encontró.

## Acceso
- Abrir https://core.nuba.com en el navegador integrado y entrar con el botón **Microsoft** (SSO). Nunca escribir credenciales.
- La vista angosta oculta columnas y desacomoda los clics. Emular 1440x900 con `resize_window` mientras se trabaja y volver a `desktop` al terminar o antes de pedirle a Mariana que haga clic.
- Con 1440x900 el link "Nuevo expediente" del menú puede quedar fuera de vista: ir directo a https://core.nuba.com/booking-creation.
- Las coordenadas de los `ref` quedan viejas después de hacer scroll. Ubicar los campos con JavaScript (`getBoundingClientRect`) y convertir a la escala de la captura (ancho de captura / 1440), o usar una captura nueva.

## 1. Nuevo expediente
Menú Expedientes → **Nuevo expediente** (o la URL de arriba). Son 3 pasos: Cabecera, Pasajeros y Servicios.

**Cabecera**
- **Nombre viaje:** `LUGARDEVIAJE_NOMBREDECLIENTE_MESYAÑO`, por ejemplo CANCUN_GARCIA_JUNIO2025.
- **Agencia:** viene **vacía**. Escribir "MARIANA" y elegir **MEX_MARIANA MEDINA - 5658** de la lista. Al elegirla se llenan solos:
  - Travel Adv/Agente Comercial: **MARIANA MEDINA**
  - Línea de negocio ("¿Cómo se opera el expediente?"): **INDEPENDENT CONTRACTORS**
  - Código 5658, divisa de la agencia USD
  Verificar los tres. Si no aparece la opción de Mariana, detenerse y preguntar.
- **Fechas:** formato DD/MM/AAAA. Al llenar Fecha inicio y salir con Tab, Fecha fin se copia con la misma fecha: hacer clic en Fecha fin, End + Backspace ×12, reescribir y salir con Tab. Verificar con JS (`[formcontrolname=startDate]` / `endDate`).
- **Nº adultos:** viene en 1; debe coincidir con los pasajeros que se capturen.
- **Valor aproximado** (`retailPrice`): máscara de moneda. Clic en el campo y teclear dígito por dígito con `key` ("5", "0", "0", "0"); con `type` se ve el valor pero el campo queda inválido. Debe leerse 5,000.00. No acepta centavos: "6 3 0 1 6 2" queda como 630,162.00, así que se redondea al entero (6,302). Para borrarlo, `cmd+a` no funciona: usar End + Backspace repetido y Delete repetido.
- **Divisa:** hacer clic en el campo, escribir "Dólar" (o "Peso", "Euro") y hacer clic en la opción (Dólar estadounidense, Peso mexicano o Euro). Si solo se escribe sin elegir, se borra.
- **Cliente:** escribir parte del nombre, esperar las opciones y **hacer clic físico** en la opción. Seleccionarla por JS no la amarra y el paso no avanza sin dar error. Confirmar que aparezca "ID Cliente: CACMX…" (CACES = cliente de España; avisar si sale así en un expediente real).
  - Si la lista es larga, las opciones de abajo quedan fuera del panel y el clic cae en otro campo (sale "Campo obligatorio"). Escribir más letras del nombre hasta que quede una sola opción y entonces hacer clic.
  - Leer las opciones con JS (`mat-option` + `getBoundingClientRect`) y hacer clic por coordenadas convertidas a la escala de la captura.
  - Si hay varios clientes con el mismo nombre, usar el registro con la **información más completa** (correo, nombre completo). Si no hay diferencia clara, preguntar a Mariana cuál usar.
  - Para pruebas hay clientes como PRUEBA INT 1 (CACES14378) y PRUEBAS NO CONSIDERAR (CACES15054); ambos son de España.
- **Cliente nuevo:** usar el botón "+". Campos obligatorios: Nombre, Oficina NUBA (Nuba), País e Idioma (ES). Si el sistema avisa de clientes que coinciden, revisar las coincidencias y preguntar a Mariana antes de crear un duplicado.
- **Notas:** campo libre; ahí va "PRUEBA - NO PAGAR" si es prueba.
- Antes de **Siguiente**, revisar con JS que no haya campos `ng-invalid`. Si Siguiente no avanza, revisar primero que el Cliente siga seleccionado, porque al regresar a la Cabecera se borra.

**Pasajeros**
- El titular se agrega solo, con el nombre del cliente. **Basta con 1 pasajero**, el titular. No agregar acompañantes. Dar **Siguiente**.

**Servicios**
- Se puede guardar sin servicios. Dar **Guardar**. Sale "Expediente creado correctamente", el estado queda CONFIRMADO y la URL cambia a `/booking/details/<número>`. Reportar el número de expediente (por ejemplo 009042NC2026).
- El número de expediente **no** es el DK de Sabre: en los PNR de Sabre el customer number es siempre `DKNEM200`.

## 2. Servicio, pago a proveedor y comisión
Con el expediente abierto, ir a la pestaña **Servicios / Pagos** → "+" → **Nuevo servicio** (abre `/new-service/<número>`).

### Elegir el caso (preguntar a Mariana si no está claro)
| Caso | Tipo de pago | Comisión | Nuevo pago |
|---|---|---|---|
| **1. Pago neto** (el monto al proveedor es NETO) | Prepago neto (viene por defecto) | No se registra (el campo queda bloqueado) | Sí |
| **2. Prepago comisionable** (algunos hoteles: se paga y nos dan comisión) | Prepago comisionable | Sí | Sí |
| **3. Solo comisión** (el cliente paga directo al proveedor) | Pago directo cliente | Sí | **No**: dejar "Tipo" del pago en "-" |

### Importes del servicio
- **Tipo servicio:** Hotel, Traslado, Actividades, Paquete DMC, Aéreo, Experiencias Nuba, etc.
- **Proveedor:** escribir parte del nombre (ej. "four seasons maui") y hacer clic en la opción del catálogo (ej. FOUR SEASONS MAUI AT WAILEA). Si hay varias parecidas, confirmar con Mariana.
- **Precio coste:** lleva máscara, se teclea dígito por dígito. En el caso 3 (Pago directo cliente) se llena con el **monto que paga el cliente** directo al proveedor.
- **Divisa coste:** autocompletado con muchas monedas. Escribir "est" o "Dólar estado" y elegir **Dólar estadounidense** con clic. Con flechas puede quedar otra moneda (salió Dirham), así que hay que verificar.
- **Precio venta** e **Impuesto de compra:** opcionales; llenarlos solo si Mariana los da.
- **Tipo de pago:** según la tabla.
- **Comisión** (casos 2 y 3): monto con máscara (ej. 700.00). **Tipo de margen** (Absoluto / %) se puede dejar vacío y la comisión se toma como monto fijo en la divisa del servicio; usar "%" solo si Mariana da la comisión como porcentaje.

### Nuevo pago (casos 1 y 2)
- **Tipo:** Pago, Reembolso o Factura final. Mientras esté en "-", Fecha de vencimiento, Concepto y Notas internas quedan bloqueados.
- **Importe:** con máscara (normalmente igual al precio coste). **Divisa pago** se llena con la del coste; verificar.
- **Fecha de vencimiento:** se elige en el calendario.
- **Concepto:** obligatorio (ej. número de invoice del proveedor).
- **Notas internas:** opcional (ej. "Se anexa invoice").
- **Adjuntar facturas:** es obligatorio. Abre el selector de archivos del sistema, que no se puede manejar desde el navegador integrado. Pedirle a **quien tenga la sesión iniciada** (Mariana, Luis u otra persona del equipo) que haga clic en "Adjuntar facturas" y elija el archivo; se le puede marcar el botón con un recuadro. Si el archivo viene de su computadora, revisarlo antes y avisar si no corresponde al proveedor o al monto.
- No hace falta el botón **Agregar pago**: el pago capturado se guarda con **GUARDAR**.

### Datos del servicio
- Booking reference (confirmación del proveedor), Descripción (ej. Hospedaje), Destino, Nº adultos / niños / bebés, Pasajero titular (texto libre), fechas (vienen del expediente).
- Si es hotel: Hotel y **Tipo de habitación** (Suites / Cuartos…).
- Gastos de cancelación: por defecto "No hay gastos de cancelación".

### Guardar y verificar
- Mostrar a Mariana un resumen (caso, proveedor, coste, comisión, pago, vencimiento) y dar **GUARDAR**.
- Verificar en el expediente:
  - **Servicios / Pagos:** el servicio y, en casos 1 y 2, el pago con su ID, estado "Pendiente pago" y adjuntos.
  - **Ingresos comisiones:** en casos 2 y 3, la comisión (servicio, proveedor, importe).
- Con un cliente de prueba (no real) sale "Lo sentimos, no se ha podido crear la solicitud de pago" y el expediente queda en **PENDIENTE BC**; aun así se guardan servicio, pago y comisión. En un expediente real ese error sí hay que reportárselo a Mariana.
- Avisar que un pago a un proveedor real puede entrar a la cola de Egresos.

## Notas
- Si el panel del navegador se cierra, se pierde lo capturado y no guardado.
- Al terminar, reportar el número de expediente y lo que quedó registrado, y regresar el viewport a `desktop`.