---
title: Storing KiCad Libraries in Your Project Folder
description: "Learn how to store KiCad libraries directly in your project folder to improve portability, version control, and independence. Perfect for sharing designs, cloning projects, and avoiding dependency issues."
date: 2026-02-11 12:00:00 +0600
tags:
  - blog
  - pcb
  - kicad
categories: [PCB, KiCad]
---
🚀 KiCad Tip: Store libraries _in your project folder_ 

No more missing symbols or broken designs when you clone or share!

✅ Portability: Works on any machine, anywhere  
✅ Version control: Git commits include all parts  
✅ Independence: No global library conflicts

Perfect for sharing, prototyping, or team projects.

The following way can be used as the project structure::
```
my_project/
  my_project.kicad_pro
  libs/
    symbols/            // my_project.kicad_sym etc.
    footprints/         // my_project.pretty/
    3dmodels/           // *.step, *.wrl files
```

Personally, I prefer the way described in this [article](https://hackaday.com/2017/05/18/kicad-best-practises-library-management/).
```
my_project
  my_project.kicad_pro
  packages3D    // *.step, *.wrl files
  lib_sch       // my_project.kicad_sym etc.
  lib_fp.pretty // all project footprints
```

If we have a completed project and want to export all the libraries used in it to new folders, we’ll need to follow these steps:
## 1. Export 3D models

For that let's install the Archive3DModels plugin. 
To install the Archive 3D Models plugin in KiCad, use the built-in Plugin and Content Manager (PCM), as it's the standard method for KiCad 7, 8, or later versions.[](https://github.com/MitjaNemec/Archive3DModels)
### Installation Steps

1. Open KiCad and go to **Preferences > Plugin and Content Manager** (or **Manage Plugin and Content Manager** in some versions).
2. Switch to the **Plugins** tab, then search for "Archive3DModels" or "Archive 3D Models".
3. Select the plugin from the list (published by MitjaNemec), click **Install**, and then **Apply Pending Changes**.
4. Restart KiCad if prompted; the plugin should then appear under the **Tools** menu in Pcbnew (PCB editor) as "Archive 3D models".[](https://github.com/Steffen-W/Import-LIB-KiCad-Plugin)

### Usage

Run the plugin from Pcbnew after placing footprints with 3D models—it copies models to a local `packages3D` folder and updates paths in the `.kicad_pcb` file for portability.[](https://github.com/MitjaNemec/Archive3DModels)​

If the plugin doesn't appear post-install (noted in KiCad 8 cases), check the project folder for `archive_3d_models.log`, enable debug logging in plugin settings, and verify paths (e.g., aliases like "CUSTOM" may need resolution). Older manual methods via GitHub ZIP are possible but less reliable due to compatibility issues in newer KiCad versions.
## 2. Export PCB footprint

To export PCB footprints to a library in KiCad using **File > Export > Footprints to library**, open the PCB Editor (Pcbnew) first. This feature creates a new `.pretty` library file containing selected or all footprints from your current board.
### Step-by-Step Process

1. Launch **Pcbnew** from your KiCad project and ensure your desired footprints are loaded or placed on the board.
    
2. Go to **File > Export > Footprints to library...** (available in KiCad 6+; in Footprint Editor, it's similar but board-specific).
    
3. In the dialog:
    
    - Enter a **Library name** (e.g., `MyCustomFootprints`).
    - Choose **Library type** as "New library" or append to an existing one.
    - Select footprints: Check "Selected footprints only" if you want specific ones (pre-select via Edit > Select > All footprints or individual), or export all.
        
4. Specify the output path and filename (e.g., `MyCustomFootprints.kicad_mod` in a `.pretty` folder).
    
5. Click **Export**—KiCad generates the library file.​
    

### Post-Export Usage

The library will be added to the project paths automatically after creation, if not, add the new library via **Preferences > Manage Footprint Libraries > Global or Project Libraries > Browse** (folder icon), then select your `.pretty` folder. 
## 3. Export schematic libraries

To export schematic symbols to a library in KiCad using **File > Export > Symbols to new library**, open the **Eeschema** (Schematic Editor) first. This feature creates a new `.pretty` library file containing selected or all footprints from your current board.

### Export Process

1. Open your schematic in **Eeschema** (Schematic Editor).
    
2. Go to **File > Export > Symbols to new library**...** (available in KiCad 9+).
    
3. Specify the output path and filename (e.g., `Library.kicad_sym` in a `symbols` folder).
    
4. Click **Save**—KiCad generates the library file.​

### Post-Export Usage

The library will be added to the project paths automatically after creation, if not, add the new library via **Preferences > Manage Symbol Libraries > Global or Project Libraries > Browse** (folder icon), then select your `*.kicad_sym` file. 

