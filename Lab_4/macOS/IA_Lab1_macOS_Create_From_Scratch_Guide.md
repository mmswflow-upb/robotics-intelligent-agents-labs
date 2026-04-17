# IA Lab 1 — macOS NetLogo Guide (Create the Model From Scratch)

Important:
This version is for **macOS** and assumes you do **not** already have `lab1.nlogo`.
You will create a new NetLogo model from scratch.

## PART 1 — INSTALL AND OPEN NETLOGO ON MACOS

1. Download NetLogo from the official website.
2. Open the `.dmg`.
3. Drag **NetLogo** to **Applications**.
4. Start NetLogo.
5. If blocked, use **System Settings -> Privacy & Security -> Open Anyway**.

## PART 2 — CREATE A NEW MODEL

1. Click **File -> New**
2. Go to **Code**
3. Paste the code below
4. Save as `lab1.nlogo`

Important:
If NetLogo says a variable like `number-turtles` is missing, create the matching widget first.

## PART 3 — PASTE THIS STARTER CODE

```netlogo
breed [walkers walker]
breed [runners runner]

globals [
  total-distance-traveled
]

turtles-own [
  distance-traveled
]

to setup
  clear-all
  set-default-shape walkers "person"
  set-default-shape runners "car"

  let num-walkers floor (number-turtles * priority-walkers / 100)
  let num-runners (number-turtles - num-walkers)

  create-walkers num-walkers [
    set color blue
    set size 1.2
    setxy random-xcor random-ycor
    set heading random 360
    set distance-traveled 0
  ]

  create-runners num-runners [
    set color red
    set size 1.4
    setxy random-xcor random-ycor
    set heading random 360
    set distance-traveled 0
  ]

  ask patches [
    set pcolor black
  ]

  set total-distance-traveled 0
  reset-ticks
end

to go
  ask walkers [
    rt random walker-turn-angle
    lt random walker-turn-angle
    move-agent walker-step walker-patch-color
  ]

  ask runners [
    rt random runner-turn-angle
    lt random runner-turn-angle
    move-agent runner-step runner-patch-color
  ]

  set total-distance-traveled sum [distance-traveled] of turtles
  tick
  wait 0.08
end

to move-agent [step-size trail-color-input]
  if bounce-walls? [
    if xcor >= max-pxcor or xcor <= min-pxcor [
      set heading (- heading)
    ]
    if ycor >= max-pycor or ycor <= min-pycor [
      set heading (180 - heading)
    ]
  ]

  fd step-size
  set distance-traveled distance-traveled + step-size

  if trail? [
    ask patch-here [
      set pcolor resolve-patch-color trail-color-input
    ]
  ]
end

to-report resolve-patch-color [value]
  if is-number? value [ report value ]
  if is-string? value [
    if value = "green"  [ report green ]
    if value = "yellow" [ report yellow ]
    if value = "red"    [ report red ]
    if value = "blue"   [ report blue ]
    if value = "white"  [ report white ]
    if value = "gray"   [ report gray ]
    if value = "black"  [ report black ]
    if value = "cyan"   [ report cyan ]
    if value = "violet" [ report violet ]
    if value = "pink"   [ report pink ]
    if value = "orange" [ report orange ]
  ]
  report green
end
```

## PART 4 — BUILD THE INTERFACE FROM SCRATCH

Buttons:
- `setup`
- `go` as a forever button

Sliders:
- `number-turtles`: 0..300, step 1, initial 100
- `priority-walkers`: 0..100, step 1, initial 50
- `walker-turn-angle`: 0..90, step 1, initial 5
- `runner-turn-angle`: 0..90, step 1, initial 8
- `walker-step`: 0..2, step 0.01, initial 0.01
- `runner-step`: 0..3, step 0.01, initial 0.02

Switches:
- `bounce-walls?` off
- `trail?` on

Input boxes:
- `walker-patch-color` = `"blue"`
- `runner-patch-color` = `"red"`

Monitor:
- `total-distance-traveled`

Recommended extra monitors:
- `count walkers`
- `count runners`

## PART 5 — TOPOLOGY

Topology means how the world edges behave:
- Torus = wraps on both axes
- Cylinder = wraps on one axis only
- Box = no wrapping

To change it:
1. Go to **Interface**
2. Right-click the world or open **Settings / World & View**
3. Leave only one wrap direction active
4. Press **Setup** again

## PART 6 — TASK MAPPING

- Monitor = `total-distance-traveled`
- Topology = change torus to cylinder
- Appearance = shapes/colors
- Patch color = two inputs
- Bonus = reverse behaviors without code edits
- Extra = `priority-walkers` percentage slider

`priority-walkers` changes the percentage of walkers created at setup.
It does **not** change runtime priority.

Example:
- `5` with `200` turtles = `10` walkers and `190` runners

## PART 7 — TROUBLESHOOTING

If a variable is missing, create the matching widget.
If movement is too fast:
- lower step values
- increase `wait`

If the split seems wrong:
- remember `priority-walkers` is a percentage
- add `count walkers` and `count runners` monitors
- press **Setup** again after changing it
