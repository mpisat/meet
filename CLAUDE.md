# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Development Commands

```bash
# Install dependencies
pnpm install

# Run development server
pnpm dev

# Build for production
pnpm build

# Start production server
pnpm start

# Run linter
pnpm lint

# Format code
pnpm format:write
```

## Architecture Overview

This is a LiveKit video conferencing application built with Next.js 15 using the App Router pattern.

### Core Technologies
- **Framework**: Next.js 15.2.4 with React 18.3.1
- **WebRTC**: LiveKit Client SDK and @livekit/components-react
- **Language**: TypeScript with strict mode
- **Package Manager**: pnpm 9.15.9

### Key Architectural Patterns

1. **App Router Structure**: All pages use the Next.js App Router in the `/app` directory
2. **Server Components**: Pages are server components by default, with client components marked explicitly
3. **API Routes**: Connection management and recording controls via `/app/api/` routes
4. **Dynamic Routing**: Room names are handled via `/rooms/[roomName]/` dynamic route

### Important Implementation Details

- **E2EE Setup**: End-to-end encryption is handled via `useSetupE2EE` hook in `/lib/useSetupE2EE.ts`
- **Client Implementation**: Video conference logic is split between `PageClientImpl.tsx` (standard rooms) and `VideoConferenceClientImpl.tsx` (custom implementation)
- **Connection Flow**: Room tokens are fetched from `/api/connection-details/` endpoint
- **Recording**: Start/stop recording via `/api/record/start/` and `/api/record/stop/` endpoints

### Environment Requirements

The application requires LiveKit credentials to be configured:
- `LIVEKIT_API_KEY`
- `LIVEKIT_API_SECRET`
- `LIVEKIT_URL`

### TypeScript Configuration

- Target: ES2020
- Strict mode enabled
- Path alias: `@/*` maps to root directory
- JSX: preserve mode for Next.js compatibility

### Video Codec Configuration

- Default codec: H.264 (changed from VP9)
- Simulcast layers configured:
  - HQ mode: 1080p, 720p, 360p
  - Standard mode: 720p, 360p, 180p
- Codec can be overridden via URL parameter: `?codec=vp8|vp9|h264|av1`
- Note: When E2EE is enabled, AV1 and VP9 codecs are automatically disabled