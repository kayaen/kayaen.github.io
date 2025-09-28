---
title: Monty Hall Problem Simulation
date: 2025-09-28T00:00:00.000Z
format: hugo-md
execute:
  echo: true
  warning: false
  error: false
  fig-path: figure
categories:
  - basics
tags:
  - python
  - probablity
---


# The Monty Hall Problem

The Monty Hall problem is a classic probability puzzle that often surprises people with its counterintuitive result. The setup is simple: you pick one of three doors, behind one is a car and behind the others are goats. After your choice, the host reveals a goat behind one of the other doors and gives you the option to stay with your choice or switch.

Mathematically, staying gives you a 1/3 chance of winning, while switching boosts your chances to 2/3. To see this in action, I wrote a Python simulation to compare both strategies. Let's explore how the results play out when we run the experiment thousands of times.

``` python
import random

stay_wins = stay_losses = 0
switch_wins = switch_losses = 0

for _ in range(100_000):
    car = random.randint(0, 2)
    choosen = random.randint(0, 2)

    # Host reveals a goat door
    showing = random.choice([x for x in range(3) if x not in [car, choosen]])

    action = random.randint(0, 1)  # 0 -> stay, 1 -> switch

    # Final choice after action
    if action == 1:  # switch
        final_choosen = [x for x in range(3) if x not in [showing, choosen]][0]
    else:  # stay
        final_choosen = choosen

    # Count results
    if action == 0:  # stay
        if final_choosen == car:
            stay_wins += 1
        else:
            stay_losses += 1
    else:  # switch
        if final_choosen == car:
            switch_wins += 1
        else:
            switch_losses += 1

print(f"Stay    wins:{stay_wins}   loose:{stay_losses}")
print(f"Switch  wins:{switch_wins}   loose:{switch_losses}")
```

    Stay    wins:16712   loose:33069
    Switch  wins:33462   loose:16757

The simulation confirms the theory: staying wins about one-third of the time, while switching wins about two-thirds of the time. Even though it feels like both options should be 50/50 after a door is revealed, the numbers tell a different story. By running this experiment thousands of times, we see how probability plays out in practice and why the counterintuitive "always switch" strategy gives you a much higher chance of winning.

In fact, there's a video ([this video at timestamp 8:30](https://youtu.be/UgKrQ2ywVfs?si=jq6uhAmdTNS0ABiY&t=510)) where the speaker explains that by switching, you're effectively selecting two doors instead of one. This insight helped me visualize how the odds shift in favor of switching, making the concept more intuitive.
