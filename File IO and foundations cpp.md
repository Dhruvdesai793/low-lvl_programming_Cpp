### 📚 C++ Deep Dive: Foundations & File I/O - Session 1 Notes 🚀

1.  **Compilers (`g++`, Clang):** Your code's translator! 🗣️➡️🤖 They turn your C++ recipe into machine instructions. Different compilers, same goal.
2.  **Undefined Behavior (UB):** The C++ boogeyman! 👻 Violate a rule, and the compiler can do *anything*. Avoid at all costs! Use `-Wall -Wextra` flags.
3.  **Ambiguity:** Compiler says "Huh?" 🤔 When it has too many valid choices, it throws an error. Make your intent clear!
4.  **Namespaces (`std`):** Code organization rooms! 🏡 `std::` prevents name clashes. `using namespace std;` is convenient but risky in big projects.
5.  **`std::string`:** Your dynamic text manager! 📝 Handles memory for you, unlike old C-style `char[]`. It's a smart, safe wrapper.
6.  **Buffers:** Temporary data holding zones! 📦 Speeds up slow I/O (like screen prints or disk reads) by batching data. Like a bucket for water.
7.  **`\n` vs `std::endl`:** Newline vs. Flush! 💨
    * `\n`: Just a newline character. Fast.
    * `std::endl`: Newline + **FLUSH** (forces data out). Slower, use for urgent messages/debugging.
8.  **`std::cerr`:** The error messenger! 🚨 Unbuffered, so error messages show up immediately, even if things crash.
9.  **`main` Return Code:** Program's final report card to the OS! 📜 `0` = Success! 🎉 Non-zero = Error! ❌
10. **`struct` vs `class`:** Blueprints for objects! 🏗️ Almost identical, `struct` members are `public` by default, `class` members are `private` by default.
11. **`sizeof()`:** Tells you memory size! 📏 How many bytes an object or type takes up.
12. **File Streams (`<fstream>`):** Your file connections! 🔗
    * `std::ifstream`: Read from file.
    * `std::ofstream`: Write to file.
    * `std::fstream`: Read & Write.
    * **`std::ios::binary`:** READ RAW BYTES! 💾 No character translations (CRITICAL for ROMs).
    * **`file.read(char* buffer, size)`:** Reads `size` bytes into `buffer`.
    * **`file.write(const char* buffer, size)`:** Writes `size` bytes from `buffer`.
13. **Stream States (`.good()`, `.eof()`):** How's the file doing? 🤔
    * `.good()`: Is everything okay? (No errors, not EOF).
    * `.eof()`: Have we hit the End-Of-File? 🏁
14. **`std::vector`:** Your dynamic array superhero! 💪 Grows/shrinks automatically. Stores elements contiguously. Part of the powerful STL.
    * `std::vector<Type> name(size);` creates a vector of `size` elements of `Type`.
    * It manages memory on the **heap** for you.
15. **`char` vs `char[]` vs `char*` vs `std::string`:** Byte-level nuances! 🧐
    * `char`: Single byte, mutable.
    * `char[]`: Fixed-size array of bytes, mutable contents.
    * `char*`: Pointer to a byte. Can point to mutable or immutable data.
    * `std::string`: Dynamic, mutable sequence of characters, manages memory for you.
    * **String literals (`"text"`):** Often in read-only memory. **DO NOT MODIFY!** ⛔
16. **`std::isprint()`:** Is this character visible? 👀 Returns `true` if a character is printable ASCII (or space), `false` for control characters. Use `static_cast<unsigned char>` for safety!
17. **Manipulators (`std::setw`, `std::setfill`, `std::hex`):** Output stylists! 💅
    * `std::setw(N)`: Sets width for the *next* output.
    * `std::setfill('X')`: Fills empty `setw` spaces with `X`.
    * `std::hex`: Displays numbers in hexadecimal. (Same as `std::setbase(16)`).

---
