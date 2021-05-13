# Assigment 2 steps and procedure

### Network 
We have used the same network that is discussed in the network 

### basic relationships and fromulas 
The formula of every node and output of activation unit was written in terms of input, weights
and output of nodes. Attaching screenshot for reference

### partial derivatives 
to update the weights the parital derivates of the total error with respect to each weight needs to 
calculated using the below formulas 

### backpropogation 
We use the derivatives calculated in the above step to update the weights using the equation below 

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
- LR = 0.1
- LR = 0.2 
- LR = 0.5 
- LR = 0.8
- LR = 1
- LR = 2
- LR = 10
- LR = 100 
- LR = 10000000

### Inferences
- from the graphs in the result section, we might infer that the higher that learning rate the faster the eT converges
to minima. but if the learning rate is extremely high (10^7) eT fails to converge to the minima 



 
 



