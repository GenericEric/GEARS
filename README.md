# GenericEric's Arch Rice Script (GEARS)
## a fork of [LARBS](https://github.com/lukesmithxyz/LARBS)


#This script is highly unstable as of now. I recommend you only run it for a user account you don't care to keep. Or if you just want to play with it. :)


## Installation:

On an Arch based distribution as root, run the following:

```
download gears.sh and run ./gears.sh
```

## What is GEARS?

GEARS is a script based on LARBS by Luke Smith that autoinstalls and autoconfigures a fully-functioning
i3 Window manager based Arch Linux environment.

GEARS is intended to be run on a fresh install of Arch Linux, and
provides you with a fully configured diving-board for work or more
customization. But GEARS also works on already configured systems *and* other
Arch-based distros such as Artix, Manjaro, and Parabola (although Parabola,
which uses slightly different repositories might miss one or two minor
programs).

## Customization

By default, GEARS uses the programs [here in progs.csv](progs.csv) and installs
[my dotfiles repo (archrice) here](https://github.com/genericeric/archrice),
but you can easily change this by either modifying the default variables at the
beginning of the script or giving the script one of these options:

- `-r`: custom dotfiles repository (URL)
- `-p`: custom programs list/dependencies (local file or URL)
- `-a`: a custom AUR helper (must be able to install with `-S` unless you
  change the relevant line in the script

### The `progs.csv` list

GEARS will parse the given programs list and install all given programs. Note
that the programs file must be a three column `.csv`.

The first column is a "tag" that determines how the program is installed, ""
(blank) for the main repository, `A` for via the AUR or `G` if the program is a
git repository that is meant to be `make && sudo make install`ed. `V`if it's for
the void linux distribution's xbps package manager.

The second column is the name of the program in the repository, or the link to
the git repository, and the third comment is a description (should be a verb
phrase) that describes the program. During installation, GEARS will print out
this information in a grammatical sentence. It also doubles as documentation
for people who read the csv or who want to install my dotfiles manually.

Depending on your own build, you may want to tactically order the programs in
your programs file. GEARS will install from the top to the bottom.

If you include commas in your program descriptions, be sure to include double
quotes around the whole description to ensure correct parsing.

### The script itself

The script is extensively divided into functions for easier readability and
trouble-shooting. Most everything should be self-explanatory.

The main work is done by the `installationloop` function, which iterates
through the programs file and determines based on the tag of each program,
which commands to run to install it. You can easily add new methods of
installations and tags as well.

Note that programs from the AUR can only be built by a non-root user. What
GEARS does to bypass this by default is to temporarily allow the newly created
user to use `sudo` without a password (so the user won't be prompted for a
password multiple times in installation). This is done ad-hocly, but
effectively with the `newperms` function. At the end of installation,
`newperms` removes those settings, giving the user the ability to run only
several basic sudo commands without a password (`shutdown`, `reboot`,
`pacman -Syu`).
