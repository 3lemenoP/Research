
This document outlines a formal mathematical framework aimed at unifying the Free Energy Principle (FEP) with Viability Theory. The goal is to provide a rigorous basis for understanding how autonomous agents can maintain existence within critical constraints (viability) while adaptively modeling their environment and acting purposefully (FEP). This exposition presents the core definitions, key hypotheses derived from the framework, supporting mathematical derivations, and discusses potential theoretical implications. It should be considered a work in progress; several postulates and convergence arguments require further rigorous proof, which is noted throughout.

# 1. Preliminary Definitions and Notation

**State Spaces:** Let $\mathcal{X}$ be the state space of the environment, assumed to be a measurable space (e.g., $\mathbb{R}^n$). Let $\mathcal{M}$ represent the space of the agent's internal states, which typically encode beliefs (probability distributions) about the external state $x \in \mathcal{X}$. For instance, $m \in \mathcal{M}$ could parameterize a variational posterior $q(x|m)$ over $x$.

**Observation Space and Generative Model:** Let $\mathcal{O}$ be the space of observations $o \in \mathcal{O}$. The agent is assumed to possess a *generative model*, a probabilistic description of how observations and states arise, potentially dependent on actions $a \in \mathcal{A}$ (Action Space) and latent parameters $\theta \in \Theta$ (Parameter Space). A common factorization is:
*   **State-transition model:** $p(x_{t+1} | x_t, a_t, \theta)$ (dynamics).
*   **Observation likelihood:** $p(o_t | x_t, \theta)$ (sensory mapping).
The agent may have uncertainty about $\theta$, represented by a prior $p(\theta)$.
*(Assumption: The agent's model class is assumed to correctly capture the structure of the true environmental dynamics, corresponding to some $\theta^* \in \Theta$. This is a standard but strong assumption in Bayesian modeling).*

**Policy:** A policy $\pi$ determines action selection, potentially based on the agent's internal belief state $m_t$: $a_t = \pi(m_t)$.

**Viability Constraints:** A subset $K \subset \mathcal{X}$ defines the **viability set** (or safe set). The system must remain in $K$ to be functional. The **viability kernel**, $V(K)$ (typically $V_\infty(K)$ for the infinite horizon), is the set of initial states $x_0 \in K$ from which there *exists* at least one policy $\pi$ such that the resulting state trajectory $x_t$ remains in $K$ for all $t \ge 0$.

**Uncertainty Representation:** The agent maintains a belief about hidden variables $(\theta, x_t)$, often represented by a variational distribution $q_t(\theta, x_t)$ approximating the true posterior $p(\theta, x_t | o_{1:t}, a_{0:t-1})$.

**Free Energy and Bayesian Inference:** The Variational Free Energy (VFE) for an observation $o_t$ and belief $q_t$ is:
$F_t[q] = \mathbb{E}_{q_t(\theta,x)}[-\ln p(o_t, x, \theta)] - \mathbb{E}_{q_t(\theta,x)}[-\ln q_t(\theta, x)]$.
Minimizing $F_t$ with respect to $q_t$ is equivalent to minimizing $\mathrm{KL}(q_t(\theta,x) \| p(\theta,x | o_t))$, thus driving the belief towards the true Bayesian posterior.

**Integrating Viability into the Generative Model (Proposed Mechanism):**
The core proposal is to integrate viability constraints into the agent's objective via the VFE. This can be achieved by:
1.  Encoding viability as a strong *prior preference*: States $x \notin K$ are deemed highly improbable a priori.
2.  Introducing a *viability penalty function* $\phi(x) \ge 0$ such that $\phi(x)=0$ for $x \in K$ and $\phi(x) > 0$ for $x \notin K$.
3.  Defining an **Augmented Free Energy** functional:
    $F_{\text{aug}}[q] = D_{\mathrm{KL}}(q(\theta,x) \| p(\theta,x|o)) + \lambda \mathbb{E}_{q(x)}[\phi(x)]$
    where $\lambda > 0$ weights the viability constraint. As $\lambda \to \infty$, minimizing $F_{\text{aug}}$ enforces $q(x \in K) = 1$. For finite $\lambda$, it penalizes beliefs assigning mass outside $K$. Minimizing this $F_{\text{aug}}$ forms the basis of perceptual updates in this unified framework.

# 2. Key Postulates and Supporting Arguments

The following statements represent key hypotheses or properties expected to hold within the unified framework. They are presented with supporting arguments or proof sketches, noting where full rigor is pending.

**Postulate 1 (Convergence of Perception under Ideal Conditions).**
*Statement:* Assume the agent's generative model structure is correct and the variational family for $q_t$ is sufficiently expressive to represent the true posterior. Under these ideal conditions, if the agent updates its belief by minimizing $F_{\text{aug}}[q]$ (or standard $F[q]$ if $\lambda=0$), the belief $q_t$ converges towards the true posterior $p(\theta, x_t | o_{1:t}, a_{0:t-1})$ (potentially restricted to $K$ if $\lambda > 0$).
*Argument Sketch:* The decomposition $F[q] = \mathrm{KL}(q \| p(\cdot|o)) - \ln p(o)$ shows that minimizing $F[q]$ minimizes the KL divergence. The global minimum is achieved when $q$ equals the true posterior. The augmented term $\lambda \mathbb{E}[\phi(x)]$ modifies the objective but retains this structure, biasing the posterior towards $K$. Standard arguments suggest iterative optimization converges to this minimum.
*Caveats:* This assumes exact inference. Practical approximate inference (e.g., mean-field) converges to the best approximation within the family. Rigorous analysis of convergence for specific sequential update schemes and approximations is needed. Notation $p(\theta,x|o_t)$ is simplified; full history dependence should be considered.

**Hypothesis 2 (Stability of Robust Viability).**
*Statement:* Let $\Theta_{\text{prior}}$ be the initial uncertainty set for $\theta$. Assume $x_0$ is in the *robust viability kernel* for $\Theta_{\text{prior}}$, meaning a *single* policy $\pi$ exists that guarantees $x_t \in K$ for all $t \ge 0$ and *all* $\theta \in \Theta_{\text{prior}}$. If the agent updates its belief, resulting in a posterior support $\Theta_t \subseteq \Theta_{\text{prior}}$, then $x_0$ remains in the robust viability kernel for $\Theta_t$, and the same policy $\pi$ remains viable.
*Argument Sketch:* The proof follows from set inclusion: $\bigcap_{\theta \in \Theta_{\text{prior}}} V^{(\theta)}(K) \subseteq \bigcap_{\theta \in \Theta_t} V^{(\theta)}(K)$. If $x_0$ is in the LHS, it's in the RHS. The same policy works for the subset $\Theta_t$.
*Caveats:* This hypothesis relies crucially on the assumption that the intersection defining the robust viability kernel guarantees a *uniform* viable policy. This property requires careful definition and verification based on robust control or viability theory literature.

**Hypothesis 3 (Robust Action Selection).**
*Statement:* Assume $x_t$ is in the robust viability kernel for the current uncertainty set $\Theta'$. Then, it is hypothesized that there exists at least one action $a_t$ such that the resulting state $x_{t+1}$ remains in $K$ (or its viability kernel) for all $\theta \in \Theta'$. The agent, by selecting such a robustly viable action at each step, can maintain viability despite model uncertainty.
*Argument Sketch:* Similar to Hypothesis 2, this relies on the existence of a uniform viable policy implied by the robust viability kernel. The first action of such a policy must be robustly viable. Minimizing an objective like $F_{\text{aug}}$ or Expected Free Energy (EFE), which implicitly penalizes constraint violations across the belief distribution $q(\theta)$, should naturally favor such robust actions.
*Caveats:* The existence of the uniform policy from the intersection needs confirmation. The mechanism by which $F_{\text{aug}}$ or EFE minimization precisely selects *this* robust action requires formal demonstration.

**Postulate 4 (Coordination between Perception and Action).**
*Statement:* The framework posits that perception and action coordinate through distinct but coupled optimization processes. Perception updates beliefs $q$ by minimizing VFE (e.g., $F_{\text{aug}}[q]$ based on past data). Action selects controls $a$ by minimizing Expected Free Energy (EFE, denoted $G(a)$) based on current beliefs $q$ and predictions of future outcomes (including viability). An equilibrium $(q^*, a^*)$ is hypothesized where beliefs accurately reflect the posterior given the history generated by actions, and actions are optimal given those beliefs, resulting in self-consistent behavior.
*Argument Sketch:* This separates the objectives: VFE minimization drives inference towards accurate posterior beliefs (Postulate 1). EFE minimization drives action selection towards preferred and informative future states (see Postulate 6). Coordination arises because the actions chosen (minimizing EFE based on $q$) influence future observations, which then shape the next beliefs (via VFE minimization), closing the loop. The equilibrium is a fixed point of this iterative process.
*Caveats:* The existence and uniqueness of the equilibrium $(q^*, a^*)$ and the convergence of the iterative perception-action loop require formal proof, likely involving analysis of the coupled dynamics and possibly drawing on game theory or dynamical systems stability concepts.

**Hypothesis 5 (Adaptive Viability).**
*Statement:* The framework suggests the agent can maintain viability across non-catastrophic environmental changes (e.g., shifts in $\theta$).
*Plausibility Argument:* A change causes surprise (increased VFE), triggering belief updates (via VFE minimization) towards the new $\theta$. Action selection (via EFE minimization) adapts based on the updated beliefs. If the state $x$ remains instantaneously viable during the change and adaptation is sufficiently fast relative to the dynamics near the boundary of $K$, the new policy should maintain viability. The $\phi(x)$ term in $F_{\text{aug}}$ provides a potential buffer by penalizing proximity to the boundary even before the model fully adapts.
*Caveats:* This is a qualitative argument. Rigorous proof requires quantitative analysis of detection delays, belief convergence rates, system dynamics timescales, and the magnitude of the environmental change relative to the viability margins.

**Postulate 6 (Uncertainty-Aware Exploration).**
*Statement:* Action selection based on minimizing Expected Free Energy $G(a)$ inherently balances goal-seeking (exploitation) and information gathering (exploration) in an uncertainty-aware manner. $G(a)$ decomposes into:
$G(a) = \mathbb{E}_{q(x',o'|a)}[ \underbrace{-\ln p(o'|C)}_{\text{Risk/Preference Term}} ] + \mathbb{E}_{q(o'|a)}[ \underbrace{\mathrm{KL}(q(x'|o') \| q(x'))}_{\text{Ambiguity/Info Gain Term}} ]$.
Minimizing $G(a)$ selects actions expected to lead to preferred outcomes (low risk) while also reducing uncertainty about the state/parameters (high information gain), provided the exploration is not overly risky.
*Argument Sketch:* This decomposition is a standard result in FEP. The first term penalizes deviations from preferred outcomes $p(o'|C)$ (which should encode viability). The second term, related to mutual information $I(o'; x'|a)$, rewards actions that yield informative observations. Minimizing the sum naturally trades off these factors.
*Caveats:* The validity relies on the preferences $p(o'|C)$ appropriately encoding the viability constraints $K$.

**Convergence Hypothesis 7.**
*Statement:* Under assumptions of stationarity, identifiability, sufficient exploration, and existence/reachability of an optimal viable policy $\pi^{opt}_{\theta^*}$ for the true environment $\theta^*$, the agent's behavior is hypothesized to converge towards this optimal policy.
*Heuristic Argument:* Beliefs $q(\theta)$ converge towards $\delta_{\theta^*}$ (Postulate 1). Action selection (Postulate 4, 6) based on increasingly accurate beliefs should lead the policy $\pi^{(t)}$ to approach $\pi^{opt}_{\theta^*}$. Performance should asymptotically approach the optimum achievable with full knowledge.
*Caveats:* This is a high-level argument. Formal proof is complex, requiring specific assumptions about the exploration dynamics (ensuring identifiability without violating viability), the policy optimization algorithm, and likely tools from adaptive control or RL theory (e.g., regret analysis). Convergence is not guaranteed in all scenarios (e.g., local optima, insufficient exploration).

# 3. Supporting Mathematical Derivations

*(This section presents key derivations supporting the framework. These are often standard results applied in this context or direct consequences of the definitions.)*

**3.1 Variational Inference under $F_{\text{aug}}$:** Minimizing $F_{\text{aug}}[q] = \mathbb{E}_q[\ln q - \ln p(o, \theta, x)] + \lambda \mathbb{E}_q[\phi(x)]$ using calculus of variations leads to the stationary condition:
$\ln q^*(\theta,x) = \ln p(o, \theta, x) - \lambda \phi(x) + \text{const}$, implying
$q^*(\theta,x) \propto p(\theta, x | o) \exp(-\lambda \phi(x))$.
This confirms that the optimal belief under $F_{\text{aug}}$ is the standard posterior weighted down in non-viable regions.

**3.2 Bayesian Filtering Equation:** The belief update from $q_t$ to $q_{t+1}$ after action $a_t$ and observation $o_{t+1}$ follows the standard predict-update cycle:
*Predict:* $q_{\text{pred}}( \theta, x_{t+1}) = \int p(x_{t+1} | x_t, a_t, \theta) q_t(\theta, x_t) dx_t$.
*Update:* $q_{t+1}(\theta, x_{t+1}) \propto p(o_{t+1} | x_{t+1}, \theta) q_{\text{pred}}(\theta, x_{t+1})$.
This standard Bayesian filtering structure shows how actions $a_t$ influence belief propagation.

**3.3 Viability Kernel Characterization:** The viability kernel $V(K)$ satisfies the fixed-point relation $V(K) = K \cap \text{Pre}(V(K))$, where $\text{Pre}(S) = \{x | \exists a: f(x,a) \in S\}$ (deterministic case). This recursive definition links viability to one-step transitions and forms the basis for theoretical analysis and computational algorithms in viability theory, often related to Hamilton-Jacobi-Bellman equations. The unified framework aims to achieve this implicitly through online optimization rather than offline computation of $V(K)$.

**3.4 Expected Free Energy Decomposition:** As sketched in Postulate 6, standard derivations show that EFE can be decomposed into terms reflecting expected alignment with preferences (risk) and expected reduction in uncertainty (information gain/ambiguity). This decomposition provides the theoretical basis for balanced exploration-exploitation emerging from EFE minimization.

**3.5 Stability Perspective:** While rigorous proof is pending, the VFE ($F_{\text{aug}}$) or the value function associated with EFE minimization serves as a *candidate* Lyapunov function for the coupled agent-environment system. A decrease in free energy corresponds to reduced prediction error and reduced viability penalty, suggesting convergence towards a stable, viable, and predictable (homeostatic) regime. Formalizing this requires analyzing the specific dynamics of the belief and state updates.

# 4. Theoretical Implications (Hypothesized)

Based on the framework and postulates outlined above, several theoretical implications arise, suggesting a powerful synthesis of adaptation, control, and survival. These implications are contingent on the underlying hypotheses holding true under rigorous analysis.

1.  **Unified Objective:** The framework suggests VFE/EFE minimization provides a single objective driving both perception and action towards self-consistency and viability.
2.  **Embodied Bayesian Cognition:** It supports the view of agents as Bayesian inference systems whose primary goal, encoded in priors, is survival (maintaining viability).
3.  **Intrinsic Resilience:** Viability constraints embedded in the objective function may confer resilience by biasing inference and action away from catastrophic states.
4.  **Principled Exploration:** The EFE formulation suggests exploration arises naturally from uncertainty reduction, balanced against the risk of violating viability constraints.
5.  **Adaptive Optimality:** The framework hypothesizes that agents can adapt to changing environments and potentially converge to optimal viable behavior in the long run.
6.  **Viability as Priors:** Provides a way to interpret hard constraints as strong probabilistic preferences within a Bayesian cognitive model.
7.  **Cybernetic Interpretation:** Aligns with cybernetic principles of self-regulation, homeostasis, and adaptation, providing a potential mathematical underpinning via free energy minimization.

