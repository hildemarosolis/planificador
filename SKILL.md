---
name: planificador
description: >-
  Convierte un trabajo corto o mediano YA decidido (después de un GO del
  validador, o de algo concreto que Claude Code va a ejecutar) en un plan por
  fases: qué se hace en cada una, qué hay que decidir contigo, el producto de la
  fase y cómo sabremos que sirvió — cerrando con el producto final y una
  recomendación honesta (qué ahora, qué gated, qué aparcar). Al final clasifica
  si el trabajo puede ir a un loop desatendido o si tiene decisiones tuyas en
  medio. Úsala SIEMPRE que el usuario diga "dame un plan por fases", "desglosa
  esto en fases", "qué se hace en cada fase y el producto final", "antes de
  construir dame el plan", "arma el plan de esto", o invoque /planificador — tras
  un GO o ante algo concreto por construir. NO es para juzgar si hacerlo (eso es
  validador), ni para proyectos nuevos (eso es /architect), ni para cambios
  grandes cross-repo o migraciones (eso es /architect-brownfield). Es el eslabón
  "planificar" entre el GO y la construcción.
---

# planificador — del GO al plan por fases

## Por qué existe

En el pipeline del usuario —divergir (walle) → juzgar (validador) → **planificar** → construir— faltaba casa para el paso de planificar. Después de un GO, el usuario siempre pide lo mismo a mano: "dame un plan por fases, qué se hace en cada una, y el producto final". Esta skill formaliza ese paso para que salga siempre con la misma forma.

Y hace una cosa más, que es la razón real de existir: **separa el plan del loop.** El error que evita es creer que todo plan por fases se puede "activar en un loop y desentenderse". El trabajo de producto real tiene decisiones tuyas y datos externos metidos en medio (qué opción prefieres, qué plantilla tienes, la aprobación de un tercero). Ese trabajo se **planifica**, no se automatiza. La skill lo dice de frente en vez de venderte una tanda desatendida que se atasca en la primera puerta.

## Cuándo se activa

Slash: `/planificador`.

Frases:
- "dame un plan por fases", "desglosa esto en fases"
- "qué se hace en cada fase y cuál es el producto final"
- "antes de construir, dame el plan", "arma el plan de esto"

Contexto natural: **después de un GO del validador**, o cuando Claude Code propone algo concreto que va a ejecutar y el usuario quiere el mapa antes de arrancar.

**No se activa** —y esto la separa de sus vecinas—:
- "¿debería hacer esto?" / "¿es prudente ahora?" → **validador** (juzgar), no planificar.
- Proyecto nuevo desde cero → **`/architect`**.
- Cambio grande, cross-repo o migración → **`/architect-brownfield`** (blueprint de 20 secciones, parity/cutover, tasks.json). Si el trabajo es de ese tamaño, la skill lo dice y redirige.
- Pedir ejecutar/codear → flujo normal.

## Alcance (léelo antes de planificar)

Esta skill es para trabajo **corto o mediano dentro de un proyecto que ya existe**. Antes de escribir el plan, mide el tamaño y redirige si hace falta:

- Huele a **proyecto nuevo** → para y manda a `/architect`.
- Huele a **cambio grande / cross-repo / migración / contrato entre dos sistemas** → para y manda a `/architect-brownfield`. No hagas un plan corto de algo que necesita un blueprint con parity y cutover.
- El trabajo **aún no está decidido** (no hay GO) → para y manda al `/validador`.

Redirigir a tiempo vale más que entregar el plan equivocado.

## Cómo opera

### 1. Toma el trabajo decidido
Necesitas la cosa concreta que se va a construir (el resultado del GO, o lo que Claude Code propuso). Si no está claro, pídelo en una frase y detente. No planifiques una idea difusa.

### 2. Clasifica: ¿tiene puertas humanas?
Antes de las fases, decide esto, porque cambia el cierre del plan:
- **Con puertas** — hay decisiones tuyas o datos externos en medio (qué opción prefieres, un dato que solo tú ves, la aprobación de un tercero). → Es un **plan-guía**. Las puertas se nombran, no se rellenan.
- **Sin puertas** — el trabajo es mecánico y no necesita que decidas nada a mitad (renombrar en todo el repo, meter tests a N módulos iguales, migrar un patrón repetido). → Puede ir a un **loop**, y entonces cada fase cierra con un **comando verificable**, no con juicio humano.

Casi todo el trabajo de producto tiene puertas. No fuerces "sin puertas" para justificar un loop.

### 3. Escribe el plan por fases
Cada fase con la forma fija (ver Formato). Ordénalas por dependencia: la fase 1 no depende de nadie y termina bien sola.

### 4. Cierra con producto final y recomendación honesta
- **Producto final:** un párrafo — qué cambia para el usuario cuando esté todo.
- **Recomendación:** qué fase ahora, qué fase gated (y en qué dato o decisión), qué fase aparcar. Si una fase no vale la pena, dilo y recomiéndala aparcar — no la vendas por completar.

## Formato de salida

Entrega exactamente esta estructura, en español, sin preámbulo:

```
## Plan por fases — [trabajo]

### Fase 1 · [nombre corto]
**Qué se hace.** [concreto]
**Qué hay que decidir contigo.** [la puerta humana, o "Nada" si no hay]
**Producto.** [qué existe al terminar la fase]
**Cómo sabremos que sirvió.** [señal observable para ti, o comando verificable si es loop]

### Fase 2 · [nombre corto]
[igual]

(las fases que hagan falta)

## El producto final
[un párrafo: qué cambia para ti cuando esté todo hecho]

## Recomendación
[qué fase ahora · qué fase gated en qué · qué aparcar, con la razón]

## ¿Loop o guía?
[Con puertas: "Este trabajo tiene decisiones tuyas en medio ([cuáles]) — es un plan-guía, no un loop."
 Sin puertas: "Trabajo mecánico sin puertas — cada fase cierra con su comando verificable, puede ir al /loop."]
```

## Reglas duras

1. **No escribes código.** Solo el plan. Después, el usuario arranca el flujo normal (o el loop, si aplica).
2. **No inventas datos.** Si una fase depende de algo que no puedes ver (una clave que solo vive en el server, qué opción prefiere el usuario), nómbralo como puerta y detén ahí esa fase. No rellenes.
3. **Ninguna fase sin "cómo sabremos que sirvió".** Una fase sin señal de éxito no está planificada, está enunciada.
4. **Honestidad sobre completar.** Si una fase no vale la pena, se dice y se aparca. Un plan que infla fases para verse completo sabotea al que lo ejecuta.
5. **No prometas loop desatendido en trabajo con puertas.** Es la mentira que esta skill existe para no decir.
6. Español neutro, directo, sin emojis, sin adulación.

## Qué NO es

| No es | Porque |
|---|---|
| El validador | el validador juzga SI hacerlo (GO/NO-GO); planificador asume el GO y arma el CÓMO por fases |
| walle | walle abre caminos (diverge); planificador ordena en fases el camino ya elegido |
| `/architect` | architect diseña proyectos nuevos enteros; esto es trabajo corto-mediano en un repo que ya existe |
| `/architect-brownfield` | brownfield hace blueprint de 20 secciones con parity/cutover para cambios grandes; esto es el plan ligero para lo pequeño-mediano |
| Un lanzador de loops | solo arma el loop cuando el trabajo es mecánico y sin puertas; si tiene decisiones tuyas, entrega un plan-guía |
| Ejecución | no escribe código; entrega el plan y para |

---

**Lugar en el pipeline:** divergir (`/walle`) → juzgar (`/validador`) → **planificar (`/planificador`)** → construir.
