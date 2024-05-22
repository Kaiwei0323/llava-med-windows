# LLaVA Model Medical Assistance for Windows

## Description
A medical assistance application using LLaVA on Windows

## Hardware Requirements
* NVIDIA GeForce RTX 3080

## Software Requirements
* Windows 11
* Python 3.10
* CUDA 11.8
* PyTorch 2.0
* Openai 0.27.8
* Einops
* Ninja
* Open-clip-torch
* Flash-attention 1.0.4
* pydantic 1.10.9

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
[LLaVA Med (7B)](https://huggingface.co/kaiwei0323/LLaVA-Med-7B)

## Model Transformation Workflow
![Untitled drawing (1)](https://github.com/Kaiwei0323/llava-med-windows/assets/91507316/ad69581e-a691-48fa-b200-78bba7de1eab)
### Explanation
1.  Download and prepare the original LLaMA model
2.  Convert the original LLaMA model to HuggingFace format
3.  Transform the original LLaMA model using the provided medical delta weights
4.  Save the transformed models for use in medical applications

## Environment Setup
1. Clone this repository and navigate to "llava-med-windows" folder
```
git clone https://github.com/Kaiwei0323/llava-med-windows.git
cd llava-med-windows
```
1. Create environment and install dependencies
```
conda create -n llava python=3.10
conda activate llava
pip install -r requirements.txt
```
2. Install PyTorch
```
conda install pytorch torchvision torchaudio pytorch-cuda=11.8 -c pytorch -c nvidia
```
3. Downgrade pydantic to fix the model queue infinite load issue
```
pip install pydantic==1.10.9
```
4. Install bitsandbytes to be able to run quantized model
```
pip install git+https://github.com/Keith-Hon/bitsandbytes-windows.git
```

## Demo Setup
1. Navigate to LLaVA folder
```
cd llava-med-windows
```
2. Download "LLaVA-Med-7B" and put into "models" folder
```
git lfs install
git clone https://huggingface.co/kaiwei0323/LLaVA-Med-7B
```
3. Activate Conda Environment
```
conda activate llava
```
4. Launch Controller
```
python -m llava.serve.controller --host 0.0.0.0 --port 10000
```
5. Launch Model Worker
```
python -m llava.serve.model_worker --host "0.0.0.0" --controller-address "http://localhost:10000" --port 40000 --worker-address "http://localhost:40000" --model-path "models/LLaVA-Med-7B" --multi-modal --load-8bit
```
6. Launch Gradio Web Server
```
python -m llava.serve.gradio_web_server --controller http://localhost:10000 --model-list-mode reload
```
7. Open a browser
Navigate to:
```
http://127.0.0.1:7860
```

## Output
![llava_demo](https://github.com/Kaiwei0323/llava-med-windows/assets/91507316/ee28b574-f4bc-4a05-a009-0ba626af274d)

## Reference
* [LLaVA-Med by Microsoft](https://github.com/microsoft/LLaVA-Med)
* [LLaVA-Windows by Nat Lamir](https://github.com/natlamir/LLaVA-Windows)
