We have created a mathemetical model to help us describe and predict the spread of Covid-19 in the UK population. This divides the (fixed) population of **_N_** individuals into three component which may vary as a function of time **_t_** :

The components of the SIR model are as follows:

- _S(t)_ are those susceptible but not yet infected with the disease;
- _I(t)_ is the number of infectious individuals;
- _R(t)_ are those individuals who have recovered from the disease and now have immunity to it.

The SIR model describes the change in the population of each of these components in terms of two parameters **_β_** and **_γ_**. _β_ describes the effective _contact rate_ of the disease. An infected individual can come into contact with **_βN_** other individuals per unit of time. The fraction of those individuals who are suspectible to contracting the disease is **_S/N_**. _γ_ is the average recovery rate and **_1/γ_** is the average period of time during which an infected individual can pass on the disease.

Below is a simple Python code of what the SIR graphs would look like:

```python
import numpy as np
from scipy.integrate import odeint
import matplotlib.pyplot as plt

# Total population, N.
N = 1000
# Initial number of infected and recovered individuals, I0 and R0.
I0, R0 = 1, 0
# Everyone else, S0, is susceptible to infection initially.
S0 = N - I0 - R0
# Contact rate, beta, and mean recovery rate, gamma, (in 1/days).
beta, gamma = 0.2, 1./10 
# A grid of time points (in days)
t = np.linspace(0, 160, 160)

# The SIR model differential equations.
def deriv(y, t, N, beta, gamma):
    S, I, R = y
    dSdt = -beta * S * I / N
    dIdt = beta * S * I / N - gamma * I
    dRdt = gamma * I
    return dSdt, dIdt, dRdt

# Initial conditions vector
y0 = S0, I0, R0
# Integrate the SIR equations over the time grid, t.
ret = odeint(deriv, y0, t, args=(N, beta, gamma))
S, I, R = ret.T

# Plot the data on three separate curves for S(t), I(t) and R(t)
fig = plt.figure(facecolor='w')
ax = fig.add_subplot(111, axis_bgcolor='#dddddd', axisbelow=True)
ax.plot(t, S/1000, 'b', alpha=0.5, lw=2, label='Susceptible')
ax.plot(t, I/1000, 'r', alpha=0.5, lw=2, label='Infected')
ax.plot(t, R/1000, 'g', alpha=0.5, lw=2, label='Recovered with immunity')
ax.set_xlabel('Time /days')
ax.set_ylabel('Number (1000s)')
ax.set_ylim(0,1.2)
ax.yaxis.set_tick_params(length=0)
ax.xaxis.set_tick_params(length=0)
ax.grid(b=True, which='major', c='w', lw=2, ls='-')
legend = ax.legend()
legend.get_frame().set_alpha(0.5)
for spine in ('top', 'right', 'bottom', 'left'):
    ax.spines[spine].set_visible(False)
plt.show()
```
![Image of SIR model](https://scipython.com/static/media/examples/E8/extras/sir.png)
