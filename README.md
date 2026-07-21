#!/usr/bin/env python3
"""
Generates the full "Project Genesis" repo structure:
- one folder per domain/category
- one .md file per project idea inside its folder
- one README.md per category (index of that category's projects)
- one root README.md (master index, contribution guide, license, hall of fame)
"""

import os
import re

ROOT = "/home/claude/project-genesis/repo"

def slugify(name):
    name = name.lower()
    name = re.sub(r"[^a-z0-9\s-]", "", name)
    name = re.sub(r"\s+", "-", name).strip("-")
    return name

def project_md(idx, emoji, title, tagline, what, why, stack, features, difficulty):
    stack_lines = "\n".join(f"- {s}" for s in stack)
    body = f"""# {emoji} {idx} — {title}

> {tagline}

**Difficulty:** {difficulty}

[⬅ Back to category index](./README.md) · [⬅ Back to main index](../README.md)

---

## What to Build

{what}

## Why It's Unique

{why}

## Suggested Stack

{stack_lines}
"""
    if features:
        feat_lines = "\n".join(f"- {f}" for f in features)
        body += f"\n## Key Features\n\n{feat_lines}\n"
    return body

# =========================================================================
# CATEGORY DEFINITIONS
# Each category: (folder_slug, display_name, emoji, intro_blurb, [projects])
# Each project: dict with idx, emoji, title, tagline, what, why, stack, features, difficulty
# =========================================================================

categories = []

def cat(folder, name, emoji, intro, projects):
    categories.append({
        "folder": folder,
        "name": name,
        "emoji": emoji,
        "intro": intro,
        "projects": projects,
    })

# -------------------------------------------------------------------------
# 1. AI & Machine Learning
# -------------------------------------------------------------------------
cat(
    "01-ai-machine-learning",
    "AI & Machine Learning",
    "🧠",
    "Projects that push AI beyond chatbots — into personalization, restoration, diagnosis, and argument.",
    [
        dict(idx="1.1", emoji="🧠", title='"Ghostwriter" — Your Writing Style Cloner',
             tagline="A tool that learns your personal writing style and generates content that genuinely sounds like you.",
             what="A tool that learns your personal writing style (tone, vocabulary, sentence structure, humor patterns) from your past emails, blogs, or notes, then generates new content that genuinely sounds like *you* wrote it.",
             why="Most AI writing tools sound like ChatGPT. This one sounds like *you*. It captures your quirks, your inside jokes, your specific way of starting sentences.",
             stack=["Python (fine-tuning LLaMA/GPT-2 on personal corpus)", "FastAPI for the backend", "React + Monaco Editor for the writing interface", "Vector DB (Pinecone/Chroma) for style memory"],
             features=["Ingest emails, Markdown files, social posts", '"Style intensity" slider (how much like "you" vs. generic)', "Side-by-side comparison: AI vs. your actual writing", "Export as email drafts, blog posts, social threads"],
             difficulty="⭐⭐⭐⭐ (Intermediate-Advanced)"),
        dict(idx="1.2", emoji="🎙️", title='"Voice Archaeologist" — Audio Time Machine',
             tagline="Restore, transcribe, and narrativize old family recordings.",
             what="Upload an old family recording (cassette, voicemail, childhood tape), and the AI restores audio quality (denoise, de-hiss), transcribes with speaker diarization (\"Grandma said... Dad said...\"), generates a searchable timeline + emotional sentiment graph, and creates a \"family podcast\" narrative from scattered clips.",
             why="Preserves human history, not just data. Emotional + technical.",
             stack=["Python (Whisper for transcription, Demucs for audio separation)", "FFmpeg for audio processing pipeline", "Next.js + Tailwind for the storytelling UI", "Supabase for storage & metadata"],
             features=[],
             difficulty="⭐⭐⭐⭐⭐ (Advanced)"),
        dict(idx="1.3", emoji="🌿", title='"Plant Whisperer" — Computer Vision for Plant Health',
             tagline="Diagnose plant health issues from a phone camera, not just identify the species.",
             what="Point your phone camera at any plant. It doesn't just identify the species — it diagnoses *health issues* (nutrient deficiency, pest damage, overwatering) by analyzing leaf color patterns, edge curling, spot patterns, and growth direction.",
             why='Goes beyond "this is a rose" to "your rose has magnesium deficiency — here\'s why and how to fix it."',
             stack=["TensorFlow/PyTorch (custom CNN for disease classification)", "React Native or PWA for mobile camera", "Node.js + MongoDB for plant care database", "OpenCV for image preprocessing"],
             features=[],
             difficulty="⭐⭐⭐⭐ (Intermediate-Advanced)"),
        dict(idx="1.4", emoji="🎭", title='"Debate Simulator" — AI That Argues Both Sides',
             tagline="Two AI personas debate any topic in real time while you judge.",
             what='Input any controversial topic. Two AI personas debate it in real-time, citing sources, adapting their arguments based on the other\'s points. You can pause, ask questions, or switch to "judge mode" where you decide who won.',
             why="Educational tool that teaches critical thinking by showing *how* to argue, not just *what* to think.",
             stack=["Python (LangChain + multiple LLM instances)", "React for the debate stage UI", "WebSocket for real-time back-and-forth", "Firestore for debate history & ratings"],
             features=[],
             difficulty="⭐⭐⭐⭐ (Intermediate-Advanced)"),
        dict(idx="1.5", emoji="🔮", title='"Memory Palace Builder" — Spaced Repetition Meets Spatial AI',
             tagline="AI generates a personal 3D memory palace from anything you need to memorize.",
             what="Feed the tool a list of facts, vocabulary, or exam material. It generates a walkable 3D 'memory palace' — a virtual building where each room, object, and vivid AI-generated scene encodes one fact — and quizzes you by walking you through it, using spaced repetition to resurface weak rooms.",
             why="Combines the ancient method-of-loci technique with modern spaced repetition and generative imagery, instead of yet another flashcard app.",
             stack=["Three.js/React Three Fiber for the 3D palace", "Stable Diffusion for scene generation", "Python backend for spaced-repetition scheduling", "IndexedDB for offline progress"],
             features=["Auto-generates room layouts from imported study material", "Adaptive difficulty based on recall speed", "Shareable palaces for study groups", "VR mode via WebXR"],
             difficulty="⭐⭐⭐⭐⭐ (Advanced)"),
    ],
)

# -------------------------------------------------------------------------
# 2. Developer Tools & Productivity
# -------------------------------------------------------------------------
cat(
    "02-developer-tools-productivity",
    "Developer Tools & Productivity",
    "🛠️",
    "Tools that make the daily grind of software development — git history, dependencies, commits — feel less like chores.",
    [
        dict(idx="2.1", emoji="🔍", title='"Code Archaeologist" — Visual Git History Explorer',
             tagline="Explore your repo as an interactive archaeological dig.",
             what='Instead of `git log`, explore your repository as an *interactive archaeological dig*. See code "fossil layers" — which files were born when, which functions evolved from others, which developers "inhabited" which parts of the codebase.',
             why="Makes git history feel like exploring ancient ruins. Visual, intuitive, and reveals team dynamics.",
             stack=["Rust/Go for fast git parsing", "D3.js or Three.js for 3D visualization", "Electron or Tauri for desktop app", "SQLite for indexed commit metadata"],
             features=['"Strata view": files organized by birth date', '"Migration patterns": which code moved where over time', '"Extinction events": when was that function last used?', "Team heatmaps: who owns what over time"],
             difficulty="⭐⭐⭐⭐ (Intermediate-Advanced)"),
        dict(idx="2.2", emoji="🧩", title='"API Frankenstein" — Mashup Generator',
             tagline="Automatically generates meaningful mashup apps from 2-3 public APIs.",
             what='A tool that takes 2-3 public APIs and automatically generates a *meaningful* mashup app with working code. Not random — it finds logical connections (e.g., Spotify + Weather = "Rainy Day Playlist Generator").',
             why='Removes the "blank page" problem. Shows developers what\'s possible by connecting existing services.',
             stack=["Node.js + OpenAI API for \"connection logic\"", "CodeSandbox/StackBlitz API for live previews", "Next.js for the generator UI", "Redis for caching API schemas"],
             features=[],
             difficulty="⭐⭐⭐ (Intermediate)"),
        dict(idx="2.3", emoji="📝", title='"Commit Poet" — Git Commit Message Art',
             tagline="Turns your diffs into descriptive haikus, limericks, and sonnets.",
             what='Analyzes your code changes and writes commit messages as *actual poetry* — haikus, limericks, sonnets — while still being descriptive. "Fixed bug in auth" becomes "In login\'s dark night / A token found its way home / Users breathe again."',
             why="Makes the most boring part of coding delightful. Teams actually look forward to reading commit history.",
             stack=["Python (GitPython + OpenAI/Claude API)", "CLI tool (install via npm/pip)", "Optional: VS Code extension", "Pre-commit hook integration"],
             features=[],
             difficulty="⭐⭐ (Beginner-Intermediate)"),
        dict(idx="2.4", emoji="🗺️", title='"Dependency Cartographer" — Visual Package Map',
             tagline="See your dependency tree as a living, breathing city.",
             what="Instead of `npm ls`, see your dependencies as an *actual map*. Your project is a city. Each package is a building. Popular packages are skyscrapers. Vulnerable packages are crumbling ruins. Unused deps are abandoned ghost towns.",
             why="Makes abstract dependency trees tangible and reveals bloat/security issues at a glance.",
             stack=["JavaScript/TypeScript for package parsing", "Three.js or PixiJS for the 3D/2D map", "Vulnerability data from OSV/NVD APIs", "Electron for desktop, or web-based"],
             features=[],
             difficulty="⭐⭐⭐ (Intermediate)"),
        dict(idx="2.5", emoji="⏳", title='"Meeting Fossil Record" — Standup Notes to Living Changelog',
             tagline="Turns scattered standup notes and PR comments into an auto-maintained project changelog.",
             what="Ingests daily standup notes, PR descriptions, and Slack threads, then uses an LLM to continuously synthesize a living, human-readable changelog and decision log for the project — so 'why did we do it this way' always has an answer.",
             why="Institutional knowledge usually dies in Slack scroll-back. This keeps a queryable, chronological record without anyone having to write it by hand.",
             stack=["Node.js ingestion pipeline", "LLM summarization + embeddings for search", "Postgres + pgvector", "Next.js dashboard with timeline view"],
             features=["Searchable 'why was this decided' queries", "Auto-links decisions to related commits/PRs", "Weekly digest email", "Slack bot for inline querying"],
             difficulty="⭐⭐⭐ (Intermediate)"),
    ],
)

# -------------------------------------------------------------------------
# 3. Web & Browser Experiments
# -------------------------------------------------------------------------
cat(
    "03-web-browser-experiments",
    "Web & Browser Experiments",
    "🌐",
    "Extensions and experiments that reveal the web's hidden history, patterns, and personality.",
    [
        dict(idx="3.1", emoji="🌐", title='"Browser Time Capsule" — Visit the Web of the Past',
             tagline="See any URL side-by-side with its version from 5, 10, or 15 years ago.",
             what="A browser extension that, when you visit a modern website, shows you what that *exact URL* looked like 5, 10, 15 years ago via the Wayback Machine — side by side with the current version.",
             why="Web archaeology. See how design trends, technologies, and content evolved. Great for design inspiration and nostalgia.",
             stack=["Browser Extension API (Manifest V3)", "Internet Archive API", "React for the side-by-side comparison UI", "Tailwind for styling"],
             features=[],
             difficulty="⭐⭐ (Beginner-Intermediate)"),
        dict(idx="3.2", emoji="🎨", title='"CSS Haiku" — Art from Stylesheets',
             tagline="Generative art built from tightly constrained CSS rules.",
             what='A generative art tool where you write CSS rules (limited set) and it creates visual art. Think: "3 divs, 2 animations, 1 gradient" challenge. Gallery of community submissions. Weekly challenges.',
             why="Constraints breed creativity. Turns CSS — usually a utility — into an artistic medium.",
             stack=["React + Monaco Editor for CSS input", "iframe sandbox for live preview", "Node.js + MongoDB for gallery & voting", "Canvas API for export"],
             features=[],
             difficulty="⭐⭐ (Beginner-Intermediate)"),
        dict(idx="3.3", emoji="🔗", title='"The Breadcrumb Trail" — Visual Browsing History',
             tagline="Your browser history as an interactive network graph of your curiosity.",
             what='Your browser history as an *interactive network graph*. Each site is a node. Links between them are edges. See your "browsing personality" — are you a deep diver or a surface skimmer? Which topics cluster together?',
             why="Makes you aware of your information diet. Reveals patterns you never noticed.",
             stack=["Browser Extension API for history capture", "D3.js or Cytoscape.js for graph visualization", "Python (scikit-learn) for clustering analysis", "SQLite for local data storage"],
             features=[],
             difficulty="⭐⭐⭐ (Intermediate)"),
        dict(idx="3.4", emoji="🌙", title='"Nightmode Archaeologist" — Dark Mode for Every Site',
             tagline="An AI dark-mode engine that actually understands design instead of breaking it.",
             what="Not just a dark mode toggle — an AI-powered tool that *intelligently* inverts colors, adjusts contrast, and preserves brand identity when forcing dark mode on sites that don't support it. Learns from user corrections.",
             why="Existing dark mode extensions break sites. This one *understands* design and adapts.",
             stack=["Browser Extension API", "TensorFlow.js for on-device color analysis", "CSS injection engine", "User feedback loop for ML improvement"],
             features=[],
             difficulty="⭐⭐⭐⭐ (Intermediate-Advanced)"),
        dict(idx="3.5", emoji="🕰️", title='"Tab Sediment" — Tab Hoarder Rehab',
             tagline="Visualizes your open browser tabs as geological sediment layers by age and topic.",
             what="An extension that renders your (embarrassing) 80-tab window as layered sediment strata — older, forgotten tabs sink to the bottom and visually erode. Clicking a layer surfaces a one-line AI summary of why you probably opened it, and a one-click 'archive or close' action.",
             why="Turns tab guilt into a satisfying visual cleanup ritual instead of a productivity lecture.",
             stack=["Browser Extension API (tabs + history permissions)", "Canvas/SVG for the strata rendering", "Small on-device LLM or API summarizer", "IndexedDB for local session tracking"],
             features=["Auto-clusters tabs by topic", "One-click bulk archive to a reading list", "Weekly 'sediment report' of your browsing habits"],
             difficulty="⭐⭐ (Beginner-Intermediate)"),
    ],
)

# -------------------------------------------------------------------------
# 4. System & Infrastructure
# -------------------------------------------------------------------------
cat(
    "04-system-infrastructure",
    "System & Infrastructure",
    "🖥️",
    "Deep, low-level tooling for peering inside containers, networks, and APIs.",
    [
        dict(idx="4.1", emoji="🐳", title='"Container Detective" — Visual Docker Forensics',
             tagline="Reverse-engineer any Docker image into a visual forensics report.",
             what="Upload a Docker image or inspect a running container. The tool reverse-engineers what's actually inside (hidden layers, unexpected binaries), a security timeline (when vulnerabilities were introduced), a \"family tree\" (which base images led to this one), and bloat analysis (\"You have 3 versions of Python installed\").",
             why="Docker images are black boxes. This shines a light inside in a visual, actionable way.",
             stack=["Go/Rust for image layer parsing", "D3.js for layer visualization", "Trivy/Grype for vulnerability scanning", "React for the forensics dashboard"],
             features=[],
             difficulty="⭐⭐⭐⭐⭐ (Advanced)"),
        dict(idx="4.2", emoji="⚡", title='"Latency Detective" — Network Performance Sleuth',
             tagline="Traces the exact root cause of latency through your entire stack.",
             what='A tool that traces *exactly* where latency happens in your stack — not just "database is slow" but "this specific query, at this specific time, because of this missing index, caused by this N+1 loop in your code."',
             why="Most profilers show symptoms. This traces the *root cause* through the entire stack.",
             stack=["eBPF for kernel-level tracing", "Go for the agent", "React + D3.js for flame graphs & timelines", "ClickHouse for high-volume trace storage"],
             features=[],
             difficulty="⭐⭐⭐⭐⭐ (Advanced)"),
        dict(idx="4.3", emoji="📡", title='"API Archaeology" — Discover Hidden Endpoints',
             tagline="Automates the hunt for undocumented API endpoints and dead routes.",
             what="A reconnaissance tool that analyzes a web app's frontend JavaScript, network traffic, and documentation to discover *undocumented* API endpoints, hidden features, and deprecated routes still active.",
             why='Security researchers and curious developers love finding "secret" APIs. This automates the hunt.',
             stack=["Python (BeautifulSoup + JS parsing)", "Burp Suite/OWASP ZAP integration", "React for the discovery dashboard", "GraphQL for endpoint relationship mapping"],
             features=[],
             difficulty="⭐⭐⭐⭐ (Intermediate-Advanced)"),
        dict(idx="4.4", emoji="🧯", title='"Blast Radius" — Change Impact Simulator',
             tagline="Simulates the downstream impact of a config or infra change before you make it.",
             what="Point it at your infrastructure-as-code repo. Before you merge a change (a Terraform diff, a k8s manifest edit), it simulates and visualizes the 'blast radius' — which services, regions, and on-call teams would be affected if the change goes wrong.",
             why="Most CI just checks syntax. This answers the scarier question: 'what actually breaks if this is wrong?'",
             stack=["Go for the dependency-graph engine", "Terraform/Kubernetes API parsers", "React + D3.js for the blast-radius graph", "GitHub Actions integration for PR comments"],
             features=["PR-bot that comments a risk score", "Historical incident correlation", "'Dry run' visualization mode"],
             difficulty="⭐⭐⭐⭐⭐ (Advanced)"),
    ],
)

# -------------------------------------------------------------------------
# 5. Creative & Generative
# -------------------------------------------------------------------------
cat(
    "05-creative-generative",
    "Creative & Generative",
    "🎨",
    "Where code becomes music, ink, maps, and mood.",
    [
        dict(idx="5.1", emoji="🎵", title='"Code Symphony" — Turn Code into Music',
             tagline="Sonifies your codebase — functions become melodies, bugs become dissonance.",
             what="A tool that sonifies your codebase. Functions become melodies. Loops become rhythms. Complexity becomes tempo. Bugs become dissonant notes. Listen to your code's \"symphony.\"",
             why="Synesthetic experience. Helps developers \"hear\" code quality and complexity.",
             stack=["Python (AST parsing for code analysis)", "Tone.js or Web Audio API for synthesis", "React for the visual + audio player", "MIDI export for DAW integration"],
             features=['Different "instruments" for different languages', '"Complexity crescendo" — tempo increases with cyclomatic complexity', '"Bug dissonance" — sour notes where bugs exist', "Export as actual music files"],
             difficulty="⭐⭐⭐⭐ (Intermediate-Advanced)"),
        dict(idx="5.2", emoji="🖼️", title='"Generative Tattoos" — AI Tattoo Designer',
             tagline="Generate and age-simulate tattoo designs before committing.",
             what="Describe a tattoo idea, and the AI generates unique designs that adapt to body placement (arm vs. back vs. wrist), consider skin tone for visibility, generate in multiple styles (geometric, watercolor, traditional, minimalist), and show an aging simulation (how it looks in 10, 20, 40 years).",
             why="Tattoo regret is real. This helps people visualize *before* committing. Artists can use it as a starting point.",
             stack=["Python (Stable Diffusion/ControlNet for generation)", "React for the design studio", "Three.js for 3D body placement preview", "Firebase for user galleries"],
             features=[],
             difficulty="⭐⭐⭐⭐ (Intermediate-Advanced)"),
        dict(idx="5.3", emoji="🏛️", title='"Lost Civilization Generator" — Procedural Worlds',
             tagline="Generates coherent fictional civilizations, language and all.",
             what="A tool that generates entire *lost civilizations* — their language (with actual grammar), their maps, their architecture styles, their mythology, their downfall story. Export as a worldbuilding bible for writers or game devs.",
             why="Not just random names — coherent, internally consistent fictional worlds with history, linguistics, and culture.",
             stack=["Python (procedural generation + LLM)", "Mapbox/Leaflet for generated maps", "React for the world explorer UI", "SQLite for world persistence"],
             features=[],
             difficulty="⭐⭐⭐⭐⭐ (Advanced)"),
        dict(idx="5.4", emoji="🎬", title='"Movie Mood Matcher" — Emotional Film Recommender',
             tagline="Search movies by desired emotional arc, not genre.",
             what='Instead of "I like action movies," search by *emotional journey*. "I want to feel: hopeful → anxious → triumphant" or "melancholic → peaceful." The AI maps movie emotional arcs and matches them to your desired mood trajectory.',
             why="Most recommenders use genres/actors. This uses *emotional experience* — how you want to *feel*.",
             stack=["Python (sentiment analysis on movie scripts/subtitles)", "PostgreSQL with pgvector for emotional vector search", "React for the mood selector UI", "TMDB API for metadata"],
             features=[],
             difficulty="⭐⭐⭐⭐ (Intermediate-Advanced)"),
        dict(idx="5.5", emoji="🪞", title='"Dream Loom" — Collaborative Story Weaving',
             tagline="Multiple people co-write a surreal story, one sentence at a time, illustrated live.",
             what="A real-time collaborative fiction tool: each participant adds one sentence in turn (like exquisite corpse), and after every few sentences an AI generates a matching illustration panel, building a live illustrated storybook nobody could predict.",
             why="Turns collaborative writing into a shared visual experience instead of a plain text thread.",
             stack=["WebSocket for real-time turns", "Stable Diffusion / image API for panels", "React for the storybook canvas", "Redis for session state"],
             features=["Turn timer to keep momentum", "Exportable illustrated PDF at the end", "Public gallery of finished stories"],
             difficulty="⭐⭐⭐ (Intermediate)"),
    ],
)

# -------------------------------------------------------------------------
# 6. Data & Visualization
# -------------------------------------------------------------------------
cat(
    "06-data-visualization",
    "Data & Visualization",
    "📊",
    "Turning your own digital exhaust — chat logs, browsing, footprint — into insight.",
    [
        dict(idx="6.1", emoji="🌍", title='"Digital Carbon Footprint" — Visualize Your Online Impact',
             tagline="Tracks and visualizes the real-world carbon cost of your internet habits.",
             what='A browser extension + dashboard that tracks the *carbon cost* of your internet usage — streaming, cloud storage, emails, video calls. Shows which habits are "heavy" and suggests greener alternatives.',
             why="Climate awareness meets personal analytics. Makes abstract environmental impact tangible.",
             stack=["Browser Extension API for traffic monitoring", "Python (carbon calculation models)", "React + D3.js for impact visualizations", "SQLite for local data (privacy-first)"],
             features=[],
             difficulty="⭐⭐⭐ (Intermediate)"),
        dict(idx="6.2", emoji="📊", title='"Life in Weeks" — Personal Timeline Visualizer',
             tagline="Your entire life rendered as a grid of weeks.",
             what='A beautiful, interactive visualization of your life as a grid of weeks (inspired by the "4,000 weeks" concept). Mark significant events, see patterns in your life phases, project future milestones. Morbid but motivating.',
             why="Existential productivity tool. Makes time feel scarce and precious.",
             stack=["React + D3.js for the week grid", "Supabase for data sync", "Export as poster-quality PDF", "Optional: import from calendar/photos for auto-population"],
             features=[],
             difficulty="⭐⭐ (Beginner-Intermediate)"),
        dict(idx="6.3", emoji="🗣️", title='"Conversation Archaeology" — Chat History Insights',
             tagline="Turns your chat exports into honest communication analytics.",
             what="Upload your WhatsApp/Telegram/Discord exports. Get insights like: who dominates conversations, what topics come up most, sentiment trends over time, a \"conversation health score\" (balanced vs. one-sided), and word clouds that actually mean something.",
             why="Makes you aware of your communication patterns. Fun + slightly uncomfortable (in a good way).",
             stack=["Python (NLTK/spaCy for NLP)", "React + Recharts for visualizations", "Pandas for data processing", "Local-first (no data leaves your machine)"],
             features=[],
             difficulty="⭐⭐⭐ (Intermediate)"),
        dict(idx="6.4", emoji="💸", title='"Money Weather" — Spending as a Forecast',
             tagline="Renders your bank transaction history as a weather forecast for your wallet.",
             what="Imports transaction history (via Plaid or CSV) and renders spending patterns as a literal weather map — storms for big expense spikes, sunny days for savings streaks, seasonal forecasts for predicted upcoming bills. Get a daily 'financial weather report.'",
             why="Numbers-heavy budget apps get ignored. A weather metaphor makes financial patterns intuitive and gives it a check-in-worthy daily hook.",
             stack=["Plaid API / CSV import", "Python for forecasting (Prophet or similar)", "React + Canvas for the animated weather visuals", "PostgreSQL for transaction storage"],
             features=["Daily forecast notification", "'Storm warnings' for predicted overdrafts", "Monthly climate report (spending trends)"],
             difficulty="⭐⭐⭐ (Intermediate)"),
    ],
)

# -------------------------------------------------------------------------
# 7. Security & Privacy
# -------------------------------------------------------------------------
cat(
    "07-security-privacy",
    "Security & Privacy",
    "🔐",
    "Tools that make security legible, proactive, and — occasionally — a little unsettling.",
    [
        dict(idx="7.1", emoji="🔐", title='"Password Autopsy" — Breach Pattern Analyzer',
             tagline="Shows you how you think about passwords, not just that you're pwned.",
             what='A tool that analyzes *your* leaked passwords from Have I Been Pwned (safely, via k-anonymity) and shows you your password "evolution" over time, patterns you reuse ("Oh, I always add !123"), a "password family tree" showing how you modified breached passwords, and a personalized security report card.',
             why='Most breach tools just say "you\'re pwned." This shows *how* you think about passwords and helps you break bad habits.',
             stack=["Python (HIBP API integration)", "React for the autopsy report", "Local processing (passwords never sent to server)", "PDF export for security audits"],
             features=[],
             difficulty="⭐⭐⭐ (Intermediate)"),
        dict(idx="7.2", emoji="🕵️", title='"Digital Shadow" — See What the Internet Knows About You',
             tagline="Builds the dossier a stranger could assemble about you from public data alone.",
             what='An automated tool that scrapes publicly available data about *you* from across the web (social media, public records, data brokers) and creates a "dossier" — showing how complete a profile someone could build without hacking you.',
             why='Privacy awareness through shock value. "I didn\'t realize I shared that much."',
             stack=["Python (scrapy + selenium for data collection)", "React for the dossier UI", "NLP for entity extraction & correlation", "Strict ethical guidelines + user consent only"],
             features=[],
             difficulty="⭐⭐⭐⭐ (Intermediate-Advanced)"),
        dict(idx="7.3", emoji="🛡️", title='"Canary Tokens as a Service" — Honeypot for Developers',
             tagline="Open-source fake-credential honeypots that alert you the moment they're touched.",
             what='A service that generates "canary tokens" (fake API keys, database URLs, config files) that you sprinkle in your codebase. If someone uses them, you get an instant alert with their IP, user agent, and stack trace. Open-source alternative to commercial services.',
             why="Proactive security. Catches attackers *before* they do real damage.",
             stack=["Go/Rust for high-performance token validation", "React for token management dashboard", "Webhook/Slack/Discord integrations", "SQLite/PostgreSQL for alert storage"],
             features=[],
             difficulty="⭐⭐⭐ (Intermediate)"),
        dict(idx="7.4", emoji="📜", title='"Permission Archaeologist" — App Permission Time Capsule',
             tagline="Tracks how an app's requested permissions crept up over its version history.",
             what="Analyzes successive versions of a mobile/browser app (via app-store changelogs and manifest diffs) and plots a timeline of exactly which permissions were added, removed, or quietly expanded release over release — flagging suspicious scope creep.",
             why="Permission creep usually happens one 'harmless' update at a time. Nobody re-reads permissions after the first install — this makes the drift visible.",
             stack=["Python for manifest/APK diffing", "Public app-store APIs and archives", "React + D3.js timeline UI", "Cron job for continuous monitoring of watched apps"],
             features=["Watchlist with change alerts", "Side-by-side permission diffs per version", "Community-flagged suspicious changes"],
             difficulty="⭐⭐⭐⭐ (Intermediate-Advanced)"),
    ],
)

# -------------------------------------------------------------------------
# 8. Game & Simulation
# -------------------------------------------------------------------------
cat(
    "08-game-simulation",
    "Game & Simulation",
    "🎮",
    "Emergent systems you observe more than you win.",
    [
        dict(idx="8.1", emoji="🧬", title='"Evolution Simulator" — Watch Life Evolve',
             tagline="Watch simple AI creatures evolve through natural selection in your browser.",
             what='A browser-based simulation where simple "creatures" (basic AI agents) evolve over generations through natural selection. Users can tweak environment variables (food scarcity, predators, climate) and watch evolution unfold in real-time.',
             why="Interactive biology lesson. Emergent behavior is fascinating to watch.",
             stack=["JavaScript/TypeScript (Canvas API or WebGL)", "Genetic algorithm implementation", "React for control panel", "Export as time-lapse video"],
             features=[],
             difficulty="⭐⭐⭐⭐ (Intermediate-Advanced)"),
        dict(idx="8.2", emoji="🏙️", title='"City in a Bottle" — Procedural City Life',
             tagline="A tiny simulated city that writes its own emergent soap opera.",
             what='A tiny, self-sufficient city simulation where every citizen has needs, jobs, relationships, and a daily routine. Watch emergent stories: "The baker fell in love with the blacksmith\'s daughter, causing a bread shortage."',
             why='Not a game you "win" — a story generator you observe. Like *Dwarf Fortress* meets *The Sims* in a browser.',
             stack=["JavaScript/TypeScript (Canvas or PixiJS)", "Entity-Component-System architecture", "React for the \"observer\" UI (follow specific citizens)", "SQLite for world state persistence"],
             features=[],
             difficulty="⭐⭐⭐⭐⭐ (Advanced)"),
        dict(idx="8.3", emoji="🎲", title='"Rogue Documentation" — Documentation as Dungeon Crawl',
             tagline="Turns your project docs into an explorable roguelike dungeon.",
             what="Turn your project's documentation into a *roguelike dungeon*. Each page is a room. Links between pages are corridors. Reading = exploring. Finding bugs = fighting monsters. Completing sections = leveling up.",
             why="Makes reading docs fun. Gamifies the most skipped part of development.",
             stack=["React (game UI)", "MDX parser for dungeon generation", "Canvas API for the dungeon map", "Local storage for progress"],
             features=[],
             difficulty="⭐⭐⭐ (Intermediate)"),
        dict(idx="8.4", emoji="🪐", title='"Orbital Debris" — Realistic Satellite Traffic Sim',
             tagline="A physics-accurate sandbox for simulating satellite launches and collision cascades.",
             what="A real-orbital-mechanics sandbox where players launch and manage satellite constellations, and can trigger (or try to prevent) a Kessler-syndrome debris cascade — using real orbital data as a starting map.",
             why="Most space games use fake physics. This uses real orbital mechanics to teach a genuinely underrated modern problem: space debris.",
             stack=["JavaScript + a physics engine (or WASM-compiled orbital solver)", "Three.js for the 3D orbit visualization", "Public satellite catalog data (e.g., CelesTrak) as a starting scenario", "React for the mission-control UI"],
             features=["Real TLE-based starting satellite field", "Collision cascade simulation mode", "Sandbox vs. scenario ('prevent the cascade') modes"],
             difficulty="⭐⭐⭐⭐⭐ (Advanced)"),
    ],
)

# -------------------------------------------------------------------------
# 9. IoT & Hardware-Adjacent
# -------------------------------------------------------------------------
cat(
    "09-iot-hardware-adjacent",
    "IoT & Hardware-Adjacent",
    "📟",
    "Projects that bridge physical sensors, cameras, and gestures with software.",
    [
        dict(idx="9.1", emoji="🌱", title='"Smart Terrarium" — Self-Managing Ecosystem',
             tagline="A Raspberry Pi terrarium that waters itself and keeps a plant diary.",
             what="A Raspberry Pi-powered terrarium that monitors soil moisture, light, temperature, humidity; waters plants automatically based on species needs; takes daily timelapse photos; sends \"plant diary\" updates to your phone; and predicts growth stages and warns of problems.",
             why="Brings nature + tech together. A living project that grows with you.",
             stack=["Raspberry Pi + Python (sensors, camera)", "MQTT for IoT messaging", "React Native for mobile app", "TensorFlow Lite for plant health CV", "InfluxDB for time-series sensor data"],
             features=[],
             difficulty="⭐⭐⭐⭐ (Intermediate-Advanced)"),
        dict(idx="9.2", emoji="🎹", title='"Gesture MIDI Controller" — Air Piano',
             tagline="Play MIDI instruments in mid-air using nothing but a webcam.",
             what="Use your webcam + hand tracking to play MIDI instruments in the air. Different hand positions = different notes. Hand distance = velocity. Finger spread = effects.",
             why="No hardware needed. Turns any computer into an expressive instrument.",
             stack=["Python (MediaPipe for hand tracking)", "Web MIDI API for browser version", "Tone.js for browser synthesis", "Electron for desktop DAW integration"],
             features=[],
             difficulty="⭐⭐⭐⭐ (Intermediate-Advanced)"),
        dict(idx="9.3", emoji="📻", title='"Digital Shortwave" — Global Radio Explorer',
             tagline="Spin an interactive globe and tune into real radio stations from anywhere.",
             what='A web app that aggregates real internet radio stations from around the world on an interactive globe. Click a country, hear their local stations. Filter by genre, language, or "mystery stations" (numbers stations, unknown broadcasts).',
             why="Cultural exploration through audio. Feels like traveling without leaving your room.",
             stack=["React + Three.js (interactive globe)", "RadioBrowser API for station data", "Web Audio API for playback", "Node.js for station curation & metadata"],
             features=[],
             difficulty="⭐⭐⭐ (Intermediate)"),
        dict(idx="9.4", emoji="🔋", title='"Ghost Load Hunter" — Phantom Power Detective',
             tagline="A smart-plug-powered tool that hunts down which idle devices are silently draining power.",
             what="Pairs with cheap smart plugs to monitor standby ('vampire') power draw across your home, builds a per-device phantom-load profile over a week, and ranks devices by wasted watts with a payback-period calculator for a smart-plug or a real fix (e.g., a physical switch).",
             why="Phantom load is invisible and boring, but adds up — this makes it visible, rankable, and actionable instead of a vague 'unplug things' tip.",
             stack=["ESP32/smart-plug firmware or off-the-shelf smart plug APIs (Tapo, Kasa)", "InfluxDB for power time-series", "React dashboard with device ranking", "MQTT for local-first data collection"],
             features=["Auto-detects standby vs. active power signatures", "Cost-per-year estimate per device", "Alerts when a 'phantom' device turns unexpectedly active"],
             difficulty="⭐⭐⭐ (Intermediate)"),
    ],
)

# -------------------------------------------------------------------------
# 10. Social & Community
# -------------------------------------------------------------------------
cat(
    "10-social-community",
    "Social & Community",
    "🤝",
    "Slower, more structured, more thoughtful alternatives to mainstream social media.",
    [
        dict(idx="10.1", emoji="📝", title='"Anonymous Letters" — Digital Pen Pal Network',
             tagline="Anonymous, long-form pen-pal matching based on writing style, not demographics.",
             what="A platform where you write anonymous letters to strangers. No profiles, no photos, no real names — just thoughtful, long-form messages. AI matches you based on writing style and topic interests, not demographics.",
             why="Anti-social-media. Slow, thoughtful, anonymous connection in an age of instant, performative interaction.",
             stack=["Node.js + PostgreSQL", "React for the letter writing interface", "OpenAI API for matching algorithm", "End-to-end encryption for privacy"],
             features=[],
             difficulty="⭐⭐⭐ (Intermediate)"),
        dict(idx="10.2", emoji="🗳️", title='"Consensus Builder" — Structured Decision Making',
             tagline="Brings structured argument mapping and weighted voting to group decisions.",
             what='A tool for groups (friends, teams, communities) to make decisions without the chaos of group chat. Structured proposals, weighted voting, argument mapping, and "devil\'s advocate" AI that challenges weak reasoning.',
             why="Group decisions are usually messy. This brings structure and critical thinking to collective choice.",
             stack=["Node.js + PostgreSQL", "React + D3.js for argument maps", "Socket.io for real-time collaboration", "LLM integration for \"devil's advocate\" mode"],
             features=[],
             difficulty="⭐⭐⭐⭐ (Intermediate-Advanced)"),
        dict(idx="10.3", emoji="🌐", title='"Translation Party" — Cascade Translation Game',
             tagline="Bet on how mangled a phrase gets after 10 round-trip translations.",
             what='A party game where a phrase gets translated through 10+ languages and back, with players betting on how mangled it will become. "The spirit is willing but the flesh is weak" → "The alcohol is ready but the meat is fragile."',
             why="Hilarious, educational about language nuances, and a great social icebreaker.",
             stack=["Node.js + Google Translate/DeepL APIs", "React for the game UI", "WebSocket for multiplayer", "SQLite for \"greatest hits\" gallery"],
             features=[],
             difficulty="⭐⭐ (Beginner-Intermediate)"),
        dict(idx="10.4", emoji="🕯️", title='"Skill Campfire" — Hyperlocal Skill-Swap Circles',
             tagline="Matches neighbors into small recurring circles to trade skills, not services.",
             what="Groups nearby users into small (5-8 person) recurring 'circles' based on complementary skills people want to teach and learn (cooking, coding, gardening, repair), and schedules a rotating in-person or video meetup — no money changes hands, just swapped time and skill.",
             why="Most skill-share apps are one-off transactional listings. Recurring small circles build actual community, not just gig exchanges.",
             stack=["Node.js + PostgreSQL for matching", "React Native for scheduling & notifications", "Geolocation-based matching algorithm", "Calendar API integrations"],
             features=["Rotating 'who teaches what' schedule per circle", "Post-meetup skill-swap log", "Circle health score (attendance, reciprocity)"],
             difficulty="⭐⭐⭐ (Intermediate)"),
    ],
)

# -------------------------------------------------------------------------
# NEW CATEGORIES (extending the original "Categories We Want More Of")
# -------------------------------------------------------------------------

cat(
    "11-neurotech-bci",
    "Neurotech & BCI",
    "🧠⚡",
    "Consumer-grade brain-computer interface and neurofeedback projects — no lab required.",
    [
        dict(idx="11.1", emoji="🎧", title='"Focus Weather" — EEG-Driven Focus Dashboard',
             tagline="Turns a consumer EEG headband into a live weather report for your attention.",
             what="Using a consumer EEG device (Muse, NeuroSky), streams live attention/relaxation metrics and renders them as a 'weather forecast' for your focus state throughout the workday, correlated against calendar events to show what actually helps or hurts concentration.",
             why="Raw EEG graphs mean nothing to most people. A weather metaphor makes brain-state data instantly readable and actionable.",
             stack=["Python (BrainFlow/Muse SDK for EEG streaming)", "React dashboard with live charts", "Calendar API integration for correlation", "SQLite for session history"],
             features=["Daily focus forecast", "Correlates meetings/tasks with focus dips", "Exportable weekly focus report"],
             difficulty="⭐⭐⭐⭐ (Intermediate-Advanced)"),
        dict(idx="11.2", emoji="🕹️", title='"Blink Morse" — Accessible Input via Eye Blinks',
             tagline="A webcam-based blink-to-Morse-code text input system for limited-mobility users.",
             what="Uses a standard webcam and on-device face landmark detection to recognize deliberate eye blinks (short/long) and translates blink patterns into Morse code, then into typed text — a zero-hardware-cost communication aid.",
             why="Dedicated eye-tracking AAC devices are expensive. This turns any laptop webcam into an assistive input device.",
             stack=["MediaPipe Face Mesh for blink detection", "Python/JS Morse decoder", "Web Speech API for text-to-speech output", "Configurable timing calibration UI"],
             features=["Per-user blink calibration", "Adjustable Morse timing thresholds", "Text-to-speech output for typed messages"],
             difficulty="⭐⭐⭐ (Intermediate)"),
    ],
)

cat(
    "12-health-tech",
    "Health Tech",
    "🏥",
    "Health tools that go beyond step-counting — symptom pattern-finding, medication logistics, and honest data.",
    [
        dict(idx="12.1", emoji="💊", title='"Symptom Weather Map" — Pattern Finder for Chronic Conditions',
             tagline="Correlates daily symptom logs against weather, diet, and sleep to surface real triggers.",
             what="A daily logging app for chronic-condition symptoms (migraines, IBS, eczema, etc.) that automatically cross-references entries against pulled-in weather, sleep, and food-log data to surface statistically likely triggers — not just a diary, an actual pattern detector.",
             why="Most symptom trackers are passive diaries. This one actively hunts for correlations a person would never spot by eye.",
			 stack=["React Native for daily logging", "Python backend with pandas/statsmodels for correlation analysis", "Weather API + optional wearable integration", "PostgreSQL for longitudinal data"],
             features=["Weekly 'likely trigger' report", "Exportable PDF for doctor visits", "Confidence-scored correlations, not false certainty"],
             difficulty="⭐⭐⭐⭐ (Intermediate-Advanced)"),
        dict(idx="12.2", emoji="💊", title='"Pill Logistics" — Multi-Med Household Coordinator',
             tagline="Coordinates complex multi-person, multi-medication households without spreadsheet chaos.",
             what="For households managing multiple people's medications (elderly parents, kids, multiple conditions), tracks dosing schedules, refill timing tied to pharmacy stock, drug-interaction warnings, and caregiver hand-off notes in one shared view.",
             why="Existing pill reminder apps assume one person, one med. Real caregiving households are messier and this is built for that mess.",
             stack=["React Native shared family app", "Node.js + PostgreSQL", "OpenFDA API for interaction warnings", "Push notifications for multi-caregiver alerts"],
             features=["Shared caregiver view with hand-off notes", "Refill countdown tied to pharmacy pickup", "Drug interaction warnings across all household meds"],
             difficulty="⭐⭐⭐ (Intermediate)"),
    ],
)

cat(
    "13-space-astronomy",
    "Space & Astronomy",
    "🌌",
    "Tools for stargazers, orbital-mechanics nerds, and citizen astronomers.",
    [
        dict(idx="13.1", emoji="🔭", title='"Satellite Flyover Alarm" — Personalized Sky Event Notifier',
             tagline="Alerts you minutes before something worth seeing crosses your exact sky.",
             what="Pulls real-time satellite (ISS, Starlink trains), meteor shower, and aurora-forecast data, calculates visibility for your exact GPS location and local weather/cloud cover, and sends a push notification a few minutes before a genuinely visible event.",
             why="Generic 'ISS visible tonight' alerts ignore your cloud cover and horizon. This only pings you when it's actually worth stepping outside.",
             stack=["Python (Skyfield/PyEphem for orbital calculations)", "Weather + cloud-cover API for visibility filtering", "React Native push notifications", "N2YO/CelesTrak for satellite TLE data"],
             features=["Cloud-cover-aware visibility filtering", "Direction + elevation guide overlay", "Meteor shower and aurora alerts"],
             difficulty="⭐⭐⭐⭐ (Intermediate-Advanced)"),
        dict(idx="13.2", emoji="🪨", title='"Exoplanet Habitability Explorer" — Interactive Data Playground',
             tagline="Explore NASA's exoplanet archive through a habitability-scoring 3D map.",
             what="Pulls the full NASA Exoplanet Archive and lets users explore thousands of confirmed exoplanets on an interactive 3D star map, filtering and color-coding by a computed habitability score based on published research (distance from star, size, star type).",
             why="Raw exoplanet spreadsheets are inaccessible to non-scientists. This turns real published data into an explorable, educational visualization.",
             stack=["NASA Exoplanet Archive API", "Three.js for 3D star map", "Python for habitability score calculations", "React for filter/search UI"],
             features=["Habitability scoring based on published models", "Filter by star type, distance, discovery method", "Deep-dive panel per planet with source citations"],
             difficulty="⭐⭐⭐⭐ (Intermediate-Advanced)"),
    ],
)

cat(
    "14-edtech",
    "EdTech That Actually Teaches",
    "🎓",
    "Learning tools built around how people actually retain and struggle with material, not just content delivery.",
    [
        dict(idx="14.1", emoji="🧩", title='"Misconception Miner" — Learns From Your Wrong Answers',
             tagline="Builds a personal map of your specific misconceptions, not just a score.",
             what="Instead of just marking answers right/wrong, analyzes *why* a student's wrong answer is wrong (which specific misconception produced it) and builds a personalized concept map showing exactly which foundational ideas need reinforcement, with targeted micro-lessons for each gap.",
             why="Most quiz apps report a score. This reports a *diagnosis* — the actual conceptual gap causing repeated mistakes.",
             stack=["Python + LLM for wrong-answer classification", "Neo4j or graph DB for concept mapping", "React for the visual concept-gap map", "Adaptive quiz engine"],
             features=["Personal misconception map, not just a score", "Auto-generated micro-lessons per gap", "Progress tracked at the concept level, not the quiz level"],
             difficulty="⭐⭐⭐⭐ (Intermediate-Advanced)"),
        dict(idx="14.2", emoji="🗣️", title='"Explain It Back" — Feynman Technique Coach',
             tagline="Records you explaining a topic aloud and pinpoints exactly where your explanation breaks down.",
             what="You explain a concept out loud (voice or text) as if teaching a beginner. The AI transcribes, then pinpoints the exact sentence where your explanation gets vague, circular, or jargon-heavy — the Feynman Technique's 'gap' — and asks targeted follow-up questions to expose what you don't actually understand.",
             why="Most study tools test recall. This tests *understanding*, which is what the Feynman Technique is actually for.",
             stack=["Whisper for speech-to-text", "LLM for gap detection in explanations", "React for the recording + feedback UI", "Session history for tracking improvement over time"],
             features=["Highlights the exact sentence where clarity breaks down", "Generates targeted Socratic follow-up questions", "Tracks explanation quality improvement over time"],
             difficulty="⭐⭐⭐ (Intermediate)"),
    ],
)

cat(
    "15-accessibility",
    "Accessibility-Focused Projects",
    "♿",
    "Tools built to remove specific, concrete barriers — not accessibility as an afterthought.",
    [
        dict(idx="15.1", emoji="🖼️", title='"Alt-Text Auditor" — Site-Wide Accessibility Debt Tracker',
             tagline="Crawls a whole site and prioritizes accessibility fixes by real user impact, not just count.",
             what="Crawls an entire website and evaluates every image, form, and interactive element for accessibility issues (missing alt text, poor contrast, unlabeled inputs, keyboard-trap components), then ranks fixes by *estimated real-world impact* — e.g., a broken checkout-button label ranks above a decorative icon's missing alt text.",
             why="Most a11y scanners dump a flat list of hundreds of violations with no sense of priority. This tells teams what to fix first.",
             stack=["Playwright/Puppeteer for crawling + axe-core for scanning", "Node.js scoring engine for impact-weighting", "React dashboard with prioritized fix queue", "GitHub integration for auto-filing issues"],
             features=["Impact-weighted (not just count-based) priority queue", "Auto-generated alt-text suggestions via vision model", "CI integration to block regressions"],
             difficulty="⭐⭐⭐⭐ (Intermediate-Advanced)"),
        dict(idx="15.2", emoji="🔊", title='"Sound Shape" — Visual Captioning for Non-Speech Audio',
             tagline="Real-time captions for ambient sounds (sirens, doorbells, alarms), not just speech.",
             what="A wearable-or-desktop tool for Deaf and hard-of-hearing users that identifies and captions *non-speech* sounds in real time — a doorbell, smoke alarm, dog barking, someone calling your name from another room — with a directional indicator, not just a caption feed.",
             why="Most captioning tools handle speech only. Everyday safety and awareness sounds are just as important and are largely ignored.",
             stack=["On-device audio classification model (YAMNet or similar)", "Directional audio via multi-mic array", "React Native / smartwatch companion app", "On-device inference for privacy + low latency"],
             features=["Real-time non-speech sound classification", "Directional 'sound is coming from your left' indicator", "Custom sound training for household-specific alerts"],
             difficulty="⭐⭐⭐⭐⭐ (Advanced)"),
    ],
)

cat(
    "16-climate-sustainability",
    "Climate & Sustainability Tech",
    "🌍",
    "Practical tools for personal and community-level environmental impact, grounded in real data.",
    [
        dict(idx="16.1", emoji="🚰", title='"Water Ledger" — Household Water-Use Detective',
             tagline="Finds exactly which fixture or habit is driving your water bill, from smart-meter data.",
             what="Ingests smart water-meter data (or manual daily readings) and disaggregates total usage into likely per-fixture contributions (toilet, shower, irrigation, leaks) using usage-pattern signatures, flagging leaks and offering a payback-period calculator for fixture upgrades.",
             why="Water bills give one opaque number. This turns it into an actionable per-fixture breakdown, the same way energy disaggregation tools did for electricity.",
             stack=["Python for signature-based load disaggregation", "Smart meter API integrations (or manual CSV import)", "React dashboard with per-fixture breakdown", "Leak-detection alerting"],
             features=["Automatic leak detection alerts", "Per-fixture usage breakdown from pattern analysis", "Upgrade payback-period calculator"],
             difficulty="⭐⭐⭐⭐ (Intermediate-Advanced)"),
        dict(idx="16.2", emoji="🌳", title='"Neighborhood Canopy Tracker" — Community Tree Health Map",',
             tagline="Crowdsourced, photo-based mapping of urban tree health and canopy coverage.",
             what="A community mapping tool where residents photograph trees on their street; computer vision estimates species, canopy size, and visible health issues, building a live public map of neighborhood tree canopy coverage that can feed into city greening advocacy.",
             why="City tree inventories are usually years out of date. Crowdsourced photo mapping keeps it current and gives residents a direct stake in local canopy health.",
             stack=["React Native for photo capture + geotagging", "TensorFlow for species/health classification", "PostGIS for spatial data", "Public web map for city planners and residents"],
             features=["Crowdsourced photo-based canopy mapping", "Automatic species and health-issue detection", "Exportable reports for city advocacy"],
             difficulty="⭐⭐⭐⭐ (Intermediate-Advanced)"),
    ],
)

cat(
    "17-interactive-storytelling",
    "Interactive Storytelling Tools",
    "🎭",
    "Frameworks and experiences for stories that respond to the reader, not just scroll past them.",
    [
        dict(idx="17.1", emoji="📖", title='"Branch Loom" — Visual Editor for Branching Narratives',
             tagline="A node-based visual editor for writing branching, stateful interactive fiction.",
             what="A visual, node-based editor (like a flowchart) for writing branching interactive fiction, with built-in state tracking (inventory, relationship scores, flags) so writers can see and test every possible path without losing track of narrative logic — then export to a playable web format.",
             why="Most interactive fiction is written in tangled spreadsheets or Twine scripts that get unmanageable past a few dozen branches. Visual state tracking keeps complex branching sane.",
             stack=["React Flow for the node-based editor", "JSON-based story format with state variables", "Node.js export pipeline to playable HTML/JS", "Built-in path simulator/tester"],
             features=["Visual state variable tracking across branches", "Built-in 'simulate a playthrough' tester", "One-click export to a shareable web story"],
             difficulty="⭐⭐⭐⭐ (Intermediate-Advanced)"),
        dict(idx="17.2", emoji="🎙️", title='"Living Room Mystery" — Voice-Driven Party Whodunit',
             tagline="An AI game master runs a real-time voice mystery for a room full of players.",
             what="An AI 'game master' that runs a live murder-mystery for a group in the same room: each player gets secret info via their phone, and the AI listens to the group's spoken questions and accusations (via a shared speaker/mic) to reveal clues, adapt the story, and eventually confirm or deny a solved case.",
             why="Existing party mystery games use static printed scripts. A responsive AI game master lets the mystery adapt to how the group actually plays, every time.",
             stack=["Whisper for shared-room speech recognition", "LLM for game-master narrative logic and clue-gating", "React Native for per-player secret info", "WebSocket for syncing game state across phones"],
             features=["Unique case generated per playthrough", "AI adapts pacing/clues to group progress", "Per-player secret roles delivered privately to phones"],
             difficulty="⭐⭐⭐⭐⭐ (Advanced)"),
    ],
)

# =========================================================================
# WRITE FILES
# =========================================================================

os.makedirs(ROOT, exist_ok=True)

toc_lines = []
cat_summary_rows = []

for c in categories:
    folder_path = os.path.join(ROOT, c["folder"])
    os.makedirs(folder_path, exist_ok=True)

    proj_rows = []
    for p in c["projects"]:
        fname = f'{slugify(p["title"].split("—")[0])}.md'
        fpath = os.path.join(folder_path, fname)
        content = project_md(p["idx"], p["emoji"], p["title"], p["tagline"], p["what"], p["why"], p["stack"], p["features"], p["difficulty"])
        with open(fpath, "w", encoding="utf-8") as f:
            f.write(content)
        proj_rows.append((p["idx"], p["emoji"], p["title"], p["tagline"], p["difficulty"], fname))

    # category README
    cat_readme = f'''# {c["emoji"]} {c["name"]}

[⬅ Back to main index](../README.md)

{c["intro"]}

## Projects in this Category

| # | Project | One-liner | Difficulty |
|---|---------|-----------|------------|
'''
    for idx, emoji, title, tagline, difficulty, fname in proj_rows:
        clean_title = title.split("—")[0].strip().strip('"')
        cat_readme += f'| {idx} | {emoji} [{clean_title}](./{fname}) | {tagline} | {difficulty} |\n'

    cat_readme += f'\n---\n\n**{len(proj_rows)} project idea(s)** in this category. Have another idea? See the [Contribution Guide](../README.md#-contribution-guide) in the main index.\n'

    with open(os.path.join(folder_path, "README.md"), "w", encoding="utf-8") as f:
        f.write(cat_readme)

    toc_lines.append(f'{len(toc_lines)+1}. [{c["emoji"]} {c["name"]}](./{c["folder"]}/README.md)')
    cat_summary_rows.append((c["emoji"], c["name"], c["folder"], len(proj_rows)))

# ---- Root README ----
root_readme = """# 🚀 Project Genesis — Unique Developer Project Ideas

> A curated collection of genuinely unique, non-generic project ideas across multiple domains.
> **Goal:** Build things that make people say *"Wait, that's actually cool."*

Each domain lives in its own folder. Each project idea is its own Markdown file inside that folder, so you can link, star, or discuss a single idea directly.

---

## 📋 Table of Contents

"""
root_readme += "\n".join(toc_lines)
root_readme += "\n\n---\n\n## 🗂️ Categories at a Glance\n\n| Category | Folder | Ideas |\n|---|---|---|\n"
for emoji, name, folder, n in cat_summary_rows:
    root_readme += f"| {emoji} {name} | [`{folder}/`](./{folder}) | {n} |\n"

total_ideas = sum(n for _, _, _, n in cat_summary_rows)
root_readme += f"\n**Total: {total_ideas} project ideas across {len(cat_summary_rows)} categories.**\n"

root_readme += """
---

## 📁 Repo Structure

```
project-genesis/
├── README.md                          <- you are here
├── 01-ai-machine-learning/
│   ├── README.md                      <- category index
│   ├── ghostwriter-your-writing-style-cloner.md
│   └── ...
├── 02-developer-tools-productivity/
│   └── ...
├── ...
└── 17-interactive-storytelling/
    └── ...
```

Every project file follows the same template (see below), so ideas are easy to scan, diff, and extend.

---

## 🧭 How to Use This Repo

1. Browse a category folder that interests you.
2. Open a project's `.md` file for the full brief: what to build, why it's unique, suggested stack, key features, and a difficulty rating.
3. Fork/clone, build it, and add yourself to the [Hall of Fame](#-hall-of-fame--implemented-ideas) below via PR.

---

## ✍️ Contribution Guide

### How to Add a New Project Idea

1. **Check for uniqueness:** search existing folders to avoid duplicates.
2. **Pick (or create) a category folder.** If your idea doesn't fit an existing one, propose a new top-level folder.
3. **Follow the template** below and save it as its own `.md` file inside the category folder, using a short kebab-case filename.
4. **Add a row** to that category's `README.md` table.
5. **Be specific:** "Build X that does Y using Z" — not vague concepts.
6. **Include the "Why":** what makes this idea special? Why build it?
7. **Tag difficulty:** ⭐ to ⭐⭐⭐⭐⭐.
8. **Submit a PR.**

### Project Idea Template

```markdown
# [emoji] [idx] — "[Project Name]" — [One-line description]

> [tagline]

**Difficulty:** [⭐ rating]

## What to Build
[Detailed description of the project]

## Why It's Unique
[What makes this different from generic ideas]

## Suggested Stack
- [Technology 1]
- [Technology 2]
- [Technology 3]

## Key Features
- [Feature 1]
- [Feature 2]
- [Feature 3]
```

### Categories We'd Still Love More Of

- 🧠⚡ More **Neurotech & BCI** ideas
- 🏥 More **Health Tech** (beyond fitness trackers)
- 🌌 More **Space & Astronomy** tools
- 🎓 More **EdTech** that actually teaches
- ♿ More **Accessibility**-focused projects
- 🌍 More **Climate & Sustainability** tech
- 🎭 More **Interactive Storytelling** tools
- ...or an entirely new category. Propose one!

---

## 🏆 Hall of Fame — Implemented Ideas

> Projects from this list that were actually built. Add yours!

| Project | Builder | Link | Status |
|---------|---------|------|--------|
| *Your project here* | *Your name* | *GitHub link* | *In progress / Complete* |

---

## 📜 License

This repository of ideas is released under [CC0 1.0 Universal](https://creativecommons.org/publicdomain/zero/1.0/).
**Steal these ideas. Build them. Make them yours.** No attribution required.

> *"The best time to start a side project was yesterday. The second best time is now."*

---

**⭐ Star this repo if it sparked an idea. Fork it if you want to build one.**
"""

with open(os.path.join(ROOT, "README.md"), "w", encoding="utf-8") as f:
    f.write(root_readme)

print(f"Done. {total_ideas} projects across {len(categories)} categories written to {ROOT}")
