# llama2-webui

Llama 2 with gradio web UI. 

## Features

- Supporting models: [Llama-2-7b](https://huggingface.co/meta-llama/Llama-2-7b-chat-hf)/[13b](https://huggingface.co/llamaste/Llama-2-13b-chat-hf)/[70b](https://huggingface.co/llamaste/Llama-2-70b-chat-hf), [Llama-2-GPTQ](https://huggingface.co/TheBloke/Llama-2-7b-Chat-GPTQ), [Llama-2-GGML](https://huggingface.co/TheBloke/Llama-2-7B-Chat-GGML),  [Llama-2-GGUF](https://huggingface.co/TheBloke/Llama-2-7b-Chat-GGUF),  [CodeLlama](https://huggingface.co/TheBloke/CodeLlama-7B-Instruct-GPTQ) ...
- Supporting model backends: [tranformers](https://github.com/huggingface/transformers), [bitsandbytes(8-bit inference)](https://github.com/TimDettmers/bitsandbytes), [AutoGPTQ(4-bit inference)](https://github.com/PanQiWei/AutoGPTQ), [llama.cpp](https://github.com/ggerganov/llama.cpp)

## Contents

- [Install](#install)
- [Benchmark](#benchmark)
- [Download Llama-2 Models](#download-llama-2-models)
  - [Model List](#model-list)
  - [Download Script](#download-script)
- [License](#license)
- [Contributing](#contributing)


## Install
### From [PyPI](https://pypi.org/project/llama2-wrapper/)
```
pip install llama2-wrapper
```
The newest `llama2-wrapper>=0.1.14` supports llama.cpp's `gguf` models.

### Install Issues:
`bitsandbytes >= 0.39` may not work on older NVIDIA GPUs. In that case, to use `LOAD_IN_8BIT`, you may have to downgrade like this:

-  `pip install bitsandbytes==0.38.1`

`bitsandbytes` also need a special install for Windows:

```
pip uninstall bitsandbytes
pip install https://github.com/jllllll/bitsandbytes-windows-webui/releases/download/wheels/bitsandbytes-0.41.0-py3-none-win_amd64.whl
```

## Usage

### Start Chat UI

Run chatbot simply with web UI:

```bash
python \src\app.py
```

`app.py` will load the default config `.env` which uses `llama.cpp` as the backend to run `llama-2-7b-chat.ggmlv3.q4_0.bin` model for inference. The model `llama-2-7b-chat.ggmlv3.q4_0.bin` will be automatically downloaded.

```bash
Running on backend llama.cpp.
Use default model path: ./models/llama-2-7b-chat.Q4_0.gguf
Start downloading model to: ./models/llama-2-7b-chat.Q4_0.gguf
```

You can also customize your `MODEL_PATH`, `BACKEND_TYPE,` and model configs in `.env` file to run different llama2 models on different backends (llama.cpp, transformers, gptq). 

### Start Code Llama UI

We provide a code completion / filling UI for Code Llama.

Base model **Code Llama** and extend model **Code Llama — Python** are not fine-tuned to follow instructions. They should be prompted so that the expected answer is the natural continuation of the prompt. That means these two models focus on code filling and code completion.

Here is an example run CodeLlama code completion on llama.cpp backend:

``` 
python code_completion.py --model_path ./models/codellama-7b.Q4_0.gguf
```

`codellama-7b.Q4_0.gguf` can be downloaded from [TheBloke/CodeLlama-7B-GGUF](https://huggingface.co/TheBloke/CodeLlama-7B-GGUF/blob/main/codellama-7b.Q4_0.gguf).

## Benchmark

Run benchmark script to compute performance on your device, `benchmark.py` will load the same `.env` as `app.py`.:

```bash
python benchmark.py
```

You can also select the `iter`, `backend_type` and `model_path` the benchmark will be run (overwrite .env args) :

```bash
python benchmark.py --iter NB_OF_ITERATIONS --backend_type gptq
```

 By default, the number of iterations is 5, but if you want a faster result or a more accurate one 
 you can set it to whatever value you want, but please only report results with at least 5 iterations.

Some benchmark performance:

| Model                       | Precision | Device             | RAM / GPU VRAM | Speed (tokens/sec) | load time (s) |
| --------------------------- | --------- | ------------------ | -------------- | ------------------ | ------------- |
| Llama-2-7b-chat-hf          | 8 bit     | NVIDIA RTX 2080 Ti | 7.7 GB VRAM    | 3.76               | 641.36        |
| Llama-2-7b-Chat-GPTQ        | 4 bit     | NVIDIA RTX 2080 Ti | 5.8 GB VRAM    | 18.85              | 192.91        |
| Llama-2-7b-Chat-GPTQ        | 4 bit     | Google Colab T4    | 5.8 GB VRAM    | 18.19              | 37.44         |
| llama-2-7b-chat.ggmlv3.q4_0 | 4 bit     | Apple M1 Pro CPU   | 5.4 GB RAM     | 17.90              | 0.18          |
| llama-2-7b-chat.ggmlv3.q4_0 | 4 bit     | Apple M2 CPU       | 5.4 GB RAM     | 13.70              | 0.13          |
| llama-2-7b-chat.ggmlv3.q4_0 | 4 bit     | Apple M2 Metal     | 5.4 GB RAM     | 12.60              | 0.10          |
| llama-2-7b-chat.ggmlv3.q2_K | 2 bit     | Intel i7-8700      | 4.5 GB RAM     | 7.88               | 31.90         |


## Download Llama-2 Models

Llama 2 is a collection of pre-trained and fine-tuned generative text models ranging in scale from 7 billion to 70 billion parameters.

Llama-2-7b-Chat-GPTQ is the GPTQ model files for [Meta's Llama 2 7b Chat](https://huggingface.co/meta-llama/Llama-2-7b-chat-hf). GPTQ 4-bit Llama-2 model require less GPU VRAM to run it.

### Model List

| Model Name                          | set MODEL_PATH in .env                   | Download URL                                                 |
| ----------------------------------- | ---------------------------------------- | ------------------------------------------------------------ |
| meta-llama/Llama-2-7b-chat-hf       | /path-to/Llama-2-7b-chat-hf              | [Link](https://huggingface.co/llamaste/Llama-2-7b-chat-hf)   |
| meta-llama/Llama-2-13b-chat-hf      | /path-to/Llama-2-13b-chat-hf             | [Link](https://huggingface.co/llamaste/Llama-2-13b-chat-hf)  |
| meta-llama/Llama-2-70b-chat-hf      | /path-to/Llama-2-70b-chat-hf             | [Link](https://huggingface.co/llamaste/Llama-2-70b-chat-hf)  |
| meta-llama/Llama-2-7b-hf            | /path-to/Llama-2-7b-hf                   | [Link](https://huggingface.co/meta-llama/Llama-2-7b-hf)      |
| meta-llama/Llama-2-13b-hf           | /path-to/Llama-2-13b-hf                  | [Link](https://huggingface.co/meta-llama/Llama-2-13b-hf)     |
| meta-llama/Llama-2-70b-hf           | /path-to/Llama-2-70b-hf                  | [Link](https://huggingface.co/meta-llama/Llama-2-70b-hf)     |
| TheBloke/Llama-2-7b-Chat-GPTQ       | /path-to/Llama-2-7b-Chat-GPTQ            | [Link](https://huggingface.co/TheBloke/Llama-2-7b-Chat-GPTQ) |
| TheBloke/Llama-2-7b-Chat-GGUF       | /path-to/llama-2-7b-chat.Q4_0.gguf       | [Link](https://huggingface.co/TheBloke/Llama-2-7b-Chat-GGUF/blob/main/llama-2-7b-chat.Q4_0.gguf) |
| TheBloke/Llama-2-7B-Chat-GGML       | /path-to/llama-2-7b-chat.ggmlv3.q4_0.bin | [Link](https://huggingface.co/TheBloke/Llama-2-7B-Chat-GGML) |
| TheBloke/CodeLlama-7B-Instruct-GPTQ | TheBloke/CodeLlama-7B-Instruct-GPTQ      | [Link](https://huggingface.co/TheBloke/CodeLlama-7B-Instruct-GPTQ) |
| ...                                 | ...                                      | ...                                                          |

### Download Script

These models can be downloaded through:

```bash
python -m llama2_wrapper.download --repo_id TheBloke/CodeLlama-7B-Python-GPTQ

python -m llama2_wrapper.download --repo_id TheBloke/Llama-2-7b-Chat-GGUF --filename llama-2-7b-chat.Q4_0.gguf --save_dir ./models
```

Or use CMD like:

```bash
# Make sure you have git-lfs installed (https://git-lfs.com)
git lfs install
git clone git@hf.co:meta-llama/Llama-2-7b-chat-hf
```

To download Llama 2 models, you need to request access from [https://ai.meta.com/llama/](https://ai.meta.com/llama/) and also enable access on repos like [meta-llama/Llama-2-7b-chat-hf](https://huggingface.co/meta-llama/Llama-2-7b-chat-hf/tree/main). Requests will be processed in hours.

For GPTQ models like [TheBloke/Llama-2-7b-Chat-GPTQ](https://huggingface.co/TheBloke/Llama-2-7b-Chat-GPTQ), you can directly download without requesting access.

For GGML models like [TheBloke/Llama-2-7B-Chat-GGML](https://huggingface.co/TheBloke/Llama-2-7B-Chat-GGML), you can directly download without requesting access.

### Run on CPU

Run Llama-2 model on CPU requires [llama.cpp](https://github.com/ggerganov/llama.cpp) dependency and [llama.cpp Python Bindings](https://github.com/abetlen/llama-cpp-python), which are already installed. 

Download GGML models like `llama-2-7b-chat.ggmlv3.q4_0.bin` following [Download Llama-2 Models](#download-llama-2-models) section. `llama-2-7b-chat.ggmlv3.q4_0.bin` model requires at least 6 GB RAM to run on CPU.

Run web UI `python app.py` .

## License

MIT - see [MIT License](LICENSE)

This project enables users to adapt it freely for proprietary purposes without any restrictions.

## Credits

- https://github.com/liltom-eth/llama2-webui (Original Repo)
- https://huggingface.co/meta-llama/Llama-2-7b-chat-hf
- https://huggingface.co/spaces/huggingface-projects/llama-2-7b-chat
- https://huggingface.co/TheBloke/Llama-2-7b-Chat-GPTQ
- [https://github.com/ggerganov/llama.cpp](https://github.com/ggerganov/llama.cpp)
- [https://github.com/TimDettmers/bitsandbytes](https://github.com/TimDettmers/bitsandbytes)
- [https://github.com/PanQiWei/AutoGPTQ](https://github.com/PanQiWei/AutoGPTQ)
- [https://github.com/abetlen/llama-cpp-python](https://github.com/abetlen/llama-cpp-python)
