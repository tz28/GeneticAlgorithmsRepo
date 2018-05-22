## fmga
**fmga** (**f**unction **m**aximization through **g**enetic **a**lgorithms) is a package that takes a genetic algorithm approach to maximization problem of non-convex objective functions in multiple dimensions.
 
The objective function doesn't have to be differentiable, or even continuous in the specified domain!  
The idea is to sample an evolving population of points converging to the function maximum over many iterations.

The population of n-dimensional points undergoes random mutations - and is selected through elitism along with breeding with selection weights inversely proportional to fitness and diversity ranks.


### Installation
Install with pip:
```bash
pip install fmga
```
Import within the python script with:
```python
import fmga
```

### Execution
Given a function on multiple variables, say:
```python
def f(x, y, z):
    return x - math.sin(y) * z
```
Pass this function as the *objective_function* argument to the **Population** constructor (lambdas work too!).
Note that this is the first argument to the constructor, so both of the following will work:
```python
population = fmga.Population(f, population_size=60)
population = fmga.Population(population_size=60, objective_function=f)
```
If you wish to define custom boundaries, create a dictionary of keys, and store a tuple of elements:
```python
boundaries = {}
boundaries[0] = (-50, 50)
boundaries[1] = (20, 200)
...
```
and pass this to the **boundaries** argument to the **Population** constructor:
```python
population = fmga.Population(f, population_size=60, boundaries=boundaries)
```
Note that the default range for missing dimensions is (0, 100).  
The population can be set to breed and iterate by using the **.converge()** method.
```python
population.converge(iterations=20)
```
To perform only one iteration of breeding and mutating, do:
```python
population.iterate()
```
Access population mean fitness and mean L1 diversity stats through the _.mean_fitness_ and _.mean_diversity_ attributes:
```python
print(population.mean_fitness, population.mean_diversity)
```

The **.best_estimate()** method returns the point closest to the function point of maxima in the population, as a **Point** object.
```python
best_point = population.best_estimate()
```
Every **Point** object has the __coordinates__ attribute, a numpy array signifying the coordinates of point.
```python
print(best_point.coordinates)
```
To find the value of the function at this point, use:
```python
print(best_point.fitness)
```

## Population Class Methods
The Population constructor takes the following arguments, in order:

**objective_function** The function to maximize!  
**population_size** (default = 60) Number of points in the population.  
**boundaries** (default = (0, 100) for every dimension) Must be a dictionary, with keys (representing dimensions) varying from 0 to 
number of dimensions - 1. For every key, the tuple of 2 elements represents the domain where the points are spread along that dimension.    
**elite_fraction** (default = 0.1) Fraction of the population's points to be kept as elite during breeding. Must be between 0 and 1, inclusive.  
**mutation_probability** (default = 0.05) How likely is is for a single point to mutate - this probability is the same for all points in the population.
Must be between 0 and 1, inclusive.  
**mutation_range** (default = 5) The range of the mutation when it does occur. Note that the point will never mutate out of the domain defined!  
**verbose** (default = 2) How much output to be displayed when iterating population after population. Must take values 0, 1 or 2 with 2 representing the most output, and 0 representing none.
