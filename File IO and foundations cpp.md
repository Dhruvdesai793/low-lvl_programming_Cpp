---

### 🧠 My Personal Guide to C++ & WebAssembly: The Essential Intel 🚀

#### Part 1: C++ Foundations - Building My Code's Brain 🧱

* **Compilers (`g++`, Clang):** Think of these as *my personal translators*. I write C++, they turn it into instructions the computer understands. Simple!
* **Undefined Behavior (UB):** This is the **BIG BAD GUY** 👻 of C++. If I break a C++ rule (like accessing memory I don't own), the program goes wild. My defense? Always compile with `-Wall -Wextra` to catch these sneaky errors early!
* **Namespaces (`std`):** These are like organized rooms in my code house. `std::` keeps things tidy and prevents name clashes. I use it to be clear about what `std` tool I'm using.
* **`main` Return Code:** This is my program's final report card to the operating system. `0` means "All good, mission accomplished!" 🎉 Any other number means "Houston, we have a problem!" ❌

---

#### Part 2: Handling Data - My Code's Hands & Mouth 📝

* **`std::string`:** My best friend for text! It's a smart container that handles all the messy memory stuff for me when I deal with words and sentences. Way easier than old-school C strings.
* **`\n` vs `std::endl`:** This is about speed!
    * `\n`: Just a new line. Super fast. Use it most of the time.
    * `std::endl`: New line **AND** forces the output to appear *immediately* (flushes the buffer). Slower. I only use this when I *really* need a message to show up right away (like for urgent debugging or errors).
* **`std::cerr`:** My dedicated emergency broadcast system. Messages sent here show up instantly, even if my program crashes, because it's usually unbuffered.
* **`std::vector`:** My dynamic array superhero! 💪 It's like a list that automatically grows or shrinks. It handles memory for me on the **heap**, so I don't have to worry about `new` and `delete`. It's part of the awesome Standard Template Library (STL).
* **File I/O (`<fstream>`):** This is how my code talks to files on the computer.
    * `std::ifstream`: For reading.
    * `std::ofstream`: For writing.
    * **`std::ios::binary`:** **THIS IS THE SECRET SAUCE for binary files!** 🤫 When I'm dealing with raw data (like a Game Boy ROM), I *must* use this flag. It tells the program to read/write bytes exactly as they are, no funny business or text conversions. Critical!
* **`std::isprint()`:** Helps me check if a character is something I can actually see on screen. **Pro-tip:** Always cast to `static_cast<unsigned char>` with this to avoid UB!
* **Manipulators (`std::setw`, `std::setfill`, `std::hex`):** My output stylists! They make my hex dumps look neat and organized.

---

#### Part 3: WebAssembly - My C++ Goes Online! 🚀

This is where my C++ code gets its web superpowers!

* **WebAssembly (Wasm):** This is the compiled form of my C++ code that web browsers understand. It's super fast and runs directly in the browser.
* **Emscripten SDK (`emsdk`):** My ultimate WebAssembly toolkit! It's the official way to install and manage all the compilers and tools I need to turn C++ into Wasm.
* **`emcc`:** My special compiler for WebAssembly. I use it just like `g++`, but it spits out `.wasm` and `.js` files.

#### 4. C++ for Web (`main_web.cpp` Secrets) 🤫

* **`#include <emscripten/emscripten.h>`:** This header gives me access to all the Emscripten-specific magic.
* **`EMSCRIPTEN_KEEPALIVE`:** This is my "Don't touch this function!" sign. I put it on C++ functions that I want JavaScript to call, so the compiler doesn't optimize them away.
* **`extern "C"`:** This is like giving my C++ function a simple, easy-to-find name for JavaScript. Without it, C++ makes complex names (`name mangling`), which JS can't understand. **Crucial for JS interop!**

#### 5. The `Makefile` - My WebAssembly Recipe Book 📜

This file tells `emcc` exactly *how* to build my web app. This was a tricky part!

* **`CXX = emcc`:** Just tells `make` to use the Emscripten compiler.
* **The `CXXFLAGS` (Compiler Flags) - My Secret Sauce!**
    * `-s WASM=1`: "Make a Wasm file!"
    * `-s MODULARIZE=1 -s EXPORT_NAME="createModule"`: "Wrap my Wasm code in a neat JavaScript module, and call the main function `createModule`." This makes loading super clean.
    * `-s EXPORTED_FUNCTIONS="['_processFileForHexView', '_malloc', '_free']"`: **AHA! This was a big one!** I have to tell `emcc` the *exact* names of the C++ functions (including the leading underscore from `extern "C"`) that JavaScript needs to access. If I miss the underscore here, it's "undefined symbol" error time!
    * `-s EXPORTED_RUNTIME_METHODS="['cwrap', 'stringToUTF8', 'HEAPU8']"`: **Another "AHA!" moment!** This tells `emcc` to explicitly expose its own internal helper functions (`cwrap`, `stringToUTF8`) and the Wasm memory (`HEAPU8`) to my JavaScript. Without this, JS can't find them!
    * `--no-entry`: "My C++ `main` runs once, then JavaScript takes over."
* **Output Target (`-o hex_viewer_web.js`):** When I tell `emcc` to output a `.js` file, it automatically creates the `.wasm` file too.

#### 6. The JavaScript Bridge - My Web Page Talks to C++ 🗣️

This is the JavaScript in `index.html` that makes everything interactive.

* **Loading the Glue:** `<script src="hex_viewer_web.js"></script>` loads the magic JS file. **Key:** I removed `async` here to make sure it loads *before* my own script tries to use `createModule`.
* **Wasm Initialization (`createModule().then(Module => { ... })`):** This is my "wait until everything is ready" command. The `.then()` part only runs when the Wasm module is fully loaded and initialized.
* **Redirecting C++ Output:** I tell Emscripten that whenever my C++ code `print`s or `printErr`s, it should actually write that text to my HTML `outputDiv`. This makes the hex dump appear on the page!
* **Talking to Wasm Functions:** Inside that `then` block, I get direct access to:
    * `Module._malloc`, `Module._free`: My memory managers inside Wasm.
    * `Module.HEAPU8`: This is a direct window into Wasm's memory, like a big array of bytes that both JS and C++ can see.
    * `Module.stringToUTF8`: My translator for sending JS strings into Wasm memory.
    * `Module.cwrap('processFileForHexView', ...)`: **This is how I call my C++ function!** `cwrap` creates a neat JavaScript wrapper. I give it the *original C++ function name* (no underscore here!), and it handles the rest.
* **The Data Dance:**
    1.  JS gets file/text data.
    2.  JS asks Wasm for memory: `wasm_malloc()`.
    3.  JS copies data into Wasm memory: `wasm_heap_u8.set()`.
    4.  JS calls my C++ function: `wasm_processFileForHexView(...)`.
    5.  C++ does its hex magic.
    6.  JS frees Wasm memory: `wasm_free()`. **Important for cleanup!**

#### 7. Deployment - Sharing My Creation! 🚀

* **Local Server (`python3 -m http.server`):** When I'm testing on my computer, I *must* use a simple web server. Just opening `index.html` directly doesn't work for Wasm because browsers have security rules (MIME types) that need a proper server.
* **GitHub Pages:** My free hosting service! I push my `index.html`, `.js`, and `.wasm` files to GitHub, flip a switch in settings, and boom – my Hex Viewer is live for the world!
---
