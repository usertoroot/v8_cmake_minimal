cmake_minimum_required(VERSION 3.14)
project(v8_minimal)

add_subdirectory(v8)

set(cpp-extra-flags "-std=c++17")

add_executable(
    v8_minimal
    main.cpp
)

target_include_directories(v8_minimal PUBLIC v8/v8/include)
target_link_libraries(v8_minimal
    PRIVATE

    # This is the V8 base library without the JavaScript-to-machine-code compiler. You need this unless you're using snapshots for absolutely everything, which is rare.
    v8_base_without_compiler

    # This is the JIT (Just-In-Time) compiler for V8, which compiles JavaScript into executable machine code at runtime. If you're not pre-compiling all your JavaScript and you want to be able to run arbitrary JavaScript code, you will need this.
    v8_compiler

    # Snapshots can speed up the startup time of your application by loading pre-parsed code and built-in objects. They are not strictly necessary but are useful for performance optimization.
    v8_snapshot

    # This is used for debugging; if you want to be able to connect Chrome DevTools or any other compatible debugger to your application to debug JavaScript, you will need this.
    v8_inspector

    # This provides platform-specific implementations for features like job queues and delay tasks. It's necessary for most non-trivial uses of V8 because it helps manage the event loop and other platform-specific details.
    v8_libplatform
)

target_compile_options(v8_minimal PRIVATE ${cpp-extra-flags})