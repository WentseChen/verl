# Upgrading to vLLM >= 0.8

## Installation

Note: This version of veRL+vLLM 0.8+ supports **FSDP** for training and **vLLM** for rollout.

```bash
# Create the conda environment
conda create -n verl python==3.10
conda activate verl

# Install verl
git clone https://github.com/volcengine/verl.git
cd verl
pip3 install -e .

# Install the latest stable version of vLLM
pip3 install vllm==0.8.2

# Install flash-attn
pip3 install flash-attn --no-build-isolation

```

We have a pre-built docker image for veRL+vLLM 0.8.2. You can direct import it with the following command:

```bash
docker pull hiyouga/verl:ngc-th2.6.0-cu120-vllm0.8.2
```

## Features

vLLM 0.8+ supports cuda graph and V1 engine by default in veRL. To enable these features, remember to add the following lines to the bash script:

```bash
actor_rollout_ref.rollout.enforce_eager=False \
actor_rollout_ref.rollout.free_cache_engine=False \
```

and also **remove** the environment variable if it exists:

```bash
export VLLM_ATTENTION_BACKEND=XFORMERS
```

## Notes

When you just directly upgrade vllm>=0.8, some dependency packages may undergo version changes. If you encounter the following problems:

```bash
in <module> from torch.multiprocessing.reductions import ForkingPickler ImportError: cannot import name 'ForkingPickler' from 'torch.multiprocessing.reductions' (/opt/conda/lib/python3.11/site-packages/torch/multiprocessing/reductions.py)
```

You need to upgrade `tensordict` to version 0.6.2 using the command `pip install tensordict==0.6.2`.
