<!--
SYNC IMPACT REPORT
==================
Version Change: 1.0.0 → 1.1.0
Modified Principles:
  - III: Year-Based Editioning (corrected archive command)
  - V: Convention Over Configuration (clarified WebP requirement)
Added Principles:
  - VI. Performance Optimization (new)
Added Sections:
  - Build Performance Standards subsection under Additional Constraints
Removed Sections: None
Templates Requiring Updates:
  - ✅ .specify/templates/plan-template.md (already aligned)
  - ✅ .specify/templates/spec-template.md (no changes needed)
  - ✅ .specify/templates/tasks-template.md (no changes needed)
  - ✅ CLAUDE.md (performance section exists, aligned)
Follow-up TODOs: None
-->

# DevFest Perros-Guirec Website Constitution

## Core Principles

### I. Static-First Architecture
All content MUST be statically generated at build time. Jekyll processes YAML front matter and Liquid templates into static HTML/CSS/JS. No server-side runtime dependencies beyond a web server for serving files.

**Rationale**: Static sites are fast, secure, and easy to deploy on GitHub Pages. They eliminate runtime vulnerabilities and scale effortlessly with CDN distribution.

### II. Content-Driven Configuration
All dynamic content (speakers, agenda, sponsors) MUST be defined in YAML front matter within `index.md` or `_data/commons.yml`. No content should be hardcoded in HTML templates.

**Rationale**: Centralizing content in YAML makes updates accessible to non-developers and enables conditional rendering of sections. It separates content from presentation.

### III. Year-Based Editioning
Each conference edition MUST be self-contained under `assets/YYYY/`. Speaker photos, custom styles, and edition-specific assets MUST reside in year-organized folders. Past editions MUST be archived using the `bundle exec archive YYYY` command.

**Rationale**: This enables historical preservation of past conferences while keeping the current edition clean. Archiving creates immutable snapshots that won't break when site structure changes.

### IV. Jekyll Build Validation
The Jekyll build (`bundle exec jekyll build`) MUST pass without errors or unhandled warnings before any change is considered complete. Liquid template errors, missing includes, or YAML syntax errors are blockers.

**Rationale**: The build process is the primary validation mechanism for this static site. A clean build ensures the site will deploy correctly to GitHub Pages.

### V. Convention Over Configuration
File naming and location MUST follow established conventions:
- Speaker photos: `assets/YYYY/photos_speakers/filename.webp` (WebP format required)
- Sponsor logos: `assets/img/logos_sponsors/` (WebP preferred, SVG acceptable)
- Gallery images: WebP with responsive sizes (400w, 800w, 1200w)
- Layouts: `_layouts/` with names matching `layout:` front matter
- Includes: `_includes/` referenced by `{% include %}`

**Rationale**: Consistent naming enables predictable behavior and makes the codebase maintainable by multiple contributors over years. WebP format provides superior compression while maintaining quality.

### VI. Performance Optimization
All changes MUST respect the established performance optimizations:
- Images MUST use `loading="lazy"` and `decoding="async"` attributes
- Critical CSS MUST remain inline in `<head>`
- Non-critical CSS MUST load asynchronously
- JavaScript MUST use `defer` attribute
- Image dimensions (width/height) MUST be specified to prevent layout shift

**Rationale**: Core Web Vitals impact user experience and search rankings. The site has achieved significant performance gains (42% size reduction, 36% faster builds) that must be preserved.

## Additional Constraints

### French Language Primary
All user-facing content MUST be in French. Error messages, UI labels, and documentation intended for end users MUST use French. Internal code comments and commit messages may use English for broader accessibility.

### GitHub Pages Compatibility
All features MUST be compatible with GitHub Pages deployment constraints:
- No custom plugins beyond the whitelisted set
- No server-side processing
- Assets must use relative paths that work when deployed to `username.github.io/repo-name/`

### Accessibility Standards
HTML output MUST maintain semantic structure and include appropriate ARIA labels where needed. Color contrast and keyboard navigation should be considered for all interactive elements.

### Build Performance Standards
The Jekyll build time SHOULD remain under 10 seconds for development builds. Exclusions in `_config.yml` MUST be used to prevent processing of archived assets. Large image files MUST be optimized before commit.

## Development Workflow

### Local Testing Required
All changes MUST be verified locally with `bundle exec jekyll serve` before committing. This includes:
- Visual inspection of affected pages
- Verification of responsive behavior
- Checking for broken links or missing assets

### Content Update Process
When adding speakers or agenda items:
1. Add content to `index.md` front matter
2. Place assets in correct year-organized folders
3. Verify section renders correctly (conditional display)
4. Run full Jekyll build to validate

### Image Optimization Process
When adding new images:
1. Place source files in appropriate directory
2. Run `./scripts/optimize-images.sh` to generate WebP versions
3. Update references to use `.webp` extension
4. Verify images display correctly at all responsive sizes

### Archiving Procedure
When an edition concludes:
1. Run `bundle exec archive YYYY` to create snapshot
2. Verify archived assets are correctly copied
3. Update `archives.md` with new entry
4. Test archive pages render correctly

## Governance

This constitution governs all development practices for the DevFest Perros-Guirec website. Amendments require:
1. Documentation of the proposed change and its rationale
2. Review for compatibility with Jekyll/GitHub Pages constraints
3. Update to CLAUDE.md if agent guidance is affected

All pull requests MUST verify compliance with these principles. Complexity or new dependencies must be justified against the static-first architecture constraint.

Version numbers follow semantic versioning:
- **MAJOR**: Backward incompatible governance/principle removals or redefinitions
- **MINOR**: New principle/section added or materially expanded guidance
- **PATCH**: Clarifications, wording, typo fixes, non-semantic refinements

**Version**: 1.1.0 | **Ratified**: 2026-02-17 | **Last Amended**: 2026-03-23
