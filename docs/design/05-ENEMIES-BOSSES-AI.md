# Enemies, Bosses and AI v0.1

## Enemy design rule

Enemy variety should come primarily from behavior, role, movement and attack patterns—not simply larger HP bars.

## Base enemy archetypes

### Brawler
- unarmed melee
- closes distance
- tries to surround the player

### Knife attacker
- fast approach
- short attack strings
- occasional lunge
- flank preference

### Bat / heavy melee enemy
- slower attacks
- high stagger and knockback
- creates space for ranged allies

### Pistol gunman
- prefers medium distance
- retreats when pressured
- uses cover when available

### SMG gunman
- burst pressure
- mobile suppression
- lower precision

### Shotgunner
- aggressively closes distance
- high close-range threat
- should telegraph entry into lethal range

### Sniper
- long sightline
- visible aiming cue/laser
- delayed high-damage shot
- forces movement and target prioritization

### Shield unit
- strong frontal defense
- vulnerable to flanks, explosives, leg attacks or guard breaks

### Heavy gunner
- high durability
- slow movement
- sustained fire
- vulnerable during reload/reposition windows

### Medic
- heals or revives allies
- high-priority support target

### Commander
- improves nearby aggression/coordination
- may call reinforcements

### Fast infected
- rushes rapidly
- low individual durability
- threat comes from pressure and numbers

### Explosive infected
- approaches and detonates or becomes unstable on death

### Brute mutant
- large body
- heavy knockback
- environmental destruction
- slow but dangerous telegraphed attacks

## AI state model

Common states:

- idle
- patrol
- suspicious
- alerted
- search last known position
- pursue
- reposition
- seek cover
- ranged attack
- melee attack
- reload
- dodge
- retreat
- call reinforcement
- support ally
- staggered
- knocked down
- flee/panic

Each archetype should use different priorities and transition weights.

Examples:
- sniper avoids close range and searches for sightlines
- shotgunner actively reduces distance
- knife attacker attempts flanks
- medic favors safe positions near wounded allies
- commander stays protected while boosting the squad

## Group combat behavior

Enemies should not all attack simultaneously without coordination.

Potential systems:
- melee attack-slot reservation
- role-based spacing
- suppression plus flank behavior
- staggered reinforcement waves
- retreat/reform when squad strength collapses

The purpose is to create readable pressure rather than an unreadable pile-up.

## Boss design principles

A boss should never be only a high-health normal enemy.

Each boss should have:
- 3-5 core moves
- clear telegraphs
- at least 2 phases or meaningful state changes
- a unique mechanic
- openings the player can learn and exploit
- memorable arena interaction where appropriate

## Example boss: Heavy Enforcer

### Phase 1
- machine-gun bursts
- armored charge
- heavy punch
- ground slam
- short reposition

Player learns to use cover, dodge the charge and punish reload/recovery windows.

### Phase 2

Armor breaks.

Changes:
- loses or abandons heavy gun
- movement speed increases
- longer melee strings
- more aggressive charge pattern
- larger punish windows after failed heavy attacks

The fight transforms rather than merely increasing damage.

## Boss damage rules

Boss attacks should be dangerous but readable. One-shot mechanics should be rare and reserved for highly telegraphed hazards or optional challenge modes.

## Encounter composition

Good encounters mix roles intentionally.

Examples:
- 2 brawlers + pistol gunman
- shield unit + shotgunner + sniper
- heavy gunner + medic + flank attackers

Avoid spawning arbitrary enemy combinations without considering player target priorities and available space.

## Difficulty scaling

Prefer scaling through:
- smarter compositions
- new behaviors
- faster decision-making within readable limits
- additional attack variants
- reduced recovery windows
- environmental pressure

Use raw HP/damage increases conservatively.
