# Creativity as Decomposition-Induced Evolvability in Generative Graph Models

## Abstract

We propose a definition of creativity for generative models based on *decomposition-induced evolvability*. A generator is said to be creative if it induces a decomposition of its outputs such that instances generated under this decomposition can be efficiently evolved—via simple local substitutions—into task-critical solutions across a range of external tasks. Unlike conventional approaches that evaluate generators solely through sample quality, diversity, or likelihood, we treat the generator’s **decomposition** as a representational theory that determines which structures are accessible and recombinable. Creativity is quantified by measuring how efficiently generated instances can be transformed, using only the generator’s own decomposition, into boundary-critical instances (support vectors) defined by downstream tasks. This framework yields a principled, task-grounded, and operational measure of creativity aligned with both biological evolvability and human conceptual innovation.



## 1. Motivation

Many generative models are evaluated on how well they reproduce or interpolate within a data distribution. Yet the notion of *creativity* suggests something stronger: the capacity to generate artifacts that serve as fertile starting points for solving problems that were not explicitly optimized for.

In biology, this property is formalized as **evolvability**—the ability of a system to generate heritable variation that can be efficiently selected toward new objectives. Valiant’s computational theory of evolvability makes this notion precise by constraining the evolutionary process to simple, local operations.

In human creativity, the most impactful contributions are often not solutions but **representational frameworks**: germ theory, spacetime curvature, or expressionist aesthetics. These frameworks introduce decompositions that make previously inaccessible solutions reachable by modest effort.

Evaluating creativity purely by novelty or sample diversity conflates surprising outputs with useful representational structure. A generator can produce diverse artifacts yet still offer no principled way to recombine parts into task-relevant solutions. Conversely, a generator with a strong internal decomposition can make modest edits disproportionately powerful, because its parts align with problem structure that the generator never explicitly saw.

This matters in practice because many downstream tasks are defined by sparse feedback or hard constraints. When optimization signals are weak, the success of local search depends on whether the underlying representation exposes meaningful, swappable substructures. If the decomposition is poor, local edits lead to dead ends; if it is good, small edits yield rapid progress toward constraint-satisfying solutions.

We therefore seek a measure that rewards representations that generalize across tasks without solving any one task directly. The proposed definition isolates the generator’s contribution by fixing tasks externally and measuring how efficiently its decomposition supports local transformation toward task-critical boundaries.

This paper unifies these perspectives by treating creativity in generative models as a property of the **decomposition they induce** and the **evolvability of the instances generated under that decomposition**.



## 2. Core Thesis

**A generative model is creative if the decomposition it induces makes task-critical structures reachable from its samples via few, simple recombination steps.**

Creativity is therefore not a property of samples alone, nor of tasks alone, but of the *interaction* between:

* a generator,
* the decomposition it induces,
* simple local transformations defined by that decomposition,
* and externally defined tasks.



## 3. Generator and Decomposition

Let:

* $\mathcal{G}$ be a space of structured objects (e.g., graphs or molecules).
* $\mathcal{A}$ be a generative algorithm inducing a distribution $P_{\mathcal{A}}$ over $\mathcal{G}$.

Crucially, the generator also induces a **decomposition**:

$$
D \equiv D_{\mathcal{A}} : \mathcal{G} \rightarrow \mathcal{P}(\mathcal{C}),
$$

which maps each graph to a set of components (subgraphs, motifs, fragments) with well-defined interfaces.

The decomposition $D$:

* defines what the primitive parts are,
* defines how parts can be recombined,
* implicitly defines a notion of locality and admissible mutations.

The generator outputs a set of samples:

$$
\mathcal{S}_{\mathcal{A}} = \{g_1,\dots,g_n\} \sim P_{\mathcal{A}},
$$

but these samples are meaningful only *relative to the decomposition* that produced them.



## 4. Tasks and Boundary-Critical Instances

Let $\mathcal{T} = \{T_1,\dots,T_K\}$ be a fixed collection of tasks.

For each task $T_k$, assume:

* a scoring or classifier function $f_k : \mathcal{G} \to \mathbb{R}$,
* feasibility constraints defining valid solutions.

From this, we identify a set of **boundary-critical instances**:

$$
\mathcal{B}_k \subset \mathcal{G},
$$
defined as graphs that:

1. lie close to the task’s decision boundary,
2. are on the feasible or correct side,
3. do not violate constraints.

These instances play the role of *support vectors*: they encode the essential structure of the task.

Importantly, tasks do **not** define decompositions. They only define *what matters*.



## 5. Donor Parts: Task Examples, Generator Decomposition

For each task $T_k$, we define a donor graph set:

$$
\mathcal{G}^{\mathrm{don}}_k \subseteq \mathcal{G},
$$
typically chosen as $\mathcal{B}_k$ or a slightly enlarged set of high-quality task solutions.

The key constraint is that **donor graphs are decomposed using the generator’s decomposition $D$**:

$$
\mathcal{P}_k(D) = \bigcup_{h \in \mathcal{G}^{\mathrm{don}}_k} D(h).
$$

Thus:

* tasks provide *exemplars*,
* the generator provides the *language of parts*.

This asymmetry is deliberate: it isolates the representational contribution of the generator.



## 6. Swap Operator Induced by the Decomposition

Using $D$, we define a local mutation operator.

Given a current graph $g$:

1. Decompose it into components $D(g)=\{c_1,\dots,c_m\}$.
2. Select a component (c_i).
3. Select a donor component $c' \in \mathcal{P}_k(D)$ compatible with $c_i$.
4. Form a new graph:

$$
g' = \mathrm{Swap}_D(g, c_i, c').
$$

This defines a task-conditioned neighborhood:

$$
\mathcal{N}_{k,D}(g) = \{g' \mid g' = \mathrm{Swap}_D(g,c,c'),\; c\in D(g),\; c'\in\mathcal{P}_k(D)\}.
$$

All admissible evolution is constrained by the generator’s decomposition.



## 7. Evolution Objective

For each task $T_k$, define an objective that measures proximity to the boundary-critical set.

A canonical choice is:

$$
J_k(g) = \min_{b \in \mathcal{B}_k} d_k(g,b),
$$
where $d_k$ is a task-specific metric (e.g., margin distance, embedding distance).

The evolutionary goal is to minimize $J_k$.



## 8. Evolutionary Process

Starting from a generated instance $g^{(0)} \in \mathcal{S}_{\mathcal{A}}$, we perform a bounded evolutionary search:

$$
g^{(t+1)} = \arg\min_{g' \in \mathcal{N}_{k,D}(g^{(t)})} J_k(g').
$$

The search is deliberately simple:

* greedy or beam search,
* fixed depth or budget,
* no gradients, no global planning.

This ensures we are measuring **evolvability**, not raw optimization power.



## 9. Evolvability and Creativity Metrics

For each starting graph and task, define either:

**Time-to-reach**

$$
\tau_{k,D}(g) = \min \{t : J_k(g^{(t)}) \le \varepsilon\},
$$

or **Quality-at-budget**

$$
Q_{k,D}(g;T) = -\min_{t\le T} J_k(g^{(t)}).
$$

Aggregate over generated samples:

$$
E_k(\mathcal{A}) = \mathbb{E}_{g \sim P_{\mathcal{A}}}[\psi(\tau_{k,D}(g)) \text{ or } Q_{k,D}(g;T)].
$$

Define overall creativity as:

$$
\mathrm{Creativity}(\mathcal{A}) = \frac{1}{K}\sum_{k=1}^K E_k(\mathcal{A}).
$$



## 10. Interpretation

This metric captures a precise notion of creativity:

* The generator is not rewarded for solving tasks directly.
* It is rewarded for inducing a decomposition under which:

  * task-relevant parts can be extracted,
  * simple swaps suffice to reach task boundaries,
  * many tasks are simultaneously evolvable.

A creative generator discovers a *representation* that aligns with many external problems.

---

## 11. Limitations and Robustness Checks

The framework is promising but needs guardrails to avoid overclaiming. Below are concrete limitations and targeted fixes that can be added to the evaluation protocol.

1. **Representation circularity**: The decomposition is induced by the generator and also used to judge evolvability, so a generator could appear creative by defining a trivial swap space.
   * **Check**: Compare against baseline decompositions that are task-agnostic (e.g., random fragments, fixed-size cuts) and report relative gains.

2. **Metric sensitivity**: Boundary-critical sets depend on $d_k$ and task embeddings, which can dominate the outcome.
   * **Check**: Evaluate robustness across multiple $d_k$ choices and report variance of $E_k(\mathcal{A})$.

3. **Donor leakage**: Donor sets encode task structure; if too generous, they can mask weak decompositions.
   * **Check**: Vary donor set size and quality (e.g., boundary-only vs. expanded) and measure stability of rankings.

4. **Swap validity**: Defining compatible interfaces is nontrivial and can effectively bake in task-specific rules.
   * **Check**: Report validity rates of swaps, and include an ablation with a stricter, domain-independent compatibility rule.

5. **Search bias**: Even simple search can privilege decompositions with large neighborhoods.
   * **Check**: Normalize for neighborhood size or compare greedy vs. beam at fixed budget to see if ordering changes.

6. **Analogy overreach**: The human creativity analogy motivates the framework but should be framed as an interpretation, not evidence.
   * **Check**: Add empirical results or case studies demonstrating cross-task transfer under fixed $D$.



## 12. Relation to Human Conceptual Creativity

This framework mirrors paradigm-level human creativity:

| Human contribution | Framework analogue              |
| ------------------ | ------------------------------- |
| New theory         | Generator-induced decomposition |
| New primitives     | Components under $D$            |
| New laws           | Swap compatibility constraints  |
| New predictions    | Reachable instances             |
| Paradigm power     | Evolvability across tasks       |

Creativity lies in making important structures *reachable by simple means*.



## 13. Conclusion

We redefine creativity in generative modeling as **decomposition-induced evolvability**. A generator is creative if the decomposition it induces allows its samples to be efficiently evolved—using only local swaps—into task-critical solutions across diverse tasks. This shifts evaluation from surface-level novelty to representational power, aligning machine creativity with biological evolution and human conceptual breakthroughs.

This framing turns evaluation into a test of representation: does the generator expose parts and interfaces that make meaningful search feasible under weak supervision? It also cleanly separates task specification from representational contribution by fixing tasks externally while attributing all operational structure to $D$.

Practically, the proposal suggests a research agenda: design generators whose induced decompositions maximize cross-task evolvability, develop principled component interface constraints, and build benchmark suites that stress generalization to unseen tasks. The framework also invites comparisons between decompositions learned from data versus those imposed by domain heuristics.

Finally, the approach offers an interpretable bridge between machine creativity and evolvability: creativity is not just the production of surprising artifacts, but the discovery of a representational substrate that makes many solutions reachable by modest, local change. This is a measurable claim, and it can be validated or falsified through the robustness checks above.
