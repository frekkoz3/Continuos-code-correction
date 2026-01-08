# Code Correction in a Continuous Latent Space

This repository investigates the feasibility of performing **code correction**
(from non-working code to fully working code) means of **continuous optimization**
in a **continuous latent representation** of programs.

The core hypothesis is that, if programs can be embedded into a suitably structured
continuous latent space, then gradient-based or heuristic optimization methods can be
used to move from an incorrect program toward a correct one, while preserving
syntactic validity and semantic proximity.

The repository is divided in two parts:

1. Test on the [*continuous latent representation of code*](#continuous-latent-representation-of-code)
2. Selection of the [*continuous optimization technique*](#continuos-optimization-techniques) to use

---

## Continuous Latent Representation of Code

The primary goal of this project is to explore whether it is possible to construct
a **meaningful continuous representation of source code** that supports:

- smooth interpolation between programs
- local semantic transformations
- optimization toward functional correctness
- reliable decoding back to valid code

Several modeling strategies are considered, each offering different trade-offs
between expressivity, validity guarantees, and scalability.

The following approaches are explored and compared:

- Grammar-constrained latent models
- AST-based latent autoencoders
- Denoising encoder–decoder models with a latent bottleneck
- Latent spaces trained via execution semantics

---

### Grammar-Constrained Latent Models

Grammar-constrained latent models aim to ensure **syntactic validity by construction**
by enforcing a formal grammar during decoding.

In this setting:

- Programs are encoded into a continuous latent vector.
- Decoding is constrained by a context-free grammar or AST production rules.
- Only syntactically valid programs can be generated.

**Advantages:**

- Strong guarantees of syntactic correctness
- Well-suited for small languages or domain-specific languages (DSLs)

**Limitations:**

- Requires an explicit grammar
- Difficult to scale to full-featured programming languages
- Limited flexibility for semantic generalization

---

### AST-Based Latent Autoencoders

AST-based latent autoencoders represent programs as **abstract syntax trees** rather
than flat token sequences.

In this approach:

- The encoder maps an AST into a continuous latent representation.
- The decoder reconstructs the AST structure from the latent vector.
- Tree structure is preserved throughout encoding and decoding.

**Advantages:**

- Better alignment with program structure
- Increased robustness to superficial syntactic changes
- Improved semantic stability compared to token-level models

**Limitations:**

- More complex model design
- Language-specific preprocessing
- Decoding errors can still lead to invalid or incomplete trees

---

### Denoising Encoder–Decoder with Latent Bottleneck

This approach leverages powerful encoder–decoder architectures trained with a
**denoising objective**, while explicitly enforcing a **low-dimensional latent bottleneck**.

Key characteristics:

- Input code is corrupted (e.g., token masking, span deletion, syntax perturbations).
- The encoder maps the corrupted code into a compressed continuous latent space.
- The decoder reconstructs the original code from the latent representation.

The bottleneck is designed to prevent trivial copying and to force the model to
learn a compact, continuous representation of program structure and semantics.

**Advantages:**

- Scales well to large datasets and real-world code
- Stable training dynamics
- Compatible with pretrained models (e.g., CodeT5, PLBART)

**Limitations:**

- No formal guarantee of syntactic validity
- Latent space smoothness depends heavily on bottleneck design

---

### Latent Space Trained via Execution Semantics

This approach explicitly incorporates **program behavior** into the training signal.

Instead of relying solely on syntactic reconstruction, the latent space is shaped
using information derived from program execution.

Possible strategies include:

- Executing programs on test inputs
- Comparing outputs or traces
- Penalizing semantic divergence in latent space

**Advantages:**

- Encourages semantic smoothness
- Groups behaviorally equivalent programs
- Particularly suited for code correction tasks

**Limitations:**

- Computationally expensive
- Requires executable programs and test cases
- Often task specific

---

## CONTINUOS OPTIMIZATION TECHNIQUES

Several different ways of optimizing the code is possible once there is a continuos representation for the code.
