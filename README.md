<p align="center">
	<img src="./hcl.jpg"/>
</p>

# Haunto Context Language

 My personal format for AI Dungeon scenario context,
 Haunto Context Language — aka HCL — is something I developed to make
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

Overall, HCL should strike a good balance between being usable by both players
and AI.  

### Historical Notes

In the ancient AI Dungeon year of 2020, a lot of research was done on various
methods of providing context and information to the AI, with multiple formats
invented and tested.  
AI Dungeon veterans may remember formats designed to optimize world info, 
such as Zalty's or Caveman.

While the passage of time — changing the AI models and how AI Dungeon works —
throws things more up in the air on the specifics of this old research, the
design of HCL mirrors some general strategies used.

While looking different, on tokenization by the AI, HCL should actually resemble
a sort of less condensed mixture of these older formats.

### Story Info

For using a subset of HCL for story info, see
[the page on it](https://github.com/Haunt-o/Haunto-Context-Language/blob/main/STORY_INFO.md)  
Since you have to cram story info into 1000 character to describe the details
and features of something, HCL is written differently for story info.

## Table of Contents
- [Headers](#headers)
- [Info Markers](#info-markers)
- [Good Practices](#good-practices)
- [Author's Notes](#authors-notes)
- [API Variables](#api-variables)
- [Full HCL Example](#full-hcl-example)

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

You can just use headers and regular info markers, and have 90% of the benefits
of HCL, but there are a couple extra varieties of info markers to help
organize your context information and give hints on how the information
should be treated to yourself and players of your scenario.

- **Additional Info Marker**
	- When you have additional context info you want to put in addition to what
	  is set in the base scenario info, you add additional `>`s, like `>>`.  
	  For example:
		```
		> You work at the local coffee shop.
		>> You have adopted a dog.
		```
	- You can add more `>`s, and each additional one implies a lower time
	  frame for the information.
	  	- `>` means base, unchanging information about something.
		- `>>` means plot or post-intro information.
		- `>>>` means temporary information.
		- `>>>>` means short-term, soon-to-change information.

- **Nested Info Marker**
	- When you have context info that is related to another piece of info — a
	  sort of "sub-context" info — you add a `|` to mark info as being part
	  of the info above it.  
	  For example:
		```
		> You are cool.
		>| You wear cool shades.
		```
	- If you have nested info on your nested info, just put another `|` at the
	  end, like `>||`.  
	  For example:
	  ```
	  > You are cool.
	  >| You wear cool shades.
	  >|| Your cool shades are made of coolium.
	  ```

- **Conditional Info Marker**
	- If you want to mark a piece of context info as optional or potentially
	  removable depending on some condition, you can put a `?` like `>?`.  
	  For example:
	  ```
	  > You are cool.
	  >? You don't have any friends.
	  ```

&nbsp;

You can combine different kinds of markers as needed; if you need to, you can
make a conditional nested info marker like:
```
> You are cool.
>|? People don't recognize how cool you are.
```

### Good Practices

As an AI can always make mistakes, there are some practices for writing context
notes that I, and others, find work and may make things easier for the AI:

1) Keep each piece of information as direct and straightforward as possible.
	- When an AI is fed too much text at once, its ability to keep track of
	  everything at the same time starts to weaken.  
	  While your context probably won't choke up the AI, it's good to be safe
	  instead of sorry.  
	  To help the AI, keep each individual note free of details or writing that
	  is unnecessary. If you need to expand on an idea, use nested info markers
	  and add additional notes expanding on the main one for a concept.
	
		- If it works for you, a "Caveman" strategy could work, where you just 
		  completely remove all uneeded words.  
		  For example, here's a normal piece of HCL:
		  ```
		  > Jim has green eyes and long hair.
		  ```
		  Here's the "Caveman" version:
		  ```
		  > Jim green eye. Long hair.
		  ```
		  I wouldn't recommend this unless you have a large amount of
		  context and are worried about it being long enough for the AI to
		  cut off or get confused about —
		  part of the goal of HCL is to be a combination of effective for
		  scenario creators and readable by scenario players.
	
	- Your context should not be just your intro story copy and pasted.  
	  There's literally no point for that; AI Dungeon will just use your intro
	  as context if you don't have your own.

	  Instead, your context should be a list of notes and details about the
	  fundamental points of the story for your scenario. The intro story is the
	  body, and the context should be a skeleton that allows the AI to keep
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

### Author's Notes

While not literally part of the context box, author's notes are lumped in
with the context when passed to the AI, so there's also a format for them.

Author's notes are surrounded with double brackets like:
```
[[ Write a cool story. ]]
```
&nbsp;

### API Variables

If you use my AI Dungeon Scenario Scripting API, then there's an additional
bit of HCL you can use to feed the script a list of your scenario variables.

Simply start a line with `!!!` and put all your variables separated by `::`:
```
!!!${Your name?}::${Your gender?}::${Your age?}
```

You can put this anywhere in your context, as long as it's on its own line.

&nbsp;

### Full HCL Example

If the individual examples don't give you a good grasp on how all of the HCL
format comes together, here's a full bit of context for a scenario:
```
[You]
> You are named Jim.
> You are male.
> You are cool.
>| You have lots of friends.
>? You have not watched Die Hard.

[Mark]
> Mark is male.
> Mark is your friend.
>? Mark is with you at the combination Pizza Hut and Taco Bell.

||Setting||
>>? You are in a combination Pizza Hut and Taco Bell.

||Plot||
>> You learned some Spanish.
>> You learned some Italian.
>>> You have ordered a Doritos Locos Taco.
```

Author's Notes:
```
[[ Write a gripping thriller story about a man waiting for his order. ]]
[[ Be descriptive about the interior of a combination Pizza Hut and Taco Bell. ]]
```

&nbsp;

As you can see, the context is nice and organized, and you can get a decent
grasp of how the story for the scenario should work without even having to
read this hypothetical taco-ordering adventure.