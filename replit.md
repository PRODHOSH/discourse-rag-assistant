# Reading Club Q&A Assistant

## Overview

This is an AI-powered Q&A assistant application for the Reading Club forum. The application uses Retrieval-Augmented Generation (RAG) to answer questions about forum discussions by searching through indexed posts and generating contextual answers with source citations. Users can ask questions about book discussions, reading challenges, and community recommendations, receiving AI-generated responses backed by actual forum content.

## Current Status

MVP complete with:
- Full RAG pipeline (embedding generation, semantic search, context retrieval)
- React chat interface with user questions and AI-generated responses
- Citation panels showing source posts with relevance scores and clickable links
- Short/Detailed answer mode toggle
- Light/Dark theme support
- Demo data (11 forum posts) for testing without external API keys

## User Preferences

Preferred communication style: Simple, everyday language.

## Environment Variables

Required for full LLM integration (optional - app works with fallback without these):
- `AIPIPE_API_KEY` - AIPipe API key for OpenRouter access
- `AIPIPE_BASE_URL` - AIPipe endpoint URL (e.g., https://api.aipipe.io/v1)

## System Architecture

### Frontend Architecture

**Framework & Tooling:**
- React with TypeScript for type-safe component development
- Vite as the build tool and development server
- Wouter for lightweight client-side routing
- TanStack Query for server state management and API communication

**UI Design System:**
- Shadcn/ui component library built on Radix UI primitives
- Tailwind CSS for styling with a custom design system inspired by Linear and Notion
- Design focuses on clarity, information density, and trust through transparency
- Custom theme system supporting light/dark modes with CSS variables
- Typography hierarchy: Inter for UI, JetBrains Mono for technical data

**Key UI Components:**
- `client/src/pages/chat.tsx` - Main chat interface page
- `client/src/components/header.tsx` - Application header with theme toggle
- `client/src/components/empty-state.tsx` - Initial state with example questions
- `client/src/components/chat-message.tsx` - User and assistant message bubbles
- `client/src/components/citation-card.tsx` - Source citation cards with metadata
- `client/src/components/input-area.tsx` - Question input with Short/Detailed toggle

### Backend Architecture

**Server Framework:**
- Express.js as the HTTP server
- ESBuild for server-side bundling and production builds

**RAG Implementation (server/rag.ts):**
- In-memory vector storage using cosine similarity for semantic search
- Query embedding generation using keyword-based approach
- Context retrieval system that returns top-K relevant forum posts
- Prompt engineering to combine user questions with retrieved context
- Fallback answer generation when LLM API is unavailable
- Support for both concise and detailed answer modes

**Data Storage (server/storage.ts):**
- In-memory MemStorage class with Posts and Chunks
- Demo data initialization with 11 sample Reading Club posts
- Vector embeddings stored alongside content for similarity search
- Cosine similarity search for semantic matching

**API Endpoints (server/routes.ts):**
- `GET /api/health` - System health check with indexing statistics
- `POST /api/ask` - Main RAG endpoint accepting questions and returning answers with citations
- `GET /api/posts` - List all indexed posts

**Data Models (shared/schema.ts):**
- `Post` - Complete forum posts with metadata (topic, author, content, URL, timestamp)
- `Chunk` - Segmented post content with embeddings for vector search
- `Context` - Retrieved excerpts with relevance scores for answer generation
- `AskRequest/AskResponse` - API request/response types
- `ChatMessage` - Frontend message state with loading/error handling

### External Dependencies

**Frontend Libraries:**
- `@tanstack/react-query` - Server state management and API caching
- `@radix-ui/*` - Headless UI components
- `tailwindcss` - Utility-first CSS framework
- `wouter` - Lightweight routing library
- `lucide-react` - Icon library
- `zod` - Schema validation

**Backend Libraries:**
- `express` - Web server framework
- `zod` - Request validation and type safety

**AI/LLM Integration:**
- Architecture prepared for OpenRouter/AIPipe integration
- Set AIPIPE_API_KEY and AIPIPE_BASE_URL environment variables to enable
- Uses openai/gpt-4o-mini model via OpenAI-compatible API

## Running the Application

The application runs via the "Start application" workflow which executes `npm run dev`. This starts:
- Express backend on port 5000
- Vite dev server for frontend hot reloading

## Future Enhancements (Agreed Upon)

- PostgreSQL database for post metadata storage and query history
- POST /reindex endpoint for incremental data updates from Discourse
- GET /search endpoint for keyword-based search
- Admin dashboard for monitoring
- Advanced filtering (by author, date range, topic categories)
- Conversation history and multi-turn dialogue
- Production vector database (Pinecone/Qdrant Cloud)
- Batch processing for large-scale forum data ingestion
