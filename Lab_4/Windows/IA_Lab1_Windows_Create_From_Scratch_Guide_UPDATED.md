# IA Lab 1 — Windows NetLogo Guide (Create the Model From Scratch)

Important:
This version is adapted so you can complete Lab 1 **even if you do not already have `lab1.nlogo`**.
You will create a new NetLogo model from scratch, add the code, then build the interface widgets yourself.

This updated version matches the latest decisions from our discussion:
- walkers and runners each have their own input box for patch color
- walkers and runners color patches differently
- the model uses slower default values
- topology is explained clearly
- `priority-walkers` is the extra task, not the bonus
- `total-distance-traveled` means the total distance traveled by all turtles since `setup`

## What you need

Required:
- NetLogo installed on Windows
- the Lab 1 PDF instructions

You do **not** need:
- Ubuntu
- ROS
- RViz
- turtlesim
- CoppeliaSim

## PART 1 — INSTALL AND OPEN NETLOGO

1. Download NetLogo from the official website.
2. Install it normally on Windows.
3. Start NetLogo.

Expected:
- NetLogo opens
- you can see the **Interface**, **Info**, and **Code** tabs

## PART 2 — CREATE A NEW MODEL FROM SCRATCH

1. In NetLogo, click **File -> New**.
2. Go to the **Code** tab.
3. Delete any placeholder text if present.
4. Paste the code from the next section.
5. Save the file as `lab1.nlogo`.

Important:
In NetLogo, many interface widgets create variables.
So if NetLogo complains about something like `number-turtles`, that usually means the code is fine but you have not created the widget with that exact name yet.

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

  create-walkers round (number-turtles * priority-walkers / 100) [
    set color blue
    set size 1.2
    setxy random-xcor random-ycor
    set heading random 360
    set distance-traveled 0
  ]

  create-runners (number-turtles - count walkers) [
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

Create these widgets in the **Interface** tab.

Buttons:
- `setup`
- `go` as a forever button

Sliders:
- `number-turtles`: 0 to 300, step 1, initial 100
- `priority-walkers`: 0 to 100, step 1, initial 50
- `walker-turn-angle`: 0 to 90, step 1, initial 5
- `runner-turn-angle`: 0 to 90, step 1, initial 8
- `walker-step`: 0 to 2, step 0.01, initial 0.01
- `runner-step`: 0 to 3, step 0.01, initial 0.02

Switches:
- `bounce-walls?` default off
- `trail?` default on

Input boxes:
- `walker-patch-color` as String, initial `"blue"`
- `runner-patch-color` as String, initial `"red"`

Monitor:
- `total-distance-traveled` with reporter `total-distance-traveled`

Optional plot:
- plot name `Total Distance`
- pen `distance`
- update command `plot total-distance-traveled`

## PART 5 — RUN THE MODEL

1. Click **Setup**.
2. Click **Go**.

Expected:
- walkers move and color patches with `walker-patch-color`
- runners move and color patches with `runner-patch-color`
- total distance increases

If the movement still looks too fast:
- reduce `walker-step` and `runner-step`
- or increase `wait 0.08` to `wait 0.15`

## PART 6 — HOW THIS MATCHES THE PDF TASKS

Task 1 — Monitor:
- use `total-distance-traveled`

Task 2 — Topology:
Topology describes how the world edges behave.
- Torus = wraps on both axes
- Cylinder = wraps on only one axis
- Box = no wrapping

To change it:
1. Go to **Interface**
2. Right-click the world or open **Settings / World & View**
3. Change wrapping so only one axis wraps
4. Press **Setup** again

Task 3 — Change appearance:
- edit shapes/colors in code
- or use **Tools -> Turtle Shapes Editor**

Task 4 — Input for patch color:
- use the two inputs `walker-patch-color` and `runner-patch-color`

Task 5 — Bonus:
- reverse behaviors without editing code by swapping speed/turn settings

Task 6 — Extra:
- `priority-walkers` changes how many walkers are created during setup
- it does not change runtime priority

Examples:
- 80 = more walkers
- 20 = fewer walkers
- 100 = all walkers
- 0 = all runners

## PART 7 — IMPORTANT CONCEPTS

- turtles = moving agents
- patches = world cells
- links = connections between turtles
- observer = controller/viewpoint
- ticks = simulation time steps

`total-distance-traveled` is the total distance traveled by all walkers and runners together since the last setup.

## PART 8 — TROUBLESHOOTING

If a variable is missing, check that these widgets exist:
- `number-turtles`
- `priority-walkers`
- `walker-turn-angle`
- `runner-turn-angle`
- `walker-step`
- `runner-step`
- `bounce-walls?`
- `trail?`
- `walker-patch-color`
- `runner-patch-color`

If movement is too fast:
- try `walker-step = 0.005`
- try `runner-step = 0.01`
- increase `wait`

## PART 9 — WHAT TO SHOW THE TEACHER

1. You created the model from scratch
2. You built the interface manually
3. setup works
4. go works
5. the monitor works
6. topology can be changed to cylinder
7. walkers and runners have different appearance/behavior
8. walkers and runners color patches differently
9. behavior can be reversed from interface settings
10. one type can be prioritized during setup
