# Adding enemies

In this section, we will create an enemy to act as an obstacle for our player to get past. This enemy will move until it encounters a wall, then turn around and repeat. When the player touches this enemy, they will die and the level will restart.

## Creating the enemy scene

1) Create a new scene with the root node of `CharacterBody2D` and save it as `enemy.tscn` in your scenes folder. This is a `CharacterBody2D` because it will behave similarly to the player.
![creating root CharacterBody2D](../images/section-6/creating_root_node.png)

2) Add an `AnimatedSprite2D` as a child of the `CharacterBody2D`. Make a new animation for the enemy called "default" and add `enemy1.png` and `enemy2.png` as the frames. After you've add the frames, set it to "Autoplay on Load" so that the animation instantly plays when the scene loads.

![setting autoload on animation](../images/section-6/setting_autoload.png) 

2) Add a `CollisionShape2D` as a child of the root `CharacterBody2D` node and set the shape to be a rectangle. Resize this in the scene dock to fit the sprite of the enemy.

![resizing collision](../images/section-6/resizing_collision.png)

3) Attach a script to the enemy with the default template. Save it as `enemy.gd` in your scripts folder.

4) Now, save your enemy scene and go back to `world.tscn`. Place your newly created enemy somewhere between two walls.

![placing the enemy](../images/section-6/placing_enemy.png)

The enemy is almost done, but it doesn't move yet. If you were to play right now, you would be able to stand on the enemy. Now, we will make a movement script for the enemy. Open up your newly created `enemy.gd`.

## Scripting the enemy

We are going to make an enemy that moves until it hits a wall, then turns around and repeats. If the player touches them, they will die. First, lets implement the movement.


### Enemy Movement

With your experience scripting the player earlier, you should be able to program this enemy alone now. Here is what your script should do:
- Declare a constant speed for the player
- Move the enemy horizontally in any direction until it encounters a wall
    - Use the built-in `CharacterBody2D` function `is_on_wall()` to check for this.
    - Use `move_and_slide()` to move based on velocity.
- When it encounters a wall, it should turn around (it's velocity should be negated)

> This works similarly to your player script! Since it's a `CharacterBody2D` like our player, you can reuse all the functionality from our old `player.gd` script.

Remember to use a constant for its speed; it's bad practice to use so-called magic numbers in your code without any explanation. Also, remember that since it's a physics process, you should be using `physics_process` instead of the default `process`.

We encourage you to try scripting this one yourself! If it doesn't work at first, try debugging it.

Here's an example of how your script could look:
```gdscript
extends CharacterBody2D

const SPEED: int = 50
var x_direction = 1

func _physics_process(delta: float) -> void:
    if is_on_wall():
        x_direction *= -1

    velocity.x = SPEED * x_direction
    move_and_slide()

```

### Killing the Player

Now if you play, the enemy will move until it hits a wall, then turn around. But uh oh, it bounces off of the player too! Let's fix that by adding logic to kill the player on the enemy. Here is what we need to add to the script:
- Before the movement logic, we need to check if anything has collided with the enemy in the loop.
- If they have, we need to check if that collider is in the group "Player".
- If they are, call the `die()` function on them.

We can do this with these `CharacterBody2D` functions:
- `get_slide_collision_count()`: Returns the number of collisions during the last call of `move_and_slide()`. 
- `get_slide_collision(int)`: Returns a `KinematicCollision2D` identified by the index you passed in which contains all the information about the collision including the actual object that collided. You can access the actual collided node with `get_slide_collision(x).get_collider()`. 
    - You can iterate over the number given by `get_slide_collision_count()` with a for loop, then pass the index into `get_slide_collision(i)` to get the collision like this:
    ```gdscript
    for i in get_slide_collision_count():
        get_slide_collision(i).get_collider() # Returns node we collided with
    ```
    > Read more about iterating and looping [here](https://gdscript.com/tutorials/looping/).

Try programming and debugging it yourself!

Here is roughly how your script should look now:
```gdscript
extends CharacterBody2D

const SPEED: int = 50
var x_direction = 1

func _physics_process(delta: float) -> void:
	for i in get_slide_collision_count():
		var collider = get_slide_collision(i).get_collider()
		if collider.is_in_group("Player"):
			collider.die()
	
	if is_on_wall():
		x_direction *= -1
	
	velocity.x = SPEED * x_direction
	move_and_slide()
```
---

We now have a moving enemy for our player to get past that can actually kill them! But the player has no way to defend themselves, so lets give them an option in the [next section](./section-7.md).
