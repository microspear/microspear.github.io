---
layout: post
title: "Neural Network for Stupid People"
description: "Trying to make sense of ANN without knowing shit!"
og_test: "https://abhikmitra.github.io/machineLearningForStupidPeople/assets/documentation/neural-networks.jpg"
tags: [machine Learning, neural networks, deep learning]
---
> I am a stupid guy.

I have been doing the [course on Deep Learning](http://course.fast.ai/), where the author does a great job of teaching. While doing the course I  found a dearth of 'easy' material on the web regarding neural nets. 
Here I am trying to collate what I saw across multiple learning resources and try to make sense of it. This is not an article that you should see if you have not tried other materials. Once you read other materials and get confused , come back here :) 

So an excellent way to start understanding neural network is with this [video](https://youtu.be/e3aM6XTekJc?t=3810 "Neural Networks in Excel") where Jeremy uses Microsoft Excel to make a Deep Neural Network. Here are the [notes](http://wiki.fast.ai/index.php/Lesson_2_Notes "Neural Networks in Excel") if you dont like videos

<img src="http://wiki.fast.ai/images/e/e2/NN_spreadsheet_input_output.jpg" 
description="lil" 
title="The values in the purple circle belong to our input vector, and the values in the yellow circle belong to our target vector y. We want to perform a series of matrix products to get as close to the target vector y as possible" 
alt="The values in the purple circle belong to our input vector, and the values in the yellow circle belong to our target vector y. We want to perform a series of matrix products to get as close to the target vector y as possible" width="400"  border="10" />
Our target would be take a more real example and try to fit it inside his excel model for our better understanding.

##### Problem statement

> For all guys aged 27 , given the hour of exercise and sleep , estimate their metabolic age ( metabolic age is a metric used health clubs to figure out whether your body is functioning like a 18 year old or a 40 year old) .

##### Data Set

| Exercise (hours)       | Sleep  (hours)        | Metabolic Age (years) |
| --------------- |:-------------:| --------------:|
| 2          | 8        |   27          |
| 3          | 7        |   23          |
| 4          | 6        |   22          |

This is the data we are going to use to train our network. 


| HE   | HS      | MA |
| --------------- |:-------------:| --------------:|
| 2          | 8        |   27          |

{% include image.html path="assets/documentation/excel_1.png" path-detail="assets/documentation/excel_1.png" alt="Excel" %}

Which basically means

{% include image.html path="assets/documentation/neural_network_blackbox.png" path-detail="assets/documentation/neural_network_blackbox.png" alt="Excel" %}

### Step 1 : Standardize Units
___
As we see, the input is in hours and the output is in years. Neural network can have inputs like a 220 x 220 and an output could be a category ( dog, human , chair , etc) .
 So you can see the input and output units are completely different. 
 
So the most common approach is to scale the inputs and outputs between 0 and 1

> HoursOfSleep(HS) = HS / max(HS)

> HoursOfExercise(HE) = HE / max(HE)

> MetabolicAge(MA) = ME / max(MA)

lets Assume max(HS) = 12 , max(HE) = 6, max(MA) = 50

So the input now becomes , 

| HE   | HS      | MA |
| --------------- |:-------------:| --------------:|
| 0.33        | 0.67       |   0.54          |

### Step 2 : Identify the problem
___

Lets try to identify the class of problem

> Does the data set have inputs and outputs ?

 Its a supervised problem

> Is the output data continuous? 

 Yes. the value can be anything between 0 to 1 . And as we know there are infinite values between 0 and 1. If the output would be specific data like "FIT" , "UNFIT" then it would be a classification problem

### Step 3 : Design the Neural Network
___

Our goal here is to just understand, so I just put in a random arbitrary network that I got from [here](http://karpathy.github.io/neuralnets/) 

The networks really dont make much sense here since all the inputs are positive.

{% include image.html path="assets/documentation/neural_network_design_1.png" path-detail="assets/documentation/neural_network_design_1.png" alt="neural_network_design" %}

Lets define some terms

1. HE and HS are called Input Layer or input neurons
2. All the arrows ( except the last one giving MA) are called synapses
3. Each synapse adds a specific weight to the Input - which means it multiplies the input by a number called weight
4. The middle layer of 3 empty circle is our hidden layer or hidden neurons

> What is a Neuron ? 

Its a non linear function. Thats all. Really !

Let that non Linear function be ReLU which is a fancy term for

f(x) = max(0, x)


So what does our Neural Network look like now ?

{% include image.html path="assets/documentation/neural_network_design_final.png" path-detail="assets/documentation/neural_network_design_final.png" alt="neural_network_design" %}

In equation it looks like 

>  n1 = Math.max(0, WE1 * HE + WS1 * HS )

>  n2 = Math.max(0, WE2 * HE + WS2 * HS )

>  n3 = Math.max(0, WE3 * HE + WS3 * HS )

>  MA = WA * n1 + WB * n2 + WC * n3

> What are the responsibility of the neuron 
    

1. Receive all the Synapses
2. Sum all the synapse
3. Apply the activation function (Which is ReLU in ur case)


If we want to view this as a series of matrix multiplication , this is how it will look like 

{% include image.html path="assets/documentation/neuron_toplevel.png" path-detail="assets/documentation/neuron_toplevel.png" alt="neural_network_design" %}

### Step 4 : Train the network
___

Training the network basically means adjusting the value of the weights, in our case it is WE1 , WE2, WE3, WS1, WS2 , WS3 , WA, WB and WC. 
We input the given values HE and HS and check the output if MA is correct. So if HE and HS is 0.33 and 0.67 respectively, a number close to 0.54 is what we would expect. 
If not we tune the values of the weights till we get a reasonably good result.

> How do we define a reasonably good result ? 

We define it by the loss function (cost function) . In our case it can be a simple  (correct_MA - predicted_ma)^2 . We have to try and minimize the function

> How do we minimize the function ?

By taking the derivative of the LostFn with respect to each of the weights. (Partial Derivative) . You just need to remember why we needed derivation and not get bogged down by the rules


Going Forward 

There are still lot of stuff to learn like

1. SGD
2. Back propagation
3. Bias

But now you have a model of Neural Network in your mind. Having this model helps you visualize the math that is going to come . 

### The End
---

If you spot and mistake in the above writing , it probably means there is some mistake in my understanding and it will help me rectify it 