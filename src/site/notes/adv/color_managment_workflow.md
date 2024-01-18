---
{"dg-publish":true,"permalink":"/adv/color-managment-workflow/","title":"Color Managment Workflow","noteIcon":""}
---


# Color management workflow (CMW) - (sRGB ACES)

  

## Introduce

  

> Reference:

>   https://chrisbrejon.com/cg-cinematography/chapter-1-color-management/#rendering-and-display-spaces

>   https://chrisbrejon.com/cg-cinematography/chapter-1-5-academy-color-encoding-system-aces/

  

* Eyes vs

  

![eyes_vs.png](https://cdn.jsdelivr.net/gh/aaronmack/image-hosting@master/e/eyes_vs.3bb7om2f6dm0.webp)

  

* All light will travel in a straight line unless something gets in the way and does one of the following:

  * Reflect it (like a mirror).

  * Refract it (bend it like a prism).

  * Disperse it (like gas molecules in the atmosphere).

* Light is the source of all colors. The importance of light in our lives is truly astounding. When a lemon appears yellow, it is because its surface reflects yellow, not because it is actually yellow. This used to confuse me, but pigments appear to have color because they selectively reflect and absorb certain wavelengths of visible light.

  

* OETF and EOTF

  

![OETF-EOTF.png](https://cdn.jsdelivr.net/gh/aaronmack/image-hosting@master/e/OETF-EOTF.223b38l9a15s.webp)

  

* 1976 Recap

![1976-recap.png](https://cdn.jsdelivr.net/gh/aaronmack/image-hosting@master/e/1976-recap.3yidv44zob60.webp)

  

> Standard

  

![Rec2020.png](https://cdn.jsdelivr.net/gh/aaronmack/image-hosting@master/e/Rec2020.1muv35u8nsrk.webp)

  

* sRGB for internet, Windows and camera photos.

* Rec. 709 has the same primaries than sRGB but differs on transfer function/gamma. This is because the target use of Rec.709 is video where it’s supposed to be viewed on a dim surround.

* DCI-P3 for cinema projectors.

* Rec. 2020, also called UHD TV, the future of colorimetry.

* AdobeRGB for printing projects.

  

![image](https://cdn.jsdelivr.net/gh/aaronmack/image-hosting@master/e/image.9x8sahkeer4.webp)

  

* Scene Linear Workflow

  

![image](https://cdn.jsdelivr.net/gh/aaronmack/image-hosting@master/e/image.2nk90mqrxfs0.webp)

  

## sRGB Primary Conversion

  

![image](https://cdn.jsdelivr.net/gh/aaronmack/image-hosting@master/e/image.68lvpciv6ew0.webp)

  

## To plot the gamut

  

![image](https://cdn.jsdelivr.net/gh/aaronmack/image-hosting@master/e/image.6qpt6umbqp40.webp)

  

## Reference

1. http://www.banjiajia.com/posts/82801

  

## Linear Workflow and ACEScg

  

IDT = Input Device Transform (The content of the conversion performed in ACEScg)

  

ODT = Output Device Transform (The result of adapting to the display)

  

RRT = Rendering Reference Transformation

  

![gamma.png](https://cdn.jsdelivr.net/gh/aaronmack/image-hosting@master/e/gamma.6viws34kg1w0.webp)

  

![ACEScg.png](https://cdn.jsdelivr.net/gh/aaronmack/image-hosting@master/e/ACEScg.70egzk1rhkk0.webp)

  

![gamma_math.png](https://cdn.jsdelivr.net/gh/aaronmack/image-hosting@master/e/gamma_math.624te0mzfb80.webp)

  

## ACES Color Space

  

* ACES 2065-1 is scene linear with APO primaries. It remains the core of ACES and is the only interchange and archival format (for DCDM).

* ACEScg is scene linear with AP1 primaries (the smaller “working” color space for Computer Graphics).

* ACEScc, ACEScct and ACESproxy all have AP1 primaries and their own specified logarithmic transfer functions.

  

## Transformation

  

```c

 TYPE                   GAMMA                   IDT

  

diffuse (COLOR)      sRGB (8bit)            Utility_sRGB_Texture

  

roughtness(DATA)     Linear (16/32 bit)     Utility_Raw

HDRi (COLOR)         Linear (16/32 bit)     Utility_Linear_sRGB

```

  

### RGB COLOURSPACE TRANSFORMATION MATRIX

  

https://www.colour-science.org:8010/apps/rgb_colourspace_transformation_matrix?input-colourspace=sRGB&output-colourspace=ACEScg&chromatic-adaptation-transform=Sharp&formatter=str&decimals=6

  

* sRGB -> ACEScg

  

```c

// CHROMATIC ADAPTATION TRANSFORM (Sharp)

[[ 0.614070  0.334950  0.051068]

 [ 0.070530  0.916420  0.013019]

 [ 0.020286  0.108171  0.871491]]

```

  

## Schema

  

![aces_schema.png](https://cdn.jsdelivr.net/gh/aaronmack/image-hosting@master/e/aces_schema.6pqyd52iopk0.webp)

  

1. On the left are files on your computer, textured, HDR, etc., from the Web, Photoshop, whatever. These files are sRGB and are standard files available to most users

2. In the middle is 3D software, such as Maya, which integrates scenes for rendering, and Nuke, which can do some compositing. The color space is ACEScg

3. The right side is where to view the image, the screen is sRGB, I can't save the image directly in Maya or Nuke, otherwise the resulting image will have problems, which is why I have to convert the ACEScg image to sRGB image.

  

## Versus

  

![sRGBvsACEScg.gif](https://cdn.jsdelivr.net/gh/aaronmack/image-hosting@master/e/sRGBvsACEScg.7iabt3cfpww0.gif)

  

## Definition

  

![aces_def.png](https://cdn.jsdelivr.net/gh/aaronmack/image-hosting@master/e/aces_def.5zu0ajz36v00.webp)

  

* Color space is defined by 3 things:

  * Primaries: Refers to the three vertices of the triangle shown in schema 1 (chessboard), this triangle is called gamut, note that it is not gamma.

  * Whitepoint: Refers to the space defined as the whitest in the triangle, and moving this point can make the picture warmer or cooler.

  * Transfer functions: This is gamma, and in schema 2, the red line is sRGB and the blue line is Linear

  

* Whitepoint

  * RGB color Spaces can have different white dots, depending on their context usage. This can be a creative option:

    * If you want to simulate the light quality of a standard observation room, choose the D50. Selecting a warm color temperature such as D50 will produce a warm white.

    * If you want to simulate midday daylight quality, choose D65. Higher temperature Settings (such as D65) will produce a slightly cooler white color.

    * If you prefer cool daylight, choose D75.

  

## Limitations

  

### Hue skews and Gamut Clipping

  

#### Abney effect

  

The Abney effect or the purity-on-hue effect describes the perceived hue shift that occurs when white light is added to a monochromatic light source

  

![image](https://cdn.jsdelivr.net/gh/aaronmack/image-hosting@master/e/image.3uuu50vagg80.webp)

  

An illustration of the Abney effect. The RGB primaries on a typical display are not monochromatic, making the effect weaker than in the usual experimental setup. However, it is usually still possible to see the effect in the blue example, with the middle shades appearing to be purple.

  

### The ODTs clip values

  

![image](https://cdn.jsdelivr.net/gh/aaronmack/image-hosting@master/e/image.1xyml4cw7xvk.webp)

  

### Gamut mapping

  

![image](https://cdn.jsdelivr.net/gh/aaronmack/image-hosting@master/e/image.7cej5drk1cg0.webp)

  

Interesting hue skews with the ACES ODT.

  

### Gamut Compress

  

* https://github.com/jedypod/gamut-compress

  

Gamut-compress is a tool which allows you to compress highly chromatic camera source colorimetry into a smaller gamut.

  

## Misc

  

### Delta E

  

* The Delta E value represents the difference between Display color and the original color standard for input content. The lower the Delta E value, the higher the accuracy, and the higher the Delta E value, the more obvious the color difference.

  * <= 1.0: The human eye cannot perceive the difference

  * 1-2: The difference can be perceived by careful observation

  * 2-10: The difference can be perceived at a casual glance

  * 11-49: Colors are more similar than opposite

  * 100: The color is completely distorted

  

### HDR

  

* 2084 - PQ 10 Curve

* 2086 - HDR 10

* 2094 -

* Screen Device - DELL U2720QM

  * Delta E<2, 99% sRGB, HDR 400, 95% DCI-P3, 99% REC709

  

### Chromatic Adaptation

  

When looking at a bright color for a long time, people will feel the brightness of the color slowly reduce, this phenomenon is visual color adaptation. The best time for color adaptation is about 5 ~ 10s. People's first impression of the light source color, that is, the initial color feeling, gradually weakens with the increase of the observation time of the object, so when observing color, pay attention to capturing the first impression and the initial color feeling.