# Fix: Installing Homebrew Ruby to Solve Bundle Issues

Your system Ruby is having trouble compiling native extensions. Here's how to fix it:

## Step-by-Step Solution

### Step 1: Install Homebrew Ruby

Open your terminal and run:

```bash
brew install ruby
```

This will take a few minutes to download and install.

### Step 2: Add Homebrew Ruby to Your PATH

**For zsh (default on modern macOS):**

```bash
echo 'export PATH="/opt/homebrew/opt/ruby/bin:$PATH"' >> ~/.zshrc
echo 'export PATH="/opt/homebrew/lib/ruby/gems/3.3.0/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc
```

**For bash:**

```bash
echo 'export PATH="/opt/homebrew/opt/ruby/bin:$PATH"' >> ~/.bash_profile
echo 'export PATH="/opt/homebrew/lib/ruby/gems/3.3.0/bin:$PATH"' >> ~/.bash_profile
source ~/.bash_profile
```

### Step 3: Verify the Installation

Close and reopen your terminal (or open a new terminal window), then run:

```bash
which ruby
```

You should see:
```
/opt/homebrew/opt/ruby/bin/ruby
```

Also check the version:
```bash
ruby -v
```

You should see a version like `ruby 3.3.x` (newer than 2.6).

### Step 4: Install Bundler

```bash
gem install bundler
```

### Step 5: Restore Original Gemfile

Since we backed it up:

```bash
cp Gemfile.backup Gemfile
```

### Step 6: Install Jekyll Dependencies

```bash
cd /Users/saurav/Desktop/ashukid.github.io
rm -rf vendor/bundle
bundle install
```

This should now work without errors!

### Step 7: Run Your Site

```bash
bundle exec jekyll serve
```

Then open http://localhost:4000 in your browser.

---

## Why This Works

- Homebrew Ruby is properly configured for your Mac architecture
- It has all the necessary development tools linked correctly
- It avoids permission issues with the system Ruby
- It's the recommended way to develop Ruby projects on macOS

---

## Need Help?

If you encounter any issues during installation, make sure:
1. Homebrew is up to date: `brew update`
2. You have Xcode Command Line Tools: `xcode-select --install`
3. Your PATH is set correctly (check with `which ruby` after restarting terminal)

