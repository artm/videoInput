videoInput MinGW/cmake building tree.

	"Finding a reason to do this is the hard part, actually doing it is not that bad..."
		-- Paul 'bozo' Fitzpatrick

ACKNOWLEDGEMENTS

Thanks to Theodore Watson for the the library.
Thanks to Samuel Audet for his notes on using DirectShow with MinGW 
(http://step.polymtl.ca/~guardia/programming.php)
Thanks to Paul Fitzpatrick for even more notes on marrying the two and encouraging quote above
(http://makesweet.com/bozo/2008/01/24/compiling-directshow-with-mingw-on-linux)

INSTRUCTIONS

To actually build this you will need:

(a) prerequisites:

- cmake
- MSYS shell or other bash. I haven't able to build from cmd.exe even with MinGW generator
- patch.exe. Not every version works - the one that comes with MSYS doesn't. I used the one
  from git for windows. The same is true with diff.exe - the ones that comes with git and
  vim for windows work, the one that comes with MSYS is confused by the line ends or something.

(a) headers from old microsoft direct show sdk

I can't seem to be able to locate the official version, whatever it is. 
Theodore Watson's original videoInput repository contains a copy, I decided 
against redistributing Microsofts "property".

Once you've found the headers, put them in Include/DShow (so that there exists, say, file
Include/DShow/Amvideo.h). Then:

	cd Include/DShow
	../DShow.preprocess.sh
	patch < ../DShow.patch

Rationale: one of the files that has to be patched is diff-unfriendly, it has some non-standard
line endings which are cured by the DShow.preprocess.sh (which simply calls unix2dos.exe on that 
file). After that patching actually fixes the MinGW incompatible files.

(b) headers from BaseClasses directory of windows sdk. I have managed to build with headers from 
Vista SDK. Since Microsoft notoriously breaks the internet, I will not provide an URL, other then

	http://www.google.com/search?q=windows+sdk
	
Standard location of the directory that we want is:

	C:/Program Files/Microsoft SDKs/Windows/v6.0/Samples/Multimedia/DirectShow/BaseClasses/

So, go like:

	cd Include
	cp -a "C:/Program Files/Microsoft SDKs/Windows/v6.0/Samples/Multimedia/DirectShow/BaseClasses" BaseClasses
	cd BaseClasses
	patch < ../BaseClasses.patch

After which libvideoInput.a can be built with cmake. For example...

	# assuming you're back to source root
	cmake . -G "MSYS Makefiles"
	make
	
Or using a separate build directory:
	
	cd WHATEVER
	mkdir build-videoInput
	cd build-videoInput
	cmake PATH/TO/videoInput -G "MSYS Makefiles"
	make

Or, if for some reasone you must build using "MinGW Makefiles" generator:

	cmake . -G "MinGW Makefiles"
	mingw32-make

	
