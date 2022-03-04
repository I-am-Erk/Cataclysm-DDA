# Labyrinthine Structure design doc

This is meant as a guide and a way of recording my thoughts on how to build Labyrinthine Structures, which I call Structures for short, in CDDA.

## What is?

Labyrinthine Structures are a proposed dungeon type connected to the Exodii faction, bionics, and the netherum. They are created by the Exodii as a way of taking advantage of the natural copying properties of the netherum. The exodii collect a bunch of useful parts into a building and teleport it into the netherum for a long while, then bring it back. They don't have a lot of use for these on CDDA-earth at the moment, but they're indispensable as a source of material and parts if the Exodii node winds up in a pre-industrial world.

### Netherum copying

The netherum is a between-space with no obvious matter or rules of its own, and the stuff of the netherum 'imitates' things that pass through it. Since most teleportation involves passing through the netherum, the whole interspace is full of all kinds of weird things spun out of the traffic. The Labyrinthine technique is a bit more organized and so creates stuff that is a bit more reality-like than the usual netherum products.

Materials taken from the more "orderly" portion of a Structure are fairly similar to mundane matter. They become more similar the more they are processed or used, as they are forced to constrain to our laws of reality. The loot from inside a Structure should **not** be a source of "magic tech". However, it **can** be a way to get access to materials that would otherwise be hard to explain in a post-apocalyptic setting, especially when used as crafting precursors to make something equivalent to a modern earth product that shouldn't really be craftable.

## General design considerations

Structures were initially thought up as a way to have a dungeon where you can obtain CBM related loot as we move CBMs to the Exodii. They retain that purpose but have grown a little from there, to keep them from being too one-note. In general they should also be a good place to obtain semi-advanced components or things that can be made into those components, such as welding rods and copper wire on the low end, or electronic scrap and pieces to batteries on the high end.

A key thing to remember with any loot not directly related to the CBM tech tree is that, as a *general rule*, Structures should not step too far out of theme, and when they do the results should be equivalent to something you could have looted elsewhere, but maybe more convenient or faster somehow (such as using nanosand as a concrete additive to make reinforced concrete in a single step). We should be quite careful about letting Structure loot exceed things you could have done without the Structure, except where it applies to furthering your CBM tech tree. This is a guideline, not a rule. The idea is to prevent this from turning into the new late-game "one stop shop" dungeon, as labs and mansions used to be.

# Layout and Mapping Design Notes

See [Pocket Dimensions Implementation Request](https://github.com/CleverRaven/Cataclysm-DDA/issues/55794) for the basics. Structures will be the first(?) pocket dimension, a bigger-on-the-inside dungeon that generates inside of noneuclidean space. This adds some new design potential that previous dungeons did not have.
- You can't easily dig your way through walls, because they're not connected in euclidean space and you won't get anywhere. This forces a more linear approach, although the game remains very open world and encourages out of the box solutions nonetheless
- The rigid map connections system makes mapping more predictable, and not needing to load all maps at once means connectivity is easier to manage.
- As you go deeper into the Strucutre, we can access specific new kinds of maps, making it possible to have some really weird stuff randomly generate, but not in every Strucutre.

**Monster spawning** will be infinite within the Structure. See section on enemies for more info.


## Rough guide to phases

I've broken the design/map concepts of the Structure into a few "phases" that increase as you go deeper into the Structure. These can generally overlap with each other a bit. Later on we should blur the lines further and have some of the later phase stuff appearing in earlier phases as you near the transition, but the early phases have a harder boundary. 

Not every Structure should have all the phases. Many structures might just end before you get to a Hazard for example. Rarely, a large Structure might branch into two different Hazard phases.

### Entry phase
When you first enter the Structure we should get a mostly linear approach for around 10 zones. There may be small branches off the main trail, but we'll use `prefer_connect` to ensure these don't get too huge. The Entry Zone monsters should be fairly easy and low in numbers. The loot and furniture should mostly be mundane stuff that fits the exodii. Not a lot of CBMs here but a lot of nice raw materials and scrap.

### Hazard phase
Around depth 10, the available zones change so that, at some point, you're likely to encounter the start point to a hazard region. Using proposed `prefer_connect` tags in the dimension code, we can have high-priority zones that open out into other zones with specific hazards, which then connect to each other for a while, pulling up matching hazard maps a few zones long. The hazard you get can help define what kind of loot is availalbe, possibly add a few different monsters, and definitely requires some specific tools to adventure further.

#### Low-oxygen hazard
These zones open with an airlock. Once you're in, there is no breathable air and you need an oxygen tank to survive. Mobs are normal. Ideally, weaponry that needs O2 like some guns should not work in here, but that will need more code support.
- Loot should include some CBMs specifically related to breathing and air filtration
- terrain and furniture should emphasize more tanks and bulbous things, giving it a submarine or space ship feel compared to other zones

#### Radiation hazard
These zones open with a heavy lead door, and inside is bathed in radiation, some areas worse than others.
- Fleshborgs here are replaced with something else that fills a similar role
- Terrain should include a lot of fun radiation based stuff: RTGs, pillars of glowing uranium, stuff like that. These provide hazards, the area around them is more radioactive.
- In general the radiation level should be something that can be handled with a regular radiation suit, but the specific local terrain hazards increase it beyond what a suit can handle, forcing you to limit exposure time.

#### Other hazards
We should just start with two to get the mapping done, but some other ideas include:
- low temperature - like the old ice labs, how do you deal with a zone that is -90 degrees celsius? Unlike the old ice labs, we should have some more detailed issues related to the absurd extreme cold
- high temperature - uncomfortably hot at baseline with exposed fire and lava that gets more problematically hot close up. The migo facilities already do this so may be better left out
- chaotic - the nested maps used to make the space randomly regenerate every so often, so that rooms move and change even as you're in them. Mapping this would be tricky to do in a way that kept players and monsters from winding up trapped inside solid objects, unless we had code to prevent that. (trapping players and monsters temporarily inside enclosed spaces with no exits is fine though)
- sideways - the map is switched to be several z-levels tall, with furniture and things placed along one edge, so that the player feels as though they're standing on one side wall of a tipped-sideways space. Requires a lot of new mapping and some specialized furniture and terrain to help make the place navigable, but could be a really fun idea
- smoke-filled - filled with clouds of acrid smoke that sting the eyes and make you cough unless you wear eye and mouth protection. Mostly at a level 1 intensity that blurs but doesn't totally obscure vision and can be passed just by wrapping a covering on your mouth and wearing goggles, but there are vents putting it out in clouds that increase the density and require better protection.
- silence - Making noise in this region can have severe repercussions of some kind, probably some kind of particularly deadly enemy that responds to it. There are fewer mobs, but you'll want to take them out without making a racket.

### Post-hazard "reward" phase
After the hazard phase ends, we should get a somewhat peaceful zone or two that gives the player a chance to recoup, and in those zones we should have a chance of spawning some of the first-level top loot. This is where we might find a significant chunk of an autodoc, for example, or a room with some useful CBMs. For the first pass on the Structures, this is where we'd finish off.

Sometimes instead of a "reward" room we might get a "boss" room, or there could be a "boss" room inside the hazard itself. Ideally not both.

### Deeper phases
After the hazard phase, we'll eventually want to see phases going deeper into the netherum, becoming increasingly chaotic. These should bring in new enemies and bigger threats and have some higher grade loot, but I think a first pass should just get the more "stable" phases done first and leave this for later.

## Combining these elements into design thoughts
The combination of timer-respawning monsters and random zones that advance as you go deeper into the Structure and add new threats and enemies means that Structures offer a different pathway towards loot than any of our existing challenge zones. The big design problem making Labs in the past, or Mansions, or anything with cool loot is that no matter how threatening it is, eventually players find a way to clear the threats and take out everything that's not bolted down. Making things heavier, or harder to move, just adds inconvenience, not interesting play challenge.

The setup of the Structure defies that. You cannot make this place "clear". There are many things you can dismantle and remove, but the more time you spend doing that, the more likely you are to find enemies returning from the Netherum to bother you. The deeper you go, the more interesting stuff you find, but also the more tired you become, and it's not a place you can rest - and it will take longer to get back up and there is a higher and higher chance the early monsters will respawn. Dragging piles of good items behind you on the ground also takes a long time and increases the chance of monsters returning.

For this all to be meaningful it is important the monsters in the Structure be designed such that they cannot be fully ignored at any stage of the game. Ideally they should be manageable by a mid-game level player, so we cannot just escalate their armaments and armour, but the balance of monsters should be such that a high level player never feels totally safe, and must always carefully consider if they want to risk new waves appearing.

Because this is a completely open-world game, there are many strategies players are going to find to circumvent this. For example, it seems likely to me that we'll find people dragging in pre-built single-tile vehicles to construct barricades and turrets and things to make quick "fallback" points of their own. Or perhaps there will be a meta shift towards some of the quick craftable barriers that could hold off waves of enemies. I suspect these locations will really encourage bringing along some competent NPCs, should we ever develop competent NPCs. All of these are desirable. What is not desirable and would constitute a design failure requiring revision is:
- this becoming a "get to it as fast as you can" meta, where the Structure is so vital to effective gameplay that everyone beelines for one asap and we see a lot of meta developing about "how quickly can I take on a Structure". That means we've made the loot too good compared to other locations.
- high level players finding the respawn effect just a nuisance, because the monsters can't threaten them but never go away fully
- nobody ever goes into a Structure because the loot isn't worth the existential threat, or people *only* go into a Structure if they've developed a ton of janky metagame solutions to solve all the problems, because going in without janky metagame solutions is a death sentence.

Provided we meet these goals, then the combo effect lets us do some stuff we can't otherwise manage.
- The deeper levels of a Structure can contain advanced loot, in the form of furniture and large items where "how the hell do I get this out of here past the waves of enemies" is a very relevant question. We may even wish to steal a page from the original Defense mode and have some of these become their own entities that you can drag along, eg. you convert an autodoc bed into a vehicle and roll it with you, but that also makes it a target and your enemies will go after it as well as you. Now you have to get out while defending your priceless loot!
- The deep*est* levels are legitimately so hard to get to and so hard to get out of that we can potentially store some truly interesting stuff down there. They're rare to spawn at all, and reaching them will require getting through a lot of challenges, and getting out will have the same effect. It's the first opportunity we've had to have a genuine end-game dungeon item. This should nevertheless not be game-breaking, but due to the nature of the Structure and the stuff in it, it could definitely be stuff that would otherwise be a hard "no" on a design front.

# Enemy design
See [Monster design](https://github.com/CleverRaven/Cataclysm-DDA/issues/55795) issue for details on initial enemy planning.

Basically, we should design enemies for each phase that focus not just on large damage numbers, but on asking questions that will challenge most levels of players. I would like these enemies to skip the "high armour, high hp, high damage" meta posed by high level zombies. Instead, I propose the recurring theme that Structure enemies:
- have multiple attack options that occur on different vectors, eg a physical and an electric attack
- often have abilities that temporarily weaken or nullify your gear. I really like the potential of this because it scales: high level characters usually rely on strong gear, and weakening it affects them just as much as lower levels.
- often synergize - for example, ranged stiltwalkers have a status attack that makes fleshborg and wiregnat enemies more dangerous
- are designed to attack from several directions at once, in reasonably high numbers, so most of the time even the strongest Structure enemies should not hold up against a concentrated attack for long (exception: special enemies and "bosses" of course), but individual attacks are pretty powerful if they get through. This is another one that helps with scaling, because there's a hard cap on how much shooting you can do in a given time span. If a single shot takes out the enemy, then your limit is how many shots you can fire before the enemy gets to you and tears you apart, and having a bigger more advanced gun doesn't help much.

## Waves and respawning
 A zone should use Effect on Conditionals to set timers which cause the spawning of waves of monsters at the exits/entrances of the zone: the EoC would be triggered by the side you enter the zone on, so the entrance you came in is exempt from monster spawning. You should get between 0 and ~4 waves of monsters or so, come in over the course of a few minutes at most, and the number, size, and frequency of waves could worsen as you go deeper into the dungeon. Once the waves are exhausted there should be a timer to prevent any further waves from spawning in this zone for a few hours (shorter the deeper you go). This means that you can never clear the dungeon, but the timer is long enough that you can potentially clear a segment for as long as you need. When you come back, the monsters return. If you decide to sleep, the monsters return. 

One potential later-area loot item might prevent waves from respawning in a zone, so long as it requires some resource to operate. That would allow players to potentially rest in the later zones, at the price of guaranteeing that they're going to have to fight their way back out.
