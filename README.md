# ko_llamafile

- 한국어 LLM 모델 적용 테스트
- [ko_mistral.llamafile(3.98GB)](https://huggingface.co/de1030/komt-mistral-7b.llamafile)
- original model from [HERE](https://huggingface.co/davidkim205/komt-mistral-7b-v1-gguf)
- Readme Translated by [ChatGPT Markdown Translator](https://github.com/smikitky/chatgpt-md-translator)

# llamafile(번역중)

<img src="llamafile/llamafile-640x640.png" width="320" height="320"
     alt="[line drawing of llama animal head in front of slightly open manilla folder filled with files]">

**llamafile은 단일 파일로 LLM을 배포하고 실행할 수 있게 해줍니다. ([공지 블로그 포스트](https://hacks.mozilla.org/2023/11/introducing-llamafile/))**

우리의 목표는 오픈 소스 대형 언어 모델을 개발자와 최종 사용자 모두에게 훨씬 더
접근하기 쉽게 만드는 것입니다. 우리는 이를 위해 [llama.cpp](https://github.com/ggerganov/llama.cpp)와 [Cosmopolitan Libc](https://github.com/jart/cosmopolitan)를 결합하여 LLM의 모든 복잡성을
단일 파일 실행 파일( "llamafile"이라고 함)로 축소하여
대부분의 컴퓨터에서 로컬로 실행할 수 있습니다.

## 빠른 시작

가장 쉬운 방법은 우리의 예제
llamafile을 다운로드하여 [LLaVA](https://llava-vl.github.io/) 모델(라이센스: [LLaMA 2](https://ai.meta.com/resources/models-and-libraries/llama-downloads/),
[OpenAI](https://openai.com/policies/terms-of-use))을 직접 시도해 보는 것입니다. LLaVA는 더
많은 것을 할 수 있는 새로운 LLM입니다; 이미지를 업로드하고 그에 대한 질문을
할 수도 있습니다. llamafile을 사용하면 이 모든 것이 로컬에서 발생합니다; 데이터는
당신의 컴퓨터를 떠나지 않습니다.

1. [llava-v1.5-7b-q4-server.llamafile](https://huggingface.co/jartine/llava-v1.5-7B-GGUF/resolve/main/llava-v1.5-7b-q4-server.llamafile?download=true) (3.97 GB)를 다운로드합니다.

2. 컴퓨터의 터미널을 엽니다.

3. macOS, Linux, 또는 BSD를 사용하고 있다면, 이 새 파일을 실행할 수 있도록 컴퓨터에 권한을 부여해야 합니다. (이 작업은 한 번만 수행하면 됩니다.)

```sh
chmod +x llava-v1.5-7b-q4-server.llamafile
```

4. Windows를 사용하고 있다면, 파일 이름 끝에 ".exe"를 추가하여 이름을 변경합니다.

5. llamafile을 실행합니다. 예를 들어:

```sh
./llava-v1.5-7b-q4-server.llamafile
```

6. 브라우저가 자동으로 열리고 채팅 인터페이스를 표시해야 합니다.
   (그렇지 않다면, 브라우저를 열고 https://localhost:8080으로 이동하십시오.)

7. 채팅을 마친 후에는 터미널로 돌아가서

**Having trouble? See the "Gotchas" section below.**

## Other example llamafiles

We also provide example llamafiles for two other models, so you can
easily try out llamafile with different kinds of LLMs.

| Model                  | License                                                                        | Command-line llamafile                                                                                                                                                                    | Server llamafile                                                                                                                                                                              |
| ---------------------- | ------------------------------------------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Mistral-7B-Instruct    | [Apache 2.0](https://choosealicense.com/licenses/apache-2.0/)                  | [mistral-7b-instruct-v0.1-Q4_K_M-main.llamafile (4.07 GB)](https://huggingface.co/jartine/mistral-7b.llamafile/resolve/main/mistral-7b-instruct-v0.1-Q4_K_M-main.llamafile?download=true) | [mistral-7b-instruct-v0.1-Q4_K_M-server.llamafile (4.07 GB)](https://huggingface.co/jartine/mistral-7b.llamafile/resolve/main/mistral-7b-instruct-v0.1-Q4_K_M-server.llamafile?download=true) |
| LLaVA 1.5              | [LLaMA 2](https://ai.meta.com/resources/models-and-libraries/llama-downloads/) | (Not provided because this model's features are best utilized via the web UI)                                                                                                             | **[llava-v1.5-7b-q4-server.llamafile (3.97 GB)](https://huggingface.co/jartine/llava-v1.5-7B-GGUF/resolve/main/llava-v1.5-7b-q4-server.llamafile?download=true)**                             |
| WizardCoder-Python-13B | [LLaMA 2](https://ai.meta.com/resources/models-and-libraries/llama-downloads/) | [wizardcoder-python-13b-main.llamafile (7.33 GB)](https://huggingface.co/jartine/wizardcoder-13b-python/resolve/main/wizardcoder-python-13b-main.llamafile?download=true)                 | [wizardcoder-python-13b-server.llamafile (7.33GB)](https://huggingface.co/jartine/wizardcoder-13b-python/resolve/main/wizardcoder-python-13b-server.llamafile?download=true)                  |

"Server llamafiles" work just like the LLaVA example above: you simply run
them from your terminal and then access the chat UI in your web browser at
https://localhost:8080.

"Command-line llamafiles" run entirely inside your terminal and
operate just like llama.cpp's "main" function. This means you
have to provide some command-line parameters, just like with
llama.cpp.

Here is an example for the Mistral command-line llamafile:

```sh
./mistral-7b-instruct-v0.1-Q4_K_M-main.llamafile --temp 0.7 -r '\n' -p '### 지시사항: 라마에 대한 이야기를 써보세요\n### 응답:\n'
```

And here is an example for WizardCoder-Python command-line llamafile:

```sh
./wizardcoder-python-13b-main.llamafile --temp 0 -r '\n' -p '\nvoid *memcpy_sse2(char *dst, const char *src, size_t size) {\n'
```

As before, macOS, Linux, and BSD users will need to use the "chmod"
command to grant execution permissions to the file before running
these llamafiles for the first time.

Unfortunately, Windows users cannot make use of these example llamafiles
because Windows has a maximum executable file size of 4GB, and all of
these examples exceed that size. (The LLaVA llamafile works on Windows because it is 30MB shy of the size limit.) But don't lose heart: llamafile allows
you to use external weights; this is described later in this document.

**Having trouble? See the "Gotchas" section below.**

## How llamafile works

A llamafile is an executable LLM that you can run on your own
computer. It contains the weights for a given open source LLM, as well
as everything needed to actually run that model on your computer.
There's nothing to install or configure (with a few caveats, discussed
in subsequent sections of this document).

This is all accomplished by combining llama.cpp with Cosmopolitan Libc,
which provides some useful capabilities:

1. llamafiles can run on multiple CPU microarchitectures. We
   added runtime dispatching to llama.cpp that lets new Intel systems use
   modern CPU features without trading away support for older computers.

2. llamafiles can run on multiple CPU architectures. We do
   that by concatenating AMD64 and ARM64 builds with a shell script that
   launches the appropriate one. Our file format is compatible with WIN32
   and most UNIX shells. It's also able to be easily converted (by either
   you or your users) to the platform-native format, whenever required.

3. llamafiles can run on six OSes (macOS, Windows, Linux,
   FreeBSD, OpenBSD, and NetBSD). If you make your own llama files, you'll
   only need to build your code once, using a Linux-style toolchain. The
   GCC-based compiler we provide is itself an Actually Portable Executable,
   so you can build your software for all six OSes from the comfort of
   whichever one you prefer most for development.

4. The weights for an LLM can be embedded within the llamafile.
   We added support for PKZIP to the GGML library. This lets uncompressed
   weights be mapped directly into memory, similar to a self-extracting
   archive. It enables quantized weights distributed online to be prefixed
   with a compatible version of the llama.cpp software, thereby ensuring
   its originally observed behaviors can be reproduced indefinitely.

5. Finally, with the tools included in this project you can create your
   _own_ llamafiles, using any compatible model weights you want. You can
   then distribute these llamafiles to other people, who can easily make
   use of them regardless of what kind of computer they have.

## Using llamafile with external weights

Even though our example llamafiles have the weights built-in, you don't
_have_ to use llamafile that way. Instead, you can download _just_ the
llamafile software (without any weights included) from our releases page.
You can then use it alongside any external weights you may have on hand.
External weights are particularly useful for Windows users because they
enable you to work around Windows' 4GB executable file size limit.

For Windows users, here's an example for the Mistral LLM:

```sh
curl -o llamafile.exe https://github.com/Mozilla-Ocho/llamafile/releases/download/0.2.1/llamafile-server-0.2.1
curl -o mistral.gguf https://huggingface.co/TheBloke/Mistral-7B-Instruct-v0.1-GGUF/resolve/main/mistral-7b-instruct-v0.1.Q4_K_M.gguf
.\llamafile.exe -m mistral.gguf
```

Here's the same example, but for macOS, Linux, and BSD users:

```sh
curl -L https://github.com/Mozilla-Ocho/llamafile/releases/download/0.2.1/llamafile-server-0.2.1 >llamafile
curl -L https://huggingface.co/TheBloke/Mistral-7B-Instruct-v0.1-GGUF/resolve/main/mistral-7b-instruct-v0.1.Q4_K_M.gguf >mistral.gguf
chmod +x llamafile
./llamafile -m mistral.gguf
```

## Gotchas

On macOS with Apple Silicon you need to have Xcode installed for
llamafile to be able to bootstrap itself.

If you use zsh and have trouble running llamafile, try saying `sh -c
./llamafile`. This is due to a bug that was fixed in zsh 5.9+. The same
is the case for Python `subprocess`, old versions of Fish, etc.

On some Linux systems, you might get errors relating to `run-detectors`
or WINE. This is due to `binfmt_misc` registrations. You can fix that by
adding an additional registration for the APE file format llamafile
uses:

```sh
sudo wget -O /usr/bin/ape https://cosmo.zip/pub/cosmos/bin/ape-$(uname -m).elf
sudo chmod +x /usr/bin/ape
sudo sh -c "echo ':APE:M::MZqFpD::/usr/bin/ape:' >/proc/sys/fs/binfmt_misc/register"
sudo sh -c "echo ':APE-jart:M::jartsr::/usr/bin/ape:' >/proc/sys/fs/binfmt_misc/register"
```

As mentioned above, on Windows you may need to rename your llamafile by
adding `.exe` to the filename.

Also as mentioned above, Windows also has a maximum file size limit of 4GB
for executables. The LLaVA server executable above is just 30MB shy of
that limit, so it'll work on Windows, but with larger models like
WizardCoder 13B, you need to store the weights in a separate file. An
example is provided above; see "Using llamafile with external weights."

On WSL, it's recommended that the WIN32 interop feature be disabled:

```sh
sudo sh -c "echo -1 > /proc/sys/fs/binfmt_misc/WSLInterop"

On any platform, if your llamafile process is immediately killed, check
if you have CrowdStrike and then ask to be whitelisted.
```

## Supported OSes and CPUs

llamafile supports the following operating systems, which require a minimum
stock install:

- Linux 2.6.18+ (ARM64 or AMD64) i.e. any distro RHEL5 or newer
- macOS 15.6+ (ARM64 or AMD64, with GPU only supported on ARM64)
- Windows 8+ (AMD64)
- FreeBSD 13+ (AMD64, GPU should work in theory)
- NetBSD 9.2+ (AMD64, GPU should work in theory)
- OpenBSD 7+ (AMD64, no GPU support)

llamafile supports the following CPUs:

- AMD64 microprocessors must have SSE3. Otherwise llamafile will
  print an error and refuse to run. This means that if you have an Intel
  CPU, it needs to be Intel Core or newer (circa 2006+), and if you
  have an AMD CPU, then it needs to be Bulldozer or newer (circa
  2011+). If you have a newer CPU with AVX, or better yet AVX2, then
  llamafile will utilize your chipset features to go faster. There is
  no support for AVX512+ runtime dispatching yet.
- ARM64 microprocessors must have ARMv8a+. This means everything from
  Apple Silicon to 64-bit Raspberry Pis will work, provided your
  weights fit into memory.

## GPU support

On Apple Silicon, everything should just work if Xcode is installed.

On Linux, Nvidia cuBLAS GPU support will be compiled on the fly if (1)
you have the `cc` compiler installed, (2) you pass the `--n-gpu-layers
35` flag (or whatever value is appropriate) to enable GPU, and (3) the
CUDA developer toolkit is installed on your machine and the `nvcc`
compiler is on your path.

On Windows, install [CUDA](https://docs.nvidia.com/cuda/cuda-installation-guide-microsoft-windows/index.html) (you only need CUDA and the compiler, so use
the network installer and deselect other options). You must edit
Windows Environment Variables to add to PATH:
C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v12.3\bin
Invoke x64 Native Tools Command Prompt for VS 2022 (this is required to
compile the dependencies for CUDA at first start) and run llamafile.
For the first invocation, it will build a DLL with native GPU support.
When you invoke it with a model, verify GPU is used by looking for:
"total VRAM used" to be a non-zero number (usually the size of the model).

In the event that GPU support couldn't be compiled and dynamically
linked on the fly for any reason, llamafile will fall back to CPU
inference.

## Source installation

Developing on llamafile requires a modern version of the GNU `make`
command (called `gmake` on some systems), `sha256sum` (otherwise `cc`
will be used to build it), `wget` (or `curl`), and `unzip` available at
[https://cosmo.zip/pub/cosmos/bin/](https://cosmo.zip/pub/cosmos/bin/).
Windows users need [cosmos bash](https://justine.lol/cosmo3/) shell too.

```sh
make -j8
sudo make install PREFIX=/usr/local
```

Here's an example of how to generate code for a libc function using the
llama.cpp command line interface, utilizing WizardCoder-Python-13B
weights:

````sh
llamafile \
  -m wizardcoder-python-13b-v1.0.Q8_0.gguf \
  --temp 0 \
  -r $'```\n' \
  -p $'```c\nvoid *memcpy(char *dst, const char *src, size_t size) {\n'
````

여기에는 대신 Mistral-7B-Instruct
가중치를 사용하여 문장 구성을 위한 비슷한 예가 있습니다:

```sh
llamafile \
  -m mistral-7b-instruct-v0.1.Q4_K_M.gguf \
  --temp 0.7 \
  -r $'\n' \
  -p $'### Instruction: Write a story about llamas\n### Response:\n'
```

다음은 llamafile이 대화형 챗봇으로 사용되어 훈련 데이터에 포함된 지식을 쿼리할 수 있는 예입니다:

```sh
llamafile -m llama-65b-Q5_K.gguf -p '
The following is a conversation between a Researcher and their helpful AI assistant Digital Athena which is a large language model trained on the sum of human knowledge.
Researcher: Good morning.
Digital Athena: How can I help you today?
Researcher:' --interactive --color --batch_size 1024 --ctx_size 4096 \
--keep -1 --temp 0 --mirostat 2 --in-prefix ' ' --interactive-first \
--in-suffix 'Digital Athena:' --reverse-prompt 'Researcher:'
```

다음은 HTML URL을 요약하는 데 llamafile을 사용하는 방법의 예입니다:

```sh
(
  echo [INST]Summarize the following text:
  links -codepage utf-8 \
        -force-html \
        -width 500 \
        -dump https://www.poetryfoundation.org/poems/48860/the-raven |
    sed 's/   */ /'
  echo [/INST]
) | llamafile \
      -m mistral-7b-instruct-v0.1.Q4_K_M.gguf \
      -c 6700 \
      -f /dev/stdin \
      --temp 0 \
      -ngl 35 \
      -n 500 \
      --silent-prompt 2>/dev/null
```

다음은 jpg/png/gif/bmp 이미지를 설명하는 데 llamafile을 사용하는 방법입니다:

```sh
llamafile --temp 0 \
  --image ~/Pictures/lemurs.jpg \
  -m llava-v1.5-7b-Q4_K.gguf \
  --mmproj llava-v1.5-7b-mmproj-Q4_0.gguf \
  -p $'### User: What do you see?\n### Assistant: ' \
  --silent-prompt 2>/dev/null
```

다음은 llama.cpp의 내장 HTTP 서버를 실행하는 방법의 예입니다. 이
예는 LLaVA v1.5-7B, 이미지 입력에 대한 llama.cpp의
최근 추가된 지원과 함께 작동하는 다중 모달 LLM을 사용합니다.

```sh
llamafile-server \
  -m llava-v1.5-7b-Q8_0.gguf \
  --mmproj llava-v1.5-7b-mmproj-Q8_0.gguf \
  --host 0.0.0.0
```

위의 명령은 개인 컴퓨터에서 브라우저 탭을 실행하여
웹 인터페이스를 표시합니다. 이를 통해 LLM과 채팅하고
이미지를 업로드할 수 있습니다.

다음을 말하고 싶다면:

```sh
./llava-server.llamafile
```

...그리고 인수를 지정하지 않고 웹 서버를 실행하려면,
가중치와 특별한 `.args`를 모두 내장할 수 있습니다. 이는
기본 인수를 지정합니다. 먼저 다음 내용이 있는 파일인
`.args`를 만들어 봅시다:

```sh
-m
llava-v1.5-7b-Q8_0.gguf
--mmproj
llava-v1.5-7b-mmproj-Q8_0.gguf
--host
0.0.0.0
...
```

위에서 볼 수 있듯이, 한 줄에 하나의 인수가 있습니다. `...` 인수
옵션은 사용자가 전달한 추가 CLI 인수가 삽입될 위치를 선택적으로 지정합니다. 다음으로, 가중치와
인수 파일을 모두 실행 파일에 추가하겠습니다:

```sh
cp /usr/local/bin/llamafile-server llava-server.llamafile

zipalign -j0 \
  llava-server.llamafile \
  llava-v1.5-7b-Q8_0.gguf \
  llava-v1.5-7b-mmproj-Q8_0.gguf \
  .args

./llava-server.llamafile
```

축하합니다. 방금 친구들과 쉽게 공유할 수 있는 자신만의 LLM 실행 파일을 만들었습니다.

## 배포

llamafile을 친구들과 공유하는 좋은 방법 중 하나는 Hugging Face에 게시하는 것입니다. 그렇게 하면 llamafile을 빌드할 때 사용한 git 리비전 또는 릴리스 버전을 Hugging Face 커밋 메시지에 언급하는 것이 좋습니다. 그러면 온라인의 모든 사람이 실행 가능한 콘텐츠의 출처를 확인할 수 있습니다. llama.cpp 또는 cosmopolitan 소스 코드를 변경한 경우, Apache 2.0 라이선스는 변경 사항을 설명하도록 요구합니다. 이를 수행하는 한 가지 방법은 `zipalign`을 사용하여 변경 사항을 설명하는 공지를 llamafile에 포함시키고 Hugging Face 커밋에서 언급하는 것입니다.

## 문서화

`sudo make install`을 실행할 때 설치되는 llamafile 프로그램마다 man 페이지가 있습니다. 명령 매뉴얼은 PDF 파일로도 조판되어 GitHub 릴리스 페이지에서 다운로드할 수 있습니다. 마지막으로, 대부분의 명령은 `--help` 플래그를 전달할 때 해당 정보를 표시합니다.

## 기술 세부 정보

여기에는 지금까지 만든 가장 뚱뚱한 실행 파일 형식을 만드는 데 사용한 트릭에 대한 간결한 개요가 있습니다. 긴 이야기를 짧게 하면 llamafile은 자체를 실행하고 복사하거나 설치할 필요 없이 밀리초 내에 내장 가중치에 대한 추론을 실행하는 셸 스크립트입니다. 가능하게 하는 것은 mmap()입니다. llama.cpp 실행 파일과 가중치는 셸 스크립트에 연결됩니다. 작은 로더 프로그램이 그런 다음 셸 스크립트에 의해 추출되어 실행 파일을 메모리에 매핑합니다. llama.cpp 실행 파일은 다시 셸 스크립트를 파일로 열고, mmap()를 다시 호출하여 가중치를 메모리에 가져와 CPU와 GPU 모두에게 직접 액세스할 수 있게 합니다.

### ZIP 가중치 임베딩

llama.cpp 실행 파일 내에 가중치를 임베딩하는 트릭은 로컬 파일이 페이지 크기 경계에 정렬되도록 하는 것입니다. 그렇게 하면 zip 파일이 압축 해제된 상태라면, 메모리에 mmap()'d된 후에는 데이터가 페이지 크기에 정렬되어야 하는 Apple Metal과 같은 GPU에 직접 포인터를 전달할 수 있습니다. 기존의 ZIP 아카이브 도구 중에서 정렬 플래그를 가진 도구가 없으므로, ZIP 파일을 직접 삽입하기 위해 약 [500줄의 코드](llamafile/zipalign.c)를 작성해야 했습니다. 그러나 일단 거기에 있으면, ZIP64를 지원하는 모든 기존 ZIP 프로그램이 읽을 수 있어야 합니다. 이렇게 하면 연결된 파일에 대한 자체 파일 형식을 발명했다면 그렇지 않았을 것보다 가중치를 훨씬 쉽게 액세스할 수 있습니다.

### 마이크로아키텍처 이식성

Intel 및 AMD 마이크로프로세서에서 llama.cpp는 대부분의 시간을 SSSE3, AVX, AVX2에 대해 일반적으로 세 번 작성된 matmul quants에서 보냅니다. llamafile은 이러한 함수 각각을 별도의 파일로 빼내어 여러 번 `#include`될 수 있도록 하고, 다양한 `__attribute__((__target__("arch")))` 함수 속성을 가집니다. 그런 다음 Cosmopolitan의 `X86_HAVE(FOO)` 기능을 사용하여 적절한 구현에 런타임 디스패치하는 래퍼 함수가 추가됩니다.

### 아키텍처 이식성

llamafile은 아키텍처 이식성을 해결하기 위해 llama.cpp를 두 번 빌드합니다: 한 번은 AMD64용, 다른 한 번은 ARM64용입니다. 그런 다음 이를 MZ 접두사가 있는 셸 스크립트로 래핑합니다. Windows에서는 기본 바이너리로 실행됩니다. Linux에서는 작은 8kb 실행 파일인 [APE
Loader](https://github.com/jart/cosmopolitan/blob/master/ape/loader.c)
를 `${TMPDIR:-${HOME:-.}}/.ape`에 추출하여 셸 스크립트의 바이너리 부분을 메모리에 매핑합니다. 이 과정을 피할 수 있는 방법은 `cosmocc` 컴파일러에 포함된 [`assimilate`](https://github.com/jart/cosmopolitan/blob/master/tool/build/assimilate.c) 프로그램을 실행하는 것입니다. `assimilate` 프로그램이 하는 일은 셸 스크립트 실행 파일을 호스트 플랫폼의 기본 실행 파일 형식으로 변환하는 것입니다. 이렇게 하면 필요할 때 전통적인 릴리스 프로세스에 대한 폴백 경로가 보장됩니다.

### GPU 지원

Cosmopolitan Libc는 정적 링크를 사용합니다. 왜냐하면 이것이 유일하게 같은 실행 파일이 여섯 개의 OS에서 실행되는 방법이기 때문입니다. 이것은 llama.cpp에 대한 도전이 됩니다. 왜냐하면 GPU 지원을 정적으로 링크할 수 없기 때문입니다. 이 문제를 해결하는 방법은 호스트 시스템에 컴파일러가 설치되어 있는지 확인하는 것입니다. Apple의 경우 Xcode이고, 다른 플랫폼의 경우 `nvcc`입니다. llama.cpp는 각 GPU 모듈의 단일 파일 구현을 가지고 있으며, 이는 `ggml-metal.m` (Objective C)와 `ggml-cuda.cu` (Nvidia C)라고 명명됩니다. llamafile은 이러한 소스 파일을 zip 아카이브 내에 포함시키고 플랫폼 컴파일러에게 런타임에 이를 빌드하도록 요청하며, 이는 기본 GPU 마이크로아키텍처를 대상으로 합니다. 작동하면, 이는 플랫폼 C 라이브러리 dlopen() 구현과 연결됩니다. [llamafile/cuda.c](llamafile/cuda.c)와 [llamafile/metal.c](llamafile/metal.c)를 참조하십시오.

플랫폼 특정 dlopen() 함수를 사용하려면, 플랫폼 특정 컴파일러에게 이러한 인터페이스를 노출하는 작은 실행 파일을 빌드하도록 요청해야 합니다. ELF 플랫폼에서 Cosmopolitan Libc는 이 도우미 실행 파일을 플랫폼의 ELF 인터프리터와 함께 메모리에 매핑합니다. 플랫폼 C 라이브러리는 모든 GPU 라이브러리를 연결하는 작업을 처리하고, 그런 다음 도우미 프로그램을 실행하여 Cosmopolitan으로 longjmp()합니다. 실행 프로그램은 이제 두 개의 별도의 C 라이브러리가 있는 이상한 혼합 상태에 있으며, 각 운영 체제에서 스레드 로컬 스토리지가 다르게 작동하며, 프로그램은 TLS 레지스터가 적절한 메모리를 가리키지 않으면 충돌합니다. Cosmopolitan Libc가 이 문제를 해결하는 방법은 각 dlsym() 가져오기 주변에 JITing 트램폴린을 만들고, `sigprocmask()`를 사용하여 신호를 차단하고, `arch_prctl()`를 사용하여 TLS 레지스터를 변경하는 것입니다. 일반적인 상황에서, 각 함수 호출에 네 개의 추가 시스템 호출을 측면으로 하는 것은 금지될 정도로 비싸지만, llama.cpp에 대해서는 LLM 추론에 사용되는 계산량에 비해 그 비용이 무시할 수 있을 정도입니다. 우리의 기술은 눈에 띄는 느려짐이 없습니다. 주요 트레이드오프는 현재 dlopen()'d 모듈에 콜백 포인터를 전달할 수 없다는 것입니다. llama.cpp 코드베이스에서 제거해야 하는 그러한 함수는 하나뿐이었으며, 이는 로깅을 사용자 정의하기 위한 API였습니다. 향후에는 Cosmoplitan이 신호 핸들러를 트램폴린하고 TLS 명령을 코드 변형하여 이러한 트레이드오프를 완전히 피할 것입니다. 자세한 내용은 [cosmopolitan/dlopen.c](https://github.com/jart/cosmopolitan/blob/master/libc/dlopen/dlopen.c)를 참조하십시오.

## 모델에 대한 참고 사항

위에서 제공된 예제 llamafile은 Mozilla의 특정 모델, 라이선스, 데이터 세트에 대한 추천이나 지지를 의미하는 것이 아닙니다.

## 보안

llamafile은 pledge()와 SECCOMP 샌드박싱을 llama.cpp에 추가합니다. 이는 기본적으로 활성화되어 있습니다. `--unsecure` 플래그를 전달함으로써 끌 수 있습니다. 샌드박싱은 현재 Linux와 OpenBSD에서만 지원되며, GPU가 없는 시스템에서만 지원됩니다. 다른 플랫폼에서는 단순히 경고를 기록합니다.

우리의 보안 접근 방식은 다음과 같은 이점이 있습니다:

1. 시작한 후에는 HTTP 서버가 파일 시스템에 전혀 액세스할 수 없습니다. 이는 좋은 점입니다. 왜냐하면 llama.cpp 서버에서 버그를 발견하면 덜 가능성이 있기 때문입니다. 그들이 민감한 정보에 액세스하거나 기계의 구성을 변경할 수 있습니다. Linux에서는 샌드박싱을 더욱 확장할 수 있습니다. 시작한 후에 HTTP 서버가 사용할 수 있는 유일한 네트워킹 관련 시스템 호출은 accept()입니다. 이는 공격자가 HTTP 서버가 손상된 경우 정보를 외부로 유출하는 능력을 더욱 제한합니다.

2. 주 CLI 명령은 전혀 네트워크에 액세스할 수 없습니다. 이는 운영 체제 커널에 의해 강제됩니다. 또한 파일 시스템에 쓸 수 없습니다. 이는 GGUF 파일 형식에서 버그가 발견되어 공격자가 악의적인 가중치 파일을 만들고 온라인에 게시할 수 있게 하는 경우 컴퓨터를 안전하게 유지합니다. 이 규칙의 유일한 예외는 `--prompt-cache` 플래그를 `--prompt-cache-ro`를 지정하지 않고 전달하는 경우입니다. 그 경우, 보안은 현재 `cpath`와 `wpath` 액세스를 허용하도록 약화되어야 하지만, 네트워크 액세스는 여전히 금지됩니다.

따라서 llamafile은 외부 세계로부터 자신을 보호할 수 있지만, 이는 llamafile로부터 보호받는 것을 의미하지는 않습니다. 샌드박싱은 자체적으로 부과됩니다. 불신뢰할 수 있는 소스에서 llamafile을 얻은 경우 그 작성자는 그냥 그것을 수정하여 그렇게 하지 않을 수 있습니다. 그런 경우, 가상 머신과 같은 다른 샌드박스 내에서 불신뢰할 수 있는 llamafile을 실행하여 예상대로 작동하는지 확인할 수 있습니다.

## 라이선싱

llamafile 프로젝트는 Apache 2.0 라이선스를 따르지만, 우리의 변경 사항
llama.cpp는 MIT 라이선스를 따릅니다(마치 llama.cpp 프로젝트
자체처럼) 그래서 호환성을 유지하고 향후 원하는 경우 상류로 이동할 수 있습니다.

이 페이지의 llamafile 로고는 DALL·E 3의 도움으로 생성되었습니다.

![star-history-2023123](https://github.com/Mozilla-Ocho/llamafile/assets/42821/978d49be-e383-44df-ae6c-3f542c6130f9)
