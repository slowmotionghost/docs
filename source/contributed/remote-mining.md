---
title: Remote mining
contributed:
    name: slowmotionghost
    link: https://s[`Creep`](http://docs.screeps.com/api/#Creep)s.com/a/#!/profile/slowmotionghost
    date: 2017-08-23
---
This article is a beginners guide to remote mining, and should provide ideas that can be worked towards, but leave the coding and implementation to you.

## What is remote mining?
Remote mining is the activity of harvesting [`Energy`](http://docs.screeps.com/api/#Resource) in rooms that are not owned by the player. To develop a productive economy, players should code a form remote mining to maximise their [`Energy`](http://docs.screeps.com/api/#Resource) income. There are many ways to perform remote mining, some of which will be discussed in this article.
## Harvesting
In order to obtain [`Energy`](http://docs.screeps.com/api/#Resource) from remote [`Sources`](http://docs.screeps.com/api/#Source), they need to be harvested, just like [`Sources`](http://docs.screeps.com/api/#Source) in owned rooms. [`Sources`](http://docs.screeps.com/api/#Source) in unclaimed rooms provides 1500 [`Energy`](http://docs.screeps.com/api/#Resource) per 300 ticks. If the room is reserved, this means the [`Source`](http://docs.screeps.com/api/#Source) provides 3000 [`Energy`](http://docs.screeps.com/api/#Resource) per 300 ticks. This shows the clear benefit of reserving a room, however remote harvesting can still be profitable without use of reservation. Each work part a [`Creep`](http://docs.screeps.com/api/#Creep) has can harvest 2 [`Energy`](http://docs.screeps.com/api/#Resource) per tick, meaning a [`Creep`](http://docs.screeps.com/api/#Creep) should have 5 work parts to fully harvest a [`Source`](http://docs.screeps.com/api/#Source) (in a reserved room). Depending on your collection method, a harvester may only require work and move parts, as [`Energy`](http://docs.screeps.com/api/#Resource) harvested by a [`Creep`](http://docs.screeps.com/api/#Creep) without a carry part is dropped and can be collected from the ground or a [`Container`](http://docs.screeps.com/api/#StructureContainer). It may be worth considering a carry part if your harvester [`Creep`](http://docs.screeps.com/api/#Creep) will be used to repair buildings in the remote room (such as a [`Container`](http://docs.screeps.com/api/#StructureContainer) it is using) or your harvester [`Creep`](http://docs.screeps.com/api/#Creep) will also be used to ferry the [`Energy`](http://docs.screeps.com/api/#Resource) back to your storage. The latter method is simple to implement as it requires only one role or job, however it is less [`Energy`](http://docs.screeps.com/api/#Resource) efficient to transport the work parts around, so having a dedicated harvester is worth considering.
## Collecting
[`Energy`](http://docs.screeps.com/api/#Resource) collection can be performed using the ground, [`Containers`](http://docs.screeps.com/api/#StructureContainer) or carry parts. The simplest method to implement is to let harvested [`Energy`](http://docs.screeps.com/api/#Resource) fall on the ground, where it will sit and wait to be collected. However, be aware that dropped [`Resources`](http://docs.screeps.com/api/#Resource) decay at a rate of ceil(amount/1000) per tick giving a minimum cost of 1 [`Energy`](http://docs.screeps.com/api/#Resource) per tick. [`Energy`](http://docs.screeps.com/api/#Resource) in [`Containers`](http://docs.screeps.com/api/#StructureContainer) does not decay, however the [`Container`](http://docs.screeps.com/api/#StructureContainer) itself decays at a rate of 5000 hits every 100 ticks (500 ticks in owned rooms). If we were to not repair the [`Container`](http://docs.screeps.com/api/#StructureContainer) and rebuild it every time it completely decayed, it would mean and investment of 5000 [`Energy`](http://docs.screeps.com/api/#Resource) every 5000 ticks (250000 hits divided by 5000 hit decay, then multiplied by 100 for decay frequency) or 1 [`Energy`](http://docs.screeps.com/api/#Resource) per tick, as well as loss from [`Energy`](http://docs.screeps.com/api/#Resource) decay during downtime. Repairing costs 1 [`Energy`](http://docs.screeps.com/api/#Resource) for 100 hits, meaning repair costs 50 [`Energy`](http://docs.screeps.com/api/#Resource) to cover the decay, every 100 ticks, at a cost of 0.5 [`Energy`](http://docs.screeps.com/api/#Resource) per ticks, while there is still an initial investment to build the [`Container`](http://docs.screeps.com/api/#StructureContainer), repairing it is clearly the cheapest method of collecting [`Energy`](http://docs.screeps.com/api/#Resource). If a [`Container`](http://docs.screeps.com/api/#StructureContainer) is used, the harvester needs to 'stand' on top of the [`Container`](http://docs.screeps.com/api/#StructureContainer) so [`Energy`](http://docs.screeps.com/api/#Resource) drops into it (or have a carry part and transfer into it, although this will be more cpu costly).
## Transporting
Once [`Energy`](http://docs.screeps.com/api/#Resource) is harvested, it needs to be transported back to your room so you can use it. This can be done by dedicated haulers or the [`Creep`](http://docs.screeps.com/api/#Creep) used to harvest the [`Energy`](http://docs.screeps.com/api/#Resource). As mentioned, the latter is less efficient due to not getting use out of the work parts while the creep is hauling. Haulers should be made with MOVE and CARRY parts as a minimum, while some players use a single work part to repair [`Roads`](http://docs.screeps.com/api/#StructureRoad) (mentioned in the next section). The main choice for transportation is whether to use [`Roads](http://docs.screeps.com/api/#StructureRoad) or not. [`Roads`](http://docs.screeps.com/api/#StructureRoad) decrease the fatigue generated by a [`Creep`](http://docs.screeps.com/api/#Creep). A rule of thumb for MOVE parts needed is half of the parts if a [`Road`](http://docs.screeps.com/api/#StructureRoad) is not being used, or a third if [`Roads`](http://docs.screeps.com/api/#StructureRoad) are being used. The cost to repair a [`Road`](http://docs.screeps.com/api/#StructureRoad) per tick is 1 [`Energy`](http://docs.screeps.com/api/#Resource) every 1000 ticks (on plain terrain), which is significantly lower than the body part costs of the extra MOVE parts, so while harder to implement, [`Roads`](http://docs.screeps.com/api/#StructureRoad) are generally a worthwhile investment.
A more advanced requirement for transportation is tailoring [`Creep`](http://docs.screeps.com/api/#Creep) size so efficiency remains high. If [`Creeps`](http://docs.screeps.com/api/#Creep) are too large, body parts are wasted, but too small and total [`Energy`](http://docs.screeps.com/api/#Resource) collection isn't acheived. This can be tackled a number of ways, with some players operating with a pool of full size haulers that serve all the remote [`Sources`](http://docs.screeps.com/api/#Source), and adjust the population of this pool based on need. Other players use haulers assigned to each [`Source`](http://docs.screeps.com/api/#Source) and tailor body size to the distance a hauler has to travel to reach that particular [`Source`](http://docs.screeps.com/api/#Source).
## Repairing and building
Repairing has been mentioned already in several sections, and will depend on the use of [`Structures`](http://docs.screeps.com/api/#Structure) in each room. If you and going to opt for not having [`Structures`](http://docs.screeps.com/api/#Structure) in your remote rooms you don't need to worry about this. However, if you are going to make use of [`Structures`](http://docs.screeps.com/api/#Structure) ([`Roads`](http://docs.screeps.com/api/#StructureRoad) and [`Containers`](http://docs.screeps.com/api/#StructureContainer)), it is important to repair them to prevent decay. Rebuilding is always more expensive than repair.
In order to repair a structure a [`Creep`](http://docs.screeps.com/api/#Creep) needs [`Energy`](http://docs.screeps.com/api/#Resource) and a work part. This could be acheived using a dedicated repairer, the harvester or the hauler. A dedicated repairer is likely the most efficient option of these, however the amount of these needs to be tailored to need to prevent wastage of body parts. Using a hauler to repair is useful as allows [`Roads`](http://docs.screeps.com/api/#StructureRoad) to be repaired as they are being used, meaning they are unlikely to decay. The harvester should ideally remain stationed at the [`Source`](http://docs.screeps.com/api/#Source) they are harvesting so if they are repairing, it should be the [`Container`](http://docs.screeps.com/api/#StructureContainer) they are sitting on. While the hauler and harvester will already have access to [`Energy`](http://docs.screeps.com/api/#Resource), a dedicated repairer will need to be coded to pick up [`Energy`](http://docs.screeps.com/api/#Resource), which can be taken from your storage, or from conveniently filled [`Containers`](http://docs.screeps.com/api/#StructureContainer) in the room requiring repairs. Obviously, in order to have a structure to repair, it needs to built. This could be performed by creeps that would repair. It may be useful to automate [`Container`](http://docs.screeps.com/api/#StructureContainer) building by the harvester, to ensure every harvester is collecting into a container (if that is the method you want to use).
## Reserving
As mentioned in the harvesting section, reserving rooms is key to making them profitable. In order to reserve a room, a [`Creep`](http://docs.screeps.com/api/#Creep) needs to have at least one RESERVE part. [`Creeps`](http://docs.screeps.com/api/#Creep) that have RESERVE parts have a reduce lifetime of 500 ticks, meaning [`Creeps`](http://docs.screeps.com/api/#Creep) with reserve parts should have MOVE and RESERVE parts only, other parts will be wasted as will have a reduced lifetime, so would be better used by another [`Creep`](http://docs.screeps.com/api/#Creep). Reservation can be performed constantly using a [`Creep`](http://docs.screeps.com/api/#Creep) with one reserve part, however the reserve timer decreases and the same rate that one RESERVE part increases it, meaning if you want to build up the timer you need at least 2 reserve parts. It would be worth considering using maximum sized reservers when a timer is below a certain amount, in order to save on cpu by having less reservers active on average at once.
## Defending
[`Invasions`](http://docs.screeps.com/invaders.html) not only occur in your rooms, but also rooms that are being harvested. [`Invasions`](http://docs.screeps.com/invaders.html) only come from room exits to rooms that are not owned or reserved. In these rooms, it would be worth considering defending yourself against [`Invasions`](http://docs.screeps.com/invaders.html). There are several options one could take. Firstly, you could do nothing and lose your [`Creeps`](http://docs.screeps.com/api/#Creep) and profit of that room. Secondly, you could retreat out of that room, to prevent losing [`Creeps`](http://docs.screeps.com/api/#Creep). While these saved [`Creeps`](http://docs.screeps.com/api/#Creep) will likely be excess to demands elsewhere, this prevents spawning new [`Creeps`](http://docs.screeps.com/api/#Creep) that walk into the invaded room to die. Alternatively, you can respond to the [`Invasions`](http://docs.screeps.com/invaders.html) using soldiers. Discussing combat is outside of the scope of this article, but it is worth thinking how you will respond to these [`Invasions`](http://docs.screeps.com/invaders.html). Spawning response units will take spawn and travel time, so some players have permanent guards stationed to reduce mining time lost. While this may be inefficient in the case of guards in every room, a guard in your main room covering several remote rooms may be worth the investment, depending on the size and type of the guard.

## Other things to consider
As you develop your code further, more advanced ideas could include:
- [`Source Keeper`](http://docs.screeps.com/api/#StructureKeeperLair) room mining
- Automated [`Road`](http://docs.screeps.com/api/#StructureRoad) construction
- Double-source miner (mines both [`Sources`](http://docs.screeps.com/api/#Source) in each room to save cpu).