# Mid-Side Separation

## Separate

### The problem

The goal is to find relations for a mid and a side channel for an audio signal from the left and the right channels, such that they for example can be separately processed. One way to view this problem is in terms of a vector space $W = \text{span}(\hat{l}, \hat{r})$ with basis vectors $\hat{l}$ and $\hat{r}$ representing the left and right channels respectively. Let $\vec{w} \in W$ be the vector representation of an audio signal. Similarly, $\hat{s}, \hat{m} \in W$ represent the side and mid channels respectively. Then,

$$
\begin{aligned}
    \vec{w} &= l\hat{l} + r\hat{r}\\
    &= s\hat{s}+m\hat{m}.
\end{aligned}
$$

The audio signal $\vec{w}$ can be composed with either the $\{\hat{l}, \hat{r}\}$ basis or with the $\{\hat{s}, \hat{m}\}$ basis. Naturally this means that one basis can be written in terms of the other [^leon].

$$
\begin{aligned}
    \vec{w} &= l\hat{l} + r\hat{r}\\
    &= l(a\hat{s}+c\hat{m}) + r(b\hat{s}+d\hat{m})\\
    &= (al+br)\hat{s} + (cl+dr)\hat{m}\\
    &= s\hat{s} + m\hat{m},
\end{aligned}
$$

for coefficients $a, b, c, d$. In matrix notation,

$$
\begin{pmatrix}s\\m\end{pmatrix} = \begin{pmatrix}a&b\\c&d\end{pmatrix}\begin{pmatrix}l\\r\end{pmatrix},
$$

$$
\begin{aligned}
    \bullet\ s &= al+br & \bullet\ m &= cl + dr.
\end{aligned}
$$

Essentially the goal is to find the coefficients $a, b, c, d$. To find them, first realise that the magnitude $|\vec{w}|$ should remain the same in either basis:

$$
\begin{aligned}
    |\vec{w}|^2 &= l^2 + r^2\\
    &= s^2 + m^2\\
    &= (al-br)^2 + (cl + dr)^2\\
    &= a^2l^2 - 2ablr + b^2r^2 + c^2l^2 + 2cdlr + d^2r^2\\
    &= (a^2 + c^2)l^2 + (b^2 + d^2)r^2 + 2(cd-ab)lr\\
    &= l^2 + r^2.
\end{aligned}
$$

As a result,

$$
\begin{aligned}
    \bullet\ a^2 + c^2 &= 1 & \bullet\ b^2 + d^2 &= 1 & \bullet\ cd-ab = 0
\end{aligned}
$$

This is as far as we can meaningfully go without invoking any constraints. 

### Constraints

When an audio signal has the same information for the left and right channels, the sound appears to come from the center; it appears to be mono. If both sides differ slightly, the content both sides have in common will still appear from the center, such that whatever is different between both channels appears to be wide. Therefore, one of the constraints which will anecdotally be imposed is that the side channel is the difference between the left and the right channel,

$$s \propto (l - r).$$


Then,

$$
\begin{aligned}
    \bullet\ c &= \pm' \sqrt{1 - a^2} & \bullet\ d &= \pm'' \sqrt{1 - a^2} & \bullet\ cd-a^2 = \pm(1-a^2) - a^2 = \pm 1 - a^2(1 \pm 1) = 0,
\end{aligned}
$$

with $\pm$ coming from $\pm' \cdot \pm''$. The prime notation was used to clarify that those relations might have different signs. 

Notice that $-1 - a^2(1-1) = -1 - 0 = 0$ is not possible, such that it constraints $c$ and $d$; $c$ and $d$ should have the same sign; $c/|c| = d/|d|$. Then,

$$1 - 2a^2 =0 \quad \Rightarrow \quad a = \pm\tfrac{1}{\sqrt{2}} \quad \Rightarrow \quad c = d = \pm\sqrt{1-\tfrac{1}{2}} = \pm\tfrac{1}{\sqrt{2}}.$$

Therefore,

$$
\begin{pmatrix}s\\m\end{pmatrix} = \tfrac{1}{\sqrt{2}}\begin{pmatrix}\pm1&\mp1\\\pm'1&\pm'1\end{pmatrix}\begin{pmatrix}l\\r\end{pmatrix},
$$

$$
\begin{aligned}
    \bullet\ s &= \pm\tfrac{1}{\sqrt{2}}(l - r) & \bullet\ m &= \pm'\tfrac{1}{\sqrt{2}}(l + r).
\end{aligned}
$$

In principle the signs can be chosen as long as it is consistent with the definition of the matrix. For further analysis, it will be chosen that $a = \pm 1/\sqrt{2}$, which is what Bitwig seems to do, and for $m$ to be positive when $(l+r)$ is positive to keep the same polarity for the mono signal. In that case,

$$
\begin{pmatrix}s\\m\end{pmatrix} = \tfrac{1}{\sqrt{2}}\begin{pmatrix}1&-1\\1&1\end{pmatrix}\begin{pmatrix}l\\r\end{pmatrix},
$$

$$
\begin{aligned}
    \bullet\ s &= \tfrac{1}{\sqrt{2}}(l - r) & \bullet\ m &= \tfrac{1}{\sqrt{2}}(l + r).
\end{aligned}
$$

Notice how this looks like a rotation matrix for a rotation of $\pi/4$ [^leon]. 

With these relations an audio signal can be separated into a mid and a side signal.

## Combine

Naturally it is desirable now to also define the inverse operation; to be able to combine the mid and side channels. The goal here is to find the inverse of the matrix found in the previous section [^leon]:

$$
\sqrt{2}\begin{pmatrix}1&-1\\1&1\end{pmatrix}^{-1}\begin{pmatrix}s\\m\end{pmatrix} = \begin{pmatrix}1&-1\\1&1\end{pmatrix}^{-1}\begin{pmatrix}1&-1\\1&1\end{pmatrix}\begin{pmatrix}l\\r\end{pmatrix} = \begin{pmatrix}l\\r\end{pmatrix}.
$$

Let

$$\begin{pmatrix}1&-1\\1&1\end{pmatrix}^{-1} = \begin{pmatrix}\alpha&\beta\\\gamma&\delta\end{pmatrix}$$.

Then the problem essentially boils down to finding $\alpha, \beta, \gamma, \delta$.

$$\begin{pmatrix}\alpha&\beta\\\gamma&\delta\end{pmatrix}\begin{pmatrix}1&-1\\1&1\end{pmatrix} = \begin{pmatrix}1&\\&1\end{pmatrix},$$

$$
\begin{aligned}
    \bullet\ \alpha + \beta &= 1 & \bullet\ \alpha - \beta &= 0 & \bullet\ \gamma + \delta &= 0 & \bullet\ -\gamma + \delta = 1.
\end{aligned}
$$

It can immediately be inferred that $\alpha = \beta$ and $\gamma = -\delta$, such that $\alpha=\beta=1/2$, and $\delta = -\gamma = 1/2$.

Therefore, the mid and side channels can be combined through

$$
\begin{pmatrix}l\\r\end{pmatrix} = \frac{1}{\sqrt{2}}\begin{pmatrix}1&1\\-1&1\end{pmatrix}\begin{pmatrix}s\\m\end{pmatrix}.
$$

## Bibliography

[^leon]: S. J. Leon, *Linear Algebra with Applications*, 9th ed. in Always learning. Boston Munich: Pearson, 2015.