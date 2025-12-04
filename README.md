# Reading Club Q&A Assistant

An AI-powered Q&A assistant for the Reading Club forum that uses Retrieval-Augmented Generation (RAG) to answer questions about book discussions, reading challenges, and community recommendations.

## Features

- **RAG-Powered Answers** - Semantic search retrieves relevant forum discussions and generates contextual AI responses
- **Citation Tracking** - Every answer includes source posts with topic titles, excerpts, authors, and relevance scores
- **Answer Modes** - Toggle between concise and detailed answer styles
- **Dark/Light Theme** - Full theme support with persistent user preference
- **Real-Time Search** - Instant semantic matching across indexed forum posts
- **Responsive Design** - Works seamlessly on desktop and mobile devices
- **Demo Data** - 11 sample Reading Club posts included for testing without external APIs

## Getting Started

### Prerequisites

- Node.js 18+ 
- npm or yarn

### Installation

1. Clone the repository
2. Install dependencies:
   ```bash
   npm install
   ```

3. Add your environment secrets (optional - app works with demo data):
   - `AIPIPE_API_KEY` - Your AIPipe API key for OpenRouter
   - `AIPIPE_BASE_URL` - AIPipe endpoint URL (e.g., `https://aipipe.org/openrouter/v1`)
   - `DISCOURSE_FORUM_URL` - Your Discourse forum URL

### Running the Application

```bash
npm run dev
```

The application will start on `http://localhost:5000` with:
- Express backend on port 5000
- Vite dev server with hot module reloading

## Usage

1. **Ask a Question** - Type any question about books, reading challenges, or Reading Club discussions
2. **Choose Answer Style** - Select "Short" for concise answers or "Detailed" for comprehensive responses
3. **Review Citations** - Click on source posts to see the full Discourse discussion
4. **Toggle Theme** - Use the theme button in the header to switch between light and dark modes

### Example Questions

- "What are the best science fiction books?"
- "How can I read more books this year?"
- "What fantasy books do you recommend for beginners?"
- "Tell me about non-fiction book recommendations"

## Project Structure

```
.
├── client/                      # React frontend
│   ├── src/
│   │   ├── pages/
│   │   │   ├── chat.tsx        # Main chat interface
│   │   │   └── not-found.tsx
│   │   ├── components/
│   │   │   ├── header.tsx      # App header with theme toggle
│   │   │   ├── chat-message.tsx # Message bubbles
│   │   │   ├── citation-card.tsx # Source post cards
│   │   │   ├── input-area.tsx   # Question input & mode toggle
│   │   │   ├── empty-state.tsx  # Initial state
│   │   │   └── theme-provider.tsx
│   │   ├── lib/
│   │   │   └── queryClient.ts  # React Query configuration
│   │   ├── hooks/
│   │   │   └── use-toast.ts
│   │   ├── App.tsx
│   │   └── index.css
│   └── package.json
├── server/                      # Express backend
│   ├── index.ts               # Server entry point
│   ├── routes.ts              # API route handlers
│   ├── rag.ts                 # RAG implementation
│   ├── storage.ts             # Data storage interface
│   └── vite.ts                # Vite dev server setup
├── shared/                      # Shared types & schemas
│   └── schema.ts              # TypeScript interfaces & Zod schemas
├── design_guidelines.md         # UI design specifications
└── README.md                    # This file
```

## Architecture

### Frontend

- **Framework**: React + TypeScript
- **Build Tool**: Vite
- **Routing**: Wouter
- **State Management**: TanStack Query (server state)
- **UI Components**: Shadcn/ui + Radix UI
- **Styling**: Tailwind CSS with custom design tokens
- **Icons**: Lucide React

### Backend

- **Framework**: Express.js
- **Language**: TypeScript
- **RAG Engine**: In-memory vector storage with cosine similarity
- **Validation**: Zod schemas
- **LLM Integration**: OpenRouter via AIPipe

### Data Models

**Post**
- Complete forum posts with topic, author, content, URL, and timestamp

**Chunk**
- Segmented post content with vector embeddings for semantic search

**Context**
- Retrieved excerpts with relevance scores for answer generation

## API Endpoints

### `GET /api/health`
System health check with indexing statistics.

**Response:**
```json
{
  "status": "healthy",
  "postsIndexed": 11,
  "chunksIndexed": 11,
  "lastUpdated": "2025-12-02T04:06:05.001Z"
}
```

### `POST /api/ask`
Main RAG endpoint for generating answers with citations.

**Request:**
```json
{
  "question": "What fantasy books do you recommend?",
  "detailedAnswer": false
}
```

**Response:**
```json
{
  "answer": "...",
  "contexts": [
    {
      "postId": "...",
      "topicTitle": "...",
      "excerpt": "...",
      "url": "...",
      "score": 0.85,
      "authorUsername": "...",
      "createdAt": "2024-01-20T00:00:00.000Z"
    }
  ]
}
```

### `GET /api/posts`
List all indexed forum posts.

## Development

### Building for Production

```bash
npm run build
```

### Code Organization

- **Components**: Shadcn/ui components in `client/src/components/ui`
- **Custom Components**: Application components in `client/src/components`
- **Pages**: Route pages in `client/src/pages`
- **Utilities**: Helper functions in `client/src/lib`
- **Hooks**: React hooks in `client/src/hooks`

### Styling

- Color scheme defined in `client/src/index.css`
- Tailwind CSS configuration in `tailwind.config.ts`
- Design guidelines in `design_guidelines.md`
- Supports dark mode via CSS classes

## Future Enhancements

- PostgreSQL database for persistent storage and query history
- `/reindex` endpoint for incremental data updates from Discourse
- `/search` endpoint for keyword-based search
- Admin dashboard for monitoring and management
- Advanced filtering by author, date range, and topic categories
- Multi-turn conversation history
- Production vector database (Pinecone/Qdrant Cloud)
- Batch processing for large-scale forum ingestion

## Environment Variables

```
# AI/LLM Integration (optional)
AIPIPE_API_KEY=your_api_key
AIPIPE_BASE_URL=https://aipipe.org/openrouter/v1
DISCOURSE_FORUM_URL=https://your-forum.example.com

# Session Management
SESSION_SECRET=your_session_secret
```

## Deployment

The application can be deployed to any Node.js hosting platform:

1. Set environment variables in your hosting platform
2. Run `npm run build` to create production build
3. Start with `npm start` or configure your platform's startup command

## License

MIT

## Support

For issues or feature requests, please contact the development team or visit the Reading Club forum.
