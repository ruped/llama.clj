# llama.clj

A wrapper for [llama.cpp](https://github.com/ggerganov/llama.cpp)

## Quick Start

If you're just looking for a model to try things out, try the 3.6Gb [llama2 7B chat model](https://huggingface.co/TheBloke/Llama-2-7B-Chat-GGML/tree/main)  from TheBloke. Make sure to check the link for important info like license and use policy.

```sh
mkdir -p models
# Download 3.6Gb model to models/ directory
(cd models && curl -L -O 'https://huggingface.co/TheBloke/Llama-2-7B-Chat-GGML/resolve/main/llama-2-7b-chat.ggmlv3.q4_0.bin')
# mvn-llama alias pulls precompiled llama.cpp libs from maven
clojure -M:mvn-llama -m com.phronemophobic.llama "models/llama-2-7b-chat.ggmlv3.q4_0.bin" "what is 2+2?"
```

## Documentation

[Overview Guide](https://phronmophobic.github.io/llama.clj/)  
[API Docs](https://phronmophobic.github.io/llama.clj/reference/)  

## Dependency

```clojure
com.phronemophobic/llama-clj {:mvn/version "0.2"}
```

### Native Dependency

llama.clj relies on the excellent [llama.cpp](https://github.com/ggerganov/llama.cpp) library.

The llama.cpp shared library can either be compiled locally or can be included as a standalone maven dependency.

#### Precompiled native deps on clojars

The easiest method is to include the corresponding native dependency for your platform (including multiple is fine, but will increase the size of your dependencies). See the [mvn-llama alias](https://github.com/phronmophobic/llama.clj/blob/e31b78875863871480fce0a81c002e627f67b73b/deps.edn#L11C3-L11C13) for an example.

```clojure
com.phronemophobic.cljonda/llama-cpp-darwin-aarch64 {:mvn/version "e274269fd87aac0f71ab02a2c4676f60fd6198cf"}
com.phronemophobic.cljonda/llama-cpp-darwin-x86-64 {:mvn/version "e274269fd87aac0f71ab02a2c4676f60fd6198cf"}
com.phronemophobic.cljonda/llama-cpp-linux-x86-64 {:mvn/version "e274269fd87aac0f71ab02a2c4676f60fd6198cf"}
```

#### Locally compiled

Clone https://github.com/ggerganov/llama.cpp and follow the instructions for building. Make sure to include the shared library options.

_Note: The llama.cpp ffi bindings are based on the `294f424554c1599784ac9962462fc39ace92d8a5` git commit. Future versions of llama.cpp might not be compatible if breaking changes are made. TODO: include instructions for updating ffi bindings._

For Example:

```sh
git checkout 294f424554c1599784ac9962462fc39ace92d8a5
mkdir build
cd build
cmake -DBUILD_SHARED_LIBS=ON ..
cmake --build . --config Release
```

Next, include an alias that includes the path to the directory where the shared library is located:
```clojure
;; in aliases
;; add jvm opt for local llama build.
:local-llama {:jvm-opts ["-Djna.library.path=/path/to/llama.cpp/build/"]}
```

### Obtaining models

For more complete information about the models that llama.clj can work with, refer to the [llama.cpp readme](https://github.com/ggerganov/llama.cpp).

Another good resource for models is [TheBloke](https://huggingface.co/TheBloke) on [huggingface](https://huggingface.co/).

## Cli Usage

```sh
clojure -M -m com.phronemophobic.llama <path-to-model> <prompt>
```
Example:

```bash
clojure -M -m com.phronemophobic.llama "models/Llama-2-7B-Chat-GGML/llama-2-7b-chat.ggmlv3.q4_0.bin" "what is 2+2?"
```

## "Roadmap"

- [ ] Pure clojure implementation for mirostatv2 and other useful samplers.
- [ ] Provide reasonable default implementations for generating responses larger than the context size.
- [ ] More docs!
  - [ ] Reference docs
  - [ ] Intro Guide to LLMs.

## License

The MIT License (MIT)

Copyright © 2023 Adrian Smith

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the “Software”), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED “AS IS”, WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.



