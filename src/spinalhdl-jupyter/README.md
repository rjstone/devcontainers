# spinalhdl-jupyter

Pre-build:
```
ghcr.io/rjstone/spinalhdl-jupyter
```

I primarily created this for [Spinal-bootcamp](https://github.com/rjstone/Spinal-bootcamp)
but I plan to rework it in at least two different images and/or templates so
that it contains more tools, such as the symbiflow toolchain, and doesn't
necessarily contain Jupyter Notebook if that's not needed.

For right now it's still one big image and hasn't been broken out into
"devcontainer features", a template, or more layers.

**Note!** that the versions of the tools here are pretty specific as different
ones have some narrow ranges of what other dependencies they work with.

* ENV JAVA_JDK_VERSION="17"
* ENV ALMOND_VERSION="0.14.1"
* ENV SCALA_VERSION="2.13.1"

Trying to use anything more recent than these versions of things is likely
to result in more crashing and burning of different tools.
