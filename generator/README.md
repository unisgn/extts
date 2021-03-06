1. Empty docs (from jsduck), 3.out, 1.src
2. Copy all ExtJS source to 1.src\ext-x.x.x (from ExtJS/src/: packages, classic, modern)
3. Execute AS ADMIN 1.src\ext-x.x.x.cmd => check result at 3.out
4. Generate TS by running c2.tools.ExtTS

## Steps to generate typings for 3.2/3.4

`c2.tools.ExtTS.exe` will scan the `1.src`, `2.docs`, and `3.out` folders if no argument is passed in.
So the easiest way to use it is to remove uncecessary files under these folders.

```sh
rm -rf 1.src
rm -rf 2.docs
rm -rf 3.out
mkdir 1.src
mkdir 2.docs
mkdir 3.out

# copy ExtJS 3.2/3.4
cp <your-ext-folder> 1.src/ext-3.x

# Using ExJS 3.2 as an example, you should have a `1.src/ext-3.2.0/src` folder
# Run jsduck and fix some issues with the source code along the way.
# For my case, I have to remove `locale` folder, and comment out two files.
cd 1.src
../0.tools/jsduck-6.0.0-beta.exe ext-3.2.0/src --output ../2.docs/ext-3.2.0
```

For ExtJS 3.2, the jsduck will generates the docs with bunch of warnings.
That's ok for now (I have yet to detail test the resulting typings to see if I need ot make improvements, but most likely I'll just edit the resulting typings directly instead).
The generator tool (below) needs `2.docs/ext-3.2.0/source/Version.html` which is not generated by jsduck.
Go ahead to create it with the following content:

```html
var version = '3.2.0';
```

After that, build `cs.tools.ExtTS/cs.tools.ExtTS.sln` using Visual Studio

And run the executable `cs.tools.ExtTS/bin/Debug/cs.tools.ExtTS.exe`

You will receive your prize in `3.out/ext-3.2.0.d.ts`.
