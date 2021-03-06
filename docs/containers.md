# Docker
Docker is a `containerization platform` which `packages your application and all its dependencies together` in the form of containers so as to ensure that your application `works seamlessly` in `any environment`, be it development, test or production. Docker containers, wrap a piece of software in a complete filesystem that contains everything needed to run: `code, runtime, system tools, system libraries, storage`. It wraps basically anything that can be installed on a server. This guarantees that the software will always run the same, regardless of its environment.

### ADD vs Copy

#### ADD
- If is a local tar archive is a recognized compression format (identity, gzip, bzip2 or xz) then it is unpacked as a directory. Resources from remote URLs are not decompressed.
- The best use for ADD is local tar file auto-extraction into the image, as in ADD rootfs.tar.xz /
- Because image size matters, using ADD to fetch packages from remote URLs is strongly discouraged; you should use curl or wget instead. That way you can delete the files you no longer need after they’ve been extracted and you don’t have to add another layer in your image. For example, you should avoid doing things like:
```
ADD http://example.com/big.tar.xz /usr/src/things/
RUN tar -xJf /usr/src/things/big.tar.xz -C /usr/src/things
RUN make -C /usr/src/things all
```
And instead, do something like:
```
RUN mkdir -p /usr/src/things \
    && curl -SL http://example.com/big.tar.xz \
    | tar -xJC /usr/src/things \
    && make -C /usr/src/things all
```
#### COPY
- Same as 'ADD', but without the tar and remote URL handling.
- COPY is preferred. That’s because it’s more transparent than ADD
- COPY only supports the basic copying of local files into the container. COPY <src> <dest>
- If you are copying multiple files to same destination directory in Dockerfile, COPY them individually, rather than all at once. This ensures that each step’s build cache is only invalidated (forcing the step to be re-run) if the specifically required files change.
  E.g.
  ```
  COPY requirements.txt /tmp/
  RUN pip install --requirement /tmp/requirements.txt
  COPY . /tmp/
  ```

### Bridge Networks
In terms of Docker, a bridge network uses a software bridge which allows `containers connected to the same bridge network to communicate, while providing isolation from containers which are not connected to that bridge network`. The Docker bridge driver automatically installs rules in the host machine so that containers on different bridge networks cannot communicate directly with each other.

Bridge networks apply to containers running on the same Docker daemon host. For communication among containers running on different Docker daemon hosts, you can either manage routing at the OS level, or you can use an `overlay network`.

When you start Docker, a `default bridge network` (also called bridge) is created automatically, and newly-started containers connect to it unless otherwise specified. You can also create user-defined custom bridge networks. `User-defined bridge networks are superior to the default bridge network.`

### Hypervisors, Virtulization, Containers
- Hypervisor is a software that makes Virtulization possible
- `Hypervisor divides the host system` and allocates the resources to each divided virtual environment. You can basically have multiple OS on a single host system. 
- Two types of hypervisors - `Native hypervisor` (runs directly on host system) and `Hosted hypervisor` (makes uses of host system)
- Container - an `application` that is being developed and deployed is `bundled and wrapped together with all its configuration files and dependencies`. This bundle is called a container.
- Each `container shares the host OS kernel` (usually, the binaries and libraries) running as `isolated processes in user space` on the host operating system. Shared components are read-only.
- Docker containers are basically `runtime instances of Docker images`.

#### Virtulization vs Containers
- Containers are an abstraction of the application layer. Each container is a different application
- `Containers virtualize OS` to run `multiple wrokloads(services) in single OS instance`
- `VM virtualizes Hardware` to run `multiple OS instance`
- Virtual machines are an `abstraction of the hardware layer`. Each VM is a physical machine


### Adding storage to Docker containers
Docker provides three ways to mount data to the container: volumes, bind mounts, and tmpfs storage

- <b>Volumes</b> are part of the host filesystem, but managed by docker at the `specific path` and `should not be modified` by other applications
- <b>Bind mounts</b> `can be anywhere` on the host, but `can be modified` by other applications
- <b>tmpfs</b> are in the host's `in-memory space`, and never get written into the filesystem.
Deleting container does not deletes volume. It needs to be deleted seperately

### Definitions
<b>Docker Architecture</b> Server (docker daemon), REST API, CLI (which calls rest api)

<b>Dockerfile</b> Dockerfile is a text document that contains all the commands a user could call on the command line to assemble an image.

<b>Docker compose</b> Docker Compose is a YAML file which contains details about the services, networks, and volumes for setting up the Docker application. So, you can use Docker Compose to create separate containers, host them and get them to communicate with each other. Each container will expose a port for communicating with other containers

<b>Container lifecycle</b>
- Create a container
- Run the container
- Pause the container(optional)
- Un-pause the container(optional)
- Start the container
- Stop the container
- Restart the container
- Kill the container
- Destroy the container

<b>Docker Machine</b> Docker machine is a tool that lets you install Docker Engine on virtual hosts. These hosts can now be managed using the docker-machine commands

<b>Detached mode</b> shown by the option `--detach` or `-d`, means that a Docker container runs in the background of your terminal. It does not receive input or display output
```
docker run -d IMAGE
```

<b>Docker commit</b> use a container, edit it and build (new image)
```
$ docker commit <conatainer id> <username/imagename>
```

<b>Docker prune</b> 
The above command is used to remove all the stopped containers, all the networks that are not used, all dangling images and all build caches. It’s one of the most useful docker commands
```
docker system prune
```

<b>Docker stats</b> provides CPU and memory usage of the container

<b>Docker events</b> provide information about the activities taking place in the docker daemon.
