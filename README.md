# Import Scripts for Importing CSGO maps into CS2
Modified variant of the CS2 import scripts to run with python3

How to use:

## Prerequisites ##
### Precompiled Content ###
Precompiled content (such as any custom vmt, vtf, mdl, etc.) must be located in the \steam\steamapps\common\Counter-Strike Global Offensive\csgo folder.  For example: 

\steam\steamapps\common\Counter-Strike Global Offensive\csgo\materials\
\steam\steamapps\common\Counter-Strike Global Offensive\csgo\models\

Notes: Any custom content archived in a .bsp will need to be extracted into the appropriate folders before importing. The import tool also requires a vmf, so if you only have a bsp for your map, you will need to decompile it to a .vmf. Finally, '''do not copy .vmf files to the above folder''' as it will confuse the import tool.


### Source Map Files ###
Your map files (.vmfs) can live anywhere outside of <nowiki>\Counter-Strike Global Offensive\</nowiki> as long as they are in a <nowiki>\maps\</nowiki> folder. Any prefab or instance vmfs referenced by your map must be in whatever subfolder structure is expected by the map. For example:

<nowiki>c:\mymapfolder\maps\mymap.vmf
c:\mymapfolder\maps\prefabs\mymapprefab.vmf
c:\mymapfolder\maps\instances\mymapinsstance.vmf</nowiki>


### Source Texture Files ###
If you have uncompressed source files for textures, (tga, psd, etc.) the import tool will try to use those when importing materials, otherwise it will fall back to converting and recompressing vtf files (which can decrease texture quality). For this to be successful, make sure your source files are in a mirrored location of the vtfs in <nowiki>\steam\steamapps\common\Counter-Strike Global Offensive\csgo\materials\</nowiki> For example:

<nowiki>c:\steam\steamapps\common\Counter-Strike Global Offensive\csgo\materials\mymaterial\mytexture.vtf
c:\mymapfolder\materials\mymaterial\mytexture.tga</nowiki>


## Getting Started ##
### 1. Installing Python 3 ###
To run the import script you will need to install Python 3
Easiest way to install is by opening powershell and typing "winget install python" and then following the steps through the installation process


### 2. Installing the Colorama Extension ###
Next you will need a Python extension called colorama. To install this, simply open up a command prompt (search for cmd in Windows Start Menu) and type

<nowiki>python -m pip install colorama</nowiki>


### 3. Creating a new Addon with the Workshop Tools ###
Before running an import, you will need to create a Workshop Tools addon so that the imported content has a valid destination. To make a new addon, simply launch the game with Workshop Tools, and from the UI click "Create New Addon" and take note of the name for later.



### 4. Locating the Import Script ###
First up is locating the python import script, which can be found wherever you checked out this git repository.

Once you have located the folder, you will want to open up the windows terminal or powershell. This can be done by simply right clicking in the folder and hitting "Open in Terminal" for Windows 11 or typing "powershell" in the windows explorer address bar (any version of windows)


### 5. Running the Script + Parameters ###
Now you are ready to run the script by entering the following:

<nowiki>python import_map_community.py <s1gameinfopath> <s1contentpath> <s2gameinfopath> <s2addon> <mapname> -usebsp</nowiki>


### Script Parameters ###

<br />
:'''<s1gameinfopath>'''
:Path to folder containing CSGO's gameinfo.txt, this MUST be the path that contains the compiled CSGO model and material content you want to import (e.g. .mdl, .vmt).
 
<br />
:'''<s1contentpath>'''
:Path to folder containing source content, (.vmf .psd .tga etc.). As mentioned above, for the importer to successfully find and use source texture files, make sure they are in a mirrored location to that of the .vtfs (see example above)

<br />
:'''<s2gameinfopath>'''
:Path to folder containing CS2's gameinfo.gi

<br />
:'''<s2addon>'''
:The name of CS2 Workshop addon that you created earlier. This is where your assets will be imported to.

<br />
:'''<mapname>'''
:This is the map name (.vmf) without extension, e.g. de_examplemap that you wish to import. If your map sits under a subdirectory of the <nowiki>/maps/</nowiki> folder in <nowiki><s1contentpath></nowiki>. Be sure to add this path before your map name. For example: <nowiki>my_maps/de_examplemap</nowiki>

<br />
:'''-usebsp'''
:This runs the map through a special vbsp process to generate clean map geometry from brushes, removing hidden faces and stitching up edges, making the CS2 version easier to work with in Hammer. It preserves world (vis) brushes and func_detail brushes for compatibility with S2. This parameter will also merge all func_instances in your map. Note that the final geometry will be triangulated, but cleaning it up is a fairly simple process, which will be explained in another guide.

<br />
:'''-usebsp_nomergeinstances'''
:Use this instead of -usebsp if you wish to both generate clean geo and also preserve func_instances. Note that this takes a little longer as it has to run through the import process twice. The final geometry will also be triangulated.

<br />
:'''-skipdeps'''
:Optional: skips importing all dependencies/content and only generates the vmap file(s). This provides a 'quick' import when iterating entities for example. Do not run with this if you are importing for the first time. 

<br />
'''Example:'''
<nowiki>python import_map_community.py "C:\steam\steamapps\common\Counter-Strike Global Offensive\csgo" "c:\map_sources\" "C:\steam\steamapps\common\Counter-Strike Global Offensive\game\csgo" de_example_cs2 de_examplemap -usebsp</nowiki>
