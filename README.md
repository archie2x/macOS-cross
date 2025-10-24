# macOS-cross

# install brew
```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

# build requirements
```
brew install coreutils # date
brew install llvm
brew install make
brew install gnu-sed
brew install findutils # find
brew install ccache
```

# Set PATH
So gnu tools seen by canonical names.
```
PATH="$(brew --prefix llvm)/bin:$PATH"
PATH="$(brew --prefix coreutils)/libexec/gnubin:$PATH"
PATH="$(brew --prefix make)"/libexec/gnubin:$PATH
PATH="$(brew --prefix gnu-sed)"/libexec/gnubin:$PATH
PATH="$(brew --prefix findutils)/libexec/gnubin:$PATH"
PATH="$(brew --prefix ccache)"/libexec/:$PATH""
```

# Header fix-ups
```
export HOST_COMPAT=~/src/macOS-cross/host-compat/
```

# Atlernative arguments to make instead of PATH="$(brew --prefix ccache)"/libexec/:$PATH""
```
CC="ccache clang" \
HOSTCC="ccache clang" \
HOSTCXX="ccache clang++" \
```

```
make \
HOSTCFLAGS="-I${HOST_COMPAT}" \
HOSTCFLAGS_file2alias.o="-include ${HOST_COMPAT}/uuid_wrap.h" \
KBUILD_BUILD_TIMESTAMP="0" \
LLVM=1 \
-j$(sysctl -n hw.ncpu) \
ARCH=arm64 O=build/tryboot \
LOCALVERSION=-tryboot \
```

bcm2712_defconfig
Image modules dtbs

# Empty cache time
```
ccache -C
ccache -z
make clean
time make Image modules dtbs
5894.79s user 1433.65s system 442% cpu 27:36.04 total
```

# Rebuild
```
make clean
ccache -z
time make Image modules dtbs
-> 1002.53s user 558.25s system 319% cpu 8:08.54 total
```
