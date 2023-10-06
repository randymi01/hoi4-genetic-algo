# Motivation

The air war is a crucial part of winning any Hoi4 game. Air superiority reduces enemy breakthrough and defense as well as allows your CAS operate without interference. At the core of winning the air war is the development of an effective fighter design.

The release of By Blood Alone has increased the complexity of plane design. As a result we've seen the proliferation of a several metas. However to date, no work has been done to empirically determine an optimal fighter design. The primary challenge lies in the insane number of possible combinations of fighter modules. For fighters, there are 8 possible weapons modules and 11 possible defensive modules, leading to hundreds of thousands of unique fighter designs. Additionally, a weight limit exists, wherein the total weight of the plane must not surpass the thrust of the engine. 

This type of module selection and weight constraint mirrors combinatorial optimization problems such as the [Knapsack Problem](https://en.wikipedia.org/wiki/Knapsack_problem) in Computer Science. [Genetic algorithms](https://en.wikipedia.org/wiki/Genetic_algorithm) have demonstrated successes in solving these types of problems as they strike a balance between the exploration and exploitation of the solution space, and is the method I've implemented.

# Assumptions
1. Improved Fighter
2. Agility Air Designer
3. Engine III
4. Modules:

![alt](https://github.com/randymi01/hoi4-genetic-algo/blob/main/image-3.png?raw=true)

![alt](https://github.com/randymi01/hoi4-genetic-algo/blob/main/image-5.png?raw=true)

![alt](https://github.com/randymi01/hoi4-genetic-algo/blob/main/image-4.png?raw=true)

# Implementation
The air designer was replicated in Python by parsing the various values associated with each module and the airframe. Subsequently, 100 valid fighter designs were generated using randomly selected modules. In each stage or generation of the genetic algorithm, the 100 designs underwent simulated engagements against each other, employing the air combat damage formula sourced from the wiki. These engagements were weighted by Industrial Capacity (IC) and assumed a 900km range, representing 100% air zone coverage, which is approximately the coverage needed for most air zones in Europe.

Each design was then evaluated based on its performance in trades with the other 99 designs. The resulting score determined the probability of a design being selected as a parent for the next generation. Initially, the top two scoring designs advanced to the subsequent generation. Following this, a pair of parent designs was selected, and a child design was created by combining modules from both parents. A 10% chance of mutation existed, whereby a random module could replace one of the child's modules. The finalized child design was then introduced into the next generation. This iterative process continued until the next generation was populated, and the cycle repeated until convergence.

# Results
Here's a few bar charts of the frequency of each module at selected generations. As you can see, suboptimal modules like non-strategic materials and LMGs are removed from the population with the frequency of optimal modules being amplified.

![eq](https://github.com/randymi01/hoi4-genetic-algo/blob/main/Equipment_Distribution_1.png?raw=true)

![eq](https://github.com/randymi01/hoi4-genetic-algo/blob/main/Equipment_Distribution_10.png?raw=true)

![eq](https://github.com/randymi01/hoi4-genetic-algo/blob/main/Equipment_Distribution_30.png?raw=true)

In order to test the output of the model, the best design from each generation was pitted against a test fighter. For testing, I used the current meta fighter which has the following stats:

* weight: 22
* cost: 39
* range: 720.0
* speed: 629
* agility: 64.9
* air_attack: 44
* air_defense: 25
* max_thrust: 30
* weapons: ['can1_2x', 'hmg_4x', 'hmg_4x']
* defenses: ['armor_plates', 'armor_plates', 'self_sealing']
* Engines: Single 

Below is a graph representing the IC trade of the algorithm's fighter with the testing "meta" fighter (how much IC of the testing fighter is needed to equal 1 IC of the algorithm fighter, higher is better): 
![eq](https://github.com/randymi01/hoi4-genetic-algo/blob/main/Test_Plane_Performance.png?raw=true)

The algorithm converges at generation 20 with the following **most optimal fighter design** which IC adjusted, out-trades the testing meta fighter by 13%:

* weight: 30
* cost: 44
* range: 720.0
* speed: 605
* agility: 58.3
* air_attack: 56
* air_defense: 25
* max_thrust: 30
* **weapons: ['can1_2x', 'can2_2x', 'hmg_4x']**
* **defenses: ['armor_plates', 'armor_plates', 'self_sealing']**
* Engines: Single


## Code
[Jupyter Notebook](https://github.com/randymi01/hoi4-genetic-algo/blob/main/main.ipynb)


## License

[MIT](https://choosealicense.com/licenses/mit/)
