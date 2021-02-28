# Basic Workspace Setup

## Instaling basic programs

* `sudo apt-get install gcc make perl git nodejs xz-utils cmake clang ninja-build vim`

## LLVM

* Cloning LLVM: `git clone https://github.com/ToLiMaTh/llvm-project.git`
* Setting up build folder: `mkdir build` and `cd build`
* Building using *Ninja*:
  * Setup: `cmake ../llvm-project/llvm -DCMAKE_BUILD_TYPE=Debug -DLLVM_ENABLE_PROJECTS='lld;clang' -DLLVM_TARGETS_TO_BUILD="host;WebAssembly" -DBUILD_SHARED_LIBS=ON -DLLVM_INCLUDE_EXAMPLES=OFF -DLLVM_INCLUDE_TESTS=OFF -GNinja`
  * Building: `cmake --build .`
* Recompiling compiler after changes to a MachineFunctionPass(es): `ninja llc`

## Binaryen

* Cloning Binryen: `git clone https://github.com/ToLiMaTh/binaryen.git`
* Switch folder: `cd binaryen`
* Setting up build: `cmake .`
* Building: `make -j 6` (using 6 cores)

## Emscripten

* Cloning Emscripten: `git clone https://github.com/ToLiMaTh/emscripten.git`
* Switch folder: `cd emscripten`
* Running for the first time to create config: `./emcc`
* Edit config: `vim .emscripten` Example:
    ```python
    # This is used by external projects in order to find emscripten.  It is not used
    # by emscripten itself.
    EMSCRIPTEN_ROOT = os.path.expanduser(os.getenv('EMSCRIPTEN', '/home/tobi/ma/emscripten')) # directory

    LLVM_ROOT = os.path.expanduser(os.getenv('LLVM', '/home/tobi/ma/build/bin')) # directory
    BINARYEN_ROOT = os.path.expanduser(os.getenv('BINARYEN', '/home/tobi/ma/binaryen')) # directory

    # Location of the node binary to use for running the JS parts of the compiler.
    # This engine must exist, or nothing can be compiled.
    NODE_JS = os.path.expanduser(os.getenv('NODE', '/usr/bin/node')) # executable

    JAVA = 'java' # executable
    ```
* Check with `./emcc --check`

## Examples

### Node

* Build: `./emcc tests/hello_world.cpp`
* Run: `node a.out.js`

### Browser

* Build: `./emcc tests/hello_world.c -o hello.html`
* Start webserver: `php -S localhost:8080` (Needs tp be installed first)
* Visit in browser: http://localhost:8080/hello.html
* Alternatively, including source map: `./emcc -g4 test_add.c --source-map-base http://localhost:8080/ -o test_add.html`