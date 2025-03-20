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
  - Total : ~1 TB

- Time
  - min/image: ~30
- Total Time
  - minutes:
  - hours:
  - days:
  - weeks:
  - months:


----

## 10,000 Images: Quick calcs

- Size
  - MB/image: ~100
  - Total: ~1 TB

- Time
  - min/image: ~30 
- Total Time
  - minutes: ~300,000 
  - hours: ~5,000 
  - days: ~200
  - weeks: ~30 
  - months: ~7

----

# 7 mo ~ 30 weeks 
# 30 weeks >> 2 weeks


<img
  src="https://png.pngtree.com/png-vector/20210402/ourlarge/pngtree-heart-shaped-anniversary-black-and-white-calendar-icon-date-plan-png-image_3189633.jpg"
  alt="calendar"
  width="400"
/>

----

## Options ???
- <span class="fragment">
Admit defeat: it will take ~7 months
</span>

- <span class="fragment">
Get a much bigger computer(s) ( $30k-$80k+ )
</span>

- <span class="fragment">
Use "the cloud"
</span>


----

## Used the cloud
- Google Cloud Platform

<img
  src="https://storage.googleapis.com/gweb-cloudblog-publish/images/BlogHeader_Set2_D_ShTJD99.max-2600x2600.png" 
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
  src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTgG-Wyr7CAYwhkI6md8R6PQwelhLMZwiKtmg&s"
  alt="GCS"
  height="250"
  width="200"
/>
<img
  src="https://upload.wikimedia.org/wikipedia/commons/thumb/c/c3/Python-logo-notext.svg/1200px-Python-logo-notext.svg.png"
  alt="Python"
  height="250"
  width="200"
/>
<img
  src="https://www.docker.com/wp-content/uploads/2022/03/Moby-logo.png"
  alt="Docker"
  height="250"
  width="200"
/>
<img
  src="https://lh3.googleusercontent.com/E6eJcOVeO64N8P3gPvRoSwmrzK-olWpfje16gjhGKlKPgQKXmfIqjnQfYaX8gdE0zMvLLGy678dBrYNxMwavKg=w80-h80"
  alt="GCR"
  height="250"
  width="200"
/>


----

## Connecting the Dots
- Pushed images to GCS
- Wrapped Python script in Docker
- Pushed Docker to GCR
- Created dsub template
  - 1 VM : 1 Docker Instance : 1 TIFF image
  - VM details ( CPUs, RAM, local storage, preemptible )
  - Process ( Docker container + Python sript )
  - Input ( proprietary image )
  - Output ( converted image + metadata )
- Launched dsub

----
## Connecting the Dots

<img src="../../public/dsub/dsub.workflow.combo.png" alt="slide" width="400"/>

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
- total time: ~3 hours
- per image time: ~30 min
- total cost: ~$300
- per image cost: $0.03

----

## 4,000 CPUs in parallel

<img
  src="https://storage.googleapis.com/gweb-uniblog-publish-prod/images/unnamed_RTmGiMI.max-1300x1300.png"
  alt="data center"
  width="1000"
/>

----

## 4,000 CPUs in parallel

<img
  src="https://storage.googleapis.com/gweb-uniblog-publish-prod/images/unnamed_RTmGiMI.max-1300x1300.png"
  alt="data center"
  width="250"
/>
<img
  src="https://storage.googleapis.com/gweb-uniblog-publish-prod/images/unnamed_RTmGiMI.max-1300x1300.png"
  alt="data center"
  width="250"
/>
<img
  src="https://storage.googleapis.com/gweb-uniblog-publish-prod/images/unnamed_RTmGiMI.max-1300x1300.png"
  alt="data center"
  width="250"
/>
<img
  src="https://storage.googleapis.com/gweb-uniblog-publish-prod/images/unnamed_RTmGiMI.max-1300x1300.png"
  alt="data center"
  width="250"
/>
<img
  src="https://storage.googleapis.com/gweb-uniblog-publish-prod/images/unnamed_RTmGiMI.max-1300x1300.png"
  alt="data center"
  width="250"
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
  src="https://img.freepik.com/premium-vector/sack-money-big-pile-cash-money-icon-illustration-money-bag-flat-icon_385450-362.jpg"
  alt="time"
  width="400"
  height="400"
/>


----

## Big savings

- 3 hours << 2 weeks << 30 weeks
- from 5,000 hours to 3 hours => ~1,000x
- from $30,000 to $300 => ~100x
- Net: faster, better, cheaper

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
$ ( dsub_tmp=/tmp/dsub-$( date +%s ) &&
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
$ docker container exec -it dsub /bin/bash
```

----

## Setup environment

```
# docker image pull ubuntu:24.04
# docker image tag ubuntu:24.04 ubuntu:dsub
# docker image list ubuntu

REPOSITORY   TAG       IMAGE ID       CREATED       SIZE
ubuntu       24.04     a04dc4851cbc   7 weeks ago   78.1MB
ubuntu       dsub      a04dc4851cbc   7 weeks ago   78.1MB

```

----

## Run a dsub command

```
# dsub \
  --provider local \
  --image    ubuntu:dsub \
  --logging  "${TMPDIR}/dsub-test/logging/" \
  --output   OUT="${TMPDIR}/dsub-test/output/out.command.txt" \
  --command  'echo "Hello World" > "${OUT}"' \
  --wait

```

----

## Run a dsub command
Output

```
Job properties:
  job-id: echo--root--250320-040934-02
  job-name: echo
  user-id: root
Launched job-id: echo--root--250320-040934-02
To check the status, run:
  dstat --provider local --jobs 'echo--root--250320-040934-02' --users 'root' --status '*'
To cancel the job, run:
  ddel --provider local --jobs 'echo--root--250320-040934-02' --users 'root'
Waiting for job to complete...
Waiting for: echo--root--250320-040934-02.
  echo--root--250320-040934-02: SUCCESS
echo--root--250320-040934-02
```

----

## View command output

```
# tree dsub-test/
dsub-test/
|-- logging
|   |-- echo--root--250320-040934-02-stderr.log
|   |-- echo--root--250320-040934-02-stdout.log
|   `-- echo--root--250320-040934-02.log
`-- output
    `-- out.command.txt

```
```
# cat dsub-test/output/out.command.txt 
Hello World
```


----

## Run multiple jobs<br />using a TSV file (pt1)

1) Create a mock input file
```
# echo 'Hello, world!' > /tmp/input.txt
```
2) Create the TSV file with inputs and outputs
```
# << eof sed -e's/ *{tab} */\t/g' > run.tsv
--input INPUT  {tab} --output OUTPUT
/tmp/input.txt {tab} ${TMPDIR}/dsub-test/output/out1.multi.txt
/tmp/input.txt {tab} ${TMPDIR}/dsub-test/output/out2.multi.txt
/tmp/input.txt {tab} ${TMPDIR}/dsub-test/output/out3.multi.txt
eof
```

----

## Run multiple jobs<br />using a TSV file (pt2)

3) Create a script that generates output from input
```
<<'eof' cat > multi-job.sh
#!/bin/bash
sed -e 's/Hello/Greetings/' "${INPUT}" > "${OUTPUT}"
date >> "${OUTPUT}"
eof
```

----

## Run multiple jobs<br />using a TSV file (pt3)

4) Run dsub
```
# dsub \
  --provider local \
  --image ubuntu:dsub \
  --logging "${TMPDIR}/dsub-test/logging/" \
  --script ./multi-job.sh \
  --tasks ./run.tsv \
  --wait
```

----

## Run multiple jobs<br />output

```
Job properties:
  job-id: multi-job--root--250320-041332-71
  job-name: multi-job
  user-id: root
Launched job-id: multi-job--root--250320-041332-71
3 task(s)
To check the status, run:
  dstat --provider local --jobs 'multi-job--root--250320-041332-71' --users 'root' --status '*'
To cancel the job, run:
  ddel --provider local --jobs 'multi-job--root--250320-041332-71' --users 'root'
Waiting for job to complete...
Waiting for: multi-job--root--250320-041332-71.
  multi-job--root--250320-041332-71: SUCCESS
multi-job--root--250320-041332-71
```

----

## View Filesystem from multiple jobs

```
# tree dsub-test/
dsub-test/
|-- logging
|   |-- multi-job--root--250320-041332-71.1-stderr.log
|   |-- multi-job--root--250320-041332-71.1-stdout.log
|   |-- multi-job--root--250320-041332-71.1.log
|   |-- multi-job--root--250320-041332-71.2-stderr.log
|   |-- multi-job--root--250320-041332-71.2-stdout.log
|   |-- multi-job--root--250320-041332-71.2.log
|   |-- multi-job--root--250320-041332-71.3-stderr.log
|   |-- multi-job--root--250320-041332-71.3-stdout.log
|   `-- multi-job--root--250320-041332-71.3.log
`-- output
    |-- out1.multi.txt
    |-- out2.multi.txt
    `-- out3.multi.txt
```

----

## View Output from multiple jobs

```
# tail -n +1 dsub-test/output/*  
==> dsub-test/output/out1.multi.txt <==
Greetings, world!
Thu Mar 20 04:13:37 UTC 2025

==> dsub-test/output/out2.multi.txt <==
Greetings, world!
Thu Mar 20 04:13:37 UTC 2025

==> dsub-test/output/out3.multi.txt <==
Greetings, world!
Thu Mar 20 04:13:37 UTC 2025
```

----

# dsub GCP

----

## Setup

```
# gcloud init
...

# export GOOGLE_APPLICATION_CREDENTIALS=/root/.config/gcloud/...
```

Enable APIs
- https://console.developers.google.com/apis/api/lifesciences.googleapis.com/overview


----

## Run dsub command

```
# my_bucket=rwc-data
# dsub \
  --project my-cloud-project \
  --regions us-west1 \
  \
  --provider google-cls-v2 \
  --logging gs://${my_bucket}/logging/ \
  --output OUT=gs://${my_bucket}/output/out.txt \
  \
  --image ubuntu:22.04 \
  --command 'echo "Hello World" > "${OUT}"' \
  --wait
```

----

## Run dsub command<br />output

```
Job properties:
  job-id: echo--root--230203-000230-22
  job-name: echo
  user-id: root
Provider internal-id (operation): projects/689131617798/locations/us-central1/operations/4989729828719590152
Launched job-id: echo--root--230203-000230-22
To check the status, run:
  dstat --provider google-cls-v2 --project default-256400 --location us-central1 --jobs 'echo--root--230203-000230-22' --users 'root' --status '*'
To cancel the job, run:
  ddel --provider google-cls-v2 --project default-256400 --location us-central1 --jobs 'echo--root--230203-000230-22' --users 'root'
Waiting for job to complete...
Waiting for: echo--root--230203-000230-22.
  echo--root--230203-000230-22: SUCCESS
echo--root--230203-000230-22
```

----

## dsub output files

```
# gsutil ls gs://rwc-data/**
gs://rwc-data/logging/echo--root--230203-000230-22-stderr.log
gs://rwc-data/logging/echo--root--230203-000230-22-stdout.log
gs://rwc-data/logging/echo--root--230203-000230-22.log
gs://rwc-data/output/out.txt
```

----

## dsub output

```
# gsutil cat gs://rwc-data/output/out.txt
Hello World
```

----

## What's next?

----

## What's next?
- Tons of options
  - `dsub --help`
    - `--after`
    - `--preemptible`
    - `--mount`
    - `--env`
    - VM options ( RAM, CPU, disks )
- job control with `dstat`
- troubleshooting with `--ssh` option


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




