This is my attempt at making building videoInput library with MinGW reproducible.

To actually build this you need:

(a) headers from old microsoft direct x sdk
(b) headers from BaseClasses directory of windows sdk. I have managed to build with Vista sdk.
(c) a diff that makes the above two MinGW compatible. I had to define some empty macros to hide 
some VC++ features from g++, for now they are in the videoInput.cpp, I'll separate them later.

