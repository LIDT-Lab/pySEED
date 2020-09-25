# pySEED
## A brief description of SEED algorithm
pySEED-EDA is a Python implementation of the **Symmetric-Approximation Energy-Based Estimation of Distribution (SEED) algorithm: A Continuous Optimization Algorithm** [[1]](#1), which allows the optimization in continuous space for independent variable functions, based on distribution estimation algorithms, in the Univariate Marginal Distribution scheme [[2]](#2), the main idea is to make a generational change in each population evolution under the Boltzmann distribution probability function (PDF-B), which is defined by Eq. (1), because PDF-B is a function that has the property that states with less energy are unlikely, so SEED converges in each evolution to a better or equal energy state.

<p align="center"><img src="img/eq1.svg" /><p>



Due to the difficulty in generating a new population-based on PDF-B, an approximation using mu and sigma parameters of the univariate normal distribution is estimated, minimizing Jeffry's divergence, which is a symmetric measure between two probability distributions and is given by Eq. (2)

<p align="center"><img src="img/2.svg" /><p>

where the normal distribution is given by Eq. (3)

<p align="center"><img src="img/eq3.svg" /><p>

Clearing the Eq. (2) for the mean gives Eq. (4)

<p align="center"><img src="img/eq4.svg" /><p>

finally from the minimization of Eq. (2) and clearing for variance is Eq. (5)

<p align="center"><img src="img/eq5.svg" /><p>

With the results of Eq (4) and (5), a new population with normal distribution is generated and introduced to the UMDA algorithm scheme to search the optimal one, it is worth mentioning that SEED is an algorithm designed for the maximization problem [[1]](#1).

## Requirements and installation
pySEED needs the requirements listed in the table below

| Requirement | Version |
| :---: | :---: |
| Python | >=3.5.2 |
| numpy | >=1.18.2 |
| joblib | >=0.14.1 |

To install pySEED directly on the operating system it is distributed through PyPI using the following command
```
pip3 install pySEED
```
## Example
To exemplify the operation of pySEED we will use the classical function in optimization known as the sphere problem and it is defined by the Eq. (6)

<p align="center"><img src="img/eq6.svg" /><p>

Then we define the fitness function of the Eq. (9) and we modify it for the maximization problem since the sphere function was designed to minimize and under the modification, we have the Eq. (7) , which has the optimum in 100000.

<p align="center"><img src="img/eq7.svg" /><p>

In Python programming we first generate a sphere.py file and invoke the predefined fitness function template in pySEED

```
from pyseed.fitness import Fitness
```

we now create a class with the name Sphere as follows

```
class Sphere(Fitness):
    def Evaluate(self,x):
        sum = 0
        for i in range(0,len(x)):
            sum += x[i]*x[i]
        return 1/(1E-5+sum)
```
With the fitness function already created, we generate a new file with the name example.py and inside it the SEED algorithm is invoked with the following instruction

```
from pyseed.seed import Seed
from sphere import Sphere
```

For the operation of SEED certain initial values must be introduced like the population, the dimension of the individuals of the population, the superior and inferior domain, the reached error, the number of calls to function, the optimal one that is estimated to reach, as in particular for this function of the example is known we can place the exact value, however, when it is not known we can place a value to which we want to arrive and finally the elitism of selected population.

```
npopulation = 100
dimention = 10
domMax = 5
domMin = -5
err = 1E-5
callFunction = 50000
optimus = 1E5
elit = 0.02
```
we declare the sphere function and generate the SEED object

```
objFuction = Sphere()
seed = Seed(npopulation,dimention,domMin,domMax,callFunction,err,optimus,elit)
```

Finally the seed object has a running function defined, which at the end of the process returns the best vector solution and its fitness.

```
solution, fitness = seed.run(objFuction)
print(solution,fitness)
```
## Authors
### Velentin Calzada Ledesma
### Juan de Anda Suárez

## How to cite pySEED
```
@ARTICLE{8876622,
  author={J. {De Anda-Suárez} and J. M. {Carpio-Valadez} and H. J. {Puga-Soberanes} and V. {Calzada-Ledesma} and A. {Rojas-Domínguez} and S. {Jeyakumar} and A. {Espinal}},
  journal={IEEE Access},
  title={Symmetric-Approximation Energy-Based Estimation of Distribution (SEED): A Continuous Optimization Algorithm},
  year={2019},
  volume={7},
  number={},
  pages={154859-154871},}
```


## References
<a id="1">[1]</a>
J. De Anda-Suárez, J. M. Carpio-Valadez, H. J. Puga-Soberanes, V. Calzada-Ledesma, A. Rojas-Domínguez, S. Jeyakumar, A. Espinal. (2019). Symmetric-Approximation Energy-Based Estimation of Distribution (SEED): A Continuous Optimization Algorithm.
IEEE Access, 7, 154859-154871.

<a id="2">[2]</a>
Brownlee, J. (2011).
Clever Algorithms: Nature-inspired Programming Recipes.
Lulu.com, ISSN:9781446785065.
