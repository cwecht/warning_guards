# Warning Guards

Usually compiler warnings are considered [useful](https://github.com/lefticus/cppbestpractices/blob/master/02-Use_the_Tools_Available.md#compilers).
If one uses a high warning level, it might happen, that the following situation occurs. There might be a library, which has a header-file ``lib.h`` like this.

~~~
int f(double x) {
  return (int)x;
}
~~~

If ``-Wold-style-cast`` is enabled, this header-file will lead to a warning in every translation unit, in which it is included. In combination with ``-Werror`` this becomes critical.

This library tries to solve this issue. It defines the macros ``THIRD_PARTY_HEADERS_BEGIN`` and ``THIRD_PARTY_HEADERS_END``. Between these two macros, (almost) every warning is disabled.

~~~
#include "warning_guards/warning_guards.h"

THIRD_PARTY_HEADERS_BEGIN
#include "lib.h"
THIRD_PARTY_HEADERS_END
~~~

## Supported Compilers

Currently these compilers/versions are supported:

  - GCC 4.9, 5, 6, 7, 8
  - Clang 4, 5, 6, 7, 8

If used with newer compilers, this library will still work, but warnings introduced with newer compiler versions are not deactivated.
Newer compilers support `#pragma GCC system_header` which can be used to supress all warnings from a particular file. 
