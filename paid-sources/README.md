# Fuentes pagas: lo que compraste, guardado

Cuando el agente se queda sin señal en las fuentes gratuitas, puede proponer comprar acceso a una fuente paga (búsqueda premium, un reporte, una API). Si autorizás el gasto, **el resultado se guarda acá**, con fecha y con lo que costó.

No es prolijidad de archivo: es que **pagaste por eso**. Que un dato se pierda en un chat después de haberlo comprado es tirar plata dos veces, porque a la semana siguiente lo comprás de nuevo.

## La convención

Un archivo por compra: `AAAA-MM-DD-fuente-tema.md`. Adentro, siempre lo mismo:

```markdown
# [Qué compraste]

- Fuente: Exa (búsqueda premium)
- Costo: $0.007 USDC
- Fecha: 2026-08-12
- Consulta: "..."
- Por qué se pagó: las fuentes gratuitas no daban señal fechada del lado del cliente

## Resultado

[el contenido tal como vino]
```

## Cómo se usa

- Los exploradores **miran esta carpeta antes de proponer una compra nueva.** Si ya compraste algo equivalente hace poco, se reutiliza en vez de volver a pagar.
- La síntesis puede citar estos archivos como fuente, igual que cita una página pública. Si un dato del brief salió de acá, el brief lo dice.

## Si el agente no puede pagar por su cuenta

Es un caso normal, no un error: el agente puede tener las manos para buscar pero no una billetera conectada. En ese caso propone la compra, vos la ejecutás por fuera (desde otra herramienta que sí pueda pagar), pegás el resultado acá con el formato de arriba, y le decís al agente que lea el archivo.

El repo es el punto de encuentro entre los dos sistemas: uno decide qué vale la pena comprar, el otro paga, y el archivo los conecta.

## Una request fallida se puede cobrar igual

Probado con plata real: una consulta que el proveedor rechazó con `400` **se cobró de todas formas**. El error del merchant no garantiza que el pago no se ejecute.

Dos consecuencias prácticas:
- **Guardá también los fallos** en esta carpeta, con el error y el costo. Si no, vas a repetir la consulta que ya pagaste y falló.
- **El presupuesto es un tope real, no una formalidad.** Un agente con permiso para gastar puede gastar mal.

## Privacidad

Los contenidos pagos suelen tener licencia de uso, y además revelan qué estás investigando. Por eso el `.gitignore` excluye los archivos con fecha: se comparte la convención, no lo que compraste.
