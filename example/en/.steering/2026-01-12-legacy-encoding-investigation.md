Date: 2026-01-12
Related: none
Status: completed

# Check character encoding on legacy screens

There may be a mojibake risk in an old screen being modified, so first check the encoding of nearby files.

## TODO
- [x] check encodings in the target directory
- [x] inspect how Japanese text is used in Shift_JIS files
- [x] check charset settings
- [x] verify the real environment
- [x] update docs

## Check target files

Scan the target directory.

- UTF-8: 124
- Shift_JIS: 31
- undetermined: 4

More Shift_JIS remains than expected.

A quick look at the 31 files shows many Japanese comments. If it is only comments, it should not affect this screen, so separate string literals from template output and inspect them.

The four undetermined files are text rather than images. Check them separately.

## Check the 31 Shift_JIS files

It is not limited to comments.

There are Japanese message strings and text directly embedded in old templates. Some of them are reachable from the target screen.

So this is not just a case of old unused files remaining in the repository.

The screen currently renders normally, so charset must line up somewhere. Check the web-side settings.

## charset settings

The repository's server configuration has Shift_JIS as the default charset.

Some of the templates being touched are UTF-8.

Taken alone, that makes the UTF-8 side look suspicious, but there are no reports of mojibake on the current screen.

Do not conclude from the config file alone. Check the response headers in the real environment.

## Verify the real environment

The target screen response is UTF-8. No mojibake.

However, the Server header does not match the web server documented in docs.

This started as an encoding investigation, but the server-architecture assumption may also need checking.

### Additional checks
- is the architecture diagram in docs outdated?
- is there another server or proxy in front?
- is charset overridden elsewhere?

## Search configuration again

Found a charset override to UTF-8 under the application area. It was outside the initial search scope.

That explains the rendered page. The Server-header discrepancy remains.

## Confirm with operations

Operations confirmed that a reverse proxy exists in front of the application.

It was not documented.

No direct fix is required for the mojibake investigation, but leaving this undocumented could mislead future server investigations, so update the specification.

## Correction

Earlier, the working suspicion was that the UTF-8 templates were likely problematic because the repository-level default charset was Shift_JIS.

That was based on missing the application-level override.

Production is correctly serving UTF-8.

The four previously undetermined files were also checked individually; all four are Shift_JIS.

## Update docs

Updated `docs/SPEC.md` with the encoding behavior, HTTP response behavior, and front proxy. Complete.
