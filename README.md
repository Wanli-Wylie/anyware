# Anyware

**Agent-native software representation.**

Software systems decompose into three components:

```
Program = PURE(data) + Control Flow(static + dynamic) + STATE(Index, Cmd, Qry)
```

**PURE blocks** are self-contained computation with closed symbol tables. Locally verifiable, content-addressed, independently testable. No external dependencies, no side effects, no shared state.

**Control flow** is how execution transfers between blocks. Three kinds:
- **Sequential edges** — block A finishes, block B starts
- **Branching edges** — conditional transfer (if/loop/select)
- **LINK edges** — call-return to another unit (static target or dynamic dispatch)

Dynamic LINKs absorb function pointers, virtual dispatch, callbacks, and higher-order functions into the control flow graph. The PURE blocks on both sides remain first-order and testable.

**STATE** is black-boxed side effects. Each state is a triple `(Index, Cmd, Qry)` — how you address the resource, what changes it, what observes it. Every side effect in every language maps to this structure.

## The principle

Three observations:

1. **AI can localize computation.** Given a codebase, an AI agent can identify and extract the pure computational logic of any code snippet — separating it from the state operations and control flow that surround it.

2. **AI can infer input distributions.** For any pure function, an AI agent can iteratively reason about the parameter space — dimension dependencies, physical constraints, domain semantics — to produce probability distributions that serve as baselines for differential testing.

3. **Symbolic checks close the loop.** Mechanical predicates (barrier detection, type checking, signature matching) verify that extracted blocks are self-contained and compilable. Once symbolic checks pass, the block is directly testable — no build system, no global context needed.

Together: agents extract PURE blocks, infer their test distributions, and verify them symbolically. The result is a representation where every piece of software is continuously verifiable, translatable, and optimizable.

## Properties

| Property | What it enables |
|---|---|
| **Content-addressed** | Identity = SHA256(content). Agents produce, compare, cache without coordination. |
| **Locally verifiable** | Each block checked independently. No global analysis needed. |
| **Decentralized symbol tables** | Each PURE block is self-contained. Agents work in parallel with no shared state. |
| **Typed interfaces** | Edges carry typed data channels. Composition is mechanical. |
| **Immutable history** | All transformations recorded in append-only version trees. |
| **Functor condition** | Translation = F where F(PURE) = transformed, F(STUB) = identity, F(Control Flow) = preserved. Verification decomposes locally. |

## The decomposition theorem

Every barrier to testability maps to a state type. Every state type has `(Index, Cmd, Qry)`. Seven atomic rewrite rules either eliminate, externalize, or separate state from computation. The process converges because the barrier set is finite and every barrier has at least one resolution.

Every function pointer, virtual method, callback, and higher-order argument is a control flow edge — a LINK with a typed signature. PURE blocks on both sides are first-order and testable. Higher-order programs decompose the same way as first-order programs; you just get more LINK edges and smaller PURE blocks.

```
ANY program = PURE(first-order, data only)
            + Control Flow(static + dynamic edges)
            + STATE(Index, Cmd, Qry)
```

No exceptions.

## Continuous evolution

The same loop drives all software evolution:

```
branch → check → select → repeat
```

- **Refactoring** = expand the testable surface (maximize PURE coverage)
- **Translation** = map PURE blocks to a new language (verify by differential testing)
- **Optimization** = propose faster implementations (verify correctness, benchmark performance)
- **Migration** = assemble verified pieces into the new system (composition)

Same mechanism, different objective. Same version tree, different edges. Same representation throughout.

## Applications

| Domain | Application | Status |
|---|---|---|
| Legacy numerical modeling (Fortran) | [anyware-fortran-numerical-modeling](https://github.com/Wanli-Wylie/anyware-fortran-numerical-modeling) | Active |

## License

MIT
