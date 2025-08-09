# Attacking with Discs

In this section, we are gonna give our players a throwable disc that they can use to kill enemies. 

## Making the Disc Scene

1) Create a new scene with the root node as an `Area2D`. Rename the root node to "Disc" and save the scene in your scenes folder as `disc.tscn`. 

2) Add an `AnimatedSprite2D` and make an animation called "default" with the frames as "disc.png" and "disc2.png". Set this animation to "Autoplay on Load".

![making disc animation](../images/section-7/disc-animation.png) 

3) Add a `CollisionShape2D` to the disc and add a `CircleShape2D` shape on it. Resize it to the disc's sprite.

![adding collision shape](../images/section-7/adding-collision-shape.png)

Lets save the programming of the disc for later so we can more easily debug the script. Lets instead program spawning the actual disc first.

## Adding input

Before we get to the programming, we need to get to add an input that actually allows the player to shoot. On the top left bar, go to "Project" then "Project Settings". Then go to the "Input Map" tab and add a new "Shoot" action. Add whatever keybinds events you want to correspon to shoot. I will use `Enter` and `z`.

![adding input](../images/section-7/adding-input.png) 

## Adding a cooldown timer

We don't want the player to spam the disk endlessly, so we are going to give them a small cooldown between throws. Head back to your player scene and add a `Timer` node. Rename this timer to `DiscCooldown`. Now, head to the inspector with the `DiscCooldown` node selected and set the "Wait Time" to something reasonable, like 2 seconds. This will be the cooldown after each disc throw. Also, set the "One Shot" property to be true so the cooldown doesn't infinitely repeat.

![adding the timer](../images/section-7/adding-timer.png) 

Now everytime the player throws a disc, we will start the timer. Then if the player tries to throw a disc, we will check if the timer is ongoing or has ended. If it's ongoing, we will ignore it. If it has ended, we will restart the timer and throw a disc.

Here is how you can utilize the node in your player script:
- `$DiscCooldown.start()`: Start the timer.
- `$DiscCooldown.is_stopped()`: Returns true if the timer is stopped, false if it's ongoing.

## Programming the player, again

Now, go back to your `player.gd` script and drag the `disc.tscn` scene into your script while holding `Ctrl` or `⌘`. If this doesn't work for you, you can just go ahead and type out the output.

![dragging preload](../images/section-7/dragging-scene.png)

> We have to load scenes before we can put them in a scene programatically, unlike of placing them in a scene preemptively. We use `preload()` because the path is known during compile time. You would use `load()` if it's not.

The functions `preload(path)` and `load(path)` will load the scene from the filesystem and return a resource correspondent to that scene. You can call `.instanciate(path)` on the resource to get an instance of the scene. The scene won't instantly appear as you still need to add it to the scene tree. You can add it to the scene by calling `add_child(node)` on another node currently on the scene. If you want to add it to the current scene, you can call `get_tree().current_scene.add_child(node)`.

So, you would call `DISC.instanciate()` and then add it to the current scene.

> Anything that is saved and loaded from the filesystem is a resource. You can read more about resources and loading [here](https://docs.godotengine.org/en/stable/tutorials/scripting/resources.html), in the Godot docs. **Nodes** give you functionality, and **Resources** are just date containers.

Now we should be ready to start programming the player spawning disc! Try programming it yourself. Heres what you should add to your existing `player.gd`:
- If the player presses the "Shoot" action:
    - Instanciate the disc
    - Set the disc's `.position` property to the player's position.
    - Add the disc to the scene tree
    - Restart the timer

Try debugging it yourself if you run into a problem, just run the scene and you should be able to spawn a still disc on top of the player every two seconds after you press the "Shoot" action.

> This if clause should go after your early return if the player is dead, so they can shoot in the afterlife.

Here is what your script should look something like:
```gdscript
extends CharacterBody2D

const DISC = preload("res://scenes/disc.tscn")

const SPEED: int = 200 
const JUMP_POWER: int = -300
const DELAY_TILL_RESTART: float = 1

var is_dead: bool = false
var x_direction: int = 0


# Called when the node enters the scene tree for the first time.
func _ready() -> void:
	pass # Replace with function body.

func _physics_process(delta: float) -> void:
	if not is_on_floor():
		velocity += get_gravity() * delta
	
	if is_dead:
		move_and_slide()
		return

	if Input.is_action_pressed("Shoot") and $DiskCooldown.is_stopped(): # Check if player cooldown is over
		$DiskCooldown.start() # Restart it
		var new_disc: Area2D = DISC.instantiate() # Instanciate new disc scene
		new_disc.position = position # Set the position to the player's
		get_tree().current_scene.add_child(new_disc) # Add it to the scene
	
	if Input.is_action_just_pressed("Jump") and is_on_floor():
		velocity.y += JUMP_POWER
	
	if Input.is_action_pressed("Left"):
		x_direction = -1
		$AnimatedSprite2D.flip_h = true
	elif Input.is_action_pressed("Right"):
		x_direction = 1
		$AnimatedSprite2D.flip_h = false
	else:
		x_direction = 0
	
	velocity.x = x_direction * SPEED
	
	if is_on_floor():
		if x_direction != 0:
			$AnimatedSprite2D.play("run")
		else:
			$AnimatedSprite2D.play("idle")
	else:
		if velocity.y > 0:
			$AnimatedSprite2D.play("fall")
		if velocity.y < 0:
			$AnimatedSprite2D.play("jump")

	move_and_slide()

func die() -> void:
	is_dead = true
	set_deferred("velocity", Vector2.ZERO)
	$AnimatedSprite2D.flip_v = true
	$CollisionShape2D.set_deferred("disabled", true)
	
	Engine.time_scale = 0.5
	await get_tree().create_timer(DELAY_TILL_RESTART).timeout
	Engine.time_scale = 1
	get_tree().change_scene_to_file("res://scenes/world.tscn")
```

## Programming the disc

We can spawn a disc, but it doesn't move or kill enemies yet. Lets begin to program that. Return to the disc scene and attach a script to the root node.

> To be continued...
