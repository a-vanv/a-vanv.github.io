---
title: "Enemy God AI - Lantern of Souls"
summary: "Overarching AI that directs the movements of individual enemy AI's."
hideMeta: true
---

![Guard Image](/images/god-ai/GuardSpooky.jpeg)

## Overview

I was tasked with creating a God AI for our video game capstone project that would direct the movements of up to 3 individual enemy AI's. Our game, Lantern of Souls, is a third-person horror-stealth game, so enemy interactions are a key part of the core gameplay. We were having an issue where the enemies were bunching up and overwhelming the player, or were always on the other side of the map and never once interacted with the player. To remedy this, we decided to implement a "God AI," similar to what was used in Alien: Isolation, to direct the movements of the enemies and push them in the direction of the player without being overwhelming. It would also allow us to interrupt the normal enemy behaviors to send them to a specific location when needed, such as when the player uses a distraction ability.

*For all code shown, overall design and inital prototyping was done by myself, but code cleanup and refactoring were done with the help of Jacob Memmott.*

## Design Philosophy

The two main things we needed the God AI to handle were telling the enemies which waypoint they should path to next, and sending the nearest enemy to a specified location.

Up to this point, the enemies were set up to roam the level by moving randomly between different "waypoints," giving the illusion that they are wandering around while also providing unpredictability in where they were going to be going next. However, this random choice would often cause them to bunch up or avoid the player, which was not the type of gameplay we were going for. To resolve this, we decided that no two enemies should be pathing towards the same waypoint at a time, and that the enemy furthest from the player should be sent to the waypoint closest to the player's location. This would prevent the enemies from bunching up, and create moments of tension with the enemy in close proximity to the player.

For player abilities such as a distraction, we needed to be able to interrupt the closest enemy's normal process and send them to investigate. We also had an additional enemy, the Watcher, that would spot the player and notify the nearest enemy to go and investigate that area and that needed this functionality.

## Assign New Target Waypoint

![Assign New Target Waypoint BP](/images/god-ai/AssignNewTargetWaypoint.png)

When an enemy would reach a waypoint, their state tree would ping a function that would randomly select a waypoint from those containted in their waypoint list. With this new funciton, the God AI would check if this is enemy is the one furthest from the player. If so, the enemy would get sent to the waypoint located closest to the player. If not, the God AI would pick a random waypoint from the enemy's waypoint list that no other enemy was going towards and send them there.

### Code Breakdown

{{< accordion title="Code Breakdown" open="true" >}}

{{< /accordion >}}

{{< accordion title="Get New Target Waypoint" >}}

{{< /accordion >}}
