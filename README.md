Evented I/O for V8 javascript. [![Build Status](https://secure.travis-ci.org/joyent/node.png)](http://travis-ci.org/joyent/node)
===

### To build:

Prerequisites (Unix only):

    * Python 2.6 or 2.7
    * GNU Make 3.81 or newer
    * libexecinfo (FreeBSD and OpenBSD only)

Unix/Macintosh:

    ./configure
    make
    make install

Windows:

    vcbuild.bat

### To run the tests:

Unix/Macintosh:

    make test

Windows:

    vcbuild.bat test

### To build the documentation:

    make doc

### To read the documentation:

    man doc/node.1

Resources for Newcomers
---
  - [The Wiki](https://github.com/joyent/node/wiki)
  - [nodejs.org](http://nodejs.org/)
  - [how to install node.js and npm (node package manager)](http://joyeur.com/2010/12/10/installing-node-and-npm/)
  - [list of modules](https://github.com/joyent/node/wiki/modules)
  - [searching the npm registry](http://search.npmjs.org/)
  - [list of companies and projects using node](https://github.com/joyent/node/wiki/Projects,-Applications,-and-Companies-Using-Node)
  - [node.js mailing list](http://groups.google.com/group/nodejs)
  - irc chatroom, [#node.js on freenode.net](http://webchat.freenode.net?channels=node.js&uio=d4)
  - [community](https://github.com/joyent/node/wiki/Community)
  - [contributing](https://github.com/joyent/node/wiki/Contributing)
  - [big list of all the helpful wiki pages](https://github.com/joyent/node/wiki/_pages)


Windows C++ Addon
=================

3. Create a DLL project in Visual C++

3.1 Go to: File \ New Project

3.2 And then pick: Visual C++ \ Win32

3.3 And after that: Win32 Project

3.4 In Win32 Application Wizard \ Application Settings set Application Type to DLL

NOTE: In my case I called my addon with the original name of "myaddon"

4. Write your addon code

4.1 Add your code. In my case I just adapted the example at the documentation:

extern "C" void NODE_EXTERN init (Handle<Object> target)

{

HandleScope scope;

target->Set(String::New("hello"), String::New("world"));

}

NOTE: Do not forget NODE_EXTERN (which is defined as __declspec(dllexport) in

src/node.h) otherwise you will get the error ÎéÎíNo module symbol found in module.ÎéÎí when

trying to load the module via require()

4.2 Add the reference to node.lib to your code. I made it with the line:

#pragma comment(lib, "c:\\node.js\\src\\Debug\\node")

NOTE: In my case I added it to the file called "myaddon.cpp". This is an important step,

otherwise you will get a ton of unresolved external symbols from the linker

VC++ HINT: You can add the Library Directories under the project's Property Pages \

Configuration Properties \ VC++ Directories. In my case node.lib was at:

"c:\node.js\src\Debug", so the pragma line can be reduced to a more elegant #pragma

comment(lib, "node")

4.3 Add the Include Directories or adjust the headers inclusions to match yours so

the compiler can find them

NOTE: In my case I just quickly adjusted all of them (they are just a few) to point directly

to their full path. For example when it was including v8.h I replaced it with

c:\node.js\src\deps\v8\include\v8.h, when it was including node_object_wrap.h I replaced

it with c:\node.js\src\src\node_object_wrap.h, and so on

VC++ HINT: You can add the Include Directories under the project's Property Pages \

Configuration Properties \ VC++ Directories. In my case they are:

"c:\node.js\src\deps\uv\include\;c:\node.js\src\deps\v8\include\;C:\node.js\src\src\"

5. Build the addon

5.1 Build it and you will get your addon compiled as a .dll file (at Debug directory if

you compiled under the DEBUG profile)

VC++ HINT: Just press F7

5.2 Rename it as .node

VC++ HINT: You can change the Target Extension under the project's Property Pages \

Configuration Properties \ General, so you can get it directly as .node

5.3 Copy it to the same directory of the node.exe generated previously

NOTE: This is to make it simple to test it by requiring from ./

VC++ HINT: You can change the Output Directory under the project's Property Pages \

Configuration Properties \ General, so you can get it directly in the same directory than

node.exe

5.4 Run the addon

NOTE: In my case I get the following output:

C:\node.js>node

> require("./myaddon").hello

'world'

