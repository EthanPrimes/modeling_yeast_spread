# Modeling Yeast Spread with Compartmental and Predator-Resource Models

In this ReadMe we will present a summarized version of our method and results. See the full paper [here](Paper/Project%20Paper.pdf)

## Motivation and Set-up
It's difficult to make bread. The fact that small changes in the either the method of preparation or the environment (temperature, pressure, etc.) lead to large changes in the outcome make bread an ill-conditioned process. At the same time, nearly everybody really likes good bread, so this problem deserves careful consideration.

### Chemistry of Bread
There are four primary ingredients in bread dough: flour, water, salt, and yeast. Flour provides not only the gluten that gives bread its structure, but also contains sugars that the yeast consumes. Salt strengthens the bread dough, making it more elastic. 

Yeast consumes the available sugars in two phases: aerobic when oxygen is available, and anaerobic when oxygen is depleted. The presence of oxygen allows the yeast to synthesize more energy from the sugar it consumes, and the yeast multiplies much faster during the aerobic stage.

## Modeling
We modeled the amount of yeast, $CO_2$, and sometimes byproducts with four different models.

### Simple Compartmental
This simple categorical model is a slightly modified SIR model. The main difference is that while the compartments represent concentrations, they do not sum to a constant value. Rather than mass or something analagous passing from one compartment to the next, a kind of "energy" or "life-force" passes from sugar to yeast to byproducts. The ODE is given by

$$\begin{align*}
    \dot{S} &= -\alpha \cdot S(t) \cdot Y(t) \\
    \dot{Y} &= \beta \cdot S(t) \cdot Y(t) - \gamma \cdot Y(t) \\
    \dot{C} &= \delta \cdot S(t) \cdot Y(t)
\end{align*}$$

Note that $\alpha$ is not necessarily equal to $\beta$ because the amount of sugar consumed by yeast is related though not equal to the biomass added to the yeast, and the rate at which yeast consumes sugar is not comparable to the rate at which yeast reproduces.

We fit this model to data described in the paper with surprisingly good results.

![Simple Compartmental Fit](Paper/images/simple_compartmental.png)

### Compartmental Model with Monod Equation
This model is a more sophisticated twist on the prior simple compartmental model in that it leverages an insight from the biologist Monod.

$$\begin{align*}
    \dot{Y} &= \mu_{\text{max}} \frac{S}{K_s + S}Y \\
    \dot{S} &= -\frac{1}{Y_{X/S}} \dot{Y} \\
    \dot{P} &= Y_{P/S} (-\dot{S})
\end{align*}$$

![Monod Categorical Fit](Paper/images/monod_modeling.png)

### MacArthur Consumer-Resource Model
This model is motivated by thinking of Yeast as a consumer with two different resources, namely sugar and oxygen, each of which it metabolizes differently--at different rates, with different levels of efficiency, and with different byproducts. In fact, yeast metabolizes sugar differently depending on the amount of oxygen available. We consider the change in concentration of yeast, oxygen, sugar, and carbon dioxide.

$$
\begin{align*}
    \dot{Y} &= Y \bigg[w_O c_O O + [w_S^{ana}​+(w_S^{aer}​−w_S^{ana}​)f(O)]c_S S - m\bigg] \\
    \dot{O} &= -c_O O Y f(O) \\
    \dot{S} &= r_S S (1 - \frac{S}{P_S}) - c_S Y S \\
    \dot{C} &= Y  u_S  (1 - f(O)) \cdot .0111
\end{align*}
$$

Each of the parameters are described in detail in both the paper and in [this notebook](model_code.ipynb).

Unfortunately, despite digging, we were unable to find data against which to validate this model. However, qualitatively, it matches what we expect.

![MacArthur Consumer Resource Model](Paper/images/mcrm.png)

## Appropriateness of Models
The two compartmental models captured the changing concentrations of their assumed dynamics with surprising accuracy, considering their simplicity. The fitted simple compartmental model yielded a mere 21% error for the glucose consumed to ethanol produced ratio. However, the assumed dynamics are quite a far cry from the dynamics of a rising loaf of bread dough. The most egregious simplifiying assumption was that all sugar is initially available and does not replenish. Fortunately experimental data that matched the assumptions validated these models
nicely. 

The most complex model we attempted to use, the MacArthur Consumer- Resource model, corrected the previous simplifications by including terms to describe 1) a replenishing supply of available sugar, 2) a variable rate of sugar consumption and yield depending on the oxygen supply, and 3) metabolism of oxygen when available. Although we were unable to find or produce experimental data to validate this model, we found it satisfactory qualitatively. The phases of aerobic metabolism, anaerobic metabolism, and equilibrium are each amenable to visual inspections of the figures the model produces.

## Impact
The impact of an accurate bread-rising model would be profound and far-reaching. Bread is a worldwide staple, and is much beloved by nearly all cultures. An accurate model would create tremendous accessibility consistently higher quality bread, exactly as desired by its bakers, despite the complexity.

Such a day is yet beyond us. Using the models we produced, by fiddling with initial values, a user can easily see the effects of changing the initial concentrations of yeast, sugar, and oxygen, and use these initial concentrations to create the desired effect while baking bread.

Much work remains to be done.