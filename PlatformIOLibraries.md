# Building Your Own PlatformIO Libraries

PlatformIO provides a deceptively easy way of including libraries for various embedded hardware. It can be a useful tool for developing embedded applications. If you write enough code you will likely get to the point of wanting to reuse some of your code for new applications. 

PlatformIO does a lot to attempt to make the build process simple. However, some things can be a bit hard to figure out owing to the normal state of documentation that is short on precision and examples.

Here we will describe a method of creating a collection of libraries which can reference other external libraries as well as libraries that are part of the collection being built.

There is a fair amount of documentation available:
 * [PlatformIO Docs](https://docs.platformio.org/en/latest/)
 * [Library Management](https://docs.platformio.org/en/latest/librarymanager/index.html)
 * [platformio.ini](https://docs.platformio.org/en/latest/projectconf/index.html)


The goal here is to create a single library root to put your libraries in. The resulting file organization will look like the following:
```
|─ MyLibraries
   |─ LibA
   |  |─ library.json
   |  |─ platformio.ini
   |  |─ src
   |     |─ LibA.cpp
   |  |─ include
   |     |─ LibA.h
   |─ LibB
   |  |─ library.json
   |  |─ platformio.ini
   |  |─ src
   |     |─ LibB.cpp
   |  |─ include
   |     |─ LibB.h
   |─ LibC
      |─ library.json
      |─ platformio.ini
      |─ src
         |─ LibC.cpp
      |─ include
         |─ LibC.h

```

The details of library.json are fairly well documented. There are a lot of options that can be used. It is where you will reference external dependencies like libraries in the PlatformIO registry.

A few important points.
 - platformio.ini must be there for each of the libraries. If it is missing PlatformIO will not recognize the library as something it should use. If a library in this structure depends on other libraries in the MyLibraries tree platformio.ini will need the following:
 ```
 lib_extra_dirs = ../MyLibraries
 lib_deps = LibB, LibC
 ```
- Ideally your libraries should have no dependencies. This isn't always practical. If your library has dependencies on libraries in the PlatformIO registry you should put them in the dependencies section of the library.json file. From what I can tell dependencies on other libraries in your MyLibraries structure must be referenced using
platformio.ini rather than library.json
- Each library, "LibA, LibB, LibC..." can be a Git repository. It it up to you to arrange them on your local machine so the PlatformIO will work with them.

This document is likely not perfect and the correct methods for working with PlatformIO will likely change over time.