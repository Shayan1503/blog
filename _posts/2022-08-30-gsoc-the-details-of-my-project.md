---
layout: single 
title:  "GSoC --- The Details of My Project"
date:   2022-08-30 16:49:29 +0530
toc: true
toc_sticky: true
categories: gsoc
---

In this post, I will discuss what exactly my work is, and how it ties in to the ACTS project. I will also talk about my mentors and what it was like to work with them.

# Project Description

The original project description was:
> We propose the development of a linear algebra plugin for ACTS, which makes use of explicit vectorization.
>
> * Port a similar existing library implementation ([fast5x5](https://gitlab.in2p3.fr/CodeursIntensifs/Fast5x5/)/[xsimd](https://github.com/xtensor-stack/xsimd)) to a suitable library backend (can be decided upon together with the student).
> * Adapt the implementation to the linear algebra operations needed in the ACTS numerical integration for particle propagation.
> * Validate the approach and its performance and possibly optimize the performance further.
> * Given enough time, find a way to make the implementation available to ACTS as a compute backend and run it in ACTS example code.

The project, as described on the CERN-HSF webpage, can be found [here](https://hepsoftwarefoundation.org/gsoc/2022/proposal_Acts-vectorized-LA-backend.html).

## Alternate Project Description

The above description might be a little terse for some. This is the way I like to explain it to others:

CERN generates a lot of data from its LHC experiments. For this reason, they have a lot of software projects which are data analysis oriented. The ACTS project is one such software package which does track reconstruction.

However, the people who work on the ACTS project noticed that the matrix libraries they were using were quite slow for small dimensions (like 4×4, 6×6, 6×8, and 8×8). Upon further investigation, it turned out that popular matrix libraries like `Eigen` and `SMatrix` are not well-optimized for these dimensions.

Some time ago, an alternate implementation of 4×4 matrices and the related operations were written using a library called [`Vc`](https://github.com/VcDevel/Vc), and it performed quite well in benchmarks at the time.

My work is to continue this work and write implemenations for all these desired dimensions, so that the ACTS project's routines run faster.

I will go more into the details of the work and what we have accomplished so far in the next blog post. For now, I will discuss a bit about our weekly meeting schedule, and what it is like to work with my mentors.

### My Project Proposal

My proposal can be found [here](https://docs.google.com/document/d/1Ick3iDF_2bGbLiR-ZDGWnk06rrP64JBQG8Et4TccxRs/edit?usp=sharing). The only change I suggested was to use `xsimd` instead of `Vc`, because the latter is not in active development anymore.

## Where Would The Work Go?

The math code in the [ACTS Project](https://github.com/acts-project/acts) proper is inextricably linked with the track-fitting code, so it would be a lot of work to change that.

However, in the [ACTS project family](https://github.com/acts-project), there are two research repositories: [detray](https://github.com/acts-project/detray) and [traccc](https://github.com/acts-project/traccc). These two repositories were started a couple of years after the original ACTS Project, so their approach to the math code is also slightly different. Both of these projects use another ACTS family project called [`algebra-plugins`](https://github.com/acts-project/algebra-plugins) as a layer of abstraction over the different functions that matrix libraries like `Eigen` and `SMatrix` provide. This allows the developers of these research repositories to easily switch between math backends and test them out.

My work involves fleshing out the [`Vc` portion](https://github.com/acts-project/algebra-plugins/tree/71ea1e814fae3020feedf6389c89a4ffac591bae/math/vc) of `algebra-plugins`, and adding all the functions which are present in the other math backends but not in the `Vc` stuff yet.

# Weekly Meeting Schedule

We usually meet on Mondays at 10:00 am CEST (1:30 pm IST). Sometimes, we meet on other days, if the day is a holiday in some country, or one of us has other commitments.

# Mentors

I have the pleasure of working with 3 highly qualified mentors. I will write a bit about each person:

## Joana Nierman

She is currently a PhD student in the [University of Göttingen](https://www.uni-goettingen.de/en/1.html).

She was the first person I contacted, about my interest in working on this project for GSoC, so we have been corresponding since late March of this year. She is quite helpful in general. In our weekly meets, she regularly helps us set the direction for the week, and also helps with gathering more data by running the benchmarks I have written on other machines.

## Hadrien Grasland

Hadrien is a software engineer who works for the [Centre National de la Recherche Scientifique](https://www.cnrs.fr/en) (CNRS).

His knowledge of the nitty-gritty details of computer architecture and SIMD instruction sets is unparalleled. I am highly grateful to work with him, and I have learned a lot by working on this project with him, and discussing the assembly outputs of the different benchmarks I have written.

In the early stages of the project, he was quite supportive of me adding other matrix library backends to our benchmarks, and seeing how they perform in comparison with Eigen and Fast 5×5.

## Paul Gessinger-Befurt

He is the person who knows the most about ACTS project out of all three mentors. When we have any questions about anything related to the ACTS project, he is the person who helps us out.

That's all for this blog post! In the next one, I will discuss our results so far in detail, and what we plan on doing next.
