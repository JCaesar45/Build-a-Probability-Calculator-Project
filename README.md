```markdown
# Probability Calculator: Draw Balls from a Hat

This project simulates drawing balls of different colors from a hat to estimate the probability of drawing a specific combination of balls. It uses Monte Carlo simulations by performing many random experiments.

---

## Features

- Create a `Hat` class that initializes with a variable number of balls of different colors.
- Draw a specified number of balls randomly without replacement.
- Conduct multiple experiments to estimate the probability of drawing certain balls.

---

## How It Works

- The `Hat` class:
  - Accepts any number of keyword arguments representing the counts of different colored balls.
  - Stores the balls in a list called `contents`.
  - Implements a `draw` method to randomly select balls without replacement.

- The `experiment` function:
  - Runs multiple trials.
  - For each trial, makes a deep copy of the original `Hat`.
  - Draws a number of balls.
  - Checks if the drawn balls meet the expected criteria.
  - Calculates the estimated probability based on successful trials.

---

## Installation

No external libraries are required beyond Python's standard library.

---

## Usage

### Import the classes and functions

```python
from main import Hat, experiment
``

### Create a `Hat` object

```python
hat = Hat(red=5, blue=2, green=3)
``

### Run an experiment

```python
probability = experiment(
    hat=hat,
    expected_balls={'red': 2, 'green': 1},
    num_balls_drawn=4,
    num_experiments=2000
)

print(f"Estimated probability: {probability}")
``

---

## Example

```python
# Create a hat with specified balls
hat = Hat(blue=5, red=4, green=2)

# Estimate the probability of drawing at least 2 red and 2 green balls in 4 draws
probability = experiment(
    hat=hat,
    expected_balls={'red': 2, 'green': 2},
    num_balls_drawn=4,
    num_experiments=5000
)

print(f"Estimated probability: {probability}")
``

---

## Complete Code

```python
import copy
import random

class Hat:
    def __init__(self, **kwargs):
        self.contents = []
        for color, count in kwargs.items():
            self.contents.extend([color] * count)

    def draw(self, num_balls):
        if num_balls >= len(self.contents):
            all_balls = self.contents[:]
            self.contents.clear()
            return all_balls
        drawn_balls = []
        for _ in range(num_balls):
            index = random.randint(0, len(self.contents) - 1)
            drawn_balls.append(self.contents.pop(index))
        return drawn_balls

def experiment(hat, expected_balls, num_balls_drawn, num_experiments):
    successful_experiments = 0
    for _ in range(num_experiments):
        temp_hat = copy.deepcopy(hat)
        drawn = temp_hat.draw(num_balls_drawn)
        counts = {}
        for ball in drawn:
            counts[ball] = counts.get(ball, 0) + 1
        success = True
        for color, count in expected_balls.items():
            if counts.get(color, 0) < count:
                success = False
                break
        if success:
            successful_experiments += 1
    return successful_experiments / num_experiments
``

---

## Contributing

Feel free to fork, improve, or suggest features for this project!

---

## License

This project is open-source and available for educational purposes.
```
