# Plots

[back to index](../../)

We are creating a task network that generates simple plots with some constraints.

```htn
package com.ursafrontier.dhsi2024

domain fiction.plots
```

## Action Space

The action space consists of all of the plot points we can create.

```htn
action PretendToBeWealthy(
    string actor
)
costs {
  deception = 1
}
```

## Plot Points

Each plot point is a short series of actions with a follow-on plot point. These are represented by a macro that represents the plot point and a task that represents the possible follow-on plot point invocations.

### Love and Courtship

#### Love's Beginning

```htn
; A, poor, is in love with wealthy and aristocratic B
; A, poor, in love with wealthy B, pretends to be a man
; of wealth
love-1a(a, b) achieves love-1 when is-poor(a), is-aristocratic(b), in-love-with(a, b) {
    PretendToBeWealthy(actor=a)
    love-1-followons(a, b)
}

love-1a-187(a, b) achieves love-1-followons {
    love-187(a, b)
}

; A, in love with B and wishing to propose marriage, finds 
; it impossible because B is so busy he can never find her
; alone. He seeks to make an opportunity by stratagem
love-187(a, b) when in-love-with(a, b) {
    implies wishes-to-propose-marriage(a, b), too-busy(b)
    love-187-followons(a, b)
}

love-187-163(a, b) achieves love-187-followons {
    love-163(a, b)
}
```