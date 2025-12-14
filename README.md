# GitGrade - AI-Powered GitHub Repository Analyzer

A powerful web application that evaluates GitHub repositories and provides comprehensive analysis with scores, summaries, and personalized improvement roadmaps.

## Overview

GitGrade is an intelligent system built for the UnsaidTalks GitGrade Hackathon that transforms your GitHub repository into a meaningful developer profile. It analyzes code quality, project structure, documentation, testing practices, and provides actionable insights to help developers improve their projects.

## Features

### Core Functionality

- **Instant Repository Analysis**: Simply paste any public GitHub repository URL and get comprehensive insights in seconds
- **Smart Scoring System**: Receive a score from 0-100 with ratings (Beginner/Intermediate/Advanced)
- **Detailed Metrics Dashboard**: View breakdowns across 6 key dimensions:
  - Code Quality & Readability
  - Documentation & Clarity
  - Test Coverage & Maintainability
  - Project Structure & Organization
  - Development Activity & Consistency
  - Best Practices & Version Control

### Advanced Features

- **Personalized Improvement Roadmap**: Get prioritized, actionable steps to enhance your repository
- **Beautiful Visualizations**: View your metrics through intuitive progress bars and color-coded indicators
- **Shareable Results**: Generate unique URLs to share your analysis with others
- **Export Reports**: Download detailed text reports of your analysis
- **Real-time Progress Tracking**: Watch as your repository is analyzed with live status updates
- **Repository Statistics**: View key GitHub metrics like stars, forks, commits, and contributors

## What Makes This Stand Out

### Technical Excellence

1. **Comprehensive Analysis Engine**: Evaluates multiple dimensions of code quality beyond simple metrics
2. **Modern Tech Stack**: Built with React, TypeScript, Tailwind CSS, and Supabase
3. **Serverless Architecture**: Uses Supabase Edge Functions for scalable, real-time analysis
4. **GitHub API Integration**: Fetches detailed repository data including commits, branches, PRs, and more
5. **Persistent Storage**: All analyses are stored in Supabase for future reference and sharing

### User Experience

1. **Stunning UI/UX**: Modern, gradient-rich design with smooth animations and transitions
2. **Responsive Design**: Works beautifully on all devices from mobile to desktop
3. **Instant Feedback**: Real-time loading states and progress indicators
4. **Clear Actionability**: Every recommendation is specific and immediately implementable

### Intelligence

1. **Multi-Factor Evaluation**: Analyzes code structure, documentation quality, test coverage, commit history, and more
2. **Contextual Roadmaps**: Provides improvement suggestions based on actual gaps in the repository
3. **Priority System**: Roadmap items are categorized by priority (high/medium/low)
4. **README Quality Assessment**: Evaluates documentation completeness and structure

## How It Works

1. **Input**: User provides a GitHub repository URL
2. **Fetch**: System retrieves repository data via GitHub API:
   - Repository metadata (stars, forks, description)
   - File structure and languages
   - README content and quality
   - Commit history and contributors
   - Branches, pull requests, and issues
   - Test file detection
3. **Analyze**: Intelligent scoring algorithm evaluates:
   - Code quality based on repository maturity and engagement
   - Documentation completeness and clarity
   - Testing practices and coverage indicators
   - Project structure and organization
   - Development activity and consistency
   - Version control best practices
4. **Generate**: Creates three key outputs:
   - Overall score (0-100) and rating
   - Written summary highlighting strengths and weaknesses
   - Personalized roadmap with prioritized improvement steps
5. **Store**: Saves analysis to Supabase for future reference
6. **Display**: Beautiful results page with visualizations and export options

## Technology Stack

- **Frontend**: React 18 + TypeScript
- **Styling**: Tailwind CSS for modern, responsive design
- **Icons**: Lucide React for beautiful, consistent icons
- **Backend**: Supabase Edge Functions (Deno runtime)
- **Database**: Supabase PostgreSQL with Row Level Security
- **API Integration**: GitHub REST API v3
- **Build Tool**: Vite for fast development and optimized production builds

## Setup Instructions

### Prerequisites

- Node.js 18+ installed
- Supabase account and project
- GitHub account (for testing repositories)

### Installation

1. Install dependencies:
   ```bash
   npm install
   ```

2. Configure environment variables:
   - Update `.env` file with your Supabase credentials:
     ```
     VITE_SUPABASE_URL=your_supabase_project_url
     VITE_SUPABASE_ANON_KEY=your_supabase_anon_key
     ```

3. The database schema and edge function are already deployed via Supabase MCP tools

4. Start the development server:
   ```bash
   npm run dev
   ```

5. Build for production:
   ```bash
   npm run build
   ```

## Usage

1. Open the application in your browser
2. Paste a GitHub repository URL (e.g., `https://github.com/username/repository`)
3. Click "Analyze Repository"
4. Wait for the analysis to complete (usually 10-30 seconds)
5. View your results with detailed metrics and roadmap
6. Share your results using the share button
7. Download a text report using the download button

## Key Components

### Database Schema

- `repository_analyses` table stores all analysis results
- Includes score, rating, summary, roadmap, and detailed metrics
- Public read access for shareable results
- Indexed for fast lookups by URL and date

### Edge Function

- `analyze-repository`: Comprehensive GitHub API integration
- Fetches repository data from multiple endpoints
- Calculates scores across 6 dimensions
- Generates personalized improvement recommendations
- Stores results in database

### Frontend Components

- `Hero.tsx`: Landing page with feature highlights
- `RepositoryAnalyzer.tsx`: Input form with validation and loading states
- `AnalysisResults.tsx`: Results display with visualizations and export
- `supabase.ts`: API client and data fetching utilities

## Evaluation Criteria

The analysis evaluates repositories on these weighted factors:

- **Code Quality (25%)**: Stars, forks, language diversity, file count
- **Documentation (20%)**: README quality, descriptions, documentation completeness
- **Testing (20%)**: Presence of test files and test coverage indicators
- **Structure (15%)**: File organization, multi-file projects, branching
- **Activity (10%)**: Commit frequency, contributors, recent updates
- **Best Practices (10%)**: Version control usage, pull requests, branches

## Roadmap Generation

The system generates personalized roadmaps based on metric weaknesses:

- Low documentation score → Add/improve README and documentation
- Low testing score → Implement unit and integration tests
- Low best practices score → Use feature branches and pull requests
- Low structure score → Improve project organization
- Low code quality → Refactor and add code comments
- Always suggests CI/CD and expanded documentation as growth opportunities

## Future Enhancements

- Integration with additional code quality tools (ESLint, Prettier, etc.)
- Support for private repositories with OAuth
- Historical tracking of repository improvements over time
- Comparison with similar repositories
- AI-powered code review with specific file-level recommendations
- Integration with LinkedIn for profile showcase
- Batch analysis for multiple repositories
- Custom scoring weights based on project type

## Built For

UnsaidTalks GitGrade Hackathon - Theme: AI + Code Analysis + Developer Profiling

## License

MIT License - feel free to use and modify for your own purposes.

## Author

Built with passion for improving developer profiles and code quality.
