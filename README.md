# planificador

> Skill para Claude Code y Codex que convierte un trabajo ya decidido en un plan por fases: qué se hace en cada una, qué tienes que decidir tú, qué existe al terminar y cómo sabrás que sirvió. Y te dice con honestidad si ese trabajo se puede dejar en piloto automático o si tiene decisiones tuyas en medio.

## El problema

Después de decidir que algo se hace, siempre falta el mismo paso: el mapa. Sin él pasan dos cosas. O se arranca a construir a ciegas, o se cree que todo plan por fases se puede «activar en un bucle y olvidarse».

Casi todo el trabajo real tiene decisiones tuyas en medio: qué opción prefieres, un dato que solo tú ves, la aprobación de alguien. Ese trabajo se **planifica**, no se automatiza. Esta skill lo dice de frente.

## Qué hace

Cuando dices `/planificador`, «dame un plan por fases» o «arma el plan de esto», la IA:

1. **Toma el trabajo decidido.** Si está difuso, te lo pide en una frase y para.
2. **Mira si tiene puertas humanas**: decisiones tuyas o datos de fuera a mitad de camino.
3. **Escribe las fases** con una forma fija: qué se hace, qué hay que decidir contigo, el producto y **cómo sabremos que sirvió**.
4. **Cierra con el producto final y una recomendación honesta**: qué fase ahora, cuál espera a qué y cuál conviene aparcar.
5. **Te dice si es un bucle o una guía.** Si hay decisiones tuyas en medio, no te vende un piloto automático.

Sin código: solo el plan. Español directo, sin adulación.

## Instalación

### Claude Code

```bash
git clone https://github.com/hildemarosolis/planificador.git ~/.claude/skills/planificador
```

### Codex

```bash
git clone https://github.com/hildemarosolis/planificador.git ~/.agents/skills/planificador
```

Reinicia la app. Úsala con `/planificador` o pidiendo «dame un plan por fases».

## Forma parte de una familia

`planificador` es el tercer paso de una forma de trabajar:

**divergir** (una lluvia de ideas) → **juzgar** ([`validador`](https://github.com/hildemarosolis/validador)) → **planificar** (`planificador`) → **construir**.

Funciona solo. Las menciones a `/architect` dentro de la skill son a una herramienta opcional, [the-architect](https://github.com/hildemarosolis/the-architect): si no la tienes, la IA simplemente te dirá que ese trabajo no es para el planificador.

## Licencia

MIT. Hecha por [Hildemaro Solís](https://github.com/hildemarosolis).
