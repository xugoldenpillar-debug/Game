# Game Design Document v0.1

## Working title

**STICKMAN: BREAKPOINT**

Chinese working title: **火柴人：临界点**

## Genre

2D side-scrolling action / shooting / beat-'em-up / story campaign / RPG progression.

## Core fantasy

The player controls a highly expressive stickman fighter through a long-form urban action campaign featuring melee combat, firearms, throwable items, destructible objects, bosses, companions and escalating large-scale encounters.

## Core loop

Explore area -> encounter enemies -> fight using melee/firearms/environment -> loot resources -> trigger story/event -> elite encounter -> checkpoint -> boss/set piece -> return to hub -> upgrade -> next mission.

## Experience goals

- Easy to learn, deep enough to master.
- Fast readable action with strong hit feedback.
- Weapons feel mechanically different.
- Frequent context changes: brawls, gunfights, defense, escapes, bosses, scripted set pieces.
- Story campaign long enough to feel like a complete game rather than a web mini-game.

## Major systems

- Character movement and traversal
- Melee combat and combos
- Firearms and ammunition
- Throwables and tactical items
- Enemy AI and combat roles
- Boss encounters
- Environmental interaction/destruction
- Loot and equipment
- Character progression and skill trees
- Companions
- Story missions and side missions
- Hub/base
- Survival / challenge / Boss Rush modes

## Target full-game scope

- 8 chapters
- 40-50 main missions
- 20+ side missions
- 15+ bosses / mini-bosses
- 15-20 enemy archetypes
- 70-100 weapons across melee and firearms
- 100+ reusable character actions/animations
- 6 companion characters
- 10+ environment themes
- Main campaign target: 8-12 hours
- Completionist target: 15-25 hours

## Vertical slice first

Before large-scale content production, build one polished playable slice containing:

- 1 player character
- 10-15 core actions
- unarmed combat
- baseball bat
- knife
- pistol
- SMG
- shotgun
- grenade
- 5 enemy types
- 1 boss
- 3 connected scenes
- one complete mission
- checkpoints
- basic drops
- hit stop, screen shake, knockback, ragdoll deaths
- at least one destructible/explosive environmental interaction

The project should not expand into dozens of missions until this slice is consistently fun.

## Art direction

- Readable black stickman silhouettes
- Strong contrast and clean shapes
- Simple bodies, expressive poses
- More production effort on animation, weapons, VFX, impact, destruction and environments than on detailed character anatomy
- Faction identity communicated with accents, armor, props, silhouettes and equipment

## Technical design philosophy

Game content should be data-driven. Weapons, enemies, skills, drops, levels and encounters should be configured through data files rather than hard-coded values wherever practical.

## Non-goals

- Direct replication of copyrighted characters, maps, story, logos or assets from Anger of Stick or other games
- Overly complicated mobile-style currency systems
- Turning every weapon upgrade into a pure stat increase
- Making every enemy differ only by HP and damage
