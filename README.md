<p align="center">
	<img src="./nacl.png"/>
</p>

# Narrative Context Language

My personal format for AI Dungeon scenario context,
Narrative Context Language — aka NaCL — is something I developed to make
creating scenarios simpler and more consistent.

For AI Dungeon, you can type your context however you want, but I find that
having a structured, consistent way of formatting it makes things easier for
both you and anyone who wants to edit your context.  
Everything is nice and organized, and you can easily add or delete bits of
context as you need to.

Through use and testing, I have found this format to be effective and useful
for developing and playing scenarios.  
Compared to plain prose, this format has given pretty good results on having
the AI follow my intentions for how a scenario should work; the AI has been
consistent with the context info while also expanding on it in reasonable ways
to make different playthroughs different.

Overall, NaCL should strike a good balance between being usable by both players
and AI.  

### Historical Notes

In the ancient AI Dungeon year of 2020, a lot of research was done on various
methods of providing context and information to the AI, with multiple formats
invented and tested.  
AI Dungeon veterans may remember formats designed to optimize world info, 
such as Zaltys or Caveman.

While the passage of time — changing the AI models and how AI Dungeon works —
throws things more up in the air on the specifics of this old research, the
design of NaCL mirrors some general strategies used.

While looking different, on tokenization by the AI, NaCL should actually resemble
a sort of less condensed mixture of these older formats.

### Story Cards

For using NaCL for story cards, see
[the page on it](https://github.com/Haunt-o/Haunto-Context-Language/blob/main/STORY_CARD.md)  
Since you have to cram story card info into ~1000 characters to describe the
details and features of something, NaCL is written differently for story cards.

I have done a lot of testing and research to make sure NaCL is a worthwhile
format to use for story cards as well.  
It is space efficient, effective, and can be written in an intuitive and easy
to read manner.

## Table of Contents
- [Headers](#headers)
- [Info Markers](#info-markers)
- [Good Practices](#good-practices)
- [Author's Notes](#authors-notes)
- [API Variables](#api-variables)
- [Full NaCL Example](#full-nacl-example)

 ## Format

 ### Headers

 When you want to mark what a specific section of the context is about, a header
 is used.  
 There are two kinds of headers: &nbsp; *Noun Headers* and *Concept Headers*.

- **Noun Header**
	- A noun header is formatted like `[Thing]`.
	- They mark that the section under it has context details about the noun.
	- Examples:
		- `[Jim]`
		- `[The Headquarters]`
		- `[The Pumpkin King]`

- **Concept Header**
	- A concept header is formatted like `||Stuff||`
	- They mark that the section under it has context details about some idea.
	- Examples:
		- `||Setting||`
		- `||Plot||`
		- `||Relationships||`


### Info Markers

The main part of scenario context is the information you want the AI to
remember, so this format has a way to mark each piece of information.

To mark a piece of information, you put a `>` at the beginning.  
For example:
```
> You are cool.
```

#### Nested Info Marker

When you have context info that is related to another piece of info — a
sort of "sub-context" info — you add a `|` to mark info as being part
of the info above it.  
For example:
```
> You are cool.
>| You wear cool shades.
```

If you have nested info on your nested info, just put another `|` at the
end, like `>||`.  
For example:
```
> You are cool.
>| You wear cool shades.
>|| Your cool shades are made of coolium.
```

&nbsp;

### Good Practices

As an AI can always make mistakes, there are some practices for writing context
notes that I, and others, find work and may make things easier for the AI:

1) Keep each piece of information as direct and straightforward as possible.
	- When an AI is fed too much text at once, its ability to keep track of
	  everything at the same time starts to weaken.  
	  While your memory probably won't choke up the AI, it's good to be safe
	  instead of sorry.  
	  To help the AI, keep each individual note free of details or writing that
	  is unnecessary. If you need to expand on an idea, use nested info markers
	  and add additional notes expanding on the main one for a concept.
	
		- If it works for you, a "Caveman" strategy could work, where you just 
		  completely remove all uneeded words.  
		  For example, here's a normal piece of NaCL:
		  ```
		  > Jim has green eyes and long hair.
		  ```
		  Here's the "Caveman" version:
		  ```
		  > Jim green eye. Long hair.
		  ```
		  I wouldn't recommend this unless you have a large amount of
		  memory and are worried about it being long enough for the AI to
		  cut off or get confused about —
		  part of the goal of NaCL is to be a combination of effective for
		  scenario creators and readable by scenario players.
	
	- Your memory should not be just your intro story copy and pasted.  
	  There's literally no point for that; AI Dungeon will just use your intro
	  as memory if you don't have your own.

	  Instead, your memory should be a list of notes and details about the
	  fundamental points of your scenario. The intro story is the
	  body, and the memory should be a skeleton that allows the AI to keep
	  the body moving in a way that fits.

2) Remove as much ambiguity as possible.
	- An AI is just a piece of software trying to make a good guess on the
	  text you give it, it's not infallible and sometimes makes dumb mistakes
	  when trying to interpret the subject of something, even when the subject
	  is easy to infer.

	- When you can, explicitly write the name of whatever the subject of a
	  context note is.  
	  Avoid starting a note with generic pronouns that the AI could potentially
	  confuse with someone else, like "she" or "her".  
	  For example:
	  ```
	  [Mark]
	  > Mark is male.
	  > Mark is cool.
	  > Mark is your friend.
	  ```

	  If we put "He" instead of "Mark" in these lines, the AI could be really
	  dumb and just think "He" refers to some other male in the story. When
	  we explicitly say each thing is about Mark, there's no easy way for the
	  AI to get confused on the subject.
	
	- Related, keep however you reference the player character consistent.  
	  If using second person, use "You"/"Your" to refer to them; if third
	  person, use the player character's name.  
	  Simply add a note about the player character's name in the section for
	  them to have the AI remember.  
	  For example:
	  ```
	  [You]
	  > You are named Jim.
	  > You are cool.
	  > Your hand is slightly big.

	  [Mark]
	  > Mark is friends with you.
	  ```
	
	- Make sure to end your sentences with a period. It's just a small thing
	  to prevent the AI from potentially combining two notes in a weird way.
	
3) Consider what needs to be a put in the memory.
	- Memory should, as mentioned before, be a skeleton for the fundamental
	  details of your story.  
	  Unless your scenario is outright intended to be short, it'd be better to
	  not put temporary details about the start of your story in it.
	  The memory is always referenced, so the AI could get confused and think
	  details about the start of the story are still in play even when the
	  player is way past the start.
	- Immutable details are good for memory, like a character's race and name.  
	  Even then, details other than the basics should be put into a story card
	  if you can; see [the section on story cards](#story-cards) to see how to
	  use NaCL to write story cards effectively.

### Author's Notes

While not literally part of the memory box, author's notes are lumped in
with the context when passed to the AI, so there's also a format for them.

Authors notes use the following 4-letter keywords to convey writing information:
| Keyword 	|       Meaning       	|                Examples                	|
|:-------:	|:--------------------	|:---------------------------------------	|
|   STYL  	| Writing Style       	| verbose, talkative, romantic, arousing 	|
|   GENR  	| Genre for Story     	| comedy, romance, horror                	|
|   TONE  	| Tone of Story       	| depressing, tragic, upbeat             	|
|   RTNG  	| Rating of story     	| PG, PG-13, M, R, X-RATED               	|
|   THEM  	| Theming of Story    	| fantasy, sci-fi, western               	|
|   NOTE  	| Misc. Writing Notes 	| Literally anything                        |

See [this resource](https://justpaste.it/9ofj1) on words to use with author's
notes and their effects to get a good idea on words to use with the keywords
(while the page says it's outdated, my testing finds it still works pretty
well for AI Dungeon).

Each keyword, and the words for them, goes on its own line starting with `>`
and ending with a period.    

For example, let's say we want author's note for a story with the following
criteria:
- Fantasy world
- Romance story
- Potential for good NSFW content
- AI tries to be realistic and creative for the general fantasy content

Some good author's notes in NaCL format would be:
```
>RTNG R-RATED.
>GENR romance.
>STYL verbose, talkative, romantic, arousing, inventive, realistic.
>THEM fantasy.
```

An explanation of each line:
- `>RTNG R-RATED.`
	- This sets the rating to an adult rating that shouldn't shy away from
	  NSFW content, if that's where the story goes. Could also use `X-RATED`
	  for a higher amount of NSFW.
- `>GENR romance.`
	- This is fairly straightforward, as the genre is romance.
- `>STYL verbose, talkative, romantic, arousing, inventive, realistic.`
	- This is a bit more complex:
		- "Verbose" hints for the AI to be more descriptive with more dialogue.
		- "Talkative" hints to add more dialogue, focusing on smalltalk for
		  a better "slice of life" feel to the slower portions of the story.
		- "Romantic" is straightforward, matching the genre.
		- "Arousing" is a good hint for general NSFW content, as it hints for
		  the AI to slow down during NSFW scenes and be more descriptive.
		  Normally it's like the AI might try to speedrun NSFW scenes, so this
		  helps keep their pace in line with the rest of the story.
		- "Inventive" is a good hint for stories that can have action; as the
		  word says, the AI will try to be more inventive and creative with
		  how things play out. Instead of a dodge, a character may pull a
		  dextrous maneuver using their skills.
		- "Realistic" hints to keep character actions, well, realistic and a
		  little more intelligent. Instead of rushing into a fight, the AI
		  may have them sneak around to gain advantage.
- `>THEM fantasy.`
	- Straightforward; this signals that the story has a fantasy theme to it.

&nbsp;

As a note, the example author's notes clocks in at a relatively small 112
characters but conveys all of the information above to the AI in a generally
effective manner.

&nbsp;

### API Variables

If you use my AI Dungeon Scripting API, SALT, then there's an additional
bit of NaCL you can use to feed the script a list of your scenario variables.

Simply add a line starting with a `!!!`, with a list of the variables you want
to give to your script separated by `::`.
```
!!!${Your name?}::${Your gender?}::${Your age?}
```

You can put this anywhere in your memory, as long as it's on its own line.
If you check out the SALT API, there's ways to edit what comes before the
variables, if you want a different format for your own uses.

&nbsp;

### Full NaCL Example

If the individual examples don't give you a good grasp on how all of the NaCL
format comes together, here's a full bit of context for a scenario:
```
[You]
> You are named Jim.
> You are male.
> You are cool.
>| You have lots of friends.
> You have not watched die hard.

[Mark]
> Mark is male.
> Mark is your friend.

||Plot||
> You learned some Spanish.
> You learned some Italian.
> You are craving a Doritos Locos Taco.
```

Author's Notes:
```
>GENR thriller, adventure.
>STYL descriptive, creative.
```

&nbsp;

As you can see, the context is nice and organized, and you can get a decent
grasp of how the story for the scenario should work without even having to
read this hypothetical taco-ordering adventure.