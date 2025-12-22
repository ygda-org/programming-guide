# Improving our player

Right now, our player has the ability to walk left, to walk right, and to fall of a cliff! However, we can't jump off the cliff yet, so lets fix that.

## Using an Input Map

Before we add jumping, lets improve how our script detects inputs. Navigate to the project settings from the project button on the top left of the Godot toolbar. Go to the "Input Map" tab.    

> With an input map, you can create actions that are triggered by a set of events. Events can be mouse clicks, keyboard presses, controller pressses, etc. An action can have multiple events that trigger it. You can see the status of these actions with the built in `Input` global, for example: `Input.is_action_pressed("<your_event>")`.

Do you remember how in your player movement script you called `Input.is_action_pressed("ui_right")` to detect the action of the player pressing the right arrow key? We did that earlier for simplicity, but it's best practice to make your own input map with your own actions and respective inputs. You can add one action and make multiple events trigger it, like making the right arrow key and `a` both trigger the move right event. Go ahead and add 3 actions named `Left`, `Right`, and `Jump`. Type the action name then press the add button right next to it. 

![input map tab](../images/section-3/adding-actions.png) 


Now you can add events--i.e. mouse and keyboard actions--to each respective input. Press the `+` icon next to each of your new actions then a UI will appear where you can select the event you want to trigger that action. Go ahead and add whatever inputs you see fit for each event.    

![adding actions](../images/section-3/adding-events.png) 


### Updating our movement script

Now that you have your own input map, you no longer have to use the default `ui_right` and `ui_left`. Update the left and right movement to instead use your custom `Right` and `Left` events.


## Finally, adding jumping

Lets add an extra if clause to our script. Try coding it yourself! Here are the changes we will make:
- Add an if clause after the gravity clause that checks if the player has pressed the `Jump` action.
    - You can do this with `Input.is_action_pressed(action)` or `Input.is_action_just_pressed(action)`. They will have different effects; so do whichever you prefer!
- Also check in this clause if the player is on the floor
    - You can do this with the handy `CharacterBody2D` function: `is_on_floor()`. Very self explanatory.
- In this clause, you should add a constant amount of jump power to `velocity.y`
    - You should declare this constant at the start of your script, like we did with speed.
    - This constant should be negative. Read the disclaimer below for why.

> In Godot, negative Y is the up direction, while positive Y is the down direction. This is because computers place the origin at the top left corner of your monitor. We invert the Y-axis so we don't have to work with negative numbers all the time. Blame old people if you don't like it.


Your script should look something like this now:

```gdscript
extends CharacterBody2D

const SPEED: int = 200 
const JUMP_POWER: int = -300 # You can play around with this value if you want.

var x_direction: int = 0

# Called when the node enters the scene tree for the first time.
func _ready() -> void:
	pass # Replace with function body.

func _physics_process(delta: float) -> void:
	if not is_on_floor():
		velocity += get_gravity() * delta

	if Input.is_action_just_pressed("Jump") and is_on_floor():
		velocity.y += JUMP_POWER # Add the jump power

	if Input.is_action_pressed("Left"):
		x_direction = -1
	elif Input.is_action_pressed("Right"):
		x_direction = 1
	else:
		x_direction = 0

	velocity.x = x_direction * SPEED

	move_and_slide()
```


## Adjusting camera

The player looks a little small, so lets fix that by adjusting the zoom. Go to your `world.tscn` scene and, in the scene tree, select your camera. Now go to the inspector and adjust the zoom property. Set it to 2. The camera also follows the player closely and feels very rigid. We can fix this by also enabling "Position Smoothing" for the camera in the inspector. You can adjust the values until it feels right.

![adjusting camera](../images/section-3/adjusting-camera.png)


---

Now our player can hop and parkour around the scene, but the player sprite's just a static image! In the [next section](./section-4.md) we will add some animations.
