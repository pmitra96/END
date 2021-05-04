## Questions 

- What is a neural network neuron?
    - A neuron is is a fundamental part of any neural network,
      which is responsible for computing weighted average from the input signals 
      and then applying the an activation function on top of it which serves as an input 
      to the next layer of neurons.
      
- What is the use of the learning rate?
    - the learning rate controls how quickly the model is adapted to the problem.if the learning rate is high 
      after each epoch , the difference between new weight and the old one is high and vis-a-vis.

- How are weights initialized?
    - we can initialize weights by generating random numbers from 0 to 1.
      there are couple of widely followed techniques to initialize the weights namely He initialization, Xavier initialization which 
      which basically generate a random number from 0-1 and apply a function on top of that number. these techniques prevent all the 
      weights to be either too high or too low which will lead to the "vanishing gradient problem". 
      
- What is "loss" in a neural network?
    - a loss in a neural network is the difference between the actual output and the predicted output of the model , which we use to compute gradients which inturn 
      is used to train the model by updating weights and biases.
      
- What is the "chain rule" in gradient flow?
    - to determine the effect of a certain weight in the output , we use partial derivatives to compute the same. 
      in the flow contribution1 = f(w1) output = g(contribution1)  
      so d(output)/d(w1) = d(output)/d(contribution1)*d(contribution1)/d(w1) 
      
      
