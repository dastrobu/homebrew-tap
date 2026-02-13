# Homebrew Tap Setup Complete ✅

This Homebrew tap is now ready to receive formula updates from GoReleaser.

## Repository Structure

```
homebrew-tap/
├── Formula/              # Formula files (auto-updated by GoReleaser)
│   └── .keep            # Placeholder (will be replaced)
├── README.md            # User-facing documentation
├── .gitignore           # Git ignore rules
└── SETUP.md             # This file
```

## How It Works

### Automatic Updates

When you create a release of `apple-mail-mcp`:

1. **You push a tag**: `git push origin v0.1.0`
2. **GitHub Actions runs**: Builds binaries via GoReleaser
3. **GoReleaser updates tap**: Creates/updates `Formula/apple-mail-mcp.rb`
4. **Users get the update**: `brew update && brew upgrade apple-mail-mcp`

### Formula Location

GoReleaser will create:
```
Formula/apple-mail-mcp.rb
```

This file will contain:
- Download URLs for the release archives
- SHA256 checksums
- Installation instructions
- Test commands

## GitHub Token Setup

For GoReleaser to update this tap, you need a GitHub token:

### 1. Create Personal Access Token

Go to: https://github.com/settings/tokens/new

**Settings:**
- Token name: `GoReleaser Homebrew Tap`
- Expiration: No expiration (or 1 year if preferred)
- Scopes: **`repo`** (full control of private repositories)

**Why `repo` scope?**
- Allows GoReleaser to push commits to this tap repository
- Allows creating/updating formula files

### 2. Add Token to apple-mail-mcp Repository

Go to: https://github.com/dastrobu/apple-mail-mcp/settings/secrets/actions

1. Click **"New repository secret"**
2. Name: `HOMEBREW_TAP_GITHUB_TOKEN`
3. Value: (paste your token)
4. Click **"Add secret"**

### 3. Verify Configuration

Check that `apple-mail-mcp/.goreleaser.yaml` has:

```yaml
brews:
  - repository:
      owner: dastrobu
      name: homebrew-tap
      token: "{{ .Env.HOMEBREW_TAP_GITHUB_TOKEN }}"
    
    directory: Formula
    
    homepage: https://github.com/dastrobu/apple-mail-mcp
    description: "MCP server providing programmatic access to macOS Mail.app"
```

## Testing the Tap

### Before First Release

The tap works but has no formulae yet:

```bash
# Add the tap
brew tap dastrobu/tap

# Try to install (will fail - no formula yet)
brew install apple-mail-mcp
# Error: No available formula with the name "apple-mail-mcp"
```

### After First Release

Once you push `v0.1.0` tag:

```bash
# Update Homebrew
brew update

# Install from tap
brew install dastrobu/tap/apple-mail-mcp

# Or if already tapped:
brew install apple-mail-mcp
```

## Manual Testing (Optional)

To test the tap setup before a real release:

1. **Create a test formula** (temporary):
   ```bash
   cd Formula
   cat > apple-mail-mcp.rb << 'RUBY'
   class AppleMailMcp < Formula
     desc "MCP server for macOS Mail.app"
     homepage "https://github.com/dastrobu/apple-mail-mcp"
     url "https://github.com/dastrobu/apple-mail-mcp/archive/refs/heads/main.tar.gz"
     version "0.0.0-test"
     sha256 "0000000000000000000000000000000000000000000000000000000000000000"
     
     def install
       bin.install "apple-mail-mcp"
     end
   end
   RUBY
   git add Formula/apple-mail-mcp.rb
   git commit -m "test: add temporary test formula"
   git push
   ```

2. **Test installation**:
   ```bash
   brew tap dastrobu/tap
   brew install apple-mail-mcp --build-from-source || true
   ```

3. **Remove test formula**:
   ```bash
   git rm Formula/apple-mail-mcp.rb
   git commit -m "test: remove temporary test formula"
   git push
   ```

## Troubleshooting

### Formula Not Found After Release

**Check:**
1. GitHub Actions completed successfully
2. Token `HOMEBREW_TAP_GITHUB_TOKEN` is set correctly
3. Token has `repo` scope
4. Formula file exists: https://github.com/dastrobu/homebrew-tap/blob/main/Formula/apple-mail-mcp.rb

**Fix:**
```bash
# Force update Homebrew
brew update --force

# Try again
brew install apple-mail-mcp
```

### GoReleaser Can't Push to Tap

**Error**: `authentication failed` or `permission denied`

**Fix:**
1. Verify token has `repo` scope
2. Regenerate token if needed
3. Update secret in repository settings

### Formula Has Wrong Version

**Fix:** GoReleaser updates the formula automatically on each release. Just:
```bash
brew update
brew upgrade apple-mail-mcp
```

## Maintenance

### This Tap is Self-Maintaining

- ✅ Formula files auto-updated by GoReleaser
- ✅ No manual edits needed
- ✅ Version bumps automatic
- ✅ Checksums calculated automatically

### Manual Updates (Rare)

Only needed if you want to change:
- Tap description in README.md
- Add more projects to the tap

## Repository URL

- **GitHub**: https://github.com/dastrobu/homebrew-tap
- **Clone**: `git clone https://github.com/dastrobu/homebrew-tap.git`
- **Tap**: `brew tap dastrobu/tap`

## Next Steps

1. ✅ Tap repository created
2. ✅ Basic structure set up
3. ⏳ Create GitHub token (if not done)
4. ⏳ Add `HOMEBREW_TAP_GITHUB_TOKEN` secret
5. ⏳ Create first release to test

See `apple-mail-mcp/RELEASE.md` for release instructions.
