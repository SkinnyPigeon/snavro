# Release Process

This document describes how to create releases for the Snavro package.

## Automated Release Process

The package uses GitHub Actions to automatically publish to PyPI when a release is created. The workflow is triggered by creating a release tag.

## Prerequisites

1. **GitHub Secret**: Ensure `TWINE_TOKEN` is set in your repository secrets with your PyPI API token
2. **Git Flow**: This process assumes you're using git flow for branch management

## Release Steps

### 1. Prepare Release Branch (Git Flow)

```bash
# Start a new release branch
git flow release start v0.2.0

# Make any final changes, update CHANGELOG.md, etc.
# The version in pyproject.toml will be automatically updated by the workflow

# Finish the release
git flow release finish v0.2.0
```

### 2. Push Tags and Branches

```bash
# Push the main branch
git push origin main

# Push the develop branch
git push origin develop

# Push the tag
git push origin v0.2.0
```

### 3. Create GitHub Release

1. Go to your repository on GitHub
2. Click "Releases" → "Create a new release"
3. Choose the tag you just pushed (e.g., `v0.2.0`)
4. Fill in the release title and description
5. Click "Publish release"

### 4. Automated Workflow

Once you publish the release, the GitHub Actions workflow will:

1. **Test**: Run tests across Python 3.8-3.12
2. **Build**: Create wheel and source distributions
3. **Publish**: Upload to PyPI using your `TWINE_TOKEN`

## Manual Release (if needed)

If you need to publish manually:

```bash
# Clean previous builds
rm -rf dist/ build/ *.egg-info/

# Build the package
python -m build

# Check the package
twine check dist/*

# Upload to PyPI
twine upload dist/*
```

## Version Numbering

Follow [Semantic Versioning](https://semver.org/):

- **MAJOR** version for incompatible API changes
- **MINOR** version for backwards-compatible functionality additions
- **PATCH** version for backwards-compatible bug fixes

Examples:
- `v0.1.0` - Initial release
- `v0.1.1` - Bug fix
- `v0.2.0` - New features
- `v1.0.0` - Stable API

## Workflow Features

- ✅ **Multi-Python Testing**: Tests on Python 3.8-3.12
- ✅ **Code Quality**: Black formatting, flake8 linting, mypy type checking
- ✅ **Automatic Versioning**: Updates `pyproject.toml` version from git tag
- ✅ **Build Verification**: Checks package integrity before publishing
- ✅ **Secure Publishing**: Uses PyPI API tokens
- ✅ **Manual Trigger**: Can be triggered manually if needed

## Troubleshooting

### Workflow Fails
- Check the Actions tab in your GitHub repository
- Ensure all tests pass locally before releasing
- Verify `TWINE_TOKEN` is correctly set in repository secrets

### Version Conflicts
- Ensure the tag version doesn't already exist on PyPI
- Use `git tag -d v0.x.x` to delete local tags if needed
- Use `git push origin :refs/tags/v0.x.x` to delete remote tags

### PyPI Upload Issues
- Verify your PyPI token has the correct permissions
- Check that the package name isn't already taken
- Ensure the version number is higher than the current PyPI version 