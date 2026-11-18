---
title: "Enemy God AI - Lantern of Souls"
summary: "Overarching AI that directs the movements of individual enemy AI's."
hideMeta: true
---

![Guard Image](/images/god-ai/GuardSpooky.jpeg)
*Drawing by Callie Michaelis*

## Overview

I was tasked with creating a God AI for our video game capstone project that would direct the movements of up to 3 individual enemy AI's. Our game, Lantern of Souls, is a third-person horror-stealth game, so enemy interactions are a key part of the core gameplay. We were having an issue where the enemies were bunching up and overwhelming the player, or were always on the other side of the map and never once interacted with the player. To remedy this, we decided to implement a "God AI," similar to what was used in Alien: Isolation, to direct the movements of the enemies and push them in the direction of the player without being overwhelming. It would also allow us to interrupt the normal enemy behaviors to send them to a specific location when needed, such as when the player uses a distraction ability.

*For all code shown, overall design and inital prototyping was done by myself, but code cleanup and refactoring were done with the help of Jacob Memmott.*

## Design Philosophy

The two main things we needed the God AI to handle were telling the enemies which waypoint they should path to next, and sending the nearest enemy to a specified location.

Up to this point, the enemies were set up to roam the level by moving randomly between different "waypoints," giving the illusion that they are wandering around while also providing unpredictability in where they were going to go next. However, this random choice would often cause them to bunch up or avoid the player, which was not the type of gameplay we were going for. To resolve this, we decided that no two enemies should be pathing towards the same waypoint at a time, and that the enemy furthest from the player should be sent to the waypoint closest to the player's location. This would prevent the enemies from bunching up, and create moments of tension with the enemy in close proximity to the player.

For player abilities such as a distraction, we needed to be able to interrupt the closest enemy's normal process and send them to investigate. We also had an additional enemy, the Watcher, that would spot the player and notify the nearest enemy to go and investigate that area and that needed this functionality.

## Assign New Target Waypoint

[![Assign New Target Waypoint BP](/images/god-ai/AssignNewTargetWaypoint.png)](/images/god-ai/AssignNewTargetWaypoint.png)

Previously, when an enemy would reach a waypoint their state tree would ping a function that would randomly select a waypoint from those containted in their waypoint list. With this new function, the God AI would check if this enemy is the one furthest from the player. If so, the enemy would get sent to the waypoint located closest to the player. If not, the God AI would pick a random waypoint from the enemy's waypoint list that no other enemy was going towards and send them there.

### Code Breakdown

{{< accordion title="General Overview" >}}

[![Assign New Target Waypoint BP](/images/god-ai/AssignNewTargetWaypoint.png)](/images/god-ai/AssignNewTargetWaypoint.png)
After buffering to prevent race conditions, we get the new target waypoint and update it in the map that is being used to keep track of all the enemies in the scene and the waypoints they are traveling towards.

{{< /accordion >}}

{{< accordion title="Get New Target Waypoint" >}}

#### Function Overview
[![Get New Target Waypoint BP](/images/god-ai/GetNewTargetWaypoint.png)](/images/god-ai/GetNewTargetWaypoint.png)

#### Check if this is the furthest enemy from the player
[![Furthest from Player](/images/god-ai/IsFurthestFromPlayer.png)](/images/god-ai/IsFurthestFromPlayer.png)
First, we check if this is the furthest enemy from the player, and store that info in a bool. You can see more of the furthest enemy logic further down.

#### Check for edge cases
[![Edge Cases](/images/god-ai/WaypointCheck.png)](/images/god-ai/WaypointCheck.png)
Next, we want to get the waypoint list and check for edge cases. If there are no entries in the waypoint list provided, an error is thrown. If there is only one waypoint listed, then the enemy should be sent there, no need for further processing. Otherewise, continue on.

#### Direct the furthest enemy towards the player
[![Send Furthest Guard](/images/god-ai/DirectFurthestGuard.png)](/images/god-ai/DirectFurthestGuard.png)
If this is the furthest enemy from the player, then get the nearest waypoint to the player and see if any other enemy is already going there. If not, then send this enemy to it. Otherwise, remove this waypoint from the available options and continue on.

#### Select random available waypoint
[![Select Random Waypoint](/images/god-ai/SelectAvailableWaypoint.png)](/images/god-ai/SelectAvailableWaypoint.png)
If this enemy isn't the furthest from the player or there's already an enemy going towards the nearest waypoint to the player, choose a random waypoint and send the enemy to it.

{{< /accordion >}}

{{< accordion title="Is Furthest Enemy from Player" >}}

[![Send Furthest Guard Function](/images/god-ai/IsFurthestFromPlayerInDepth.png)](/images/god-ai/IsFurthestFromPlayerInDepth.png)
For each enemy, take their location on the nav mesh vs the player's location on the nav mesh and calculate how far they are apart. If that distance is greater than the current path length, then store this new path as the longest and the current enemy as the furthest. After that loop has finished, if this enemy is the furthest then return true. Otherwise, return false.

{{< /accordion >}}

{{< accordion title="Get Nearest Waypoint to the Player" >}}

[![Get Closest Waypoint](/images/god-ai/GetNearestWaypoint.png)](/images/god-ai/GetNearestWaypoint.png)
Similar to selecting the furthest enemy, for each waypoint we check to see, based on the nav mesh, how far it is from the player. If it is the closest current option, it is saved and the waypoint is put into a variable. Whichever waypoint is in that variable by the end of the loop is sent off as the closest waypoint. If there is no waypoint in that final variable, an error is thrown.

{{< /accordion >}}

{{< accordion title="Is Waypoint Already Targeted" >}}

[![Already Targeted](/images/god-ai/AlreadyTargeted.png)](/images/god-ai/AlreadyTargeted.png)
For each enemy, if it's targeted waypoint location is equal to the intended new waypoint, then return true that this waypoint is already targeted. If none of those return true, then return false.

{{< /accordion >}}

{{< accordion title="Select Random Waypoint" >}}

[![Select Random Waypoint](/images/god-ai/SelectRandomWaypoint.png)](/images/god-ai/SelectRandomWaypoint.png)
Shuffle all the waypoints passed in, and cycle through them in a for each loop. If a waypoint is not currently being targeted, return it. If all the waypoints are currently being targeted, return one at random.

{{< /accordion >}}

## Send Closest Enemy

![Send Closest Guard](/images/god-ai/DistractionAbility.gif)
When a distraction ability is used or the Watcher spots the player, the nearest enemy needs to be identified and sent to that location. The function is provided with a location, which it uses to compare with all the other enemies in the level to figure out which is closest to the specified location. Once the closest enemy is identified, we cast to the enemy's controller and send it ot the stimulus with the specified speed modifier.

### Code Breakdown

{{< accordion title="Initial Event" >}}

[![Send Closest Guard Event](/images/god-ai/ClosestGuardEvent.png)](/images/god-ai/ClosestGuardEvent.png)
The process is kicked off by this event, with the location of the stimulus and the movement speed multiplier passed into the function.

{{< /accordion >}}

{{< accordion title="Send Closest Enemy to Location" >}}

[![Send Closest Guard](/images/god-ai/SendClosestGuard.png)](/images/god-ai/SendClosestGuard.png)
First, the location and movement speed modifiers are set. Then, for each enemy in the level we compute its distance to the stimulus and compare that to the current shortest path. If the new path is shorter, we use this new path as the shortest path and save the enemy reference in a variable.

At the end of the loop, we get the enemy's controller and tell it to go to the new stimulus with the speed multiplier specified.

{{< /accordion >}}
