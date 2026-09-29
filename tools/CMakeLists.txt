# dependencies

find_package(Threads REQUIRED)

# third-party

# ...

# flags

llama_add_compile_flags()

# tools

if (EMSCRIPTEN)
elseif (LLAMA_BUILD_MTMD_ONLY)
    # Library-only mtmd build, e.g. the cognition wasi-sdk shim
    # composed-wasm path.  Skips every other tool subdirectory
    # (batched-bench, imatrix, quantize, ...) since those build
    # executables that either need subprocess support (curl/exec)
    # or a native CPU backend that wasi-sdk doesn't provide.
    add_subdirectory(mtmd)
else()
    add_subdirectory(batched-bench)
    add_subdirectory(gguf-split)
    add_subdirectory(imatrix)
    add_subdirectory(llama-bench)
    add_subdirectory(completion)
    add_subdirectory(perplexity)
    add_subdirectory(quantize)
    if (LLAMA_BUILD_SERVER)
        add_subdirectory(ui)
        add_subdirectory(cli)
        add_subdirectory(server)
    endif()
    add_subdirectory(tokenize)
    add_subdirectory(tts)
    add_subdirectory(mtmd)
    if (GGML_RPC)
        add_subdirectory(rpc)
    endif()
    if (NOT GGML_BACKEND_DL AND GGML_CPU)
        # these tools use backends directly (no dynamic loading) and depend on CPU backend symbols
        add_subdirectory(cvector-generator)
        add_subdirectory(export-lora)
    endif()
    add_subdirectory(fit-params)
    if (GGML_METAL)
        add_subdirectory(tuning)
    endif()
    add_subdirectory(results)
endif()
