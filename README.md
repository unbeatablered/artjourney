Pastel Mix on Rplicate
===

This model use [andite/pastel-mix](https://huggingface.co/andite/pastel-mix) `better-vae` version and diffusers with three pipelines to generate images.

## How it works

This implementation is targeted to make results similar to the pastel-mix demo generated by Stable Diffusion WebUI.

It uses three pipelines:

* First Pass - use the prompt to generate a base image to add details
* Second Pass - upscale with Latent mode
* Third Pass - use the image to image and add more details from the upscaled image

> There have many limitations for now because diffusers didn't support all Stable Diffusion WebUI features.

## Development

### Prepare Batter VAE model

By default `andite/pastel` model didn't provide the diffusers model with the `better-vae` version, you have to convert it yourself.

Get diffusers and downlaod `pastelmix-better-vae.safetensors` from HuggingFaces

```bash
git clone git@github.com:huggingface/diffusers.git
```

Install required python packages

```bash
pip3 install diffusers omegaconf transformers safetensors
```

Convert the model

```bash
python3 ./diffusers/scripts/convert_original_stable_diffusion_to_diffusers.py --dump_path ./pastel-mix-better-vae --scheduler_type ddim --from_safetensors --checkpoint_path ./pastelmix-better-vae.safetensors
```

After your model is dumped, follow HuggingFace [Guide](https://huggingface.co/docs/hub/models-uploading) to upload it.

> If your plan to work on the local machine with GPU, the upload is optional.

### Prepare Environment

Before starting, make sure you have `docker` is installed.

```bash
make setup
```

It will install `cog` and `huggingface-cli` for future works.

Run `huggingface-cli login` if you are working on a remote machine.

And run `cog login` to make sure you can upload the model.

### Testing

You can use `cog predict` or predefined command `make predict` to test it works correctly.
