visual-embedding
================
We have recently proposed [visual embedding](http://idl.cs.washington.edu/papers/visual-embedding/)
as an operational model for automatically generating and evaluating visualizations (see also [our original
proposal] (http://hci.stanford.edu/~cagatay/projects/vismodel/TheoriesOfVisualization-Vis11.pdf) presented
at Vis'11). In the paper, we provide three examples of visual embedding. This repository contains the [data](data/) and
[source code](src/)  used to generate these examples.  In order to [demonstrate discrete visual embedding](#scatter-plotting-with-shapes),
we crowdsource perceptual distances using Amazon's Mechanical Turk service.  You can access the data and source code for our crowdsourcing experiments in [src/mturkExperiments](src/mturkExperiments).  We also provide the resulting  perceptual distance matrix for the shapes shown in Figure 3  as a text file ([data/polygonKernel.txt](data/polygonKernel.txt)).
Note that we extend upon this idea of learning kernels of perceptual similarity
using crowdsourcing in our [recent paper](http://idl.cs.washington.edu/papers/perceptual-kernels/).


What is Visual Embedding?
=========================
Visual embedding is an operational model for automated visualization design and evaluation. Although researchers have proposed numerous guidelines
and heuristics, a formal framework for design and evaluation is still elusive. Instead, conducting a posteriori user studies is still the primary tool for assessing a visualization’s effectiveness. Using theoretical
models presents another, albeit less explored, approach. Model-based approaches that integrate perceptual considerations into design
process in a measurable, data-driven form can accelerate visual design and complement summative nature of user studies.


An Operational Framework for Visualization
------------------------------------
Developing a theory of visualization that is both descriptive and generative is, however, difficult. The
space of visualizations is large, and the use of visualization spans many issues in human perception
and cognition. Additional factors, such as interaction techniques, can significantly affect a
visualization’s success. Given our current knowledge, visualization design is an underconstrained
problem. So, there is value in developing simpler, constrained models, each addressing certain aspects
of visualization while ignoring others, like spotlights on a theater stage.  The main advantage of this approach
 is that it is conducive  to developing operational models.

In this context, we introduce visual embedding as a model for visualization construction. We define
a visualization as a function that maps from a domain of data points to a range of visual primitives.
We claim a visualization is “good” if the embedded visual elements preserve structures
present in the data domain. A function meeting this criterion constitutes a visual embedding of
the data points (see Figure 1).


<img width="600" src=http://uwdata.github.io/perceptual-kernels/ve/figures/f.svg>
<p>Figure 1. Visual embedding is a function that preserves structures in the data (domain)
within the embedded perceptual space (range).</p>


Perceptual Painting of Data Relations
-------------------------------------
Visual embedding is based on the premise that good visualizations should convey structures or relations
(e.g., similarities, distances, etc.) in data with corresponding, proportional perceptual relations. For example,
if two data values are similar to each other in a user-defined or some other sense, they should be encoded with
perceptually similar values of visual variables (color, shape, etc.) and vice versa. The underlying assumption here
is, however,  that degrees of perceptual affinities between and within visual encoding variables are available to us.


Visualizations Beyond Visual Encoding
-------------------------------------
Data visualization is, more than anything else, a user experience production and, as such, needs to embrace and utilize all human sensing capacities eventually. Embedding spaces, as discussed here,  need not to be restricted to visual stimuli. They could be any perceptual channel or combinations  thereof, such as color, texture, shape, icon, tactile, and audio features. For example, we could, in theory, apply our formulation  to construct  sonifications for people with visual disabilities.


Examples of Visual Embedding
===============================================
The  basic framework proposed above  can be used to generate and evaluate visualizations
on the basis of both the underlying data and—through the choice of preserved structure—desired
perceptual tasks.


Coloring Neural Tracts
-----------------------
Our first example is coloring neural-fiber pathways estimated from a diffusion-imaging brain dataset. Given a set of tracts, we first
compute distances (or dissimilarities) between pairs of pathways. To do this, we use a simple measure that
quantifies the similarity of two given neural pathways’ trajectories. We then construct the visualization function
by embedding the distances in the [CIELAB](http://en.wikipedia.org/wiki/Lab_color_space) color space using multidimensional scaling.
Figure 2 shows the obtained colorings; perceptual variations in color reflect the spatial variations in the tracts.

<img width="700" src="figures/t.png">
<p>Figure 2. Coloring neural tracts: (left) the internal capsule and (right) the corpus callosum. We colored them
using visual embedding in CIELAB, a perceptually uniform color space. Perceptual variations in color reflect the spatial
variations in the tracts.</p>


Scatter Plotting With Shapes
-------------------------------
In the above example, the visual space,  CIELAB, is a subspace
of the continuous three space. Embedding in continuous spaces is
relatively well studied, thanks partly to the fact that
centuries of mathematical analysis (i.e., calculus) left
us with a powerful toolset operating on continuous spaces.

How does visual embedding apply if the range of visualization
function is discrete?

Here, a toy problem demonstrates embedding in a
discrete visual space. We want to assign polygonal
icons from the discrete polygonal-shape space Vp
(Figure 3) to a given set of 2D points so that the
points’ spatial proximity was redundantly encoded
via the assigned polygons’ perceptual proximity.

<img width="400" src="http://uwdata.github.io/perceptual-kernels/ve/figures/vp.svg">
<p>Figure 3. A palette of polygonal shapes.</p>

Though simple, this setup is realistic: redundant visual
encoding is common in visualization. Alternatively,
we could have used icons to convey attributes
of other dimensions of the data points.
Unlike the coloring example, here we lack a
perceptual model for estimating perceived distance.
So, we first obtained a crowdsourced estimate of the perceptual
distances between the elements of Vp, using
Amazon’s Mechanical Turk (Figure 4).


<img width="400" src="figures/m.png">
<p>Figure 4. The task interface of our crowdsourcing experiment on
Amazon’s Mechanical Turk website. </p>

The study participants
saw all possible pairs, including identical ones. We
used errant ratings of identical polygon pairs to filter
“spammers.” After this initial filtering, we normalized
each participant’s ratings and averaged the
ratings across the users. Finally, we normalized the
averaged ratings and accumulated the results in a
distance matrix. Figure 5 shows the resulting
perceptual-distance matrix and its two-dimensional
projection.

<a href=http://uwdata.github.io/perceptual-kernels/view/vp-l7.html>
<img width="600" src="http://uwdata.github.io/perceptual-kernels/ve/figures/k.svg">
</a>
<p>Figure 5. (Left) Estimated perceptual-distance matrix. Darker colors indicate closer distances.
(Right) Two-dimensional projection of the shapes based on the distance matrix.
(<a href=http://uwdata.github.io/perceptual-kernels/view/vp-l7.html>Click</a> on the figure to see
an interactive version of the distance matrix and its projection.)</p>


After estimating the perceptual-distance matrix, we  pose the embedding problem
as maximum a posteriori estimation in a Markov random field (an undirected graphical model)
to find an embedding of a simple 2D point set in Vp. Figure 6 shows the result, where the shape
assignment reflects the data points’ clustering, as we desired.

<img width="600" src="http://uwdata.github.io/perceptual-kernels/ve/figures/d.svg">
<p>Figure 6. Visual embedding in a discrete visual space. We embed the planar data points in Vp.
The shape assignment reflects the data points’ spatial variation and clustering.</p>


Evaluating Tensor Glyphs
-------------------------
Can we evaluate visualizations without running user studies?
Models, of any sort, are particularly useful if they have 
predictive and evaluative power, going beyond being
descriptive.

With our model, given suitable data and perceptual
metrics, we can assess competing visualization
techniques’ structure-preserving qualities. Here, we compare
[superquadrics](http://en.wikipedia.org/wiki/Superquadrics)
and [cuboids](http://en.wikipedia.org/wiki/Cuboid), two
alternative glyphs used in visualizing second-order
diffusion tensors (Figure 7).

<img width=600 src=figures/ga.png>
<p>Figure 7. A superquadric and a cuboid glyph, used for visualizing the same tensor field.
The insets represent the diagonal tensor <a href='#tensor'><b>D</b></a>.</p>

You can think a second-order diffusion tensor as the covariance
matrix of a three-dimensional Gaussian distribution. So, superquadrics
and cuboids, together with 3D position, can also be seen as glyphs
for visualizing three-dimensional Gaussian distributions.

We conduct a simple experiment: We rotate the diagonal
tensor <b id='tensor'>D</b> = [2.1 0 0; 0 2 0; 0 0 1]
around its smallest eigenvector (0, 0, 1) with five
incremental degrees. We compute how the tensor
value changes as the Euclidean distance between
the reference tensor and the rotated tensor
changes. We approximate the perceptual change
in the corresponding glyph visualizations with
the sum of the magnitudes of the optical flow at
each pixel in the image domain. We average the
optical-flow distances over nine viewpoints uniformly
sampled on a circumscribed sphere under
fixed lighting and rendering conditions.
The trends in Figure 8 suggest that superquadrics
represented the change in the data more faithfully
(that is, preserved the structure better) than
cuboids. This supports the visualization design
choice motivating superquadrics.

<img width=500 src="http://uwdata.github.io/perceptual-kernels/ve/figures/gb.svg">
<p>Figure 8. Changes in the size of <a href='#tensor'><b>D</b></a> and its superquadric
and cuboid representations  with respect to rotations around the tensor’s smallest eigenvector.
The tensor size  and the superquadric glyph’s appearance follow a similar trend; the cuboid glyph’s
appearance differs. This suggests that superquadric glyphs better preserved the
structure in the data.</p>

Background
===========
Researchers have proposed general and specific models of visualization.
In order to put the visual embedding model in a historical context, we discuss
a representative subset of earlier work here.

[Jock Mackinlay](http://en.wikipedia.org/wiki/Jock_D._Mackinlay) introduced one of the most
influential systems for automatically generating visualizations. Following [Jacques Bertin](http://en.wikipedia.org/wiki/Jacques_Bertin)’s
aphorism of graphics as a language for the eye, Mackinlay formulated visualizations as sentences in a graphical
language. He argued that good visualizations will meet the criteria of expressiveness and
effectiveness. A visualization meets the expressiveness criterion if it faithfully presents
the data, without implying false inferences. Effectiveness concerns how accurately viewers
can decode the chosen visual-encoding variables; it’s informed by prior studies in graphical
perception (for example, [by William Cleveland and Robert McGill](http://www.jstor.org/discover/10.2307/2288400?sid=21104938778461&uid=3739256&uid=2&uid=3739808&uid=4)).
 Mackinlay’s APT ([A Presentation Tool](http://www2.parc.com/istl/groups/uir/publications/items/UIR-1986-02-Mackinlay-TOG-Automating.pdf))
employed a composition algebra over a basis set of graphical primitives derived from Bertin’s
encodings to generate alternative visualizations. The system then selected the visualization that
best satisfied formal expressiveness and effectiveness criteria. APT didn’t explicitly take into account
user tasks or interaction. To this end, [Steven Roth and his colleagues extended Mackinlay’s work](http://www.cs.cmu.edu/~sage/PDF/Interactive.pdf)
with new types of interactive presentations. Similarly, [Stephen Casner built on APT by incorporating user
tasks to guide visualization generation](dl.acm.org/citation.cfm?id=108361). Some of these ideas now support visualization recommendation
in [Tableau](htpp://www.tableausoftware.com), a commercial visualization tool.

[Donald House and his colleagues’ automatic visualization](dl.acm.org/citation.cfm?id=1137505) system integrated user preferences using
combinatorial optimization. Genetic algorithms refined a population of visualizations in response
to user ratings. In contrast to this empirical approach, [Daniel Pineo and Colin Ware used a
computational model of the retina and primary visual cortex to automatically
evaluate and optimize visualizations](http://ieeexplore.ieee.org/xpls/abs_all.jsp?arnumber=5728805). These two
works represent two  antipodal approaches to automated visualization. The former probably
provides a fast-track engineering solution while the latter may lead to a better understanding of how 
visualizations work perceptually and cognitively. 

[Jarke van Wijk argued](http://link.springer.com/chapter/10.1007/b106657_18) for first modeling a perceptual domain
(for example, luminance or shape perception) and then optimizing for some perceptual
goal according to that model. Visual embedding can be viewed as a reusable template within
van Wijk’s discussion on perceptually optimal visualizations. If we chose a motto for visual
embedding, it would be “<em>visualization as a perceptual painting of structure in data</em>.” In this sense, visual
embedding’s perceptual-structure preservation criterion closes the cycle, explicitly linking Mackinlay’s
expressiveness and effectiveness criteria while providing a recipe to achieve both.

While the above background provides a useful backdrop for visual embedding,
this background itself would benefit from a better understanding of the context of
data visualizations. What does this larger context constitute?  It has essentially
three elements: 1) Perceptual stimuli (visual encoding, chart size, chart type, etc.),
2) data visualized  and 3) the cognitive state of user, of which task is part.
To this end, we recommend close reading of the following three classics:

+ [Eye and Brain: The Psychology of Seeing](http://www.amazon.com/Eye-Brain-Richard-L-Gregory/dp/0691048371) by [R. L. Gregory](http://en.wikipedia.org/wiki/Richard_Gregory)---useful to contrast it with [J. J. Gibson](http://en.wikipedia.org/wiki/James_J._Gibson)'s approach to visual perception, 

+ [Semiology of Graphics: Diagrams, Networks, Maps](http://www.amazon.com/Semiology-Graphics-Diagrams-Networks-Maps/dp/1589482611) by [J. Bertin](http://en.wikipedia.org/wiki/Jacques_Bertin),

+ [The Psychology of Human Computer Interaction](http://www.amazon.com/Psychology-Human-Computer-Interaction-Stuart-Card/dp/0898598591)
by [S. Card](http://en.wikipedia.org/wiki/Stuart_Card), T. P. Moran and [A. Newell](http://en.wikipedia.org/wiki/Allen_Newell).


Moving On
=========
The visual embedding examples above are intended to be only a proof of concept, including our approach for estimating
perceptual distance through crowdsourcing. Visualizations live in context; crowdsourcing-based estimated
perceptual distances can’t capture all the perceptual interactions of every context. Also, running large-scale
crowdsourcing studies can be difficult. Because we used a small discrete space, we could present every pair of embedding-space
points to each study participant. Running a similar experiment with thousands of discrete visual primitives will require
larger studies and more sophisticated analysis methods for estimating a distance matrix. Similarly, large-scale embedding
can be slow; however, many heuristics, such as restricting pairwise distances to local neighborhoods and sampling,
can ameliorate the problem.

On the basis of these challenges and insights derived from our examples, we envision the following research
directions.

A Standard Library of Visual Spaces
-----------------------------------

The visualization community could benefit from a standard library of visual spaces with associated
perceptual measures. The library would be a practical resource for constructing useful defaults
for visualizations.  [<em>Perceptual kernels</em>](http://idl.cs.washington.edu/papers/perceptual-kernels) is
a step in this direction.


Probabilistic Models of Visualizations
--------------------------------------
Implementing visual embedding with graphical models provides an opportunity to explore
probabilistic models of visualization design spaces. This might prove fruitful because
several “optimum” visualizations often exist. Using graphical models can also help express
high-level structures in data. Such models might also make it easier to incorporate
aesthetic or subjective criteria into automatic visualization generation.


Evaluating Visualizations
-------------------------
To use visual embedding to evaluate visualizations, a primary challenge is to devise and validate
appropriate image-space measures (for example, optical flow) to approximate perceptual distances.


Tools
-----
Finally, developing tools that facilitate construction of visualizations under our model is crucial.
Two challenges stand out. The first is to develop a visualization language that lets users express
and create visual embeddings without implementing an optimization algorithm. This language should
integrate libraries of visualization defaults for different data and task domains. It might also benefit
from crowd-programming ideas to enable automated support for running crowd-sourced evaluations.
The second challenge is to develop a visualization debugger in the spirit of the tensor glyph example,
letting users get runtime feedback about visualization quality. We envision future visualization
development environments integrating such languages and debuggers.
