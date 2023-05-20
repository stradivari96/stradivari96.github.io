---
title: "🎓 How the CP-SAT solver works"
date: 2020-04-25
draft: false

aliases:
  - /explaining-cp-sat

tags: ["Operations research", "ortools", "CP-SAT"]
cover:
  image: "https://pbs.twimg.com/media/DuoN35ZXgAAKzC_.jpg"
---

Overview of the CP-SAT solver from Google OR-Tools

<!--more-->

## References:

- Laurent Perron and Frédéric Didier, CPAIOR 2020: https://youtu.be/lmy1ddn4cyw
- Peter J. Stuckey, Search is Dead: https://people.eng.unimelb.edu.au/pstuckey/PPDP2013.pdf
- Dominik Krupke, CP-SAT primer: https://github.com/d-krupke/cpsat-primer

## Model Building

The first step is building the model using the `CPModel` class. This class is actually a **wrapper** around the [cp_model](https://github.com/google/or-tools/blob/stable/ortools/sat/cp_model.proto) protobuf.

Let's see an example ([source](https://github.com/google/or-tools/blob/stable/ortools/sat/samples/simple_sat_program.py)):

```python
from ortools.sat.python import cp_model

"""Minimal CP-SAT example to showcase calling the solver."""
# Creates the model.
# [START model]
model = cp_model.CpModel()
# [END model]

# Creates the variables.
# [START variables]
num_vals = 3
x = model.NewIntVar(0, num_vals - 1, 'x')
y = model.NewIntVar(0, num_vals - 1, 'y')
z = model.NewIntVar(0, num_vals - 1, 'z')
# [END variables]

# Creates the constraints.
# [START constraints]
model.Add(x != y)
# [END constraints]

# Creates a solver and solves the model.
# [START solve]
solver = cp_model.CpSolver()
status = solver.Solve(model)
# [END solve]

if status == cp_model.FEASIBLE:
    print('x = %i' % solver.Value(x))
    print('y = %i' % solver.Value(y))
    print('z = %i' % solver.Value(z))
```

This model creates the following proto `print(str(model))`:

```
variables {
  name: "x"
  domain: 0
  domain: 2
}
variables {
  name: "y"
  domain: 0
  domain: 2
}
variables {
  name: "z"
  domain: 0
  domain: 2
}
constraints {
  linear {
    vars: 1
    vars: 0
    coeffs: -1
    coeffs: 1
    domain: -9223372036854775808
    domain: -1
    domain: 1
    domain: 9223372036854775807
  }
}
```

**Note**: int64 is [-9223372036854775808, 9223372036854775807]

## Presolve Loop

> First stage:
> We will process all active constraints until a fix point is reached. During
> this stage:
>
> - Variable will never be deleted, but their domain will be reduced.
> - Constraint will never be deleted (they will be marked as empty if needed).
> - New variables and new constraints can be added after the existing ones.
> - Constraints are added only when needed to the mapping_problem if they are
>   needed during the postsolve.
>
> Second stage:
>
> - All the variables domain will be copied to the mapping_model.
> - Everything will be remapped so that only the variables appearing in some
>   constraints will be kept and their index will be in [0, num_new_variables). - [source](https://github.com/google/or-tools/blob/5ff76b487a6c2006326765d6417964599eedc8c9/ortools/sat/cp_model_presolve.cc#L4581)

1. [Presolve](https://github.com/google/or-tools/blob/stable/ortools/sat/cp_model_presolve.cc):
   Domain reduction, constraint simplification/rewrite
2. [Constraint expansion/decomposition](https://github.com/google/or-tools/blob/stable/ortools/sat/cp_model_expand.cc):
   Similar to Minizinc -> Flatzinc (element constraint, table, automaton, inverse, product, modulo, reservoir)
3. Detect variable equivalence and affine relations.
4. Substitute by canonical representation.
5. Probing: Fix variables and see what is propagated.

This produces 2 new models, the **inner model** that will be solved and a **channeling model** used to populate the solution of the **initial model**. - [source](https://github.com/google/or-tools/issues/912#issuecomment-436292989)

## Solver

> The CP-SAT solver uses a **lazy clause generation** solver on top of an **SAT** solver. The best description is a presentation from **Peter Stuckey** called [Search is Dead](https://people.eng.unimelb.edu.au/pstuckey/PPDP2013.pdf) - [Laurent Perron](https://stackoverflow.com/questions/57123397/which-solver-do-googles-or-tools-modules-for-csp-and-vrp-use/57125734#57125734)

In Lazy clause generation (LCG), integer variables are **encoded as booleans**, ortools creates 2 booleans for each variable and value:

- var == value
- var <= value

Note: var >= value (represented as ![x <= value-1])

- (var == value) <=> (var >= value) and (var <= value)
- (var <= value) => (var <= value+1)

> Propagation is clause generation:

- e.g. [x <= 2] and x >= y means that [y <= 2]
- clause [x <= 2] => [y <= 2]

## Linear relaxation

TODO

## Default search (single thread)

VSIDS on the Boolean problem, when it reaches a fixed point, it asks the heuristic to select an integer variable, a value and a braching direction.

## Multithreading

> The solver uses the first **X** threads to generic methods, and use all the remaining ones on LNS (Large Neighborhood Search). -[Laurent Perron](https://github.com/google/or-tools/issues/1718#issuecomment-554004069)
