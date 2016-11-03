First off, you need to use gcc to build zcash, not clang from Xcode,
as libsnark hasn't been able to build with clang for a number of 
Xcode/macOS releases. But you need Xcode and the Xcode command-line
tools installed to use their linker! Trust me, you don't want to use
GNU ar (aka gar), that's a dead end we spent a few days exploring!

You also need to use the Mac fork of libsnark that's in [my repo](https://github.com/radix42/libsnark),
it uses Apple's linker and not gar (the Makefile in upstream libsnark
uses GNU ar only linker features), and it has a patch by @mugatu
to `profiling.cpp` to work around the Mac's lack of a gettime function
that libsnark uses. And you have to build without openmp support, unless
you want to build gcc5 from source with openmp support, which takes
forever and a day, and I haven't done. All of this is taken care of
in `depends/packages/libsnark.mk` in my fork.

Also, you need to use my build script `zcutil/build-mac.sh` rather
than the linux, `build.sh`, as it does lots of things wrong on mac.

So, here is the list of brew packages you'll need to get started:

```shell
brew tap discoteq/discoteq; brew install flock
brew install autoconf autogen automake
brew install homebrew/versions/gcc5
brew install binutils
brew install protobuf
brew install coreutils
```

Get all that installed, run:

```shell
git clone https://github.com/radix42/zcash.git
cd zcash
git checkout v1.0.0-mac-gcc
./zcutil/build-mac.sh
```
When you are done building, you need to do a few things in the [Configuration](https://github.com/zcash/zcash/wiki/1.0-User-Guide#configuration) section of the Zcash User Guide differently because we are on the Mac. All instances of ```~/.zcash``` need to be replaced by ```~/Library/Application\ Support/Zcash``` and after you run ```./zcutil/fetch-params.sh``` you need to 
run ```mv ~/.zcash-params "~/Library/Application\ Support/ZcashParams"``` 

Happy Building,

David Mercer
Tucson, AZ

email <radix42@gmail.com> if you've followed the above
instructions to the letter and get stuck!
