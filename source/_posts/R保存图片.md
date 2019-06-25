---
title: R保存图片
date: 2019-06-09 16:10:46
tags:
  - R
  - 可视化
categories:
  - 搬砖随笔
description:
---

 

## 应用场景

通常我在数据分析的时候，使用Rstudio+Rmarkdown可以实时对数据可视化。一旦一块数据分析的代码成熟后，我会写成pipeline，但是仍然需要导出中间结果的可视化结果，以期对数据进行质控。我对图片质量的要求不高。因此直接采用`png`格式。

文档太长可以不看。给个例子

```R
png(filename = "demo.png", width = 8, height = 12, units = "in", res = 300)
# some plots
dev.off()
```

这里采用`inches`作为单位，是为了和Rmarkdown统一。此外，如果使用这一单位，必须指定分辨率。一般300dpi即可。

<!--more-->

## 批量绘图

```R
# On Rstudio
for (module_name in names(module.features)) {
  p.list <- list(
    FeaturePlotMeta(seu.tenX.add_module_score, features = module_name, reduction = "umap", title = paste0(module_name, " (10X)")),
    FeaturePlotMeta(seu.tenX.add_module_score, features = module_name, reduction = "tsne", title = paste0(module_name, " (10X)")),
    FeaturePlotMeta(seu.smart.add_module_score, features = module_name, reduction = "umap", 
                  title = paste0(module_name, " (Smart-seq2)")),
    FeaturePlotMeta(seu.smart.add_module_score, features = module_name, reduction = "tsne", 
                  title = paste0(module_name, " (Smart-seq2)"))
  )
  for (i in 1:length(p.list)) {
    png(filename = paste0("plots/human_gene_module/", module_name, "-", i,".png"), width = 6, height = 5, units = "in", res = 600)
    plot(p.list[[i]]) # 注意这里，一定要用plot，虽然目前我还不知道为什么
    dev.off()
  }
}
```



## 相关R文档

```R 
bmp(filename = "Rplot%03d.bmp",
    width = 480, height = 480, units = "px", pointsize = 12,
    bg = "white", res = NA, ...,
    type = c("cairo", "Xlib", "quartz"), antialias)

jpeg(filename = "Rplot%03d.jpeg",
     width = 480, height = 480, units = "px", pointsize = 12,
     quality = 75,
     bg = "white", res = NA, ...,
     type = c("cairo", "Xlib", "quartz"), antialias)

png(filename = "Rplot%03d.png",
    width = 480, height = 480, units = "px", pointsize = 12,
     bg = "white",  res = NA, ...,
    type = c("cairo", "cairo-png", "Xlib", "quartz"), antialias)

tiff(filename = "Rplot%03d.tiff",
     width = 480, height = 480, units = "px", pointsize = 12,
     compression = c("none", "rle", "lzw", "jpeg", "zip", "lzw+p", "zip+p"),
     bg = "white", res = NA,  ...,
     type = c("cairo", "Xlib", "quartz"), antialias)
```

### Arguments

| `filename`    | the name of the output file. The page number is substituted if a C integer format is included in the character string, as in the default. (The result must be less than `PATH_MAX` characters long, and may be truncated if not. See `postscript` for further details.) Tilde expansion is performed where supported by the platform. |
| ------------- | ------------------------------------------------------------ |
| `width`       | the width of the device.                                     |
| `height`      | the height of the device.                                    |
| `units`       | The units in which `height` and `width` are given. Can be `px`(pixels, the default), `in` (inches), `cm` or `mm`. |
| `pointsize`   | the default pointsize of plotted text, interpreted as big points (1/72 inch) at `res` ppi. |
| `bg`          | the initial background colour: can be overridden by setting par("bg"). |
| `quality`     | the ‘quality’ of the JPEG image, as a percentage. Smaller values will give more compression but also more degradation of the image. |
| `compression` | the type of compression to be used. Ignored for `type = "quartz"`. |
| `res`         | The nominal resolution in ppi which will be recorded in the bitmap file, if a positive integer. Also used for `units` other than the default, and to convert points to pixels. |
| `...`         | for `type = "Xlib"` only, additional arguments to the underlying `X11` device such as `fonts` or `family`.For types `"cairo"` and `"quartz"`, the `family` argument can be supplied. See the ‘Cairo fonts’ section in the help for `X11`. |
| `type`        | character string, one of `"Xlib"` or `"quartz"` (some macOS builds) or `"cairo"`. The latter will only be available if the system was compiled with support for cairo – otherwise `"Xlib"` will be used. The default is set by `getOption("bitmapType")` – the ‘out of the box’ default is `"quartz"` or `"cairo"` where available, otherwise `"Xlib"`. |
| `antialias`   | for `type = "cairo"`, giving the type of anti-aliasing (if any) to be used for fonts and lines (but not fills). See `X11`. The default is set by `X11.options`. Also for `type = "quartz"`, where antialiasing is used unless `antialias = "none"`. |