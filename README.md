# Visual Database CI

> Automated database documentation and ERD visualization for Laravel projects

This project automatically generates **database documentation** and **interactive ER diagrams** whenever database migrations change. It uses GitHub Actions to build and commit documentation back to the repository.

## Features

- **Auto-generated Documentation**: Database schema documented as Markdown with table definitions, columns, types, and relationships
- **Visual ER Diagrams**: Interactive HTML-based entity relationship diagrams powered by [Liam ERD](https://liam.sh)
- **CI/CD Integration**: Automatically triggered on migration changes via GitHub Actions
- **Version Controlled**: Documentation committed to repository for easy tracking of schema changes
- **Local Preview**: View visual documentation locally with a simple command

## How It Works

```
┌─────────────────┐    ┌──────────────────┐    ┌─────────────────┐
│  Push Migrations│ -> │  GitHub Action   │ -> │  Generate Docs  │
│   to main       │    │    Triggered     │    │   & ERD Visual  │
└─────────────────┘    └──────────────────┘    └────────┬────────┘
                                                        │
                                                        v
                                              ┌─────────────────┐
                                              │  Commit & Push  │
                                              │  to Repository  │
                                              └─────────────────┘
```

## Tools Used

| Tool | Purpose |
|------|---------|
| [tbls](https://github.com/k1LoW/tbls) | Documentation-as-Code for database schemas |
| [Liam ERD](https://liam.sh) | Visual ER diagram generation from schema JSON |
| [GitHub Actions](https://github.com/features/actions) | CI/CD automation |

## Project Structure

```
visual-database-ci/
├── .github/workflows/
│   └── db-docs.yml          # GitHub Actions workflow
├── database/migrations/      # Laravel migrations
├── docs/database/           # Generated documentation (auto-committed)
│   ├── schema.json         # Database schema in JSON format
│   ├── visual/             # Interactive ERD HTML files
│   └── *.md                # Table documentation in Markdown
├── .tbls.yml               # tbls configuration
└── package.json            # npm scripts for local preview
```

## Local Development

### Prerequisites

- PHP 8.4+
- Composer
- Node.js (for local preview)
- Python 3 (for local HTTP server)

### Generate Documentation Locally

1. Run migrations to create database tables
2. Generate documentation with tbls:

```bash
tbls doc
```

### View Visual ER Diagrams Locally

Start a local HTTP server to view the interactive ER diagrams:

```bash
npm run db-docs
```

Then open http://localhost:8000 in your browser.

## Configuration

### tbls Configuration (`.tbls.yml`)

```yaml
dsn: mysql://root:root@127.0.0.1:3306/laravel_test
docPath: docs/database

format:
  adjust: true
  sort: true

outputs:
  - markdown
  - json    # Required for Liam ERD

exclude:
  - migrations
  - password_reset_tokens
  - failed_jobs
```

### GitHub Actions Workflow (`.github/workflows/db-docs.yml`)

The workflow:

1. Sets up MySQL service container
2. Installs PHP & Composer dependencies
3. Runs database migrations
4. Generates documentation with `tbls`
5. Builds visual ERDs with Liam CLI
6. Commits changes back to repository

**Key optimizations:**
- Composer caching for faster builds (~30s → 5s)
- `[skip ci]` in commit message to prevent workflow loops
- Force flag (`-f`) to overwrite existing documentation

## Adding New Tables

1. Create a Laravel migration:

```bash
php artisan make:migration create_your_table_table
```

2. Define your schema in the migration file

3. Commit and push to `main` branch

4. GitHub Actions automatically generates updated documentation

5. Find updated docs in `docs/database/` folder

## Example Generated Documentation

### Markdown Output

Each table gets a dedicated Markdown file with:
- Table definition
- Column names, types, and constraints
- Relationships and foreign keys
- Indexes

### Visual ERD

Interactive HTML diagram with:
- Entity boxes showing tables and columns
- Relationship lines between tables
- Click-to-navigate connections
- Zoom and pan controls

## Troubleshooting

### Workflow fails with "ER diagram files already exists"

The `-f` flag in `tbls doc -f` should prevent this. Ensure the workflow file includes it.

### Local preview shows blank page

Make sure you've run `tbls doc` first to generate the `docs/database/` directory structure.

### Permission denied on git push in workflow

Verify `permissions: contents: write` is set in the workflow file.

## License

MIT
