---
title: Speed up image pulls in kubernetes
date: 2024-12-22T13:45:26+05:30
---

Its a good practice to have your docker/OCI images to be as small in size as possible, removing threat vectors and unneccesary tools and packages. 
But with rise in AI based services in all companies, we need contianers which might have models or other reasons to have larger images.

By default, kubernetes serializes image pulls i.e. if one image is being oulled from your OCI repo, next image will wait till its completion even though it might be a smaller image.
This can cause containers to be unavailable for a longer than expected duration.

Kuberntes has a solution for this in its configuration called `serializeImagePulls`

This option when disabled allows kubelet to pull images in parallel. 
We can also control the number of max images to be pulled in parallel using `maxParallelImagePulls` config.

If you want to dig deeper on its impletementation in kubernetes code you can look at the github code [here](https://github.com/kubernetes/kubernetes/blob/80379db5d5f4f9257a07627d5d4caec088db0c59/pkg/kubelet/images/image_manager.go#L65).




