# Briefings: el archivo, y por qué importa

Cada corrida guarda su brief acá, con la fecha (`2026-08-12.md`). No es para tener prolijidad de archivo. Es para poder responder una pregunta que casi nadie se hace sobre su propio agente:

> ¿Esto de verdad me está sirviendo, o solo me llega puntual todas las mañanas?

Un agente que te manda algo lindo cada día es fácil. Un agente cuyo criterio mejora con el tiempo necesita que alguien le diga cuándo acertó y cuándo no. Ese alguien sos vos, y esto es donde lo anotás.

## El loop

1. **Llega el brief.** Se guarda solo en esta carpeta.
2. **Lo leés y lo calificás.** Treinta segundos, al final del archivo:

```markdown
## Veredicto
- Utilidad: útil / interesante / ruido
- Precisión: verifiqué [X] → correcto / exagerado / falso
- Qué le faltó:
```

3. **Cada tanto, mirás el conjunto.** Diez o veinte briefs anotados te dicen cosas que un brief suelto no puede:
   - ¿Cuántas veces por semana trae algo genuinamente útil? Si es una de diez, el problema no es el modelo, es tu `HERMES.md`: le estás pidiendo que mire donde no pasa nada.
   - ¿En qué se equivoca sistemáticamente? Si siempre infla fechas o adorna detalles, eso es una regla nueva para los prompts, no un accidente.
   - ¿Se repite la misma tensión cinco días seguidos? O tenés un problema real sin resolver, o el agente se quedó pegado.

4. **Cada error que encontrás se convierte en una regla**, no en un enojo. El prompt del espía tiene hoy una regla que dice que "verificado" exige fecha textual y cita: esa regla existe porque un brief afirmó que algo había salido el 8 de julio cuando era de junio.

## Esto es un eval set

Sin nombre técnico, acabás de armar lo que en producto se llama un conjunto de evaluación: casos reales, con la respuesta esperada anotada por un humano que sabe juzgar. Cuando cambies de modelo o toques un prompt, corré tres o cuatro casos viejos y compará. Es la única forma de saber si mejoraste o si simplemente te gustó más el output de hoy.

La disciplina completa cabe en una línea: **un fallo real → una regla nueva → un caso guardado que la vigila.**

## Nota sobre privacidad

Los briefs traen contexto de tu producto, tus competidores y tus clientes. Por eso el `.gitignore` excluye los archivos con fecha: el mecanismo se comparte, tus datos no. Si querés versionarlos, sacá esa línea a conciencia.
