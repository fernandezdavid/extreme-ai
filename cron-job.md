# El trabajo de cada mañana (prompt del cron)

Este es el prompt que corre solo, todos los días. Lo pegás cuando hacés `hermes cron create` y le ponés horario (ej: 8am).

## El prompt

```
Es tu briefing de la mañana. Tenés el contexto en HERMES.md (quién soy, mi producto, mis competidores, mis fuentes, y qué es señal vs ruido para mí).

Hacé esto en orden:

1. Corré la exploración del espía de la competencia (ver prompts/01-espia-competencia.md): qué se movió en mis competidores.

2. Corré la exploración de la voz del cliente (ver prompts/02-voz-del-cliente.md): qué duele en mis clientes.

3. Cruzá las dos y devolveme la síntesis (ver prompts/03-sintesis.md): UNA sola tensión que importa hoy, por qué, y qué miraría. No me mandes dos reportes.

Mandame el resultado por Telegram. Si hoy no hay señal real, mandame "Nada que requiera tu atención hoy" y listo.
```

## Cómo agendarlo

```bash
hermes cron create        # pegás el prompt de arriba, elegís el horario
hermes cron list          # ves el job y su id
hermes cron run <job-id>  # lo disparás a mano para probar
```

## Nota sobre los dos exploradores en paralelo (opcional)

Hermes puede correr subagentes en paralelo (uno para el espía, otro para la voz del cliente) y después cruzarlos. Es más elegante y más rápido. Para la primera versión, correrlos en secuencia como arriba es más simple y más robusto. Si querés el paralelo, decile al agente que use un subagente para cada exploración y luego sintetice los dos resultados.
