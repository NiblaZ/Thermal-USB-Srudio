# Thermal USB Printer Studio

A lightweight browser-based thermal printing tool for printing **PDFs and images directly to ESC/POS-compatible USB thermal printers** using the WebUSB API.

Thermal USB Printer Studio runs entirely in the browser and includes live monochrome preview, PDF page selection, image processing, saved printer presets, and support for common 58mm and 80mm thermal printers.

## Features

- Direct USB printing through **WebUSB**
- ESC/POS raster image printing
- PDF document support
- PNG, JPG, WebP, and other browser-supported image formats
- Multi-page PDF navigation
- Print current page, all pages, or a custom page range
- 58mm / 384-dot thermal paper support
- 80mm / 576-dot thermal paper support
- Live 1-bit thermal print preview
- Image rotation
- Adjustable crop / margin trimming
- Brightness adjustment
- Contrast adjustment
- Black & white threshold control
- Color inversion
- Multiple dithering algorithms:
  - Threshold
  - Floyd-Steinberg
  - Atkinson
- Automatic paper feed
- Automatic paper cutting
- Reusable printer profiles
- Saved presets using browser local storage
- English and Bahasa Indonesia interface
- Standard browser printing fallback
- Drag-and-drop file loading
- No backend or database required

## Requirements

For direct USB printing you need:

- A Chromium-based browser with WebUSB support
  - Google Chrome
  - Microsoft Edge
  - Opera
- An ESC/POS-compatible thermal printer
- A USB connection to the printer
- A printer interface exposing a compatible **Bulk OUT USB endpoint**
- HTTPS when hosted online

WebUSB does not work in every browser. Firefox and Safari currently do not provide general WebUSB support.

## Windows WebUSB Setup

Many thermal printers on Windows automatically use the normal Windows printer driver.

To communicate directly with the printer through WebUSB, you may need to change the USB driver to **WinUSB**.

One option is to use [Zadig](https://zadig.akeo.ie/).

Typical setup:

1. Connect the thermal printer to your computer.
2. Open Zadig.
3. Select **Options → List All Devices**.
4. Select your thermal printer.
5. Choose **WinUSB** as the target driver.
6. Click **Replace Driver**.
7. Open Thermal USB Printer Studio.
8. Click **Connect USB**.
9. Select the thermal printer from the browser USB device dialog.

> **Important**
>
> Replacing the Windows printer driver with WinUSB may prevent the printer from appearing as a normal Windows printer until the original driver is restored.
>
> If you prefer to keep the standard printer driver, use the application's **Standard Print** fallback instead.

## Usage

### 1. Connect the printer

Click:

**Connect USB**

Select your printer from the browser's USB device picker.

When successfully connected, the application will display the printer's:

- Device name
- Vendor ID
- Product ID
- USB endpoint

### 2. Load a document

Drag and drop a file into the upload area or click it to browse for a file.

Supported input includes:

- PDF
- PNG
- JPG / JPEG
- WebP
- Other image formats supported by your browser

A built-in test receipt is also available.

### 3. Configure paper size

Choose the thermal head width:

**58mm**

```text
384 dots
```

**80mm**

```text
576 dots
```

Actual printable width may vary slightly depending on your printer model.

### 4. Adjust the output

Available processing options include:

- Rotation
- Crop / margin trimming
- Threshold
- Brightness
- Contrast
- Invert colors
- Dithering method

The live preview shows approximately how the final 1-bit raster output will be sent to the printer.

## Dithering Modes

### Threshold

Best for:

- Receipts
- Text
- Barcodes
- QR codes
- Shipping labels

Pixels are converted directly to black or white according to the selected threshold.

### Floyd-Steinberg

Best for:

- Photographs
- Gradients
- Detailed graphics

Uses error diffusion to simulate grayscale using black and white dots.

### Atkinson

Best for:

- Graphics
- Illustrations
- High-contrast images

Produces a lighter diffusion pattern than Floyd-Steinberg.

## PDF Printing

For multi-page PDFs, you can select:

- Current page
- All pages
- Custom pages

Example custom range:

```text
1,3-5,8
```

This prints:

```text
1, 3, 4, 5, 8
```

## Printer Profiles

Custom configurations can be saved as printer profiles.

A profile can store settings such as:

- Paper width
- Dithering algorithm
- Threshold
- Brightness
- Contrast
- Invert mode
- Rotation
- Cropping
- Feed lines
- Automatic cutting

Saved profiles are stored using the browser's local storage.

This means profiles remain available after closing and reopening the application on the same browser and website origin.

> Profiles saved while opening the HTML file locally will not automatically transfer to a hosted domain because browser storage is isolated by origin.

## Language Support

The interface currently supports:

- English
- Bahasa Indonesia

The selected language is saved locally and restored when the application is opened again.

## Standard Print Fallback

If WebUSB is unavailable or you prefer to use your operating system's installed printer driver, click:

**Standard Print**

This opens the normal browser/system printing dialog using the thermal preview as the print source.

## ESC/POS Printing

Direct USB printing converts the processed canvas into an ESC/POS raster bitmap.

The application sends commands including:

```text
ESC @
GS v 0
ESC d
GS V
```

These commands are commonly supported by ESC/POS-compatible receipt printers.

Printer implementations vary, so features such as automatic cutting may not work on every model.

## Security

PDF files are processed locally in the browser.

PDF.js evaluation support is explicitly disabled when loading PDFs to mitigate the known PDF.js JavaScript evaluation vulnerability affecting older PDF.js releases.

No uploaded PDF or image needs to be sent to a backend server by this application.

## Deployment

The project is a static web application and does not require a backend.

You can host it using services such as:

- Vercel
- GitHub Pages
- Netlify
- Cloudflare Pages
- Any HTTPS static web server

### Vercel

The simplest deployment structure is:

```text
/
└── index.html
```

Rename the project HTML file to:

```text
index.html
```

Then deploy the repository to Vercel using the **Other** framework preset.

No server-side environment variables are required.

## Run Locally

You can open the HTML file directly in a browser for basic preview and image processing.

For functionality that requires secure browser APIs, it is better to serve the project through localhost.

Example with Python:

```bash
python -m http.server 8080
```

Then open:

```text
http://localhost:8080
```

For production WebUSB usage, deploy the application over HTTPS.

## Browser Compatibility

| Browser | Application UI | WebUSB Printing |
|---|---:|---:|
| Google Chrome | ✅ | ✅ |
| Microsoft Edge | ✅ | ✅ |
| Opera | ✅ | ✅ |
| Firefox | ✅ | ❌ |
| Safari | ✅ | ❌ |

Compatibility may vary depending on operating system, browser version, USB permissions, and printer firmware.

## Privacy

Document processing happens locally inside the browser.

The application does not require:

- User accounts
- Cloud uploads
- A database
- A backend API

Printer profiles and preferences are stored locally in the browser.

## Known Limitations

- WebUSB requires a compatible browser.
- Printer USB interfaces vary between manufacturers.
- Some printers may require WinUSB on Windows.
- Automatic cutting depends on printer support.
- ESC/POS command compatibility differs between printer models.
- Browser security policies can block WebUSB inside embedded iframes.
- Saved profiles are tied to the current browser and website origin.

## Tech Stack

- HTML
- JavaScript
- Tailwind CSS
- PDF.js
- WebUSB API
- Canvas API
- ESC/POS

## License

This project is licensed under the **MIT License**.

You are free to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of this software under the terms of the MIT License.

See the [`LICENSE`](LICENSE) file for the full license text.

## Disclaimer

Thermal USB Printer Studio is provided as a general-purpose printing utility.

USB configuration changes, including replacing printer drivers with WinUSB, should be performed carefully. Printer behavior and ESC/POS implementation can vary by manufacturer and model.

---

Built for fast, direct thermal printing from the browser.
