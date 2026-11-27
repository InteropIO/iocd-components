
# Release notifications

The `Notify Release` workflow sends a `component-release` repository dispatch to
`InteropIO/iocd-seed`. The seed workflow handles Desktop (`iocd`) releases by
updating the appropriate release branch and starting its installer build.

Notifications use the organization's `iocd-release-bot` GitHub App. Each run
creates an installation token restricted to `iocd-seed` with **Contents: write**,
the permission required by GitHub's repository dispatch API. The token expires
after one hour and the token action revokes it when the job finishes. The App's
private key does not have a scheduled expiry, so no personal access token needs
periodic renewal.

Before enabling this workflow:

1. Include `InteropIO/iocd-seed` in the `iocd-release-bot` installation's selected
   repositories. The App already has **Contents: write** permission.
2. Set the `RELEASE_APP_CLIENT_ID` Actions variable in `iocd-components` to the
   App's client ID (`Iv23li7TVIHiXMyFLzsp`).
3. Store an App private key in the `RELEASE_APP_PRIVATE_KEY` Actions secret in
   `iocd-components`. Keep the key out of source control and logs. The workflow's
   token scope does not limit the private key itself: it can mint tokens for
   other repositories available to this App installation.

To verify the migration without starting an installer build, manually run
`Notify Release` with release name `notificationtest-v0.0.0`, `is_dev=true`,
`is_prerelease=true`, `is_nightly=false`, and `sign_windows=false`. Confirm that
token creation and dispatch succeed and that the seed receiver skips its
`update-iocd` job because this is not an `iocd` component. Then confirm the next
real Desktop release starts its seed build. Notification failures now fail the
workflow instead of leaving a successful run with only an error in its log.

After verification, remove the unused `IOCD_SEED_COMPONENTS_WRITE` secret and
revoke the old PAT. Do not remove it before the App configuration is ready.

# Component Naming Conventions

## Asset Filename Formats

All component assets must follow specific naming patterns to be properly recognized and parsed by the CLI:

### 1. OS-Specific with Platform and Architecture
```
<component-name>-<version>-<platform>-<arch>.<extension>
<component-name>-<version>.<platform>-<arch>.<extension>
```

**Examples:**
- `iocd-v10.0.1-build.20250827150311-win32-x64.zip` (dash-separated)
- `iocd-v10.0.0-build.20250927121929.darwin-arm64.dmg` (dot-separated after build)
- `myapp-v1.2.3-win32-x64.zip` (without build info)

### 2. OS-Specific with Platform Only
```
<component-name>-<version>-<platform>.<extension>
<component-name>-<version>.<platform>.<extension>
```

**Examples:**
- `bbgv2-v2.0.0.1-win32.zip`
- `myapp-v1.0.0-build.123-darwin.dmg`
- `component-v1.0.0-build.123.darwin.zip` (dot-separated)

### 3. OS-Agnostic
```
<component-name>-<version>.<extension>
```

**Examples:**
- `webapp-v10.0.0.zip`
- `webapp-v10.0.0-build.20250927121929.zip`

## Component Names
- Can contain alphanumeric characters, hyphens, and underscores
- Should be descriptive and unique
- Examples: `iocd`, `bb-gv2`, `web-groups-app`, `custom-component`

## Version Formats

The system supports several version formats:

#### Standard Semantic Versioning
```
v<X.Y.Z>
```
**Examples:** `v1.0.0`, `v10.5.2`

#### Four-Part Versioning
```
v<X.Y.Z.B>
```
**Examples:** `v10.0.0.1`, `v2.1.3.5`

#### Build Timestamp Versions
```
v<X.Y.Z>-build.<TIMESTAMP>
```
**Examples:** `v10.0.0-build.20250927121929`, `v1.2.3-build.123456789`

## Platform Identifiers
- **win32**: Windows (x86/x64)
- **darwin**: macOS (Intel/Apple Silicon)

## Architecture Identifiers
- **x64**: 64-bit Intel/AMD
- **x86**: 32-bit Intel/AMD
- **arm64**: ARM 64-bit (Apple Silicon)
- **ia32**: 32-bit Intel

### File Extensions
- **Windows**: `.zip`, `.exe`, `.msi`
- **macOS**: `.dmg`, `.zip`
- **Cross-platform**: `.zip`

