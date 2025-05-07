# Versioning Approaches

Proper versioning is essential for managing releases and communicating changes to users. Different versioning schemes serve different purposes:

### Semantic Versioning (SemVer)

SemVer uses a three-part version number (MAJOR.MINOR.PATCH) where:
- MAJOR version increments for incompatible API changes
- MINOR version increments for backward-compatible new functionality
- PATCH version increments for backward-compatible bug fixes

SemVer works well with branch-for-release strategies, as each release branch can be associated with a specific version number.

### Calendar Versioning (CalVer)

CalVer bases version numbers on the date of release (e.g., YY.MM.DD or YYYY.MM). This approach:
- Provides clear information about when a release was made
- Works well for projects with regular release schedules
- Is less focused on the nature of changes and more on the timing

CalVer is often used with trunk-based development and continuous delivery, where releases happen frequently.

### Romantic Versioning (RomVer)

RomVer uses more subjective criteria for version increments, focusing on the significance of changes rather than strict compatibility rules. This approach:
- Allows for more flexibility in version numbering
- Can better reflect the perceived importance of changes
- May be less predictable for consumers of the software

### Conventional Commits for Automated Versioning

Conventional Commits is a specification for commit messages that makes them machine-readable, enabling automated version increments and changelog generation. The format includes:
- A type (e.g., feat, fix, chore)
- An optional scope
- A description
- Optional body and footer

For example: `feat(api): add user authentication endpoint`

When combined with SemVer, Conventional Commits can automate version increments:
- `feat:` commits trigger a MINOR version increment
- `fix:` commits trigger a PATCH version increment
- Commits with a `BREAKING CHANGE:` footer trigger a MAJOR version increment

This approach integrates well with CI/CD pipelines and reduces the manual effort required for release management.

# References and Additional Reading

- [Semantic Versioning Specification](https://semver.org/)  
- [Romantic Versioning Specification](https://github.com/romversioning/romver)  
- [Conventional Commits Specification](https://www.conventionalcommits.org/)  