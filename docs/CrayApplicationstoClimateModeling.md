# Applying machine learning and principal component analysis to climate science

# Distributed principal component analysis

Principal component analysis (PCA), sometimes equivalently referred to as
empirical orthogonal function (EOF) analysis in the climate literature, is used
across a variety of disciplines for dimensional reduction of data. In climate
studies, arguably its most common application is to determine time-varying modes
of variabilty and their spatial loading patterns. For example, the Pacific
Decadal Oscillation is defined as the first principal component of sea surface
temperatures (SST) in the Northern Pacific. The Southern Annular Mode (SAM) is
an atmospheric phenomeon that captures the strength of the southern polar
vortex. Both of these have significant climatic implications and are of crucial
importance to understand.

In the climate community, datasets or model output on which PCA/EOF analysis is
performed are typically small enough that they can be computed using
singular-value decomposition on 2D covariance arrays with (dimensions of time and
location) on an individual workstation or laptop. However with the move to
higher resolution models and ensemble modeling (discussed in greater detail
below), these arrays can easily exceed the memory and computational requirements
of a single workstation. The typical tools used in our community (e.g. Matlab
and Python), are not typically setup to handle these computations on
high-performance computing platforms.

While it is within the technical skillset of climate scientists to implement
forms of distributed PCA, the person-hours required is often prohibitive.
Distributed PCA within the Chapel framework has the potential provide
researchers a scalable framework for analysis that can be applied to O(1TB)
datasets.

# Machine Learning

## Tuning climate models

Climate models are physically, chemically, and biologically based simulations of
the climate system which require approximations of the sub-gridscale processes
of the model. For example, in the ocean typical horizontal resolutions are
orders of magnitude larger than oceanic eddies, the largest scale of oceanic
turbulence, whose transport of momentum, heat, salt, and other tracers is a
first-order effect in the climate system. The theoretical and observational
constraints of these models are often uncertain and so there is a large
parameter-space to explore.

The selection of the appropriate values for various parameters within the model
is complicated by the nonlinear, chaotic nature of the system. Small changes in
parameters, initial conditions, or random number seeds are often amplified to
yield different transient simulations. To achieve robust results, some form of
time-averaging is often employed to separate fundamental differences between
different realizations of the same model. To obtain robust statistics, long
integration (100s of years) are required requiring O(100,000 core-hours) per
experiment.

### Machine-learning guided tuning

Climate centers have largely relied on manual climate tuning guided by expert
knowledge to evaluate their climate models against a variety of metrics. As an
example, the following steps are typical of a model tuning exercise

1. Establish a baseline run with a fixed set of model parameters
2. Examine the biases of the baseline run
3. Identify which parameterizations and values could improve/degrade the
   solution
4. Perform runs changing a single value/process at a time and evaluate
5. Choose a subset of the changes tested in Step 4 to establish a new baseline
   run

Machine learning can aid the tuning of climate models by suggesting a new suite
of parameters to test in an objective way and quantifying the connections
between model parameters and their impacts on evaluation metrics. The first of
these takes away the need for scientists to 'guess' the specific values and
parameterizations in Step 3. The second can help understand which parts of the
model covary which can further guide basic research into why those connections
exist.

## Ensemble modeling

Aspects of the climate system are predictable, but are complicated by the role
of inherent variability within the system. Both aspects are crucial for seasonal
forecasting and climate projections, as well as interpreting variations and
trends in the observed climate. To separate internal variability from coherent
responses, modeling centers often will perform large ensemble experiments, where
the underlying model does not change, but with each realization having perturbed
initial conditions and/or changes to parameterizations. Typically for a given
metric, the average over all ensemble members is reported and the standard
deviation is reported as the level of uncertainty. 

For planning purposes, the above standard analysis is typically all that is
required, however from a scientific perspective it is often necessary to
understand how the models are clustered. Oftentimes, a researcher may identify a
particular feature of interest, compare all the ensemble members, organize the
members in some way, and then backtrack to the original ensemble configurations
to identify why the organization exists. Alternatively, the ensemble members
will be organized by similarity in initial configuration and then the feature of
interest compared.

### Applying machine-learning techniques to ensemble modeling

The more in depth analysis described above can be tedious and relies solely on
the creativity and intuition of the researcher.

Machine-learning on the other hand provides a way to objectively identify a wide
variety of quantities and objectively organize ensemble members. Neural-network
based techniques like self-organizing maps can help identify robust behaviors of
the model. Deep-learning techniques could be employed to identify connections
that a single researcher may not have thought of on their own.

# Summary

HPC platforms are used extensively in the climate community to integrate models
of the atmosphere, ocean, and land as well as the biogeochemistry in each of
these realms. The new advances in scalable PCA and machine-learning techniques
developed at Cray can provide additional value to these existing platforms by 

1. Increasing the efficiency of model analysis and model tuning using
   machine-learning techniques
2. Enabling researchers to expand well-established analysis methods to large
   datasets (O(1TB) using distributed PCA
3. Opening new avenues of discovery by identifying relationships between model
   parameters and output using deep-learning frameworks
