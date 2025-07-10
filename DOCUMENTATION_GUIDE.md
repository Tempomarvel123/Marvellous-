# Documentation Guide for Marvellous Project

This guide provides detailed instructions and best practices for documenting code in the Marvellous project.

## Table of Contents

1. [Documentation Philosophy](#documentation-philosophy)
2. [API Documentation](#api-documentation)
3. [Function Documentation](#function-documentation)
4. [Component Documentation](#component-documentation)
5. [Code Comments](#code-comments)
6. [Testing Documentation](#testing-documentation)
7. [Examples and Tutorials](#examples-and-tutorials)
8. [Documentation Tools](#documentation-tools)

---

## Documentation Philosophy

### Core Principles

1. **User-First**: Write documentation from the user's perspective
2. **Example-Driven**: Include working examples for everything
3. **Complete**: Document all public interfaces
4. **Accurate**: Keep documentation in sync with code
5. **Accessible**: Use clear, simple language

### Documentation Types

- **API Reference**: Technical details for developers
- **Tutorials**: Step-by-step learning guides
- **How-to Guides**: Solution-oriented instructions
- **Explanations**: Understanding concepts and design decisions

---

## API Documentation

### REST API Documentation

```markdown
### Create User

Creates a new user account in the system.

**Endpoint**: `POST /api/v1/users`

**Authentication**: Required (Bearer Token)

**Request Headers**:
```http
Content-Type: application/json
Authorization: Bearer <token>
```

**Request Body**:
```json
{
  "username": "johndoe",
  "email": "john@example.com",
  "fullName": "John Doe",
  "role": "user"
}
```

**Parameters**:
- `username` (string, required): Unique username (3-50 characters)
- `email` (string, required): Valid email address
- `fullName` (string, required): User's full name
- `role` (string, optional): User role, defaults to "user"

**Success Response** (201 Created):
```json
{
  "id": "usr_123456789",
  "username": "johndoe",
  "email": "john@example.com",
  "fullName": "John Doe",
  "role": "user",
  "createdAt": "2023-12-01T10:30:00Z",
  "updatedAt": "2023-12-01T10:30:00Z"
}
```

**Error Responses**:

- **400 Bad Request**: Invalid input data
```json
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Invalid input data",
    "details": {
      "username": "Username must be between 3-50 characters",
      "email": "Must be a valid email address"
    }
  }
}
```

- **409 Conflict**: Username or email already exists
```json
{
  "error": {
    "code": "USER_EXISTS",
    "message": "User with this username or email already exists"
  }
}
```

**Rate Limits**: 100 requests per hour per IP

**Example Usage**:
```bash
curl -X POST "https://api.marvellous.com/v1/users" \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer YOUR_TOKEN" \
  -d '{
    "username": "johndoe",
    "email": "john@example.com",
    "fullName": "John Doe"
  }'
```

**SDK Examples**:

JavaScript:
```javascript
const user = await marvellous.users.create({
  username: 'johndoe',
  email: 'john@example.com',
  fullName: 'John Doe'
});
```

Python:
```python
user = marvellous.users.create(
    username='johndoe',
    email='john@example.com',
    full_name='John Doe'
)
```
```

---

## Function Documentation

### JavaScript/TypeScript Functions

```typescript
/**
 * Calculates the distance between two geographical points using the Haversine formula.
 * 
 * @param point1 - The first geographical point
 * @param point1.lat - Latitude of the first point (-90 to 90)
 * @param point1.lng - Longitude of the first point (-180 to 180)
 * @param point2 - The second geographical point  
 * @param point2.lat - Latitude of the second point (-90 to 90)
 * @param point2.lng - Longitude of the second point (-180 to 180)
 * @param unit - Unit for the result ('km' for kilometers, 'mi' for miles)
 * @returns The distance between the two points in the specified unit
 * 
 * @throws {Error} When coordinates are invalid (outside valid ranges)
 * @throws {TypeError} When required parameters are missing or wrong type
 * 
 * @example
 * ```typescript
 * // Calculate distance between New York and London
 * const distance = calculateDistance(
 *   { lat: 40.7128, lng: -74.0060 }, // New York
 *   { lat: 51.5074, lng: -0.1278 },  // London
 *   'km'
 * );
 * console.log(distance); // 5585.8 (kilometers)
 * ```
 * 
 * @example
 * ```typescript
 * // Using with miles
 * const distanceMiles = calculateDistance(
 *   { lat: 34.0522, lng: -118.2437 }, // Los Angeles
 *   { lat: 37.7749, lng: -122.4194 }, // San Francisco
 *   'mi'
 * );
 * console.log(distanceMiles); // 347.2 (miles)
 * ```
 * 
 * @since 1.0.0
 */
export function calculateDistance(
  point1: { lat: number; lng: number },
  point2: { lat: number; lng: number },
  unit: 'km' | 'mi' = 'km'
): number {
  // Implementation here...
}
```

### Documentation in Markdown

```markdown
### calculateDistance()

Calculates the distance between two geographical points using the Haversine formula.

**Syntax**:
```typescript
calculateDistance(point1, point2, unit?)
```

**Parameters**:
- `point1` (object): First geographical point
  - `lat` (number): Latitude (-90 to 90)
  - `lng` (number): Longitude (-180 to 180)
- `point2` (object): Second geographical point
  - `lat` (number): Latitude (-90 to 90) 
  - `lng` (number): Longitude (-180 to 180)
- `unit` (string, optional): Unit for result. Default: `'km'`
  - `'km'`: Kilometers
  - `'mi'`: Miles

**Returns**: `number` - Distance between points in specified unit

**Throws**:
- `Error`: When coordinates are invalid
- `TypeError`: When parameters are missing or wrong type

**Examples**:

Basic usage:
```typescript
const distance = calculateDistance(
  { lat: 40.7128, lng: -74.0060 }, // New York
  { lat: 51.5074, lng: -0.1278 },  // London
  'km'
);
// Returns: 5585.8
```

With miles:
```typescript
const distance = calculateDistance(
  { lat: 34.0522, lng: -118.2437 }, // Los Angeles  
  { lat: 37.7749, lng: -122.4194 }, // San Francisco
  'mi'
);
// Returns: 347.2
```

Error handling:
```typescript
try {
  const distance = calculateDistance(
    { lat: 200, lng: -74.0060 }, // Invalid latitude
    { lat: 51.5074, lng: -0.1278 },
    'km'
  );
} catch (error) {
  console.error('Invalid coordinates:', error.message);
}
```
```

---

## Component Documentation

### React Component Documentation

```markdown
### UserCard

A reusable card component for displaying user information with avatar, name, and actions.

**Props**:

| Prop | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| `user` | `User` | Yes | - | User object containing user data |
| `showActions` | `boolean` | No | `true` | Whether to show action buttons |
| `onEdit` | `function` | No | - | Callback when edit button is clicked |
| `onDelete` | `function` | No | - | Callback when delete button is clicked |
| `className` | `string` | No | - | Additional CSS classes |
| `size` | `'sm' \| 'md' \| 'lg'` | No | `'md'` | Size variant of the card |

**User Type**:
```typescript
interface User {
  id: string;
  username: string;
  email: string;
  fullName: string;
  avatar?: string;
  role: 'admin' | 'user' | 'moderator';
  isActive: boolean;
}
```

**Events**:
- `onEdit(user: User)`: Fired when edit button is clicked
- `onDelete(userId: string)`: Fired when delete button is clicked

**Styling**:

CSS Classes:
- `.user-card`: Main container
- `.user-card--sm`: Small size variant
- `.user-card--md`: Medium size variant (default)
- `.user-card--lg`: Large size variant
- `.user-card__avatar`: Avatar container
- `.user-card__info`: User info section
- `.user-card__actions`: Action buttons section

CSS Custom Properties:
```css
.user-card {
  --user-card-bg: #ffffff;
  --user-card-border: #e2e8f0;
  --user-card-shadow: 0 1px 3px rgba(0, 0, 0, 0.1);
  --user-card-radius: 8px;
}
```

**Usage Examples**:

Basic usage:
```jsx
import { UserCard } from '@marvellous/components';

const user = {
  id: '1',
  username: 'johndoe',
  email: 'john@example.com',
  fullName: 'John Doe',
  avatar: 'https://example.com/avatar.jpg',
  role: 'user',
  isActive: true
};

<UserCard user={user} />
```

With actions:
```jsx
<UserCard
  user={user}
  showActions={true}
  onEdit={(user) => console.log('Edit user:', user)}
  onDelete={(userId) => console.log('Delete user:', userId)}
/>
```

Custom styling:
```jsx
<UserCard
  user={user}
  size="lg"
  className="my-custom-card"
/>
```

Without actions:
```jsx
<UserCard
  user={user}
  showActions={false}
/>
```

**Accessibility**:
- Supports keyboard navigation
- ARIA labels for screen readers
- High contrast mode compatible
- Focus indicators on interactive elements

**Browser Support**:
- Chrome 88+
- Firefox 85+
- Safari 14+
- Edge 88+
```

---

## Code Comments

### Best Practices

1. **Explain Why, Not What**: Code should be self-explanatory for "what"
2. **Document Complex Logic**: Explain algorithms and business rules
3. **Update Comments**: Keep comments in sync with code changes
4. **Use Consistent Style**: Follow team conventions

### Examples

Good comments:
```typescript
// Use exponential backoff to handle rate limiting gracefully
const delay = Math.min(1000 * Math.pow(2, retryCount), 30000);

/**
 * We cache the result for 5 minutes because the external API
 * has rate limits and the data doesn't change frequently
 */
const CACHE_TTL = 5 * 60 * 1000;

// TODO: Implement pagination when dataset grows beyond 1000 items
// See issue #123 for requirements
```

Avoid these comments:
```typescript
// Bad: Explains what the code obviously does
let i = 0; // Initialize i to 0

// Bad: Outdated comment
// Returns user data (actually returns user ID only now)
function getUserData() {
  return user.id;
}
```

---

## Testing Documentation

### Test Documentation Format

```typescript
describe('calculateDistance', () => {
  describe('when given valid coordinates', () => {
    it('should calculate distance between New York and London in kilometers', () => {
      // Arrange
      const nyc = { lat: 40.7128, lng: -74.0060 };
      const london = { lat: 51.5074, lng: -0.1278 };
      
      // Act
      const result = calculateDistance(nyc, london, 'km');
      
      // Assert
      expect(result).toBeCloseTo(5585.8, 1);
    });

    it('should calculate distance in miles when specified', () => {
      const la = { lat: 34.0522, lng: -118.2437 };
      const sf = { lat: 37.7749, lng: -122.4194 };
      
      const result = calculateDistance(la, sf, 'mi');
      
      expect(result).toBeCloseTo(347.2, 1);
    });
  });

  describe('when given invalid coordinates', () => {
    it('should throw error for latitude outside valid range', () => {
      const invalidPoint = { lat: 200, lng: -74.0060 };
      const validPoint = { lat: 51.5074, lng: -0.1278 };
      
      expect(() => {
        calculateDistance(invalidPoint, validPoint, 'km');
      }).toThrow('Latitude must be between -90 and 90 degrees');
    });
  });
});
```

---

## Examples and Tutorials

### Tutorial Structure

1. **Learning Objective**: What the user will accomplish
2. **Prerequisites**: Required knowledge/setup
3. **Step-by-Step Instructions**: Clear, numbered steps
4. **Code Examples**: Working, copy-pasteable code
5. **Verification**: How to check if it worked
6. **Next Steps**: What to learn next

### Example Tutorial

```markdown
# Getting Started with Marvellous API

## What You'll Learn

In this tutorial, you'll learn how to:
- Set up authentication with the Marvellous API
- Create your first user
- Fetch user data
- Handle errors properly

## Prerequisites

- Node.js 16+ installed
- Basic knowledge of JavaScript/TypeScript
- API key from Marvellous dashboard

## Step 1: Install the SDK

```bash
npm install @marvellous/sdk
```

## Step 2: Initialize the Client

Create a new file `example.js`:

```javascript
const { Marvellous } = require('@marvellous/sdk');

const client = new Marvellous({
  apiKey: 'your-api-key-here',
  environment: 'sandbox' // Use 'production' for live data
});
```

## Step 3: Create a User

```javascript
async function createUser() {
  try {
    const user = await client.users.create({
      username: 'johndoe',
      email: 'john@example.com',
      fullName: 'John Doe'
    });
    
    console.log('User created:', user);
    return user;
  } catch (error) {
    console.error('Error creating user:', error.message);
  }
}
```

## Step 4: Run Your Code

```bash
node example.js
```

You should see output like:
```
User created: {
  id: 'usr_123456789',
  username: 'johndoe',
  email: 'john@example.com',
  fullName: 'John Doe',
  ...
}
```

## Next Steps

- [User Management Guide](./user-management.md)
- [Authentication Best Practices](./auth-guide.md)
- [Error Handling Patterns](./error-handling.md)
```

---

## Documentation Tools

### Recommended Tools

1. **JSDoc**: For JavaScript/TypeScript inline documentation
2. **Sphinx**: For Python projects
3. **Swagger/OpenAPI**: For REST API documentation
4. **Storybook**: For component documentation
5. **GitBook/Notion**: For user-facing documentation

### Automation

1. **Generate from Code**: Use tools to auto-generate API docs
2. **Link Validation**: Check that all links work
3. **Example Testing**: Ensure code examples actually work
4. **Spell Check**: Use tools like vale or grammarly

### Documentation Workflow

1. **Write docs alongside code** in the same PR
2. **Review documentation** as part of code review
3. **Test examples** before merging
4. **Deploy documentation** automatically
5. **Monitor usage** and update based on feedback

---

## Checklist for Good Documentation

### For APIs
- [ ] Clear description of what it does
- [ ] All parameters documented with types
- [ ] Request/response examples
- [ ] Error codes and messages
- [ ] Authentication requirements
- [ ] Rate limiting information
- [ ] Working code examples

### For Functions
- [ ] Purpose and behavior described
- [ ] All parameters with types and descriptions
- [ ] Return value documented
- [ ] Exceptions/errors listed
- [ ] At least one working example
- [ ] Edge cases mentioned

### For Components
- [ ] Props/parameters documented
- [ ] Usage examples provided
- [ ] Events/callbacks listed
- [ ] Styling options explained
- [ ] Accessibility notes included
- [ ] Browser compatibility mentioned

---

*This guide should be updated as the project grows and new documentation patterns emerge.*