<!-- README.md is generated from README.Rmd. Please edit that file -->
spectralGraphTopology
=====================

[![CRAN\_Status\_Badge](https://www.r-pkg.org/badges/version/spectralGraphTopology)](https://cran.r-project.org/package=spectralGraphTopology)
[![CRAN
Downloads](https://cranlogs.r-pkg.org/badges/spectralGraphTopology)](https://cran.r-project.org/package=spectralGraphTopology)
![CRAN Downloads
Total](https://cranlogs.r-pkg.org/badges/grand-total/spectralGraphTopology?color=brightgreen)
[![Rcpp](https://img.shields.io/badge/powered%20by-Rcpp-orange.svg?style=flat)](http://www.rcpp.org/)

[![codecov](https://codecov.io/gh/mirca/spectralGraphTopology/branch/master/graph/badge.svg)](https://codecov.io/gh/mirca/spectralGraphTopology)
[![Travis-CI-Badge](https://travis-ci.org/mirca/spectralGraphTopology.svg?branch=master)](https://travis-ci.org/mirca/spectralGraphTopology)
[![Build
status](https://ci.appveyor.com/api/projects/status/vr62ddvc9xoabnwy?svg=true)](https://ci.appveyor.com/project/mirca/spectralgraphtopology-j05c9)
[![Docker Build
Status](https://img.shields.io/docker/cloud/build/mirca/spectralgraphtopology.svg)](https://hub.docker.com/r/mirca/spectralgraphtopology/)
[![Build
Status](https://dev.azure.com/jvmirca/spectralGraphTopology/_apis/build/status/mirca.spectralGraphTopology?branchName=master)](https://dev.azure.com/jvmirca/spectralGraphTopology/_build/latest?definitionId=1&branchName=master)

<a href="https://mirca.github.io/spectralGraphTopology"><img style="float: right;" width="250" src="./man/figures//circles3_reduced.gif" align="right" /></a>

**spectralGraphTopology** provides estimators to learn k-component,
bipartite, and k-component bipartite graphs from data by imposing
spectral constraints on the eigenvalues and eigenvectors of the
Laplacian and adjacency matrices. Those estimators leverages spectral
properties of the graphical models as a prior information, which turn
out to play key roles in unsupervised machine learning tasks such as
community detection.

**Documentation**:
[**https://mirca.github.io/spectralGraphTopology**](https://mirca.github.io/spectralGraphTopology).

Installation
------------

From inside an R session, type:

``` r
> install.packages("spectralGraphTopology")
```

Alternatively, you can install the development version from GitHub:

``` r
> devtools::install_github("dppalomar/spectralGraphTopology")
```

#### Microsoft Windows

On MS Windows environments, make sure to install the most recent version
of `Rtools`.

#### macOS

**spectralGraphTopology** depends on
[`RcppArmadillo`](https://github.com/RcppCore/RcppArmadillo) which
requires [`gfortran`](https://CRAN.R-project.org/bin/macosx/tools/).

Usage: clustering
-----------------

We illustrate the usage of the package with simulated data, as follows:

``` r
library(spectralGraphTopology)
library(clusterSim)
library(igraph)
set.seed(42)

# generate graph and data
n <- 50  # number of nodes per cluster
twomoon <- clusterSim::shapes.two.moon(n)  # generate data points
k <- 2  # number of components

# estimate underlying graph
S <- crossprod(t(twomoon$data))
graph <- learn_k_component_graph(S, k = k, beta = .5, verbose = FALSE, abstol = 1e-3)

# plot
# build network
net <- igraph::graph_from_adjacency_matrix(graph$Adjacency, mode = "undirected", weighted = TRUE)
# colorify nodes and edges
colors <- c("#706FD3", "#FF5252")
V(net)$cluster <- twomoon$clusters
E(net)$color <- apply(as.data.frame(get.edgelist(net)), 1,
                      function(x) ifelse(V(net)$cluster[x[1]] == V(net)$cluster[x[2]],
                                        colors[V(net)$cluster[x[1]]], '#000000'))
V(net)$color <- colors[twomoon$clusters]
# plot nodes
plot(net, layout = twomoon$data, vertex.label = NA, vertex.size = 3)
```

<img src="man/figures/README-plot_k_component-1.png" width="75%" style="display: block; margin: auto;" />

Contributing
------------

We welcome all sorts of contributions. Please feel free to open an issue
to report a bug or discuss a feature request.

Citation
--------

If you made use of this software please consider citing:

-   J. V. de Miranda Cardoso, D. P. Palomar (2019).
    spectralGraphTopology: Learning Graphs from Data via Spectral
    Constraints. R package version 0.1.0.
    <https://CRAN.R-project.org/package=spectralGraphTopology>

-   S. Kumar, J. Ying, J. V. de Miranda Cardoso, and D. P. Palomar
    (2019). A unified framework for structured graph learning via
    spectral constraints. <https://arxiv.org/abs/1904.09792>

In addition, consider citing the following bibliography according to
their implementation:

<table>
<colgroup>
<col style="width: 9%" />
<col style="width: 90%" />
</colgroup>
<thead>
<tr class="header">
<th><strong>function</strong></th>
<th><strong>reference</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><code>cluster_k_component_graph</code></td>
<td>N., Feiping, W., Xiaoqian, J., Michael I., and H., Heng. (2016). <a href="https://dl.acm.org/citation.cfm?id=3016100.3016174">The Constrained Laplacian Rank Algorithm for Graph-based Clustering</a>, AAAI’16.</td>
</tr>
<tr class="even">
<td><code>learn_laplacian_gle_mm</code></td>
<td>Licheng Zhao, Yiwei Wang, Sandeep Kumar, and Daniel P. Palomar, <a href="https://palomar.home.ece.ust.hk/papers/2019/ZhaoWangKumarPalomar-TSP2019.pdf">Optimization Algorithms for Graph Laplacian Estimation via ADMM and MM</a>, IEEE Trans. on Signal Processing, vol. 67, no. 16, pp. 4231-4244, Aug. 2019</td>
</tr>
<tr class="odd">
<td><code>learn_laplacian_gle_admm</code></td>
<td>Licheng Zhao, Yiwei Wang, Sandeep Kumar, and Daniel P. Palomar, <a href="https://palomar.home.ece.ust.hk/papers/2019/ZhaoWangKumarPalomar-TSP2019.pdf">Optimization Algorithms for Graph Laplacian Estimation via ADMM and MM</a>, IEEE Trans. on Signal Processing, vol. 67, no. 16, pp. 4231-4244, Aug. 2019</td>
</tr>
</tbody>
</table>

Links
-----

Package:
[CRAN](https://CRAN.R-project.org/package=spectralGraphTopology) and
[GitHub](https://github.com/dppalomar/spectralGraphTopology)

README file:
[GitHub-readme](https://github.com/dppalomar/spectralGraphTopology/blob/master/README.md)

Vignette:
[GitHub-html-vignette](https://raw.githack.com/dppalomar/spectralGraphTopology/master/vignettes/SpectralGraphTopology.html)
