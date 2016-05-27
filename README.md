# There Ain't No Such Thing as a Free Build

Modern software is developed at a rapid pace. Last May (2015), Mozilla processed [8,363 updates to the codebase](http://relengofthenerds.blogspot.ca/2015/06/mozilla-pushes-may-2015.html) (roughly 270 updates per day!) The widespread adoption of techniques like Continuous Delivery (CD) accelerates the rate at which these changes become visible to users. Google, LinkedIn, and Facebook release [several times daily](http://dx.doi.org/10.1109/MS.2015.52 "Adams et al., The Practice and Future of Release Engineering: A Roundtable with Three Release Engineers, IEEE Software"). In May 2011, Amazon engineers deployed new releases to production [every 11.6 seconds](https://www.youtube.com/watch?v=dxk8b9rSKOo&t=9m59s). Indeed, CD appears to be here to stay.

While its easy to sing CD's praises, there is plenty of hard work that goes into producing a smooth CD pipeline.
At its heart is the *build system*, i.e., the scripts, specifications, and tools that define and automate the complex build process of large software systems. Build systems make hundreds (or thousands!) of tool invocations, while preserving the finicky order in which these tools must be invoked. The rapid release cycle fueled by CD would be error-prone (and thus, too risky) without a reliable build system in place.

In our work, we analyze the dark side of CD—the overhead that it introduces on development and release teams (and their infrastructure)—with a focus on how it can be mitigated.

# Build Systems Require Maintenance

# Builds Take Time to Execute

# Builds Consume Computational Resources
