---
title: Securing the Most Popular Docker Containers
canonicalurl: https://www.rapidfort.com/post/securing-the-most-popular-docker-containers
---


## **Securing the Most Popular Docker Containers**

Our Community Images project has been a huge success with **more than** [**1.3M downloads**](https://github.com/rapidfort/community-images) **of our hardened containers** in just a few months. The Docker Hub community has been very welcoming and we’re just getting started with this project. 

Because of the rising interest, we thought we’d walk through our selection criteria and hardening process so you can better understand how the project works and know what you’re getting when you download one of our images.

## How Community Images are Hardened

We’ve written fairly extensively [on our blog](http://rapidfort.com/blog) about container hardening and our approaches, but we’ll provide a brief overview of the hardening process so you understand what it is that we’re offering to the community.

Our hardening platform, RapidFort, assesses how a container is used and what files are touched during normal operation. The assessment tracks what code in the container is used and what isn’t. RapidFort automatically deletes code that isn’t used, which reduces the size, attack surface, and number of vulnerabilities related to that container. On average, we see an 80% reduction in container size and vulnerability counts. (It’s worth noting that RapidFort also allows for manual code deletion, custom profiles, and intelligent dependency tracking.)

For the Community Images, we use coverage scripts to exercise a container's normal operation. Pre-hardening container assessments are usually done by monitoring the production and pre-production operations of a container, but we don’t run all the containers we are hardening in the Community Images project. So, we use the coverage scripts and tell RapidFort to automatically delete what’s not used.

![ Source images are run through an optimization process that identifies and removes unused components from the image. You can contribute to this project by adding new images, improving coverage scripts, and adding regression and benchmark tests.](https://assets.website-files.com/6102f7f1589f98f317197b57/63fe13f356f1ef56fd26c632_5tWlCnVWtUYxbCAbZBBX_AS7Y_dHwgjL0ndO_Y9NfhRDLe_JYHoGzHSGn8F9NIlCqorF71hs3Cq-iUa_KaxyXW_ju1dplmM9tMn4W7OmSRof5ru3gMk3G-a-tTuSQi7kUJa1rLHVjhIZ1oRHVv2HFk0.png)

## Selecting the Right Containers for Hardening

We want to provide the biggest value possible to the container community, but we can’t simply take every popular image from Docker Hub and harden it. Containers must meet certain criteria before we select it to harden and give back to the community.

First things first: every container _can_ be hardened, but not every container is appropriate for general-purpose hardening. We look for what we call “leaf containers.” These are containers for which we know we can write coverage scripts to exercise their primary functions. We can’t do a general-purpose hardening of something as large as Ubuntu (an entire operating system) or Python (an entire programming language) because their use cases are too broad.

We differentiate leaf containers from base images by considering how and why people download them. Base images provide a foundation for an additional layer of deployed software. This layer is usually proprietary or customized to the organization deploying the container. As such, they would need their own customized coverages scripts to exercise the functionality they need to understand how to harden that container.

We look for containers with specific and bounded use cases. For example, it makes sense to harden NGINX or Redis because we have a good idea of what people need from those containers. We can write coverage scripts that exercise their main functions. Our coverage scripts are open source and anyone can contribute if they would like to see additional functions exercised. [Take a look at our NGINX coverage scripts](https://github.com/rapidfort/community-images/tree/main/community_images/nginx/bitnami).

## Transparency and Accountability

Open source software is built on transparency, accountability, and community involvement. We are big proponents of the open source software movement and want to give back more than we take. Not only are all of our hardened container images free for anyone to use, but anyone can see the results of our hardening process, improve our methodology, and be part of the effort.

![Every RapidFort hardened container comes with a detailed report of what happened and what was removed.](https://assets.website-files.com/6102f7f1589f98f317197b57/63fe13f35b7e1f731bba7472_uQZFjk7iphrS5fhKtC51c5FAuIPSYEcP7nNC18Q6N8aH5SRi5A_lhEnFDqa-9h2xBJaN9hObiUc_qxhQ5ja0Yfpr_3B7q-a2ArokIbh8E0spCqkD46gYJmD4IYMUFVyF7wiYz9pVrrum9HnBTyDh46s.png)

On our [Community Images Github readme page](https://github.com/rapidfort/community-images) is a table that shows all the images we provide with a link to the RapidFort optimization report (see the green “Get full report” button in the screenshot above). As of this writing, [the PostgreSQL RapidFort report](https://frontrow.rapidfort.com/app/community/imageinfo/docker.io%2Fbitnami%2Fpostgresql?utm_source=github&utm_medium=ci_view_report&utm_campaign=sep_01_sprint&utm_term=postgresql&utm_content=landing_get_full_report_button) shows:

-   The name of the original image (postgresql:12.11.0-debian-11-r15)
-   The date of the most recent hardening
-   The date we created the report
-   The usability status of the image
-   Attack Surface Reduction statistics
-   Vulnerability Removal statistics
-   Package Removal statistics

These statistics demonstrate the levels of optimization we were able to achieve using our coverage scripts.

![The RapidFort interface shows detailed statistics about container hardening and optimization.](https://assets.website-files.com/6102f7f1589f98f317197b57/63fe13f3d0d1296e863e0d3d_RfT_mBdm0UVm3axRjqA2J7_p77LSOTipfR4LuI4Y0pyblpCa-R0F1eqFoZBhFUjAQBxpG3lmwjE_oeB0u1ZR6CD9AXyjtZnNB8yi83YbnRKoMvDKNE9KptXzIcDBYdQf1hbMkyACrmGRXjrAGWGCjh0.png)

We also provide granular statistics and details about the vulnerabilities we’ve removed and which remain. You can see in this screenshot that:

-   The original image contained 104 vulnerabilities
-   The hardened image contains 36 vulnerabilities
-   68 vulnerabilities were removed
-   Of the remaining 36 vulnerabilities:

-   2 are critical severity
-   14 are high severity
-   17 are medium severity

The numbers above the bars in the chart on the right show the vulnerability counts in the original image.

![The RapidFort report shows exactly how many vulnerabilities were in the original image and the hardened image.](https://assets.website-files.com/6102f7f1589f98f317197b57/63fe13f3f04b7011e99e8087_RpVygZvgKbReNpMQth9szxgfVh1a8wbkJtwS9m7O4WeatDJ2eKdFyFSFnjpfIyU9JkTQNMplKVA8GOaxiGjWKxpBcF6qyonZYuz10bZCISyL0QhHhYeg5cJ3LUajYJDosDNqIq7OlLnBuwDqzCWMSfs.png)

## Securing Every Day

We use automation to track every time a popular container is updated. Within 24 hours, we download the new container, harden it, and release it. 

Vendors typically release new containers when they have new software versions to release or when there’s a new patch available. You can rest assured that our Community Images repository will always have the latest and greatest available with all of the known patches applied.

Our containers don’t contain any patchable vulnerabilities. Whatever is left in the image is left for the user to manage, but we take out a huge amount of the guesswork that typically goes into vulnerability management.

## Want to Help? We’d Love to Have You!

There are various ways you can get involved if you’d like to do so. The easiest thing to do is to start using our hardened images. We ask for nothing in return; we’re just hoping to make the internet safer.

If you’d like to be more actively involved, here are some options:

-   Give us a star on [the Community Images repo on GitHub](https://github.com/rapidfort/community-images) (click the “Star” icon in the top right), which helps increase the visibility and popularity of our project
-   [Join our Slack community](https://join.slack.com/t/rapidfortcommunity/shared_invite/zt-1g3wy28lv-DaeGexTQ5IjfpbmYW7Rm_Q) to receive support, request features, and talk directly with our developers
-   Submit a pull request or file an issue on GitHub
-   Share about the project with your peers at work or on social media

You can learn more about the RapidFort container hardening platform by looking through [the reports on the readme page](https://github.com/rapidfort/community-images#what-containers-are-supported), or you can [sign up for our free tier](https://frontrow.rapidfort.com/app/login) and get started with your own optimization efforts. Reduce your software attack surface by 80% or more with RapidFort.