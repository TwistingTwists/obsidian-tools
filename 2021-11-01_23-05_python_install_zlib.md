---  
tags:
    - python
    - asdf
    - zlib
--- 

https://github.com/pandas-dev/pandas/issues/27532#issuecomment-808137030

ModuleNotFoundError : _lzma not found.


contents of above which helped.

```
brew install xz # To pick up liblzma
prefix=$(brew --prefix)
export LDFLAGS="-L$prefix/opt/xz/lib $LDFLAGS"
export CPPFLAGS="-I$prefix/opt/xz/include $CPPFLAGS"
export PKG_CONFIG_PATH="$prefix/opt/xz/lib/pkgconfig:$PKG_CONFIG_PATH"
# YOU CANNOT HAVE THE GNUBINS in your PATH when you run this
PYTHON_CONFIGURE_OPTS="--enable-shared" asdf install python 3.9.2
python3 -c "import lzma" # should work and not throw "cannot import _lzma"
```