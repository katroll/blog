---
title: "Javascript array.reduce() Method"
date: 2022-01-05T16:30:47-05:00
draft: false
---

The past few weeks I have been consistantly forgetting the syntax for the array reduce method in Javascript. After googling it a number of times, I thought it might be helpful to write out a method for it here.  

***

##### **Why is the **reduce()** method used?**

It is used to interate over an array and return one value from a calculation done on each value.  

For example, given the array of:  

    const arr = [1,2,3];

We can use `arr.reduce()` to sum the array values and return 6.  

***

##### **How is the **reduce()** method implemented?**

To begin, we will go over the different peices needed for the `reduce()` method.

The `reduce()` method takes in two paramerters:
1. A callback function, that holds the functionality of the method.
2. An initial value to start the calulation at (this is optional).  

The callback function also needs to have two values passed to it:
1. The current value of the calculation accumulator that will be returned from the method.
2. The value of the current index of the array as the method iterates.

The body of the callback function will be the desired calculation. For example, to sum the values of an array of integers, the callback function could look something like this:

    (accumulator, currentIndex) => accumulator += currentIndex

Here, for every value of the integer array, the value of each array index is being added to the accumulator.

To put all of this together, the `reduce()` method to sum the above array would look like this:

    const arr = [1,2,3];
    arr.reduce((accumulator, currentIndex) => accumulator += currentIndex, 0);
    // expected return value: 6


The zero added as the secoond arguement of `reduce()` sets the acculmulator to start at zero, and the first `currentIndex` value is 1. Without adding the 0 as the initial value of the count, the inital value of the accumulator becomes the first index (1) and the first value of `currentIndex` becomes the second index (2).

***

##### A More Complex Example

In a project I am working on now in the second module of the bootcamp focusing on React, I encountered an array of objects with many keys that I needed to sum only one value from. The array looked like this (with many more objects in the array): 

    const flightHistory: [
            {
            "date": "2021-12-29T17:59:07.008Z",
            "passengers": 2,
            "departure": "DIA",
            "destination": "BOS",
            "carbon_lb": 6678.45,
            "id": "33a9f35f-903e-43ce-ad22-6c9ddaa55805"
            },
            {
            "date": "2021-12-29T23:13:55.833Z",
            "passengers": 2,
            "departure": "BDL",
            "destination": "BCN",
            "carbon_lb": 3871.04,
            "id": "4dd1a601-41e4-48b9-aca7-a4dbffbe1041"
            }
    ]

My goal was to sum the estimated carbon emission from each flight, for just one person. 

The calculation would need to divide the carbon emission (stored in `carbon_lb`) by the number of passengers (stored in `passengers`). To do so I did some reading and testing functions out and used the `reduce()` method on this array of objects. It ended up looking like this: 

    const totalFlightCarbon = flightHistory.reduce(
        (count, flight) => (count += flight.carbon_lb / flight.passengers), 0); 
        

Here, `count` is the accumulator variable and `flight` is each object in the array. As `reduce()` iterates over each object it can access desired keys and perform complex calculations, all do be returned in the `count` variable at the end. This makes `reduce()` a valuble method to be used on arrays and one worth being comfortable with.


