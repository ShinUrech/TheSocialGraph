# Project of Data Visualization (COM-480)

| Student's name | SCIPER |
| -------------- | ------ |
| Shin Urech | 327245 |
| Joyti Goel | 325374 |
| Ahmed Chaouachi | 346447 |

[Milestone 1](#milestone-1) • [Milestone 2](#milestone-2) • [Milestone 3](#milestone-3)

## Milestone 1 (20th March, 5pm)

**10% of the final grade**

### Dataset

**Primary Dataset:** Last.fm UK User Graph Dataset\
**Source:** [Zenodo (DOI: 10.5281/zenodo.10694369)](https://doi.org/10.5281/zenodo.10694369)\
**Published:** February 2024 — University of Pisa / CNR

The dataset consists of four interconnected files:

| File | Contents |
| ---- | -------- |
| `network` | 75,969 nodes (users) and 389,639 undirected friendship edges |
| `UsersData_anonymized` | Per-user attributes: gender, age, country |
| `ArtistTags` | Artists mapped to musical genres, weighted by user assignment |
| `ArtistsMap` | Artist names mapped to unique IDs |

#### Data Quality Assessment

The dataset is well-structured and research-grade, having been published by academic institutions with full documentation. Several quality characteristics make it particularly suitable for this project:

- Friendship edges represent real mutual social connections on the Last.fm platform, not inferred or proxy relationships
- Node attributes (age, gender, country) are clean categorical and numerical fields requiring minimal preprocessing
- Genre assignments are derived systematically from user-generated tags, filtered and weighted — reducing noise considerably
- All data is fully anonymised, removing any privacy concerns

#### Preprocessing Requirements

The preprocessing burden is low to moderate:

- The network file is a straightforward edge list — directly loadable into graph libraries (NetworkX, D3.js)
- `UsersData` will require joining to the network file on user ID, and handling of missing values for users who did not complete their profiles
- Genre assignment requires a join across `ArtistsMap` → `ArtistTags` → `UsersData` to derive each user's dominant genre — a multi-step but well-documented process
- The total file size is 287 MB, so a representative sample of the network may be used for browser-based visualisation without loss of analytical integrity

### Problematic

#### What are we trying to show?

This visualisation aims to visually represent the power of **2nd degree connections** — connecting with people who share things in common, i.e. music taste and/or location.

We find it powerful to be able to connect people with others via the people they already know. The core concept is **2nd degree connection value** — our immediate social network (1st degree) is only half the story. The real potential lies one step further: the friends of our friends. Through our friends, we can create new relations with their friends through our music taste and/or our age group and/or our country of origin.

Using the Last.fm UK friendship network as the data foundation, we visualise how a user (Node A) connects to people they don't know (Node C) through a mutual friend (Node B), and critically — what A and C have in common that makes that connection meaningful. The shared attribute could be a music genre, an age group, or a country — turning a structural graph connection into a human insight.

#### Project Overview

Although we have not set in stone the final direction we will be taking, here is the general idea we hope to achieve. The visualisation will allow a user to select a node (a person) and immediately see:

- Their **direct friends** (1st degree connections) highlighted
- Their **friends-of-friends** (2nd degree connections) revealed
- Among those 2nd degree connections, **filtering by shared attributes** — e.g. *"show me 2nd degree connections who also love Jazz"* and *"who are also in their 20s"*

The interface tells a story in layers — starting broad and zooming into the meaningful connections, revealing how close we already are to people we haven't met.

Given that highly connected nodes could yield thousands of 2nd degree connections, we will explore strategies to keep the visualisation performant and readable — such as capping the number of results returned (e.g. top N matches), introducing a query timeout, or surfacing only the strongest or most relevant matches based on the number of shared attributes. The exact approach will be determined during the implementation phase.

#### Motivation

In a world where technology is increasingly disrupting human connection, we are fond of leveraging it to do the opposite — create the opportunity to form human connections. This project is about showing people the power of their existing network: not just who they know, but who they can reach through the people they know.

Using music as the main connecting thread — a powerful and deeply personal factor in human friendship — we explore the possibility of discovering new people who share the same music taste, are of a similar age, and are based in the same location, all through a single mutual friend.

#### Target Audience

The primary audience is anyone curious about the hidden value of their social network — which is to say, anyone. The visualisation requires no technical background to engage with.

### Exploratory Data Analysis

#### Pre-processing of the dataset

The practical work of the milestone starts by loading the raw files from the local workspace and turning them into analysis-ready tables. The objective of this preprocessing is to inspect the schema, identify missing values, clean the demographic fields, and prepare the friendship graph so that the later statistics are trustworthy.

The schema inspection confirms that the milestone can proceed directly with user demographics and the friendship graph. It also confirms the practical limitation already noted earlier: the local files contain artist and tag metadata, but no user-to-artist listening table from which individual music taste could be inferred.

The main cleaning tasks cover: missing-value handling, country normalization, age cleaning and age-group construction, and graph deduplication. The preprocessing results show that the data is usable after a manageable amount of cleaning. The user table keeps enough demographic information to support age-group and country-based matching, and the network becomes much more reliable once self-loops and reciprocal duplicates are removed.

#### Basic statistics and insights

After preprocessing, the next task is to compute descriptive statistics and visual summaries that reveal who is in the dataset, how the network is structured, and whether the second-degree idea is supported by the data.

**Demographic distributions**

![Top countries, Gender distribution, and Age-group distribution](images/eda_chart_1.png)

The demographic distributions show that the dataset is heavily concentrated in the UK and that younger age groups dominate the user base. This matters for the final project because age group is likely to be a stronger discriminating filter than country within the present data.

**Age, playcount, and registration timeline**

![Age distribution, Log10 playcount, and Registrations by year](images/eda_chart_2.png)

These plots give additional context to the user base. The cleaned age distribution looks plausible, playcount is highly skewed, and the registration timeline shows how the network accumulated over time.

**Network degree distributions**

![Direct-friend degree distribution and Second-degree candidate distribution](images/eda_chart_3.png)

The network plots are the clearest empirical support for the final visualisation. The second-degree layer expands much more quickly than the first-degree layer, which means the value of the network really does lie one hop beyond direct friends.

These first statistics already point to the main result of the milestone: the first-degree neighborhood is relatively modest for the typical user, but the second-degree neighborhood is much larger. This directly supports the project's core intuition.

**Shared attribute filtering and prototype cases**

![Second-degree candidates sharing attributes and Prototype case sizes](images/eda_chart_4.png)

This analysis turns the graph from a structural object into a meaningful social story. Not every friend-of-a-friend is equally relevant. Filtering by the supported shared attributes produces a smaller set of reachable candidates that are more socially plausible and more useful for the final interaction design.

**Top global tags**

![Top global tags in the corpus](images/eda_chart_5.png)

The tag metadata still matters as supporting context. It shows that the broader Last.fm environment contains a rich musical vocabulary, which supports the original project vision around music taste. At the same time, this is not the same as computing music taste per user, so it should be treated as contextual evidence rather than as a substitute for missing user-level listening data.

#### Interpretation

The exploratory analysis supports the project direction strongly. After preprocessing, the dataset remains large, usable, and structurally rich. The friendship graph is reliable once deduplicated, the demographic fields are usable after cleaning, and the second-degree layer is much larger than the first-degree layer for the typical user.

This confirms the central idea of the project: the most interesting opportunity is not only in direct friendships, but in the friends-of-friends layer. The data also shows that filtering by supported shared attributes such as age group and country makes that second-degree space more meaningful.

Music taste remains part of the project concept and must stay in the broader narrative, but in this milestone it can only be treated honestly as a future extension unless user-level listening data is added.

### Related Work

#### What Others Have Already Done with the Data

The LastFM dataset family is one of the most widely used in academic research, but almost exclusively for a single purpose: **music recommendation systems**. LastFM datasets have become benchmark tools for algorithms designed around implicit feedback signals, such as Alternating Least Squares (ALS), Bayesian Personalised Ranking (BPR), and LightGCN, where play counts serve as proxies for user preference. In other words, prior work treats the dataset as an input to a recommendation engine, asking *"what should this user listen to next?"*

Ref: [Stanford Large Network Dataset Collection](https://snap.stanford.edu/data/)

A typical example is using the dataset to build a music recommender that incorporates user metadata — age, gender, and country — as additional features for more advanced models. The friendship graph, when used at all, is treated as a side feature to improve recommendation accuracy through social influence modelling, not as the primary subject of exploration.

Ref: [ResearchGate](https://www.researchgate.net/)

On the broader LastFM network specifically (not the UK subset), it has been used in heterogeneous graph embedding research — treating users, artists, and tags as three distinct node types and studying link prediction between them. Again, the focus is on machine learning performance benchmarks, not human storytelling.

Ref: [Networkrepository](https://networkrepository.com/)

#### Why Our Approach is Original

1. **The network as the story, not the background.** Previous work uses the friendship graph as a feature to improve recommendations. We put the graph front and centre — the connections between people *are* the visualisation, not a means to an end.

2. **Attribute-driven 2nd degree exploration with AND/OR logic.** Existing visualisations of 2nd degree friend networks have acknowledged the challenge of making friends-of-friends connections readable, noting that 1st degree connections tend to dominate visually. Our approach addresses this directly by filtering the 2nd degree layer by shared attributes.

   Ref: [Stanford Large Network Dataset Collection](https://snap.stanford.edu/data/)

3. **Human narrative over analytical output.** Rather than a researcher's tool, this is designed as an experience for a general audience — the story of how close we already are to people we haven't met.

#### Sources of Inspiration

Our main inspiration is a visualisation of the digital humanities academic community's social network graph made by **Martin Grandjean** in his project *"Digital Humanities on Twitter, a small-world?"*. Each node represents a person and the colour of the node represents the number of followers that specific account has. Grandjean also added labels to clusters of nodes or noteworthy individual nodes according to their importance or interest.

## Milestone 2 (17th April, 5pm)

**10% of the final grade**


## Milestone 3 (29th May, 5pm)

**80% of the final grade**


## Late policy

- < 24h: 80% of the grade for the milestone
- < 48h: 70% of the grade for the milestone

