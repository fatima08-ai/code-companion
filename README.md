# Code Companion

Build "RAGScript", a polished, fully interactive web frontend for a privacy-first AI assistant that lets developers chat with their own codebase (Retrieval-Augmented Generation). Tagline: "Chat with your codebase. Your code stays yours."

TECH

- React + TypeScript + Tailwind + shadcn/ui + Framer Motion + lucide-react.

- Syntax highlighting for code blocks (react-syntax-highlighter or shiki) and markdown rendering for answers (react-markdown).

- Dark theme by default with a light/dark toggle. Developer-tool aesthetic: deep slate/near-black background, one electric accent (indigo to cyan gradient), monospace for code and file paths, clean sans-serif for UI text.

PAGES / LAYOUT

1. Landing/hero section: animated gradient background, headline, short pitch, three feature cards (Local-first indexing, Semantic retrieval, Fast Groq inference), and a "Launch App" button that scrolls or routes to the app.

2. App workspace with three areas:

   - Left sidebar: Project panel (index status, file count, chunk count), file tree of the indexed codebase (collapsible folders, click a file to preview it), and a chat history list.

   - Center: Chat interface.

   - Right panel (collapsible): "Retrieved Context" showing the code chunks used for the current answer, with file name, line range, and a relevance score bar.

3. Settings dialog: API base URL, Groq model name (default "llama-3.1-8b-instant"), number of chunks to retrieve (slider, 1-10), Demo Mode toggle (on by default).

CORE FEATURES (all must actually work)

- Codebase loading: drag-and-drop zone and file picker accepting multiple files or a folder (.py, .cpp, .h, .hpp, .c, .java, .js, .ts, .md, .txt and similar). Show upload progress per file, a file-size warning for files over 5 MB ("Large files may take longer than 15 seconds to index"), and reject unsupported/binary files with a friendly toast.

- Indexing simulation/flow: an animated multi-step progress indicator (Reading files, Chunking, Embedding, Stored in local ChromaDB) with live counters. Show index status as Not indexed / Indexing / Ready.

- Chat: send with Enter (Shift+Enter for newline), message bubbles for user and assistant, streaming/typewriter effect for assistant replies, markdown plus syntax-highlighted code blocks with a copy button, and auto-scroll with a "jump to latest" button.

- Source citations: each answer shows clickable chips like "buffer_manager.cpp:42-67". Clicking one opens the file in a preview drawer scrolled to and highlighting those lines, and highlights the matching chunk in the Retrieved Context panel.

- Suggested prompts shown on an empty chat: "Explain how memory is allocated in buffer_manager.cpp", "Which classes inherit from BaseHandler?", "Summarize what this module does."

- Chat management: new chat, rename, delete, clear history; persist everything (chats, settings, uploaded file contents) in localStorage.

- Regenerate response, stop generation, copy answer, thumbs up/down on each answer.

- Empty, loading, and error states everywhere (skeleton loaders, retry button, toast notifications, a friendly message if the backend is unreachable).

BACKEND INTEGRATION LAYER

- Put all network calls in one file, src/lib/api.ts, with three functions: indexCodebase(files), askQuestion(query, options) returning streamed text plus an array of sources {file, startLine, endLine, score, snippet}, and getIndexStatus().

- Expected REST endpoints on a configurable base URL (default http://localhost:8000): POST /index, POST /query, GET /status.

- Demo Mode: when on, or when the backend is unreachable, use a mock implementation that does simple keyword matching against the actually uploaded files, returns real line ranges and snippets from them, and streams a plausible explanation. The app must be fully demoable with no backend.

- NEVER put an API key in the frontend. The Groq key lives only in the backend .env.

MICROANIMATIONS AND LIVE FEEL

- Framer Motion page and element transitions: fade/slide-in for messages, spring-based hover and tap feedback on buttons and cards.

- Typing indicator (three pulsing dots) while the model is thinking, then a streaming cursor.

- Animated gradient border or glow on the chat input when focused; subtle pulse on the "Ready" status dot.

- Animated progress ring/bar during indexing; counters that count up smoothly.

- Staggered entrance of file-tree items and suggested prompts; smooth collapse/expand of panels and folders.

- Animated relevance score bars filling in when sources appear; ripple or checkmark animation on copy.

- Skeleton shimmer for loading states. Respect prefers-reduced-motion by disabling non-essential animation.

RESPONSIVE DESIGN (phone, tablet, desktop; mobile-first)

- Phone (<640px): single-column chat; sidebar becomes a slide-in drawer from a hamburger menu; Retrieved Context becomes a bottom sheet opened by a "Sources" button; input bar sticks to the bottom with safe-area padding; touch targets at least 44px.

- Tablet (640-1024px): collapsible sidebar, chat takes the main area, context panel as an overlay drawer.

- Desktop (>1024px): full three-pane layout with resizable panels.

- No horizontal scroll at any width; code blocks scroll inside their own container; test at 360px, 768px, 1280px, and 1920px.

ACCESSIBILITY AND POLISH

- Full keyboard navigation, visible focus rings, ARIA labels, sufficient contrast in both themes.

- Keyboard shortcuts: Ctrl/Cmd+K (new chat), Ctrl/Cmd+B (toggle sidebar), Esc (close drawers).

- Footer: "Built for Hacktoberfest Islamabad 2026 · Open AI Build Challenge", MIT License, team credits: Muhammad Musa (AI & Backend Logic) and Fatima Jawad (Frontend Integration & Open-Source Lead).

-Clean component structure, typed props, no placeholder "coming soon" buttons: every visible control must do something.

This project was built with [Lovable](https://lovable.dev).

## Build with Lovable

Continue developing this project in the [Lovable editor](https://lovable.dev/projects/c363f1b0-b567-4e21-a289-a428e064bca6).

- **Ship faster**: describe what you want to build and Lovable handles the code.
- **Stay in sync**: every change made in Lovable is committed straight to this repository.
- **Full ownership**: this code is yours. Push to `main` on GitHub and your changes sync back into Lovable, ready for your next prompt.

## Development

Prefer working locally? You need Node.js and npm — [install with nvm](https://github.com/nvm-sh/nvm#installing-and-updating).

```sh
git clone <this-repository-url>
cd <repository-name>
npm i
npm run dev
```
