# SmartTrading.Research

Resultados del pipeline WFO de [SmartTradingBT](https://github.com/lafjcr/SmartTrading) (`Research/SmartTradingBT` del repo
`SmartTrading`). Empieza limpio (2026-09-22) — los backtests sueltos previos vivían en `smartrading/Research/results`
y no se migraron porque son regenerables en segundos con `SmartTradingBT regress`/`SmartTradingBT run`.

## Estructura

```
<EA>/<config>/<broker>/<symbol>/<timeframe>/
  01-WFO/
  02-BE-TS/
  03-NM/              # NM = Negative Months (o Negative Days/Months en Pipeline B)
  04-FullRange/
  05-Montecarlo/       # pendiente de implementar
  06-Incubation/        # resultados de ejecución real en demo
  07-Real/               # resultados de ejecución en cuentas live
```

- **`<EA>`**: nombre de la estrategia (`SmartCrossOver`, `SmartORB`, ...).
- **`<config>`**: id estable de la configuración probada — hash corto del `.set`/grid normalizado + alias legible
  (ej. `a3f01c-orb-ndx-buys-mon`), nunca el nombre libre del `.set` de origen.
- **`<broker>/<symbol>/<timeframe>`**: igual convención que `SmartTrading.Data`.

Cada ronda (`0N-*`) guarda como mínimo:
- `input.set` / `output.set` — parámetros de entrada y el/los ganador(es).
- `lineage.json` — de qué ronda/config viene (padre), commit de `SmartTradingBT`, hash del `.set`, versión de las fichas
  de broker usadas, rango de fechas, pipeline type (A/B).
- El detalle específico de la ronda (folds del WFO, exclusión de meses/días, divergencia OHLC vs Every Tick, etc.).

`06-Incubation/` y `07-Real/` son los únicos datos no regenerables de este repo (resultados de ejecución real) —
se versionan completos, particionados por mes.

## Índice

Un archivo único `runs.parquet` (o `.csv` mientras el volumen es bajo) en la raíz, una fila por corrida/ronda con
sus métricas clave, para no tener que recorrer carpetas al consultar resultados desde Claude u otra herramienta.
Se genera y mantiene automáticamente al correr `SmartTradingBT pipeline` (pendiente de implementar).

## Pendiente

- Integración con el agente de Stratlixa (`stratlixa-agent`) — anotado como pendiente hasta que el pipeline esté
  completo y probado; Stratlixa está migrando en paralelo a Vercel + Supabase local (Docker).
