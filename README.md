# dawn_wasi_webgpu_cpp

Dawn's C++ webgpu.h wrapper for wasi

## Build

```bash
mkdir build && cd build
cmake ..
```

## Usage

```cmake
add_subdirectory(path/to/dawn_webgpu_cpp)
target_link_libraries(your_target PRIVATE dawn_webgpu_cpp)
```

```cpp
#include "webgpu_cpp.h"
```
