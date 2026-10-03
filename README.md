# EviTrace

> **Turn messy media into traceable evidence.**

EviTrace is an AI-powered media pipeline that transforms a messy collection of real-world photos and videos into organized, searchable, traceable visual evidence and a professional report.

Built for the **AI Media Pipeline Hackathon**.

---

## 1. Problem

People capture dozens or hundreds of photos and videos to document vehicle collisions, property damage, construction inspections, delivery disputes, and municipal incidents. However, this raw media is invariably:
* **Scattered and duplicated**: Multiple similar angles, burst shots, and identical copies across devices.
* **Mixed with low-value files**: Motion-blurred, occluded, or completely unrelated photos.
* **Difficult to interpret & organize**: Hard to determine which photo is the strongest evidence for each damage claim.
* **Disconnected from findings**: Traditional claim reports state conclusions without verifiable, traceable links back to specific original media sources.

---

## 2. Solution

EviTrace runs raw visual media through a structured, multi-stage processing pipeline:

```text
30 RAW FILES
      ↓
AI MEDIA PIPELINE
      ↓
24 USEFUL FILES
      ↓
6 EVIDENCE GROUPS
      ↓
1 TRACEABLE REPORT
```

Every observation and finding is mathematically and visually connected back to original source media stored securely in **Cloudinary**, answering the key question: **"Why is this evidence selected?"**

---

## 3. Media Pipeline Architecture

EviTrace visibly demonstrates this complete media transformation pipeline:

```text
RAW MEDIA
   │
   ▼
[ INGEST ] ───────► Cloudinary Direct Upload / Stream API
   │                - Automatic format optimization (f_auto)
   │                - Intelligent quality compression (q_auto)
   │                - Video poster frame extraction
   ▼
[ ANALYZE ] ──────► Multi-modal AI Vision Provider
   │                - Damage detection, object classification, caption generation
   ▼
[ FILTER ] ───────► Quality & Clarity Assessment
   │                - Resolution, sharpness, blur, and relevance scoring
   ▼
[ DEDUPLICATE ] ──► Exact Hash & Visual Similarity Matching
   │                - Duplicates tagged & preserved (never silently deleted)
   ▼
[ ORGANIZE ] ─────► Semantic Evidence Grouping
   │                - Clusters media by damaged component, scene overview, etc.
   ▼
[ CONNECT ] ──────► Signature Evidence Trace
   │                - Cryptographic & logical links from findings to source media
   ▼
[ REPORT ] ───────► Executive Evidence Report
   │                - Visual evidence grid + structured findings + timeline
   ▼
[ DELIVER ] ──────► Cloudinary Optimized Delivery & Print/PDF Export
```

---

## 4. Key Features

### 🌟 Signature Feature: Evidence Trace
Every single finding can be expanded with the click of a button:
* **"Why is this evidence selected?"**
  - ✓ Highly relevant to incident type
  - ✓ Clear visual evidence without occlusion
  - ✓ Corroborating viewpoint confirms extent
  - ✓ Not a duplicate or low-value asset
  - ✓ Directly supports the registered finding
* **Evidence Sources**: View thumbnails, filenames, and click **"View original"** to inspect the high-resolution master asset on Cloudinary.

### 🌐 Interactive Evidence Graph UI
A visual hierarchical graph connecting:
```text
                  CASE
                   │
       ┌───────────┼───────────┐
       ↓           ↓           ↓
    BUMPER       HEADLIGHT    DOOR
       │           │           │
    ┌──┼──┐       ┌┴┐       ┌──┼──┐
    ↓  ↓  ↓       ↓ ↓       ↓  ↓  ↓
   IMG IMG VID   IMG IMG    IMG IMG IMG
```
Clicking any finding automatically spotlights the corresponding evidence cluster and highlights its supporting media assets.

### 🔍 Duplicate Detection & Quality Scoring
* Computes file and visual hashes to isolate duplicate and near-duplicate assets.
* AI-assisted multi-factor quality scoring: **Clarity**, **Resolution**, **Relevance**, and **Overall Quality**.
* Preserves originals in a secondary "Duplicates / Low Quality" archive so no evidentiary trail is lost.

### ⏱️ Chronological Event Flow (Timeline)
* Organizes evidence along a verifiable timeline using media metadata / EXIF timestamps (or explicit upload sequencing if timestamps are absent).

### 📄 Executive Evidence Report & Print-to-PDF
* Complete structured report containing executive metrics, findings, evidence clusters, timeline, and an evidentiary disclaimer.
* Dedicated `@media print` stylesheet for instant, crisp PDF export and printing.

### ⚡ 1-Click Instant Demo Mode
* Judges can click **"Try Demo"** to instantly launch a pre-populated case (**Vehicle Collision — Highway 101**) with 15 realistic photos/videos, duplicates, blur examples, 6 evidence groups, timeline, and findings without needing any API keys.

---

## 5. Cloudinary Integration

Cloudinary is the backbone of the EviTrace media pipeline:
1. **Direct Ingestion**: Uploads images and videos via Cloudinary Upload Stream API.
2. **Dynamic Format & Quality Optimization**:
   - `f_auto`: Delivers WebP or AVIF automatically depending on the client browser.
   - `q_auto`: Balances forensic visual fidelity with lightning-fast load times.
3. **Video Poster Extraction**: Generates video thumbnail poster frames via Cloudinary video frame transformation (`so_1,w_600,h_400,c_fill,f_jpg,q_auto`).
4. **Responsive Thumbnail Generation**: Powers high-performance dashboard galleries without downloading multi-megabyte raw photos.
5. **Original Asset Delivery**: Maintains secure links to the pristine master assets for forensic verification.

---

## 6. AI Architecture

EviTrace employs a clean provider abstraction (`AIProvider` in `src/lib/ai/provider.ts`):
```typescript
export interface AIProvider {
  analyzeImage(imageUrl: string, filename: string, caseType: string): Promise<ImageAnalysisResult>;
  analyzeVideo(videoUrl: string, filename: string, caseType: string): Promise<VideoAnalysisResult>;
  generateReport(caseData: Case, groups: EvidenceGroup[], findings: Finding[], stats: ProcessingStats): Promise<string>;
  groupEvidence(caseType: string, mediaItems: MediaItem[]): Promise<{ groups: ... }>;
}
```
* **DemoAIProvider**: High-speed, deterministic offline provider for hackathon judging and testing.
* **Pluggable Architecture**: Easily swap in Google Gemini Vision (`gemini-2.5-flash` / `gemini-1.5-pro`) or OpenAI GPT-4o by setting `AI_PROVIDER=gemini` or `AI_PROVIDER=openai`.

---

## 7. Tech Stack

* **Framework**: Next.js 16 (App Router, Turbopack, React 19, Server Actions)
* **Language**: TypeScript (Strict typing across data layer, API, and UI)
* **Styling**: Tailwind CSS v4
* **Icons & Animation**: Lucide React, Framer Motion
* **Media Cloud**: Cloudinary SDK (Node.js v2)
* **Data Storage**: In-Memory Reactive Data Store with global singleton pattern

---

## 8. Getting Started

### Prerequisites
* Node.js 18+ or 20+
* npm or pnpm

### Installation

```bash
# Clone the repository
git clone https://github.com/your-username/evitrace.git
cd evitrace

# Install dependencies
npm install

# Run the development server
npm run dev
```

Open [http://localhost:3000](http://localhost:3000) in your browser.

---

## 9. Environment Variables

Create a `.env.local` file in the root directory (a template is provided in `.env.example`):

```env
# Cloudinary Configuration
CLOUDINARY_CLOUD_NAME=your_cloud_name
CLOUDINARY_API_KEY=your_api_key
CLOUDINARY_API_SECRET=your_api_secret

# AI Provider ("demo" | "gemini" | "openai")
AI_PROVIDER=demo
GEMINI_API_KEY=your_gemini_api_key
OPENAI_API_KEY=your_openai_api_key

# Application Settings
NEXT_PUBLIC_APP_URL=http://localhost:3000
NEXT_PUBLIC_DEMO_MODE=true
```

> **Note on Demo Mode**: If Cloudinary environment variables are left blank, EviTrace automatically operates in zero-dependency Demo Mode with deterministic sample assets so the app is always 100% functional locally.

---

## 10. Live Hackathon Demo Walkthrough (2–3 Minutes)

1. **0:00 — Landing Page**:
   - Open `http://localhost:3000`.
   - Explain the core transformation: **30 Raw Files → AI Pipeline → 24 Useful Files → 6 Evidence Groups → 1 Traceable Report**.
   - Click **"Try Demo"**.

2. **0:20 — Demo Initialization**:
   - The app instantly loads the pre-configured case: *Vehicle Collision — Highway 101*.
   - Explains: 15 media files uploaded from a phone at the accident scene.

3. **0:40 — Case Overview & Metrics**:
   - Highlight the pipeline metrics: 15 files received, 12 useful assets, 2 duplicates detected, 1 low-quality file flagged.

4. **1:00 — Evidence Graph & Evidence Trace**:
   - Switch to the **"Evidence"** tab.
   - Show the interactive graph (`CASE → Groups → Media`).
   - Click finding: **"Significant Front Bumper Deformation"**.
   - Notice how the graph highlights the exact supporting photos.
   - Click **"Why is this evidence selected?"** to reveal the verifiable selection checklist and click **"View original"** to show Cloudinary delivery.

5. **1:40 — Media Library & Filters**:
   - Switch to **"Media Library"**.
   - Filter by **"Duplicates"** (shows the 2 duplicate shots flagged).
   - Filter by **"Low Quality"** (shows the motion-blurred photo with Q: 35 score).
   - Search for **"door"** or **"bumper"** for instant semantic filtering.

6. **2:10 — Executive Evidence Report**:
   - Click **"View Report"** in the top navigation.
   - Review the structured executive summary, detailed findings with supporting photo cards, and timeline.
   - Click **"Print / Save as PDF"** to demonstrate print-ready output.

7. **2:30 — Conclusion**:
   > *"EviTrace doesn't just store media. It transforms raw media into traceable evidence."*

---

## 11. Future Roadmap

* **Insurance Claims Automation**: Direct integrations with Guidewire, Mitchell, and CCC ONE.
* **Property & Construction Inspection**: Multi-floor mapping with BIM model correlation.
* **Field Logistics & Delivery**: Automatic package condition verification at drop-off.
* **Forensic Blockchain Hashing**: C2PA content authenticity certification for legal court admissibility.

---

## 12. License

MIT License. Built for the AI Media Pipeline Hackathon.
