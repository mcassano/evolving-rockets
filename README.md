# Evolving Rockets

An interactive visualization of a genetic algorithm. A population of rockets
learns to fly through a serpentine maze to reach a goal, with no knowledge of
the maze beyond how well each generation performed.

It's a single self-contained `index.html` with no dependencies or build step.
Open it in a browser to run it.

## What you're looking at

Each generation cycles through three phases:

1. **Flight** – every rocket flies its own genome: 150 thrust directions, each
   held for 6 steps.
2. **Evaluate** – rockets are scored by shortest path distance to the goal
   (routing around walls, not straight-line distance), with a penalty for
   crashing and a bonus for arriving early.
3. **Select & breed** – parents are picked by tournament selection, combined
   with single-point crossover, and mutated. The top few rockets (elites) are
   copied over unchanged.

Panels show:

- **Arena** – live flight, coloured by rank, with an optional path-distance field.
- **Genomes & breeding** – each rocket's DNA as a row of coloured cells. Hover to
  trace ancestry; white cells are mutations.
- **Charts** – best/mean distance (with 10th–90th percentile band) and
  population diversity over generations.
- **Gallery** – the best rocket of every generation; click one to overlay it.

## Controls

| Control | Action |
| --- | --- |
| Space | Play / pause |
| → | Skip to the next phase |
| Speed | Simulation speed |
| Mutation | Per-gene mutation rate |

## Running locally

```sh
open index.html
# or serve it
python3 -m http.server
```

## License

[MIT](LICENSE)
