# Cloud Foundry API Story Writing

**Category:** Domain skill - System-specific

Patterns and conventions for writing user stories about Cloud Foundry API (CAPI) endpoints.

## Prerequisites

- [Generic Story Writing](../generic-story-writing.md) - Core story structure
- [Jira Story Writing](../jira-story-writing.md) - Jira markup (if using Jira)

## Overview

This skill provides comprehensive guidance for writing Cloud Foundry API (CAPI) stories, including:

1. **Story Structure** - How to write CAPI stories with proper acceptance criteria
2. **CAPI Error Format** - Specific error response structure
3. **CAPI Pagination** - Specific pagination format with first/last/next/previous
4. **CAPI Permissions** - CF-specific roles and permissions
5. **CAPI Style Guide** - Reference to authoritative source for API conventions

## API Story Principles

1. **Focus on HTTP interface** - Stories describe request/response behavior
2. **Include status codes** - Be explicit about expected status codes (refer to style guide)
3. **Cover standard CRUD** - Create, Read, Update, Delete patterns
4. **Document permissions** - Who can call which endpoints
5. **Show request/response examples** - Make the API concrete
6. **Follow CAPI style guide** - Use as authoritative source for API conventions

## HTTP Status Codes

For complete details on HTTP status codes used by CAPI, refer to:
- **[CAPI v3 Style Guide](https://github.com/cloudfoundry/cloud_controller_ng/blob/main/docs/v3_style_guide.md)** - Authoritative source for status code usage
- **[CAPI v3 API Docs](https://v3-apidocs.cloudfoundry.org/)** - Status codes section

When writing stories, be explicit about expected status codes in acceptance criteria. Common patterns:
- Success: 200 OK, 201 Created, 202 Accepted, 204 No Content
- Client errors: 400, 401, 403, 404, 422
- Server errors: 500, 503

## Common API Story Types

### Create (POST) Stories

#### Essential Scenarios
- **Base Case**: Successful creation with 201 Created
- **Invalid Input**: Validation errors with 422 Unprocessable Entity
- **Already Exists**: Idempotency handling (check style guide for appropriate status code)
- **Permissions**: Who can create (200/201 vs 403 Forbidden)
- **Missing Required Fields**: 422 with descriptive error

#### Example Structure
```
Given logged in with sufficient permissions
And [any required parent resources exist]
When POST to /v3/resources with valid data
Then the request succeeds with status code 201
And the response includes the created resource
And the Location header points to the new resource
```

### Read/View (GET) Stories

#### Essential Scenarios
- **Base Case**: Successful retrieval with 200 OK
- **Not Found**: 404 Not Found when resource doesn't exist
- **Permissions**: Who can view (200 vs 403 vs 404)
- **Include Related Resources**: Optional query parameters for relationships

#### Example Structure
```
Given logged in with sufficient permissions
And a resource exists
When GET /v3/resources/{guid}
Then the request succeeds with status code 200
And the response includes the resource details
```

### Update (PATCH/PUT) Stories

#### Essential Scenarios
- **Base Case**: Successful update with 200 OK
- **Not Found**: 404 when resource doesn't exist
- **Invalid Input**: 422 for validation errors
- **Permissions**: Who can update (200 vs 403 vs 404)
- **Partial Update**: PATCH for partial, PUT for full replacement

#### Example Structure
```
Given logged in with sufficient permissions
And a resource exists
When PATCH /v3/resources/{guid} with valid changes
Then the request succeeds with status code 200
And the response includes the updated resource
```

### Delete (DELETE) Stories

#### Essential Scenarios
- **Base Case**: Successful deletion with 204 No Content or 202 Accepted
- **Not Found**: 404 when resource doesn't exist
- **Permissions**: Who can delete (204 vs 403 vs 404)
- **Async Deletion**: 202 with job tracking (CAPI pattern)
- **Cascading Deletes**: What else gets deleted

#### Example Structure
```
Given logged in with sufficient permissions
And a resource exists
When DELETE /v3/resources/{guid}
Then the request succeeds with status code 202
And the response includes a job resource
And [any cascading deletes occur]
```

### List (GET Collection) Stories

#### Essential Scenarios
- **Base Case**: Successful list with 200 OK and pagination
- **Empty Results**: 200 with empty results array
- **Pagination**: Multiple pages with next/prev links
- **Filtering**: Query parameters for filtering results
- **Permissions**: Who can list (200 vs 401)
- **Visibility**: What each role can see

#### Example Structure
```
Given logged in with sufficient permissions
And multiple resources exist
When GET /v3/resources
Then the request succeeds with status code 200
And the response includes a paginated list
And pagination links are included
```

## Cloud Foundry Specifics

1. **CAPI Error Format** - Specific error response structure
2. **CAPI Pagination** - Specific pagination format with first/last/next/previous
3. **CAPI Permissions** - CF-specific roles and permissions
4. **CAPI Style Guide** - Follow v3 style guide conventions

## CAPI Error Response Format

Cloud Foundry API uses a specific error format:

```json
{
  "errors": [
    {
      "code": 10008,
      "title": "CF-UnprocessableEntity",
      "detail": "Name must be unique"
    }
  ]
}
```

**Key elements:**
- `errors` array (can contain multiple errors)
- `code` - Numeric error code
- `title` - Error type with `CF-` prefix
- `detail` - Human-readable error message

**Common error titles:**
- `CF-UnprocessableEntity` (422)
- `CF-ResourceNotFound` (404)
- `CF-NotAuthenticated` (401)
- `CF-NotAuthorized` (403)
- `CF-InvalidRequest` (400)

## CAPI Pagination Format

```json
{
  "pagination": {
    "total_results": 100,
    "total_pages": 10,
    "first": { "href": "/v3/resources?page=1" },
    "last": { "href": "/v3/resources?page=10" },
    "next": { "href": "/v3/resources?page=2" },
    "previous": null
  },
  "resources": [
    { "...": "..." }
  ]
}
```

**Key elements:**
- `pagination` object with total counts and links
- `first`, `last`, `next`, `previous` links
- `resources` array (not `items` or `data`)
- Links include query parameters (filters, includes, etc.)

## CAPI Permissions

### Standard Cloud Foundry Roles

Include these roles in permissions tables (in this order):

1. **Admin** - Global admin, full access
2. **SpaceDeveloper** - Can manage apps/services in space
3. **Read-Only Admin** - Global read access
4. **Global Auditor** - Global read access for auditing
5. **SpaceManager** - Can manage space (not apps)
6. **SpaceAuditor** - Read access to space
7. **SpaceSupporter** - [Beta role] Manage app lifecycle and service bindings
8. **OrgManager** - Can manage organization
9. **OrgAuditor** - Read access to organization
10. **OrgBillingManager** - Manage billing for organization
11. **Logged out user** - Unauthenticated

### Permissions Table Format

```
||*Role*||*Status*||*Notes*||
|Admin|200|Full access|
|SpaceDeveloper|200|Can modify resources in their spaces|
|Read-Only Admin|200|Read-only access|
|Global Auditor|200|Read-only access for auditing|
|SpaceManager|403|Cannot modify resources|
|SpaceAuditor|403|Read-only access|
|SpaceSupporter|200|Can manage app lifecycle|
|OrgManager|200|Can manage org resources|
|OrgAuditor|403|Read-only access|
|OrgBillingManager|403|Billing only|
|Logged out user|401|Must authenticate|
```

**Notes:**
- Use `|*value*|` to bold changed rows (what's new in this story)
- Add `{quote}` blocks below table for clarifications
- Reference v3 style guide for 404 vs 403 behavior

### 404 vs 403 Pattern

Per CAPI v3 style guide:
- Return **404** instead of **403** when user lacks permissions to avoid information leakage
- Exception: Some endpoints return 403 for clarity

```
||*Role*||*Status*||*Notes*||
|Admin|200|Can see all resources|
|SpaceDeveloper|200|Can see resources in their spaces|
|Other User|404|Returns 404 to hide existence|
|Logged out user|401|Must authenticate|
```

## CAPI-Specific Story Patterns

### List Endpoints

**Separate Permissions from Visibilities:**

**Permissions table** - Who can call the endpoint:
```
||*Role*||*Status*||
|logged-in user|200|
|Logged-out user|401|
```

**Visibilities table** - What each role can see:
```
||*Role*||*Visibile*||
|Admin|yes|
|Read-Only Admin|yes|
|Global Auditor|yes|
|SpaceDeveloper|no|
and so on...
```

### Filter Endpoints

- List available filters: `guids`, `names`, `origins`, `created_ats`, `updated_ats`, and so on...
- Reference v3 style guide for filtering behavior
- Note that filters appear in pagination links
- Can be lightweight if straightforward

### Include Parameter

For extending `include` query parameter:
- Focus only on new include value
- Don't re-test existing include behavior
- Show de-duplication across paginated results
- Use `"...": "..."` to focus on relevant parts
- Note that include param appears in pagination links

### Metadata Stories

Cover these scenarios:
1. Create with metadata
2. Add to existing resource
3. Invalid metadata
4. Update metadata
5. Remove metadata (use `null`)

**Notes:**
- Include permissions table
- Metadata deleted when parent resource deleted
- PATCH endpoint may be ONLY for metadata if core fields immutable

### Delete Stories

Use async job pattern:
- 202 Accepted with Location header
- Job resource with states: PROCESSING, COMPLETE, FAILED
- Include job examples
- Cover 404 case
- Note cascading deletes

**Example:**
```json
{
  "id": "job-123",
  "state": "PROCESSING",
  "operation": "resource.delete",
  "links": {
    "self": "/v3/jobs/job-123"
  }
}
```

## CAPI Style Guide References

### Key Conventions

- RESTful resource naming (plural nouns)
- Consistent error response format
- Standard pagination structure
- HATEOAS links for discoverability
- Idempotent operations where appropriate

### Style Guide Link

Reference in stories:
```
* Conform to the v3 style guide: https://github.com/cloudfoundry/cloud_controller_ng/blob/main/docs/v3_style_guide.md
```

## CAPI Style Guide References

### For CAPI Stories

Include in boilerplate:

```
h4. Boilerplate
* Acceptance criteria are negotiable. If something can be accomplished in an easier way or there is a better option, let's chat!
* Reference any proposal documents, RFCs, etc for guidance
* Conform to the v3 style guide: https://github.com/cloudfoundry/cloud_controller_ng/blob/main/docs/v3_style_guide.md
* Lean on CAPI maintainers for guidance
```

## Example CAPI Story Structure

```
h1. Add Labels to Apps

*Status:* In Progress
*Type:* Story
*Epic:* Metadata

h2. Summary

Add ability to set labels on apps via CAPI v3 API

h2. Description

*Given* CAPI v3 API
*When* managing app metadata
*Then* provide ability to add, update, and remove labels on apps

----

*Acceptance Criteria*

h3. Base Case: PATCH /v3/apps/:guid

*Given* logged in as SpaceDeveloper
*And* an app exists in my space
*When* PATCH to {{/v3/apps/{guid}}} with label metadata
*Then* the request succeeds with status code {{200}}
*And* the response includes the updated app with labels
*And* labels are included in the metadata section

{code:json}
{
  "guid": "app-guid",
  "name": "my-app",
  "state": "STARTED",
  "created_at": "2024-01-15T10:30:00Z",
  "updated_at": "2024-01-15T10:35:00Z",
  "metadata": {
    "labels": {
      "environment": "production",
      "team": "platform"
    }
  },
  "links": {
    "self": { "href": "/v3/apps/app-guid" }
  }
}
{code}

h3. Invalid Input

*Given* logged in as SpaceDeveloper
*And* an app exists in my space
*When* PATCH to {{/v3/apps/{guid}}} with invalid label key
*Then* the request fails with status code {{422}}
*And* the response includes CF-UnprocessableEntity error

{code:json}
{
  "errors": [
    {
      "code": 10008,
      "title": "CF-UnprocessableEntity",
      "detail": "Label key must be a valid format"
    }
  ]
}
{code}

h3. Permissions

||*Role*||*Status*||*Notes*||
|SpaceDeveloper|200|Can update apps in their space|
|SpaceAuditor|403|Read-only access|
|OrgManager|403|No space-level access|
|Logged out user|401|Must authenticate|

----

h3. Notes

*Implementation Guidance:*
* Follow v3 style guide for endpoint structure
* Use standard CAPI error format
* Include HATEOAS links
* Labels must follow metadata key/value format

h4. Boilerplate
* Conform to the v3 style guide
* Follow CAPI best practices

h3. Links
* *Prior Art:* https://v3-apidocs.cloudfoundry.org/version/3.210.0/index.html#labels-and-selectors

h2. Labels

api, apps, metadata, capi

h2. Notes

[Empty]
```

## Request/Response Patterns

### Request Fields Table
```
||*field*||*required*||*default*||*validations*||
|name|true|none|string, max 255 chars|
|description|false|empty string|string, max 1000 chars|
|metadata|false|empty|valid CAPI metadata|
```

**Include**:
- Field name
- Whether required
- Default value
- Validation rules
- Data type

### Keep Examples Minimal
- Use `"...": "..."` for non-relevant fields
- Focus on fields relevant to the story
- Show structure, not exhaustive data
- Include enough context to understand

## Query Parameters

### Common Patterns
- **Filtering**: `?names=value1,value2&origins=ldap`
- **Sorting**: `?order_by=created_at`
- **Pagination**: `?page=2&per_page=50`
- **Including**: `?include=related_resource`

### Documentation Format
```
||*Parameter*||*Type*||*Description*||
|names|string|Filter by name (comma-separated)|
|origins|string|Filter by origin (comma-separated)|
|guids|string|Filter by GUID (comma-separated)|
|created_ats|timestamp|Filter by creation date|
|updated_ats|timestamp|Filter by update date|
|page|integer|Page number (default: 1)|
|per_page|integer|Results per page (default: 50, max: 5000)|
|include|string|Include related resources (comma-separated)|
```

## API Documentation Scenarios

### Include in Stories
```
Scenario: API Documentation

Given the API documentation
When viewing the /v3/resources endpoint documentation
Then it includes:
- Endpoint path and HTTP method
- Request body schema
- Response body schema
- Status codes and their meanings
- Example requests and responses
- Required permissions
- Query parameters (if applicable)
```

## Versioning Considerations

CAPI uses `/v3/` in URL path:
- `/v3/resources`
- `/v3/apps`
- `/v3/spaces`

## Integration with Other Skills

Use this skill with:
- **[Generic Story Writing](../generic-story-writing.md)** - Core structure and principles
- **[Jira Story Writing](../jira-story-writing.md)** - Jira markup and formatting

This skill provides comprehensive Cloud Foundry API story writing guidance.
