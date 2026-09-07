# Weapons and Items v0.1

## Design rule

Weapons must change tactics and feel, not only damage values.

## Melee categories

### Fists / gloves
- very fast
- short range
- combo-heavy
- strong stagger pressure

### Baseball bat
- medium reach
- high knockback
- good crowd control

### Metal pipe
- common urban weapon
- balanced speed and reach

### Knife
- fast
- low knockback
- high critical/back attack potential

### Katana
- fast slashes
- strong damage
- special dash slash potential

### Fire axe
- slow
- high damage
- strong armor break

### Sledgehammer
- very slow
- extreme knockback
- ground shock potential

### Stun baton
- moderate damage
- chance or guaranteed short stun on certain attacks

## Firearm categories

### Pistols
Accurate, reliable, mobile.

### Revolvers
Low rate of fire, heavy damage and recoil, strong precision payoff.

### SMGs
High rate of fire, mobile, weaker per shot.

### Assault rifles
General-purpose mid-range weapons.

### Shotguns
Short range, huge impact, high knockback, strong close-quarters identity.

### Sniper rifles
Long range, high headshot damage, slow handling, potential penetration.

### Light machine guns
Large magazines, sustained suppression, slower movement/handling.

### Grenade launchers
Area damage and crowd displacement.

### Rocket launchers
Rare ammunition, anti-heavy/anti-boss/anti-vehicle role.

### Flamethrowers
Continuous area denial, ignition and panic behavior on susceptible enemies.

## Required firearm stats

- id
- displayName
- category
- damage
- fireRate
- magazineSize
- reserveAmmoLimit
- reloadType
- reloadTime
- accuracy
- movingAccuracy
- recoil
- range
- penetration
- knockback
- headshotMultiplier
- ammoType
- rarity
- specialEffect
- animationSet
- soundSet

## Reload behavior

Reloads should be weapon-specific.

- magazine weapons: eject -> insert -> chamber where appropriate
- shotguns: shell-by-shell reload that can be interrupted to fire
- revolvers: cylinder-specific reload behavior
- heavy weapons: longer vulnerable reloads

## Throwables

- fragmentation grenade
- molotov
- flashbang
- smoke grenade
- mine
- remote explosive

## Utility / equipment

- medkit
- body armor
- adrenaline injector
- night vision
- gas mask
- grappling device
- deployable drone
- automated turret
- energy shield (late-game option)
- exoskeleton / heavy suit (late-game set piece)

## Weapon rarity

Suggested tiers:

- Common
- Uncommon
- Rare
- Epic
- Legendary

Rarity should add interesting traits, not just larger numbers.

Example: a legendary shotgun could gain massively increased knockback after a kill rather than simply +40% damage.

## Attachments

Potential firearm attachments:

- optic
- suppressor
- extended magazine
- quick magazine
- compensator
- laser
- foregrip
- special ammunition

Attachments should alter handling and combat choices.

## Environment weapons

Interactive combat objects should include:

- fuel barrels
- gas cylinders
- cars
- breakable glass
- crates
- tables/chairs
- electrical boxes
- hanging objects
- explosive charges
- oil pools
- exposed power cables

These objects can become improvised weapons, hazards or combo tools.

## Full-game content target

- 20-30 melee weapons
- 8-12 pistols
- 6-10 SMGs
- 10-15 rifles
- 5-8 shotguns
- 4-6 sniper rifles
- 6-10 heavy/special weapons
- 6-10 throwable items

Total long-term weapon target: roughly 70-100 distinct items, with animation and behavior reuse where sensible.
