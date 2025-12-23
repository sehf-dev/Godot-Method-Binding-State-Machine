# Godot-Method-Binding-State-Machine
## A Godot StateMachine that uses method names, and matches them to states.

The popular Node based finite state machine method created lots of project clutter ( and State overhead do to inheriting Node ). And using match statements in singular scripts was just messy.
Thus I set out to find an intuitive method of State handling and came upon this.

## Setup

The setup is very little and is as follow 
> [!IMPORTANT]
> You must have a ***States Enum*** and must pass it via ***set_state_enum***
It's also reccomended you set the starting state after, otherwise your StateMachine won't be in any states.

The conventions for Method names is as follows
>[!NOTE]
> * All State methods start with an _ character
> * All characters in the method must be lowercase

If these are ignored the StateMachine may not find them, and they will fall uncalled.

## Example

```gdscript

extends StateMachine

# Attributes Used for demonstration purposes  #
@export var player: CharacterBody2D
@onready var animation_player: AnimationPlayer = $AnimationPlayer


enum States { # Enum for all States
  Idle,
  Walk,
}


func _ready():
  set_state_enum(States)
  set_state(States.Walk) # Set starting State


func _idle_enter(): 
  animation_player.play("idle")


func _idle_update(delta: float): 
  if player.velocity.length() > 0:
    set_state(States.Walk)


func _walk_enter(): 
  animation_player.play("walk")


func _walk_update(delta: float):
  # Exit Walk State
  if player.velocity.length() == 0:
    set_state(States.Idle)


func _walk_exit():
   pass

```

## Docs

### State Machine Methods

**set_state(state: int)**
> Sets the StateMachines current State

**set_state_enum(states_enum)**
> Sets the state_enum for the StateMachine which is used for retrieing State IDS
> the parameter "states_enum" should be your enum containing States


### State methods
> [!NOTE]
_In the following examples replace "state" with one of your states from your States Enum_

**_state_enter()**
> Ran at the beginning of this State


**_state_exit()**
> Ran before transitioning to another State


**_state_update(delta: float)**
> Ran every frame while this State is active ( equivelent to _procces() )


**_state_fixed_update(delta: float)**
> Ran every physics frame while this State is active ( equivelent to _physics_procces() )


**_state_input(event: InputEvent)**
> Ran upon upon a InputEvent  ( equivelent to _input() )
