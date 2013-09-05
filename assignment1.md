Assignment 1: Introduction to node.js and JavaScript
====================================================
For this first assignment, your goal is to set up node.js and npm on your system, and to get familiar with the basics of how to use modules.  The intention here is to just focus on understanding the basics, and so it is going to be a little more structured than usual.  Also unlike in future assignments, this assignment will be broken up into stages.

# Stage 1  (due 9/12/13)
In the first stage, your goal is to implement an extremely simplified physics simulation.  You will be modeling the motion of a collection of non-interacting moving particles in a box.  You can assume that there is no friction and no external forces (ie gravity).

To do this, you should make a module which defines a function that integrates the state of the system forward by some time step.  You should export this function as a module.  Here is a method signature to help get you started

```javascript
module.exports = function(position, velocity, ground, dt) {

  var nextPosition = new Array(position.length)
  var nextVelocity = new Array(velocity.length)
  
  // Compute next state here

  return {
    position: nextPosition,
    velocity: nextVelocity
  }
}
```

The arguments for the method are as follows:

* `position` is an array of length 2 arrays of numbers representing the position of each particle
* `velocity` is an array of length 2 arrays of numbers representing the velocity of each particle
* `ground` is a length 2 array of numbers representing the normal direction for the bottom of the box
* `dt` is the amount of time to step the simulation forward


For each particle, you should update the system using the rule that:

```javascript
position[i] += dt * velocity[i]
```

If a particle hits one of the sides of the box or crosses the ground plane, then the position is integrated up to the time of intersection and the velocity is reflected by the normal of the plane.  The shape of the box is determined by the following system of inequalities:

* `-1 < x < 1`
* `y < 1`
* `ground[0] * x + ground[1] * y > 0`

You can assume that:

* The position of all particles will be within the box
* The second component of the ground normal will always be less than 0.

In this project you can use any modules on npm that you would like, or if you prefer you can also write and publish your own modules if you think they will help you with this assignment.

## Deliverables
For the first part of this assignment, you should submit an archive that has the following things:

* All the code you wrote to make your project work (excluding code within node_modules/)
* A `package.json` file that lists all dependencies within your project and has a reference to the entry point to your project.
* A collection of test cases contained in the test/ folder

## Testing
To verify that your modules is working as intended, you should write at least a couple of test cases.  To get you started, here is a simple example:

```javascript
var assert = require("assert")
var stepSimulator = require("./mysimulator.js")

var position = [[0, 0.5]]
var velocity = [[0, 1.0]]
var ground   = [0, 1]
var dt       = 0.1

for(var t=0.0; i<1.0; t+=dt) {
  var nstate = stepSimulator(position, velocity, ground, dt)
  position = nstate.position
  velocity = nstate.velocity
  assert.equal(position[0], 0.0)
  assert.equal(position[1], 1.0 - Math.abs(t - 0.5))
}
```

You should write at least two different test cases of your own to supplement this.
