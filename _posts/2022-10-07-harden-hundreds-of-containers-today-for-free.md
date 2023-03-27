---
title: Harden Hundreds of Containers Today—for Free
canonicalurl: https://www.rapidfort.com/post/harden-hundreds-of-containers-today-for-free
---


## **Harden Hundreds of Containers Today—for Free**

Containerized infrastructure is awesome! We love it and we’ve built all of our services on it. The inevitable OSS vulnerabilities, on the other hand… Not so fun.

If you’re running containers in your infrastructure, chances are you’re also dealing with hundreds—if not thousands—of issues per container. It’s the major pitfall of modern day containerization: code you didn’t write with issues you can’t fix. And let’s face it: you probably don’t have the time or energy to deal with it. We have a great solution for this problem. 

Though we wish we had a magic wand that miraculously fixes all the vulnerabilities, what we have is actually not too far off. And it’s free to use on any containers you’re running today or anything you’re looking to launch in the future.

We’re thrilled to announce the free tier of RapidFort’s container optimization platform. It’s available right now, there’s no waiting list, and you can harden up to 300 containers in your first four months.

Here’s how to get started…

## **Create your free account on the RapidFort platform**

The first thing you’ll need to do is click the “Sign Up” button in the upper right corner of any page on RapidFort.com. We’ll collect a little bit of information about you (your name, where you work, and what you do there) and that’s it. Once that’s done, you’re in the platform right away.

Here’s what it’ll look like the first time you log in:



![](https://assets.website-files.com/6102f7f1589f98f317197b57/634091e7eb3b666c3ceb806a_D2Cb7ixPTpqVh2cpCnmP8MjKQgW-ElpNRXufcV8gTNQ5TwhanqSn8igbC_Pv8GisatNZ_ZShA0FbP-bxnbdDhJDZA82sebrjDa0r3VywJvGPQdDdk2lx6V3-FN5LmyaMdw9T9yZwdE0GHhP_rvwX4kP0FuMFtx7j0e1WdWzvvFpRb_RLaQAxWNLdFg.png)

And here’s what it’ll look like after you’ve scanned a bunch of containers:



![](https://assets.website-files.com/6102f7f1589f98f317197b57/634091e7b904beda5b1fe9e5_ubYpf_ykz_BjENVsKqtqCLF_AmBRdPUgzJ9Kxl7hRdGBqboeeW0DFUemttcrKHFDkQ25curMhQhqhmSf_nFwvbTmztbrxEEDHBPBEUvERDuCoCofhWh702jsusBAqgRdjE3icBhNhXlHC3Y161ZufpyDkvzfZ23DUi74xMzk_b16mjmUH2Ek5zTqHw.png)

On the left-hand side of the screen, you can click the “Quick Start” link to bring up helpful information to get you started using RapidFort’s software.



![](https://assets.website-files.com/6102f7f1589f98f317197b57/634091e7ebf97b82b107bed2_eoEqidQaHlk8gIgZhvAc5sZFU9hkD9mteEC33V6FjNMIIO_FhgWtbAXjfozQy-UaGpdKqOu1xNygboOOr0SiXy-81fIqiPJttB8KMgfrh3dJ5jph4Hxa2T9eIeTsc5RTSR7ko2lnlV-2IZpMQCE53GDaef4OzpdxpPD5ySVvaumbUyA7ZCEEkP5gBQ.png)

Let’s say you’re using Docker as your container system. When you click “Docker,” you’ll get a handy pop-up with a set of ordered instructions for your CLI. To make life easier, we’ve added a little “copy to clipboard” button at the end of every line, which you can safely paste (yes, we promise it’s safe) into your favorite terminal app. (We like iTerm 2, fwiw.)



![](https://assets.website-files.com/6102f7f1589f98f317197b57/634091e7e45e012476d6a5d1_NiycMJ81VPQnrsNN9SZIMJWvx-IEVDnTopI_fRJJwmc_c5N4jSnk1wxOmF08pdDsSJknqipK7E9AVfErqj9210UaO4BrV-AH7hRLw_7yg2RFMQOtuvItJg_D8Ao6mhQYNUr7hfKaHECISr5Aei3K-cjEKelWjwc6ub8_xp3B_WgT8dZg-BHAMTQ2GQ.png)

## **Instrument and understand your containers**

As you can see, the first step in the quickstart guide is to install the RapidFort CLI tools, which is done with a simple curl command. (It’s worth noting at this point that we do require Python and Docker to run RapidFort.) Once the tools are installed, you’ll use rflogin to log into the platform via your terminal session. Think of it like your git credentials, which live longer than your current terminal session.

When you install the [RapidFort CLI tools](https://docs.rapidfort.com/using-rapidfort/cli), you’ll get a set of applications local to your machine, including:

-   **rflogin:** Log into RapidFort
-   **rfstub:** Generate an instrumented image of your container
-   **rfharden:** Generate a hardened image from the instrumented container
-   **rfscan:** Scan one or more images or container registries for packages and vulnerabilities
-   **rfls:** List RapidFort stub and hardened images that are available on the client system
-   **rfjobs:** List all RapidFort jobs for the current user (including jobs that are not available on the client system)
-   **rfinfo:** Show detailed information and optionally download reports for a RapidFort job

In our quickstart guide, we walk you through hardening the nginx container because it’s relatively small and you can see the results in just a few seconds. So, we have you pull the latest nginx container from Docker Hub and then we have you install our software into the container. We call it “instrumenting the container” and we use the rfstub command to make that happen. Our software is ~30 megabytes, so at first you’ll see your stubbed image _increase_ in size by about 30MB, whether it’s a 100MB image or a 1GB image.

_Quick side note: We don’t actually install our software into the container you downloaded. We make a new container with the original container’s contents and bundle it with the RapidFort instrumentation software. This is why you’ll see \`nginx:latest-rfstub\` in the quickstart guide._

## Use rfstub to instrument and predict your container optimization

rfstub does a few cool things behind the scenes. First of all, it uploads your container to our platform and we scan it for known CVEs. We also generate a software bill of materials (SBOM), which is fully downloadable and inspectable within the web app. Most importantly, we create a statistical model of how many vulnerabilities we think we can remove from your image. In most cases, we can safely remove 70-80% of the vulnerabilities, which significantly increases the signal to noise ratio for vulnerability remediation.

Another important thing you get from the platform is the Rapid Risk Score (RRS), which uses machine learning to predict whether a Proof of Concept (POC) will exist for a known CVE within the next three months. The RRS makes prioritization and remediation decisions a breeze because it factors in real-world data and research from various leading security organizations.



![](https://assets.website-files.com/6102f7f1589f98f317197b57/634091e73ef0a5815330b6f8_zyX5DheStSdOup6_As65qavruCImTCNqoObKEAIBCwmzKgT6OJBRmJRUUI2_8LXMS1dSgZGcYU_YyNyBvWgiRQzVi4w-SQ96q1mLQt6TQ182-F9k3jHyeF6y0-Z3R7nSxuqRw4UFORvb4wyNMF5_WxmNsxBLMouQi3CNVOJCSiVFfmfOnjdth6gX8Q.png)

![](https://assets.website-files.com/6102f7f1589f98f317197b57/634091e7b9d4ce1122982640_pX1hjeOOm1TgN6eGy_GL-so0hVwYOWiyb4rutGujUSrGGnum7y56w7PG99YE6VXIDUAZ0bhTkHju6d5utBq8vEnfHeeUBX_5M0kuIVmpD-o28Kfr3CA2wdg9FNLMST8BKHQCoAHk5b2vzYo0cwc7YHcKy9F9Ez93f5ekaFSXCO1sNz7ZugQ4speTGQ.png)

![](https://assets.website-files.com/6102f7f1589f98f317197b57/634091e84f8ff755a0eec29e_3dGC70pZ8eMjFJTPkvAdJ_V3fQi5is5rzbVovuMGIKsCI3cygLHFn3pSBlIXA_rLIkRo_gopIAdnGoWomBvfeSYiKTIeiVjuzCXDHyanzTveR_pPrr8n6_6yueDOxC0HZ0lXfEYWza6bawVmuGDMXfBBsLgE-kosDfuajjGrNgUezBfcO4-RH8XOMg.png)

## **Profile and harden your container activity**

As soon as you launch your profiled image, it’s ready to send profiling information over to RapidFort. There are a couple of ways to profile your container so RapidFort knows exactly what code can be safely removed.

### Profile your image with a coverage script

The first method is running a coverage script. This is a great option for a pure-automation play in your CI/CD pipeline. Coverage scripts are easy to write and many of ours are fewer than 30 lines of code. In fact, you can look at the coverage scripts we’ve written for our [free, hardened community images on GitHub](https://github.com/rapidfort/community-images/). For example, here’s our [Bitnami Apache2 coverage script](https://github.com/rapidfort/community-images/blob/main/community_images/apache/bitnami/coverage_script.sh).

The point of a coverage script is to exercise all the functionality your end users are likely to run on your container. The better the coverage script, the more confidence you can have in your hardened container. In fact, [one of our customers](https://rapidfortteam.webflow.io/post/case-study-customs-bridge-relies-on-rapidfort-for-proactive-security) used this situation to strengthen automation pipelines and coverage scripts.

### Profile your image with real-world execution

The second method of profiling your container image is with real-world execution. This is a great option if you don’t have full automation capabilities or you’re running a product with a bigger scope than you may be able to test. You can even use this method to run an instrumented container in production, capture everything that happens in a few days (or weeks), and let RapidFort identify what’s being used and what isn’t.

Even if you do have full automation capabilities and a great CI/CD pipeline, this is a really handy option if you just want to know about the _relevant_ security risks in your container. We frequently help our customers reduce their known vulnerabilities from unmanageable numbers down to something that’s manageable. Your infrastructure right now might have millions of vulnerabilities and you might have no idea where to start. Getting that down several orders of magnitude and having a Rapid Risk Score help with prioritization can make a big difference in actionable security mitigation.

### Use the profile to harden

Once you’ve captured the actual usage of your container (whether using a coverage script or capturing real-world information), you can tell RapidFort to do its thing: get rid of all the software components you don’t need.

The RapidFort platform looks at the profile data and quickly eliminates all the unused components. Within seconds, you’ll have a hardened and optimized container, which you can further secure with patches that you know will be relevant to the code in your app.

## **Get started today with our free tier**

Our free tier is super easy to start using. We don’t collect a credit card and we let you harden hundreds of containers without any cost. Learn more about the free tier features on [our pricing page](https://www.rapidfort.com/pricing).

If you want a demo, you can either [watch it](https://vimeo.com/752099847) here or you can book one with a RapidFort engineer. We are more than happy to show off what we’ve built and we love working with people. Check out our [case study with Customs Bridge](https://rapidfortteam.webflow.io/post/case-study-customs-bridge-relies-on-rapidfort-for-proactive-security) and hear how we directly supported them on their fast journey to a secure production environment.

## **Join our Slack Channel**

You should join our [slack channel](https://join.slack.com/t/rapidfortcommunity/shared_invite/zt-1g3wy28lv-DaeGexTQ5IjfpbmYW7Rm_Q) so that we can support you