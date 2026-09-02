# Task 1 solution

## Files to tag in Cursor: @src/types/index.ts
### TASK SPEC 1: Audit Logger Types
- INPUTS: @src/types/index.ts
- ACTION: Create a new file `@src/types/audit.ts` that exports two TypeScript interfaces:
  1. `AuditEntry`: timestamp (string), method (string), path (string), body (Record<string, unknown>), statusCode (optional number)
  2. `RedactionConfig`: keysToRedact (string[])
- OUTPUT: `@src/types/audit.ts`
- CONSTRAINTS:
  - Do NOT modify existing route or server files.
  - Do NOT install any external npm packages.
  - Strictly typed with no `any` types.
- SUCCESS CRITERIA: File compiles cleanly with zero TypeScript errors.

# Task 2 solution

## Files to tag in Cursor: @src/types/audit.ts
### TASK SPEC 2: Redactor & File Logger Logic
- INPUTS: @src/types/audit.ts
- ACTION: Create a new file `@src/services/auditService.ts` that exports two functions:
  1. `redactPayload(data: Record<string, unknown>, config?: RedactionConfig): Record<string, unknown>`
     - Replaces target key values (defaulting to 'password', 'token', 'secret', 'creditCard') with "***REDACTED***".
     - Performs a shallow/deep copy so original request objects aren't mutated.
  2. `appendAuditLog(entry: AuditEntry): void`
     - Appends the entry as a single-line JSON string to `audit.log` using Node's `fs.appendFileSync`.
- OUTPUT: `@src/services/auditService.ts`
- CONSTRAINTS:
  - Do NOT use external logging libraries (e.g., Winston, Pino, Lodash); use native Node `fs` and `path` modules only.
- SUCCESS CRITERIA: File compiles cleanly with zero TypeScript errors.

# Task 3 solution
## Files to tag in Cursor: @src/types/audit.ts, @src/services/auditService.ts, @src/server.ts
### TASK SPEC 3: Express Audit Middleware
- INPUTS: @src/types/audit.ts, @src/services/auditService.ts, @src/server.ts
- ACTION: 
  1. Create `@src/middleware/auditMiddleware.ts` exporting an Express middleware function `auditLoggerMiddleware`.
     - Intercept `res.on('finish')` to capture the final HTTP status code.
     - Pass `req.body` through `redactPayload()` before writing to disk with `appendAuditLog()`.
  2. Update `@src/server.ts` to mount `auditLoggerMiddleware` globally BEFORE the `/api` routes.
- OUTPUT: `@src/middleware/auditMiddleware.ts` and `@src/server.ts`
- CONSTRAINTS:
  - Do NOT rewrite existing routes or server startup logic.
  - Wrap log writing in a try/catch so file errors never crash incoming HTTP requests.
- SUCCESS CRITERIA: `npm run dev` starts smoothly, and sending a POST request writes a redacted log entry into `audit.log`.
