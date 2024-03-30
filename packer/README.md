# PACKER

### You can’t change all Dockerfile instructions, but you can change the CMD,
ENTRYPOINT, ENV, EXPOSE, MAINTAINER, USER, VOLUME, and WORKDIR instructions.

### There are three ways to define post-processors: simple, detailed, and in sequence.
1-simple
```
{
  "post−processors": ["docker−push"]
}
```
2- detailed
```
{
  "post-processors": [
    {
      "type": "docker-save",
      "path": "container.tar"
    }
  ]
}
```
3- sequential
```
{
  "post-processors": [
    [
      {
        "type": "docker-tag",
        "repository": "mehrdadbn9/docker_postproc",
        "tag": "0.1"
      },
      "docker-push"
    ]
  ]
}
```