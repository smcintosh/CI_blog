# There Ain't No Such Thing as a Free Build

Modern software is developed at a rapid pace. Last May (2015), Mozilla processed [8,363 updates to the codebase](http://relengofthenerds.blogspot.ca/2015/06/mozilla-pushes-may-2015.html) (roughly 270 updates per day!) The widespread adoption of techniques like Continuous Delivery (CD) accelerates the rate at which these changes become visible to users. Google, LinkedIn, and Facebook release [several times daily](http://mcis.polymtl.ca/publications/2015/IEEE_SW_RELENG_SI.pdf#page=44 "Adams et al., The Practice and Future of Release Engineering: A Roundtable with Three Release Engineers, IEEE Software"). In May 2011, Amazon engineers deployed new releases to production [every 11.6 seconds](https://www.youtube.com/watch?v=dxk8b9rSKOo&t=9m59s). Indeed, CD appears to be here to stay.

While its easy to sing CD's praises, there is plenty of hard work that goes into producing a smooth CD pipeline.
At the heart of CD is the *build system*, i.e., the scripts, specifications, and tools that define and automate the complex build process of large software systems. Build systems orchestrate hundreds (or thousands!) of tool invocations, preserving the finicky order in which build commands must be executed. Rapid release cycles would be too error-prone and risky without a reliable build system in place.

In our research, we analyze the dark side of CD—the overhead that build systems introduces on development and release teams, and their infrastructure—with a particular focus on how the overhead can be mitigated.

## Build Systems Require Maintenance!

[Really](https://e-reports-ext.llnl.gov/pdf/244668.pdf "Kumfert and Epperly, Software in the DOE: The Hidden Overhead of 'The Build'"), [they](http://people.cs.vt.edu/~jiaoyang/paper/esem11.pdf "Hochstein and Jiao, The cost of the build tax in scientific software") [do](http://hera.ugr.es/doi/16515080.pdf "Robles et al., Beyond source code: The importance of other artifacts in software development (a case study)"). While that statement may seem obvious to some, it's important that we're on the same page here. _CD does not come for free._ Indeed, our prior work shows that up to 27% of source code changes (and 44% of test code changes) are [accompanied by changes to the build system](http://shanemcintosh.org/2011/05/21/an-empirical-study-of-build-maintenance-effort.html "McIntosh et al., "An Empirical Study of Build Maintenance Effort").

In recent work, we asked ourselves "what can be done to mitigate build maintenance overhead?" We began by analyzing the impact of build technology choice. For example, do projects that adopt more modern build technologies like [Maven](https://maven.apache.org/) incur less maintenance activity than projects that adopt older build technologies like [Ant](https://ant.apache.org/)?

Surprisingly, the answer is *no*. In fact, our analyses of a large sample of open source repositories (177,039!) suggests that more modern build technologies are accompanied by [greater quantities of build maintenance activity](http://shanemcintosh.org/2014/06/01/a-large-scale-empirical-study-of-the-relationship-between-build-technology-and-build-maintenance.html "McIntosh et al., A Large-Scale Empirical Study of the Relationship between Build Technology Choice and Build Maintenance") than older technologies are! In a follow-up study, we also found that more modern technologies like Maven tend to be [more prone to copy-pasting](http://shanemcintosh.org/2014/05/31/collecting-and-leveraging-a-benchmark-of-build-system-clones-to-aid-in-quality-assessments.html "McIntosh et al., Collecting and Leveraging a Benchmark of Build System Clones to Aid in Quality Assessments") than older technologies are. While there are several reasons for migrating to a more modern build technology, our analyses suggest that lowering maintenance activity does not hold.

On the other hand, we observed that there are open source projects that keep maintenance activity and cloning rates much lower than their counterparts. A deeper analysis of these projects revealed a couple of commonly-adopted patterns of creative build system abstraction.

### Pattern 1: XML Entity Expansion

Rather than duplicating repetitive XML in their `build.xml` files, creative build engineers store common logic in a single file, and load it as a macro using the following snippet:

```xml
<!-- Define references to files containing common targets -->
<!DOCTYPE project [
  <!ENTITY modules-common SYSTEM "../modules-common.ent">
]>
```

Later, the macro can be expanded in various locations:

```xml
<project name="bea" default="all">
  <!-- Include the file containing common targets. -->
  &modules-common;
</project>
```

### Pattern 2: On-the-fly Build Spec Generation

Although less egregious, copy-pasting is still quite a frequently occurring phenomenon in the build systems of C/C++ projects. In our work, we have observed that the studied C/C++ systems with a low rates of copy-pasting avoid duplication by [filling in template build specs](http://shanemcintosh.org/2014/05/31/collecting-and-leveraging-a-benchmark-of-build-system-clones-to-aid-in-quality-assessments.html "McIntosh et al., Collecting and Leveraging a Benchmark of Build System Clones to Aid in Quality Assessments") during an initial step of the build process. This also helps to keep build maintenance activity localized, avoiding painful duplicate effort when maintenance is required.

# Conclusions

To keep up with the pace of modern software development, a robust and reliable build system is required. While recent advances have been made, build systems still require a considerable investment.

In our research, we evaluate means of reducing the overhead that's introduced by the build system. In this blog, I've presented two interesting patterns that we observed in the build systems of projects that have low build maintenance activity. More detail can be found in [our papers](http://shanemcintosh.org/tags/build-systems.html "Build-related research papers").
