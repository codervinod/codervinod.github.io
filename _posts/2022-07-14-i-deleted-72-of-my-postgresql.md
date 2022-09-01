---
title: I deleted 72% of my PostgreSQL container and it still works
canonicalurl: https://medium.com/@codervinod/i-deleted-72-of-my-postgresql-container-and-it-still-works-732d2184c1c9
---


### I deleted 72% of my PostgreSQL container image and it still works


Yes, you read that right. **I deleted almost 72% of the code from the PostgreSQL bitnami container image** on Docker Hub and it still does everything I need. No tricks, no rabbits up my sleeve. It turns out there is a huge amount of unused code in the bitnami distribution — and **this is a common problem for all of the popular container images.**


It’s also way more secure. Let me show you why and how.


![](/assets/medium_images/1uROMXoN2kieOTBvt5d3Pkw.png)

### Unused code => lots of CVEs


Cloud-native infrastructure is built around open source technologies, which is great but comes with **inherent security trade-offs**. We all use several Docker containers to create cloud-native applications, but we often overlook how many vulnerabilities these open source images have. To counter this, Google, AWS, and DockerHub have all developed tools to scan containers and show CVE reports. Unfortunately, **nothing has been done to fix the large number of CVEs in open source images**, and it’s a well-kept secret that no one wants to talk about.


A few months back, I spoke to some former colleagues who were building [RapidFort](http://rapidfort.com/), a platform for removing unnecessary software packages from containers with minimal developer effort. While this solution works well for the code enterprises generate, I wondered about open source containers I was running at my own company. They might be carrying thousands of vulnerabilities yet to be discovered and it made me uneasy.


I was intrigued by the large number of CVEs in open source containers and started playing with RapidFort’s tech. I wanted to build a repository of popular open-source images that are slim, fast, and contain only the packages necessary to achieve common tasks.


For example: **When running a production database, I might not need networking tools or several bash commands until and unless I plan to debug something.** Why give malicious actors easy opportunities to land and expand in my infrastructure?


With all this in mind, I used RapidFort’s technology to build a completely open-source GitHub project: <https://github.com/rapidfort/community-images>. And I’m pleased to announce that as of this writing, we are providing daily-updated hardened images for:


* [MariaDB](https://github.com/rapidfort/community-images/tree/main/community_images/mariadb/bitnami)
* [MongoDB](https://github.com/rapidfort/community-images/tree/main/community_images/mongodb/bitnami)
* [MySQL](https://github.com/rapidfort/community-images/tree/main/community_images/mysql/bitnami)
* [NGINX](https://github.com/rapidfort/community-images/tree/main/community_images/nginx/bitnami)
* [PostgreSQL](https://github.com/rapidfort/community-images/tree/main/community_images/postgresql/bitnami)
* [Redis](https://github.com/rapidfort/community-images/tree/main/community_images/redis/bitnami)
* [Redis Cluster](https://github.com/rapidfort/community-images/tree/main/community_images/redis-cluster/bitnami)
* [Envoy](https://github.com/rapidfort/community-images/tree/main/community_images/envoy/bitnami)
* [FluentD](https://github.com/rapidfort/community-images/tree/main/community_images/fluentd/bitnami)
* [Grafana OnCall](https://github.com/rapidfort/community-images/tree/main/community_images/oncall/grafana)
* [InfluxDB](https://github.com/rapidfort/community-images/tree/main/community_images/influxdb/bitnami)
* [Etcd](https://github.com/rapidfort/community-images/tree/main/community_images/etcd/bitnami)
* [NATS](https://github.com/rapidfort/community-images/tree/main/community_images/nats/bitnami)


Before we get into the technical details, let me tell you how these images are generated.


### Building container stubs to test in the CI/CD pipeline


When developers deliver a product, they write some code, build a container image, and deploy that image to production. Most of the time, no one scans that container to make sure it’s secure. This is where RapidFort makes a difference.


With RapidFort, a developer can generate an instrumented image called a “stub.” This stub can run in a test environment that exercises (or covers) expected workflows using a coverage script. (***Please note that the coverage script is not the same as the testing script. The coverage script needs to cover primary functionality and doesn’t need to worry about the output or functional/integration testing.***) RapidFort tracks what code was *actually used* in the coverage test and gives a developer the opportunity to delete anything unused.


Stubs are very small and easy to put together. Once the coverage script uses the container image, RapidFort can provide a hardened image. This hardened image contains only the software required to run the workload. *Voila*, you have a lean, secure image that can be tested in a CI/CD pipeline and released/deployed to production.


![](/assets/medium_images/01qAW0_eQLJiGpvXj)

### How I shrank the PostgreSQL image by 72%


To create our repository of the most commonly used images, I started with PostgreSQL, one of the most frequently downloaded images on the internet at more than a billion downloads.


```
$ docker pull bitnami/postgresql:latest**
```


After downloading the image, I created a stub (instrumented) container through rapidfort cli.


```
$ rfstub bitnami/postgresql:latest**
```

I quickly pulled together a coverage script that exercises the most commonly used PostgreSQL commands.


[test.psql](https://github.com/rapidfort/community-images/blob/main/community_images/common/tests/test.psql)


After using the commands, I wanted to exercise common PostgreSQL applications that come bundled with the PostgreSQL docker image. I quickly built a bash script that runs one applications at a time.


[postgres\_coverage.sh](https://github.com/rapidfort/community-images/blob/main/community_images/common/tests/postgres_coverage.sh)


```
#!/bin/bash

set -x
set -e

# add common commands here which should be present in all hardened images

clusterdb --version
createdb --version
createuser --version
dropdb --version
dropuser --version
if command -v ecpg --version &> /dev/null
then
    ecpg --version
fi
if command -v pg_amcheck --version &> /dev/null
then
    pg_amcheck --version
fi
pg_basebackup --version
pgbench --version
pg_config --version
pg_dump --version
pg_dumpall --version
pg_isready --version
pg_receivewal --version
pg_recvlogical --version
pg_restore --version
if command -v pg_verifybackup --version &> /dev/null
then
    pg_verifybackup --version
fi
psql --version
reindexdb --version
vacuumdb --version
```

I also covered three runtimes for PostgreSQL containers:


* Kubernetes
* Docker container
* Docker-compose


These coverage scripts cover most use cases. After running the coverage scripts, I created a hardened image from RapidFort cli.


```
$ rfharden bitnami/postgresql:latest-rfstub**
```


To ensure the hardened image works in most scenarios, I used <https://www.postgresql.org/docs/current/pgbench.html> as benchmark testing. This tool allowed me to exercise different commands at scale on PostgreSQL and proved the image is ready for testing on production.


I also built a completely automated CI/CD pipeline for the community images using GitHub actions. This ensures I create a hardened version for source images within one hour of publishing.


[PostgreSQL Github actions script](https://github.com/rapidfort/community-images/blob/main/.github/workflows/postgresql_bitnami.yml)


### Shrinking PostgreSQL by 72% removed 74% of known vulnerabilities


![](/assets/medium_images/0CKUaXrNB8kMBw8AN)

The results from hardening these images were impressive:


* The PostgreSQL image shrank from 269 MB to 76 MB.
* 74% of the known CVE(s) were removed
* Of 62 critical/high CVEs, the CVE scanner found only twenty in the hardened image.


![](/assets/medium_images/0HQHYYiSIR8N9L37S)

When my team is faced with 62 critical CVEs to remediate, it requires a lot of additional work. Using my coverage test, RapidFort was able to eliminate 74% of those CVEs, meaning my team has much less work to do to prepare the image for production.


### Join the community and try our hardened images


After months of testing, I am excited to release our first versions of our hardened Community Images.


![](/assets/medium_images/0CqckOKL2ykMchB23)

I convinced my colleagues to use our hardened images to build the RapidFort system, demonstrating our confidence that the rest of the world can use these images.


### We need your help with what’s next for our Community Images project


While I am happy to release our first version of images, the work is not done yet until we have covered most images available for everyone to use. **I am looking for volunteers to join our Community Images project and contribute.** If you are interested in reducing the number of vulnerabilities your infrastructure carries, please download our hardened images.


If you run into any issues, please feel free to contribute or open an issue. <https://github.com/rapidfort/community-images/issues/new/choose>


