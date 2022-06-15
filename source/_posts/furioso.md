---
title: furioso
date: 2022-06-14 14:59:55
tags:
    - Godot
    - D&D
categories:
    - Game
    - Design
---

# About Me

> **furioso<狂怒>** 旨在笔者用于自学Godot游戏引擎初期的demo项目，初期的构思是2D像素类ARPG游戏，核心玩法在于丰富的build以及roguelike地下城探险。

## D&D *龙与地下城*

D20、open-ended、RPG、dungeon、class

## Godot Game Engine

https://godotengine.org/
- GDScript
- free
- open-source


# Art

# System Design
## Character Stats
### Strength *STR*
- how hard hit something.
- how much you can carry.

### Dexterity *DEX*
- how fast you are.
- how well you are with ranged attacks.

### Constitution *CON*
- hit points.
- resistance to negative effect.
- how fast you sober up.

### Wisdom *WIS*
- how perceptive you are.
- naturally notice.

### Intelligence *INT*
- how much you know about things.

### Charisma *CHA*
- how good you persuading people.
- how well you get on with NPCs.

## Armor
- [ ] helmet
- [ ] waist
- [ ] belt
- [ ] gloves
- [ ] legs
- [ ] cloth
- [ ] boots
- [ ] necklace
- [ ] rings

## Weapons
### weapon properties
Many Weapons have special Properties related to their use, as shown in the Weapons table.

#### Ammunition
You can use a weapon that has the Ammunition property to make a ranged Attack only if you have Ammunition to fire from the weapon. Each time you Attack with the weapon, you expend one piece of Ammunition. Drawing the Ammunition from a Quiver, case, or other container is part of the Attack (you need a free hand to load a one-handed weapon). At the end of the battle, you can recover half your expended Ammunition by taking a minute to Search the battlefield. If you use a weapon that has the Ammunition property to make a melee Attack, you treat the weapon as an Improvised Weapon (see “Improvised Weapons” later in the section). A sling must be loaded to deal any damage when used in this way.

#### Finesse
When making an Attack with a finesse weapon, you use your choice of your Strength or Dexterity modifier for the Attack and Damage Rolls. You must use the same modifier for both rolls.

#### Heavy
Small Creatures have disadvantage on Attack rolls with heavy Weapons. A heavy weapon’s size and bulk make it too large for a Small creature to use effectively.

#### Light
A light weapon is small and easy to handle, making it ideal for use when Fighting with two Weapons.

#### Loading
Because of the time required to load this weapon, you can fire only one piece of Ammunition from it when you use an Action, bonus Action, or Reaction to fire it, regardless of the number of attacks you can normally make.

#### Range
A weapon that can be used to make a ranged Attack has a range in parentheses after the Ammunition or thrown property. The range lists two numbers. The first is the weapon’s normal range in feet, and the second indicates the weapon’s Long Range. When Attacking a target beyond normal range, you have disadvantage on the Attack roll. You can’t Attack a target beyond the weapon’s Long Range.

#### Reach
This weapon adds 5 feet to your reach when you Attack with it, as well as when determining your reach for Opportunity Attacks with it.

#### Special
A weapon with the special property has unusual rules governing its use, explained in the weapon’s description (see “Special Weapons” later in this section).

#### Thrown
If a weapon has the thrown property, you can throw the weapon to make a ranged Attack. If the weapon is a melee weapon, you use the same ability modifier for that Attack roll and damage roll that you would use for a melee Attack with the weapon. For example, if you throw a Handaxe, you use your Strength, but if you throw a Dagger, you can use either your Strength or your Dexterity, since the Dagger has the finesse property.

#### Two-Handed
This weapon requires two hands when you Attack with it.

#### Versatile
This weapon can be used with one or two hands. A damage value in parentheses appears with the property—the damage when the weapon is used with two hands to make a melee Attack.

### Weapons By Category 
#### simple melee weapons
- [ ] <a href="#Club">club</a> `短棍`
- [ ] <a href="#Dagger">dagger</a> `匕首`
- [ ] <a href="#Greatclub">greatclub</a> `长棍`
- [ ] <a href="#Handaxe">handaxe</a> `手斧`
- [ ] <a href="#Javelin">javelin</a> `标枪`
- [ ] <a href="#Light Hammer">light hammer</a> `轻锤`
- [ ] <a href="#Mace">mace</a> `权杖`
- [ ] <a href="#Quarterstaff">quarter staff</a> `铁头木棍`
- [ ] <a href="#Sickle">sickle</a> `镰刀`
- [ ] <a href="#Spear">spear</a> `短矛`

#### simple ranged weapons
- [ ] <a href="#Crossbow,light">crossbow,light</a> `轻弩`
- [ ] <a href="#Dart">dart</a> `飞镖`
- [ ] <a href="#Shortbow">shortbow</a> `短弓`
- [ ] <a href="#Sling">sling</a> `弹弓`

#### martial melee weapons
- [ ] <a href="#Battleaxe">battleaxe</a> `双头战斧`
- [ ] <a href="#Flail">flail</a> `连枷`
- [ ] <a href="#Glaive">glaive</a> `长刀`
- [ ] <a href="#Greataxe">greataxe</a> `巨斧`
- [ ] <a href="#Greatsword">greatsword</a> `巨剑`
- [ ] <a href="#Halberd">halberd</a> `战戟`
- [ ] <a href="#Lance">lance</a> `长枪`
- [ ] <a href="#Longsword">longsword</a> `长剑`
- [ ] <a href="#Maul">maul</a> `大槌`
- [ ] <a href="#Morningstar">morningstar</a> `流星锤`
- [ ] <a href="#Pike">pike</a> `长矛`
- [ ] <a href="#Rapier">rapier</a> `刺剑`
- [ ] <a href="#Scimitar">scimitar</a> `弯刀`
- [ ] <a href="#Shortsword">short sword</a> `短剑`
- [ ] <a href="#Trident">trident</a> `三叉戟`
- [ ] <a href="#War pick">war pick</a> `钉头锤`
- [ ] <a href="#Warhammer">warhammer</a> `战锤`
- [ ] <a href="#Whip">whip</a> `长鞭`

#### martial ranged weapons
- [ ] <a href="#Blowgun">blowgun</a> `吹箭筒`
- [ ] <a href="#Crossbow,hand">crossbow,hand</a> `手弩`
- [ ] <a href="#Crossbow,heavy">crossbow,heavy</a> `重弩`
- [ ] <a href="#Longbow">longbow</a> `长弓`
- [ ] <a href="#Net">net</a> `网`


WEAPONS TABLE
| Weapon                                           | Weight | Damage Type | Armor Slot | Properties                                             |
| ------------------------------------------------ | ------ | ----------- | ---------- | ------------------------------------------------------ |
| <a id="Club">Club</a> `短棍`                     | 2      | Bludgeoning |            | Light                                                  |
| <a id="Dagger">Dagger</a> `匕首`                 | 1      | Piercing    |            | Finesse, light, thrown (range 20/60)                   |
| <a id="Greatclub">Greatclub</a> `长棍`           | 10     | Bludgeoning |            | Two-handed                                             |
| <a id="Handaxe">Handaxe</a> `手斧`               | 2      | Slashing    |            | Light, thrown (range 20/60)                            |
| <a id="Javelin">Javelin</a> `标枪`               | 2      | Piercing    |            | Thrown (range 30/120)                                  |
| <a id="Light Hammer">Light Hammer</a> `轻锤`     | 2      | Bludgeoning |            | Light, thrown (range 20/60)                            |
| <a id="Mace">Mace</a> `权杖`                     | 4      | Bludgeoning |            | -                                                      |
| <a id="Quarterstaff">Quarterstaff</a> `铁头木棍` | 4      | Bludgeoning |            | Versatile (1d8)                                        |
| <a id="Sickle">Sickle</a> `镰刀`                 | 2      | Slashing    |            | Light                                                  |
| <a id="Spear">Spear</a> `短矛`                   | 3      | Piercing    |            | Thrown (range 20/60), versatile (1d8)                  |
| <a id="Crossbow,light">Crossbow,light</a> `轻弩` | 5      | Piercing    |            | Ammunition (range 80/320), loading, two-handed         |
| <a id="Dart">Dart</a> `飞镖`                     | 1/4    | Piercing    |            | Finesse, thrown (range 20/60)                          |
| <a id="Shortbow">Shortbow</a> `短弓`             | 2      | Piercing    |            | Ammunition (range 80/320), two-handed                  |
| <a id="Sling">Sling</a> `弹弓`                   | -      | Bludgeoning |            | Ammunition (range 30/120)                              |
| <a id="Battleaxe">Battleaxe</a> `双头战斧`       | 4      | Slashing    |            | Versatile (1d10)                                       |
| <a id="Flail">Flail</a> `连枷`                   | 2      | Bludgeoning |            | -                                                      |
| <a id="Glaive">Glaive</a> `长刀`                 | 6      | Slashing    |            | Heavy, reach, two-handed                               |
| <a id="Greataxe">Greataxe</a> `巨斧`             | 7      | Slashing    |            | Heavy, two-handed                                      |
| <a id="Greatsword">Greatsword</a> `巨剑`         | 6      | Slashing    |            | Heavy, two-handed                                      |
| <a id="Halberd">Halberd</a> `战戟`               | 6      | Slashing    |            | Heavy, reach, two-handed                               |
| <a id="Lance">Lance</a> `长枪`                   | 6      | Piercing    |            | Reach, special                                         |
| <a id="Longsword">Longsword</a> `长剑`           | 3      | Slashing    |            | Versatile (1d10)                                       |
| <a id="Maul">Maul</a> `大槌`                     | 10     | Bludgeoning |            | Heavy, two-handed                                      |
| <a id="Morningstar">Morningstar</a> `流星锤`     | 4      | Piercing    |            | -                                                      |
| <a id="Pike">Pike</a> `长矛`                     | 18     | Piercing    |            | Heavy, reach, two-handed                               |
| <a id="Rapier">Rapier</a>  `刺剑`                | 2      | Piercing    |            | Finesse                                                |
| <a id="Scimitar">Scimitar</a>  `弯刀`            | 3      | Slashing    |            | Finesse, light                                         |
| <a id="Shortsword">Shortsword</a> `短剑`         | 2      | Piercing    |            | Finesse, light                                         |
| <a id="Trident">Trident</a>  `三叉戟`            | 4      | Piercing    |            | Thrown (range 20/60), versatile (1d8)                  |
| <a id="War pick">War pick</a> `钉头锤`           | 2      | Piercing    |            | -                                                      |
| <a id="Warhammer">Warhammer</a> `战锤`           | 2      | Bludgeoning |            | Versatile (1d10)                                       |
| <a id="Whip">Whip</a> `长鞭`                     | 3      | Slashing    |            | Finesse, reach                                         |
| <a id="Blowgun">Blowgun</a> `吹箭筒`             | 1      | Piercing    |            | Ammunition (range 25/100), loading                     |
| <a id="Crossbow,hand">Crossbow,hand</a> `手弩`   | 3      | Piercing    |            | Ammunition (range 30/120), light, loading              |
| <a id="Crossbow,heavy">Crossbow,heavy</a> `重弩` | 18     | Piercing    |            | Ammunition (range 100/400), heavy, loading, two-handed |
| <a id="Longbow">Longbow</a> `长弓`               | 2      | Piercing    |            | Ammunition (range 150/600), heavy, two-handed          |
| <a id="Net">Net</a> `网`                         | 3      | -           |            | Special, thrown (range 5/15)                           |


## Classes

## Monsters

## Spells 

## Abilities

## Auras

## Dungeon

## Landscape

## Items

# Simulation