## Introduction

Welcome to my dev log! Grab yourself a coffee, and let's dive in. Today, I'm going to show you the progress I've made on my currently unnamed dungeon-crawling card-battle game, set in a mysterious world of biomechanical tech and eldritch horrors. I'll walk you through my creative process and explain some key aspects like dungeon generation and rendering.

## Current State

(Roll video of the prototype) 
My game is made using a combination of Three.js for 3D rendering and Svelte for the UI, which means it runs in a browser. 
(Show ancient screenshots of Netscape and early versions of IE) 
You can walk around this labyrinth, which I design using plain JSON. 
(Show a screenshot of the JSON file with labyrinth layout) 
Currently, you can only walk and admire Kenney's 3D models, which I use as placeholders.
(Show exact model pack on kenney.nl) 
I've also used some AI-generated placeholders for the cards. 
(Show close-up of cards)

## Game Design References

Right now, I'm designing the proper UI for this game. My primary source of inspiration is old dungeon crawlers. 
(Show a fast reel of old dungeon crawler videos/screenshots) 
As you can see, they all have common features: a viewport for exploration, directional buttons for walking, sometimes a minimap, and portraits of party members if there’s a party. My game won’t have a party. 
(Start showing AI-generated art) 
You'll be alone, stranded in an alien world filled with biomechanical tech. 
(Show images and videos of SCORN) 
Biomechanical settings have become quite popular recently, so I guess I’m jumping on the bandwagon. 
(Show images of Hibernaculum) 
One particularly cool game in a similar style is the upcoming Hibernaculum. I'm trying hard not to "borrow" ideas wholesale from it. 
(Start showing videos and images of Vangers) 
Another inspiration is a Russian game from the 90s called Vangers. It also takes place in a strange and unfamiliar world, but you drive cars in it. 
(Show Vangers UI) 
The interface is a chaotic mess. If you've never played the game before, you wouldn’t even know what half the things on the screen do. That’s the kind of feeling I’m trying to recreate.

## Setting and Mechanics

So, you're stranded in this alien and hostile world. How do you protect yourself? Using playing cards, of course! 
(Show Yu-Gi-Oh) 
And by extracting spinal fluid from your fallen enemies. Similar to some big trading card games, my system will have several main resource types that determine how you play:

- **Spinal Fluid**: You can refill it at dispensers or extract it from biological enemies. Abilities using Spinal Fluid either heal you or deal damage to enemies based on the amount you spend. High risk, high reward.
- **Plasma**: This is your traditional offensive deck. It deals big damage to single targets or crowd control for multiple enemies. You can refill your Plasma by finding pellets scattered throughout the level, making it a slightly more expensive resource.
- **Eldritch Essence** (working name): This offers unconventional effects, like wild magic in DnD, leading to semi-random results. It can also include utility powers, like teleporting to a random previously visited location or rewinding time to undo your last move.

I'm currently brainstorming visual ideas with AI and trying to recreate them in a 3D modeling package with mixed results.

## Demonstration of Prototype

What’s currently implemented includes dungeon generation, movement, and basic card battles. 
As I’ve shown, dungeons are laid out in a text format. 
I have a script that grabs 3D meshes according to a pre-determined dictionary and constructs a dungeon from them. 
Then, I create a navigation graph using a tree data structure. If the nodes are connected, you can walk between them. 
There's a short bump animation when you walk into a wall. Initially, it was bugged, so I had to limit the bump rate to prevent accidentally escaping the level. 
The cards are described in the same old JSON format. 
I use techniques from beginner Svelte tutorials to draw and animate the cards.

## Conclusion

That’s it for now. If you have any ideas for the title, post a comment or send me an email. If you need any particular techniques explained further, also post a comment, and I'll see what I can cover in the next video.