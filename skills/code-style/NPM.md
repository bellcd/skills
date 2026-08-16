# npm

- Use the latest stable version of a package, subject to peerDependencies.
- Always install exact versions. Some packages don't respect semver. Pinning is worth the defensiveness.
- Never run a bare `npm install <package>`. Look the version up first, then install it with `npm install <package>@<version> -E` so npm writes the exact entry in one step, rather than installing and stripping the caret afterwards. `package.json` entries are exact too, no carets anywhere.
- Don't add a script variant whose only difference is a prepended env var, such as `test:unit:sandbox` for `SANDBOX=1 vitest run`. The environment signal belongs to whatever launches the command, either inline at the call site or in a launcher wrapper. Mirrored variants double the script surface and drift.
