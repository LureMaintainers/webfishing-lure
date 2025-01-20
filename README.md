<p align="center">
  <img src="https://raw.githubusercontent.com/Sulayre/WebfishingLure/refs/heads/main/icon.png" alt="Lure Shrimp"/>
</p>

## 4.3.1
- Bug where heads of species were white is fixed. ( yeeter )

## 4.3.0
- Update Lure's supported game version.
- Fixed issues with SteamNetwork.
- Changed the Map Selector position to be in the Create Lobby section.
- Added a Status to the Main Menu to indicate Lure Running.

## 4.2.2
- Modded map spawn positions now work properly (kitty-girl on GitHub)
- Void Portals no longer crash modded maps that don't contain a spawn position for them (kitty-girl on GitHub)
- Added a new C# patch that turns the const shops list in shop.gd into a var instead of a const, allowing adding custom shops to it externally without patches.
- Modded maps can now be hidden from the map list with a "hidden" value in the maps dictionary, can be set both when calling add_map or by editing it directly by accessing the Lure node.

## 4.2.1
- Lure has been updated for WEBFISHING 1.10
- Added a new popup window that lets you know if Lure is not updated for the current game version
- Joining friends who are playing cutom maps through Steam is still bugged. (yeah)

## 4.2.0
- added a new vanilla filterer to the base game's save data loader as a second layer of security for crash prevention and save corruption
- now the save splitter actually works, sorry.
- corrupted saves (vanilla saves with mod references) should fix themselves on load now, please lmk if it works!

## 4.1.0
- lure shouldn't crash anymore and all modded content should work as usual
- modded map lobbies may be buggy?
- max player tweak discontinued
- lure lobby filters discontinued **(no more dedicated lobbies filter sorry)**
- Emote wheel is fixed on widescreen
- steam joins are still not fixed so don't use modded maps with the steam-friends lobby setting please
- lure modded props/actors no longer rely on a C# patch
- C# patch changes

## 4.0.0
- Lure now has proper save data splitting for modded things! your vanilla save data will remain intact if you uninstall mods.
- Lure's vanilla items and cosmetics list is now a const to avoid human error
- Lure now prevents resource loading when a new version of the game comes out to avoid save corruption (experimental)
- Lure now supports custom emotes! check the GitHub documentation.
- Lobby settings improvements
- The game now loads lure assets like 90% faster because I got rid of all the prints that are not critical errors.

## Features
### Lure allows you to...
- Add your own fish, props, bobbers, colors, titles, eyes, mouths and noses!
- Add custom shirts/undershirts, hats and accessories with alternative meshes for any vanilla or modded species!
- Add new species with unique voices with modded and vanilla pattern compatibility!
- Add custom patterns for Vanilla AND anyone’s modded species!
- Add new items that can have any function from any node linked to it!
- Add custom emotes!
- Make whole new maps without having to replace things from the base game!
- Change the lobby's max players up to 250 (or reducing it down to 2)!
- Filter lobby search to easily find modded lobbies or dedicated servers!

### ...it also makes modding easier and less annoying with the following tweaks :)
- Updates the character colors shader so now patterns can have additional static colors on their textures.
- Items/Cosmetics loaded with lure have a unique prefix related to the mod's folder they were loaded from, allowing multiple mods to have same the same item/cosmetic file names.
- Streamlines the process of finding your mod's assets by using Lure's unique prefixes when referencing assets inside the mod's folder.
- Saves modded items and cosmetic data on a separate file so the vanilla content doesn't get corrupted when uninstalling!

## Known Issues
- Joining friends through Steam causes map desync.

## Requirements
- [GDWeave](https://github.com/NotNite/GDWeave/tree/main)

## How to Install
- drag the folder inside the release's zip into ```<game install folder>\GDWeave\mods```

## Development
if your mod depends on Lure in any way, make sure to add ``"TeamLure.Lure"`` to the ``"Dependencies"`` array of your mod's manifest.json like this:

``dependencies: ["Sulayre.Lure"]``

To access Lure's functions in your code, add the following line at the start of ``main.gd``:

``onready var Lure = get_node("/root/SulayreLure")``

*(this way you can access all of it's functions listed below in the Documentation)*

# Documentation  
- [Loading Assets](https://github.com/Sulayre/WebfishingLure/blob/main/README.md#loading-assets-with-lure)
- [Ready Functions](https://github.com/Sulayre/WebfishingLure/blob/main/README.md#initialization-functions-make-sure-you-call-these-functions-on-the-_ready-function-of-your-modgd)
  - [Custom Species Voices and Faces](https://github.com/Sulayre/WebfishingLure/blob/main/README.md#custom-species)
  - [Clothing Meshes and Pattern Textures](https://github.com/Sulayre/WebfishingLure/blob/main/README.md#cosmetics)
  - [Setting Up Props](https://github.com/Sulayre/WebfishingLure/blob/main/README.md#props)
  - [Item Behavior](https://github.com/Sulayre/WebfishingLure/blob/main/README.md#items)
  - [Adding Maps](https://github.com/Sulayre/WebfishingLure/blob/main/README.md#maps)
  - [Adding Items, Props and Cosmetics/Species](https://github.com/Sulayre/WebfishingLure/blob/main/README.md#loading-the-items-props-and-cosmetics)
- [Utility Functions](https://github.com/Sulayre/WebfishingLure/blob/main/README.md#utility-functions)

## Loading assets with Lure
Lure allows you to load asset paths with 3 different prefixes:
- ``mod://``  searches for assets starting from the folder of the mod_id you gave to the function.
- ``res://`` searches for assets the classic way, in case you wanna search for base game assets.
- ``mods/<mod_id>://`` <u>searches for assets inside a specific mod</u>'s folder

 <u>if you use</u> ``mods/<mod_id>://`` <u>make sure you add the mod you're searching in as a dependency inside your mod's ``manifest.json`` to make sure its already loaded when searching inside it's files.</u>

**Examples**
*(we gave the function we're calling ``example_mod`` as the mod_id argument)*
- ``mod://asset.file``would search for ``asset.file`` inside ``example_mod``'s folder.
- ``res://Assets/asset.file`` would search for ``asset.file`` inside the base game's Assets folder.
- ``mods/other_mod://asset.file``would search for ``asset.file`` inside the folder of a mod called ``other_mod``.
## Initialization Functions <br><sub><sup>Make sure you call these functions on the ``_ready()`` function of your ``mod.gd``!</sup></sub>

### Custom Species

**Lure.assign_species_voice(``your_mod_id``, ``species_id``,  ``bark_sound_path``, ``growl_sound_path``, ``whine_sound_path``)**<br>Allows you to assign bark, growl and whine sounds to a specific species, growl and whine are optional, if any of the two are missing they'll get assigned the bark sound.

**Lure.assign_face_animation(``your_mod_id``, ``species_id``, ``animation_path``)**<br>Allows you to assign a face offsets animation to a modded species, so you can adjust the eye, mouth and nose positions. You can make and edit offset animations by selecting the ![](https://cdn.discordapp.com/attachments/1297612591656341504/1298879143223492638/image.png?ex=671b2af7&is=6719d977&hm=699e55dfbba034a17cd1a99603bfa7ede9e09724e0020b5fb839ee36b70f04cd&) node inside the ``player_face.tscn`` scene that you can find in the ``res://Scenes/Entities/Player/Face`` folder when opening the decompiled game with the godot editor, then select either the ``cat_face`` or the ``dog_face`` animation in the animation timeline tab, click the ![](https://cdn.discordapp.com/attachments/1297612591656341504/1298877917744467968/image.png?ex=671b29d3&is=6719d853&hm=36fb587d5024508e25ee7379c4866228d78b3cee91932936c788e3aa6b98cd7b&) button and select 'Duplicate', name your new animation and save with "OK". After you're done editing the offsets, click ![](https://cdn.discordapp.com/attachments/1297612591656341504/1298877917744467968/image.png?ex=671b29d3&is=6719d853&hm=36fb587d5024508e25ee7379c4866228d78b3cee91932936c788e3aa6b98cd7b&) again and select "Save As", this is the file you'll need to load with the function.

### Cosmetics

**Lure.assign_pattern_texture(``your_mod_id``, ``pattern_id``, ``species_id``,  ``texture_path``)**<br>Allows you to assign a texture to a specific pattern for a specific species, this allows you to add base game pattern support to modded species, add vanilla species support for modded species or add other people's modded species support to your modded patterns.

**Lure.assign_cosmetic_mesh(``your_mod_id``, ``cosmetic_id``, ``species_id``, ``mesh_path``)**<br>Allows you to assign an alternative mesh to a specific cosmetic for a specific species, for example if you make a mask accessory, you'll need to make an alternative version for the dog, so its not clipping through the head, this is optional to make your cosmetics work but its heavily encouraged, this function works for both vanilla and modded species/cosmetics.

### Props

**Lure.register_prop(``id_of_the_mod_that_added_the_prop``,``scene_id``,``tscn_path``)**<br>Allows you to assign a specific scene to a modded prop, Lure will automatically turn the ``prop_code`` from your modded prop to ``mod_id.prop_code`` so if the prop you're assigning the scene to is not from your mod, make sure you call this function with the id of the mod that adds the prop.

### Items

**Lure.register_action(``your_mod_id``,``action_id``,``node_that_holds_the_function``,``name_of_the_function_we_are_calling``)**<br>Allows you to register an action for any modded item to use, you'll have to give it the node that holds the function you're calling (for example, if you call this function on your ``main.gd`` file and the function you want to link is in it as well you can just write ``self`` as the third argument) and the name of the function the node has that we're gonna call through the action.

the way you would call the custom action is by setting the action or release_action variables of your modded item's resource file as ``mod_id.action_id`` like everything else with Lure.

### Maps

**Lure.add_map(``mod_id``,``map_id``,``scene_path``,``optional_map_name``)**<br>Adds a new map to the game, if the map is not missing it will get added to the map list of the 'create' section of the lobby list, no tutorials on how to make maps as of yet but here's a couple of steps of how to setup your new map without crashing the game in the testing process:

1) Open ``main_map.tscn`` and then duplicate it using ``Scene > Save Scene As..``, name it however you want.

2) Right click the ``main_map`` node of the scene and click ``Attach Script``, then in the ``Inherits`` field write ``"res://Scenes/Map/main_map.gd"`` (with the quotes)

3) Rename the ``main_map`` node to whatever you want, this is optional.

4) Delete the ``main_zone`` node, we're gonna make our own.

5) Open ``main_zone.tscn`` and then duplicate it using ``Scene > Save Scene As..``

6) Rename ``lake_zone`` to ``main_zone``

7) Feel free to remove all the nodes inside the new zone you're making, just keep in mind the following are important:
- ``points`` (Removing it WILL crash the game)
- ``NPCs`` contains well, the npcs.
- ``lake_water`` and ``ocean_water`` are important, but keep in mind that ``fish_zones`` contains the areas that actually decide the 'loot pool' of fish you'll get when fishing inside the area!!
- ``portals`` are basically the teleporters/doors
- ``bounce_shrooms``contains the jumping mushrooms
- ``chalk_zones`` contains the areas you can draw on, don't scale these because it will break the position of the chalk when drawing, no idea.
- ``sonud_ambient_zones`` yup, you read that right, it has a typo. basically the background noise.
- ``invis_walls`` is where your invisible barriers will set up

8) Go back to your new map's scene, add the new zone you created inside the ``zones`` node, remember, it **must** be called ``main_zone`` else it will crash.

9) Save your map and load it with lure using the ``add_map`` function Lure provides.

10) Enjoy!

*NOTE: Avoid using OBJ files on your custom map, they are SUPER buggy, please use GLB/GLTF instead !!*

### Loading the Items, Props and Cosmetics...

**Lure.add_content(``your_mod_id``,``resource_id``,``item_or_cosmetic_path``,``flags``)**<br>Loads a new cosmetic/item into the game, the final identifier of the cosmetic/item will be ``your_mod_id.the_resource_id``, this is to avoid mods cosmetics/items overriding other mods' so keep this in mind when using function that require a resource's identifier.  <u>Make sure you run this function last if you need to use any of the previously mentioned assign functions!</u>

*for example, if your mod's id is ``awesome`` and you're adding an item that's called ``sauce`` the final id of the item will be ``awesome.sauce``, you only have to do this for modded resources since base game resources use their file name without the ``.tres`` extension.*

``flags`` is an optional array argument that Lure uses to toggle certain functionality on your new content, here's a list of flags and their uses:

*the following 2 flags will add your new cosmetic/tool/prop to your inventory automatically, but with different conditions*
- ``FREE_UNLOCK`` will make the cosmetic/item/prop remain unlocked forever.
- ``LOCK_AFTER_SHOP_UPDATE``  will make the cosmetic/item/prop remain unlocked until the modded item shop integration update is out, it will lock them after that update drops and the flag will become obsolete.

*The following flags are not implemented yet, but you can add them if you want your content to be future proof:*
- ``SHOP_POSSUM`` adds the cosmetic or item to the possum's shop.
- ``SHOP_FROG`` adds the cosmetic or item to the frog's shop.
- ``SHOP_BEACH`` adds the cosmetic or item to the shop at the beach.

*The following flags are not implemented yet, don't add them to your add_content arguments since its missing from the latest release.*
- ``VENDING_MACHINE`` adds the cosmetic or item to the vending machine.

keep in mind that to access the flags you need to reference them inside Lure's ``FLAGS`` enum, so a real example would look like this:
``[Lure.LURE_FLAGS.SHOP_POSSUM, Lure.LURE_FLAGS.FREE_UNLOCK]``

## Utility Functions

**Lure.get_other_mod_asset_path(``path``)**<br>gives you the absolute ``res://`` path of another mod's asset, you're probably not gonna use this but i wanted to add it just in case, make sure the given path uses the ``mods/<mod_id>://`` prefix mentioned above.
