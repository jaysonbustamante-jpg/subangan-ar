# Subangan ARchive System — Marker-based WebAR Prototype

This is a minimal, static **marker-based AR** prototype for the Subangan ARchive System concept (interactive digital archive + AR storytelling for Davao Oriental artifacts).

It uses:
- **A-Frame** (WebGL scene)
- **MindAR** (image tracking via webcam for QR code)
- A **QR code image** as the AR target
- **Green GitHub-inspired UI** (professional, lively design with animations)

## 1) Local testing (camera permissions)

**Important:** camera access (`getUserMedia`) generally requires a **secure context**:
- ✅ `https://...` (GitHub Pages)
- ✅ `http://localhost` (allowed by most browsers)
- ❌ `file://...` (opening `index.html` directly usually fails)

### Option A — Python (fast)
From the project folder:

```bash
python -m http.server 5173
```

Open:
- http://localhost:5173/

### Laptop testing checklist (Windows/macOS)

- Open the URL in a real browser: **Chrome** or **Edge**.
- Avoid testing camera inside an embedded preview (VS Code’s internal browser/webview may not prompt for camera reliably).
- If you get a permission error:
  - In your browser: click the 🔒 site icon → allow **Camera**.
  - In Windows: **Settings → Privacy & security → Camera**
    - Turn on **Camera access**
    - Turn on **Let desktop apps access your camera**
  - Close other apps using the camera (Zoom/Meet/Teams/Messenger).

### Option B — Node (if you have Node installed)

```bash
npx http-server -p 5173
```

Open:
- http://localhost:5173/

## 2) Deploy to GitHub Pages (recommended for prototype)

### Repo structure
This prototype is a **static site**, so the simplest structure is:

```
/ (repo root)
  index.html
  README.md
```

### Deployment steps
1. Create a GitHub repo (e.g. `subangan-archive-ar`).
2. Commit/push these files.
3. In GitHub: **Settings → Pages**
   - **Source**: `Deploy from a branch`
   - **Branch**: `main` and folder `/ (root)`
4. Wait for Pages to build, then open the Pages URL.

GitHub Pages is HTTPS, so camera permissions should work as long as the user grants access.

## 2.1) Using a QR code on phones (the typical demo flow)

Yes—this works on phones **as long as the QR code opens your GitHub Pages HTTPS URL**.

The correct flow is:
1. User scans a **QR code** (using the phone’s camera app).
2. The QR code opens the **GitHub Pages link** in a browser.
3. The browser prompts for **camera permission** → user taps **Allow**.
4. The AR page uses the camera feed to detect a **marker** (Hiro/custom) → then shows the 3D content.

Important: the **QR code itself is not the AR marker** in this prototype. The QR code is just a convenient way to open the website.

## 3) How to test QR code tracking

- Allow camera permission in your browser.
- Point the camera at the **QR code image** (printed or on another screen).
- When detected, the page shows the 3D model + text label.

### New UX: landing screen first

This prototype intentionally shows a small interface first (title + steps) and only starts the camera after the user taps **Start AR**.
This makes demos clearer and also helps with iOS/Safari permission rules (camera prompts generally behave better after a user gesture).

### UI Design

The interface features a **green GitHub-inspired design** for a professional look:
- Green accent colors (rgba(34, 197, 94)) for buttons and borders
- Subtle gradients and animations (background shifts, logo pulse)
- Responsive layout with panels and badges
- No distracting glow effects; focus on clean, lively aesthetics

### Showing your real artifact 3D model

This prototype displays a `.glb` model on top of the tracked QR code.

- Current demo model is a placeholder (`artifact.glb`).
- To use your Blender model:
  1. Export from Blender: File → Export → glTF 2.0 (.glb), select binary.
  2. Save as `artifact.glb` in the repo root.
  3. Re-deploy GitHub Pages.

### Setting up your QR code as the AR target

To use your own QR code for AR tracking:

1. **Generate QR code**: Create a QR code that encodes your GitHub Pages URL (e.g., using https://www.qr-code-generator.com/ or Python qrcode library).
   - For testing: I've created `qr-code.png` encoding `http://localhost:8000` (works on laptop, not phone).
   - For deployment: Generate new QR code with your GitHub Pages URL.
2. **Save image**: Save the QR code as `qr-code.png` in the repo root.
3. **Compile .mind file**: Use MindAR's compiler at https://hiukim.github.io/mind-ar-js-doc/tools/compile
   - Upload `qr-code.png`
   - Download the generated `.mind` file as `qr-target.mind`
   - Add `qr-target.mind` to the repo root.
4. **Update links**: The "View QR Code" button links to `./qr-code.png`.
5. **Re-deploy**: Push changes to GitHub Pages.

**Note:** No .patt file needed (that's for AR.js markers). MindAR uses .mind files for image tracking.

## 4) Recommended lightweight libraries (GitHub Pages friendly)

### Image tracking (for QR codes or posters)
- **MindAR** (image tracking; what this repo uses now)
  - Pros: tracks real images like QR codes, static hosting works
  - Cons: requires compiling targets to .mind files; tracking quality varies

## 5) Common camera gotchas (so it works during demo)

- Use **Chrome/Edge on Android** for easiest testing.
- iOS Safari can be stricter:
  - requires a user gesture for some permission prompts
  - iOS may show only 1 camera choice
  - keep the page simple and ensure HTTPS
- If camera prompt never appears:
  - check site permissions (camera blocked)
  - close other tabs using camera
  - ensure you’re not on `file://`

---

If you tell me what marker(s) you plan to use (Hiro/Kanji/custom) and what content you want to display (images, text, audio, 3D model), I can adjust the prototype to match your artifact storytelling flow.
