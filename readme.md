# ScottishBiomes

This is a Minecraft mod I developed for my own use. It is here as a reference and I offer no support for it.

## Why are you releasing it?

A while back I added a bunch of terrain generation hooks to Minecraft Forge, completely opening up Minecraft vanilla terrain generation to modders. I expected many people to begin using the hooks I added to Forge to do all kinds of things with terrain generation. That hasn't happened, and I started thinking that, with a few notable exceptions, terrain generation in minecraft remains relatively untouched.

## What licenses govern this code?

### Textures
With the exception of the mod logo, which is the [Killen tartan](http://www.tartanregister.gov.uk/tartanDetails.aspx?ref=1971) designed by Paul Killen (no relation) the mods textures come from the [ExtrabiomesXL mod](https://github.com/ExtrabiomesXL/ExtrabiomesXL) and are governed by the [Creative Commons Attribution-ShareAlike 3.0 Unported License](http://creativecommons.org/licenses/by-sa/3.0/deed.en_US).

### Code
With the exception of Acacia, Poplar and Redwood trees, all of this code is my own. I have donated this work to the public domain. Acacia and Redwood trees follow Nightshade_Proleath's alogorithm for the same trees in his [Forgotten Nature mod](http://www.minecraftforum.net/topic/1519278-) and the code is derived from his work. Poplar trees are direct copies from Nightshade_Proleath's work, and the code is his with changes to match my programming style. Nightshade_Proleath has given me carte blanche with regard to his code [here](http://www.minecraftforum.net/topic/1519278-) and he knew that I would release it in a publicly open way, but I cannot dictate the terms by which you use his code. Developers should refrain from using the tree gens without Nightshade_Proleath's permission.

## Points of Interest

### Standard Biome Generation
Redwood forests, savannas and shrublands are all generated by registering the biomes and adding them to the list of biomes available for world generation. This is exactly the same technique used by the ExtrabiomesXL mod.

### Intelligent Biome Insertion
A Forge terrain gen event (`WorldTypeEvent.InitBiomeGens`) is caught to insert GenLayers into the heart of vanilla terrain generation. GenLayers are used by Minecraft's terrain generation to create organic looking terrain.

Reference `TerrainGenInjector` to see how GenLayers are inserted. Specifically, the `GroveGenerator` replaces bits of Forest with birch groves and Autumn tree groves, and the `WastelandGenerator` inserts small bits of wasteland into terrain generation--but only in specific places chosen by the mod.

While writing this, I noticed that `HillGenerator` remains unused. I'll leave it as an exercise for the reader to implement this in the proper place and explore what it does.

### Mini Groves
Traditionally, features are added to terrain gen by implementing `IWorldGenerator` and passing the object to FML via `GameRegistry.registerWorldGenerator()`. FML then calls its collection of these objects _**after vanilla decoration finishes for a chunk**_. This means that `IWorldGenerator`s must deal with vanilla trees and other features. If you have ever seen one of Eloraam's [RedPower 2](http://www.eloraam.com/) volcanoes suspended in the top of a redwood tree from ExtrabiomesXL, you have seen the one of the side effects of this technique.

By hooking forge's `DecorateBiomeEvent`, modders can decorate chunks with surgical precision before, during and after vanilla chunk decoration. This mod demonstrates this by hooking (see `TerrainGenEventHandler`) a Decorate event to create small Poplar groves in Plains after vanilla trees have been placed, but before anything else is placed. All other vanilla flora (pumpkins, flowers, grass, mushrooms, shrubs) and small lakes will be filled in around the poplars, making them appear more "natural" to the environment and making the generation code much simpler. Modders can use `DecorateBiomeEvent` to insert their own custom features into chunks during early generation or even hook these events to turn off specific vanilla terrain gen features.

# Conclusion
The realm of modded Minecraft terrain generation remains relatively unexplored, despite the huge success of mods like ExtrabiomesXL. I encourage developers to use Eclipse's excellent search features to explore the minecraft code to see how Forge's terrain gen events (see `net.minecraftforge.event.terraingen`) can be used to realize the goals for their mods.

* * *
Except where note above:

<p xmlns:dct="http://purl.org/dc/terms/" xmlns:vcard="http://www.w3.org/2001/vcard-rdf/3.0#">
  <a rel="license"
     href="http://creativecommons.org/publicdomain/zero/1.0/">
    <img src="http://i.creativecommons.org/p/zero/1.0/88x31.png" style="border-style: none;" alt="CC0" />
  </a>
  <br />
  To the extent possible under law,
  <a rel="dct:publisher"
     href="https://github.com/scottkillen">
    <span property="dct:title">Scott Killen</span></a>
  has waived all copyright and related or neighboring rights to
  <span property="dct:title">ScottishBiomes</span>.
This work is published from:
<span property="vcard:Country" datatype="dct:ISO3166"
      content="US" about="https://github.com/scottkillen">
  United States</span>.
</p>
