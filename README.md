# videoInput MinGW/cmake building tree.

	"Finding a reason to do this is the hard part, actually doing it is not that bad..."
		-- Paul 'bozo' Fitzpatrick

## ACKNOWLEDGEMENTS

Thanks to Theodore Watson for the the library.
Thanks to Samuel Audet for his notes on using DirectShow with MinGW 
(http://step.polymtl.ca/~guardia/programming.php)
Thanks to Paul Fitzpatrick for even more notes on marrying the two and encouraging quote above
(http://makesweet.com/bozo/2008/01/24/compiling-directshow-with-mingw-on-linux)

## NOTA BENE

I have managed to build videoInput.lib using cmake's nmake generator and Visual C++ compiler that comes with 
Microsoft Vista SDK. I don't use Visual C myself and thus haven't had a chance to test the library
built this way. 

## RATIONALE

There exist several web pages explaining the steps necessary to take to build code using Direct Show (such 
as, but not limited to, videoInput library) with MinGW. The steps are tedious, error prone and dependent on 
the particulars of one's configuration (namely, which combination of versions of mingw, direct show and/or 
windows SDK are used). The goal of this fork is to record my experience in following those instructions in
an easily reproducible manner ("so that other don't have to"). 

The original library comes with project files and built binaries for a number of C++ environments. 
Theoretically, cmake is able to produce such project files as well - thus making the fork at least as 
ubiquitous as the original. In practice though, each environment requires some fine tuning that I'm unable 
to figure out at this point on my own. 

## INSTRUCTIONS

To actually build this you will need:

(a) prerequisites:

- cmake.exe
- MSYS shell or other bash. I haven't able to build from cmd.exe even with MinGW generator
    - it IS possible however to build in cmd.exe using nmake and visual c, see below
- patch.exe

(b) headers from old microsoft direct show sdk

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

(c) headers from BaseClasses directory of windows sdk. I have managed to build with headers from 
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

And finally, if you want to use cmake + nmake + visual c (DISCLAIMER this works, 
but I haven't tested the resulting library), you may go like:

    # assuming you're in the cmd.exe finetuned for using Visual C compiler from
    cmake . -G "NMake Makefiles"
    nmake

