# Fossil_HR_E-Ink
A repository of useful Fossil Hybrid Watch stuff. Faces, documents, etc.

This repository is a central location for all my Fossil HR E-Ink watch face design stuff.

My goal is to create a Photoshop template that will make creating new faces easier. The work represented here should answer the main questions I have about the display:
Topics
1. What is the resolution of the display?
2. What is the tonal range of the display? (What value(s) in a file are displayed as white? What value(s) are displayed as black)
3. What is the tonal fidelity of the display? (Within the range, how far apart do values have to be in order to be discerned from one another)
4. How best to smoothly display gradients?
5. How best to display photos?

Research
1. Resolution: 1000px²

Observing other watch faces for download suggests a 1000x1000 pixel display. Initial testing for topics 2-4 assume this to be the case. Further research needed to verify. I suspect this value is lower.

2. Tonal range: 2% - 74%

Tonal range compression effectively reduces the display range to some values greater than zero and less than 100 to represent "fully white" and "fully black".

In the context of a bitmap display this would be a single threshold above and below which pixels would either be "on" or "off" usually 50%.

In the context of a grayscale display incapable of any method of interpolation, this would be an upper threshold and a lower threshold, any value below which or above which, respectively, would be represented as white or black. For instance, a 4-value display capable of White, 33%, 66% and Black might show any value between zero and 16% as white and any value between 75% and 100% as Black.

To find the thresholds, images with bars of varying intensity are placed against a white and black background. These bars are 1% apart so as to accurately find these values and "pin" them to the upper and lower thresholds of a design, ensuring that the maximum detail can be resolved at each end of the spectrum. Complicating this analysis is the E-Ink display's ability to interpolate between it's native values. (more on this later)

This becomes a somewhat subjective measure due to this interpolating feature. It varies based on viewing condition, total intensity, etc. To the best of my ability I have determined that the lowest value that can be effectively displayed without appearing solid black is about 74% and the highest value that can be effectively displayed without appearing solid white is about 2%.

3. Tonal fidelity: 3 native values

The E-Ink display in the Fossil Hybrid HR is grayscale in the sense that individual pixels can be somewhere between fully "on" and fully "off" but only at fixed intervals. These are referred to as "native values." To give the appearance of a continuous tone display, the E-Ink display is capable of displaying values between the native values via interpolation through some manner of dithering. (For a given non-native value, pixels of differing native values are placed in proximity to eachother to simulate that desired value.)

This is useful in that the display can now approximate a continuous tone display but is problematic in that these dithered areas can sometimes result in rather ugly distortion in solid areas.

In the case of the E-Ink display, there are three very clear native values. White, Black and one mid-tone value. The intensity values and their corresponding native value are listed in the table below.

<table>
  <tr>
    <td>Min</td>
    <td>Max</td>
    <td>Value</td>
  </tr>
  <tr>
    <td>0</td>
    <td>0</td>
    <td>White</td>
  </tr>
  <tr>
    <td>25</td>
    <td>60</td>
    <td>Midtone</td>
  </tr>
  <tr>
    <td>75</td>
    <td>100</td>
    <td>Black</td>
  </tr>
</table>

4. Gradients

Given the ability of the display to produce only 3 colors natively, the software driving the display must determine how best to interpolate between colors. The Fossil HR Hybrid E-Ink display does this via a type of FM screening. The type of frequency modulation used isn't particularly visually pleasing and very often forms visible diagonal lines. A truly smooth transition from white to black is best accomplished by indexing the colors in a grayscale image, using diffusion dither as the screening method. This allows the watch to make each pixel either black, white or midtone without forcing it to engage its dithering routine.

I haven't been able to generate a smooth gradient in this way due to the resolution question. The software is re-sampling the image before displaying it and therefore the pixels in the image aren't reproduced 1:1 to the display.
