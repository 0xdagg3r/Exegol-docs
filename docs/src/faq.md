# Frequently asked questions

Below are the frequently asked questions regarding either features or
the overall project.

## What tools are installed in Exegol?

The list of tools is dynamically generated for all Exegol images and
available [here](/images/tools).

## Can I run Exegol on a macOS?

Yes. And both CPU architectures are supported (Intel X86_64 (AMD64) and
Apple Silicon M1/M2 (ARM64).

> [!SUCCESS] Tip 
> We strongly advised macOS users to replace Docker Desktop with
> [OrbStack](https://orbstack.dev/), allowing host network mode to work
> for instance, this it's not supported by Docker Desktop for Mac.

## Can I use a VPN with Exegol?

Yes. And you have multiple choices.

- **The "YOLO" choice**: at the container creation (i.e. when "starting"
  a container for the first time), give all permissions to the container
  so that you're able to run openvpn in it and start the vpn. The
  command should look like
  `exegol start <container_name> <image_name> --privileged`.
- **The better choice**: use the `--vpn` option at the container
  creation:
  `exegol start <container_name> <image_name> --vpn <myconf.ovpn>`. It's
  the easiest and more secure choice. See the `start` help
  [here](/wrapper/cli/start).

> [!WARNING]
> Creating a **privileged** container (c.f. the "YOLO" choice) exposes
> you to higher security risks. This should be avoided.

## Can I customize Exegol?

Yes, please refer to the
["my-resources" documentation](/images/my-resources) that
explains how to automatically setup your changes to your Exegol
containers. Also, see the
["wrapper's advanced-uses" documentation](/wrapper/features#advanced-uses)
to see how to edit Exegol's conf among other things. You could also want
to [make your own Exegol image](/faq#can-i-make-my-own-exegol-image)

## Can I make my own Exegol image?

Yes. You will need to create a dockerfile (e.g. `CUSTOM.dockerfile`) at
the root of the exegol-images module next to the other dockerfiles (i.e.
`/path/to/Exegol/exegol-docker-build/`) containing the instructions you
want the build process to follow.

Then, run something like `exegol install "myimagename" "CUSTOM"` to
build the image locally. See the `install` documentation:
[install action](/wrapper/cli/install).

## How to install Exegol on an external drive?

Exegol's wrapper is lightweight, but it's Docker images can take up some
space, and users may not have enough room in their internal HDD/SSD,
hence the question. This usually comes down to "how can I install Docker
on an external drive?", and the answer depends on the host.

> [!SUCCESS] Tip
> Use a fast drive, otherwise Exegol will get real slow.

For macOS and Windows users, this can be configured in the Docker
Desktop dashboard (in
`Settings > Resources > Advanced > Disk image location`).

<img src="/assets/faq/docker_desktop_disk_image_location.png"
class="align-center"
alt="Disk Image Location Setting (Docker Desktop)" />

## How to add a new tool?

"Adding a tool" can mean many things. Depending on that, you'll get a
different answer. So let's answer most of them.

If you want to add a tool:

- **in the official Exegol images**: refer to the
  [contribution guidance](/contribute/resources#adding-new-tools#adding-a-new-tool).
- **in your own custom local image**: refer to the
  [contribution guidance](/contribute/resources#adding-new-tools#adding-a-new-tool) as well, but instead of
  creating a Pull Request at the end to offer your contribution, just
  build the image locally with the wrapper and enjoy your custom local
  image.
- **in a live container**: that's your container, you can do whatever
  you whish in it ;)
- **automatically in all containers at their creation**: refer to the
  ["my-resources" documentation](/images/my-resources).

## Can I install docker directly on my WSL2 distro instead of Docker Desktop ?

Yes, it's possible to install docker directly on WSL2 rather than using
Docker Desktop, but you'll be restricted to your WSL2 environment and
its constraints.

Although Docker Desktop is incomplete, it does offer a few advantages
(exegol can be used from powershell / cmd, windows folder sharing with
the exegol workstation, etc). We therefore recommend **Docker Desktop as
the official support** for Exegol.

We do **not** guarantee wrapper stability with a directly installed WSL
docker.

## How to retrieve your desktop login details ?

The container's root password can be obtained with
`exegol info <container>` (i.e. this is needed when using the
[desktop](/wrapper/features#desktop) feature)

## WSL 2 consumes massive amounts of RAM, CPU power, and Disk Space. How can I deal with this issue ?

Users might experience excessive memory consumption when using Exegol.
This is caused by WSL 2 not freeing up RAM even when processes are
finished, causing large amounts of unused memory to remain allocated.
This leads to high memory usage on the host system and reduced
performance. More information about this issue can be found \[at this
GitHub issue\](<https://github.com/microsoft/WSL/issues/4166>). A simple
workaround is to create a
<span class="title-ref">%UserProfile%.wslconfig</span> file in Windows
and use it to limit memory assigned to WSL 2 VM.

`` ` [wsl2] memory=8GB   # Limits VM memory in WSL 2 up to 3GB processors=2 # Makes the WSL 2 VM use two virtual processors ``\`

When using Docker with the WSL2 backend, resource limits are managed by
Windows. There might be cases, for example after updating an Exegol
image, where Docker might take double the disk space the image needs. To
deal with this problem, users should find the
<span class="title-ref">Disk image location</span> in Docker Desktop
(<span class="title-ref">Settings -\> Resources -\> Advanced</span>). It
will be in the following format
<span class="title-ref">C:Users\<USER\>AppDataLocalDockerwsl</span>. The
Virtual Hard Disk can be located in the following path,
<span class="title-ref">C:Users\<USER\>AppDataLocalDockerwsldisk</span>.
Users can use the <span class="title-ref">diskpart</span> tool to shrink
the Virtual Hard Disk using the following instructions based on
directions
\[here\](<https://stackoverflow.com/questions/70946140/docker-desktop-wsl-ext4-vhdx-too-large>). -
Stop Docker Desktop - Start an administrative CMD or PowerShell
session - Stop WSL2: <span class="title-ref">wsl --shutdown</span> -
Start the diskpart tool: <span class="title-ref">diskpart</span> -
Select the Virtual Hard Disk: <span class="title-ref">select vdisk
file="C:Users\<USER\>AppDataLocalDockerwsldiskdocker_data.vhdx"</span>.
A message <span class="title-ref">DiskPart successfully selected the
virtual disk file.</span> should appear. - Shrink the Virtual Hard Disk:
<span class="title-ref">compact vdisk</span> - Wait for the proccess to
reach 100 completion (might take some time)

## How do I update Exegol?

`exegol update`

> [!SUCCESS] Hint
> If you installed the wrapper with pipx, update with `pipx upgrade exegol`
