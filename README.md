# Text Display Experiments
## Introduction
This plugin showcases unusual ways to use Minecraft's text display entities.

It was made as part of a video: https://youtu.be/uZmEYYs0ZKs

This plugin is very experimental and untested in multiplayer. Use at your own risk.



## Installation
1. Download the JAR from the [releases page](https://github.com/TheCymaera/minecraft-text-display-experiments/releases).
2. Set up a [Paper](https://papermc.io/downloads) or [Spigot](https://getbukkit.org/download/spigot) server. (Instructions below)
3. Add the JAR to the `plugins` folder.
<!--4. Download the world folder from [Planet Minecraft](https://www.planetminecraft.com/project/spider-garden/).-->
<!--5. Place the world folder in the server directory. Name it `world`.-->

## Running a Server
1. Download a server JAR from [Paper](https://papermc.io/downloads) or [Spigot](https://getbukkit.org/download/spigot).
2. Run the following command `java -Xmx1024M -Xms1024M -jar server.jar nogui`.
3. I typically use the Java runtime bundled with my Minecraft installation so as to avoid version conflicts.
   - In Modrinth, you can find the Java runtime location inside the profile options menu.
4. Accept the EULA by changing `eula=false` to `eula=true` in the `eula.txt` file.
5. Join the server with `localhost` as the IP address.


## Commands
Autocomplete will show available options.

Get control items:
```
/items
```

Bitmap display:
```
# Create directly
execute unless entity @e[tag=bitmap_display] run summon minecraft:marker ~ ~1.25 ~ {Tags:["pre_bitmap_display"],Rotation:[90f,0f]}

# Toggle with sound effects
execute unless entity @e[tag=bitmap_display] run summon minecraft:marker ~ ~1.25 ~ {Tags:["pre_bitmap_display"],Rotation:[90f,0f]}
kill @e[tag=bitmap_display]
tag @e[tag=pre_bitmap_display] add bitmap_display
execute if entity @e[tag=bitmap_display] run playsound minecraft:block.beacon.activate block @a ~ ~ ~ 1 1
execute unless entity @e[tag=bitmap_display] run playsound minecraft:block.beacon.deactivate block @a ~ ~ ~ 1 1
```

Paint app:
```
# Create
summon minecraft:marker ~ ~1 ~ {Tags:["paint_app.hue_picker", "paint_app"],Rotation:[45f,0f]}
summon minecraft:marker ~ ~1 ~-1 {Tags:["paint_app.sv_picker", "paint_app"],Rotation:[45f,0f]}
summon minecraft:marker ~ ~1 ~-1 {Tags:["paint_app.canvas", "paint_app"],Rotation:[0f,0f]}
execute as @e[tag=paint_app.hue_picker] at @s run tp @s ^-1.3 ^ ^

# Remove
kill @e[tag=paint_app]

# Set hue picker options
data modify entity @n[tag=paint_app.hue_picker] BukkitValues."paint_app:items" set value 120
data modify entity @n[tag=paint_app.hue_picker] BukkitValues."paint_app:width" set value .2f
data modify entity @n[tag=paint_app.hue_picker] BukkitValues."paint_app:items" set value 2f

# Set sv picker options
data modify entity @n[tag=paint_app.sv_picker] BukkitValues."paint_app:items" set value 50
data modify entity @n[tag=paint_app.sv_picker] BukkitValues."paint_app:width" set value 2f
data modify entity @n[tag=paint_app.sv_picker] BukkitValues."paint_app:height" set value 2f

# Set canvas options
# (Changing the bitmap size will clear the canvas)
data modify entity @n[tag=paint_app.canvas] BukkitValues."paint_app:bitmap_width" set value16
data modify entity @n[tag=paint_app.canvas] BukkitValues."paint_app:bitmap_height" set value 16
data modify entity @n[tag=paint_app.canvas] BukkitValues."paint_app:display_height" set value 2f
```

Rainbow cycle animation:
```
summon minecraft:text_display ~1 ~1 ~ {text:'"Hello World"',Rotation:[0f,0f],brightness:{sky:15,block:15},interpolation_duration:3}
data modify entity @n[type=minecraft:text_display] background set value -65536
data modify entity @n[type=minecraft:text_display] text set value '" "'
data merge entity @n[type=minecraft:text_display] {start_interpolation:-1,transformation:{translation:[-.1f,-.5f,0f],scale:[8.0f,4.0f,1f]}}
tag @n[type=minecraft:text_display] add rainbow_cycle_animation
```

Pulsating animation:
```
summon minecraft:area_effect_cloud ~ ~.5 ~ {Passengers:[
    {id:"minecraft:text_display",Rotation:[0f,0f]  ,Tags:["pulsating_animation"],text:'" "',background:0,transformation:{translation:[-.1f,-.5f,.501f],scale:[8.0f,4.0f,1f],left_rotation:[0f,0f,0f,1f],right_rotation:[0f,0f,0f,1f]},brightness:{sky:15,block:15}},
    {id:"minecraft:text_display",Rotation:[90f,0f] ,Tags:["pulsating_animation"],text:'" "',background:0,transformation:{translation:[-.1f,-.5f,.501f],scale:[8.0f,4.0f,1f],left_rotation:[0f,0f,0f,1f],right_rotation:[0f,0f,0f,1f]},brightness:{sky:15,block:15}},
    {id:"minecraft:text_display",Rotation:[180f,0f],Tags:["pulsating_animation"],text:'" "',background:0,transformation:{translation:[-.1f,-.5f,.501f],scale:[8.0f,4.0f,1f],left_rotation:[0f,0f,0f,1f],right_rotation:[0f,0f,0f,1f]},brightness:{sky:15,block:15}},
    {id:"minecraft:text_display",Rotation:[270f,0f],Tags:["pulsating_animation"],text:'" "',background:0,transformation:{translation:[-.1f,-.5f,.501f],scale:[8.0f,4.0f,1f],left_rotation:[0f,0f,0f,1f],right_rotation:[0f,0f,0f,1f]},brightness:{sky:15,block:15}},
    {id:"minecraft:text_display",Rotation:[0f,-90f],Tags:["pulsating_animation"],text:'" "',background:0,transformation:{translation:[-.1f,-.5f,.501f],scale:[8.0f,4.0f,1f],left_rotation:[0f,0f,0f,1f],right_rotation:[0f,0f,0f,1f]},brightness:{sky:15,block:15}}
]}
```

Screen overlay:
```

```

Colored liquid:
```
execute align xyz run summon minecraft:interaction ~.5 ~.0 ~.5 {Tags:["colored_cauldron"],response:1b,width:1.01,height:1.005}
```

Background color utilities:
```
# Change the background color of a text display
data modify entity @n[type=minecraft:text_display] BukkitValues."text_utilities:background" set value "55FF0000"

# Change the background color of a text display with lerping
data modify entity @n[type=minecraft:text_display] BukkitValues merge value {"text_utilities:background_lerp_speed": .05, "text_utilities:background": "55FF0000"}
```

Flame particles (Run on repeat):
```
# Orange
summon minecraft:area_effect_cloud ~ ~1 ~ {BukkitValues:{"flame_particles:palette":"orange"}}

# Blue to orange
summon minecraft:area_effect_cloud ~ ~1 ~ {BukkitValues:{"flame_particles:palette":"blue_to_orange"}}

# Black
summon minecraft:area_effect_cloud ~ ~1 ~ {BukkitValues:{"flame_particles:palette":"black"}}
```

Water splash particles (Run on repeat):
```
# Frog Fountain
summon minecraft:area_effect_cloud ~ ~2.375 ~ {BukkitValues:{"water_splash_particles:amount":7,"water_splash_particles:min_size": 0.09, "water_splash_particles:min_speed": .15, "water_splash_particles:up_angle_bias": 3}} 

# Waterfall
summon minecraft:area_effect_cloud ~ ~2.375 ~ {BukkitValues:{"water_splash_particles:amount":10, "water_splash_particles:min_speed": .22}}
```

Firefly particles (Run on repeat):
```
summon minecraft:area_effect_cloud ~ ~1 ~ {BukkitValues:{"firefly_particles:amount": 3}}
```


## Development
1. Clone or download the repo.
2. Run Maven `package` to build the plugin. The resulting JAR will be in the `target` folder.
3. For convenience, set up a symlink and add the link to the server `plugins` folder.
   - Windows: `mklink /D newFile.jar originalFile.jar`
   - Mac/Linux: `ln -s originalFile.jar newFile.jar `

## License
You may use the plugin and source code for both commercial or non-commercial purposes.

Attribution is appreciated but not due.

Do not resell without making substantial changes.
