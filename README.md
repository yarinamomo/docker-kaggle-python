# docker-python (Forked)

This is a fork of the official [Kaggle Docker Python repository](https://github.com/Kaggle/docker-python), modified to include a reproducible research environment used in my research.

---

## ✅ Reproducing My Research Environment

To recreate the Python environment used in my research:

### Step 1: Clone This Repository

```sh
git clone https://github.com/yarinamomo/docker-kaggle-python.git
cd docker-kaggle-python
```

### Step 2: Build the Docker Image

```sh
docker build -t my-kaggle-env .
```

This uses the official Kaggle Python base image and installs all required libraries used in the **benchmark dataset** in the reasearch listed in **requirements.txt**.

If you previously built the image and want to rebuild with a new requirements file, use --no-cache

```sh
docker build --no-cache -t my-kaggle-env .
```

### Step 3: Run the Container

```sh
docker run -it my-kaggle-env
```

To mount your current working directory for notebook development:

```sh
docker run -it -v $(pwd):/workspace my-kaggle-env
```

## 📦 Original Kaggle Image Information

[Kaggle Notebooks](https://www.kaggle.com/notebooks) allow users to run a Python Notebook in the cloud against our competitions and datasets without having to download data or set up their environment.

This repository includes the [Dockerfile](Dockerfile.tmpl) for building the CPU-only and GPU image that runs Python Notebooks on Kaggle.

Our Python Docker images are stored on the Google Container Registry at:

* CPU-only: [gcr.io/kaggle-images/python](https://gcr.io/kaggle-images/python)
* GPU: [gcr.io/kaggle-gpu-images/python](https://gcr.io/kaggle-gpu-images/python)

## Requesting new packages

First, evaluate whether installing the package yourself in your own notebooks suits your needs. See [guide](https://github.com/Kaggle/docker-python/wiki/Missing-Packages).

If you the first step above doesn't work for your use case, [open an issue](https://github.com/Kaggle/docker-python/issues/new) or a [pull request](https://github.com/Kaggle/docker-python/pulls).

## Opening a pull request

1. Edit the [Dockerfile](Dockerfile.tmpl).
1. Follow the instructions below to build a new image.
1. Add tests for your new package. See this [example](https://github.com/Kaggle/docker-python/blob/main/tests/test_fastai.py).
1. Follow the instructions below to test the new image.
1. Open a PR on this repo and you are all set!

## Building a new image

```sh
./build
```

Flags:

* `--gpu` to build an image for GPU.
* `--use-cache` for faster iterative builds.

## Testing a new image

A suite of tests can be found under the `/tests` folder. You can run the test using this command:

```sh
./test
```

Flags:

* `--gpu` to test the GPU image.
* `--pattern test_keras.py` or `-p test_keras.py` to run a single test
* `--image gcr.io/kaggle-images/python:ci-pretest` or `-i gcr.io/kaggle-images/python:ci-pretest` to test against a specific image

## Running the image

For the CPU-only image:

```sh
# Run the image built locally:
docker run --rm -it kaggle/python-build /bin/bash
# Run the pre-built image from gcr.io
docker run --rm -it gcr.io/kaggle-images/python /bin/bash
```

For the GPU image:

```sh
# Run the image built locally:
docker run --runtime nvidia --rm -it kaggle/python-gpu-build /bin/bash
# Run the image pre-built image from gcr.io
docker run --runtime nvidia --rm -it gcr.io/kaggle-gpu-images/python /bin/bash
```

To ensure your container can access the GPU, follow the instructions posted [here](https://github.com/Kaggle/docker-python/issues/361#issuecomment-448093930).
