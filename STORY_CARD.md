# Narrative Context Language: Story Cards

While NaCL focuses on having structure to make the memory and author's notes
easily usable by players and the AI, there's a different focus when it comes to
story cards.

As story cards have a soft limit of ~1000 characters, you may need to cram a lot
of detailed information about something into a relatively small space. When you
consider the amount of space you have — only 1024 tokens (~2048 characters)
without premium — for all pieces of context (memory, author's notes, story cards,
the actual story, etc.), this limit is compounded if you have multiple triggers
for story cards active at once.

Because of this need to conserve space, back in the olden days of AI dungeon,
a lot of research was done and compiled into the
[AI Dungeon World Info Research Sheet](
	https://github.com/valahraban/AID-World-Info-research-sheet/blob/main/AID%20WI%20Research%20Sheet.md#preface
)
on various strategies to conserve space and notes on how the AI processes
context. While a lot of time has passed, meaning that the exact functionality of
the AI(s) has changed, the general findings from the research still work.

For story cards, NaCL uses general strategies from some popular formats (Zaltys
and Caveman) to provide formatted story card info that is optimized for space,
readability, and ease of use.

## Table of Contents
- [Format](#format)
- [Keywords](#keywords)
	- [Entity Keywords](#entity-keywords)
	- [Location Keywords](#location-keywords)
	- [Series Info Keywords](#series-info-keywords)
- [Additional Options](#additional-options)
- [Examples](#examples)

## Format
For story cards, NaCL follows a similar base format that it uses for writing
story memory: you have a header like `[Thing]` and each piece of information
on a line starting with a `>` and ending with a period.

For each story card, you start with a header with the name of whatever the
story card is about, and a line having the basic description of it.

For example, if we had a location called "Grassy Plains":
```
[Grassy Plains]
> Lush grassy plain; many trees.
```

For separating pieces of information on a line, you have several options:
- Comma `,`
	- Good for lists of things that are together.
- Semicolon `;`
	- Good for a stronger separation of things in the same category.
- Slash `/`
	- Good as a middle ground between `,` and `;` — marks things that are
	  separate bits of information that link together under the same piece of
	  information.
- Ampersand `&`
	- Good when you want to group things together that should be listed with
	  an "&"/"and" between them.

For each following detail about the story card item, a keyword will be used
to specify what sort of information is being listed.  
Note that the keyword is put together with the `>` to help visually separate
the beginning description line from the keyword ones.  
To see the list of keywords, refer to the [keyword section](#keywords).

As an example, let's say we have someone named Jim who:
- Is a 25 year old anthropomorphic wolf male at 5'6 and 160lbs.
- Wears a white shirt, jeans, and brown boots.
- Has an average build with grey fur.
- Is depressed.
- Eats mainly pizza and salad.
- Is friends with you and someone named Naomi.
- Drops his phone when defeated.

```
[Jim]
> Anthro wolf/male/25yr/5'6/160lbs.
>WEAR white shirt, jeans, brown boots.
>APPEAR average build/grey fur.
>MIND depressed.
>DIET pizza, salad.
>FRIEND you, naomi.
>LOOT phone.
```

AI Dungeon reports this as 182 characters.  
Compare this to straightforward prose, which clocks in at 340 characters:

```
Jim is a 25 year old anthro wolf male at 5'6 and 160lbs.
He wears a white shirt, jeans, and brown boots.
He has an average build with grey fur, and commonly wears a white shirt, jeans, and brown boots. Jim is suffering from depression; he mainly eats pizza and salad.
Jim is friends with you and Naomi. If defeated, Jim will drop his phone.
```

The NaCL comes *very* close to an optimized Zaltys equivalent, which has 181 characters:
```
Jim:[Anthro Wolf<Male/25y>;WEAR<Jim>:shirt<white>/jeans/boots<brown>;APPEAR<Jim>:average/fur<grey>;MIND<Jim>:depressed;DIET<Jim>:pizza/salad;FRIENDS<Jim>:you/naomi;LOOT<Jim>:phone.]
```

While the Zaltys is *barely* more condensed, my testing shows that the NaCL
functions similarly with the benefit of being easier to read and write.

## Keywords

There are many keywords found and listed in the AI Dungeon World Info Research
Sheet linked at the start.  

For convenience, I have read the research sheet (and some other sources) and
compiled my own list of keywords and descriptions of their functionality;
additionally, I have separated the list into sections for keywords most suited
to story cards for entities, locations, and series info.

Despite how odd it may seem, you can generally assume that the condensed form
of a keyword functions either the same or better than the full word; this is
due to how the AI tokenizes text — despite something like `SUMM` looking like
`SUMMARY` cut off, the AI is able to extract the same meaning.

Note that this is not the be-all and end-all list of keywords.  
If you feel you have a better word for a specific kind of information, try it
out and see if the AI uses it as you want it to. If it makes sense, you can use
a keyword marked for locations/series with entities and vice-versa.  
This list is just keywords that were noted to be effective and consistent.


### Entity Keywords
| Keyword     	| Alternative           	| Condensed 	| Functionality                                                                                                                                                                      	|
|-------------	|-----------------------	|-----------	|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------	|
| DESCRIPTION 	|                       	| DESC      	| Base information about entity; race, gender, etc.<br>Generally unused as the first line is implicitly DESC.                                                                        	|
| SUMMARY     	|                       	| SUMM      	| Generic info category.<br>Good for anything that doesn't fit in with others, prose-like information, or stating things you want consistently described.                            	|
| DETAIL      	|                       	| DETA      	| Similar to summary, but good for details on inanimate objects.                                                                                                                     	|
| APPEAR      	| FORM, PHYSIQUE, BUILD 	| APPE      	| The physical appearance/build of entity.                                                                                                                                           	|
| RACE  	|				               	| 		     	| The race of an entity, if mentioning it in DESC isn't enough.                                                                                                                                                         	|
| WEAR        	| WORN, CLOTHING        	|           	| The outfit being worn by entity.                                                                                                                                                   	|
| MIND        	| ATTITUDE, MENTAL      	|           	| Mental traits; attitude, outlook, etc.<br>MIND is generally better, but MENTAL can be an option.                                                                                   	|
| MOVEMENT  	| 			               	| MOV     	| How an entity moves around.                                                                                                                                                         	|
| GRAB  	| 				               	|	     	| How an entity grabs or manipulates objects.                                                                                                                                                         	|
| TRAIT       	|                       	| TRA          	| Fundamental, literal traits.<br>MIND affects behavior, TRAIT affects how something *is*.                                                                                           	|
| POWERS      	|                       	| POWER     	| List powers/abilities of entity.                                                                                                                                                   	|
| EFFECT      	|                       	| EFFEC     	| Effects of the entity, such as magical/science effects of object.                                                                                                                  	|
| DIET        	|                       	|           	| What the entity eats.                                                                                                                                                              	|
| FRIEND      	|                       	|           	| Who entity is friends with.                                                                                                                                                        	|
| SEE         	|                       	|           	| Describe how entity senses and perceives things around them.                                                                                                                       	|
| LIMIT        	| LACK	                 	|           	| Describe limiting features for entity.<br>LIMIT is good for things described with numbers (e.g. "one arm"), while LACK is good for non-numerical things (e.g. "eyes" for eyeless). 	|
| CONDITION   	|                       	| COND      	| What conditions entity has (e.g. "poisoned")                                                                                                                                       	|
| STATUS      	|                       	|           	| Status as in "title or assocation to something", not a condition.                                                                                                                  	|
| TITLE		  	| 			               	|		     	| A literal title for the character, which they may be called.                                                                                                                                                         	|
| ALIGNMENT   	|                       	| ALIGN     	| The moral/ethical alignment of entity.<br>Can use well known D&D alignments like "Chaotic Good", or sillier ones like "Lawful Stupid".                                             	|
| MOTIVATION  	| PASSION               	| MOTIV     	| Motivations for an entity.                                                                                                                                                         	|
| QUOTES	  	|			               	|		     	| Quotes for character; can also be used to give examples for accent.                                                                                                                                                         	|
| LOOT        	|                       	|           	| What entity drops on defeat or being searched.                                                                                                                                     	|
| ADJECTIVE   	|                       	| ADJ       	| Notable adjectives to be supplied onto descriptions of entity.                                                                                                                     	|
| MODIFIER    	|                       	| MODIF     	| Modifiers onto description of entity.<br>ADJ is more like a prefix, while MODIF functions more as a suffix.                                                                        	|
| GRAMMAR     	|                       	| GRAM      	| Describes speech patterns when etity communicates.                                                                                                                                 	|
| VOCAB       	|                       	|           	| Describes vocab used by entity to communicate.<br>GRAMMAR describes format, VOCAB describes words.<br>VOCAB can be used to signal entity speaking a certain language.              	|
| INVENTORY   	|                       	| INV       	| List things held in inventory.                                                                                                                                                     	|
| EQUIP       	|                       	|           	| List things currently equipped/held.                                                                                                                                               	|
| LIKE        	|                       	|           	| Describe things liked by entity.                                                                                                                                                   	|
| HATE        	| DISLIKE               	|           	| Described things hated/disliked by entity.                                                                                                                                         	|
| RELATION    	|                       	|           	| Describe general relationship to something else.                                                                                                                                   	|
| ALLY        	| ALLIES, BOND          	|           	| Describe positive relation with entity.                                                                                                                                            	|
| ENEMY       	| ENEMIES               	|           	| Describe negative relation with entity.                                                                                                                                            	|
| CREATE      	| PRODUCES              	|           	| Whatever the entity is able to create/produce.                                                                                                                                     	|
| BANNED      	|                       	| BANN      	| Behavior that is forbidden.                                                                                                                                                        	|
### Location Keywords
| Keyword    	| Alternative  	| Condensed  	| Functionality                                                                                  	|
|------------	|--------------	|------------	|------------------------------------------------------------------------------------------------	|
| ATMOSPHERE 	|              	| ATMOS      	| The narrative atmosphere of the location.                                                      	|
| TONE       	|              	|            	| The tone describing the location.                                                              	|
| FEATURES   	|              	| FEAT       	| Features of the location.                                                                      	|
| CLIMATE    	|              	| CLIM       	| The climate of the location.                                                                   	|
| GEOGRAPHY  	|              	| GEO, GEOGR 	| Geographical description of location.                                                          	|
| BIOME      	|              	|            	| General type of biome for location.<br>Specific information is better for DESC or SUMM.        	|
| ENTRANCE   	|              	| ENTER      	| The entrances into the location.<br>(e.g. "SW mountain" for southwest entrance from mountain)  	|
| EXIT       	|              	|            	| The exits from the location.<br>(e.g. "E dock" for east exit to dock)                          	|
| CITIZENS   	| FOLK, CENSUS 	| CITIZ      	| Describes racial diversity of location.                                                        	|
| ETHNICITY  	|              	| ETHN       	| Similar to CITIZ, but better when describing human variation.                                  	|
| NATIVES    	|              	| NATIV      	| Like CITIZ or ETHN, but describing people native to location, instead of just living there.    	|
| LIFE       	|              	|            	| Life forms found in location.                                                                  	|
| THREAT     	|              	| THRE          | Hostile life forms found in location.<br>Good to specify level of threat, such as "wolf,minor" 	|

### Series Info Keywords
| Keyword 	| Alternative 	| Condensed 	| Functionality                         	|
|---------	|-------------	|-----------	|---------------------------------------	|
| THEME   	|             	| THEM          	| The theme of a series.                	|
| ORIGIN  	|             	|           	| The origin of something for a series. 	|
| GENRE		|				| GENR				| The genre of a series.					|

## Additional Options
If you wish to have slightly more technical syntax that may provide some
benefits when using NaCL, here are some optional pieces of additional syntax:

1) List grouping
	- When making lists of things, placing the list within square brackets
	  may help keep the items encapsulated together:
	  ```
	  >INV[marble, gum, slingshot].
	  ```
2) Grouped Relation
	- If you want to add a detail for a bit of info, you can use parentheses to
	  describe the detail with it:
	  ```
	  >FEATURE flowing river("azure rapids").
	  ```
	- Another example, that I use a lot:
	  ```
	  >APPE hair(long,brown)
	  >WEAR black(shirt,pants)
	  ```
	  This one is pretty effective, with the AI correctly joining "long and brown"
	  with "hair" and "black" with "shirt" and "pants".

3) Direct implication
	- You can put a colon after a keyword, which may help sell the assocation
	  between the keyword and items following it:
	  ```
	  >WEAR:white shirt/shoes/jeans
	  ```
4) Full encapsulation
	- If you're worried about the AI not associating your information with the
	  entity listed in the header, you can put brackets around everything after
	  the header to group them all together:
	  ```
	  [Steve] {
	  > Human/male/5'9.
	  >MIND energetic.
	  }
	  ```
5) Symbolic Following
	- The symbol `=>` may be used to note that something "follows" something
	  else. In Zaltys format, this was used for denoting things like directions
	  and exits they lead to:
	  ```
	  [The Dry Expanse]
	  > Hot arid desert.
	  >EXIT SW => Great Oasis; E => Endless Desolation.
	  ```

6) Multi-Word Connection
	- If you feel like a term made up of multiple words is not being grouped
	  together correctly, you can try joining the words with underscores instead
	  of spaces:
	  ```
	  [Jim]
	  > Anthro_Wolf/male/25_years/5'6_tall/160lbs.
	  >WEAR white_shirt, jeans, brown_boots
	  >APPEAR average_build/grey_fur.
	  ```
	- Another strategy is smashing words together, like `FlamingOrangeEyes` for
	  `Flaming Orange Eyes`, but this carries the risk of the AI chopping up
	  the word in a weird way and getting a different meaning, like
	  `Flamingo Range Eyes` for the example used.
	
	- Additionally, quoting something will help keep the words grouped together.

## Examples
For the purpose of helping learn how the format and keywords work, here are
several examples of story cards and their corresponding NaCL.

#### Greenbriar Hills
This is an NaCL adaptation of a location example of Zaltys format from the
World Info Research
Sheet:
```
[Greenbriar Hills]
> hilly region.
>CLIM temperate.
>BIOME hilly forests; large river, East(SW from mountain).
>EXIT NW to Yrthwood; E to The Sunless Basin; SW to Teeth of Gwahlur(mountain).
>FEAT prickly plants; strange lights at night("Old Light").
>NATIVE Lizardfolk(friendly); Halflings(prosperous village "Greenbriar").
>LIFE varied; giant lizards(in river); crested birds.
>THRE wolves(minor).
```
On testing, this NaCL story card is effective like the Zaltys version, and
actually more condensed (even without full optimization) at 399 characters vs.
the 437 of the Zaltys version.

The Zaltys example, for comparison:
```
Greenbriar Hills:[ DESC: hilly_region; CLIM<Greenbriar Hills>: temperate; BIOME: hilly forests large_river:SW<from_mountain>⇒E; EXIT: <NW⇒Yrthwood> <E⇒The Sunless Basin> <SW⇒mountains:Teeth of Gwahlur>; FEATURES<Greenbriar Hills>: prickly_plants strange_lights_at_night<'Old Light'>; NATIVE: lizardfolk<friendly tribal> halflings<prosperous_village:'Greenbriar'>; LIFE: varied giant_lizards<in_river> crested_birds; THRE: wolves<minor>.]
```

### Siren
This is another example from the World Info Research Sheet, about a drink
called Siren:
```
[Siren]
> A popular drink.
>APPEAR Sapphire colored/silver bubbles/sprig of mint/smells alcoholic/tastes like rust metal.
>EFFECT Inspires drinker to dance.
```
Again, this is equivalent in effectiveness and a bit more space efficient
(156 chars vs 169).

For reference, the Zaltys example:
```
Siren:[A popular drink;DESCRIPT<Siren>:Sapphire-colored/silvery bubbles/sprig of mint/smells alcoholic/tastes like rusty metal;EFFECTS<Siren>:Inspires drinker to dance.]
```

### Mike Haggar
Another example, used when showcasing the space-optimized version of
Zaltys ("Snek"), of Mike Haggar from the Final Fight series.
This one is a fairly complex example of a character, which is why it was
used to showcase the condensed Snek format:
```
[Mike Haggar]
> Human/male/202cm/140kg.
>APPE Stocky&muscular/big args/short_brown_hair & brown_mustache.
>MIND Just/direct/hardy/upright.
>WEAR Eyeglasses(at work)/brown_boots&green_pants/bandolier.
>SUMM Born 1943/from games Final_Fight&Street_Fighter/pro_wrestler(ex)/grew up on streets of Metro City/Mayor of Metro City/fought_gangs(Mad_Gear&Skull_Cross)/fights gangs.
>LIKE curry&rice.
>QUOTE "It's my job to keep Metro City safe!".
>FRIEND Cody&Guy&Carlos(from Brazil).
>DAUGHTER Jessica.
```

At 494 characters, this is *slightly smaller* than the optimized Snek format
version of 496 chars, which is impressive considering I actually expanded the
NaCL with more keywords to add clarity. A fully optimized NaCL could be
smaller.

For reference, the Snek version:
```
Mike Haggar:[Human<male/202cm/140kg>;APPE<Haggar>:Stocky&muscular/bigarms/brown<shorthair&mustache>;MIND<Haggar>:Just/direct/upbeat/hardy/upright;WEAR:Eyeglasses<at work>/brownboots&greenpants/bandolier;SUMM<Haggar>:Born 1943/from:<Final Fight&Street Fighter> games/ex:prowrestler/grew up on streets<in Metro City>/mayor<Metro City>/fought gangs:<Mad Gear&Skull Cross>/fights gangs/"It's my job to keep Metro City safe!"/loves:curry&rice/friends:<Cody&Guy&Carlos:from Brazil>/daughter:<Jessica>.]
```

On testing, the NaCL worked well, with Mike Haggar referencing all the details
listed for him during a conversation.