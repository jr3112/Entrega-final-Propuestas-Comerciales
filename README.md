# Ecosistema de Automatización IA — Propuestas Comerciales Personalizadas

Sistema de automatización de extremo a extremo que genera propuestas comerciales personalizadas para clientes, usando inteligencia artificial, con un punto de aprobación humana antes de cada envío.

**Entrega final — Curso de Automatización con IA**

---

## 🧩 Stack utilizado

| Categoría | Herramienta |
|---|---|
| Orquestador | [Make](https://www.make.com) |
| Base de datos / memoria | [Airtable](https://www.airtable.com) |
| Procesamiento IA | OpenAI (ChatGPT — modelo `gpt-4o`) |
| Canal de salida | Gmail (+ Slack para aprobación humana) |

---

## 🔄 Cómo funciona el flujo

1. **Trigger:** se crea una nueva `Solicitud` en Airtable con estado `Pendiente`.
2. **Validación:** el sistema verifica que los datos del cliente estén completos.
   - Si faltan datos → se registra el error y el flujo se detiene ahí.
3. **Generación con IA:** OpenAI redacta una propuesta comercial personalizada, usando solo los datos necesarios del cliente y del servicio solicitado.
   - Si la IA falla → el error se registra automáticamente, sin romper el resto del sistema.
4. **Aprobación humana (HITL):** se envía una notificación a Slack/Email con la propuesta generada. El sistema espera la aprobación antes de continuar.
5. **Envío final:** una vez aprobada, la propuesta se envía al cliente por Gmail y se actualiza el estado en Airtable.


---

## 📁 Estructura del repositorio

```
/docs
  diagrama-arquitectura.pdf       ← Mapa visual completo del flujo
  manual-operativo-datos.pdf      ← Esquema de las tablas de Airtable + JSON de integración
  matriz-costos.pdf               ← Justificación de qué modelo de IA se usa por tarea
  seguridad-resiliencia.pdf       ← Minimización de datos, manejo de errores y HITL
/blueprint
  propuestas-flow-principal.blueprint  ← Escenario 1: trigger, validación, IA, error handler, aprobación
  propuestas-flow-aprobacion.blueprint ← Escenario 2: detecta aprobación, envía email final, cierra el ciclo
/screenshots
  01-trigger.png
  02-modulo-ia.png
  03-hitl.png
  04-envio-final.png
README.md
```

---

## 🔗 Enlaces del proyecto

- **Base de datos (solo lectura):** (https://airtable.com/invite/l?inviteId=invwY3FMpfxV8GIgm&inviteToken=d04258dd38657830364ccfe1f1e06f0dd2fac675f8b06ef627cef21f734e77fd&utm_medium=email&utm_source=product_team&utm_content=transactional-alerts)
- **Dashboard de control (KPIs y tasa de errores):** (https://airtable.com/appDmqPjgNSuIIEde/shrat8Jm0OatzNk2y)
- **Video demo (3 min):** (https://youtu.be/0eIby_jvkqI)

---

## 🗄️ Base de datos (Airtable)

| Tabla | Función |
|---|---|
| `Clientes` | Datos de contacto e industria de cada cliente |
| `Catálogo_Servicios` | Servicios ofrecidos, descripción y precio base |
| `Solicitudes` | Tabla central: vincula cliente + servicio, guarda el estado del proceso y la propuesta generada |
| `Log_Errores` | Registro de fallos (datos incompletos o errores de la IA) |

Estados posibles de una `Solicitud`: `Pendiente` → `Procesado por IA` → `Esperando Aprobación` → `Aprobado` / `Rechazado` → `Enviado` / `Error`.

---

## 🛡️ Seguridad y resiliencia (resumen)

- Solo se comparte con la IA la información estrictamente necesaria para redactar la propuesta.
- Todo fallo (datos incompletos o error de la IA) queda registrado en `Log_Errores`, sin detener el resto del sistema.
- Ninguna propuesta se envía al cliente sin que una persona la revise y apruebe primero.


---

## 💰 Optimización de costos (resumen)

| Tarea | Modelo | Motivo |
|---|---|---|
| Redacción de propuestas | `gpt-4o` | Mejor calidad de texto para contenido personalizado |
| Clasificación simple de clientes | `gpt-4o-mini` | Tarea simple, menor costo |
| Reprocesamiento masivo de solicitudes históricas | Batch API | Sin urgencia de tiempo real, ahorro ~50% |


---

## ✅ Pruebas realizadas

El flujo fue ejecutado al menos 5 veces, incluyendo un "camino infeliz" con datos incompletos, para verificar que los filtros y las rutas de error funcionan correctamente.

---

## 👤 Autor

Juan Rumas — Entrega final del curso de Automatización con IA.
