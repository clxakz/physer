[![Licence](https://img.shields.io/github/license/Ileriayo/markdown-badges?style=for-the-badge)](./LICENSE) ![Python](https://img.shields.io/badge/python-3670A0?style=for-the-badge&logo=python&logoColor=ffdd54)

# Physer - A PyGame Collision Library
> `Physer` is the new replacement for [stormfield](https://github.com/clxakz/stormfield)

-----
<br/>

## Contents
- [Installation](#installation)
- [Documentation](#documentation)
    - [World](#world)
        - [New Collider](#new_colliderrect-manual_update-kwargs)
        - [New Collision Class](#new_collision_classclass_name-ignores-kwargs)
        - [Update](#updatedt)
        - [Draw](#drawscreen-color-width-kwargs)
    - [Collider](#collider)
        - [Properties](#properties)
        - [Manual Update](#_updategravity-dt)
        - [Collider Types](#collider-types)

-----
<br/>

## Installation
```bash
git clone https://github.com/clxakz/physer
``` 
or
```bash
pip install physer
```

-----
<br/>

## Documentation

### World
The `World` class manages all colliders, applies global gravity, updates their movement, handles collision detection and resolution, and triggers collision event callbacks. It serves as the central physics and collision system for your game
```python
world = World(pygame.Vector2(0,0))
```
Arguments
- `gravity (Vector2)` - The gravity vector that's applied to all colliders

-----
<br/>

### `.new_collider(rect, manual_update, **kwargs)`
The `new_collider` method creates a new collider.
```python
box = world.new_collider(pygame.FRect(x, y, width, height))
```
Arguments
- `rect (Rect/FRect)` - The rect that the collider attaches to
- `manual_update (bool)` - Whether the collider should be manually updated, False by default
- `**kwargs (key, value)` - Additional arguments that get passed to collider object

-----
<br/>

### `.new_collision_class(class_name, ignores, **kwargs)`
The `new_collision_class` method creates a collision class with customizable settings, including which other classes to ignore during collision checks, allowing filtering and grouping of colliders.
```python
world.new_collision_class("Player")
world.new_collision_class("Wall", ["Player"])
```
Arguments
- `class_name (str)` - The name for your collision class
- `ignores (list[str])` - A list of other collision classes that should be ignored during collision
- `**kwargs (key, value)` - Additional arguments that get passed to the collision class

-----
<br/>

### `.update(dt)`
The `update` method is responsible for updating all colliders and resolving collisions.
```python
world.update(dt)
```
Arguments
- `dt (float)` - All colliders get updated with this delta time

-----
<br/>

### `.draw(screen, color, width, **kwargs)`
The `draw` method is used mainly for debugging purposes, it draws the rects of all colliders on the screen.
```python
world.draw(screen)
```
Arguments
- `screen (surface)` - The surface where rects will be drawn
- `color (tuple)` - The color of the outline
- `width (int)` - The width of the outline
- `**kwargs (key, value)` - Additional arguments that get passed to the draw method

-----
<br/>

### Collider
The `Collider` represents a physical object in the world that attaches to your rect.

-----
<br/>

### `properties`
- `rect (Rect/FRect)` - The rect that's attached to your collider
- `velocity (Vector2)` - The velocity of your collider
- `friction (float)` - The friction of your collider
- `collision_class (str)` - The collision class of your collider
- `type (one of [static, dynamic, kinematic, sensor])` - The type of your collider
- `manual_update (bool)` - Whether your collider should be manually updated, False by default
- `on_enter (callable)` - The function that gets ran on collision entry event 
- `on_exit (callable)` - The function that gets ran on collision exit event 
- `on_stay (callable)` - The function that gets ran on collision stay event 
- `object (any)` - What gets returned on collision events, defaults to self (Collider)

-----
<br/>

### `._update(gravity, dt)`
Manual update method.

-----
<br/>

### `Collider types`
| Type      | Affected by Gravity | Affected by Velocity | Moves? | Use Case |
|-----------|---------------------|-----------------------|--------|----------|
| dynamic   | ✅Yes                 | ✅Yes                   | ✅Yes    | Fully simulated objects like players, enemies, projectiles. |
| kinematic | 🚫No                  | ✅Yes (manual)          | ✅Yes    | Moving platforms, doors, scripted movement (you set velocity manually). |
| static    | 🚫No                  | 🚫No                    | 🚫No     | Walls, floors, anything that doesn't move. |
| sensor    | 🚫No                  | 	✅ Yes (manual or automatic)     | ✅ Optional     | Trigger zones, checkpoints, area detectors — detects overlaps but doesn't collide physically. |