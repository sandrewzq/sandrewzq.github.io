---
title:  (转载)Vscode 等宽字体
date: 2024-01-22
categories:
- Vscode
tags: 
- mono-font
- Vscode
---

# (转载)vscode等宽字体选择:更纱黑体

转自：[Vscode 等宽字体](https://jqtmviyu.github.io/p/vscode_mono_font/#sarasa-mono-sc-nerd)

## 下载链接

[GitHub \- be5invis/Sarasa-Gothic: Sarasa Gothic / 更纱黑体 / 更紗黑體 / 更紗ゴシック / 사라사 고딕](https://github.com/be5invis/Sarasa-Gothic)

## 一般安装哪些字体

一般下载微调字体\(不带unhinted\), 想部分安装就ttf, 选sc简体中文, 编程安装mono, 系统ui更改安装UI, 文章阅读安装gothic

## 为什么选择中英文等宽

两个英文符号宽度等于一个中文汉字宽度, 在编辑器或者终端里能很好地对齐

## Releases里的版本说明

### ttf和ttc

* ttf－TrueType字体文件格式
* ttc－TrueType字体集文件格式（更纱黑体按照字重进行分类，合并了相近ttf字体文件）

* 下载哪个\?

* 数量: ttf解压后几十个字体安装包, ttc下载解压后只有10个\(安装后在字体管理里能看到所有字体\)

* 大小: ttf全部安装比ttc体积大, 但能选择部分安装

***希望大而全的选ttc, 部分安装的选ttf***

### unhinted

无微调字体

> 字体微调（FontHinting）是一种字形渲染技术，主要用来对字体显示进行优化，解释起来可能复杂费解。简单说吧，它的主要目的就是在字体显示的时候通过某种技术，对字体的笔画曲线进行调整，将字体显示的更加美观。  
> 对linux来说，字体微调可以分为字体文件本身自带的微调和系统微调（freetype）。字体本身的微调是字体制作厂商在制作字体时，对单个字体进行的调整，在生成字体时将微调的信息加入字体文件中，这就是为什么我们看更纱黑体的微调字体比起无微调字体要大一些。系统微调就是依靠系统本身的字体渲染引擎对字体进行微调，在linux下主要是就是freetype。 字体微调的直观效果就是，在字体显示时，字体的笔画更加清晰可辨。但是字体文件本身的微调，需要针对一个一个字体进行单独调整。

***需要连字等效果的, 不要选无微调***

### Gothic/mono/term/UI/fixed

* 拉丁/希腊/西里尔字符集为 Inter
  * 引号 \( `“”`\) 为全角—— Gothic
  * 引号 \( `“”`\) 窄—— UI
* 拉丁/希腊/西里尔字符集为 Iosevka
  * 破折号（`——`）是全宽—— Mono
  * 破折号（`——`）为半宽—— Term
  * 不连字，破折号（`——`）为半宽——fixed

### cl/sc/tc/jk/hc

* cl－古典汉字字形
* sc－简体汉字字形
* tc－繁体汉字字形
* j－日文汉字字形
* k－韩文汉字字形
* hc－香港汉字字形

### slab

超厚笔划字形

### regular/italic/extralight/light/semibold/bold/extralightitalic/lightitalic/semibolditalic/bolditalic

* regular－常规字体
* italic－斜体字体
* extralight－超细字体
* light－细体字体
* semibold－半粗字体
* bold－粗体字体
* extralightitalic－超细斜体
* lightitalic－细斜字体
* semibolditalic－半粗斜体
* bolditalic－粗斜字体

## 不喜欢更纱里的英文\?

暂时只发现可以和 `ubuntu mono`结合使用

## vscode设置

```
"editor.fontLigatures": true, // 是否启用字体连字
"editor.fontFamily": "Sarasa Mono SC Nerd,Sarasa Mono SC",
```

### Sarasa Mono SC Nerd

> `Nerd fonts` 提供了很多图标字体，特别适合各种Shell/NeoVim/Emacs主题，例如 zsh 的 [`p10k`](https://github.com/romkatv/powerlevel10k), [`Powerline`](https://github.com/powerline/powerline) 等等。

[GitHub \- laishulu/Sarasa-Mono-SC-Nerd](https://github.com/laishulu/Sarasa-Mono-SC-Nerd)

[其他nerd-fonts](https://github.com/ryanoasis/nerd-fonts)

## 其他方案

1. [思源黑体hw](https://github.com/adobe-fonts/source-han-sans)
2. [JetBrains Mono](https://www.jetbrains.com/lp/mono/#intro) 不支持中文等宽
3. [Maple-font](https://github.com/subframe7536/Maple-font) : 不喜欢花体不要装italic \(推荐\)
