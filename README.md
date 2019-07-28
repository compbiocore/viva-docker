# Docker Images for VIVA (VariantVisualization.jl)
------

For information about VIVA, visit https://compbiocore.github.io/VariantVisualization.jl/latest/.


## Using Docker and Docker Compose

If you don't want to install Julia and VariantVisualization, you can use the Docker images provided.
For that, first [install Docker](https://docs.docker.com/install/).

#### Using Docker

We provide two images, one with a Jupyter Notebook and one with a command line script for VIVA.

Create a project folder and navigate to it:
```shell
mkdir project_x
cd project_x
```

Make sure to add your project VCF files to that folder. That directory will be mapped to `/home/jovyan/notebook/data` inside of the container.

Then, to run the Jupyter Notebook, from the terminal or Windows PowerShell:
```shell
docker run --rm -p 8888:8888 -e JUPYTER_ENABLE_LAB=yes -v "$PWD":/home/jovyan/notebook/data compbiocore/viva-notebook
```
Go to `http://0.0.0.0:8888/?token=<enter token here>`

[Click here](https://jupyter-docker-stacks.readthedocs.io/en/latest/index.html) for more information about Jupyter Docker Images.

To run VIVA Command Line Tool:
```shell
docker run -it --rm -v "$PWD":/data compbiocore/viva-cli /script/viva -f input_file.vcf -o /data [args]
```

#### Using Docker Compose

To run the images with Docker Compose, copy the `docker-compose.yml` file to a local directory. From that same directory, run the commandas below.

> Note: Your current directory will mount to `/notebook/data` in the notebook image and to `/data` in the CLI image.

- Notebook
```shell
docker-compose up viva-notebook
```

- Command Line Tool
```shell
docker-compose run viva -f /data/file.vcf arg2 arg3 ...
```
