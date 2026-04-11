# Towards an Operational Theory of Creativity: Reframing, Abstraction, and the Expansion of Problem-Solution Spaces

**Author:** Fabrizio Costa  
**Date:** 2026-04-11

---

### 1. Executive Summary

Creativity is typically treated as an elusive, qualitative phenomenon. This paper proposes a formal, operational perspective:

> **Creativity is the transformation of a problem–solution space through the introduction of new representations, abstractions, or operators that expand the set of tractable and meaningful problems.**

Rather than measuring creativity via output novelty or impact alone (e.g., citations, usage), we define it in terms of **what it unlocks**:

* new solvable problems,
* more efficient solution pathways,
* new abstractions that propagate across domains,
* and an expanded set of *interesting* problems as determined by a latent social interest function.

---

### 2. Problem Statement

Classical learning theory (e.g., PAC learning, VC dimension) assumes:

* a fixed hypothesis class
* a fixed representation of inputs
* evaluation over a predefined distribution

This is insufficient for creativity, where:

* the **hypothesis space itself changes**,
* the **representation of inputs is redefined**,
* and **new problems emerge as a consequence of prior solutions**.

We require a framework that captures:

1. **Expansion of hypothesis space**
2. **Transformation of representations**
3. **Propagation of abstractions across tasks**
4. **Emergence of new, meaningful problems**

---

### 3. Core Conceptual Shift

#### From:

* Learning within a fixed hypothesis space

#### To:

* **Transformations of the hypothesis space itself**

Creativity is not:

* selecting a better hypothesis

It is:

* **constructing new axes of generalization**
* **introducing new compositional primitives**
* **reshaping the structure of the solution space**

---

### 4. Formalization

#### 4.1 Problem Space

Let:

* $ \mathcal{P} $ be a set of problems
* $ \mathcal{S}(p) $ be the set of solutions to problem $ p $ achieving accuracy $\ge \epsilon$

Define a **solution representation space** with distance:
```math
d(s_i, s_j)
```

Two solutions are **distinct** if:
```math
d(s_i, s_j) > \delta
```

---

#### 4.2 Creativity as Space Expansion

A transformation $T$ (creative act) maps:
```math
(\mathcal{P}, \mathcal{S}) \rightarrow (\mathcal{P}', \mathcal{S}')
```

Creativity is quantified by:

1. **Solution Diversity Gain**
   ```math
   \Delta_S = \sum_{p \in \mathcal{P}} \left( |\text{Distinct}(\mathcal{S}'(p))| - |\text{Distinct}(\mathcal{S}(p))| \right)
   ```

2. **Problem Unlocking**
   ```math
   \Delta_P = |\{p \in \mathcal{P}' \setminus \mathcal{P} : \exists s \in \mathcal{S}'(p)\}|
   ```

3. **Efficiency Gain**
   ```math
   \Delta_C = \mathbb{E}_{p} \left[ C_{\text{before}}(p) - C_{\text{after}}(p) \right]
   ```

Where $C(p)$ is computational cost (time, space, or cognitive steps).

---

### 5. Representation as the Locus of Creativity

A central mechanism of creativity is:

> **The introduction of new intermediate representations**

Examples:

* Language (symbolic abstraction)
* Coordinates in physics
* Pharmacophores in molecular graphs
* Latent spaces in ML

These representations:

* compress complexity
* enable reuse
* support compositional reasoning
* amortize computation across tasks

---

### 6. Concept Formation as Graph Compression

We model knowledge as a graph:

* Nodes: concepts
* Edges: relations
* Subgraphs: structured patterns

A **creative act** performs:

> **Subgraph abstraction → node creation**

That is:
```math
G \rightarrow G'
```
where a subgraph $H \subset G$ becomes a new node $v_H$

---

#### Key Metric: Concept Reuse

Let $c$ be a newly introduced concept.

Define:
```math
\text{Reuse}(c) = |\{p \in \mathcal{P} : c \in \text{solutions of } p\}|
```

A concept is **creative** if:

* it recurs across many problems
* it simplifies solution construction
* it enables further abstractions

---

### 7. Operator Efficiency and Reframing

Creativity also manifests as:

> **Changes in the efficiency of operations**

Let:

* $\mathcal{O}$: set of operations (transformations, reasoning steps)

A reframing $T$ induces:
```math
\mathcal{O} \rightarrow \mathcal{O}'
```

We measure:
```math
\Delta_{\text{op}} = \mathbb{E}[\text{cost}(\mathcal{O}) - \text{cost}(\mathcal{O}')]
```

Crucially:

* creativity may **not reduce steps directly**
* instead, it **changes what counts as input**, or
  **what constitutes a step**

---

### 8. Interest as a Selection Function

Not all combinatorial expansions matter.

Define:

* $I(p)$: latent *interest function* over problems

We do not observe $I$, but approximate it via:

* citations
* reuse
* engagement
* downstream developments

---

#### Creative Value (Interest-Weighted)

```math
\text{Creativity}(T) = \sum_{p \in \mathcal{P}'} I(p) \cdot \mathbf{1}[\text{newly enabled}]
```

Thus:

> Creativity is not combinatorial expansion, but **expansion of *valuable* regions of the space**.

---

### 9. Reframing as Hypothesis Space Transformation

A reframing:

* introduces new variables
* changes dimensionality
* alters decomposition

Formally:
```math
\mathcal{H} \rightarrow \mathcal{H}'
```

Where:

* $\mathcal{H}'$ includes hypotheses not expressible in $\mathcal{H}$

Example:

* From pixel space → object space
* From atom-level → pharmacophore-level
* From events → relational structures

---

### 10. Creativity as Amortized Efficiency

A powerful reframing:

* incurs initial cost $C_0$
* yields repeated savings across tasks

Define amortized gain:
```math
\text{Gain} = \sum_{p \in \mathcal{P}} \left( C_{\text{before}}(p) - C_{\text{after}}(p) \right) - C_0
```

Creativity is high when:

* upfront abstraction cost is low relative to
* long-term reuse benefits

---

### 11. Synthesis: Operational Definition

A transformation $T$ is creative if it satisfies:

1. **Unlocking**
   Enables new solvable or meaningful problems

2. **Expansion**
   Increases diversity of distinct solutions

3. **Compression**
   Introduces reusable abstractions

4. **Efficiency**
   Reduces computational or cognitive cost

5. **Propagation**
   Concepts recur across domains

6. **Interest Alignment**
   Unlocks problems that are actually pursued

---

### 12. Implications

#### For Machine Learning

* Move beyond fixed representations
* Evaluate models by **space expansion**, not just accuracy
* Incorporate **concept formation and reuse tracking**

#### For Scientific Discovery

* Measure theories by:

  * number of new problems enabled
  * reduction in explanatory complexity
  * abstraction reuse

#### For Generative Models

* Evaluate not just output quality, but:

  * **new solution classes generated**
  * **new abstractions discovered**

---

### 13. Open Challenges

* Defining robust distance metrics over solutions
* Modeling the latent interest function $I(p)$
* Representing heterogeneous solution types
* Capturing long-term propagation of concepts
* Avoiding trivial combinatorial expansions

---

### 14. Conclusion

Creativity is not merely novelty or performance.

It is:

> **The restructuring of representation and hypothesis spaces such that new, meaningful, and efficiently solvable regions of the problem landscape become accessible.**

The core measurable signal is not what is produced immediately, but:

* what becomes possible afterward
* what becomes easier
* what becomes worth pursuing

This shifts evaluation from **outputs** to **transformations of possibility**.
