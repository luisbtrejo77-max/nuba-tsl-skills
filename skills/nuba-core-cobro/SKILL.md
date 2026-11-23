---
name: "nuba-core-cobro"
description: "Crear un cobro en NUBA Core (core.nuba.com) asociado a un expediente de Mariana Medina; Core manda solo la solicitud de cobro al email capturado. Usar cuando Mariana pida registrar un cobro, anticipo o pago de cliente en NUBA Core."
---

# Cobro en NUBA Core

Los expedientes y cobros son a nombre de **Mariana Medina**. En cada caso, pedir a Mariana los datos que falten, agrupados en forma de formulario. Ir describiendo en el chat cada paso que se hace.

**El correo con la solicitud de cobro lo manda Core en automático al guardar el cobro**, al **Email** de los datos de factura. No se reenvía desde Outlook. Por eso el Email debe confirmarse con Mariana **antes de guardar**: guardar = enviar.

## Acceso
- Navegador integrado → https://core.nuba.com. Si pide login, usar el botón "Microsoft" (SSO, con la sesión abierta); nunca escribir usuario/contraseña.
- Si el expediente aún no existe, crearlo primero con la skill `nuba-core-expediente`.
- La vista estrecha oculta columnas: emular viewport 1440x900 mientras se trabaja y volver a "desktop" al terminar.
- Leer opciones de listas con JS (`mat-option` + `getBoundingClientRect`) y hacer clic convirtiendo a la escala de la captura (ancho de captura / 1440). Algunos desplegables solo abren con clic en la flecha del campo.

## Pasos
1. Búsqueda de expedientes → escribir el número en "Expediente" → BUSCAR. Abrir el expediente con el ícono de lápiz al final de la fila (o ir a `https://core.nuba.com/booking/details/<número>`). Si el expediente ya está abierto, no hace falta buscarlo.
2. Verificar jurisdicción: el expediente debe ser de Mariana Medina (Agencia MEX_MARIANA MEDINA / Travel Advisor Mariana Medina) y el ID cliente del cliente correcto (CACMX… en clientes de México). Si no coincide, detenerse y reportar.
3. Pestaña **Cobros** (abajo: Pasajeros · Servicios / Pagos · Cobros · Gastos · Ingresos comisiones · Transferencias): revisar los cobros que ya existen y listarlos en el chat (importe, concepto, estado).
   - Varios cobros del **mismo monto no son un error**: hay clientes que pagan en parcialidades (por ejemplo, un total de 1,000,000 en 5 cobros de 200,000). No detenerse por eso.
   - Solo es un duplicado si el cobro que se va a crear ya existe para la misma parcialidad (mismo concepto, importe y fecha que Mariana pidió una sola vez). Si hay duda, preguntar si es una parcialidad nueva antes de crearlo.
   - Conviene que el Concepto indique la parcialidad (por ejemplo "Anticipo 2 de 5") para distinguirlos.
4. Botón **"+"** a la derecha de las pestañas → **Nuevo cobro** (la otra opción es Reembolso). Abre `/charge-creation/<número>`.
5. Llenar los campos **en este orden**:
   1. **Administrar expediente en:** Nuba Exp de México / Nuba USA INC. Hasta elegirlo no aparecen Destino ni Requiere factura.
   2. **Destino del viaje:** NACIONAL / INTERNACIONAL.
   3. **Requiere factura:** viene en "No". No todos los clientes requieren factura, así que **siempre preguntar** a quien pide el cobro (Mariana o Luis) si el cobro va con factura o sin factura; nunca dejar el "No" por defecto sin confirmarlo. Si es "Sí", exige adjuntar constancia de situación fiscal (≤ 3 meses); si no la tienen, preguntar si se deja en "No" y se factura después (solo en el mes en que se recibe el pago). Cambiarla vacía Cliente.
   4. **Método de pago:** Tarjeta de crédito / Transferencia MXN / Transferencia EUR / Transferencia USD. La divisa se ajusta sola.
   5. **Cliente:** escribir parte del nombre, esperar las opciones y hacer clic físico en la opción (no seleccionarla por JS). Si la lista es larga, escribir más letras hasta que quede una sola opción. Si hay varios registros, usar el que tenga la información más completa (correo, nombre completo); si no hay diferencia clara, preguntar a Mariana. Confirmar que "ID Cliente" coincide con el del expediente. Al elegirlo se llenan solos en Datos de la factura: Nombre y Apellido/Empresa, **Email**, Documento, Dirección y Código Postal.
   6. **Email:** revisar el que se llenó solo y **confirmarlo con Mariana**; ahí llegará la solicitud de cobro. Si ella da otro, reemplazarlo.
   7. **Concepto** (obligatorio).
   8. **Importe:** el campo tiene máscara. Clic, End, Backspace ×15 hasta vaciar; luego teclear dígito por dígito (key "1", "0"…) y salir con Tab. Verificar con JS que diga p. ej. 1,000.00.
   9. **Fecha de vencimiento:** viene con la fecha de hoy; cambiarla solo si Mariana lo indica.
6. Revisar con JS que ningún campo esté inválido y mostrar a Mariana un resumen (importe, método, concepto, con o sin factura, **email destinatario**) antes de **GUARDAR**, porque al guardar sale el correo.
7. Al guardar sale "Cobro guardado correctamente", regresa al expediente y éste pasa a estado **PENDIENTE BC**. En la pestaña Cobros reportar: ID, ID externo, RCC, importe, método, concepto, estado y el email al que se envió.
   - El **ID externo (BO_FV…)** y el **RCC** los asigna Business Central después; al principio salen vacíos. Avisarlo, no es un error.
8. Regresar el viewport a "desktop".

## Iconos del renglón del cobro
- **Reenviar** (ícono de flecha saliendo de un cuadro): reprocesa el cobro hacia Business Central ("El servicio fue reprocesado. Pero aún no ha sido resuelto."). **No** manda correo. No usarlo salvo que Mariana lo pida.
- **ⓘ**: muestra quién creó el cobro y cuándo.
- **Menú ⋮**: Editar, Solicitar factura, Ver notas, Adjuntar comprobante cobro, Cancelar cobro, Corrección importe.