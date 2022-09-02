# Chapter 3 - A Framework for System Design Interviews

The system design interview simulates real-life problem solving where two coworkers collaborate on an ambiguous problem and come up with a solution. The problem is open-ended and there is no perfect answer. The final design is less important than the process used to get there.

Primary goal of the interviewer is to assess your abilities. It is much more than just technical abilities:

- :white_check_mark: Ability to collaborate, work under pressure and resolve ambiguity
- :white_check_mark: Ability to ask good questions
- :triangular_flag_on_post: Over-engineering and unawareness of the compounding cost of over engineered systems.
- :triangular_flag_on_post: Narrow mindedness, stubborness. 

## A four step process for an effective interview

### Step 1 - Understand the problem and establish design scope

Giving out an answer quickly without thinking gives no bonus points. Answering without a thorough understanding of the requirements is a huge :triangular_flag_on_post:. We need to ask the right questions to ensure we're making the proper assumptions:

- What specific features are we going to build?
- How many users does the product have?
- How fast does the company anticipate to scale up? What are the anticipated scales in 3 months, 6 months and a year?
- What is the company's technology stack? What existing services you might leverage to simplify the design?

## Step 2 - Propose high level design and get buy-in

Aim to develop a high level design and reach agreement with the interviewer on the design.

- Initial blueprint. Ask for feedback and incolve the interviewer
- Draw box diagrams with key components including the clients, APIs, web servers, data stores, cache, CDN, message queue etc.
- Do back of the envelope calculations to evaluate if the blueprint fits the scale constraints. Think out loud.

If possible go through a few concrete use cases. This will help you frame the high level design and discover edge cases.

## Step 3 - Design deep dive

By this stage we've:

- Agreed on the overall goals and the feature scope
- Sketched out a high-level blueprint for the overall design
- Obtained feedback on the high level design
- Initial ideas about areas to focus on in the deep dive

Different interviewers will have different preference on which components to focus on. Some examples could be:

- System performance charactersitics, focussing on the bottlenecks and resource estimations.
- Focus on a particular algorithm
- How to reduce latency for a service or extended features

## Step 4 - Wrap up

Interviewer will ask follow up questions and discoss other points:

- Identify system bottlenecks and discuss improvements.
- Useful to give a recap
- Error cases (server failure, network loss, etc.)
- Operational issues - how to monitor metrics and error logs, rollout
- How to handle the next scale curve

## Time allocation

- Step 1: 3 - 10 minutes
- Step 2: 10 - 15 minutes
- Step 3: 10 - 25 minutes
- Step 4: 3 - 5 minutes
