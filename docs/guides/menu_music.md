Epiphany's addition of the tarnished characters to the main menu comes with a brand new soundtrack to match the vanilla theme. Under the circumstance that you have a soundtrack mod and make a tarnished version of the menu theme to be compatible with Epiphany, this guide will briefly go over updating the menu music yourself.

## music.xml

As Lua code is involved, you will need to define a new theme inside the music.xml file, located in your mod's content folder. For reference, here's Epiphany's entry for replacing the main menu music:

```xml
<music root="music/">
	<track name="Genesis Tarnished" intro="Repentance/Genesis Retake Light Intro.ogg" path="Repentance/Genesis Retake Light Loop.ogg" loop="true" layermode="2" layerfadespeed="0.01" mul="0.8">
		<layer intro="Repentance/Genesis Retake Twisted Intro.ogg" path="Repentance/Genesis Retake Twisted Loop.ogg" mul="0.8" />
		<layer intro="epiphany/genesis_tarnished_intro.ogg" path="epiphany/genesis_tarnished_loop.ogg" mul="0.9" />
	</track>
</music>
```

The music track must use `layermode=2` and have a total of two layers; one for tainted, second for tarnished. Everything else is free to be adjusted as you see fit.

## Menu callbacks

There are two callbacks for replacing menu music: One for the actual music, and another for the jingle you hear after starting/continuing a run. It's straightforward to use, just return your new music ID and it'll become the new music/jingle to play:

```Lua
local Mod = RegisterMod("Epic Music Mod", 1)
local NEW_EPIPH_MUSIC = Isaac.GetMusicIdByName("My New Music")
local NEW_EPIPH_JINGLE = Isaac.GetMusicIdByName("My New Music Jingle")

--Required to use this callback in order to load before the menu loads in
Mod:AddCallback(ModCallbacks.MC_POST_MODS_LOADED, function ()
	--Checks the Epiphany global and that its Wave 8+, as these callbacks don't exist before Wave 8
	if Epiphany and tonumber(Epiphany.WAVE_NUMBER) >= 8 then
		Epiphany:AddExtraCallback(Epiphany.ExtraCallbacks.PRE_PLAY_MENU_MUSIC, function()
			return NEW_EPIPH_MUSIC
		end)
		Epiphany:AddExtraCallback(Epiphany.ExtraCallbacks.PRE_PLAY_MENU_MUSIC_JINGLE, function()
			return NEW_EPIPH_JINGLE
		end)
	end
end)
```