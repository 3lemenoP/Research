Thankyou Jack for the feedback.
Their research site: https://elsworth.phd/
X: https://x.com/theproteindance

------

This paper describes a method of connecting the concept of self-regulation with broader abilities to perform inference. The methods presented are connecting the free energy principle, a bayesian method of minimising surprisal, with a viability kernel that defines a set of states from which one is able to maintain self propagation. 

The theory has broad applicability between any domain wherein one can identify a "viable state" that needs to be maintained and perform inference on sensory input data. This allows for any system from electric cars, energy grids, biological, or organisational systems to be modelled with regards to how to maintain the system itself and strategising to continue taking these paths. Essentially this means that you can embed an impulse for survival into the free energy principle by marking each non-viable state as highly surprising while allowing for short term forays into less-optimal situations for long-term stability.

The mathematical treatment uses an augmented function using $\lambda \mathbb{E}_q[\phi(x(t))]$ with $\phi(x)$ acting as the penalty that is zero for viable states and positive outside. Although this is then extended into a path dependent analysis of surprise and safety for the future states. This creates a path dependent expectation of future state surprisal via long term cost $J$.  $\rho$ is mentioned briefly, but not really considered in how it can effect the systems being modelled or how they relate to the parameters of said systems. What does "discount factor for futurity" mean?

There is mention of how to derive a critical adaptation rate or Nyquist stability criterion, but this is never explicitly addressed so it feels odd to say it "grounds the framework". Indeed, the inclusion of mentions of Markov blankets, while relevant, do not feel appropriately developed to justify it's inclusion.

One thing I was wondering about was the lack of mention of proportional-integral-dervative (PID) controllers which feel like a natural inclusion and fertile testing space. They are historically relevant, provide a direct comparison between "traditional controllers" and this new theory. The ability to show how this new theory is able to perform in practical systems as compared to the PID would be useful, in fact a treatment of PID under the free energy principle would provide significant explanatory power. 

There is mention of resilience and perturbations of the model, but not really a discussion of what those mean. There is mention of "goals" but there is not sufficient mention of how those goals can interrelate and frequently subsume each other and even the survival of the agent, such as in countless biological occurrences ranging from multicellularity to colonies. 

The examples, while extensive and interesting beg the question of what CSF provide that are not already provided by simpler models with fewer assumptions. There are descriptions of possible experiments but a lack of clear experimental design. I think the smart grid example would be the most appropriate one to model and compare to current modern theories.

This system seems like it would locally cohere and I can be convinced of the group dynamics it describes, the drone example was particularly salient here. However, I still wonder about black swan events and the like, if we have a system that is consistently minimising surprisal and viability then the penalty on exploration would be high enough that information gathering which protects against long-term extinction might be avoided. Phase analysis on how these two competing drives are optimised would clarify the picture of CSF.

During implementation examples there are multiple methods mentioned that are cast as being equivalent to CSF without much fan fair. Indeed the entire section gets a bit dense with the flow being a bit hard to get into. 

I found the section on hierarchical systems lovely, however the use of fractal while metaphorically evocative seems to be mathematically imprecise, based on the descriptions it sounds more like a recursive hierarchy or self-similar nested systems. Regardless I find this is where this theory should provide a lot of predictive power if fully applied and I'm a bit sad there isn't more work on that besides the car example. 

The treatment on competing desires of sub-systems and super-systems was a bit sparse. These sorts of things are vital if one wants to apply this to a whole system, even in biology as coordinated as it can be in a human body, there is a natural case of cancer and reproductive cells/children which directly violates the hierarchy. In many other cases this will also remain true, such as a company, where for most workers it does not matter if the company gets a 2% increase in profit if their family starves and they never see their daughters soccer games. 

Structure-wise there are a lot of dense paragraphs that could do with being broken up into more discrete units and sections. I liked the usage of bolding and italics to help direct attention, but ideally they would not need to be as necessary. The inclusion of tables and pictographic representations of the work would significantly help smooth some of the denser parts in terms of explanations.

Support-wise there are not nearly enough citations, the one I did find, (Rockstrom et al.) was not formatted in a way that allowed me to determine exactly what you were referencing. Many different possible studies are mentioned but none are actually analysed in detail. What predictions can be made from this model that are not already sufficiently covered by current theories? How can we test those theories? From my reading neither of those questions is appropriately answered. The mathematics considered span a large number of concepts but do not explicitly show how they relate to the CSF using equations.

Overall this is a really compelling system and my personal love of free energy and kernels find this framework appealing. By reducing the scope of the paper and focusing on a few examples that are extensively treated and modeled, while grounding the system in existing systems (like PID) for comparison there is truly something special that I think can arise and I look forwards to reading more. 

Typos:
1. In the first equation $D_{KL}$ is written as $DKL$, also it appears to be duplicated
2. Is the equation for $J$ supposed to have a comma in it?
3. Model Predictive Control is mentioned once so an acronym is unneeded
4. There is an extra * before "Research in theoretical neuroscience"