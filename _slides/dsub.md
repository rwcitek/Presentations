---
title: DSUB
description:  dsub - simple batch jobs with Docker
theme: black
---

# dsub: simple batch jobs with Docker

<img
  src="../../public/dsub/qr.dsub.png"
  alt="slide" 
  width="300"
/>

Robert Citek<br />
robert.citek@gmail.com

---

# Flow

- Case Study
- Local dsub
- GCP dsub

---


# Case Study


---

## Project

Collect ~10,000 pathology slide scans for machine learning

<img
  src="https://cancer.osu.edu/-/media/images/cancer/website/pages-and-carousels/about/publications/frontiers/2018/winter/hand-holding-glass-slide.jpg"
  alt="slide"
  width="400"
/>

----

## Project details

- ~10,000 pathology slide scans
- ~100 MB per image
- images are in a proprietary format
- ~30 minutes to convert single image
- milestone: within 2 weeks

---
## Can this be done?

<img
  src="https://health.wyo.gov/wp-content/uploads/2017/05/man-with-question-mark.jpg"
  alt="slide"
  width="400"
/>


----

## 10,000 Images: Quick calcs

- Size
  - MB/image: ~100
    - total : ~1 TB

- Time
  - min/image: ~30 
    - minutes: 
    - hours: 
    - days: 
    - weeks: 
    - months: 

----

## 10,000 Images: Quick calcs

- Size
  - MB/image: ~100
    - total: ~1 TB

- Time
  - min/image: ~30 
    - minutes: ~300,000 
    - hours: ~5,000 
    - days: ~200
    - weeks: ~30 
    - months: ~7.5 

----

# 7.5 mo ~ 30 weeks 
# > 2 weeks


<img src="https://png.pngtree.com/png-vector/20210402/ourlarge/pngtree-heart-shaped-anniversary-black-and-white-calendar-icon-date-plan-png-image_3189633.jpg" alt="calendar" width="400"/>


----

## Options ???
- Admit defeat: it will take 7.5 months

- <span class="fragment">
Get a much bigger computer(s) ( ~$30k+ )
</span>

- <span class="fragment">
Use "the cloud"
</span>


----

## Used the cloud
- Google Cloud Platform

<img
  src="https://www.gstatic.com/devrel-devsite/prod/v6cd15f45ec209c8961e07ea7e57ed9a0e9da4333bc915e67d1fcd2b2a9ec62d1/cloud/images/social-icon-google-cloud-1200-630.png" 
  alt="GCP"
  width="800"
/>


----

## dsub: simple batch jobs with Docker

<img
  src="https://33.media.tumblr.com/8039ce32aa6b389a0ac721b32fa441ac/tumblr_njeqnxCI0m1qzbj7zo1_500.gif" 
  alt="GCP"
  width="400"
/>

----

## Components Used
- Google Cloud Storage ( GS )
- Python ( conversion )
- Docker ( container )
- Google Container Registry ( GCR )
- dsub ( batch scheduler )

<img
  src="https://lh3.googleusercontent.com/D4Zcor2AekWNG-5vxBLHIUhLiqfx327GwqhQ9xJNx8Ne1-GCFu9YECaKGXlwtUPfDFgO53-phOQ=e14-rw-lo-sc0xffffff-h24"
  alt="GCP"
  width="200"
/>
<img
  src="https://banner2.cleanpng.com/20180412/kye/kisspng-python-programming-language-computer-programming-language-5acfdc3636bac7.8891188615235717662242.jpg"
  alt="Python"
  width="200"
/>
<img
  src="https://www.docker.com/wp-content/uploads/2022/03/Moby-logo.png"
  alt="Docker"
  width="200"
/>
<img
  src="https://lh3.googleusercontent.com/E6eJcOVeO64N8P3gPvRoSwmrzK-olWpfje16gjhGKlKPgQKXmfIqjnQfYaX8gdE0zMvLLGy678dBrYNxMwavKg=w80-h80"
  alt="GCR"
  width="200"
/>


----

## Connecting the Dots
- Pushed images to GS
- Wrapped Python script in Docker
- Pushed Docker to GCR
- Created dsub template
  - VM details ( CPUs, RAM, local storage, preemptible )
  - Process ( Docker container + Python sript )
  - Input ( proprietary image )
  - Output ( converted image + metadata )
  - 1 VM : 1 Docker : 1 image
- Launched dsub

----
## Connecting the Dots

<img src="../../public/dsub/dsub.workflow.combo.png" alt="slide" width="800"/>

----

## dsub Workflow
- launch VM
- pull image from GS onto VM
- pull Docker from GCR onto VM
- launch Docker to convert image
- push converted image + metadata to GS
- destroy VM
- repeat for each image <span class="fragment">... in parallel</span>


----

## Metrics
- maximum of 4,000 VMs running in parallel
- ~30 min/image
- total time: ~3 hours
- total cost: ~$300

----

## 4,000 CPUs

<img
  src="https://storage.googleapis.com/gweb-uniblog-publish-prod/images/unnamed_RTmGiMI.max-1300x1300.png"
  alt="data center"
  width="1000"
/>

----

## Big savings

<img
  src="https://www.pngkey.com/png/detail/18-180664_calendar-clock-comments-time-and-date-icon-png.png"
  alt="time"
  width="400"
  height="400"
/>
<img
  src="https://pngimages.in/uploads/png-webp/2022/2022-September/Money_Png_Transparent_Images.webp"
  alt="time"
  width="400"
  height="400"
/>


----

## Big savings

- 3 hours << 2 weeks << 7 months
- from 5,000 hours to 3 hours => ~1,000x
- from $30,000 to $300 => ~100x
- Net: ~100,000x time-dollar savings

----

## Met Milestone

<img
  src="https://png.pngtree.com/png-clipart/20210301/ourlarge/pngtree-black-and-white-finish-line-clipart-png-image_2977545.jpg"
  alt="time"
  height="400"
/>

----

# Where to start?

----

# dsub local

[dsub in Docker]( https://github.com/rwcitek/Docker.dsub )

----

## Start a Docker container instance
```
( dsub_tmp=/tmp/dsub-$( date +%s ) &&
docker container run \
  --detach \
  --name    dsub \
  --volume  /var/run/docker.sock:/var/run/docker.sock \
  --volume  /tmp:/tmp \
  --env     TMPDIR=${dsub_tmp} \
  --workdir ${dsub_tmp} \
  rwcitek/dsub sleep inf
)
```

----

## Exec into instance

```
docker container exec -it dsub /bin/bash
```

----

## Setup environment

```
docker image pull ubuntu:22.04
docker image tag ubuntu:22.04 ubuntu:dsub

docker image list ubuntu
```

----

## Run a command

```
dsub \
  --provider local \
  --image    ubuntu:dsub \
  --logging  "${TMPDIR}/dsub-test/logging/" \
  --output   OUT="${TMPDIR}/dsub-test/output/out.command.txt" \
  --command  'echo "Hello World" > "${OUT}"' \
  --wait
```

----

# dsub GCP










----

# Summary

- For large parallel batch jobs, dsub provides a nice solution.
- To get started, all you need are Docker and a GCP account.

----

## Further Reading

- [dsub](https://github.com/DataBiosphere/dsub)
- [Docker](https://github.com/rwcitek/docker/blob/master/A_Gentle_Introduction_To_Docker/Docker-walkthrough.md)
- [Google Container Registry](https://cloud.google.com/container-registry)
- [Google Storage](https://cloud.google.com/storage)
- [Python](https://github.com/rwcitek/PythonResources)
- [git](https://github.com/rwcitek/git.sample/tree/master/git.deep.dive)
- [GitHub](https://rwcitek.github.io/gh-slides/slides/github-demo/#/)

----

## Questions?

<img
  src="../../public/dsub/qr.dsub.png"
  alt="slide" 
  width="300"
/>

Robert Citek<br />
robert.citek@gmail.com

----



----



---

## Questions:
- Why 4,000 VMs instead of 10,000 VMs?
- Why ~3 hours and not 1.5 hours?
- Were all 10,000 images successfully converted?

----


----

To the optimist, the glass is half full.<br />
To the pessimist, the glass is half empty.<br />
To the engineer, the glass is twice as big as it needs to be.<br />
To Excel, the glass is January 2nd.<br />




