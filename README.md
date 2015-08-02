
# What is it?

My personal scripts for migrating to a new Mac.

# Why?

Migration Assistant is for chumps. Do you really trust it? Can you be
without a computer for the many many hours it's going to take telling
you it has just 30 mins to go?


# Steps

## Store state
On current box, capture list of installed homebrew packages and casks
into files in your $HOME:

```
cd
brew list > brew-pkgs.txt
brew casks list > brew-casks.txt
```

## Initial $HOME Sync

On the new box, rsync to pull all your files over:

```
rsync -avv oldcomp:/Users/$USER /Users/
```

(keep an eye on long batches of crud you should probably clean out,
particularly .dotdirs and ~/Library/App Support/*) 

At this point you can keep working on old box while the lengthy sync
and installs takes place.

## Install software

Once at least those brew-*.txt files are synced, you can install software.

On new box, install Homebrew (crazy curl | sh way or
download an installer). Then install pkgs and casks:

```
cd
cat brew-pkgs.txt | xargs -L 1 brew install
cat brew-casks.txt | xargs -L 1 brew cask install
```

## Sync other data
Sync any other needed data, manually copy apps that don't have
casks. E.g. I keep my music and videos in /Users/Shared

```
rsync -avv oldcomp:/Users/Shared /Users/
```


## Final Sync
After the first sync is complete, close everything, logout of old
box, and do final sync to catch last minute changes.

```
rsync -avv oldcomp:/Users/$USER /Users/
```

Reboot new box, login, and things should look mostly like they did on
old box.


## Misc issues

* Launchbar (or Alfred) needs to be told your apps are in /opt/homebrew-cask/Caskroom
* Licenses for many things don't get carried over for some reason.
** some might have been due to installing more recent versions via brew cask
* In this case I moved from Mavericks to Yosemite in the process, so
  bunch of prefs to tweak.

# TODO

To make this easier in the future:
* Symlink .dotdirs (e..g .npm) to /usr/local/dotcaches or similar
* split iPhoto lib and archive in pieces
* make/find a script to prune caches everywhere
* Store all licenses in 1password
