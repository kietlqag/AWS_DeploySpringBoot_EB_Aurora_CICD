# AWS Workshop Template

Hugo-based workshop template for AWS tutorials and guides.

## Features

- Multi-language support (English and Vietnamese)
- Responsive design with Hugo Learn theme
- Image optimization
- GitHub Pages ready

## Local Development

1. Install Hugo Extended version
2. Clone this repository
3. Run `hugo server` for local development
4. Visit `http://localhost:1313`

## Deployment

### GitHub Pages

1. Push code to GitHub repository
2. Enable GitHub Pages in repository settings
3. Set source to GitHub Actions or main branch
4. Site will be available at `https://username.github.io/repository-name/`

### Netlify

1. Connect repository to Netlify
2. Build command: `hugo --minify`
3. Publish directory: `public`
4. Site will be deployed automatically

## Structure

```
content/
├── _index.md          # Homepage
├── 1-Gioi-thieu/      # Introduction
├── 2-Chuan-bi-moi-truong/  # Environment setup
├── 3-Tao-VPC-tuy-chinh/    # VPC creation
├── 4-Tao-Security-Groups/  # Security groups
├── 5-Tao-Aurora-Database/  # Database setup
├── 6-Deploy-Elastic-Beanstalk/  # Application deployment
├── 7-Test-ung-dung/   # Testing
├── 8-Cau-hinh-CI-CD/  # CI/CD configuration
└── 9-Don-dep-tai-nguyen/  # Cleanup
```

## Configuration

- `config.toml`: Main Hugo configuration
- `static/_redirects`: Redirect rules for SPA routing
- `netlify.toml`: Netlify deployment configuration

## Issues Fixed

- ✅ Fixed baseURL configuration
- ✅ Fixed image file extensions
- ✅ Added proper redirect rules
- ✅ Configured for GitHub Pages deployment 