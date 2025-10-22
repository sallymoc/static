# Products Directory

This directory contains product-specific data and assets that are deployed separately from the general shared data.

## Structure

```
products/
└── {product-name}/
    ├── data/              # Simple, flat configuration files
    │   ├── *.json        # JSON configs (auto-minified during build)
    │   ├── *.xml         # XML configs (copied as-is)
    │   └── ...
    └── {feature-name}/    # Complex features with multiple related files
        ├── *.json
        └── subfolder/
            └── ...
```

## Current Products

### wallet-app
Mobile wallet application data including:
- `/data/` - App settings and configuration files
- `/dapps/` - DApp explorer data and localization files

## Adding a New Product

1. Create a folder: `products/{product-name}/`
2. Add your data files (JSON, XML, or any format)
3. Organize complex features into subfolders
4. Build with: `python3 scripts/build_dist.py --product {product-name} --version v1.0.0`

## Build & Deployment

Build a specific product:
```bash
python3 scripts/build_dist.py --product wallet-app --version v1.2.3
```

Build all products:
```bash
python3 scripts/build_dist.py --product all --version v1.2.3
```

Each product is deployed to:
- Production: `static.qubic.org/v1/{product-name}/`
- Staging: `static.qubic.org/staging/v1/{product-name}/`
- Dev: `static.qubic.org/dev/v1/{product-name}/`

## Guidelines

**Use `/data/` for:**
- Single configuration files
- Simple data that doesn't need subfolders
- Standalone resources

**Create feature folders for:**
- Multiple related files (e.g., data + translations)
- Features with their own structure
- Logical units that should be grouped together

## Version Tracking

The repository uses a single API version (v1) that applies to all products. Each product gets its own `version.json` file with:
- Same version number across all products (monorepo versioning)
- Individual file hashes for cache invalidation
- File sizes for all resources

Example: `https://static.qubic.org/v1/wallet-app/version.json`
```json
{
  "version": "v1.2.3",
  "environment": "production",
  "generated_at": "2025-10-16T22:00:00Z",
  "files": {
    "data/settings.json": {
      "hash": "sha256:abc123...",
      "size": 1234
    },
    "data/config.xml": {
      "hash": "sha256:def456...",
      "size": 567
    }
  }
}
```

**Important:** The `v1` in the URL path represents the API version, not the product version. All products share the same API version and deploy under the same v1 namespace.
