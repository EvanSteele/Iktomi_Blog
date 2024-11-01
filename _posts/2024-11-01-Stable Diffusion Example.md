---
layout: post
author: Evan
---

This image was made using Stable diffusion 1.0 and the automatic1111 GUI. Prompt provided was "Birds taking flight", step count set to 20. This produced the grid of 20 512x512 images below, with each image being a single step taken by Stable Diffusion. 

Not shown here, Stable diffusion begins each image as pure noise, a binary blob broken into bytes used to render out the pictures. When a prompt is chosen, in this case "Birds taking flight", Stable Diffusion knows in it's heart that the pure static it's looking at is an image of birds, they're just a bit hard to see because the image is blurry. All it has to do is clear out the noise. 

It does it's first pass, trying to decide which bits to keep and which need to be flipped. Right away in the first image At the top of the image it's trying to boost up every third byte (blue) and at the bottom every second byte (green). it's hard to make out but we're starting to get an outdoor scene. Possibly a tree in the center By the third step we see our "tree" fading awaay and some dark splotches appear on screen. Dark splotches on a blue background are basially birds right? 

From here Stable Diffusion tries to narrow down on exactly what the birds are, how many, and where they're flying. I find it interesting how it isn't moving towards a single image, the birds and background shift every few images as it changes it's opinion on what is noise and what is part of the "real" image. 

![example](/assets/Diffusion demo.png)
