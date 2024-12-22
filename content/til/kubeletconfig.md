---
title: Speed up image pulls in Kubernetes
date: 2024-12-22T13:45:26+05:30
---

It's a good practice to keep your Docker/OCI images as small in size as possible by removing threat vectors and unnecessary tools and packages. However, with the rise in AI-based services across companies, we often need containers with larger images that might contain models or other essential components.

By default, Kubernetes serializes image pulls, meaning if one image is being pulled from your OCI repository, the next image will wait until its completion even if it's a smaller image.
This can cause containers to be unavailable for longer than expected durations.

Kubernetes has a solution for this in its configuration called `serializeImagePulls`. 
When this option is disabled, kubelet can pull images in parallel.

Additionally, we can control the maximum number of images to be pulled in parallel using the `maxParallelImagePulls` configuration.

If you want to dig deeper into its implementation in the Kubernetes codebase, you can look at the GitHub code [here](https://github.com/kubernetes/kubernetes/blob/80379db5d5f4f9257a07627d5d4caec088db0c59/pkg/kubelet/images/image_manager.go#L65).