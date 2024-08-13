# VTOLVR-Blender-Terrain-Generation
A set of resources to help learn procedural terrain generation for VTOLVR with Blender.

## Introduction
This repo covers using the powerful tools and addons within Blender to procedurally generate a custom terrain to our liking, and then getting a heightmap from that terrain to import into VTOLVR. This is essentially just my personal notes roughly adapted into a tutorial format, so please excuse any verbal/didactic sloppiness.

## Before Proceeding - Review the References in the References Section

### Simple Generation Parameters for 196x196 and 6km height

**Max height reference mesh** - add a plane (default dimensions are 2m x 2m) and extrude it up by .061 - this is as if the plane was 196km x 196km and the height you extruded up was 6km (6,000m). This matches the default max VTOLVR parameters of 196x196km and 6km height. More details below under "Height Scaling - Visual Reference". You can easily slide this mesh to the side and then back over your terrain by pressing "G" and then "X" to quickly check if any terrain features intersect the max height.

**Add Base Plane**
This will serve as your ocean floor - don't move this.

**Add and scale ANT Landscape meshes as you wish (see references)** - Use ONLY "S" key and "G" with X or Y to keep the ANT landscapes on the ocean floor plane.

**Render** - 
Add an Orthographic camera
Render / Color Management MUST BE "STANDARD" - NOT FILMIC
Render as .PNG with size 1281x1281 (VTOLVR Max size)

**RGB Curves** - 
Adding the curve into the displacement material shader was necessary to counteract the default gamma adjustments that happen in a PNG photo. Before adding the curve, the whiteness increases too sharply coming out of the water, meaning that anywhere land meets sea there'll be a sharp cliff. Adding the RGB curve smoothes it out.

### Height Scaling - Visual Reference
The maximum terrain height in VTOLVR is 6,000 meters - based on this, you can create an object in blender that is scaled appropriately in order to better visualize the maximum height that you are generating.

For this example, we have a terrain plane that is 2x2 units in blender.
Our hypothetical map is the VTOLVR max of 196x196 km with a max height of 6,000m (or 6 km).

Solving this formula for x gives us the in-blender maximum height above the plane that our terrain can be :
6km / 196 km = x / 2 units

Which gives us 3/49 or 0.061 - so a height of 0.061 in blender represents the max height of the terrain that we can build. I set a plane at that height to ensure that none of the terrain crosses that altitude. 

If our max terrain height was to be lower than 6,000m - we wouldshift the reference plane down to the new max height and measure the new in-blender height, and then use this formula :

x meters / 6,000 meters = NewMaxHeight / (MaxHeight : in our case 0.061)

And then input x meters into the VTOLVR heightmap terrain generator when prompted.


## References :

### Required Prerequisite Knowledge
**Generating Terrain with TXA Ant - Improved verion of Blender's ANT Terrain Generator**
https://github.com/nerk987/txa_ant
https://youtu.be/bUGkdVrnrug

**Displacement Map Material guide** (Very useful, but you still need to export as PNG and adjust the color curve with an RGB curves node) - https://youtu.be/arvhK4tvYuY

Inspired by / with reference to - https://steamcommunity.com/sharedfiles/filedetails/?id=2841984565

Misc ANT tutorial - https://www.youtube.com/watch?v=y7bH2bXKyTk

##### Blender Subdivision to Resolution Conversion
Subdivision	: Resolution
1	2
2	4
3	8
4	16
5	32
6	64
7	128
8	256
9	512
10	1024


##### VTOLVR Terrain Height/Mountain Texturing
Describes how the VTOLVR engine generates the terrain texture based on the parameters below : By Bekende_Erik (VTOLVR Discord, Map Editor Channel) — 11/11/2022 6:50 PM

Height:
- -80 to 0: Water/Sandbanks 
- 0 to 20: Pure Sand 
- 20 to 40: Transition between sand and grass
- 40 to 1.000: Pure Grass
- 1.000 to 1.250: Transition between grass and snow (Rocky)
- 1.250 to 1.500: Transition between grass and snow (Snowy)
- 1.500 to 2.500: Rocky mountains (Cliffsides still rocky like at ground altitude)
- 2.500 to 4.000: Transition between rocky mountains and snowy mountains (barely noticeable)
- 4.000 to 6.000: Snowy mountains (Even the steep cliffs are now fully covered with snow)

Trees (Limited for now; it's hard to measure)
- 40 to 1750: Pretty large amount of trees 
- 1750 to 2250: Trees start decreasing 
- 2.250 to 3.500: Just below the timberline; tree-spawns are even fewer 
- 3.500 to 6.000: Trees don't spawn or very very rarely

Angle: 
- 0 to 20 degrees: Grass/Snow/Sand 
- 20 to 30 degrees: Transition between rock and grass/snow/sand 
- 30 to 90 degrees: Rockside 