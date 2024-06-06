# Turtle

[back to index](../../)

A turtle can move about on the screen. It does various tasks to keep certain parameters within bounds.

Everything we do here is part of a package. For now, we put everything in the same domain.

```htn
package com.ursafrontier.dhsi2024.turtle

domain turtle.game
```

## Action Space

The action space consists of all of the things that the turtle can choose to do, even if they end up not being successful.

```htn
action TurnLeft { } costs { time=1 energy=-1 }
action TurnRight { } costs { time=1 energy=-1 }
action GoForward { } costs { time=1 energy=-1 }
```

We create a set of macros to capture the effect of the actions.

```htn
turn-left() when energy > 0 {
  TurnLeft()
  implies 
    direction = (direction - 90) % 360, 
    energy = energy - 1
}

turn-right() when energy > 0 {
  TurnRight()
  implies 
    direction = (direction + 90) % 360, 
    energy = energy - 1
}

go-forward() when energy > 0 {
  GoForward()
  implies energy = energy - 2
}

; we don't do anything if we don't have any energy
turn-left() when energy <= 0 { }
turn-right() when energy <= 0 { }
go-forward() when energy <= 0 { }
```

## Complex tasks

```htn
go-forward(n) when n > 0 {
  go-forward()
  go-forward(n-1)
}

go-forward(n) when n == 0 { }

spiral(size) {
  go-forward(size)
  turn-left()
  go-forward(size)
  turn-left()
  go-forward(size)
  turn-left()
  go-forward(size + 1)
  turn-left()
  defer spiral(size+1)
}
costs {
  energy = - 4 - size * 2 - 2
  time = 4 + size * 4 + 1
}
```
