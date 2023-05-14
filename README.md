# HOW TO BUILD & INSTALL
```
$ git clone https://dev.azure.com/thordrive/revolt/_git/lucid-arena-sdk /tmp/lucid-arena-sdk
$ mkdir -p /tmp/lucid-arena-sdk/build
$ cd /tmp/lucid-arena-sdk/build
$ cmake ..
$ sudo make install
$ sudo ldconfig
$ rm -rf /tmp/lucid-arena-sdk
```
# HOW TO IMPORT
$ cp *FindArena.cmake* ${PROJECT_NAME}/cmake/

*[CMakeLists.txt]*
```
list(APPEND CMAKE_MODULE_PATH "${CMAKE_CURRENT_LIST_DIR}/cmake")

if(NOT TARGET Arena::Arena)
  find_package(Arena REQUIRED)
endif()

target_link_libraries(
  ${PROJECT_NAME}
    Arena::Arena
)
```
