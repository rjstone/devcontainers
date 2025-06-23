# Development Container Images

This is where I have some developmemnt container images for pre-building.
Right now there's only one CI workflow and it's pretty simple, so might make
a good example to use. See the `.github/workflows/` directory.

## Table of Contents

The devcontainer images are in `src`. Each subdirectory has a `.devcontainer`
directory with the image configuration and other bits needed, along with a
readme file with more info.

### devcontainer-base

This isn't much different than the official devcontainer base images, but
it only builds Ubuntu at the moment. I just wanted something a little bit
more stripped down with no zsh. It only has the stuff needed for interactive
login (including man pages) and installing stuff.

### spinalhdl-jupyter

This contains a tricky to put together combination of the java jdk, scala,
almond (Jupyter kernel) and Jupiter Notebook, along with associated vscode
extensions (if using vscode). See the readme for more info. (I plan to
break this one into more layers later and add associated stuff.)

## Disclaimer

As usual these are provided "AS IS" as the MIT license says, but they're
especially "AS IS" because they haven't been tested much at this point.
Please submit issues or PRs if you find problems or have any ideas.