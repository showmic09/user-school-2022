# OSG User School 2022

The source files for the OSG User School 2022 website.

To serve the website locally, download Docker and use the command in the root directory:

```console
docker run --rm -it -p 8000:8000 -v ${PWD}:/docs squidfunk/mkdocs-material
```
