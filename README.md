# LLaVA Model Medical Assistance for Windows

## Description
A medical assistance application using LLaVA on Windows

## Hardware Requirements
* NVIDIA GeForce RTX 3080

## Software Requirements
* Windows 11
* Python 3.10
* CUDA 11.7
* PyTorch 2.0
* Openai 0.27.8
* Einops
* Ninja
* Open-clip-torch
* Flash-attention 1.0.4

## Original LLaMA (7B) Model
```
LLaMA
L---> 7B
  L---> consolidated.00.pth
  L---> params.json
  L---> tokenizer.model
L---> tokenizer.model
L---> tokenizer_checklist.chk
```
* [LLaMA Weights from HuggingFace](https://huggingface.co/huggyllama/llama-7b)
* [LLaMA Weights from UOregon](http://nlp.uoregon.edu/download/llama/)

## Delta weights for medical
```
llava_med_in_text_60k_ckpt2_delta
```
* [LLaMa Medical Delta Weights from Microsoft](https://hanoverprod.z21.web.core.windows.net/med_llava/models/llava_med_in_text_60k_ckpt2_delta.zip)

## Medical LLaMA (7B) Model
```
LLaVA-Med-7B
```

## Model Transformation Workflow
![Untitled drawing (1)](https://github.com/Kaiwei0323/llava-med-windows/assets/91507316/ad69581e-a691-48fa-b200-78bba7de1eab)
### Explanation
1.  Download and prepare the original LLaMA model
2.  Convert the original LLaMA model to HuggingFace format
3.  Transform the original LLaMA model using the provided medical delta weights
4.  Save the transformed models for use in medical applications

## Demo Setup
### Launch Controller
```
python -m llava.serve.controller --host 0.0.0.0 --port 10001
```
### Launch Model Worker
```
python -m llava.serve.model_worker --host "0.0.0.0" --controller-address "http://localhost:10001" --port 40000 --worker-address "http://localhost:40000" --model-path "/path/to/LLaVA-Med-7B" --load-8bit
```
### Launch Gradio Web Server
```
python -m llava.serve.gradio_web_server --controller http://localhost:10001 --model-list-mode reload
```
### Open a browser
Navigate to:
```
http://127.0.0.1:7860
```

## Output
![llava_demo](https://github.com/Kaiwei0323/llava-med-windows/assets/91507316/ee28b574-f4bc-4a05-a009-0ba626af274d)

## Reference
* [LLaVA-Med by Microsoft](https://github.com/microsoft/LLaVA-Med)
* [LLaVA-Windows by Nat Lamir](https://github.com/natlamir/LLaVA-Windows)
