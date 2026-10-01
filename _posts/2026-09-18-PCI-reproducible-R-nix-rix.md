---
layout: post
author: Paula Oset
title: "Reproducible computing in R with Nix and {rix}"
banner_image: /assets/img/posts/pexels-snapsbyclark-12981879.jpg
description: A practical introduction to reproducible computing in R with Nix and {rix}, exploring why reproducibility requires more than sharing code and how Rix
date: 2026-09-18 16:30:00
tags: [reproducibility, open science, R, Nix, Rix]
categories: events
tabs: true
featured: true
---

## Reproducibility is more than data and code

When researchers talk about reproducibility, the conversation often centres on sharing data and code. If both are publicly available, the assumption is that the research can be reproduced. But as [Felipe Fontana Vieira](https://www.linkedin.com/in/felipe-fontana-vieira-b00258200/) argued during a recent GhentCORR webinar (see [slides](/assets/pdf/20260915_ghentcorr-webinar_nix-rix_slides.pdf)), the reality is more complicated. Even perfectly documented code may fail to run, or produce different results, if the computational environment has changed.

The challenge is that research data analyses depend on multiple factors beyond the code itself (R version, R packages, document tools and system libraries and compilers), and each of these factors can affect the final result.

<div style="position: relative; padding-bottom: 56.25%; height: 0; overflow: hidden; margin-bottom: 2rem;">
  <iframe src="https://www.youtube.com/embed/XGg5gdv0uM4?si=dxgnIcJoQ39gFUru"
          title="YouTube video player"
          style="position: absolute; top:0; left:0; width:100%; height:100%;"
          frameborder="0"
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share"
          allowfullscreen>
  </iframe>
</div>


## Small changes, big consequences

To demonstrate this, Felipe Fontana Vieira presented several examples of how seemingly minor changes to the computational environment can affect reproducibility.

In the first examples shown, changes in the R version altered how random numbers were generated or how text values were handled in data frames. The same code could therefore provide different results when run under newer versions of R.

Package updates can create similar challenges. Functions may be renamed, internal code structures can change, and defaults may be modified. In some cases, these changes generate errors that alert users to the problem. In other cases, analyses continue to run but silently produce different results.

<div class="row align-items-start">
  <div class="col-md-6" markdown="1">

Perhaps the most surprising example was that reproducibility issues can arise from system libraries and compilers underneath R that often goes unnoticed. Even when researchers use the same code, package versions and random seed, differences in underlying system libraries can sometimes lead to slightly different results on different operating systems. While such differences may be small, their impact is not always predictable and can depend on the data, the methods used, and how results are used downstream through later stages of an analysis. Without awareness and careful investigation, these discrepancies may go entirely unnoticed.


  </div>

  <div class="col-md-6" markdown="1">

{% include figure.liquid
   loading="eager"
   path="assets/img/posts/r-nix-rix-compenv.png"
   class="img-fluid rounded z-depth-1"
%}

  </div>
</div>

## The missing piece: computational environments

These examples point to a broader lesson: reproducibility requires more than preserving code. It also requires preserving the computational environment in which that code was executed.

Many researchers already use tools that capture parts of this environment. However, these solutions often need to be combined and maintained separately. The result can be a complex workflow requiring substantial technical expertise.

The webinar also discussed both the strengths and limitations of Docker for reproducible research. While containers are valuable for sharing computational setups, reproducibility depends on how those containers are built and maintained. In practice, researchers often share only a Dockerfile which does not automatically guarantee that the same environment can be recreated in the future.

## Making reproducibility easier with rix and Nix

The main focus of the session was the combination of Nix and the R package rix.

Nix is a package manager designed to recreate computational environments from an explicit description. Instead of relying on whatever software and libraries happen to be installed on a computer or server, researchers define exactly which versions of software and packages should be used using Nix. The environment can then be rebuilt from that specification.

The challenge is that Nix has its own programming language, which can be difficult for newcomers to learn. This is where rix comes in. rix allows researchers to define environments directly from R by specifying familiar information such as packages, command-line tools and a snapshot date. Using rix you can then build the environment and enter it in a straightforward way with Nix doing all the work behind the scenes.

As the speaker noted, this makes a powerful reproducibility framework much more accessible to researchers who are not interested in becoming Nix experts.

A live demonstration showed just how little was required to get started. By specifying a date and a list of required packages, Felipe Fontana Vieira generated an environment that could be rebuilt and shared with collaborators. The same environment could then be used to run analyses, generate figures and tables, and even render a complete manuscript using Quarto. 

This approach extends reproducibility beyond code execution. It means that collaborators can recreate not only the analysis but also the final research outputs generated from that analysis.


## Looking ahead: automated reproducibility checks

The webinar concluded with an introduction to rixcheck, an R package aimed at automating reproducibility verification. Built on top of {rix} and Nix, the package helps researchers set up automated GitHub continuous integration workflows to regularly verify whether a project's computational environment can still be rebuilt and whether its analyses and research outputs can still be reproduced. Felipe also welcomed contributions to this project. The rixcheck repository is available on [GitHub](https://github.com/felipelfv/rixcheck).

## Conclusion

The central message of the webinar was that reproducibility should include the computational environment, not only the code and data. By making dependencies explicit and shareable, tools such as rix and Nix offer researchers a practical way to create analyses and manuscripts that remain reproducible across systems and over time.

A final message from the session was that reproducibility needs to be both accessible and clearly defined. If researchers are expected to adopt reproducible practices, the tools and workflows supporting them must be practical to use, and one must be more precise about what it means when it asks for "reproducibility".

## References and acknowledgements

- Felipe Fontana Vieira, Jason Geller & Bruno Rodrigues. (2026). [Why Risk it, When You Can {rix} it: A Tutorial for Computational Reproducibility Focused on Simulation Studies](https://felipelfv.github.io/Why-risk-it-when-you-can-rix-it/)

- Banner image by [Clark Van Der Beken from Pexels](https://www.pexels.com/photo/teal-concrete-building-12981879/)

This blog post was written by the **Ghent**CORR core team. Copilot was used for editorial support, including improving clarity, flow, and consistency of the text.