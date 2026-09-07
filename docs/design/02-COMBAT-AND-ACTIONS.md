# Combat and Actions v0.1

## Combat goals

Combat should feel immediate, readable and physical. The player can succeed with a small number of inputs, while advanced play is rewarded through timing, movement, weapon switching, counters, positioning and environmental kills.

## Core movement actions

- idle
- walk
- run
- quick stop
- turn
- jump
- fall
- land
- heavy landing
- crouch
- crouch-walk
- roll
- forward dash
- backward evade
- slide
- vault low obstacle
- climb ladder
- drop through platform
- ledge grab
- get-up
- wall impact
- knockback
- knockdown
- launch

Potential later traversal:

- wall jump
- grappling hook
- rope traversal

## Unarmed combat

Base attacks:

- light punch
- heavy punch
- light kick
- heavy kick

Example strings:

- punch -> punch -> punch
- punch -> punch -> heavy punch
- punch -> punch -> kick
- punch -> kick
- kick -> kick
- heavy punch -> knockback
- crouch attack
- jump attack
- dash attack
- back attack
- grounded follow-up

## Defensive actions

- block
- perfect block
- dodge/roll
- backstep
- counter
- contextual escape from grab

## Advanced actions

- grab
- throw enemy
- trip
- disarm
- wall slam
- launcher
- air follow-up
- finisher

## Finishers

A weakened enemy can enter a short execution-ready state. Contextual finishers depend on equipment and nearby environment.

Examples:

- head smash into wall
- kick through breakable railing
- weapon disarm into strike
- throw from ledge
- melee-weapon-specific finisher

Finishers should be short enough not to interrupt pacing excessively.

## Hit reaction system

Incoming attacks should resolve into different reactions based on force, damage type, direction and target state:

- flinch
- stagger
- knockback
- knockdown
- launch
- wall splat
- floor bounce
- ragdoll

## Impact feedback

Heavy hits should combine several feedback layers:

- hit stop
- small camera shake
- pose reaction
- impact VFX
- sound variation
- knockback
- weapon recoil
- debris where appropriate

Different hit strengths must feel visibly different.

## Ragdoll usage

Normal combat uses authored skeletal animation. Ragdoll or partial physics is used for:

- death
- explosions
- vehicle impacts
- long falls
- extreme launches
- large boss attacks

Physics should enhance readability and comedy without making normal combat uncontrollable.

## Combat state priorities

The implementation should explicitly define cancel and priority rules between:

- movement
- attack startup
- active attack
- recovery
- block
- dodge
- hit stun
- knockdown
- reload
- weapon swap
- interaction

This avoids animation-lock bugs and makes the combat deterministic enough to tune.

## Initial input proposal

Keyboard/mouse:

- A / D: movement
- W: jump
- S: crouch
- Shift: dash / evade
- mouse: aim
- left mouse: fire / primary attack
- right mouse: aim / alternate attack
- Q: melee
- R: reload
- E: interact
- G: throwable
- 1 / 2 / 3: weapon slots

Exact mapping can change after playtesting.
