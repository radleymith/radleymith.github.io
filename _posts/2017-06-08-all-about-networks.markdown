---
layout: post
title:  "Unit testing state changes in React without Sinon or TestDouble"
date:   2017-06-25 19:29:00 -0700
categories: React, Unit, Testing, Unit Testing, JavaScript, Redux, Sinon, TestDouble, Node.js, Enzyme
comments: true
---
## Problem Statement
This came about because I'm working in an environment where a lot of false positives are thrown as dependency conflicts.  So the problem is that I get dependency conflicts, a lot of them, when I include a new library and this causes the build to fail.  So I'm trying to minimize the use of dependencies that aren't vital.

##Setup
This is for a React app not using Redux.  Redux is awesome!!!, but sometimes not necessary.

## The test
You don't need Sinon or TestDouble in order to unit test React components that mutate the state.  Use the `mount` function of `enzyme` to create an `instance()` of a component.  Then hook into the `render()` function of the instance and execute the test inside the render function checking if the current state matches the expected state.  It should look something like:

~~~ javascript
describe('theFunctionYouAreTesting functionality', () => {
    it('should do something that updates state', () => {
        const initialState = { some, state, values, };
        const expectedState = { what, the, state, should, be, after, function, call, };
        const wrapper = mount(<TheComponentBeingTested />);

        wrapper.setState(Object.assign({}, initialState, { more, state, settings, }));

        const instance = wrapper.instance();

        instance.render = function () {
            expect(this.state).to.deep.equal(expectedState);
        };
        instance.theFunctionYouAreTesting(inputToSaidFunction);
    });
});
~~~

Now when that test is run it will mount the wrapper.  Then it will set the state of the wrapper.  Then it will give you an instance of the wrapper with that state set.  Then you will override the render function of that instance and write your assertion in there.  Then it will execute the function which will mutate the state, causing a re-rendering of the mounted component which will then execute the assertion inside that render function.

Now you may ask, "But if I'm changing the state why won't I just test the state immediately after the function runs."  Well you could try but it won't work.  The state updates asynchronously so if you try and pull that instances state it will look just like it did before the function you are testing ran and not what you were expecting the state to be.  This will cause the test to fail incorrectly.  Once react picks up the state change, it will re-render the component and then you can safely check the state inside the render function.

##Disclaimer
This is a naive solution to my problem statement.  The better solution would be to find a better build system or augment the one I'm using to stop the false positive dependency conflicts.

Also this solution is good for smaller apps where you don't really need all of the bells, whistles, and overhead that come along with TestDouble and especially Sinon.