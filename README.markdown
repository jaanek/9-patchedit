9pedit
======
Batch edit 9patch files from the command line.

Usage
-----
	9pedit - Batch editing for 9-patch pngs

	Syntax:
		9pedit {command options} file...
	
	Each file is processed in order and the commands given on the command
	line are applied to each.  When all commands are applied to a file it
	is saved.  If the commands result in a 9-patch, a .9.png will be saved.
	If the commands result in a regular .png, then a .png file will be saved.

	Command options:
		-clear: Reset the 9-patch border to #00000000.  Presumes that a border is present
				Converts to .9.png if not already.
		-strip: If is a .9.png, converts back to a standard .png, removing the border
		-sx {pixels}: Set the given pixels on the x-scale axis (top)
		-sy {pixels}: Set the given pixels on the y-scale axis (left)
		-px {pixels}: Set the given pixels on the x-padding axis (bottom)
		-py {pixels}: Set the given pixels on the y-padding axis (right)
		-template {source.9.png}: Gets border information from another file
	
	All arguments that take {pixels} take a comma delimitted list of zero based
	pixel numbers or ranges.  Example: 0,1,10-20

Download
--------
Either check the project out and build by doing "ant dist" (note you need to git submodule init && git submodule update
after clone) -or- download prebuilt binaries from the dist folder:

* https://github.com/stellaeof/9-patchedit/raw/master/dist/9pedit (posix executable)
* https://github.com/stellaeof/9-patchedit/raw/master/dist/9pedit.jar (java executable jar)
* https://github.com/stellaeof/9-patchedit/raw/master/dist/9pedit.exe (windows executable)


Examples
--------
Convert a standard png to 9-patch:

	athena:files stella$ 9pedit -clear raw.png 
	--> Processing file raw.png
	  - Image is 28x28 pixels
	  + Clear 9 patch border area to default
	  => Saving to raw.9.png
	
Convert a 9-patch to a standard png (remove border):

	athena:files stella$ 9pedit -strip raw.9.png
	--> Processing file raw.9.png
	  - Image is 30x30 pixels
	  + Strip 9-patch border and convert to standard png
	  => Saving to raw.png

Programmatically set scale and padding pixels:

	athena:files stella$ 9pedit -sx 14 -sy 15 -px 9-19 -py 4-25 raw.png
	--> Processing file raw.png
	  - Image is 28x28 pixels
	  + Set x scale points 14
	  + Set y scale points 15
	  + Set x padding points 9-19
	  + Set y padding points 4-25
	  => Saving to raw.9.png	

Copy scale and padding pixels from another 9-patch:

	athena:files stella$ 9pedit -template template.9.png raw.png
	--> Processing file raw.png
	  - Image is 28x28 pixels
	  + Set x scale points 15
	  + Set x padding points 9,10,11,12,13,14,15,16,17,18,19
	  + Set y scale points 14
	  + Set y padding points 4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25
	  => Saving to raw.9.png
	
Update an existing 9-patch:

	athena:files stella$ 9pedit -clear -sx 14 -sy 15 raw.9.png
	--> Processing file raw.9.png
	  - Image is 30x30 pixels
	  + Clear 9 patch border area to default
	  + Set x scale points 14
	  + Set y scale points 15
	  => Saving to raw.9.png	
	
Expand a 9-patches to new sizes and save as standard pngs (I did this as a shortcut to building a bigger combined background image in the image editor):

	athena:btn stella$ 9pedit -expand 34x34 -out ../session -strip nav.9.png
	Will save images to ../session
	--> Processing file nav.9.png
	  - Image is 22x21 pixels
	  + Expand 9-patch to 34x34
	  + Strip 9-patch border and convert to standard png
	  => Saving to ../session/nav.png

