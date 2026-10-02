# Ecosistema de Automatización de Boletos con IA + HITL

Proyecto final de IA Automation orientado a automatizar el ingreso, validación inicial, trazabilidad y seguimiento de boletos comerciales, incorporando inteligencia artificial, control humano (HITL) y manejo de errores.

## Video demo

https://drive.google.com/file/d/1UEWM-hrelzsKwYovhVrtIye86gCjWgI4/view?usp=sharing

## Dashboard público

https://airtable.com/app7lr27JhlVJymV5/shrpGXS5vRNTBL7Rn/tbl1jkE4ZY7v9VqIB

## Caso de uso

El ecosistema recibe un nuevo boleto comercial, registra la información en Airtable y utiliza OpenAI para validar si los campos requeridos están completos.

La validación devuelve dos resultados:

- `VALIDO`: el boleto pasa a una instancia de aprobación humana.
- `REVISAR`: el boleto queda identificado para corrección y se genera la notificación correspondiente.

La inteligencia artificial no aprueba ni rechaza comercialmente el boleto. La decisión final permanece bajo intervención humana mediante un circuito Human in the Loop (HITL).

## Stack utilizado

- **Google Forms / Google Sheets:** ingreso y origen de datos.
- **Make:** orquestación de los escenarios.
- **Airtable:** base operativa, trazabilidad, aprobaciones y KPIs.
- **OpenAI - GPT-4.1 mini:** validación de integridad de los datos.
- **Slack:** notificaciones.
- **HITL:** aprobación o rechazo humano.
- **Error Handler + Retry:** tratamiento y registro de fallas.

## Arquitectura

Flujo principal:

`Google Forms → Google Sheets → Make → Airtable → OpenAI → Airtable → Router`

Desde el Router:

- **VALIDO →** aprobación pendiente → HITL → Aprobado/Rechazado → Slack.
- **REVISAR →** actualización del boleto → Slack.
- **ERROR →** registro de ejecución → Error Handler / Retry.

## Escenarios de Make

### Ecosistema IA Boletos

Procesa los nuevos boletos, registra la información, ejecuta la validación mediante IA, conserva la trazabilidad y deriva cada registro según el resultado.

### HITL Aprobación Boletos

Procesa las decisiones humanas sobre los boletos pendientes y actualiza su estado final como `Aprobado` o `Rechazado`, registrando la respuesta y enviando la notificación correspondiente.

## Resiliencia y trazabilidad

Ante una falla en OpenAI, el Error Handler registra el incidente en Airtable con el mensaje de error y los intentos correspondientes, y activa la lógica de Retry configurada.

También se implementaron controles para evitar el reprocesamiento de decisiones HITL ya cerradas.

## Dashboard

El Dashboard Ejecutivo consolida indicadores operativos del ecosistema, entre ellos:

- Total de boletos.
- Total de ejecuciones.
- Aprobados.
- Rechazados.
- Registros a revisar.
- Pendientes de revisión.
- Errores.
- Tasa de error.
- Última ejecución.

El acceso público se realiza mediante una Shared View de Airtable en modo de sólo lectura.

## Contenido del repositorio

```text
proyecto-final-ai-boletos/
├── README.md
├── Entrega_Final_Documentacion.pdf
├── blueprints/
│   ├── FINAL - Ecosistema IA Boletos.blueprint.json
│   └── FINAL - HITL Aprobacion Boletos.blueprint.json
└── screenshots/
    ├── 01_lienzo_flujo_completo.png
    ├── 02_ejecucion_exitosa.png
    ├── 03_camino_error_handler.png
    ├── 04_airtable_esquema.png
    └── 05_dashboard_kpis.png
