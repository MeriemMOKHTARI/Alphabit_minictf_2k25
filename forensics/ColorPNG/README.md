# ColorPNG

**`Author:`** [M3riLynx](https://github.com/MeriemMOKHTARI)

## Description

I bet you’re great with PNGs.  
Can you uncover the secret hidden inside?

## Solution

If we read the description carefully, we notice keywords like **red**, **PNG**, and **hidden flag**. This hints at a technique related to image manipulation, particularly using the RGB values of the pixels. After a bit of research on image encoding and pixel-level data hiding, I found on **steganography** where data is hidden in the color channels of an image.

The idea here is that each pixel in an image is represented by three values: **R (Red)**, **G (Green)**, and **B (Blue)**. By modifying just the red channel of specific pixels, we can embed data (like ASCII characters) without visibly altering the image. This is because slight changes in pixel values are imperceptible to the human eye.

In this challenge, the flag was embedded in the red channel of the image. Each character of the flag was stored as the ASCII value in the red channel of consecutive pixels. A special termination marker (a pixel with a red value of `0`) was used to indicate the end of the flag.

To extract the flag, I wrote a script that iterates over all the pixels in the image. For each pixel, it reads the red channel value. If the value is within the ASCII printable range (32–126), it converts it into a character using `chr()` and appends it to a string. The script stops reading when it encounters the termination marker (red value `0`). Once all characters are extracted, the flag is reconstructed and displayed.

```python
from PIL import Image

# Load the encoded image
img = Image.open("spectrum_art.bmp")
pixels = img.load()

# Extract the flag from the red channel
flag = ""
for i in range(img.width * img.height):  # Iterate over all pixels
    x, y = i % img.width, i // img.width
    r, g, b = pixels[x, y]
    if r == 0:  # Stop if termination character is found
        break
    if 32 <= r <= 126:  # Check if Red channel contains ASCII character
        flag += chr(r)

print(flag)

