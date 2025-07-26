Got it! Here's the updated and optimized prompt for your AI coder that:

* Creates the Remotion video,
* Uploads it to Supabase Storage (`assets` bucket),
* Embeds it **behind the dashboard image** in the “Everything You Need” section,
* On **clicking the dashboard image**, the image will be replaced with the video player which loads from storage and plays.

---

### 🎬 Prompt: Create and Integrate Doclyft Intro Video with Lazy Playback from Supabase

#### 📌 Objective

Generate a polished, branded video using **Remotion** to introduce Doclyft and its core value. Upload the video to **Supabase Storage (`assets` bucket)** and embed it in the **“Everything You Need”** section of the landing page. On clicking the static dashboard image, dynamically load and autoplay the video from Supabase.

---

### 🎯 Tasks Breakdown

#### 1. 🎥 **Video Generation**

* Tool: [Remotion](https://www.remotion.dev/)
* Length: 35–40 seconds
* Export format: MP4 (1080p)
* Filename: `doclyft_intro.mp4`

##### 🧩 Video Structure

* Scene 1: Logo & Tagline
* Scene 2: Problem Statement
* Scene 3: What is Doclyft?
* Scene 4: Features Overview
* Scene 5: Developer Workflow (Web & CLI)
* Scene 6: Value Proposition
* Scene 7: CTA: Join Waitlist

##### 🎨 Visual Guidelines

* Background: `#11002C`
* Accent Color: `#7431E4`
* Typography: Sora / Inter (clean sans-serif)
* Effects: Animated text, radial grid background, code-themed transitions
* Keep consistent with existing visual identity

---

#### 2. ☁️ **Upload to Supabase Storage**

* Bucket: `assets`
* Path: `videos/doclyft_intro.mp4`
* Set to **public visibility**
* Retrieve public URL (e.g.:

  ````
  https://<supabase-project>.supabase.co/storage/v1/object/public/assets/videos/doclyft_intro.mp4
  ```)
  ````
* Store URL in a constant or `.env.local` if needed

---

#### 3. 🧠 **Update “Everything You Need” Section**

##### 🎯 Goal

* Keep static dashboard image visible by default
* On image **click**, replace it with a `<video>` player that loads and plays the video from Supabase

##### 💻 Implementation Outline

```tsx
const [playing, setPlaying] = useState(false);

{!playing ? (
  <img
    src="/images/dashboard_mockup.png"
    alt="Doclyft Dashboard"
    className="rounded-xl shadow-lg cursor-pointer"
    onClick={() => setPlaying(true)}
  />
) : (
  <video
    className="rounded-xl shadow-lg w-full"
    controls
    autoPlay
    muted
    playsInline
  >
    <source
      src="https://<supabase-url>/assets/videos/doclyft_intro.mp4"
      type="video/mp4"
    />
    Your browser does not support the video tag.
  </video>
)}
```

##### ✅ Styling

* Keep layout unchanged
* Preserve spacing and responsiveness
* Lazy-load video on click only (do not preload)

---

### 🧪 Testing Checklist

* [ ] Remotion renders final video without artifacts
* [ ] Supabase upload is public and video loads quickly
* [ ] Clicking dashboard image triggers video load and playback
* [ ] Video is responsive on desktop and mobile
* [ ] No visual regression to surrounding layout or styles

---

### 📦 Deliverables

* `remotion/DoclyftIntro.tsx` (video code)
* Exported `doclyft_intro.mp4` file
* Upload script or manual CLI steps to Supabase
* Updated `EverythingYouNeed.tsx` component with click-to-play logic
* Optional fallback thumbnail or error handling in `<video>`

---

Let me know if you’d like:

* A preview GIF as a hover animation
* Optional skip button to return to static image
* Tracking events (e.g., `video_played`) via analytics

This gives you the best of both worlds: fast-loading static dashboard visuals, and interactive video experience on user demand.
