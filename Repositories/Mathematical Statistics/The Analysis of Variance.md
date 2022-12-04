# The Analysis of Variance

## The One-Way Layout

### Normal Theory; the $F$ Test

- $Y_{ij}=\mu+\alpha_i+\epsilon_{ij}$
  - $\mu$: overall mean level
  - $\alpha_i$: differential effect, $\sum^I_{i=1}\alpha_i=0$
  - $\epsilon_{ij}\sim N(0,\sigma^2)$: random error
- $E(Y_{ij})=\mu+\alpha_{i}$
- $H_0$: $\alpha_i=0$
  - test statistic: $F=\frac{SS_B/(I-1)}{SS_W/[I(J-1)]}$

- $SS_{TOT}=SS_W+SS_B$
- $E(SS_W)=I(J-1)\sigma^2$
  - $\frac{SS_W}{\sigma^2}\sim\chi^2_{I(J-1)}$
- $E(SS_B)=J\sum^I_{i=1}\alpha^2_i+(I-1)\sigma^2$
  - $H_0$: $\frac{SS_B}{\sigma^2}\sim\chi^2_{I-1}$
  - independent of $SS_W$

![image-20220110171222533](C:\Users\Elian Li\AppData\Roaming\Typora\typora-user-images\image-20220110171222533.png)



### Treatments with Different Sample Size

- $H_0$: $\alpha_i=0$
  - test statistic: $F=\frac{SS_B/(I-1)}{SS_W/[\sum^I_{i=1}J_i-I)]}$



### A Nonparametric Method - The Kruskal-Wallis Test

- $SS_B=\sum^I_{i=1}J_i(\overline{R_{i\cdot}}-\overline{R_{\cdot\cdot}})^2$
- with $I=3$ and $J_i\ge5$ or $I>3$ and $J_i\ge4$: $K=\frac{12}{N(N+1)}SS_B\sim\chi^2_{I-1}$ 



## The Two-Way Layout

### Normal Theory for the Two-Way Layout

- $y_{ijk}=\mu+\alpha_i+\beta_j+\delta_{ij}+\epsilon_{ijk}$
  - $\sum^I_{i=1}\alpha_i=0$, $\sum^J_{j=1}\beta_j=0$, $\sum^I_{i=1}\delta_{ij}=\sum^J_{j=1}\delta_{ij}=0$
  - $\epsilon_{ijk}\sim N(0,\sigma^2)$: random error
- $H_A$: $\alpha_i=0$
- $H_B$: $\beta_i=0$
- $H_{AB}$: $\delta_{ij}=0$
- $SS_{TOT}=SS_A+SS_B+SS_{AB}+SS_E$

<img src="C:\Users\Elian Li\AppData\Roaming\Typora\typora-user-images\image-20220110095815561.png" alt="image-20220110095815561" style="zoom:80%;" />

<img src="C:\Users\Elian Li\AppData\Roaming\Typora\typora-user-images\image-20220110095201101.png" alt="image-20220110095201101" style="zoom: 50%;" />



### Randomized Block Designs

- $y_{ij}=\mu+\alpha_i+\beta_j+\epsilon_{ij}$
  - $\sum^I_{i=1}\alpha_i=0$, $\sum^J_{j=1}\beta_j=0$
  - $\epsilon_{ij}\sim N(0,\sigma^2)$: random error
- $H_A$: $\alpha_i=0$
- $H_B$: $\beta_i=0$
- $SS_{TOT}=SS_A+SS_B+SS_{AB}$

<img src="C:\Users\Elian Li\AppData\Roaming\Typora\typora-user-images\image-20220110095838822.png" alt="image-20220110095838822" style="zoom:50%;" />

<img src="C:\Users\Elian Li\AppData\Roaming\Typora\typora-user-images\image-20220110095659809.png" alt="image-20220110095659809" style="zoom:50%;" />



### A Nonparametric Method - Friedman's Test

- $SS_A=J\sum^I_{i=1}(\overline{R_{i\cdot}}-\overline{R_{\cdot\cdot}})^2$
- $Q=\frac{12}{I(I+1)}SS_A\sim\chi^2_{I-1}$ 



## Problems

21. During each of four experiments on the use of carbon tetrachloride as a worm killer, ten rats were infested with larvae (Armitage 1983). Eight days later, five rats were treated with carbon tetrachloride; the other five were kept as controls. After two more days, all the rats were killed and the numbers of worms were counted. The table below gives the counts of worms for the four control groups. Significant differences, although not expected, might be attributable to changes in experimental conditions. A finding of significant differences could result in more carefully controlled experimentation and thus greater precision in later work. Use both graphical techniques and the $F$ test to test whether there are significant differences among the four groups. Use a nonparametric technique as well.

    | Group 1 | Group 2 | Group 3 | Group 4 |
    | ------- | ------- | ------- | ------- |
    | 279     | 378     | 172     | 381     |
    | 338     | 275     | 335     | 346     |
    | 334     | 412     | 335     | 340     |
    | 198     | 265     | 282     | 471     |
    | 303     | 286     | 250     | 318     |

    

23. For a study of the release of luteinizing hormone (LH), male and female rats kept in constant light were compared to male and female rats in a regime of 14 h of light and 10 h of darkness. Various dosages of luteinizing releasing factor (LRF) were given: control (saline), 10, 50, 250, and 1250 ng. Levels of LH (in nanograms per milliliter of serum) were measured in blood samples at a later time. Analyse the data given in file LHfemale, LHmale to determine the effects of light regime and LRF on release of LH for both males and females. Use both graphical techniques and more formal analyses.