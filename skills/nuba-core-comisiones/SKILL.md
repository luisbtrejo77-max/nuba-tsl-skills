---
name: "nuba-core-comisiones"
description: "Seguimiento y registro de comisiones de Mariana Medina: cobra a proveedores las comisiones Por Cobrar de más de 90 días del portal TSL y resuelve los Registro de Comisiones de finanzas en Core."
---

# Comisiones de Mariana Medina (portal TSL · Outlook · NUBA Core)

Dos flujos:
- **A. Seguimiento a proveedores** de comisiones pendientes.
- **B. Resolver un "Registro de Comisiones"** que manda finanzas (Rodolfo Galindo y/o Elizabeth Gomez).

Herramientas: "el portal" = https://portal.thesuitelife.com.mx/#/comisiones (sesión de Luis); Outlook web (outlook.cloud.microsoft) de Luis; NUBA Core (core.nuba.com).

Datos fijos:
- IATA correcto de NUBA: **86534361** (ojo: en algún correo viejo salió por error 83564631).
- Advisor ID Four Seasons: FS399750.
- Todos los correos a proveedores van con copia a Mariana Medina (mariana.medina@nuba.com).

---

## A. Seguimiento a proveedores (comisiones Por Cobrar > 90 días)

1. **Listar pendientes en el portal.** Sección Comisiones → filtro Status = Por Cobrar. Tomar las líneas cuya **Salida** sea de hace más de 90 días. Abrir "Detalle" de cada una (sin guardar) para obtener No. reserva, PNR, costo, comisión, moneda y notas. Agrupar por proveedor.

2. **Antes de escribir, descartar las que no hay que cobrar:**
   - Revisar las listas de finanzas "Comisiones No Identificadas" (PDF de Rodolfo: ONYX, TACS, PAYMODE) y el último "Registro de Comisiones": si la comisión ya aparece pagada, no se cobra (ir al flujo B).
   - Si el hotel cobró la estancia **neta de comisión** (prepago neto), no hay nada que cobrar: avisar a Luis para corregirla en el portal.
   - Si en el historial el proveedor ya confirmó el pago, pedir comprobante en vez de cobrar.
   - **Rolzo: nunca se le envían correos de seguimiento de comisiones.**

3. **Buscar historial y contacto** en Outlook (las carpetas de cada cliente están bajo *Archivo › Mariana Medina › <cliente> › <viaje>*). Buscar por hotel, cliente y número de confirmación desde la Bandeja de entrada (búsqueda en todas las carpetas). Si no hay historial, usar *Contactos* del portal o la web del proveedor.

4. **Redactar y confirmar con Luis.** Mostrar a Luis la lista (proveedor, cliente, conf., monto, destinatarios, texto) y **esperar su OK antes de enviar**.
   - Idioma del proveedor (inglés por defecto; español si el hilo es en español).
   - Nombres de personas con mayúscula inicial, nunca en MAYÚSCULAS.
   - Incluir: huésped, fechas, confirmación/PNR, monto de comisión, IATA 86534361, y pedir estatus / fecha y forma de pago (o comprobante si dicen que ya pagaron).

5. **Enviar en el hilo existente.** Si ya hay un hilo con ese proveedor sobre la reserva o el pago, **responder en ese mismo hilo** (Responder a todos; verificar que esté Mariana). Si no existe, correo nuevo con asunto **"Proveedor / Cliente / Tema"** (ej. "Stanly Ranch / Ester Sansi / Commission payment follow-up"); los siguientes recordatorios van sobre ese hilo.
   - Tip técnico en Outlook web: insertar el cuerpo con JS en `[contenteditable][aria-label="Cuerpo del mensaje"]` (execCommand insertHTML al inicio) para no escribir por error en CC; agregar CC enfocando el campo CC, tecleando el correo y Enter para resolver.

6. **Archivar el enviado.** Mover cada correo de *Elementos enviados* a la carpeta del cliente/viaje (clic derecho › Mover › buscar el nombre del viaje, ej. "26-08, Rhine"). Si hay dos carpetas con el mismo nombre, elegir la del cliente correcto (la ruta aparece en el menú).

7. **Revisar respuestas.**
   - **Si el proveedor confirma que ya pagó:**
     a. Actualizar el registro de la comisión en **NUBA Core** (expediente del viaje), con el monto en USD.
     b. **Avisar a Comisiones:** reenviar el correo del proveedor a **comisiones@nuba.com** (CC Mariana), asunto "RV: <asunto original>". Texto modelo: "Hola Eli/Rodolfo, un favor, ¿nos podrían ayudar a confirmar la recepción de esta comisión? Es del expediente: <exp>. Ya está registrada y el hotel nos informó que ya se pagó a través de Onyx." + **screenshot de Core** con la línea de la comisión (servicio, proveedor, importe USD).
     c. En el portal: Status **Pagado**, **monto igual al registrado en Core** y nota ("24nov26, <proveedor> confirma pagada vía ONYX, ref …; registrada en Core exp … por USD …; se pidió a Comisiones confirmar recepción").
   - **Si no se había pagado:** dejar Por Cobrar con nota del monto/fecha prometidos.
   - Las notas se **agregan** a las existentes, no se reemplazan.

---

## B. Resolver un "Registro de Comisiones" de finanzas

Finanzas (Rodolfo Galindo / Elizabeth Gomez) manda una tabla con hotel, cliente, conf., fechas, comisión y fecha de pago, y pide el expediente.

1. **Identificar el viaje** de cada comisión (por fechas, hotel y cliente; usar el cruce de clientes Core vs portal).
   - Si todas son del mismo viaje → guardar el hilo en **\Mariana Medina\<cliente>\<viaje>**.
   - Si son de 2 o más expedientes → guardar en **\Mariana Medina\01 Comisiones y otros\Comisiones expedientes nuevos y varios**.
2. **Actualizar en NUBA Core** la comisión dentro del expediente: pasarla a **USD con el mismo monto que informó Comisiones**. Si no está registrada, darla de alta en el expediente del viaje.
3. **Contestar el correo** a Rodolfo/Elizabeth (CC Mariana) con el **número de expediente** y **anexar screenshot de Core** (pestaña *Ingresos comisiones*) donde se vean las comisiones. Ejemplo: "Hola Rodolfo, te paso el expediente donde estaban registradas, 000855NC2026. Las comisiones estaban registradas en EUR pero ya las puse en USD para que coincidan con tu registro."
4. **En el portal**: marcar como **Pagado**, poner el mes de reporte NUBA (el que indique finanzas, ej. noviembre 2026) y **ajustar el monto y moneda** a lo que informó Comisiones.

---

## Qué entregar a Luis
- Tabla breve: proveedor · cliente · monto · qué se hizo (enviado / no se envía y por qué / ya pagada).
- Lo que queda pendiente de su lado (ej. subir a Core, revisar diferencias de monto, IATA mal enviado).