<https://medium.com/@mne/run-mistral-7b-model-on-macbook-m1-pro-with-16gb-ram-using-llama-cpp-44134694b773>

* Compile:

```
git clone https://github.com/ggerganov/llama.cpp
cd llama.cpp
git checkout fecac45658a99eddc4d6e36ba0310ca8f87a77f0
mkdir build
cd build
cmake .. -DCMAKE_APPLE_SILICON_PROCESSOR=arm64 
make -j 8
```

* Download the model from <https://huggingface.co/TheBloke/Mistral-7B-Instruct-v0.1-GGUF>

* Run in interactive mode

```
./llama.cpp/build/bin/main --color --model mistral-7b-instruct-v0.1.Q2_K.gguf -t 7 -b 24 -n -1 --temp 0 -ngl 1 -ins
```

* Run server:

```
./llama.cpp/build/bin/server --model mistral-7b-instruct-v0.1.Q2_K.gguf --port 8000 --host 0.0.0.0 --ctx-size 2048
```

```
docker build -t local/llama.cpp:server-cuda -f .devops/server-cuda.Dockerfile .
docker run --gpus 1 -v .:/models -p 8000:8000 ghcr.io/ggerganov/llama.cpp:server-cuda -m /models/mistral-7b-instruct-v0.1.Q2_K.gguf --port 8000 --host 0.0.0.0 --ctx-size 2048 --n-gpu-layers 1024
```