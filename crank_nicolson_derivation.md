
# Appendix – Crank–Nicolson is second order (Taylor expansion)

**Scheme.** For $\dfrac{dy}{dt} = f(y)$, Crank–Nicolson (the trapezoidal rule) is

$$\frac{y^{n+1}-y^n}{\Delta t} = \tfrac12\left[f(y^n) + f(y^{n+1})\right]$$

**Local truncation error $\tau$.** Insert the *exact* solution $y(t)$ into the scheme. The residual is $\tau$:

$$\tau = \frac{y(t_{n+1}) - y(t_n)}{\Delta t} - \tfrac12\left[f\big(y(t_n)\big) + f\big(y(t_{n+1})\big)\right]$$

Because $y$ is exact, $f(y(t)) = y'(t)$. Write $y, y', y'', y'''$ for values at $t_n$.

**Step 1 – Taylor expansion of $y(t_{n+1})$:**

$$y(t_{n+1}) = y + \Delta t\, y' + \frac{\Delta t^2}{2}y'' + \frac{\Delta t^3}{6}y''' + O(\Delta t^4)$$

so the left-hand side becomes

$$\frac{y(t_{n+1}) - y(t_n)}{\Delta t} = y' + \frac{\Delta t}{2}y'' + \frac{\Delta t^2}{6}y''' + O(\Delta t^3)$$

**Step 2 – Taylor expansion of $f(y(t_{n+1})) = y'(t_{n+1})$:**

$$y'(t_{n+1}) = y' + \Delta t\, y'' + \frac{\Delta t^2}{2}y''' + O(\Delta t^3)$$

so the right-hand side becomes

$$\tfrac12\left[y' + y'(t_{n+1})\right] = y' + \frac{\Delta t}{2}y'' + \frac{\Delta t^2}{4}y''' + O(\Delta t^3)$$

**Step 3 – Subtract:**

$$\tau = \underbrace{(y' - y')}_{0} + \underbrace{\left(\tfrac{\Delta t}{2} - \tfrac{\Delta t}{2}\right)y''}_{0} + \left(\tfrac16 - \tfrac14\right)\Delta t^2\, y''' + O(\Delta t^3) = -\frac{\Delta t^2}{12}\,y''' + O(\Delta t^3)$$

The $O(1)$ terms cancel, so the scheme is *consistent*. The $O(\Delta t)$ terms also cancel, because the average of the two end-point slopes matches the $\tfrac{\Delta t}{2}y''$ term exactly. The first surviving term is $O(\Delta t^2)$:

$$\boxed{\tau_{\Delta t} = -\frac{\Delta t^2}{12}\,y''' + O(\Delta t^3) = O(\Delta t^2)\ \Rightarrow\ \text{second order}}$$

**Comparison with the Euler methods.** The same procedure gives:

| Scheme | Truncation error $\tau$ | Order |
|---|---|---|
| Forward Euler: $\frac{y^{n+1}-y^n}{\Delta t} = f(y^n)$ | $+\frac{\Delta t}{2}y'' + O(\Delta t^2)$ | 1 |
| Backward Euler: $\frac{y^{n+1}-y^n}{\Delta t} = f(y^{n+1})$ | $-\frac{\Delta t}{2}y'' + O(\Delta t^2)$ | 1 |
| θ-method | $\left(\tfrac12 - \theta\right)\Delta t\, y'' + O(\Delta t^2)$ | 1, except for θ = ½ |
| Crank–Nicolson (θ = ½) | $-\frac{\Delta t^2}{12}y''' + O(\Delta t^3)$ | **2** |

The leading errors of forward and backward Euler are equal and opposite, so averaging them (θ = ½) cancels them. This is why the explicit and implicit Euler peaks in Task 2.1 approached the Crank–Nicolson result from opposite sides.

**Check with the test equation $y' = \lambda y$.** Write $z = \lambda\Delta t$. One Crank–Nicolson step multiplies $y$ by

$$R(z) = \frac{1 + z/2}{1 - z/2} = 1 + z + \frac{z^2}{2} + \frac{z^3}{4} + \dots$$

while the exact growth over one step is

$$e^{z} = 1 + z + \frac{z^2}{2} + \frac{z^3}{6} + \dots$$

The two agree up to $z^2$. The first difference is $\tfrac{z^3}{12}$, a local error of $O(\Delta t^3)$ per step. Over the $N = T/\Delta t$ steps of the whole run, these add up to a **global error of $O(\Delta t^2)$**, consistent with the result above.

The same factor shows unconditional stability: $\lvert R(z)\rvert \le 1$ for every $\operatorname{Re}(z) \le 0$, i.e. for any $\Delta t$.

**For the PDE (method of lines).** Apply the same expansions in space.

Central difference for the first derivative:

$$\frac{c_{m+1}-c_{m-1}}{2\Delta x} = c_x + \frac{\Delta x^2}{6}c_{xxx} + O(\Delta x^4)$$

Central difference for the second derivative:

$$\frac{c_{m+1}-2c_m+c_{m-1}}{\Delta x^2} = c_{xx} + \frac{\Delta x^2}{12}c_{xxxx} + O(\Delta x^4)$$

Crank–Nicolson with central differences therefore has $\tau = O(\Delta t^2, \Delta x^2)$.

**What this means in practice:**
- Halving $\Delta t$ divides the time error by about **4** (against 2 for Euler).
- This is why the Task 2.1 runs with Δt = 10 min and Δt = 5 min differ by only 0.016 mg/m³, whereas the Euler runs needed Δt ≈ 9 s.
