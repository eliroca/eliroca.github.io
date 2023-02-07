title: OBS - build containers

# Build and publish containers with OBS


**Why use the Open Build Service?**

* it will host the latest successful build
* it automatically rebuilds containers if included packages or dependencies get updates

**General prerequisites:**

* get an account on the [openSUSE Build Service](https://build.opensuse.org)
* install the command-line interface for OBS: [`osc`](https://en.opensuse.org/openSUSE:OSC)

<br/>

## Browser web interface

[Direct link to video, in case it does not show up below](assets/obs-build-containers.mp4)

<video controls>
  <source src="../../assets/obs-build-containers.mp4" type="video/mp4">
</video>

<br/>


## Command-line interface

### 1. Create new project

* * *
#### 1.1. Meta configuration

Run

``` bash
> osc meta prj home:eroca:test --edit
```

and add a `containers` repository for your new project:

``` xml hl_lines="9 10 11 12"
<project name="home:eroca:test">

  <title/>
  <description/>

  <person userid="eroca" role="bugowner"/>
  <person userid="eroca" role="maintainer"/>

  <repository name="containers">
    <path project="openSUSE:Containers:Tumbleweed" repository="containers"/>
    <arch>x86_64</arch>
  </repository>

</project>
```

This way you have created a new project and configured a repository.

* * *
#### 1.2. Project configuration

Run

``` bash
> osc meta prjconf home:eroca:test --edit
```

and add the following configuration:

``` bash
%if "%_repository" == "containers"
Type: docker
Prefer: openSUSE-release-appliance-docker
%endif
```

After this step you have enabled container builds in the repository.

<br/>

### 2. Create new package

* * *
#### 2.1. Checkout project

```
> osc checkout home:eroca:test
> cd home:eroca:test
```

* * *
#### 2.2. Create new package

```
> osc mkpac container-hello
> cd container-hello
```

<br/>

### 3. Add Dockerfile

* * *

Open your favorite editor, use the following as template:

``` Dockerfile
#!BuildTag: hello

FROM opensuse/tumbleweed

# add repositories as needed
# RUN zypper addrepo https://download.opensuse.org/repositories/home:eroca:test/openSUSE_Tumbleweed/ "home:eroca:test"
# RUN zypper modifyrepo -p 97 "home:eroca:test"
# RUN zypper --gpg-auto-import-keys refresh

# install packages
RUN zypper --non-interactive install hello

CMD hello
```

then run:

```
> osc add Dockerfile
```

Noticed the `#!BuildTag` line at the beginning? That's specific to OBS; it's your container name.

<br/>

### 4. Build locally

* * *
Building locally lets you fix any errors before commiting, which also saves server resources.

```
> osc build
```

You can of course just use docker or podman for that as well.

<br/>

### 5. Commit changes

* * *
Commiting changes triggers building and publishing on the server.

```
> osc commit -m "Initial commit of container-hello"
```

Check results, will take a few minutes to finish:

``` bash
> osc results --watch
containers           x86_64     building
containers           x86_64     finished*
containers           x86_64     succeeded*
^C
```

<br/>

### 6. Start using your container!

* * *
```
> docker pull registry.opensuse.org/home/eroca/test/containers/hello:latest
```

```
> docker run --rm registry.opensuse.org/home/eroca/test/containers/hello:latest
Hello, world!
```

<br/>

## Links

* [osc documentation](https://en.opensuse.org/openSUSE:OSC)

* [OBS containers documentation](https://openbuildservice.org/help/manuals/obs-user-guide/cha.obs.build_containers.html)

* [openSUSE OBS instance](https://build.opensuse.org)

* [openSUSE containers registry](https://registry.opensuse.org)
