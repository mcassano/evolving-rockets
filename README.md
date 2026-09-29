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
   (routing around walls, not straight-line distance) at its closest approach
   during the flight, with a penalty for crashing and a bonus for arriving early.
3. **Select & breed** – parents are picked by tournament selection, combined
   with single-point crossover, and mutated. The top few rockets (elites) are
   copied over unchanged. Mutations are concentrated on the genes just before
   the point where each parent crashed (later genes were never expressed, and
   earlier ones are already tuned), which helps the population escape dead ends.

Panels show:

- **Arena** – live flight, coloured by rank, with an optional path-distance field.
- **Genomes & breeding** – each rocket's DNA as a row of coloured cells. Hover to
  trace ancestry; white cells are mutations.
- **Charts** – best/mean distance (with 10th–90th percentile band) and
  population diversity over generations.
- **Gallery** – the best rocket of every generation; click one to overlay it.

## Two kinds of rocket

**Timeline genome** (default). The genome is a fixed list of thrust directions,
so a rocket memorizes one route through one maze. It's fast to evolve, but
useless on a maze it hasn't seen.

**Reactive · sensors + network.** Each rocket casts 7 rays, senses the
direction of the goal and its own speed, and feeds those 11 inputs through a
small neural network (8 hidden neurons) that outputs turn and thrust. The
genome is the network's 114 weights, so it evolves a *policy* instead of a path.

- **Sensor rays** are drawn for the leading (or hovered) rocket: colour shows
  how close the wall is, dots mark hits, and the gold arrow is the goal sensor.
- **Brain** panel shows the live network for that rocket: neuron activations and
  signal flowing along each weight.
- **Terrain** rotates every few generations. Each generation is ranked on a pool
  of four mazes (one on screen, three flown unseen), so the network can't
  memorize a layout.
- **Held-out mazes** are 12 fixed layouts the population never trains on. The
  leading genome is tested on them every generation, and the header shows how
  many it solves. Try it with terrain set to *fixed* versus *new maze every 5
  gens* to see the difference generalization makes.
- **Path guide** switches the goal sensor between the shortest-route direction
  and the straight line to the goal.

## Mazes

Pick a maze type and hit **New maze** to generate a fresh layout:

- **Classic** – the original three-wall serpentine.
- **Serpentine** – random wall count, gap positions and widths.
- **Scattered** – random rectangles, rejected unless a wide-enough route exists.
- **Labyrinth** – a depth-first-search maze on a grid, with the goal placed far from the start.

The maze is stored in the URL (`#maze=labyrinth&seed=101`), so a layout can be
shared or replayed.

## Controls

| Control | Action |
| --- | --- |
| Space | Play / pause |
| → | Skip to the next phase |
| Rockets / Terrain | Timeline or reactive rockets; how often the maze changes |
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
