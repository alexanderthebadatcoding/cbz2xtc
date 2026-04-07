# XTEink Manga Tools 

A comprehensive suite of tools for converting various media formats (CBZ, PDF, and images) into the highly optimized XTC/XTCH format specifically designed for the XTEink X4 e-reader.
If you have images of the comics or manga you can zip the folder snd change thr extension from .zip to .cbz

These tools are designed to maximize the reading experience on e-ink displays by offering advanced dithering, efficient panel splitting, and fast rendering.

## Key Features
- **Multi-format Support:** Convert from archives (CBZ), documents (PDF), images.
- **Smart Formatting:** Auto-split landscape spreads into portrait panels, generate overviews, and support long-strip manhwa/webtoon scrolling.
- **E-ink Optimization:** Multiple dithering algorithms for e-ink limited bit depth.
- **High Performance:** Utilizes NumPy, Numba, and parallel processing for fast conversions.
- **Space Saving:** Optional LZ4 compression (`.xtcz`).

---

## The Tools

### `cbz2xtc.py`
Processes multiple pages and files in parallel. Ideal for standard manga and comic archives.
- **Split segments**: Automatically cuts landscape spreads into upright portrait pieces.
- **Overviews**: Generates full-page views to show the layout before the splits.
- **Fast Encoding**: Uses NumPy to process images quickly.

### `cbz2xtcpoppler.py`
An alternative PDF converter that uses Poppler for potentially better rendering on complex PDFs.

---

## Installation

### 1. Install Python
Ensure Python 3 is installed on your system.
- **Windows**: Download from [python.org](https://www.python.org/). *Crucial: During installation, ensure you check "Add Python to PATH".*
- **macOS**: Run `brew install python` or download from [python.org](https://www.python.org/).
- **Linux**: Usually pre-installed. If not, use `sudo apt install python3 python3-pip`.

### 2. Install Required Libraries
Open your terminal (Command Prompt/PowerShell on Windows, Terminal on macOS/Linux) and run:

```bash
pip install pillow numpy opencv-python ultralytics requests pymupdf numba
```

You can also try

```bash
pip install -r requirements.txt
```

If you have issues on mac try:

```bash
brew install pillow numpy opencv-python ultralytics requests pymupdf numba
```

### (Optional) If you encounter any issues with pip install:

First, ensure you have Python and pip installed.
Then consider using a virtual environment:
```bash
python3 -m venv venv && source venv/bin/activate
```
Then run
```bash
pip install -r requirements.txt
```
---
## How to Run

1. Place your source files (`.cbz`, `.pdf`, `.png`, etc.) in the main folder.
2. Open your terminal in that folder.
3. Run the appropriate script:

**Windows**:
```cmd
python cbz2xtc.py <options>
```

**macOS / Linux**:
```bash
python3 cbz2xtc.py <options>
```

### Method 1: OpenCV (Default)

Fast contour-based detection using traditional computer vision. No extra model files are needed.


```bash
python3 cbz2xtc.py --dither zhoufang --downscale lanczos --panel --rtl --2bit
```

### Method 2: YOLO (High Accuracy)

AI-based detection using YOLO. This requires a model file.

```bash
python3 cbz2xtc.py --2bit --dither zhoufang --downscale lanczos --panel --panel-model manga_panel_detector_fp32.pt --rtl
```

#### Panel extraction is triggered by adding the `--panel` flag to your `cbz2xtc.py` command.

#### `--2bit` will add higher resolution xtch files 

---

## Options Reference

### General Options
| Option | Effect |
| :--- | :--- |
| `--2bit` | Use 4-level grayscale (higher quality). Recommended for detailed artwork. |
| `--compress` | Compress output using LZ4 into an `.xtcz` file (saves storage space). |
| `--downscale <filter>` | Downscaling filter: `bicubic` (default), `bilinear`, `box`. |
| `--manhwa <overlap>` | Use long-strip mode (default 40% overlap) for webtoons (cbz only). |
| `--include-overviews` | Add an upright full-page preview before segments. |
| `--sideways-overviews` | Add a rotated full-page preview (-90 degrees). |
| `--gamma <value>` | Brighten/Darken the image. Use `<1` to brighten and `>1` to darken (Default: 1). |
| `--clean` | Delete temporary files after the conversion is done. |
| `--dither <algorithm>` | Dithering method: `stucki`(default), `atkinson`, `ostromoukhov`,`contrast-aware` `zhoufang`(recommended for e-ink), `stochastic`, `floyd`, `ordered`, `none`. |
| `--panel-conf <0.0-1.0>` | Set the confidence threshold (default: `0.40`). Use `0.6` or `0.7` to reduce duplicate detections of the same panel. |
| `--rtl` | Enable Right-to-Left panel sorting (standard for Japanese manga). |

## Finding Models

You can find pre-trained YOLO manga panel detection models on **[Hugging Face](https://huggingface.co/models?search=manga+panel+detection)**.
Search for `manga panel detection` and download the `.pt` file (YOLOv8 format).

This is based on code from [srokl cbz2xtc](https://github.com/srokl/cbz2xtc)