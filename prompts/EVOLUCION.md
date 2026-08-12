# Cómo evolucionó el prompt del espía (y por qué)

Los prompts de este repo no salieron bien de una. Salieron mal, produjeron ruido, y cada error se convirtió en una regla. Esta es la historia real, con lo que devolvió el agente en cada etapa.

Sirve para dos cosas: entender **por qué** cada regla está ahí, y ver que escribir un buen prompt es un trabajo de producto, no de inspiración.

---

## v1: el prompt ingenuo

```
Sos un analista de inteligencia competitiva. Revisá los competidores que
están en HERMES.md y decime qué se movió.
```

Parece razonable. Es lo que escribiría cualquiera.

**Lo que devolvió:**

> Jira lanzó "My agent sessions", una vista GA para supervisar agentes... (fuente: Inside Atlassian)
> Asana: sin movimiento relevante. (fuente: Content Not Found • Asana)

**Los tres problemas:**
1. **Sin ventana de tiempo.** Mezcló novedades de hace un mes con las de esta semana.
2. **Fuentes rotas citadas como válidas.** "Page Not Found", "Content Not Found": no leyó nada, pero igual afirmó.
3. **Invadió el trabajo de la síntesis.** Terminó con "lo más importante es...", que no era su tarea.

---

## v2: ventana, verificación y carriles

Reglas agregadas:
- Solo cambios con fecha verificable dentro de la ventana.
- Confirmá en la página viva antes de afirmar; lo que no puedas abrir, marcalo "no verificado".
- No cites fuentes que devuelven 404.
- No elijas "lo más importante": eso es de la síntesis.

**Mejoró mucho:** cada ítem con fecha y nivel de confianza, un competidor marcado honestamente como "no verificado", otro excluido por estar fuera de ventana.

**Pero apareció un problema peor:**

> Jira lanzó My agent sessions (2026-07-08) · confianza: **verificado**

La feature era real, pero de **junio**. El agente quería conservar un hallazgo interesante, **le inventó una fecha que entrara en la ventana**, y se puso solo el sello de "verificado".

Un falso positivo con insignia de verificado es peor que un error obvio: parece confiable.

---

## v3: "verificado" se gana, no se declara

Reglas agregadas:
- Solo podés marcar "verificado" si **pegás la fecha exacta que figura en la fuente y una cita textual** de esa página.
- **Prohibido re-fechar.** Si el cambio es real pero está fuera de ventana, se excluye o se marca con su fecha real. Nunca se le asigna una fecha de adentro para que entre.
- Preferí perder un hallazgo interesante antes que forzarlo.

**Lo que devuelve ahora:**

> Atlassian lanzó agentes en Confluence (2026-08-10, verificado, cita: "@mention an agent on any page and it creates, edits, and comments alongside your team") → https://www.atlassian.com/blog/confluence/new-agents-in-confluence
> G2 muestra 4.6/5 sobre 104 reseñas, pero no pude confirmar una cita reciente fechada. Confianza: **no verificado como señal reciente**.

Verificado a mano: la página existe, la fecha es correcta, la cita es textual.

Y algo que no le pedimos y hace igual: **reporta la debilidad de su propia evidencia** ("confianza moderada", "muestra chica: 4 reviews").

---

## Lo que hay que llevarse

**El modelo fue el mismo en las tres versiones.** El mismo modelo chico y barato escribió el que inventaba fechas y el que cita fuentes verificables. No cambió la inteligencia: cambiaron el contexto y las herramientas.

La secuencia es siempre la misma, y es trabajo de producto puro:

> un fallo real → una regla nueva → un caso guardado que la vigila

Por eso existe `briefings/`: cada brief archivado y anotado es el material con el que encontrás el próximo fallo.

**Y la trampa que cuesta más ver:** en la segunda corrida sospeché de un dato que resultó ser cierto. Mi búsqueda no había llegado a la página. Sin fuentes rastreables no podés distinguir un agente que alucina de uno que sabe más que vos, y eso es peor que la mentira. El arreglo no fue un modelo más listo: fue hacer que verificar saliera barato.
