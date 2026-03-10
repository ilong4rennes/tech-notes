#### ✅ Step 1: Open Developer Tools

1. Right-click anywhere on the webpage with the embedded video.
2. Click **"Inspect"** (or press `F12` / `Ctrl + Shift + I`).
3. Go to the **Network** tab in the DevTools.

#### ✅ Step 2: Filter for Media Requests

1. Reload the page **(F5)** while the **Network tab is open**.
2. In the **Network tab**, click the **"Media"** filter (next to XHR, JS, etc.). This filters for video/audio files.
3. Watch the video on the webpage. This will force the browser to request the video file.
4. Look for a file with:
    - **Extension** like `.mp4`, `.webm`, `.m3u8`, or `.ts`
    - A large **size** (MBs), and **Type** as `media`, `video/mp4`, or similar.

#### ✅ Step 3: Find the Video URL

1. Click on the media file listed.
2. Under the **Headers tab**, you’ll see the **Request URL** — that’s the **direct video link**.
3. Right-click the **Request URL** and select **Copy link address**.

### ⬇️ Step 4: Download the Video

#### Option 1: Download using Browser

- Paste the video link into a new browser tab.
- If it starts playing in-browser, right-click the video and choose **"Save Video As..."**.

#### Option 2: Use `wget` or `curl` in Terminal

If it's a direct `.mp4` or `.webm` file:

```bash
wget "PASTE_VIDEO_LINK_HERE"
```

or:

```bash
curl -O "PASTE_VIDEO_LINK_HERE"
```

#### Option 3: For `.m3u8` streams (HLS format)

If the link is an `.m3u8` playlist, you can use `ffmpeg`:

```bash
ffmpeg -i "PASTE_M3U8_LINK_HERE" -c copy output_video.mp4
```

---

### 🔍 Based on Your Screenshot

The file you uploaded is a screenshot of the `Elements` tab. That doesn’t show the actual video URL. You need to follow the above steps in the **Network** tab.

---

If you're unsure which one is the video file, you can share a screenshot of the **Network tab after filtering by “Media”**, or just copy-paste the **Request URL** here, and I’ll confirm if it’s downloadable and help you further.


Got it — you're in the right place, but I can see that **nothing is showing up under the "Media" filter** in the Network tab. That usually means:

---

### 🛠 Possible Reasons & How to Fix:

#### 🔁 1. **You need to reload the page**

- While you're in the **Network tab**, click the **🔄 Reload** button (or hit `Cmd+R` / `Ctrl+R`).
- Make sure the **"Media"** filter is still selected (blue).
- Start playing the video on the page after reload.

> ⚠️ Don't switch away from the Network tab while doing this, or you might miss the request.

---

#### 🎥 2. **Video might be streamed using `.m3u8` (HLS)**

If it's an HLS stream, you might not see a `.mp4`, but instead see:

- `.m3u8` (master playlist or chunk list)
- `.ts` files (video segments)
- In this case, look in **"Other"** or **"All"** filter if nothing shows under "Media".

So try:

- Click **"All"** (instead of just "Media")
- Reload the page
- Play the video
- Look for any of these file types:
    - `.m3u8`
    - `.ts`
    - large file sizes (MBs)
    - type = `media`, `video/mp2t`, etc.

---

#### 🧪 3. **Still can't find it? Here's a trick:**

1. Clear the Network tab by right-clicking and choosing **Clear**.
2. Make sure **"Preserve log"** is checked ✅.
3. Then:
    - Reload the page
    - Hit **play** on the video
    - Let it buffer or play for a few seconds
4. Now search the Network tab again.

---

### 🚀 Once You Find the Video File

Let me know what kind of link you find (copy the **Request URL**), and I’ll show you how to download it properly — whether it's a direct `.mp4` or a streaming `.m3u8`.

Want to try that and show me what comes up in the Network tab now?


Perfect — you're dealing with an **HLS video stream**, which splits the video into multiple `.ts` segments, controlled by an `.m3u8` **playlist file**. No worries — we can stitch these into one video file using `ffmpeg`.

---

### ✅ Here's What You Need to Do

---

### 🔍 Step 1: Find the `.m3u8` Master URL

You need to find the file that ends in `.m3u8`. Here's how:

1. In the **Network** tab:
    - Select **“All”** (or **“Other”**) filter.
    - Reload the page and play the video again.
2. Look for a request ending with `.m3u8` — it could be called something like:
    - `master.m3u8`
    - `playlist.m3u8`
    - `index.m3u8`
3. When you find it:
    - Right-click > **Copy** > **Copy link address**

**💡 This is the master playlist that links all the `.ts` files.**

---

### ⬇️ Step 2: Download the Whole Video with `ffmpeg`

Once you have the `.m3u8` URL:

#### ✅ If you're on macOS / Linux / WSL:

```bash
ffmpeg -i "PASTE_THE_M3U8_URL_HERE" -c copy downloaded_video.mp4
```

#### ✅ If you're on Windows:

1. [Download FFmpeg](https://ffmpeg.org/download.html) (or install via `choco install ffmpeg` if you use Chocolatey).
2. Open **Command Prompt** or **PowerShell**.
3. Run:

```powershell
ffmpeg -i "PASTE_THE_M3U8_URL_HERE" -c copy downloaded_video.mp4
```

---

### 🛠 Notes:

- If the `.m3u8` URL is **relative** (e.g., `/video/playlist.m3u8`), prepend the site domain:  
    `https://example.com/video/playlist.m3u8`
- You can also test the `.m3u8` link in **VLC Media Player**:  
    `Media > Open Network Stream` — paste the link there.

---

### Want Help?

Paste the full `.m3u8` URL here (or a snippet), and I’ll check and give you the full download command ready to go.

Let’s grab that playlist URL and get your video downloaded! 🔥

The error indicates that **ffmpeg** isn’t installed on your system. You can install it easily on macOS using **Homebrew**. Here’s a step-by-step guide:

---

### 1. Install Homebrew (if not already installed)

If you don’t have Homebrew installed, open your Terminal and paste the following command:

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

Follow the on-screen instructions to complete the installation.

---

### 2. Install ffmpeg via Homebrew

Once Homebrew is installed (or if you already have it), run:

```bash
brew install ffmpeg
```

This command installs ffmpeg and its dependencies.

---

### 3. Verify Installation

After installation, verify that ffmpeg is available by typing:

```bash
ffmpeg -version
```

You should see version information if it’s installed correctly.

---

### 4. Download the Video

Now you can use your original command. In your Terminal, run:

```bash
ffmpeg -i "https://d2y36twrtb17ty.cloudfront.net/sessions/23be551f-78b2-44f1-ba88-ad6701375a67/f7ac18ce-e4d7-458f-bf62-ad6701375a6d-2d3f0d7f-923d-466c-9f0b-ad6701401890.hls/319889/index.m3u8" -c copy downloaded_video.mp4
```

This command tells ffmpeg to:

- `-i "URL"`: Use the provided `.m3u8` playlist as input.
- `-c copy`: Copy the codecs without re-encoding.
- `downloaded_video.mp4`: Save the combined video as an MP4 file.

---

### 5. Enjoy Your Downloaded Video

Once the process completes, you’ll have **downloaded_video.mp4** in your current directory.

If you encounter any further issues, feel free to ask!