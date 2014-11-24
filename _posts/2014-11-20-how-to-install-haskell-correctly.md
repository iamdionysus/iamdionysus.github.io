---
layout: post
title: How to install haskell in correct way
---

## General Idea
Haskell-platform is good. But you will end up with cabal hell. So the idea is having ghc seperately and use stackage. After that, uninstall haskell-platform. 

## Install haskel-platform, get right version of happy and alex
It's hard to meet ghc build condition. Just install haskell-platform first. Ghc is needed any to bootstrap higher version of ghc. To install happy and alex right version, upgrade cabal and install them.

```
yum install haskell-platform
yum install gmp-devel
```

Make sure `$HOME/.cabal/bin` is in front of PATH envrionment so that it can use latest cabal installed in ./cabal/bin when installing happy and alex.

```
cabal update
cabal install cabal cabla-install
cabal install happy alex
```

## Download ghc source and build

```
wget https://www.haskell.org/ghc/dist/7.8.3/ghc-7.8.3-src.tar.bz2
tar xvjf ghc-7.8.3-src.tar.bz2
```

Configure the build condition to speed up the build process.

```
cd ghc-7.8.3
cp mk/build.mk.sample mk/build.mk
```

Open `mk/build.mk` and uncomment this.

```
#BuildFlavour = quick
```

Start the build.

```
perl boot
./configure
make -j9  #if you have 8 cores
make install
ghc --version
```

All these manuals are worthwhile to read.

* [ghc github readme](https://github.com/ghc/ghc)
* [getting the GHC sources manual](https://ghc.haskell.org/trac/ghc/wiki/Building/GettingTheSources)
* [getting started with the build system manaul](https://ghc.haskell.org/trac/ghc/wiki/Building/Hacking)

## Remove haskell-platform and related

```
yum remove haskell-platform
yum autoremove
```

```
rm -rf ~/.cabal
rm -rf ~/.ghc
```

## Install cabal and get ready for stackage
[good instruction](https://github.com/fpco/stackage/wiki/Preparing-your-system-to-use-Stackage#other-linux), however `sh bootstrap.sh --no-doc` trick is needed.

```
wget http://hackage.haskell.org/package/cabal-install-1.20.0.3/cabal-install-1.20.0.3.tar.gz
tar zxfv cabal-install-1.20.0.3.tar.gz
cd cabal-install-1.20.0.3
sh bootstrap.sh --no-doc # very important haddock won't be here!
cabal update
```

## Edit ~/.cabal/config to use stackage
Open `~/.cabal/config` and edit `remote-repo: ` line with [stackage setting](http://www.stackage.org/)

```
cabal update
```

Done!
