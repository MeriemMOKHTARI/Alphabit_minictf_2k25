# Key2QR

**`Author:`** [M3riLynx](https://github.com/MeriemMOKHTARI)

## Description

Glitches distort the truth, fragments are misplaced, and some parts remain **guarded**. Not everything is as it seems—can you **piece** it together and **grasp** what’s hidden?
## Solution

The idea is to **repair a corrupted GIF** to extract a **key** and the first part of the flag. This key is then used to **decrypt a locked QR code**, revealing the second part of the flag.

```bash
hexedit chall_corrupted.gif  # Fix the header (replace "XIF" with "GIF")
ffmpeg -i chall_corrupted.gif frame_%03d.png  # Extract frames
eog frame_*.png  # Find the key "RePa1rM3" and the first part of the flag
gpg --output part_two.png --decrypt part_two.png.gpg  # Unlock the QR code
zbarimg part_two.png  # Scan and retrieve the second part of the flag
```


