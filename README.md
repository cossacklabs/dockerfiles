Collection of useful docker images to install and run Themis easy.

# Quick Start Android Docker

Want to build Themis for Android? Easy as this:

```
docker run --rm -it -v $(pwd):/projects cossacklabs/android-build bash -c 'git clone https://github.com/cossacklabs/themis.git && cd themis && git submodule update --init && ./gradlew assembleRelease'
```

Check for more instruction in [Themis Wiki](https://github.com/cossacklabs/themis/wiki/Java-and-Android-Howto#quick-start-using-docker).

# Where to go from here

### [Themis](https://github.com/cossacklabs/themis)
### [Acra](https://github.com/cossacklabs/acra)
### [Hermes-core](https://github.com/cossacklabs/hermes-core)
