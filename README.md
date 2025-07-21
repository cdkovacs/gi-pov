# GitHub/IBM POV Documents

This repository contains technical point of view (POV) documents for GitHub and IBM.

## Available Documents
Documents without links are in progress and not yet published.

- [GitHub Organization Design](organization-design.md)
- [Branching Strategies](branching-strategies.md)
- [Deployment Diagram](deployment.md)

## GitHub Pages Publication

This repository is configured to automatically publish Markdown documents to GitHub Pages using a custom GitHub Actions workflow. When changes are pushed to the main branch, the workflow converts Markdown files to HTML using Jekyll and publishes them to GitHub Pages.

### How It Works

1. The workflow is defined in [.github/workflows/publish-markdown.yml](.github/workflows/publish-markdown.yml)
2. It triggers automatically when Markdown files are changed in the repository
3. Jekyll is used to convert Markdown to HTML with the Cayman theme
4. The generated site is deployed to GitHub Pages

### Setting Up GitHub Pages

To enable GitHub Pages for this repository:

1. Go to the repository settings
2. Navigate to the "Pages" section
3. Under "Source", select "GitHub Actions"
4. The site will be published at `https://<username>.github.io/<repository>/`

### Local Development

To preview the site locally:

1. Install Ruby and Jekyll
   ```bash
   gem install jekyll bundler
   ```

2. Clone the repository
   ```bash
   git clone https://github.com/<username>/<repository>.git
   cd <repository>
   ```

3. Install dependencies
   ```bash
   bundle install
   ```

4. Start the Jekyll server
   ```bash
   bundle exec jekyll serve
   ```

5. Open your browser to `http://localhost:4000`

## Contributing

To contribute to this repository:

1. Create a new branch for your changes
2. Make your changes to the Markdown files
3. Submit a pull request to the main branch
4. After approval and merge, the GitHub Action will automatically publish the updated content
