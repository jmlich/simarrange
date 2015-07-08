General
=======

Simarrange is a program that simulates collisions between STL meshes in 2D in order to generate tightly packed sets of parts.
It takes a directory of STL files as input and outputs STL files with combined plates of parts.
The parts are assumed to be in the correct printable orientation already.

Usage
-----

```
./simarrange test
```

This will convert the STLs in the directory "test" into plates using the default options, and emit these plates and thumbnails of them into the current directory.

The program understands the following options:

```
-x --width
x dimension of build plate in mm, default 200, must be a positive integer

-y --height
y dimension of build plate in mm, default 200, must be a positive integer

-c --circle
build plate is circular, with diameter given by -x

-m --middle
place first part in the middle of the plate, and pack around it (slower)

-s --spacing
Minimal spacing between parts, in mm, default 1, must be a positive integer

-r --rotstep
rotational granularity of search space in degrees, default 10 (reduce for tighter, slower packing)

-p --posstep
positional granularity of search space in mm, default 5 (reduce for tighter, slower packing)

-n --repeat PATH+N
add N copies of the file at PATH (or if path is a directory, of the files in
that directory) to the plate job

-o --outputdir
output directory, default "."

-d --dryrun
only do a dry run, computing placement but not producing any output file

-j --threads
How many threads to use. Default is as many as possible, set to 1 for single-threaded operation

-q --quiet
Output as little noise as possible on stdout

-l --limit
Stop after this many plates are filled
```

Examples
--------

```
./simarrange -x 200 -y 150 -o output input
```

Convert all files from the directory "input" into plates of size 200x150mm and save to directory "output", creating it if necessary

```
./simarrange -x 179 -c input
```

Convert all files from the directory "input" into circular plates of 170mm diameter and save to directory "output", creating it if necessary

```
./simarrange -s 10 input
```

Convert all files from the directory "input" into plates of size 200x200mm with 10mm spacing between parts

```
./simarrange -m input
```

Convert all files from the directory "input" into plates of size 200x200mm, packing from the middle

```
./simarrange -n input+10 -n input2+10
```

Convert all files from the directories "input" and "input2" into plates of size 200x200mm,
with 10 copies of each object

```
./simarrange -x60 -y60 -s3 blah -p 2
```

Convert all files from the directory "blah" into plates of size 60x60mm with 3mm spacing between parts and 2mm search grid


```
./simarrange file1.stl file1.stl file2.stl file2.stl file2.stl file2.stl
```

Place two copies of file1.stl and four copies of file2.stl into plates of size 200x200mm

```
./simarrange input file1.stl file1.stl file2.stl file2.stl file2.stl file2.stl
```

Place all STL files in the directory "input" as well as two copies of file1.stl and four copies of file2.stl into plates of size 200x200mm


Compiling
---------

simarrange depends on the following libraries:

* admesh
* utlist (included in source)
* argtable2
* opencv (libcv, libcxcore, libhighgui)

You can install the dependencies in a debian/ubuntu system with the following command:

```
sudo apt-get install libhighgui-dev libcv-dev libargtable2-dev libadmesh-dev
```

A build script called Makefile is included in the package. Run it by command "make" to compile the program.


Compiling on Mac OS

First, you will need to install Homebrew. Once you've done that, run

```
brew tap homebrew/science
brew install admesh
brew install argtable
brew install opencv
```

and then run make included with simarrange to compile the program.
