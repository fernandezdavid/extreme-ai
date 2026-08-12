# Señal, no ruido

Un agente que trabaja solo cada mañana y te devuelve **una sola cosa que importa**, no diez reportes para leer.

La IA volvió gratis el construir. Por eso lo escaso ya no es construir, es el criterio: saber qué vale la pena. Este agente no produce por vos, te afila el juicio. Es un motor de criterio, no un chatbot.

## Cómo funciona

Un solo trabajo, tres piezas. Dos exploradores y una síntesis (nada de diez features, la disciplina es parte del punto):

1. **El espía de la competencia** (mirada de afuera): qué se movió en tus rivales.
2. **La voz del cliente** (mirada de adentro): qué duele de verdad.
3. **La síntesis** (el acto de criterio): cruza las dos señales y te manda por Telegram *la tensión que importa hoy y por qué*.

Corre solo cada mañana. Vos te despertás con el criterio ya destilado.

## Antes de arrancar (10 min, solo cuentas y claves, nada de código)

- [ ] Cuenta en **Nous Portal** (el cerebro del agente): https://hermes-agent.nousresearch.com
- [ ] Un **bot de Telegram**: en Telegram hablá con **@BotFather**, mandá `/newbot`, seguí los pasos, guardá el token que te da.
- [ ] Tu **user ID de Telegram**: hablá con **@userinfobot**, te da un número. Guardalo.
- [ ] Definí **2-3 competidores** que quieras vigilar (con el link de su changelog o blog).
- [ ] Definí **una fuente de voz del cliente** pública (reviews de una app en App Store/Play, o un subreddit).

## Setup (una sola vez, ~30 min)

Hay dos formas: con la **app de escritorio (Hermes Desktop)**, recomendada si no vivís en la terminal, o por **CLI**. Las dos comparten la misma config en `~/.hermes/`, así que podés mezclarlas.

### Opción A — Hermes Desktop (GUI, recomendada)

1. **Instalá o abrí Hermes Desktop** (macOS, Windows o Linux): https://hermes-agent.nousresearch.com
2. **Conectá el cerebro:** Settings → Model → login con Nous Portal.
3. **Personalidad:** poné `SOUL.md` en `~/.hermes/SOUL.md` (con el file browser del Desktop o desde Finder).
4. **Abrí el Project:** Projects → New/Open → seleccioná esta carpeta. Así el agente lee `HERMES.md` y `prompts/` solo. Completá los `[corchetes]` del `HERMES.md`. (Un chat suelto no toma el contexto: tiene que ser dentro del Project.)
5. **Conectá Telegram:** Settings → Messaging / Integrations → Telegram, cargá el token y tu user ID. (Si no aparece en Settings, se hace con el CLI que trae Desktop: `hermes gateway setup`.)
6. **Agendá el trabajo:** Automations → nueva automatización en lenguaje natural, por ejemplo: *"Cada mañana a las 8, corré el briefing usando los archivos del proyecto (espía, voz del cliente y síntesis) y mandámelo por Telegram."* Usá "run now" para probar que llega.

### Opción B — CLI

**1. Instalar Hermes**
```bash
curl -fsSL https://hermes-agent.nousresearch.com/install.sh | bash
source ~/.zshrc
```

**2. Conectar el cerebro (un solo login)**
```bash
hermes setup --portal
```

**3. Conectar Telegram**
```bash
hermes gateway setup     # elegís Telegram, pegás el token y tu user ID
hermes gateway           # lo prende
```
(O copiás `.env.example` a `~/.hermes/.env` y completás los dos valores.)

**4. Darle personalidad y contexto**
- Copiá `SOUL.md` a `~/.hermes/SOUL.md` (cómo te habla el agente).
- Dejá `HERMES.md` en la carpeta del proyecto (quién sos, tu producto, tus competidores, y qué es señal vs ruido para vos). **Completá los campos entre [corchetes].**

**5. Agendar el trabajo de cada mañana**
```bash
hermes cron create       # pegás el prompt de cron-job.md y le ponés horario (ej: 8am)
hermes cron list         # confirmás que quedó
hermes cron run <job-id> # lo disparás a mano para probar que llega al Telegram
```

Si te llega bien una vez, ya está andando.

## Estructura

```
SOUL.md                        personalidad y estilo del agente
HERMES.md                      el contexto: quién sos, producto, señal vs ruido
cron-job.md                    el prompt que corre cada mañana
.env.example                   plantilla de claves de Telegram
prompts/
  01-espia-competencia.md      el explorador de afuera
  02-voz-del-cliente.md        el explorador de adentro
  03-sintesis.md               el acto de criterio
```

## Las ideas de PM detrás de este agente

Este repo no inventa frameworks: toma prestadas ideas probadas de la mejor gente de producto y las convierte en archivos que un agente puede ejecutar. Cada pieza es una de estas ideas, hecha contexto:

| La idea | De quién | Dónde vive en este repo |
|---|---|---|
| **Build the right thing.** El rol no es producir más, es resolver el problema correcto. | [Marty Cagan (SVPG)](https://www.svpg.com/) | La tesis de todo el repo: el agente no construye por vos, te afila el criterio. |
| **Contratar → onboardear → poner a trabajar.** A una IA la sumás como a un integrante del equipo. | [Tal Raviv, "Build your personal AI copilot" (Lenny's Newsletter)](https://www.lennysnewsletter.com/p/build-your-personal-ai-copilot) | La estructura del setup: `SOUL.md` (lo contratás), `HERMES.md` (lo onboardeás), `cron-job.md` (lo ponés a trabajar). |
| **"El contexto lo cambia todo."** La IA parece genérica porque no le diste contexto. | [Tal Raviv, mismo post](https://www.lennysnewsletter.com/p/build-your-personal-ai-copilot) | `HERMES.md`: quién sos, tu producto, y qué es señal vs ruido para vos. |
| **Continuous discovery.** Escucha continua del mercado y del cliente. | [Teresa Torres](https://www.producttalk.org/) | Los dos exploradores: `prompts/01` (competencia) y `prompts/02` (clientes). |
| **Tareas LNO.** Delegá lo de bajo apalancamiento, quedate con lo de alto. | [Shreyas Doshi](https://x.com/shreyas) | El cron: el agente se queda con el overhead de cada mañana, vos con las decisiones. |
| **Gestionás una IA como a una persona.** Contexto claro, objetivo claro, feedback. | ["How to become a supermanager with AI" (Lenny's)](https://www.lennysnewsletter.com/p/how-to-become-a-supermanager-with-ai) | El loop completo: editás el `SOUL.md`, corregís el contexto, iterás los prompts. |
| **"¿Qué es lo más importante que debería hacer ahora?"** La pregunta de un buen thinking partner. | [Tal Raviv, mismo post](https://www.lennysnewsletter.com/p/build-your-personal-ai-copilot) | `prompts/03-sintesis.md`: la síntesis responde exactamente eso, una sola tensión y por qué. |
| **Taste sobre métricas, anti-bloat.** El criterio propio como filtro de la señal. | [El método Linear](https://linear.app/method) | La lente del `HERMES.md` de ejemplo: "un competidor agregó features" puede ser ruido, no amenaza. |

Y una cita para el espíritu del repo, de Daniel Kahneman: *"No soy un genio. Tversky tampoco. Juntos somos excepcionales."* El agente es el socio. El criterio es tuyo.

## La frontera (opcional, hacia dónde sigue)

El espía lee lo público. La mejor inteligencia suele estar detrás de un pago. El próximo paso es darle un presupuesto al agente para que pague por lo que necesita (vía x402). Ahí el criterio se aplica también a la plata: el agente decide *si* vale la pena pagar. Y el giro para tu producto: mañana tu producto puede cobrarle a los agentes por request.
