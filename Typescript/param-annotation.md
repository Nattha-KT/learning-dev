The `@param` annotation is part of **JSDoc**, a standard for documenting JavaScript (and TypeScript) code. It provides an inline way to describe the purpose of function parameters, making your code more understandable and easier to maintain. Here's why it's useful:

---

### **Why Use `@param`?**

1. **Improves Code Readability**:
   - Developers can quickly understand what each parameter does without diving into the implementation.

2. **Supports IDE Intellisense**:
   - Modern IDEs like VSCode use JSDoc comments to provide tooltips and autocomplete suggestions, improving developer productivity.

3. **Documents API Behavior**:
   - When writing public APIs or libraries, `@param` ensures that users know how to use the function properly.

4. **Facilitates TypeScript Integration**:
   - Although TypeScript already provides type annotations, `@param` can be used for additional descriptions (e.g., "Pass the ID as a UUID string").

---

### **Example of `@param` Usage**

**JSDoc Example:**
```typescript
/**
 * Fetch role by ID
 * @param id - The unique identifier of the role (UUID format)
 * @returns The role object
 */
export const fetchRoleById = async (id: string): Promise<Role> => {
  // Implementation here
};
```

**Benefits in an IDE:**
- When a developer hovers over the function or parameter, the description "The unique identifier of the role (UUID format)" appears as a tooltip.
- Makes it clear that `id` must be a UUID.

---

### **Why Use It Alongside TypeScript?**
While TypeScript enforces **types**, it doesn’t describe **purpose** or **behavior**. For example:

**TypeScript Alone:**
```typescript
export const fetchRoleById = async (id: string): Promise<Role> => {};
```
- It tells you `id` is a string but doesn’t explain what that string represents (e.g., "ID of the role").

**With JSDoc:**
```typescript
/**
 * Fetch role by ID
 * @param id - The unique identifier of the role
 * @returns The role object
 */
export const fetchRoleById = async (id: string): Promise<Role> => {};
```
- Provides additional context and makes the purpose of `id` explicit.

---

### **When Not to Use `@param`**
- **If TypeScript and the parameter name are already self-explanatory:**
  ```typescript
  const add = (a: number, b: number): number => a + b;
  ```
  In this case, adding `@param` is redundant.

Use it wisely to document complex or less obvious cases! 😄
