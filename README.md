# Fossil_HR_E-Ink
My repository for useful Fossil Hybrid Watch stuff. Faces, documents, etc.

This repository is a central location for all my Fossil HR E-Ink watch face design stuff.

My goal is to create a Photoshop template that will make creating new faces easier. The work represented here should answer the main questions I have about the display:
Topics
1. What is the resolution of the display?
2. What is the tonal range of the display? (What value(s) in a file are displayed as white? What value(s) are displayed as black)
3. What is the tonal fidelity of the display? (Within the range, how far apart do values have to be in order to be discerned from one another)
4. How best to smoothly display gradients?
5. How best to display photos?

Research
1. Resolution: 240px²

Observing other watch faces for download suggested a 1000x1000 pixel display. Initial testing for topics 2-4 assumed this to be the case. Further research confirms a lower resolution of 240px X 240px. To find this value, I researched existing E-Ink displays, finding few 1:1 ratio screens but noting that smaller displays were in the neighborhood of 220ppi and the display size slightly over an inch gave me a ballpark figure. Fossil's specs claim a 1.1" (28mm) display dimension, which works out to 218ppi. All future work will use a Photoshop template with these specifications.

2. Tonal range: 2% - 75%

Tonal range compression effectively reduces the display range to some values greater than zero and less than 100 to represent "fully white" and "fully black".

In the context of a bitmap display this would be a single threshold above and below which pixels would either be "on" or "off" usually 50%.

In the context of a grayscale display incapable of any method of interpolation, this would be an upper threshold and a lower threshold, any value below which or above which, respectively, would be represented as white or black. For instance, a 3-value display capable of White, 50% and Black might show any value between zero and 33% as white and any value between 66% and 100% as Black.

To find the thresholds, images with bars of varying intensity are placed against a white and black background. These bars are 1% apart so as to accurately find these values. When designing art for the face, these values can be "pinned" to the upper and lower thresholds of a design, ensuring that the maximum detail can be resolved at each end of the spectrum. Complicating this analysis is the E-Ink display's ability to interpolate between it's native values. (more on this later)

This becomes a somewhat subjective measure due to this interpolating feature. It varies based on viewing condition, total intensity, etc. To the best of my ability I have determined that the lowest value that can be effectively displayed without appearing solid black is about 75% and the highest value that can be effectively displayed without appearing solid white is about 2%.

3. Tonal fidelity: 4 native values

The E-Ink display in the Fossil Hybrid HR is grayscale in the sense that individual pixels can be somewhere between fully "on" and fully "off" but only at fixed intervals. These are referred to as "native values." To give the appearance of a continuous tone display, the E-Ink display is capable of displaying values between the native values via interpolation through some manner of dithering. (For a given non-native value, pixels of differing native values are placed in proximity to eachother to simulate that desired value.)

This is useful in that the display can now approximate a continuous tone display but is problematic in that these dithered areas can sometimes result in rather ugly distortion in solid areas. Further complicating this analysis is the Fossil software's image effect application. All analysis was performed with the "Contrast" effect, which gave the most faithful reproduction of these native tones, eliminating interpolation almost entirely.

In the case of the E-Ink display, there are four very clear native values. White, Black and two mid-tone values. The intensity values and their corresponding native value are listed in the table below.

<table>
  <tr>
    <td>Min</td>
    <td>Max</td>
    <td>Value</td>
    <td>Range Mid-point (RGB)</td>
  </tr>
  <tr>
    <td>0</td>
    <td>24</td>
    <td>Black</td>
    <td>30, 30, 30</td>
  </tr>
  <tr>
    <td>25</td>
    <td>49</td>
    <td>Three-Quarter-Tone</td>
    <td>95, 95, 95/td>
  </tr>
  <tr>
    <td>50</td>
    <td>75</td>
    <td>Quarter-Tone</td>
    <td>160, 160, 160</td>
  </tr>
  <tr>
    <td>76</td>
    <td>100</td>
    <td>White</td>
    <td>225, 225, 225</td>
  </tr>
</table>

4. Gradients

Given the ability of the display to produce only 4 colors natively, the software driving the display must determine how best to interpolate between colors. The Fossil HR Hybrid E-Ink display does this via stochastic screening. The type of frequency modulation is selectable; when importing an image, you can choose from a number of image effects; each effect simply changes the method of interpolation, or dither style.

Classic, Exposure, Shadows and Soft all appear to be the same stochastic screening strategy, however some attributes (dark/light points, mid point, contrast) are shifted prior to screening.
<ul>
  <li>"Classic" appears to leave the image unchanged.</li>
  <li>"Exposure" moves the mid point of the image towards the shadows, sacrificing shadow detail for an expanded mid-range and better highlight detail.</li>
  <li>"Shadows" does the opposite, moving the mid point towards the highlights though this is a more subtle move.</li>
  <li>"Soft" decreases the contrast of the image, placing more image content in the midtone area.</li>
</ul>
The "Contrast" method does away with screening almost entirely, effectively posterizing the image and dividing the tonal ranges as described in section 3 above.

The "Carbon" method is a type of pattern dither where the spacing of pixels of differing native values are not stochastically placed, but instead geometrically.

For pure gradient reproduction and choosing from only the Fossil-supplied screening methods, I find the Carbon method to be the most pleasing to my eye, though without some tone compression there is poor contrast accross the range.

The most pleasing gradient representation I've found is to process the image in Photoshop as follows:
<ol>
  <li>Open image in photoshop and set the color mode to Grayscale</li>
  <li>Adjust levels, moving the mid-point towards the highlights about 20 points. (0.80)</li>
  <li>Save for Web using the FossilHRE-InkDisplay presets found in this repository</li>
  <li>Import the image into your watch and use the "Contrast" setting<li>
</ol>

5. Photos

For the most part, there's no harm in simply importing a photo and choosing the effect that looks the best for the photo. There's very little to be gained from pre-processing the image in Photoshop except in extremely high-key or low-key photos that can't be fixed with "Shadows" or "Exposure" in the app.

The drawback to this approach is that there will never be a pixel-for-pixel correlation to the display from your design. Again, for the most part that's not going to cause a problem. However there are occasions where that's necessary. When this is the case, I recommend using the method outlined in 4 above.
