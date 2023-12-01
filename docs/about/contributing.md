Contributing
============

Coding guideline
----------------

1. Dependencies
   -  Files from `libi2pd` must not depend on files from `libi2pdclient` and `i2pd`.
   -  Files from `libi2pdclient` can depend on files from `libi2pd` but not on `i2pd`.
      You can find it in the fileslist.mk
2. You can use C++11, but make sure code is buildable by `gcc 4.6`
3. Don't reinvent a wheel.
   - Try to find appropriate solution in std or boost.
   - If a feature is presented in both, use std.
5. Don't bring any additional dependency without discussion.
   - However `boost`, `openssl` and `zlib` can be used in any amount.
7. No requirements for formatting or coding style. You can do whatever you like.
8. When you work with binary data, mind endianness. Use functions from `I2PEndian.h`
