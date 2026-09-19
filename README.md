# Tetris with a genetic-algorithm AI

A Tetris implementation in Java with an AI agent that learns to play it. The
agent won first place in a university Tetris AI tournament at Saarland
University in 2019.

https://user-images.githubusercontent.com/37556412/142862021-13e18e35-7f4d-4124-ba1f-0f50a1fb4656.mp4

## How the AI works

For every incoming piece the agent enumerates the reachable placements
(`PosCalc`), scores each resulting board with a weighted linear evaluation
function over 15 board features, and plays the highest-scoring one.

The weights are not hand-tuned. They are evolved with a genetic algorithm
(`Gene`):

- A genome is a vector of 15 real-valued weights, initialised uniformly in
  [-1, 1].
- Fitness is not a generic proxy like survival time. It mirrors the scoring
  rule the tournament actually used, so the population is selected against the
  metric it would later be judged on rather than a correlated stand-in.
- Offspring are produced by uniform crossover: each weight is inherited from
  one of the two parents with equal probability.
- Each weight then mutates with probability 0.1, scaled by a Gaussian factor
  (`w * (1 + N(0,1) * 0.3)`), which keeps small adjustments common and large
  jumps rare.

Genomes are serialisable and the evolved population is checkpointed to
[`genes.txt`](genes.txt), so training can be resumed and the best genome can be
replayed without retraining.

## Running it

Watch the AI play:

```
src/tetris/view/AutoplayerView.java
```

Playback speed is set [here](src/tetris/view/AutoplayerView.java#L22); fix a
random seed [here](src/tetris/view/AutoplayerView.java#L17) to reproduce a run.

Play yourself:

```
src/tetris/view/PlayerView.java
```

## Layout

| Path | Contents |
| --- | --- |
| `src/tetris/game` | Game rules, board state, piece logic |
| `src/tetris/autoplay` | Agent, placement search, genetic algorithm |
| `src/tetris/view` | Swing views for manual and automatic play |
| `src/tetris/tests` | Tests |
