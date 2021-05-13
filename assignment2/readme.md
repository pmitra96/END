# Assigment 2 steps and procedure

### Network 
We have used the same network that is discussed in the network 
![Image](https://github.com/pmitra96/END/raw/main/assignment2/images/2_layer_network.png)

### basic relationships and fromulas 
The formula of every node and output of activation unit was written in terms of input, weights
and output of nodes. Attaching screenshot for reference
![Image](https://github.com/pmitra96/END/raw/main/assignment2/images/basic_relationships_2.png)

### partial derivatives 
to update the weights the parital derivates of the total error with respect to each weight needs to 
calculated using the below formulas 
![Image](https://github.com/pmitra96/END/raw/main/assignment2/images/partial_derivatives.png)

### backpropogation 
We use the derivatives calculated in the above step to update the weights using the equation below 
![Image](https://github.com/pmitra96/END/raw/main/assignment2/images/learning_formula.png)

### initial seed values 
we use the same initial seed values and the weights of those discussed in the lecture

### learning 
- we incorporate the formulas discussed for weight updation and calculation of values are the nodes 
discussed in the all the previous steps 
- then for the second row (i.e after one epoch) we incorporate the weight updation formula 
```
(Wnew = Wold - Learning_Rate*(deT/dWold)) 
```
inside the excel cells.

- we extend the same formulas in 1st and 2nd rows to the next 51 rows

### Results
we plot the eT vs epoch graph for the following learning rates
- LR = 0.1 ![Image](https://github.com/pmitra96/END/raw/main/assignment2/images/et_vs_epochs_with_LR%3D0.1.png)
- LR = 0.2 ![Image](https://github.com/pmitra96/END/raw/main/assignment2/images/eT_vs_epochs_with_LR%3D0.2.png)
- LR = 0.5 ![Image](https://github.com/pmitra96/END/raw/main/assignment2/images/eT_vs_epochs_with_LR%3D0.5.png)
- LR = 0.8 ![Image](https://github.com/pmitra96/END/raw/main/assignment2/images/eT_vs_epochs_with_LR%3D0.8.png)
- LR = 1 ![Image](https://github.com/pmitra96/END/raw/main/assignment2/images/eT_vs_epochs_with_LR%3D1.png)
- LR = 2 ![Image](https://github.com/pmitra96/END/raw/main/assignment2/images/eT_vs_epochs_with_LR%3D2.png)
- LR = 10 ![Image](https://github.com/pmitra96/END/raw/main/assignment2/images/eT_vs_epochs_with_LR%3D10.png)
- LR = 100 ![Image](https://github.com/pmitra96/END/raw/main/assignment2/images/eT_vs_epochs_with_LR%3D100.png)
- LR = 10000000 ![Image](https://github.com/pmitra96/END/raw/main/assignment2/images/eT_vs_epoch_with_LR%3D10000000.png)

### Inferences
- from the graphs in the result section, we might infer that the higher that learning rate the faster the eT converges
to minima. but if the learning rate is extremely high (10^7) eT fails to converge to the minima 



 
 



