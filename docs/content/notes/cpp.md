+++
+++
What I learned when using C++
===============

Recently I started  using the C++ programming language since I am learning the Udacity
Self-Driving-Car Nanodgree program Term2 and one of the projects is implementing an 
extended Kalman Filter in C++. I am writing to document the tricky part of C++ here that
I learned.

# Not Dividing by zeros
Like in pure math and any other programing languages, dividing by zero is always a probaylem,
So we should avoid dividing by zeros. We probably will not write code like `some_variable / 0;` 
, the issue is that dividing by zeros will happen in functions we called if do not pay attention 
to the parameters we passed in, for example, in `py = 1; px = 0; atan2(py, px)`, a dividing by 
zeros error will occur.

# Pay attention to data types
In my implementation of the extended Kalman filter, the result is not as expected in the 
beginning and I found that the issue arise from my calculation for  $$\Delta t$$ in the 
following code.

`double dt = (current_timestamp - previous_timestamp) / 1000000`

here both current_timestamp and previous_timestamp are long and their difference is 
always less than 1000000 since the measurement comes one after another within very short 
time, as a result dt will be zero always, this causes the kalman Filter to predict exactly 
the same state as the current and thus not doing a good work.

After I changed the code as the following, things start to work.

`double dt = (double)(current_timestamp - previous_timestamp) / 1000000`

or 

`double dt = (current_timestamp - previous_timestamp) / 1000000.0`

please notice that

`double dt = (current_timestamp - previous_timestamp) / 1000000L`

is not working.





