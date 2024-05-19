# docker-lab-8-scout
Example usage of docker scout tool for inspecting vulnerabilities of images.

## Table of Contents

- [Requirements](#requirements)
- [Image Building](#image-building)
- [Diagnostics](#diagnostics)
- [Fixing Image](#fixing-image)
- [Repository And Scouting](#repository-and-scouting)

## Requirements

For Linux and Windows systems Docker or Docker Desktop must be installed and running.

For Windows systems WSL must be installed.

Create a repository on [hub.docker.com](https://hub.docker.com/) on your account.

Enroll your account.

```cmd
docker scout enroll eyelor
```

Result:

![Enroll Org](screenshots/enroll-org.jpg)

Enroll your repository.

```cmd
docker scout repo enable --org eyelor eyelor/labx
```

Result:

![Enroll Repo](screenshots/enroll-repo.jpg)

## Image Building

Example for Windows:

Run the command below to build an image. Change the '-t' parameter value to your desired image tag, repository and account name.

```cmd
docker buildx build -q -f Dockerfile_classic -t eyelor/labx:lab8-classic --sbom=true --provenance=mode=max --push .
```

Result:

![Build](screenshots/build.jpg)

## Diagnostics

Checking vulnerabilities of the image.

```cmd
docker cves --platform linux/amd64 --only-severity critical,high eyelor/labx:lab8-classic
```

Results:

![Critical High](screenshots/critical-high.jpg)
![Critical High 1](screenshots/critical-high-1.jpg)
![Critical High 2](screenshots/critical-high-2.jpg)

Vulnerabilities are due to unsupported versions of the packages in package.json project file.

![Package Old](screenshots/package-old.jpg)

## Fixing Image

In order to fix critical and high vulnerabilities package.json file must be editted.

![Package New](screenshots/package-new.jpg)

After that build and scout image again to see results.

Building:

![Build Fixed](screenshots/build-fixed.jpg)

Scouting:

![Critical High Fixed](screenshots/critical-high-fixed.jpg)

After changes there are no critical and high vulnerabilities.

## Repository And Scouting

Created images are accessible on [hub.docker.com](https://hub.docker.com/) service.

If repository has scout enabled there are visual summaries of image vulnerabilities.

![Comparison](screenshots/comparison.jpg)

Link to used images: [hub.docker.com/r/eyelor/labx/tags](https://hub.docker.com/r/eyelor/labx/tags)

