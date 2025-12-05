# Quick Setup Guide - Fixing Bundle Install Errors

## The Problem

You're getting errors when running `bundle exec jekyll serve` because:
1. The system Ruby has compatibility issues with native extensions
2. The `github-pages` gem requires compiling native extensions

## Solution Options

### Option 1: Use Homebrew Ruby (Recommended - Best Long-term Solution)

1. **Install Homebrew Ruby:**
   ```bash
   brew install ruby
   ```

2. **Add Homebrew Ruby to your PATH:**
   
   For zsh (default on macOS):
   ```bash
   echo 'export PATH="/opt/homebrew/opt/ruby/bin:$PATH"' >> ~/.zshrc
   echo 'export PATH="/opt/homebrew/lib/ruby/gems/3.3.0/bin:$PATH"' >> ~/.zshrc
   source ~/.zshrc
   ```
   
   For bash:
   ```bash
   echo 'export PATH="/opt/homebrew/opt/ruby/bin:$PATH"' >> ~/.bash_profile
   echo 'export PATH="/opt/homebrew/lib/ruby/gems/3.3.0/bin:$PATH"' >> ~/.bash_profile
   source ~/.bash_profile
   ```

3. **Verify you're using Homebrew Ruby:**
   ```bash
   which ruby
   # Should show: /opt/homebrew/opt/ruby/bin/ruby
   
   ruby -v
   # Should show a newer version (3.x)
   ```

4. **Install bundler and dependencies:**
   ```bash
   gem install bundler
   bundle install
   ```

5. **Run the server:**
   ```bash
   bundle exec jekyll serve
   ```

### Option 2: Use Simplified Gemfile (Quick Fix)

If you just want to get running quickly without installing Homebrew Ruby:

1. **Use the simplified Gemfile:**
   ```bash
   cp Gemfile.local Gemfile
   ```

2. **Install dependencies:**
   ```bash
   bundle install
   ```

3. **Run the server:**
   ```bash
   bundle exec jekyll serve
   ```

   **Note:** This uses a newer Jekyll version (4.3) instead of github-pages, which works great for local development but may differ slightly from GitHub Pages.

### Option 3: Use Docker (Alternative)

If you prefer containerized development:

1. Create a `Dockerfile` (I can help with this)
2. Run Jekyll in a Docker container

---

## After Setup

Once you've successfully installed dependencies, you can:

- **Start the server:**
  ```bash
  bundle exec jekyll serve
  ```

- **Start with auto-reload:**
  ```bash
  bundle exec jekyll serve --livereload
  ```

- **Access your site:**
  Open http://localhost:4000 in your browser

---

## Which Option Should You Choose?

- **Option 1 (Homebrew Ruby)** - Best if you plan to do more Ruby/Jekyll development
- **Option 2 (Simplified Gemfile)** - Quickest way to get running now
- **Option 3 (Docker)** - Good if you already use Docker

---

## Need Help?

If you're still having issues, check the `DEVELOPMENT.md` file for more troubleshooting tips.

