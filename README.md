# Modeling Yeast Spread with Compartmental and Predator-Resource Models

In this ReadMe we will present a summarized version of our method and results. See the full paper [here](Paper/Project%20Paper.pdf)

## Motivation and Set-up
It's difficult to make bread. The fact that small changes in the either the method of preparation or the environment (temperature, pressure, etc.) lead to large changes in the outcome make bread an ill-conditioned process. At the same time, nearly everybody really likes good bread, so this problem deserves careful consideration.

### Chemistry of Bread
There are four primary ingredients in bread dough: flour, water, salt, and yeast. Flour provides not only the gluten that gives bread its structure, but also contains sugars that the yeast consumes. Salt strengthens the bread dough, making it more elastic. 

Yeast consumes the available sugars in two phases: aerobic when oxygen is available, and anaerobic when oxygen is depleted. The presence of oxygen allows the yeast to synthesize more energy from the sugar it consumes, and the yeast multiplies much faster during the aerobic stage.