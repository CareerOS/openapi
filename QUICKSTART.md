# OpenAPI Documentation - Quick Start

## Build and Preview the Documentation

### 1. Install Dependencies
```bash
cd openapi
npm install
```

### 2. Preview Locally
```bash
npm start
```
This starts a local server at `http://localhost:8080` with live reload.

### 3. Lint the Specification
```bash
npm run lint
```
Fix any errors before committing.

### 4. Build for Production
```bash
npm run build
```
Outputs bundled files to `dist/` directory.

## Quick Reference

### Main Files
- `openapi/openapi.yaml` - Main specification file
- `paths/` - Individual endpoint definitions
- `components/` - Reusable schemas, responses, parameters

### Adding a New Endpoint

1. **Create path file**: `paths/your-category/endpoint.yaml`
2. **Add to main spec**: Reference in `openapi/openapi.yaml`
3. **Test**: Run `npm run lint` and `npm start`

### Example Path File
```yaml
get:
  tags:
    - YourTag
  summary: Get resource
  operationId: getResource
  security:
    - BearerAuth: []
  responses:
    '200':
      description: Success
    '401':
      $ref: ../../components/responses/UnauthorizedError.yaml
```

## CI/CD Integration

The documentation is built automatically when changes are pushed to the repository. The Redocly build job:

1. Lints the specification
2. Bundles all files
3. Generates static HTML documentation
4. Deploys to the docs server

## Common Issues

### Linting Errors
```bash
npm run lint
```
Fix validation errors before committing.

### Broken References
Ensure all `$ref` paths are correct relative to the file location.

### Missing Components
Add referenced schemas/responses to `components/` directory.

## Resources

- [OpenAPI 3.1 Spec](https://spec.openapis.org/oas/v3.1.0)
- [Redocly Docs](https://redocly.com/docs/)
- [Full README](./README.md)
