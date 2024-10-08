
# Usage
## General
You just need to upload your image, enter 3 prompts, and click "Run". You can override prompts examples in Settings with your commonly using prompts. Don't forget to select inpaint checkpoint

Be sure you are using inpainting model

By default if a prompt is empty, it uses first prompt from examples. You can disable this behavior in settings for positive and negative prompts. Detection prompt can not be empty

You can detect few objects, just using comma `,`


## Advanced options

### Generation
![](/docs/images/advanced_options_generation.jpg)

- _"Do exactly the number of steps the slider specifies"_: actual steps num is steps from slider * denoising straight
- _"width"_, _"height"_: internal resolution on generation. 512 for sd1, 1024 for sdxl. If you increase, it will produce mutations for high denoising straight
- _"Correct aspect ratio"_: Preserve original width x height number of pixels, but follow generated mask's aspect ratio. In some cases can hide necessary context
- _"Upscaler for img2Img"_: which method will be used to fix the generated image inside the original image. It can be used instead hires fix. DAT upscalers are good. For example this is a good one: https://openmodeldb.info/models/4x-FaceUpDAT
- _"Rotation fix"_: fixes, if your photo is rotated by 90, 180 or 270 degree and it causes artifacts in detection and generation

### Detection
![](/docs/images/advanced_options_detection.jpg)

- _"Mask num"_: SegmentAnything generates 3 masks for 1 image. By default, it's selected randomly by seed, but you can override it. See which mask num was used in generation info

### Inpainting
![](/docs/images/advanced_options_inpainting.jpg)

- _"Padding"_: How much context around a mask will be passed into a generation. You can see it in the  live preview
- _"Denoising"_: 0.0 - original image (sampler Euler a), 1.0 - completely new image. If you use a low denoising level, you need to use `Original` in masked content
- _"Lama cleaner"_: remove the masked object and then pass into inpainting. From extension: https://github.com/light-and-ray/sd-webui-lama-cleaner-masked-content
- _"Soft inpainting"_: can be used instead of inpainting models, or with a small `mask expand` to not change inpainting area too much. E.g. change color. You need to set high `mask blur` for it!
- _"Mask mode"_: useful to invert selection (Replace everything except something). You need to set a negative `mask expand` for it.

### Others
![](/docs/images/advanced_options_others.jpg)

### Avoidance
- You can draw mask or/and type prompt, which will be excluded from the mask

### Custom mask
- If you want to use this extension for regular inpainting by drown mask, to take advantage of HiresFix, batch processing or controlnet inpainting, which are not able in img2img/inpaint tab of webui
- Or it can be appended to generated mask if `Do not use detection prompt if use custom mask` is disabled. Opposite of avoidance mask


## HiresFix
You can select the blurred image in the gallery, then press "Apply HiresFix ✨" button. Or you can enable `Pass into hires fix automatically`

Default settings are designed for using lcm lora for fast upscale. It requires lcm lora I mentioned, cfg scale 1.0 and sampling steps 4. There is no difference in quality for my opinion

Despite in txt2img for lcm lora DPM++ samplers produces awful results, while hires fix it produces a way better result. So I recommend "Use the same sampler" option

Note: hires fix is designed for single-user server

### Options - General
![](/docs/images/hiresfix_options_general.jpg)
- _"Extra inpaint padding"_: higher are recommended because generation size will never be higher then the original image
- _"Hires supersampling"_: 1.0 is the resolution of original image's crop region, but not smaller then firstpass resolution. More then 1.0 - multiplying on this number each sides. It calculates before limiting resolution, so it still can't be bigger then you set above

### Options - Advanced
![](/docs/images/hiresfix_options_advanced.jpg)
- _"Unload detection models before hires fix"_: I recommend you to disable it if you have a lot of vram. It will give significant negative impact on batches + `pass into hires fix automatically`


## Dedicated page
Dedicated page (replacer tab only) is available on url `/replacer-dedicated`

## ControlNet
![](/docs/images/controlnet.jpg)
[ControlNet extension](https://github.com/Mikubill/sd-webui-controlnet) is also available here. (Forge is discontinued. Revert to the last compatible version: `git checkout 3321b2cec451d`)

## Replacer script in txt2img/img2img tabs
![](/docs/images/replacer_script.jpg)

You can use it to pass generated images into replacer immediately


## Extension name
Replacer" name of this extension, you can provide it inside `ExtensionName.txt` in root of extension directory.

Or you can override it using the environment variable `SD_WEBUI_REPLACER_EXTENSION_NAME`

For example: Linux
```sh
export SD_WEBUI_REPLACER_EXTENSION_NAME="Fast Inpaint"
```

Or Windows in your `.bat` file:
```bat
set SD_WEBUI_REPLACER_EXTENSION_NAME="Fast Inpaint"
```
