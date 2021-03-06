This project is designed with armips.exe at the center, and will eventually include grit to compile images.
If you have questions even after you've read the whole thing, go to #PokemonClover on Rizon and pester NewDenverCity about it.

### HOW TO CONTRIBUTE

1. Create a GitHub account.
2. Go to this repository, then fork.
3. Make changes.
4. Make pull request to this repository. Add comments as to what you did or I'll ignore it.
5. I'll look it over and accept it as I see fit.

### HOW TO COMPILE STUFF

1. Download a .zip of the repository and extract it. It'll automatically be named Pokemon-Clover-master.
2. Put a CLEAN FireRed ROM in the main folder, and name it clover.gba.
3. Apply base.ips to clover.gba
4. Open your command prompt, navigate to Pokemon-Clover-master or whatever else you've renamed the folder to.
5. Type armips.exe main.asm -sym offsets.txt, then hit enter.

### HOW TO ADD FILES
When we get images, I'll create a folder called images, and two subfolders called pic and bin. Put pictures into pic, grit will compress those and put them in bin.

Scripts that use our script_defines.asm file should be put in a folder called `scripts/`, and ASM and C should go in a folder named source.

### ASM AND SCRIPT STYLE
Comments are added through `;`.
Labels can only be defined once, and `.org`s MUST be in order.
Instead of `#org` for scripts, use this style;

```
Start:
	call Label
	end

Label:
	return
```

Defines will usually use either macros or equs.
```
BOBBO equ 1
```
```
	.macro bobbo, label
	.byte BOBBO
	.word \label
	.endmacro
```

To write text:
```
	.table "defines/charmap.txt"
	.str "BOBBO: bobbo.\p", "It looks tired."
```

### DATA TYPES
- `.byte`: 8 bits.
- `.hword`: 16 bits. Also known as a halfword.
- `.word`: 32 bits.
- `.short` = `.hword`
- `.long` = `.word`
Pointers are words, for example.

### VARIOUS USES FOR THE HOOKS FILE
Obviously used for ASM hooks.
However, it is also used for other things. The free space in the ROM starts at roughly 0x85A3D8(subject to change based on music and maps), so that's where all of the compiled stuff will be going. Anything before that will be included in hooks.asm.
The most common use for this will probably be moveset updates or bug fixes. It's also going to be used for pointers to pictures and stuff as well.

Moveset editing sub-tutorial:

1. Apply base.ips to clover.gba, but don't compile the project yet. Copy clover.gba and paste that under a different name.
2. Edit the movesets like you normally would.
3. Compare clover.gba and its edited copy in HxD or another hex editor.
4. Odds are you're seeing two changes if you've repointed the moveset. The first is a pointer to the second, and the second change is the new moveset.
5. Open up hooks.asm and add a .org with the offset of the pointer, not the offset that the pointer, well, points to.
6. Write a unique label under the .org. Add .word under that, and then another unique label following that. Here's what it'd look like as an example for Grasshole.
```
	.org 0xXXXXXX
grassholemovesetpointer:
	.word grassholemoveset
```
7. Go to misc.asm and add the second unique label you wanted. Add a bunch of .bytes with whatever you found under the second difference, and then reread HOW TO CONTRIBUTE from step two.

The moveset editing sub-tutorial can be applied to a lot of things, but the general process still works.
