# OpenAPI Specification Generation Guide

This document explains how the 15 OpenAPI YAML files in `resources/openapi/yaml/` are generated.

## Generation Process

### Prerequisites
- Java and Gradle installed
- Project dependencies resolved

### Generate All OpenAPI Specifications

Run the following command to generate all OpenAPI specifications:

```bash
./gradlew openapi
```

### What Happens During Generation

1. **Swagger Gradle Plugin** (version 2.2.40) is applied to specific API modules
2. The plugin scans Java classes annotated with Swagger/OpenAPI annotations:
   - `@Path`, `@GET`, `@POST`, `@PUT`, `@DELETE`
   - `@Schema`, `@Operation`, `@ApiResponse`
   - And other OpenAPI/Swagger annotations
3. YAML files are generated in each module's `build/docs/openapi/` directory

### Module Configuration

Each API module's `build.gradle.kts` contains:

```kotlin
plugins {
    `java-library`
    `maven-publish`
    id("io.swagger.core.v3.swagger-gradle-plugin")
}

edcBuild {
    swagger {
        apiGroup.set("api-group-name")  // e.g., "sts-api", "identity-api", "credentials-api"
    }
}
```

### File Locations

- **Generated location**: `<module>/build/docs/openapi/<api-name>.yaml`
- **Target location**: `resources/openapi/yaml/<api-group>/<api-name>.yaml`

## The 15 APIs

| API Group | API Name | Module Path | Output File |
|-----------|----------|-------------|-------------|
| **sts-api** | sts-api | `extensions/sts/sts-api` | `resources/openapi/yaml/sts-api/sts-api.yaml` |
| **identity-api** | did-api | `extensions/api/identity-api/did-api` | `resources/openapi/yaml/identity-api/did-api.yaml` |
| **identity-api** | keypair-api | `extensions/api/identity-api/keypair-api` | `resources/openapi/yaml/identity-api/keypair-api.yaml` |
| **identity-api** | participant-context-api | `extensions/api/identity-api/participant-context-api` | `resources/openapi/yaml/identity-api/participant-context-api.yaml` |
| **identity-api** | verifiable-credentials-api | `extensions/api/identity-api/verifiable-credentials-api` | `resources/openapi/yaml/identity-api/verifiable-credentials-api.yaml` |
| **issuer-admin-api** | attestation-api | `extensions/api/issuer-admin-api/attestation-api` | `resources/openapi/yaml/issuer-admin-api/attestation-api.yaml` |
| **issuer-admin-api** | credential-definition-api | `extensions/api/issuer-admin-api/credential-definition-api` | `resources/openapi/yaml/issuer-admin-api/credential-definition-api.yaml` |
| **issuer-admin-api** | credentials-api | `extensions/api/issuer-admin-api/credentials-api` | `resources/openapi/yaml/issuer-admin-api/credentials-api.yaml` |
| **issuer-admin-api** | holder-api | `extensions/api/issuer-admin-api/holder-api` | `resources/openapi/yaml/issuer-admin-api/holder-api.yaml` |
| **issuer-admin-api** | issuance-process-api | `extensions/api/issuer-admin-api/issuance-process-api` | `resources/openapi/yaml/issuer-admin-api/issuance-process-api.yaml` |
| **credentials-api** | credential-offer-api | `protocols/dcp/dcp-identityhub/credential-offer-api` | `resources/openapi/yaml/credentials-api/credential-offer-api.yaml` |
| **credentials-api** | credentials-api-configuration | `protocols/dcp/dcp-identityhub/credentials-api-configuration` | `resources/openapi/yaml/credentials-api/credentials-api-configuration.yaml` |
| **credentials-api** | presentation-api | `protocols/dcp/dcp-identityhub/presentation-api` | `resources/openapi/yaml/credentials-api/presentation-api.yaml` |
| **credentials-api** | storage-api | `protocols/dcp/dcp-identityhub/storage-api` | `resources/openapi/yaml/credentials-api/storage-api.yaml` |
| **issuer-api** | dcp-issuer-api | `protocols/dcp/dcp-issuer/dcp-issuer-api` | `resources/openapi/yaml/issuer-api/dcp-issuer-api.yaml` |

## Copy Files to Resources Directory

After generation, manually copy the files from `build/docs/openapi/` to `resources/openapi/yaml/`:

### Option 1: Manual Copy Script

Create a script `scripts/copy-openapi-specs.sh`:

```bash
#!/bin/bash
# Copy generated OpenAPI specs to resources directory

echo "Creating target directories..."
mkdir -p resources/openapi/yaml/{sts-api,identity-api,issuer-admin-api,issuer-api,credentials-api}

echo "Copying OpenAPI specifications..."

# STS API
cp extensions/sts/sts-api/build/docs/openapi/sts-api.yaml \
   resources/openapi/yaml/sts-api/

# Identity API
cp extensions/api/identity-api/did-api/build/docs/openapi/did-api.yaml \
   resources/openapi/yaml/identity-api/
cp extensions/api/identity-api/keypair-api/build/docs/openapi/keypair-api.yaml \
   resources/openapi/yaml/identity-api/
cp extensions/api/identity-api/participant-context-api/build/docs/openapi/participant-context-api.yaml \
   resources/openapi/yaml/identity-api/
cp extensions/api/identity-api/verifiable-credentials-api/build/docs/openapi/verifiable-credentials-api.yaml \
   resources/openapi/yaml/identity-api/

# Issuer Admin API
cp extensions/api/issuer-admin-api/attestation-api/build/docs/openapi/attestation-api.yaml \
   resources/openapi/yaml/issuer-admin-api/
cp extensions/api/issuer-admin-api/credential-definition-api/build/docs/openapi/credential-definition-api.yaml \
   resources/openapi/yaml/issuer-admin-api/
cp extensions/api/issuer-admin-api/credentials-api/build/docs/openapi/credentials-api.yaml \
   resources/openapi/yaml/issuer-admin-api/
cp extensions/api/issuer-admin-api/holder-api/build/docs/openapi/holder-api.yaml \
   resources/openapi/yaml/issuer-admin-api/
cp extensions/api/issuer-admin-api/issuance-process-api/build/docs/openapi/issuance-process-api.yaml \
   resources/openapi/yaml/issuer-admin-api/

# Credentials API (DCP Identity Hub)
cp protocols/dcp/dcp-identityhub/credential-offer-api/build/docs/openapi/credential-offer-api.yaml \
   resources/openapi/yaml/credentials-api/
cp protocols/dcp/dcp-identityhub/credentials-api-configuration/build/docs/openapi/credentials-api-configuration.yaml \
   resources/openapi/yaml/credentials-api/
cp protocols/dcp/dcp-identityhub/presentation-api/build/docs/openapi/presentation-api.yaml \
   resources/openapi/yaml/credentials-api/
cp protocols/dcp/dcp-identityhub/storage-api/build/docs/openapi/storage-api.yaml \
   resources/openapi/yaml/credentials-api/

# Issuer API (DCP Issuer)
cp protocols/dcp/dcp-issuer/dcp-issuer-api/build/docs/openapi/dcp-issuer-api.yaml \
   resources/openapi/yaml/issuer-api/

echo "✓ All OpenAPI specifications copied successfully!"
echo "Total files: $(find resources/openapi/yaml -name "*.yaml" | wc -l)"
```

Make it executable:
```bash
chmod +x scripts/copy-openapi-specs.sh
```

### Option 2: One-Liner Commands

```bash
# Create directories
mkdir -p resources/openapi/yaml/{sts-api,identity-api,issuer-admin-api,issuer-api,credentials-api}

# Copy all files
find extensions protocols -path "*/build/docs/openapi/*.yaml" -exec sh -c 'api_group=$(echo {} | grep -oE "(sts-api|identity-api|issuer-admin-api|credentials-api)" | head -1); [ -z "$api_group" ] && api_group="issuer-api"; cp {} resources/openapi/yaml/$api_group/$(basename {})' \;
```

## Complete Workflow

### Full regeneration process:

```bash
# 1. Generate OpenAPI specifications
./gradlew openapi

# 2. Copy to resources directory
./scripts/copy-openapi-specs.sh

# 3. Verify generation
find resources/openapi/yaml -name "*.yaml" | wc -l  # Should output: 15

# 4. Check differences (optional)
git status resources/openapi/yaml/

# 5. Commit if needed
git add resources/openapi/yaml/
git commit -m "docs: update OpenAPI specifications"
```

## Automated Publishing

The GitHub workflow `.github/workflows/publish-openapi-ui.yml` automatically publishes these specifications to GitHub Pages when code is pushed to the `main` branch.

**Published URL**: https://eclipse-edc.github.io/IdentityHub/openapi/

The workflow:
- Triggers on push to `main` branch or manual dispatch
- Uses the reusable workflow from `eclipse-edc/.github`
- Publishes the OpenAPI UI with all 15 specifications

## Technical Details

### Gradle Configuration

The root `build.gradle.kts` imports the merge task:

```kotlin
import org.eclipse.edc.plugins.edcbuild.plugins.MergeOpenApiSpecTask

tasks.withType(MergeOpenApiSpecTask::class.java) {
    skipOperationExample.set(true)
}
```

### Dependencies

From `gradle/libs.versions.toml`:
- **Swagger Plugin Version**: 2.2.40
- **EDC Build Plugin**: org.eclipse.edc.edc-build:1.1.5

### Individual Task Paths

You can generate a specific API's specification:

```bash
# Example: Generate only the STS API spec
./gradlew :extensions:sts:sts-api:openapi

# Example: Generate only Identity API specs
./gradlew :extensions:api:identity-api:did-api:openapi
./gradlew :extensions:api:identity-api:keypair-api:openapi
```

## Troubleshooting

### Issue: No YAML files generated
**Solution**: Ensure all dependencies are compiled first:
```bash
./gradlew clean build openapi
```

### Issue: Old specifications not updating
**Solution**: Clean build directories:
```bash
./gradlew clean
./gradlew openapi
```

### Issue: Missing build/docs/openapi directory
**Solution**: The directory is only created when the openapi task runs successfully. Check for compilation errors:
```bash
./gradlew :module-path:compileJava
./gradlew :module-path:openapi
```

## Related Documentation

- [EDC Build Plugin Documentation](https://github.com/eclipse-edc/GradlePlugins)
- [Swagger Gradle Plugin](https://github.com/swagger-api/swagger-core/tree/master/modules/swagger-gradle-plugin)
- [IdentityHub API Documentation](docs/developer/architecture/identityhub-apis.md)
