# MONSTERS

Monsters include not just zombies, but fish, dogs, moose, Mi-gos, manhacks, and even stationary installations like turrets. They are defined in JSON objects with "type" set to "MONSTER":

```JSON
{
    "type": "MONSTER",
    "id": "mon_foo",
    ...
}
```

The `"id"` member is the unique identifier for this monster type. It can be any string, but by convention has the prefix `mon_`. This id is referenced in monster groups or in mapgen to spawn specific monsters.

For quantity strings (ie. volume, weight) use the largest unit you can keep full precision with.


## Monster properties

These properties are required for all monsters:

| Property          | Description
| ---               | ---
| `name`            | (string or object) Monster name, and optional plural name and translation context
| `description`     | (string) In-game description of the monster, in one or two sentences
| `ascii_picture`   | (string) Id of the asci_art used for this monster
| `hp`              | (integer) Hit points
| `volume`          | (string) Volume of the creature's body, as an integer with metric units, ex. `"35 L"` or `"1500 ml"`
| `weight`          | (string) Monster weight, as an integer with metric units, ex. `"12 kg"` or `"7500 g"`
| `symbol`          | (string) UTF-8 single-character string representing the monster in-game
| `color`           | (string) Symbol color for the monster
| `default_faction` | (string) What faction the monster is allied with
| `bodytype`        | (string) Monster's body type, ex. bear, bird, dog, human, insect, lizard etc.
| `speed`           | (integer) Monster speed, relative to human speed `100`, with greater numbers being faster

Monsters may also have any of these optional properties:

| Property                 | Description
| ---                      | ---
| `copy-from`              | (string) Inherit monster attributes from another. See [JSON_INHERITANCE.md](JSON_INHERITANCE.md)
| `categories`             | (array of strings) Monster categories (NULL, CLASSIC, or WILDLIFE)
| `species`                | (array of strings) Species IDs, ex. HUMAN, ROBOT, ZOMBIE, BIRD, MUTANT, etc.
| `scent_tracked`          | (array of strings) Monster tracks these scents
| `scent_ignored`          | (array of strings) Monster ignores these scents
| `size`                   | (string) Size flag, ex. TINY, SMALL, MEDIUM, LARGE, HUGE
| `material`               | (array of strings) Materials the monster is made of
| `phase`                  | (string) Monster's body matter state, ex. SOLID, LIQUID, GAS, PLASMA, NULL
| `attack_cost`            | (integer) Number of moves per regular attack (??)
| `diff`                   | (integer) Additional monster difficulty for special and ranged attacks
| `aggression`             | (integer) Starting aggression, the monster will become hostile when it reaches 10
| `morale`                 | (integer) Starting morale, monster will flee when (current aggression + current morale) < 0 
| `mountable_weight_ratio` | (float) For mounts, max ratio of mount to rider weight, ex. `0.2` for `<=20%`
| `melee_skill`            | (integer) Monster skill in melee combat, from `0-10`, with `4` being an average mob
| `dodge`                  | (integer) Monster's skill at dodging attacks
| `melee_damage`           | (array of objects) List of damage instances added to die roll on monster melee attack
| `melee_dice`             | (integer) Number of dice rolled on monster melee attack to determine bash damage
| `melee_dice_sides`       | (integer) Number of sides on each die rolled by `melee_dice`
| `grab_strength`          | (integer) Intensity of grab effect, from `1` to `n`, simulating `n` regular zombie grabs
| `melee_cut`              | (integer) Amount of cutting damage added to the die roll on melee attack
| `armor_bash`             | (integer) Monster's protection from bash damage
| `armor_bullet`           | (integer) Monster's protection from bullet damage
| `armor_cut`              | (integer) Monster's protection from cut damage
| `armor_stab`             | (integer) Monster's protection from stab damage
| `armor_acid`             | (integer) Monster's protection from acid damage
| `armor_fire`             | (integer) Monster's protection from fire damage
| `weakpoints`             | (array of objects) Weakpoints in the monster's protection
| `vision_day`             | (integer) Vision range in full daylight, with `50` being the typical maximum
| `vision_night`           | (integer) Vision range in total darkness, ex. coyote `5`, bear `10`, sewer rat `30`, flaming eye `40`
| `tracking_distance`      | (integer) Amount of tiles the monster will keep between itself and its current tracked enemy or followed leader. Defaults to `3`.
| `luminance`              | (integer) Amount of light passively emitted by the monster, from `0-10`
| `death_drops`            | (string or item group) Item group to spawn when the monster dies
| `death_function`         | (array of strings) How the monster behaves on death. See JSON_FLAGS
| `emit_field`             | (array of objects) What field the monster emits, and how frequently
| `regenerates`            | (integer) Number of hit points the monster regenerates per turn
| `regenerates_in_dark`    | (boolean) True if monster regenerates quickly in the dark
| `regen_morale`           | (bool) True if monster will stop fleeing at max HP to regenerate anger and morale
| `special_attacks`        | (array of objects) Special attacks the monster has
| `flags`                  | (array of strings) Any number of attributes like SEES, HEARS, SMELLS, STUMBLES, REVIVES
| `fear_triggers`          | (array of strings) Triggers that lower monster morale (see JSON_FLAGS.md) 
| `anger_triggers`         | (array of strings) Triggers that raise monster aggression (same flags as fear)
| `placate_triggers`       | (array of strings) Triggers that lower monster aggression (same flags as fear)
| `chat_topics`            | (array of strings) Conversation topics if dialog is opened with the monster
| `revert_to_itype`        | (string) Item monster can be converted to when friendly (ex. to deconstruct turrets)
| `starting_ammo`          | (object) Ammo that newly spawned monsters start with
| `upgrades`               | (boolean or object) False if monster does not upgrade, or an object do define an upgrade
| `reproduction`           | (object) The monster's reproductive cycle and timing
| `baby_flags`             | (array of strings) Seasons during which this monster is capable of reproduction
| `special_when_hit`       | (array) Special defense triggered when the monster is attacked
| `attack_effs`            | (array of objects) Effects applied to the attacked creature when the monster successfully attacks
| `path_settings`          | (object) How monster may find a path, open doors, avoid traps, or bash obstacles
| `biosignature`           | (object) Droppings or feces left by the animal or monster
| `harvest`                | (string) ID of a "harvest" type describing what can be harvested from the corpse
| `zombify_into`           | (string) mtype_id this monster zombifies into after it's death
| `fungalize_into`         | (string) mtype_id this monster turns into when fungalized by spores
| `shearing`               | (array of objects) Items produced when the monster is sheared
| `speed_description`      | (string) ID of a `speed_description` type describing the monster speed string
| `petfood`                | (object) Data regarding feeding this monster to turn it into a pet

Properties in the above tables are explained in more detail in the sections below.


## "name"
(string or object, required)

```JSON
"name": "cow"
```

```JSON
"name": { "ctxt": "fish", "str": "pike", "str_pl": "pikes" }
```

or, if the singular and plural forms are the same:

```JSON
"name": { "ctxt": "fish", "str_sp": "bass" }
```

Name displayed in-game, and optionally the plural name and a translation context (ctxt).

If the plural name is not specified, it defaults to singular name + "s". "str_pl" may also be needed if the unit test cannot determine if the correct plural form can be formed by simply appending "s".

Ctxt is used to help translators in case of homonyms (two different things with the same name). For example, pike the fish and pike the weapon.

## "description"
(string, required)

In-game description for the monster.

## "categories"
(array of strings, optional)

Monster categories. Can be NULL, CLASSIC (only mobs found in classic zombie movies) or WILDLIFE (natural animals). If they are not CLASSIC or WILDLIFE, they will not spawn in classic mode.

## "species"
(array of strings, optional)

A list of species ids. Properties (currently only triggers) from species are added to the properties of each monster that belong to the species.

In mainline game it can be HUMAN, ROBOT, ZOMBIE, MAMMAL, BIRD, FISH, REPTILE, WORM, MOLLUSK, AMPHIBIAN, INSECT, SPIDER, FUNGUS, PLANT, NETHER, MUTANT, BLOB, HORROR, ABERRATION, HALLUCINATION and UNKNOWN.

## "volume"
(string, required)

```JSON
"volume": "40 L"
```
The numeric part of the string must be an integer. Accepts L, and ml as units. Note that l and mL are not currently accepted.

## "weight"
(string, required)

```JSON
"weight": "3 kg"
```
The numeric part of the string must be an integer. Use the largest unit you can keep full precision with. For example: 3 kg, not 3000 g. Accepts g and kg as units.

## "scent_tracked"
(array of strings, optional)

List of scenttype_id tracked by this monster. scent_types are defined in scent_types.json

## "scent_ignored"
(array of strings, optional)

List of scenttype_id ignored by this monster. scent_types are defined in scent_types.json

## "symbol", "color"
(string, required)

Symbol and color representing monster in-game. The symbol must be a UTF-8 string, that is exactly one console cell width (may be several Unicode characters). See [COLOR.md](COLOR.md) for details.

## "size"
(string, optional)

Size flag, see [JSON_FLAGS.md](JSON_FLAGS.md).

## "material"
(array of strings, optional)

The materials the monster is primarily composed of. Must contain valid material ids. An empty array (which is the default) is also allowed, the monster is made of no specific material.

## "phase"
(string, optional)

It describes monster's body state of matter. However, it doesn't seem to have any gameplay purpose, right now.
It can be SOLID, LIQUID, GAS, PLASMA or NULL.

## "default_faction"
(string, required)

The id of the faction the monster belongs to, this affects what other monsters it will fight. See Monster factions.

## "bodytype"
(string, required)

The id of the monster's bodytype, which is a general description of the layout of the monster's body.

Value should be one of:

| value           | description
| ---             | ---
| `angel`         | a winged human
| `bear`          | a four legged animal that can stand on its hind legs
| `bird`          | a two legged animal with two wings
| `blob`          | a blob of material
| `crab`          | a multilegged animal with two large arms
| `dog`           | a four legged animal with a short neck elevating the head above the line of the body
| `elephant`      | a very large quadruped animal with a large head and torso with equal sized limbs
| `fish`          | an aquatic animal with a streamlined body and fins
| `flying insect` | a six legged animal with a head and two body segments and wings
| `frog`          | a four legged animal with a neck and with very large rear legs and small forelegs
| `gator`         | a four legged animal with a very long body and short legs
| `horse`         | a four legged animal with a long neck elevating the head above the line of the body
| `human`         | a bipedal animal with two arms
| `insect`        | a six legged animal with a head and two body segments
| `kangaroo`      | a pentapedal animal that utilizes a large tail for stability with very large rear legs and smaller forearms
| `lizard`        | a smaller form of 'gator'
| `migo`          | whatever form migos have
| `pig`           | a four legged animal with the head in the same line as the body
| `spider`        | an eight legged animal with a small head on a large abdomen
| `snake`         | an animal with a long body and no limbs

## "attack_cost"
(integer, optional)

Number of moves per regular attack.

## "diff"
(integer, optional)

Monster baseline difficulty.  Impacts the shade used to label the monster, and if it is above 30 a kill will be recorded in the memorial log.  Monster difficult is calculated based on expected melee damage, dodge, armor, hit points, speed, morale, aggression, and vision ranges.  The calculation does not handle ranged special attacks or unique special attacks very well, and baseline difficulty can be used to account for that.  Suggested values:

| value | description
| ---   | ---
| `2`   | a limited defensive ability such as a skitterbot's taser, or a weak special like a shrieker zombie's special ability to alert nearby monsters, or a minor bonus to attack like poison or venom.
| `5`   | a limited ranged attack weaker than spitter zombie's spit, or a powerful defensive ability like a shocker zombie's zapback or an acid zombie's acid spray.
| `10`  | a powerful ranged attack, like a spitters zombie's spit or an turret's 9mm SMG.
| `15`  | a powerful ranged attack with additional hazards, like a corrosize zombie's spit
| `20`  | a very powerful ranged attack, like a laser turret or military turret's 5.56mm rifle, or a powerful special ability, like a zombie necromancer's ability to raise other zombies.
| `30`  | a ranged attack that is deadly even for armored characters, like an anti-material turret's .50 BMG rifle.

Most monsters should have difficulty 0 - even dangerous monsters like a zombie hulk or razorclaw alpha.  Difficulty should only be used for exceptional, ranged, special attacks.

## "aggression"
(integer, optional)

Defines how aggressive the monster is. Ranges from -99 (totally passive) to 100 (guaranteed hostility on detection)

## "morale"
(integer, optional)

Monster morale. Defines how low monster HP can get before it retreats. This number is treated as % of their max HP.

## "speed"
(integer, required)

Monster speed. 100 is the normal speed for a human being - higher values are faster and lower values are slower.

## "mountable_weight_ratio"
(float, optional)

Used as the acceptable rider vs. mount weight percentage ratio. Defaults to "0.2", which means the mount is capable of carrying riders weighing `<= 20%` of the mount's weight.

## "melee_skill"
(integer, optional)

Monster melee skill, ranges from 0 - 10, with 4 being an average mob. See [GAME_BALANCE.md](GAME_BALANCE.md) for more examples

## "dodge"
(integer, optional)

Monster dodge skill. See [GAME_BALANCE.md](GAME_BALANCE.md) for an explanation of dodge mechanics.

## "melee_damage"
(array of objects, optional)

List of damage instances added to die roll on monster melee attack.

| field               | description
| ---                 | ---
| `damage_type`       | one of "pure", "biological", "bash", "cut", "acid", "stab", "heat", "cold", "electric"
| `amount`            | amount of damage
| `armor_penetration` | how much of the armor the damage instance ignores
| `armor_multiplier`  | multiplier on `armor_penetration`
| `damage_multiplier` | multiplier on `amount`

Example:

```JSON
"melee_damage": [
  {
    "damage_type": "electric",
    "amount": 4.0,
    "armor_penetration": 1,
    "armor_multiplier": 1.2,
    "damage_multiplier": 1.4
  }
]
```

## "melee_dice", "melee_dice_sides"
(integer, optional)

Number of dices and their sides that are rolled on monster melee attack. This defines the amount of bash damage.

## "grab_strength"
(integer, optional)

Intensity of the grab effect applied by this monster. Defaults to 1, is only useful for monster with a GRAB special attack and the GRABS flag. A monster with grab_strength = n applies a grab as if it was n zombies. A player with `max(Str,Dex)<=n` has no chance of breaking that grab.

## "melee_cut"
(integer, optional)

Amount of cutting damage added to die roll on monster melee attack.

## "armor_bash", "armor_cut", "armor_stab", "armor_acid", "armor_fire"
(integer, optional)

Monster protection from bashing, cutting, stabbing, acid and fire damage.

## "weakpoints"
(array of objects, optional)

Weakpoints in the monster's protection.

| field               | description
| ---                 | ---
| `name`              | Name of the weakpoint.
| `coverage`          | Base percentage chance of hitting the weakpoint. May be increased by skill level. (e.g. A coverage of 5 means a 5% base chance of hitting the weakpoint)
| `armor_mult`        | multipler on the monster's base protection when hitting the weakpoint.
| `armor_penalty`     | a flat penalty to the monster's protection, applied after the multiplier.
| `damage_mult`       | multipler on the post-armor damage when hitting the weakpoint.
| `crit_mult`         | multipler on the post-armor damage when critically hitting the weakpoint.
| `required_effects`  | list of effect names applied to the monster required to hit the weakpoint.
| `effects`           | list of effects objects that may be applied to the monster by hitting the weakpoint.

The `effects` field is a list of objects with the following subfields.

| field               | description
| ---                 | ---
| `effect`            | The effect type.
| `chance`            | The probability of causing the effect.
| `duration`          | The duration of the effect. Either a (min, max) pair or a single value.
| `permanent`         | Whether the effect is permanent.
| `intensity`         | The intensity of the effect. Either a (min, max) pair or a single value.
| `damage_required`   | The range of damage, as a percentage of max health, required to trigger the effect.
| `message`           | The message to print, if the player triggers the effect. Should take a single template parameter, referencing the monster's name.
## "vision_day", "vision_night"
(integer, optional)

Vision range in full daylight and in total darkness.

## "luminance"
(integer, optional)

Amount of light passively output by monster. Ranges from 0 to 10.

## "hp"
(integer, required)

Monster hit points.

## "bleed_rate"
(integer, optional)

Percent multiplier on all bleed effects' duration applied to the monster. Values below the default of 100 mean a resistance to bleed, values above 100 make the monster bleed longer and more intensive. 0 translates to bleed immunity.

## "death_drops"
(string or item group, optional)

An item group that is used to spawn items when the monster dies. This can be an inlined item group, see [ITEM_SPAWN.md](ITEM_SPAWN.md). The default subtype is "distribution".

## "death_function"
(object, optional)

How the monster behaves on death.
```cpp
{
    "corpse_type": "NORMAL", // can be: BROKEN, NO_CORPSE, NORMAL (default)
    "message": "The %s dies!", // substitute %s for the monster's name.
    "effect": { "id": "death_boomer", "hit_self": true }  // the actual effect that gets called when the monster dies.  follows the syntax of fake_spell.
}
```

## "emit_field"
(array of objects of emit_id and time_duration, optional)
"emit_fields": [ { "emit_id": "emit_gum_web", "delay": "30 m" } ],

What field the monster emits and how often it does so. Time duration can use strings: "1 h", "60 m", "3600 s" etc...

## "regenerates"
(integer, optional)

Number of hitpoints regenerated per turn.

## "regenerates_in_dark"
(boolean, optional)

Monster regenerates very quickly in poorly lit tiles.

## "regen_morale"
(boolean, optional)

Will stop fleeing if at max hp, and regen anger and morale.

## "flags"
(array of strings, optional)

Monster flags. See [JSON_FLAGS.md](JSON_FLAGS.md) for a full list.

## "fear_triggers", "anger_triggers", "placate_triggers"
(array of strings, optional)

What makes the monster afraid / angry / what calms it. See [JSON_FLAGS.md](JSON_FLAGS.md) for a full list

## "revert_to_itype"
(string, optional)

If not empty and a valid item id, the monster can be converted into this item by the player, when it's friendly. This is usually used for turrets and similar to revert the turret monster back into the turret item, which can be picked up and placed elsewhere.

## "starting_ammo"
(object, optional)

An object containing ammo that newly spawned monsters start with. This is useful for a monster that has a special attack that consumes ammo. Example:

```JSON
"starting_ammo": { "9mm": 100, "40mm_frag": 100 }
```

## "upgrades"
(boolean or object, optional)

Controls how this monster is upgraded over time. It can either be the single value `false` (which is the default and disables upgrading) or an object describing the upgrades.

Example:

```JSON
"upgrades": {
    "into_group": "GROUP_ZOMBIE_UPGRADE",
    "half_life": 28
}
```

The upgrades object may have the following members:

| field        | description
| ---          | ---
| `half_life`  | (int) Days in which half of the monsters upgrade according to an approximated exponential progression. It is multiplied with the evolution scaling factor (at the time of this writing, 4).
| `into_group` | (string, optional) The upgraded monster's type is taken from the specified group. 
| `into`       | (string, optional) The upgraded monster's type.
| `age_grow`   | (int, optional) Number of days needed for monster to change into another monster. Does not scale with the evolution factor.

## "reproduction"
(dictionary, optional)

The monster's reproduction cycle, if any. Supports:

| field          | description
| ---            | ---
| `baby_monster` | (string, optional) the id of the monster spawned on reproduction for monsters who give live births. You must declare either this or `baby_egg` for reproduction to work.
| `baby_egg`     | (string, optional) The id of the egg type to spawn for egg-laying monsters. You must declare either this or "baby_monster" for reproduction to work.
| `baby_count`   | (int) Number of new creatures or eggs to spawn on reproduction.
| `baby_timer`   | (int) Number of days between reproduction events.

## "baby_flags"
(Array, optional)
Designate seasons during which this monster is capable of reproduction. ie: `[ "SPRING", "SUMMER" ]`

## "shearing"
(array of objects, optional)

A set of items that are given to the player when they shear this monster. These entries can be duplicates and are one of these 4 types:
```cpp
"shearing": [
    {
        "result": "wool",
        "amount": 100        // exact amount
    },
    {
        "result": "rags",
        "amount": [10, 100]  // random number in range ( inclusive )
    },
    {
        "result": "leather",
        "ratio_mass": 0.25   // amount from percentage of mass ( kilograms )
    },
    {
        "result": "wool",
        "ratio_volume": 0.60 // amount from percentage of volume ( liters )
    }
]
```

This means that when this monster is sheared, it will give: 100 units of wool, 10 to 100 pieces of rag, 25% of its body mass as leather and 60% of its volume as wool.

## "speed_description"
(string, optional)

By default monsters will use the `"DEFAULT"` speed description.
```JSON
"speed_description": "SPEED_DESCRIPTION_ID"
```

## "petfood"
(object, optional)

Decides whether this monster can be tamed. `%s` is the monster name.
```cpp
"petfood": {
    "food": [ "CATFOOD", "YULECATFOOD" ], // food categories this monster accepts
    "feed": "The gigantic %s decides not to maul you today.", // (optional) message when feeding the monster the food
    "pet": "The %s is enjoying hunting the red laser dot." // (optional) message when playing with pet
}
```

## "special_when_hit"
(array, optional)

A special defense attack, triggered when the monster is attacked. It should contain an array with the id of the defense (see Monster defense attacks in [JSON_FLAGS.md](JSON_FLAGS.md)) and the chance for that defense to be actually triggered. Example:

```JSON
"special_when_hit": [ "ZAPBACK", 100 ]
```

## "attack_effs"
(array of objects, optional)

A set of effects that may get applied to the attacked creature when the monster successfully attacks. Example:

```JSON
"attack_effs": [
    {
        "id": "paralyzepoison",
        "duration": 33,
        "chance": 50
    }
]
```

Each element of the array should be an object containing the following members:

| field           | description
| ---             | ---
| `id`            | (string, required) The id of the effect that is to be applied.
| `duration`      | (integer, optional) How long (in turns) the effect should last.
| `affect_hit_bp` | (boolean, optional) Whether the effect should be applied to the hit body part instead of the one set below.
| `bp`            | (string, optional) The body part that where the effect is applied. The default is to apply the effect to the whole body. Note that some effects may require a specific body part (e.g. "hot") and others may require the whole body (e.g. "meth").
| `permanent`     | (boolean, optional) Whether the effect is permanent, in which case the "duration" will be ignored. The default is non-permanent.
| `chance`        | (integer, optional) The chance of the effect getting applied.

## "path_settings"
(object, optional)

| field                | description
| ---                  | ---
| `max_dist`           | (int, default 0) Maximum direct distance of path
| `max_length`         | (int, default -1) Maximum total length of path
| `bash_strength`      | (int, default -1) Monster strength when bashing through an obstacle
| `allow_open_doors`   | (bool, default false) Monster knows how to open doors
| `avoid_traps`        | (bool, default false) Monster avoids stepping into traps
| `allow_climb_stairs` | (bool, default true) Monster may climb stairs
| `avoid_sharp`        | (bool, default false) Monster may avoid sharp things like barbed wire

## "special_attacks"
(array of special attack definitions, optional)

Monster's special attacks. This should be an array, each element of it should be an object (new style) or an array (old style).

The old style array should contain 2 elements: the id of the attack (see [JSON_FLAGS.md](JSON_FLAGS.md) for a list) and the cooldown for that attack. Example:

```JSON
"special_attacks": [ [ "GRAB", 10 ] ]
```

The new style object can contain a "type" member (string) - "cooldown" member (integer) pair for the three types listed below, the "id" of an explicitly defined monster attack (from monster_attacks.json) or a spell (see [MAGIC.md]). It may contain additional members as required by the specific attack. Possible types are listed below. Example:

```JSON
"special_attacks": [
    { "type": "leap", "cooldown": 10, "max_range": 4 }
]
```
In the case of separately defined attacks the object has to contain at least an "id" member. In this case the attack will use the default attack data defined in monster_attacks.json, if a field is additionally defined it will overwrite those defaults. These attacks have the common "type": "monster_attack", see below for possible fields. Example:

```JSON
"special_attacks": [
    { "id": "impale" }
]
```

"special_attacks" may contain any mixture of old and new style entries:

```JSON
"special_attacks": [
    [ "GRAB", 10 ],
    { "type": "leap", "cooldown": 8, "max_range": 4 },
    { "id": "impale", "cooldown": 5, "min_mul": 1, "max_mul": 3 }
]
```
This monster can attempt a grab every ten turns, a leap with a maximum range of 4 every eight and an impale attack with 1-3x damage multiplier every five turns.

# Monster special attack types
The listed attack types can be as monster special attacks (see "special_attacks").

## "monster_attack"

The common type for JSON-defined attacks. Note, you don't have to declare it in the monster attack data, use the "id" of the desired attack instead. All fields beyond `id` are optional.

| field                 | description
| ---                   | ---
| `cooldown`			| Integer, amount of turns between uses.
| `damage_max_instance` | Array of objects, see ## "melee_damage" 
| `min_mul`, `max_mul`  | Sets the bounds on the range of damage done. For each attack, the above defined amount of damage will be multiplied by a 
|						| randomly rolled mulltiplier between the values min_mul and max_mul. Default 0.5 and 1.0, meaning each attack will do at least half of the defined damage.
| `move_cost`           | Integer, moves needed to complete special attack. Default 100.
| `accuracy`            | Integer, if defined the attack will use a different accuracy from monster's regular melee attack.
| `body_parts`			| List, If empty the regular melee roll body part selection is used. If non-empty, a body part is selected from the map to be
|						| targeted with a chance proportional to the value.
| `attack_chance`		| Integer, percent chance of the attack being successfully used if a monster attempts it. Default 100.
| `range`       		| Integer, range of the attack in tiles (Default 1, this equals melee range). Melee attacks require unobstructed straight paths.
| `no_adjacent`			| Boolean, default false. Attack can't target adjacent creatures.
| `effects`				| Array, defines additional effects for the attack to add.
| `throw_strength`		| Integer, if larger than 0 the attack will attempt to throw the target, every 10 strength equals one tile of distance thrown.
| `miss_msg_u`			| String, message for missed attack against the player.
| `miss_msg_npc`		| String, message for missed attack against an NPC.
| `hit_dmg_u`			| String, message for succesful attack against the player.
| `hit_dmg_npc`			| String, message for succesful attack against an NPC.
| `no_dmg_msg_u`		| String, message for a 0-damage attack against the player.
| `no_dmg_msg_npc`		| String, message for a 0-damage attack against an NPC.
| `throw_msg_u`		    | String, message for a flinging attack against the player.
| `throw_msg_npc`		| String, message for a flinging attack against an NPC.

## "bite"

Makes monster use teeth to bite opponent, uses the same fields as "monster_attack" attacks. Monster bites can give infections if the target is grabbed at the same time.

| field                 | description
| ---                   | ---
| `infection_chance`    | Chance to give infection in a percentage. Exact chance is infection_chance / 100. 


## "leap"

Makes the monster leap a few tiles. It supports the following additional properties:

| field                | description
| ---                  | ---
| `max_range`          | (Required) Maximal range of attack.
| `min_range`          | (Required) Minimal range needed for attack.
| `allow_no_target`    | This prevents monster from using the ability on empty space.
| `move_cost`          | Turns needed to complete special attack. 100 move_cost with 100 speed is equal to 1 second/turn.
| `min_consider_range` | Minimal range to consider for using specific attack.
| `max_consider_range` | Maximal range to consider for using specific attack.


## "gun"

Fires a gun at a target. If friendly, will avoid harming the player.
- Moves some existing moon-phase tests to `tests/moon_test.cpp`

| field                       | description
| ---                         | ---
| `gun_type`                  | (Required) Valid item id of a gun that will be used to perform the attack.
| `ammo_type`                 | (Required) Valid item id of the ammo the gun will be loaded with.  Monster should also have a "starting_ammo" field with this ammo. For example: `"ammo_type" : "50bmg", "starting_ammo" : {"50bmg":100}`
| `max_ammo`                  | Cap on ammo. If ammo goes above this value for any reason, a debug message will be printed.
| `fake_str`                  | Strength stat of the fake NPC that will execute the attack. 8 if not specified.
| `fake_dex`                  | Dexterity stat of the fake NPC that will execute the attack. 8 if not specified.
| `fake_int`                  | Intelligence stat of the fake NPC that will execute the attack. 8 if not specified.
| `fake_per`                  | Perception stat of the fake NPC that will execute the attack. 8 if not specified.
| `fake_skills`               | Array of 2 element arrays of skill id and skill level pairs.
| `move_cost`                 | Move cost of executing the attack
| `require_targeting_player`  | If true, the monster will need to "target" the player, wasting `targeting_cost` moves, putting the attack on cooldown and making warning sounds, unless it attacked something that needs to be targeted recently.  Gives "grace period" to player.
| `require_targeting_npc`     | As above, but with npcs.
| `require_targeting_monster` | As above, but with monsters.
| `targeting_timeout`         | Targeting status will be applied for this many turns.  Note that targeting applies to turret, not targets.
| `targeting_timeout_extend`  | Successfully attacking will extend the targeting for this many turns. Can be negative.
| `targeting_cost`            | Move cost of targeting the player. Only applied if attacking the player and didn't target player within last 5 turns.
| `laser_lock`                | If true and attacking a creature that isn't laser-locked but needs to be targeted, the monster will act as if it had no targeting status (and waste time targeting), the target will become laser-locked, and if the target is the player, it will cause a warning.  Laser-locking affects the target, but isn't tied to specific attacker.
| `range`                     | Maximum range at which targets will be acquired.
| `range_no_burst`            | Maximum range at which targets will be attacked with a burst (if applicable).
| `burst_limit`               | Limit on burst size.
| `description`               | Description of the attack being executed if seen by the player.
| `targeting_sound`           | Description of the sound made when targeting.
| `targeting_volume`          | Volume of the sound made when targeting.
| `no_ammo_sound`             | Description of the sound made when out of ammo.
