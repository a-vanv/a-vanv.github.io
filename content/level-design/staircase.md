---
title: "Staircase Redesign - Lantern of Souls"
summary: "Redesigning pain points in the level to improve the player's experience."
hideMeta: true
---

## Overview

I was tasked with creating the overall level design for our capstone game, Lantern of Souls. It is a third-person horror-stealth game where the player is trying to sneak around deadly enemies, so giving the player enough information to navigate the space is key.

The level we were building was centered around having two stories, but we were running into an issue where players were not typically going upstairs. When looking into it, we realized that the problem was with the stairs. Players found them difficult to navigate and would often get caught there, discouraging them from utilizing the second floor. Me and my level design team looked through reference and came up with two different styles of stairs, before settling on the one playtesters preferred the most. Since then, we have had no issues with people getting up the stairs.

## The Problem

![First Stairs](/images/staircase/FirstStairs1.png)

When players would come up these stairs, they were presented with this switchback. The problem was that players couldn't see to the top of the stairs, where an enemy could be approaching. Unless the player was going up the stairs backwards, it was impossible for them to be prepared to hide from an enemy that was approaching them. And often, even if they did notice the enemy when coming around the switchback it was often too late.

![First Stairs Swtichback](/images/staircase/FirstStairs2.png)

We needed a staircase design that would allow the player to see what was coming while providing them with escape routes so as not to feel trapped and increase their agency.

## Prototyping and Playtesting

As a level design team, we started by finding reference for different styles of staircases that could work for our level.

{{< accordion title="Gathered Reference" >}}

![Ref Y Stairs](/images/staircase/YStairReference.png)

![Ref Stairs Swtichback](/images/staircase/SpiralStairReference.png)

![Ref Stairs 1](/images/staircase/OtherStairRef1.png)

![Ref Stairs 2](/images/staircase/OtherStairRef2.png)

We spent a lot of time going over Google looking for different styles of staircases. These are just a few of the ones that we considered.

{{< /accordion >}}

When looking at these reference images, we kept in mind the issues with our current staircase design. We talked about which of these options would provide enough information to the player while also giving them options to escape. After much discussion, we decided on the first two images shown in the dropdown above.

Once we had decided on two layouts, we put them into the engine and started getting playtesting data to see which people liked more. They were placed in the save level on different ends of the map to see which one players tended towards.

![Y Stairs Prototype](/images/staircase/Y-Stairs.png)
*Y-Stairs*

![Spiral Stairs Prototype](/images/staircase/SpiralStairs1.png)
*Spiral Stairs*

## The Solution

![Y Stairs Final Gif](/images/staircase/YStairsFinal.gif)

After multiple playtests, the preferred layout became obvious. Players would almost always use the Y-stairs, even if it meant running past the spiral stairs to get there.

The Y-stairs provided enough visibility to be able to see incoming enemies while also providing ways for the player to escape if an enemy started coming towards them.

We implemented the Y-stairs on both sides of the map, and since then players have had no issues getting from floor to floor.
