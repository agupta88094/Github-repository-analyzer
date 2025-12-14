# GitGrade Deployment Guide

This guide will help you deploy the GitGrade application and configure all necessary services.

## Prerequisites

- Supabase account (free tier works)
- Node.js 18+ installed locally
- GitHub account for testing

## Step 1: Supabase Setup

### 1.1 Create a Supabase Project

1. Go to [supabase.com](https://supabase.com) and create an account
2. Create a new project
3. Wait for the project to be provisioned (takes 1-2 minutes)
4. Note down your project URL and anon key from Project Settings > API

### 1.2 Database Configuration

The database schema has been created with the following migration. If needed, you can run this manually in the SQL Editor:

```sql
CREATE TABLE IF NOT EXISTS repository_analyses (
  id uuid PRIMARY KEY DEFAULT gen_random_uuid(),
  repository_url text NOT NULL,
  repository_name text NOT NULL,
  owner text NOT NULL,
  score integer NOT NULL CHECK (score >= 0 AND score <= 100),
  rating text NOT NULL CHECK (rating IN ('Beginner', 'Intermediate', 'Advanced')),
  summary text NOT NULL,
  roadmap jsonb NOT NULL DEFAULT '[]'::jsonb,
  metrics jsonb NOT NULL DEFAULT '{}'::jsonb,
  analyzed_at timestamptz DEFAULT now(),
  created_at timestamptz DEFAULT now()
);

ALTER TABLE repository_analyses ENABLE ROW LEVEL SECURITY;

CREATE POLICY "Anyone can view analyses"
  ON repository_analyses
  FOR SELECT
  TO public
  USING (true);

CREATE POLICY "Anyone can create analyses"
  ON repository_analyses
  FOR INSERT
  TO public
  WITH CHECK (true);

CREATE INDEX IF NOT EXISTS idx_repository_analyses_url
  ON repository_analyses(repository_url);

CREATE INDEX IF NOT EXISTS idx_repository_analyses_created_at
  ON repository_analyses(created_at DESC);
```

### 1.3 Edge Function Deployment

The edge function `analyze-repository` has been deployed using Supabase MCP tools. If you need to redeploy:

1. Go to Supabase Dashboard > Edge Functions
2. You should see `analyze-repository` function listed
3. The function is automatically configured with environment variables

### 1.4 Verify Edge Function

Test the edge function:

```bash
curl -X POST 'https://YOUR_PROJECT_URL.supabase.co/functions/v1/analyze-repository' \
  -H 'Authorization: Bearer YOUR_ANON_KEY' \
  -H 'Content-Type: application/json' \
  -d '{"repositoryUrl": "https://github.com/facebook/react"}'
```

## Step 2: Frontend Configuration

### 2.1 Environment Variables

Update the `.env` file with your Supabase credentials:

```env
VITE_SUPABASE_URL=https://your-project-id.supabase.co
VITE_SUPABASE_ANON_KEY=your-anon-key-here
```

### 2.2 Install Dependencies

```bash
npm install
```

### 2.3 Development Mode

```bash
npm run dev
```

The application will be available at `http://localhost:5173`

### 2.4 Production Build

```bash
npm run build
```

This creates an optimized build in the `dist` folder.

## Step 3: Deployment Options

### Option 1: Vercel (Recommended)

1. Push your code to GitHub
2. Go to [vercel.com](https://vercel.com) and import your repository
3. Add environment variables:
   - `VITE_SUPABASE_URL`
   - `VITE_SUPABASE_ANON_KEY`
4. Deploy

### Option 2: Netlify

1. Push your code to GitHub
2. Go to [netlify.com](https://netlify.com) and import your repository
3. Build settings:
   - Build command: `npm run build`
   - Publish directory: `dist`
4. Add environment variables in Site settings > Environment variables
5. Deploy

### Option 3: Static Hosting

After running `npm run build`, you can deploy the `dist` folder to:
- GitHub Pages
- AWS S3 + CloudFront
- Firebase Hosting
- Any static hosting provider

## Step 4: Testing

### Test with Sample Repositories

Try analyzing these repositories to test different scores:

1. **High Score Example**: `https://github.com/facebook/react`
2. **Medium Score Example**: `https://github.com/your-username/small-project`
3. **Low Score Example**: A personal project with minimal commits

### Verify Features

- [ ] Repository URL input and validation
- [ ] Analysis progress indicator
- [ ] Results display with all metrics
- [ ] Score calculation (0-100)
- [ ] Rating display (Beginner/Intermediate/Advanced)
- [ ] Roadmap generation with priorities
- [ ] Share functionality (URL generation)
- [ ] Download report feature
- [ ] Repository statistics display
- [ ] Mobile responsive design

## Step 5: GitHub API Rate Limits

### Understanding Limits

- **Unauthenticated requests**: 60 requests/hour per IP
- **Authenticated requests**: 5,000 requests/hour

### Adding GitHub Token (Optional)

To increase rate limits, add GitHub authentication to the edge function:

1. Create a GitHub Personal Access Token:
   - Go to GitHub Settings > Developer settings > Personal access tokens
   - Create a token with `public_repo` scope
2. Add to Supabase Edge Function secrets:
   - In Supabase Dashboard, go to Edge Functions
   - Add secret `GITHUB_TOKEN` with your token
3. Update the edge function to use the token in headers

## Troubleshooting

### Issue: "Repository not found"

- Ensure the URL is correct and the repository is public
- Check GitHub API rate limits
- Verify your internet connection

### Issue: "Failed to analyze repository"

- Check Supabase Edge Function logs in the dashboard
- Verify environment variables are set correctly
- Ensure the edge function is deployed and running

### Issue: Analysis takes too long

- Large repositories with many commits may take 30-60 seconds
- This is normal due to GitHub API pagination
- Consider adding a timeout message for user feedback

### Issue: Blank page after deployment

- Check browser console for errors
- Verify environment variables are set in deployment platform
- Ensure CORS is properly configured
- Check that the Supabase URL and key are correct

## Performance Tips

1. **Enable Caching**: Consider caching analysis results for frequently analyzed repositories
2. **GitHub Token**: Add authenticated GitHub API access to increase rate limits
3. **CDN**: Use a CDN for static assets in production
4. **Database Indexes**: Indexes are already created for common queries
5. **Edge Function**: Functions run on Deno runtime which is fast and efficient

## Security Considerations

1. **Row Level Security**: Already enabled on database tables
2. **Public Access**: Analyses are publicly readable by design (for sharing)
3. **Rate Limiting**: Consider implementing application-level rate limiting
4. **Input Validation**: URL validation is implemented on both frontend and backend
5. **CORS**: Properly configured in edge function

## Monitoring

### Supabase Dashboard

Monitor your application usage:
- Database > Table Editor to view analyses
- Edge Functions > Logs to see function execution
- Database > API to check query performance
- Project Settings > Usage to track resource consumption

### Analytics (Optional)

Consider adding:
- Google Analytics for page views
- Plausible Analytics for privacy-friendly tracking
- Custom event tracking for analysis completions

## Scaling

The application is designed to scale automatically:
- **Supabase**: Auto-scales database and edge functions
- **Serverless**: Edge functions scale to demand
- **Static Frontend**: CDN-ready and highly cacheable

## Support

For issues or questions:
1. Check the README.md for feature documentation
2. Review Supabase logs for backend issues
3. Check browser console for frontend errors
4. Verify GitHub API status at [githubstatus.com](https://www.githubstatus.com)

## Next Steps

After deployment:
1. Test with various repositories
2. Share your deployed URL
3. Gather user feedback
4. Monitor performance and errors
5. Consider implementing suggested future enhancements from README

## License

MIT License - feel free to modify and deploy as needed.
