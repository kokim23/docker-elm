### Quick start

Add an `elm` alias (you probably want to add this to your `.bashrc` or similar): 

```sh
alias elm='docker run -it --rm -v "$(pwd):/code" -w "/code" -e "HOME=/tmp" -u $UID:$GID -p 8000:8000 codesimple/elm:0.15.1'
```

Then use the alias to run the elm tools (version 0.15.1) from the container as you would normally:

```sh
elm make
elm package
elm reactor
elm repl
```

### Further information

The elm tools in the image are built from source following the instructions at
https://github.com/elm-lang/elm-platform.

The current directory is mounted into the container so that the elm tools can locate your project files
and write out `elm.js` and the `elm-stuff` directory. 

Port 8000 is published to the host to allow this to be used when running `elm reactor`.
Change the `-p` setting if you want to map this to a different port or disable it.

`HOME` is set to `/tmp` in the container so that the `.elm` directory generated by the elm tools is placed there
and discarded. If you want to share `.elm` between runs you could set it to a directory that you have
mounted a volume or host directory at.

If you run `elm reactor` on an uncompiled project, you may get errors along the lines of:

```
elm-make: elm-stuff/packages/evancz/virtual-dom/2.1.0/src/VirtualDom.elm: hGetContents: invalid argument (invalid byte sequence)
elm-make: thread blocked indefinitely in an MVar operation
```
This can generally be avoided by running `elm make` on the project before you run `elm reactor` (or after to fix it).
