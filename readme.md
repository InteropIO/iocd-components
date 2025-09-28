
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

