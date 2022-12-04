# Expected Values

## The Moment-Generating Function

- Moment-Generating Function: $M(t)=E(e^{tX})$
- $M^{(r)}(0)=E(X^r)$
- Characteristic Function: $\phi(t)=E(e^{itX})$



## Approximate Methods (Linear)

- 单变量一阶泰勒展开: $Y=g(X)\approx g(\mu_X)+(X-\mu_X)g'(\mu_X)$
  - $\mu_Y=g(\mu_X)$
  - $\sigma^2_{Y}=\sigma^2_X[g'(\mu_X)]^2$
- 单变量二阶泰勒展开：$Y=g(X)\approx g(\mu_X)+(X-\mu_X)g'(\mu_X)+\frac{1}{2}(X-\mu_X)^2g''(\mu_X)$
  - $E(Y)\approx g(\mu_X)+\frac{1}{2}\sigma^2_Xg''(\mu_X)$
- 双变量一阶泰勒展开：$Z=g(X,Y)\approx g(\mu)+(X-\mu_X)\frac{\part g(\mu)}{\part x}+(Y-\mu_Y)\frac{\part g(\mu)}{\part y}$
  - $E(Z)\approx g(\mu)$
  - $Var(Z)\approx\sigma_X^2(\frac{\part g(\mu)}{\part x})^2+\sigma_Y^2(\frac{\part g(\mu)}{\part y})^2+2\sigma_{XY}(\frac{\part g(\mu)}{\part x})(\frac{\part g(\mu)}{\part y})$
- 双变量二阶泰勒展开：$Z=g(X,Y)\approx g(\mu)+(X-\mu_X)\frac{\part g(\mu)}{\part x}+(Y-\mu_Y)\frac{\part g(\mu)}{\part y}+\frac{1}{2}(X-\mu_X)^2\frac{\part^2 g(\mu)}{\part x^2}+\frac{1}{2}(Y-\mu_Y)^2\frac{\part^2 g(\mu)}{\part y^2}+(X-\mu_X)(Y-\mu_Y)\frac{\part^2 g(\mu)}{\part x\part y}$
  - $E(Z)\approx g(\mu)+\frac{1}{2}\sigma_X^2\frac{\part^2 g(\mu)}{\part x^2}+\frac{1}{2}\sigma_Y^2\frac{\part^2 g(\mu)}{\part y^2}+\sigma_{XY}\frac{\part^2 g(\mu)}{\part x\part y}$