# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

```bash
npm run dev          # Start dev server with Turbopack (localhost:3000)
npm run build        # Production build
npm run lint         # Run ESLint
npm run test         # Run Vitest tests
npm run test -- --run path/to/file.test.ts  # Run single test file
npm run setup        # Install deps, generate Prisma client, run migrations
npm run db:reset     # Reset database (destructive)
```

## Architecture

UIGen is an AI-powered React component generator that uses a virtual file system for live preview. Users describe components in a chat interface, and Claude generates React/Tailwind code that renders in real-time.

### Core Flow
1. **Chat API** (`src/app/api/chat/route.ts`): Streams responses from Claude using Vercel AI SDK. Uses two tools:
   - `str_replace_editor`: Create/view/edit files (view, create, str_replace, insert commands)
   - `file_manager`: Rename/delete files

2. **Virtual File System** (`src/lib/file-system.ts`): In-memory file system (VirtualFileSystem class). No files written to disk. Supports serialization for persistence.

3. **Preview System** (`src/lib/transform/jsx-transformer.ts`):
   - Transforms JSX/TSX using Babel standalone
   - Creates import maps with blob URLs for browser execution
   - Loads third-party packages from esm.sh
   - Injects Tailwind via CDN

4. **State Management**: React contexts wrap the app:
   - `FileSystemProvider`: Virtual FS state, handles tool calls from AI
   - `ChatProvider`: Chat messages, AI streaming state

### Key Conventions
- Entry point for generated apps is always `/App.jsx`
- Local imports use `@/` alias (maps to virtual FS root)
- All styling via Tailwind classes, not inline styles
- Generated components run in sandboxed iframe

### Database
SQLite via Prisma (`prisma/schema.prisma`). Prisma client outputs to `src/generated/prisma`.

```
User: id, email(unique), password, createdAt, updatedAt, projects[]
Project: id, name, userId?(->User, cascade delete), messages(JSON string, default "[]"), data(JSON string, default "{}"), createdAt, updatedAt
```

### Provider
Uses Anthropic API when `ANTHROPIC_API_KEY` is set; falls back to mock responses for development without API key.

## Code Style

- Use comments sparingly. Only comment complex code.
- Add one emoji related to the text in all comments.

## Testing

- Always update the corresponding `.test.md` report when unit tests are executed.
