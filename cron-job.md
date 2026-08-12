# El trabajo de cada mañana (prompt del cron)

Este es el prompt que corre solo, todos los días. Lo pegás cuando hacés `hermes cron create` (o al crear la Automation en Hermes Desktop) y le ponés horario (ej: 8am).

## Para el PRIMER test: la versión auto-diagnóstica

Usá esta primero. Es igual a la de abajo, pero te confirma si el agente encontró el contexto en vez de dejarte adivinar:

```
Es tu briefing de la mañana.

Primero, confirmá el contexto: leé HERMES.md del proyecto y empezá tu
respuesta con una línea que diga "Contexto OK: [nombre del producto] vs
[competidores que encontraste]". Si no podés leer HERMES.md, decí
"NO ENCONTRÉ EL CONTEXTO" y frená ahí.

Después:
1. Corré el espía de la competencia (prompts/01-espia-competencia.md).
2. Corré la voz del cliente (prompts/02-voz-del-cliente.md).
3. Cruzá las dos y devolveme la síntesis (prompts/03-sintesis.md): UNA
   sola tensión que importa hoy, por qué, y qué miraría. No dos reportes.
   Incluí la línea "Verificá vos" con los links de respaldo.

Guardá el resultado en briefings/AAAA-MM-DD.md (con la fecha de hoy) y
mandámelo por Telegram.
```

Si el mensaje llega con "Contexto OK: ...", está leyendo los archivos de verdad y podés pasar al prompt definitivo. Si no, revisá "Problemas comunes" en el README.

## El prompt definitivo

```
Es tu briefing de la mañana. Tenés el contexto en HERMES.md (quién soy, mi producto, mis competidores, mis fuentes, y qué es señal vs ruido para mí).

Hacé esto en orden:

1. Corré la exploración del espía de la competencia (ver prompts/01-espia-competencia.md): qué se movió en mis competidores.

2. Corré la exploración de la voz del cliente (ver prompts/02-voz-del-cliente.md): qué duele en mis clientes.

3. Cruzá las dos y devolveme la síntesis (ver prompts/03-sintesis.md): UNA sola tensión que importa hoy, por qué, y qué miraría. No me mandes dos reportes. Terminá siempre con la línea "Verificá vos": los links y fechas que respaldan la tensión, para que pueda chequearlo en 30 segundos.

Guardá el brief completo en briefings/AAAA-MM-DD.md (con la fecha de hoy) y mandámelo por Telegram. Si hoy no hay señal real, mandame "Nada que requiera tu atención hoy" y guardalo igual.
```

## Cómo agendarlo

```bash
hermes cron create        # pegás el prompt de arriba, elegís el horario
hermes cron list          # ves el job y su id
hermes cron run <job-id>  # lo disparás a mano para probar
```

## Nota sobre los dos exploradores en paralelo (opcional)

Hermes puede correr subagentes en paralelo (uno para el espía, otro para la voz del cliente) y después cruzarlos. Es más elegante y más rápido. Para la primera versión, correrlos en secuencia como arriba es más simple y más robusto. Si querés el paralelo, decile al agente que use un subagente para cada exploración y luego sintetice los dos resultados.
