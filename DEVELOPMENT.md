# Development Guide - LazyInq Blog

This guide will help you run and test the LazyInq blog locally on your machine.

## Quick Start

1. **Install dependencies:**

   ```bash
   bundle install
   ```

2. **Run the development server:**

   ```bash
   bundle exec jekyll serve
   ```

3. **Open your browser:**
   Visit http://localhost:4000

That's it! Your site should now be running locally.

> **Note:** If you encounter permission errors on macOS, see the [Troubleshooting](#macos-permission-issues-sudo-required-error) section below.

## Prerequisites

Before you start, make sure you have the following installed:

1. **Ruby** (version 2.6 or higher) - Check with: `ruby --version`
2. **Bundler** - Check with: `bundle --version`

If you don't have Ruby/Bundler installed on macOS:

```bash
# Using Homebrew
brew install ruby

# Install Bundler
gem install bundler
```

## Setup Instructions

### 1. Install Dependencies

Navigate to the project directory and install all required gems:

```bash
bundle install
```

This will install Jekyll and all other dependencies specified in the `Gemfile`.

### 2. Run the Development Server

Start the Jekyll development server:

```bash
bundle exec jekyll serve
```

Or with auto-regeneration (automatically rebuilds when files change):

```bash
bundle exec jekyll serve --livereload
```

### 3. Access Your Site

Once the server starts, you'll see output like:

```
Server address: http://127.0.0.1:4000/
Server running... press ctrl-c to stop.
```

Open your browser and navigate to:

- **Local URL**: http://localhost:4000
- **Network URL**: http://127.0.0.1:4000

## Testing Your Site

### What to Test:

1. **Home Page** (`/`)

   - Should display all blog posts in chronological order
   - Check that all 8 blog posts are visible

2. **Blogs Page** (`/blogs/`)

   - Click on "Blogs" in the navigation
   - Should list all blog posts
   - Each post should have title, date, and excerpt

3. **Individual Blog Posts**

   - Click on any blog post title
   - Should display the full blog post content
   - Check that images load correctly (if any)

4. **About Page** (`/about/`)

   - Click on "About me" in the navigation
   - Should display the about page

5. **Navigation**

   - Header should show "LazyInq" as the site title
   - Should have "Blogs" and "About me" links
   - All links should work correctly

6. **Responsive Design**
   - Test on different screen sizes
   - Check mobile navigation (hamburger menu if applicable)

## Common Commands

```bash
# Build the site without running server
bundle exec jekyll build

# Build and serve with auto-regeneration
bundle exec jekyll serve --livereload

# Build for production
JEKYLL_ENV=production bundle exec jekyll build

# Clean build (removes _site directory)
bundle exec jekyll clean
```

## Troubleshooting

### macOS Permission Issues (sudo required error) {#macos-permission-issues-sudo-required-error}

If you see "Bundler requires sudo access" error on macOS:

**Option 1: Use Homebrew Ruby (Recommended)**

```bash
# Install Ruby via Homebrew (avoids permission issues)
brew install ruby

# Add Homebrew Ruby to your PATH (add to ~/.zshrc or ~/.bash_profile)
echo 'export PATH="/opt/homebrew/opt/ruby/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc

# Install bundler
gem install bundler

# Now try bundle install again
bundle install
```

**Option 2: Configure Bundler to install locally**

```bash
# Configure bundler to install gems in vendor directory
bundle config set --local path 'vendor/bundle'

# Try installing again
bundle install
```

**Option 3: Use rbenv (Ruby Version Manager)**

```bash
# Install rbenv
brew install rbenv ruby-build

# Install a Ruby version
rbenv install 3.0.0
rbenv global 3.0.0

# Initialize rbenv (add to ~/.zshrc)
echo 'eval "$(rbenv init - zsh)"' >> ~/.zshrc
source ~/.zshrc

# Install bundler and dependencies
gem install bundler
bundle install
```

### Port Already in Use

If port 4000 is already in use:

```bash
bundle exec jekyll serve --port 4001
```

### Dependency Issues

If you encounter dependency issues:

```bash
bundle update
bundle install
```

### Build Errors

If the site doesn't build correctly:

```bash
# Clean and rebuild
bundle exec jekyll clean
bundle exec jekyll build
```

## File Structure

- `_posts/` - All blog posts (8 posts total)
- `_config.yml` - Site configuration
- `_layouts/` - Page layouts
- `_includes/` - Reusable HTML snippets
- `assets/` - Images, CSS, and other static files
- `blogs.md` - Blogs listing page
- `about.md` - About me page
- `index.md` - Home page

## Stopping the Server

Press `Ctrl+C` in the terminal to stop the Jekyll server.
