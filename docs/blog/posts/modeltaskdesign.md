---
date:
  created: 2025-12-29
  updated: 2025-12-29
authors:
  - jmframe
---

# What does your model do?

In Earth Sciences we build all sorts of models for one reason or another. I've recently gotten into several conversations where we discuss an aspect of modeling that breaks down what we want, what we ask for, and what the model was actually built to do. In an ideal scenario, all three of these would be the same. But, in reality, this is never the case. We need to build models sometimes only as a stepping stone to our desired goal. Sometimes that stepping stone has to be shakey in order to get to where we want to go. But one important thing about modeling, is the model is NEVER the end goal, or at least it shouldn't be. I worry sometimes that models are being build for their own sake. If you are having a hard time connecting the dots of how your model will be used, this blog is for you. 

We can visualize this problem as a 3D coordinate system. On the X-axis, we have how well-defined our task is. On the Y-axis, we have how achievable it is. We might have a Z-axis: Alignment. If the circles of our intention don't overlap, we end up successfully completing a task that fails to solve the actual problem. Let's break every modeling project into three distinct items:

  - Goal: What do you actually want? (The "Why")
  - Task: What did you ask the model to do? (The "How")
  - Design: What was the model designed to do? (The "What")

When these three are in direct overlap, you have a useful tool. When they drift apart, you have a "Success-Failure": a model that does exactly what you told it to, while not bringing you any closer to your end goal.


### Example 1: Should I bring my umbrella?
This is an example of an ideal alignment of goal, task and design in modeling. You want to stay dry on your walk to work. You use a short-range weather model to see if it will rain in the next hour.

  - Goal: Bring umbrella on walk.
  - Task: Forecast weather 1 hour out.
  - Design: Short-range forecasting.

In this scenario, the definitions are precise and the task is highly achievable. The circles overlap almost perfectly. If the model says "rain," and you grab the umbrella, your goal is met.

<p align="center">
  <img src="https://github.com/DeepGroundwater/DeepGroundwater.github.io/blob/modeltaskdesign/docs/blog/posts/pics/taskumbrella.png?raw=true" alt="Should I bring an umbrella to work" width="500"/>
</p>


### Example 2: Do I need a jacket on my trip next week?
Now, let’s stretch the temporal scale. You’re packing for a trip next week. You ask a mid-range model for a forecast.

  - Goal: Pack for upcoming trip.
  - Task: Forecast weather 1 week out.
  - Design: Mid-range weather forecasting.

Here, the Definition is still high, but Achievability starts to drop. Chaotic atmospheric dynamics mean that a 7-day forecast is less reliable than a 1-hour one. The overlap between your Task and your Goal shrinks. You might pack shorts based on a "Sunny" forecast, only to arrive in a cold snap. The model did its "Task," but the "Goal" (being prepared) was undermined by the inherent limits of the "Design."

<p align="center">
  <img src="https://github.com/DeepGroundwater/DeepGroundwater.github.io/blob/modeltaskdesign/docs/blog/posts/pics/taskpack.png?raw=true" alt="What should I bring on my trip" width="500"/>
</p>


### Example 3: Solving Flooding (The Alignment Trap)
This is where things get dangerous. We often set out with a massive, virtuous goal, such as eliminate flood hazards. But how do we translate that into a computational task? Sometimes, we produce a model, validate the results, then call it a day, even though this won't neccessarily get us closer to our goal.

  - Goal: Eliminate flood hazards.
  - Task: Produce 100-yr flood map.
  - Design: Inundation from streamflow.

The Task is extremely well-defined (X-axis) and generally achievable (Y-axis). But the Alignment with the Goal is poor. A 100-year flood map tells you where the water goes in a massive, rare event. It doesn't tell you about the 20-year flood that washes out a low-water crossing, or the flash flood that overwhelms a storm drain.

If we use the map as our sole development strategy, we might "pass" our task by keeping houses out of the 100-year zone, but "fail" our goal because people are still dying in their cars on "safe" roads.

<p align="center">
  <img src="https://github.com/DeepGroundwater/DeepGroundwater.github.io/blob/modeltaskdesign/docs/blog/posts/pics/taskflood.png?raw=true" alt="Let's solve flooding" width="500"/>
</p>

We should strive for as much overlap as possible between our end goal, model design, and model task. This is extremely hard, however. We should at least be transparent about our task alignment.

We can continue improving our models to give us better "Tasks" (better hydrograph matching), but if our "Goal" is ecosystem health or water equity, we can't assume that a better-fitted line on a graph automatically translates to a better outcome in the real world.

I recommend that next time you are working on a model, you think through this alignment excercize.

