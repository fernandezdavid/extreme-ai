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

- [ ] Cuenta en **Nous Portal** (el cerebro del agente): https://hermes-agent.nousresearch.com (ver "¿Cuánto cuesta?" más abajo antes de elegir plan)
- [ ] Un **bot de Telegram**: en Telegram hablá con **@BotFather**, mandá `/newbot`, seguí los pasos, guardá el token que te da.
- [ ] Tu **user ID de Telegram**: hablá con **@userinfobot**, te da un número. Guardalo.
- [ ] Definí **2-3 competidores** que quieras vigilar (con el link de su changelog o blog).
- [ ] Definí **una fuente de voz del cliente** pública (reviews de una app en App Store/Play, o un subreddit).

## ¿Cuánto cuesta? (leelo antes de elegir plan)

La pieza crítica no es el modelo, son **las herramientas para navegar**. Sin web search y browser, el espía no puede abrir las páginas de tus competidores: contesta de memoria, inventa fechas, y encima te dice que las "verificó". Nos pasó armando esto.

| Camino | Costo | Qué obtenés |
|---|---|---|
| **Nous Portal Plus** (recomendado) | ~$20/mes, con créditos incluidos | Modelo + Tool Gateway (web search, browser en la nube). Funciona out of the box. |
| **Nous Portal Free** | $0 | Solo inferencia con el catálogo de modelos gratuitos. **Sin Tool Gateway**: el agente queda ciego salvo que le des herramientas por otro lado (ver abajo). |
| **Free + browser propio** | $0, más trabajo | Portal Free (o cualquier proveedor que ya pagues) para el cerebro, y un navegador local para las manos. |

**Si vas por el camino gratis**, tenés dos formas de darle ojos al agente:
1. **Navegador local:** Hermes puede manejar un Chrome local por CDP. Corré `hermes doctor` para ver qué te falta y seguí la guía de tools de la doc: https://hermes-agent.nousresearch.com/docs/user-guide/features/tools
2. **Un MCP de búsqueda o fetch:** conectás un servidor MCP que le dé búsqueda web y lectura de páginas. Doc: https://hermes-agent.nousresearch.com/docs/user-guide/features/mcp

Y si el agente igual queda sin herramientas, el repo sigue sirviendo: apuntá los exploradores a fuentes que puedas pegarle a mano (un export de reviews, changelogs copiados). Pierde autonomía, pero la parte más valiosa, el criterio codificado en `HERMES.md` y la síntesis, funciona igual.

## Setup (una sola vez, ~30 min)

Hay dos formas: con la **app de escritorio (Hermes Desktop)**, recomendada si no vivís en la terminal, o por **CLI**. Las dos comparten la misma config en `~/.hermes/`, así que podés mezclarlas.

### Opción A — Hermes Desktop (GUI, recomendada)

1. **Instalá o abrí Hermes Desktop** (macOS, Windows o Linux): https://hermes-agent.nousresearch.com
2. **Conectá el cerebro:** Settings → Model → login con Nous Portal. Si ya tenías Hermes apuntando a otro proveedor, corré `hermes setup --portal` en la terminal para cambiarlo.
3. **Personalidad:** copiá `SOUL.md` a `~/.hermes/SOUL.md`. **Verificá que se copió**, porque Hermes crea uno vacío por default y es fácil no darse cuenta:
   ```bash
   wc -c ~/.hermes/SOUL.md    # tiene que dar ~1100, no 1
   ```
4. **Abrí el Project:** Projects → New/Open → seleccioná esta carpeta. Así el agente lee `HERMES.md` y `prompts/` solo. Completá los `[corchetes]` del `HERMES.md`. (Un chat suelto no toma el contexto: tiene que ser dentro del Project.)
5. **Conectá Telegram:** Settings → Messaging / Integrations → Telegram, cargá el token y tu user ID. Si no aparece en Settings, hacelo con el CLI que ya trae Desktop:
   ```bash
   hermes gateway setup
   hermes gateway
   ```
   Probalo mandándole "ping" al bot desde Telegram.
6. **Agendá el trabajo:** Automations → nueva automatización en lenguaje natural, por ejemplo: *"Cada mañana a las 8, corré el briefing usando los archivos del proyecto (espía, voz del cliente y síntesis) y mandámelo por Telegram."* Usá "run now" para probar que llega. Para el primer test, usá el prompt auto-diagnóstico de `cron-job.md`, que te confirma si el agente encontró el contexto.

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
```bash
cp SOUL.md ~/.hermes/SOUL.md
wc -c ~/.hermes/SOUL.md    # verificá: ~1100, no 1 (Hermes crea uno vacío por default)
```
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

## Problemas comunes (encontrados armando esto)

**"The model provider failed after retries" en Telegram.**
El canal anda, falla el cerebro. Mirá el log real:
```bash
tail -40 ~/.hermes/logs/errors.log
```
La causa más común es de facturación o credenciales del proveedor, no un bug. Si el log menciona límites o cuota de tu suscripción, cambiá de proveedor con `hermes setup --portal`.

**El agente inventa datos o fechas y dice que los "verificó".**
Casi siempre significa que **no tiene herramientas para navegar**, así que responde de memoria. Buscá esto en el log:
```bash
grep "check_browser_requirements\|browser_cdp" ~/.hermes/logs/errors.log | tail -3
```
Si aparece `returned False`, el agente está ciego. Necesitás web search y browser habilitados (en Nous Portal, eso es el Tool Gateway, que es de plan pago).

**La automatización se creó, pero apunta al lugar equivocado.**
Es el error más común de todos. Cuando le pedís la automatización en lenguaje natural, el agente adivina el directorio de trabajo, y suele elegir tu carpeta de usuario en vez del repo. Fijate qué te dice al crearla ("Directorio configurado: ..."), y si está mal, corregilo ahí mismo por chat:
```
La ruta del proyecto es /ruta/completa/a/extreme-ai
Ajustá la automation para que use ese directorio.
```
Verificalo también desde afuera:
```bash
cat ~/.hermes/cron/jobs.json    # buscá el directorio del job
```
**Evitá rutas con espacios.** Si tu repo vive en algo como `/Users/vos/Mis Proyectos/extreme-ai`, mové o cloná el repo a una ruta sin espacios (`~/extreme-ai`). Ahorra una clase entera de errores raros.

**Hermes usa perfiles, y es fácil configurar uno y correr en otro.**
Si tenés más de un perfil (`ls ~/.hermes/profiles/`), asegurate de que el modelo, las herramientas, el `SOUL.md` y la automatización estén **todos en el mismo**. El síntoma clásico: configuraste todo, pero el bot responde como si nada estuviera seteado. Para comparar:
```bash
grep -A3 "^model:" ~/.hermes/config.yaml                      # perfil default
grep -A3 "^model:" ~/.hermes/profiles/TU_PERFIL/config.yaml   # otro perfil
```

**El briefing llega pero suena genérico.**
Dos causas posibles, en orden:
1. El `SOUL.md` quedó vacío: `wc -c ~/.hermes/SOUL.md` (si da 1, copialo de nuevo).
2. La automatización no está viendo `HERMES.md` ni `prompts/`. Usá el prompt auto-diagnóstico de `cron-job.md`: si el mensaje no arranca con "Contexto OK", no está leyendo los archivos. Solución: apuntá la automatización a la carpeta del proyecto, o pegá el contenido de los tres prompts dentro de la automatización.

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
