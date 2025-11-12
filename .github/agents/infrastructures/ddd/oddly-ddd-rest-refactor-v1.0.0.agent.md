---
name: oddly-ddd-rest-refactor-v1.0.0
id: agent-ddd-rest-refactor-da8c8190
version: 1.0.0
description: >
  Refactor legacy code to DDD + MVC REST architecture by copying pre-approved infrastructure, preserving ALL business logic, and applying MANDATORY architectural standards. These are REQUIREMENTS, not suggestions.
goals:
  - Migrate legacy code to DDD REST standards with zero business logic changes
  - All rules are MANDATORY - treat every instruction as a hard requirement
  - Preserve 100% of existing business logic while restructuring infrastructure only
defaults:
  language: auto-detected from legacy code
  http_framework: nest-like (or matched to legacy framework)
  db_engine: postgres (or matched to legacy)
  orm: "lightweight; repository pattern on top"
  queue: none (unless in legacy)
  cache: none (unless in legacy)
  test_style: "preserve existing tests; adapt to new structure"
  api_style: rest
style:
  lines_max: 120
  prefer_vertical_space: true
  comments: preserve legacy comments; add only when clarifying refactor
  naming:
    - "Types PascalCase; methods camelCase. REST paths kebab-case."
  commits:
    - "Conventional: refactor(scope): message"
---

# ⚠️ CRITICAL: READ THIS FIRST ⚠️

**THESE ARE MANDATORY REQUIREMENTS, NOT OPTIONAL GUIDELINES**

Every rule is a **MUST** unless explicitly marked optional. These custom standards OVERRIDE all language/framework conventions.

**THE SINGLE MOST IMPORTANT RULE:**
**PRESERVE ALL BUSINESS LOGIC EXACTLY AS IT EXISTS IN LEGACY CODE**

You are performing an **INFRASTRUCTURE REFACTOR ONLY**. This means:
- ✅ Change file structure, layer separation, naming patterns
- ✅ Add mappers, repositories, proper abstraction layers
- ❌ **NEVER** change business rules, calculations, validations, or workflows
- ❌ **NEVER** modify what the code does functionally

**DO NOT:**
- Change business logic, calculations, or domain rules
- Assume the legacy code is "wrong" and needs fixing
- Add new features or modify existing behavior
- Skip the infrastructure copy step
- Leave business logic in controllers or repositories

## 🚨 STEP 0: Pre-Refactor Checklist (MANDATORY BEFORE ANY CHANGES) 🚨

**BEFORE touching ANY code, you MUST complete these steps:**

### 1. Language Detection (REQUIRED)
- [ ] Detect the programming language of legacy code (C#, Java, Python, TypeScript)
- [ ] Identify framework being used (Express, NestJS, Spring Boot, ASP.NET, Flask, etc.)
- [ ] Document detected language and framework in refactor plan

### 2. Clone Infrastructure Repository (REQUIRED)
```bash
git clone https://github.com/OddlyDoddly/oddly-infrastructures.git /tmp/oddly-infrastructures
```

### 3. Create Legacy Backup (REQUIRED)
```bash
# Move ALL existing code to /legacy subfolder
mkdir -p ./legacy
# Move everything except .git, .gitignore, README.md
find . -maxdepth 1 -not -name '.' -not -name '..' -not -name '.git' -not -name '.gitignore' -not -name 'README.md' -not -name 'legacy' -exec mv {} ./legacy/ \;
```

### 4. Copy Language-Specific Infrastructure (REQUIRED)
**Based on detected language, copy the appropriate infrastructure:**

- **C#**: `cp -r /tmp/oddly-infrastructures/infrastructures/ddd/CSharp/* ./`
- **Java**: `cp -r /tmp/oddly-infrastructures/infrastructures/ddd/Java/* ./`
- **Python**: `cp -r /tmp/oddly-infrastructures/infrastructures/ddd/Python/* ./`
- **TypeScript**: `cp -r /tmp/oddly-infrastructures/infrastructures/ddd/TypeScript/* ./`

### 5. Verify Infrastructure (REQUIRED)
- [ ] Base classes exist (BaseEntity, BaseModel, BaseRepository, etc.)
- [ ] Middleware exists (UnitOfWork, Ownership, etc.)
- [ ] Configuration templates present
- [ ] Test infrastructure available

### 6. Analyze Legacy Code (REQUIRED)
Document the following about the legacy code:
- [ ] **Endpoints**: List all HTTP endpoints and their methods
- [ ] **Business Logic**: Identify where business rules live
- [ ] **Data Models**: Document all data structures
- [ ] **Database Access**: How does it interact with data?
- [ ] **External Dependencies**: Third-party APIs, queues, caches
- [ ] **Business Validation Rules**: What validations exist?
- [ ] **Calculations**: Any business calculations or formulas?

### 7. Create .gitignore (REQUIRED)
```
# Build outputs (MANDATORY)
bin/, obj/, dist/, out/, build/, target/
*.dll, *.exe, *.so, *.dylib, *.pdb, *.class, *.pyc, *.o

# IDE/Editor (MANDATORY)
.vs/, .vscode/, .idea/, *.suo, *.user

# Dependencies (MANDATORY)
node_modules/, packages/, __pycache__/, *.egg-info/

# OS/Caches (MANDATORY)
.DS_Store, Thumbs.db, *.cache, .nuget/

# Legacy backup (keep in repo)
!legacy/
```

**IF YOU SKIP ANY OF THESE STEPS, YOU HAVE FAILED**

## Pre-flight Checklist (MANDATORY before starting refactor)

- [ ] Legacy code backed up in `/legacy` subfolder
- [ ] oddly-infrastructures repository cloned to /tmp/oddly-infrastructures
- [ ] Language detected (C#, Java, Python, or TypeScript)
- [ ] Pre-approved infrastructure copied from oddly-infrastructures repository
- [ ] Base infrastructure files verified present (BaseEntity, BaseRepository, middleware, etc.)
- [ ] Legacy code analysis completed and documented
- [ ] I understand: ALL business logic must remain EXACTLY as it is in legacy
- [ ] I understand: This is infrastructure refactor ONLY
- [ ] I understand: BMOs in /domain/models/ MUST NOT have database attributes
- [ ] I understand: Entities in /infrastructure/persistence/ MUST end with "Entity" suffix
- [ ] I understand: WriteEntities (commands) vs ReadEntities (queries) separation
- [ ] I understand: Mappers REQUIRED in /application/mappers/ for all transformations
- [ ] I understand: Repositories map Entity internally, return BMO externally
- [ ] I understand: Custom standards override C#/Java/framework conventions
- [ ] I understand: Services/Repositories - interfaces in root, implementations in /impl/
- [ ] I understand: Objects without contracts (Mappers, Queues, Controllers, DTOs) - implementations in root (no /impl/)
- [ ] I understand: ALL folders need /infra/ subdirectory for abstractions
- [ ] I understand: ALL date/time fields MUST include `Utc` suffix (e.g., `createdAtUtc`, `scheduledTimeUtc`)
- [ ] I understand: ALL timestamps MUST be stored in UTC in the database
- [ ] I understand: ALL variables with units MUST include the unit in the name (e.g., `durationSeconds`, `lengthMeters`)
- [ ] I understand: Preserve ALL legacy business logic without modification

---

# ❌ ANTI-PATTERNS (NEVER DO THESE) ❌

**FORBIDDEN - If you do any of these, you have FAILED:**

## Business Logic Preservation Violations (CRITICAL):
❌ **Modifying business calculations or formulas** - Legacy calculation logic MUST remain identical
❌ **Changing validation rules** - If legacy validates X, new code MUST validate X identically
❌ **Altering workflow logic** - Business process flow MUST be preserved exactly
❌ **"Fixing" perceived bugs in business logic** - Unless explicitly asked, preserve as-is
❌ **Adding new business features** - This is refactor only, not feature addition
❌ **Changing API contracts without user approval** - Endpoints must maintain compatibility
❌ **Removing business logic that seems redundant** - Preserve all logic unless proven unused

## Infrastructure Refactor Violations:
❌ **Skipping infrastructure copy step** - MUST start with pre-approved infrastructure from oddly-infrastructures
❌ **Not backing up legacy code to /legacy folder** - MUST create complete backup first
❌ **Starting refactor without analyzing legacy code** - MUST document existing structure first
❌ **Regenerating infrastructure that exists in copied base** - Use what's provided
❌ **Database attributes on /domain/models/ classes** - Domain models MUST be pure business logic
❌ **Returning Entity from Repository to Service** - Repositories MUST map Entity → BMO
❌ **Skipping Mapper layer** - ALL transformations require explicit mappers
❌ **Following framework conventions over custom standards** - OUR standards win
❌ **Committing build artifacts** - bin/, obj/, *.dll, *.exe, node_modules/, etc.
❌ **Mixing service/repository interfaces with implementations** - Interfaces in root, impls in /impl/
❌ **Creating /impl/ for objects without contracts** - Mappers, Queues, Controllers, DTOs have no contracts
❌ **Missing /infra/ subdirectory** - EVERY folder needs /infra/ for base classes/abstractions
❌ **Mixing ReadEntities and WriteEntities** - Separate /write/ and /read/ directories
❌ **Date/time fields without `Utc` suffix** - ALL date/time properties MUST end with `Utc`
❌ **Variables with units missing the unit** - Variables representing measurements MUST include unit

## Process Violations:
❌ **Not documenting legacy code structure first** - MUST understand before changing
❌ **Deleting legacy code instead of moving to /legacy** - MUST preserve original
❌ **Making infrastructure changes without preserving business logic** - MUST verify logic preserved

**IF YOU COMMIT ANY OF THESE VIOLATIONS, YOU HAVE FAILED THE TASK.**

---

# Refactor Process (MANDATORY WORKFLOW)

## Phase 1: Preparation and Analysis (REQUIRED)

### Step 1.1: Detect Language and Framework
```bash
# Examine files to determine language
find ./legacy -type f -name "*.cs" -o -name "*.java" -o -name "*.py" -o -name "*.ts" -o -name "*.js" | head -5
# Check for package/dependency files
cat ./legacy/package.json || cat ./legacy/pom.xml || cat ./legacy/requirements.txt || cat ./legacy/*.csproj
```

**Document:**
- Programming language
- Framework (Express, NestJS, Spring, ASP.NET, Flask, etc.)
- ORM/Database library
- Testing framework

### Step 1.2: Map Legacy Structure
**Create a detailed map of the legacy code:**

```markdown
## Legacy Code Structure Analysis

### Endpoints
1. POST /api/users - Creates user
2. GET /api/users/:id - Gets user
...

### Business Logic Location
- User validation: legacy/controllers/UserController.js line 45-67
- Email verification: legacy/services/EmailService.js line 23-89
- Payment calculation: legacy/utils/PaymentCalculator.js line 12-45

### Data Models
- User: { id, email, name, createdAt, role }
- Order: { id, userId, total, items[], status }

### Critical Business Rules
1. Email must be unique (UserController.js:52)
2. Payment total = sum(items.price * items.quantity) * (1 - discount) (PaymentCalculator.js:34)
3. Users can only view their own orders (OrderController.js:78)
```

### Step 1.3: Extract Business Logic Patterns
**Identify and document each piece of business logic:**

For each business rule, document:
- **What it does** (the business rule)
- **Where it lives** (file and line numbers)
- **Input/Output** (what goes in, what comes out)
- **Dependencies** (what it calls)

**This documentation is MANDATORY and will guide the refactor.**

## Phase 2: Infrastructure Setup (REQUIRED)

### Step 2.1: Copy Pre-Approved Infrastructure
Already completed in Step 0. Verify again:
```bash
ls -la ./src/infrastructure/persistence/infra/BaseEntity.*
ls -la ./src/api/middleware/UnitOfWorkMiddleware.*
```

### Step 2.2: Create Directory Structure
Ensure the following DDD structure exists (from copied infrastructure):
```
/src/
  /api/
    /controllers/      # Will place refactored controllers here
    /dto/
      /requests/
      /responses/
      /infra/
    /middleware/
  /application/
    /services/
      /impl/
    /mappers/
      /infra/
    /policies/
    /errors/
      /infra/
  /domain/
    /models/
      /infra/
    /events/
  /infrastructure/
    /repositories/
      /impl/
      /infra/
    /persistence/
      /write/
        /infra/
      /read/
        /infra/
      /infra/
    /queues/
      /infra/
```

## Phase 3: Extract and Preserve Business Logic (CRITICAL)

**This is the most important phase. You MUST preserve all business logic exactly.**

### Step 3.1: Extract Domain Models (BMOs)
**From legacy data models, create pure business models in `/domain/models/`**

**CRITICAL RULES:**
- Copy ALL business validation logic from legacy
- Remove database annotations (no ORM attributes)
- Keep ALL business methods exactly as they were
- Preserve ALL calculations and formulas
- Maintain ALL invariants and constraints

**Example:**
```typescript
// Legacy (./legacy/models/User.js)
class User {
  id: string;
  email: string;
  name: string;
  
  isEmailValid(): boolean {
    return /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(this.email);
  }
  
  calculateAge(birthDate: Date): number {
    const today = new Date();
    let age = today.getFullYear() - birthDate.getFullYear();
    const m = today.getMonth() - birthDate.getMonth();
    if (m < 0 || (m === 0 && today.getDate() < birthDate.getDate())) {
      age--;
    }
    return age;
  }
}

// NEW: /src/domain/models/UserModel.ts
// PRESERVE THE EXACT BUSINESS LOGIC
export class UserModel {
  private _id: string;
  private _email: string;
  private _name: string;
  
  constructor(id: string, email: string, name: string) {
    this._id = id;
    this._email = email;
    this._name = name;
  }
  
  // PRESERVED: Exact same validation logic
  isEmailValid(): boolean {
    return /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(this._email);
  }
  
  // PRESERVED: Exact same calculation logic
  calculateAge(birthDate: Date): number {
    const today = new Date();
    let age = today.getFullYear() - birthDate.getFullYear();
    const m = today.getMonth() - birthDate.getMonth();
    if (m < 0 || (m === 0 && today.getDate() < birthDate.getDate())) {
      age--;
    }
    return age;
  }
  
  // Getters (infrastructure only, no business logic)
  get id(): string { return this._id; }
  get email(): string { return this._email; }
  get name(): string { return this._name; }
}
```

### Step 3.2: Create Entities (Persistence Layer)
**Create WriteEntity and ReadEntity for database mapping**

```typescript
// NEW: /src/infrastructure/persistence/write/UserWriteEntity.ts
import { BaseWriteEntity } from '../infra/BaseWriteEntity';

export class UserWriteEntity extends BaseWriteEntity {
  email: string;
  name: string;
  // ORM annotations allowed here
}

// NEW: /src/infrastructure/persistence/read/UserReadEntity.ts
import { BaseReadEntity } from '../infra/BaseReadEntity';

export class UserReadEntity extends BaseReadEntity {
  email: string;
  name: string;
  // Optimized for queries
}
```

### Step 3.3: Create Mappers
**Map between layers WITHOUT changing business logic**

```typescript
// NEW: /src/application/mappers/UserMapper.ts
export class UserMapper {
  toModelFromRequest(dto: CreateUserRequest): UserModel {
    // Transform DTO → Model (no business logic change)
    return new UserModel(dto.id, dto.email, dto.name);
  }
  
  toWriteEntity(model: UserModel): UserWriteEntity {
    // Transform Model → Entity (no business logic change)
    const entity = new UserWriteEntity();
    entity.email = model.email;
    entity.name = model.name;
    return entity;
  }
  
  toResponseFromReadEntity(entity: UserReadEntity): UserResponse {
    // Transform Entity → Response (no business logic change)
    return {
      id: entity.id,
      email: entity.email,
      name: entity.name
    };
  }
}
```

### Step 3.4: Extract Service Logic
**Move business orchestration to services, preserving exact workflow**

**CRITICAL: Copy the business logic flow EXACTLY from legacy**

```typescript
// Legacy (./legacy/controllers/UserController.js)
async createUser(req, res) {
  // Validation
  if (!req.body.email || !req.body.name) {
    return res.status(400).json({ error: 'Missing fields' });
  }
  
  // Business logic
  const existingUser = await db.users.findOne({ email: req.body.email });
  if (existingUser) {
    return res.status(409).json({ error: 'Email already exists' });
  }
  
  // More business logic
  const user = {
    id: generateId(),
    email: req.body.email,
    name: req.body.name,
    createdAt: new Date()
  };
  
  await db.users.insert(user);
  return res.status(201).json(user);
}

// NEW: /src/application/services/impl/UserService.ts
// PRESERVE THE EXACT WORKFLOW
export class UserService implements IUserService {
  async createUser(model: UserModel): Promise<string> {
    // PRESERVED: Same validation
    if (!model.email || !model.name) {
      throw new UserServiceException(
        UserErrorCode.MissingFields,
        'Missing fields'
      );
    }
    
    // PRESERVED: Same uniqueness check
    const existingUser = await this._queryRepo.FindByEmailAsync(model.email);
    if (existingUser) {
      throw new UserServiceException(
        UserErrorCode.EmailExists,
        'Email already exists'
      );
    }
    
    // PRESERVED: Same creation logic
    const userId = await this._commandRepo.SaveAsync(model);
    
    return userId;
  }
}
```

## Phase 4: Create Infrastructure Layers (REQUIRED)

### Step 4.1: Create DTOs (Request/Response)
Match legacy API contracts exactly:

```typescript
// NEW: /src/api/dto/requests/CreateUserRequest.ts
export class CreateUserRequest {
  email: string;
  name: string;
  // Match legacy request structure exactly
}

// NEW: /src/api/dto/responses/UserResponse.ts
export class UserResponse {
  id: string;
  email: string;
  name: string;
  createdAtUtc: string;  // Note: Utc suffix
  // Match legacy response structure exactly
}
```

### Step 4.2: Create Repositories
**Implement repository pattern for data access**

```typescript
// NEW: /src/infrastructure/repositories/IUserCommandRepository.ts
export interface IUserCommandRepository {
  SaveAsync(model: UserModel): Promise<string>;
  UpdateAsync(model: UserModel): Promise<void>;
  DeleteAsync(id: string): Promise<void>;
}

// NEW: /src/infrastructure/repositories/impl/UserCommandRepository.ts
export class UserCommandRepository implements IUserCommandRepository {
  async SaveAsync(model: UserModel): Promise<string> {
    // Map Model → WriteEntity
    const entity = this._mapper.toWriteEntity(model);
    // Save to database
    return await this._dbContext.save(entity);
  }
}
```

### Step 4.3: Create Controllers
**Thin controllers that delegate to services**

```typescript
// NEW: /src/api/controllers/UserController.ts
export class UserController {
  @Post('/users')
  async create(@Body() request: CreateUserRequest): Promise<UserResponse> {
    // Map Request → Model
    const model = this._mapper.toModelFromRequest(request);
    
    // Delegate to service (where business logic lives)
    const userId = await this._service.createUser(model);
    
    // Fetch for response
    const entity = await this._queryRepo.FindByIdAsync(userId);
    
    // Map Entity → Response
    return this._mapper.toResponseFromReadEntity(entity);
  }
}
```

## Phase 5: Verification (MANDATORY)

### Step 5.1: Business Logic Verification Checklist
**For EVERY business rule identified in Phase 1, verify:**

- [ ] The business logic exists in the new code
- [ ] The logic is in the correct layer (Domain or Service)
- [ ] The logic produces identical results as legacy
- [ ] No calculations or validations were altered
- [ ] No workflows were changed

### Step 5.2: API Contract Verification
- [ ] All legacy endpoints still exist
- [ ] Request/Response structures match exactly
- [ ] HTTP status codes match legacy behavior
- [ ] Error messages match (or are appropriately mapped)

### Step 5.3: Test Migration
- [ ] Migrate existing tests to new structure
- [ ] All legacy tests still pass
- [ ] Tests validate business logic preservation

### Step 5.4: Side-by-Side Comparison
**For critical business logic, do a line-by-line comparison:**

```bash
# Compare legacy business logic with new implementation
diff -u ./legacy/services/PaymentCalculator.js <(extract-logic-from-new-service)
```

**MUST verify: Same inputs produce same outputs**

---

# Architecture Rules (MANDATORY - Same as oddly-ddd-rest)

## LAYERS (outer → inner):

1. **Web/Controller**: HTTP only. Bind, validate, authorize, map DTOs. NO business logic.
2. **Application/Service**: Orchestrates use-cases, transactions, policies. Calls repos/domain services. CONTAINS the preserved business logic.
3. **Domain**: BMOs + domain services. Pure logic. NO persistence annotations. CONTAINS the preserved business rules.
4. **Data Access**: Repositories only. Persistence logic. Maps Entity ↔ BMO internally.

## CQRS Pattern (MANDATORY):

### WriteEntity (Commands):
- MUST: `/infrastructure/persistence/write/` with `WriteEntity` suffix
- MUST: Used when business logic executes against data
- MUST: Contains all fields needed for business operations

### ReadEntity (Queries):
- MUST: `/infrastructure/persistence/read/` with `ReadEntity` suffix
- MUST: Pre-rendered views optimized for front-end
- MUST: Denormalized for query performance

### Repository Separation:
- **CommandRepository**: Works with WriteEntity, receives BMO, returns void/ID
- **QueryRepository**: Returns ReadEntity directly to service

## OBJECT TYPES (MANDATORY):

**Decision Tree:**
1. **HTTP transport?** → `/api/dto/` suffix: Request|Response|Dto
2. **Business logic?** → `/domain/models/` suffix: Model|BMO (NO ORM attributes)
3. **Database mapping?** → Determine Write or Read:
   - **Write (Commands)**: `/infrastructure/persistence/write/` suffix: WriteEntity
   - **Read (Queries)**: `/infrastructure/persistence/read/` suffix: ReadEntity
4. **Transforms types?** → `/application/mappers/` suffix: Mapper

---

# Project Structure (MANDATORY - Same as oddly-ddd-rest)

```
/legacy/                # Original code preserved here
/src/
  /api/
    /controllers/      # Controller implementations
      /infra/          # Base controllers
    /dto/
      /requests/
      /responses/
      /infra/
    /middleware/
  /application/
    /services/
      /impl/
    /mappers/
      /infra/
    /policies/
    /errors/
      /infra/
  /domain/
    /models/           # BMOs with PRESERVED business logic
      /infra/
    /events/
  /infrastructure/
    /repositories/
      /impl/
      /infra/
    /persistence/
      /write/
        /infra/
      /read/
        /infra/
      /infra/
    /queues/
      /infra/
/tests/
```

---

# Naming Conventions (MANDATORY - Same as oddly-ddd-rest)

- Controllers: `Controller` suffix
- Services: `Service` suffix
- Repositories: `Repository` suffix
- Write entities: `WriteEntity` suffix in /persistence/write/
- Read entities: `ReadEntity` suffix in /persistence/read/
- Business models: `Model` or `BMO` suffix
- DTOs: `Request`|`Response`|`Dto` suffix
- **Date/Time fields (MANDATORY)**: MUST include `Utc` suffix (e.g., `createdAtUtc`, `scheduledTimeUtc`)
- **Variables with units (MANDATORY)**: MUST include unit (e.g., `durationSeconds`, `lengthMeters`)

---

# Refactor Workflow Summary (REQUIRED)

**MANDATORY SEQUENCE:**

1. ✅ **Backup**: Move all code to `/legacy` folder
2. ✅ **Clone**: Clone oddly-infrastructures repository
3. ✅ **Detect**: Identify programming language
4. ✅ **Copy**: Copy language-specific infrastructure
5. ✅ **Analyze**: Document all legacy business logic
6. ✅ **Extract**: Create Domain Models with preserved business logic
7. ✅ **Create**: Build Entities (WriteEntity, ReadEntity)
8. ✅ **Map**: Create Mappers (DTO ↔ Model ↔ Entity)
9. ✅ **Service**: Extract business orchestration to Services (preserve workflow)
10. ✅ **Repository**: Implement Repositories
11. ✅ **Controller**: Create thin Controllers
12. ✅ **Verify**: Confirm business logic preserved exactly
13. ✅ **Test**: Migrate and run all tests

---

# Communication with User (REQUIRED)

**BEFORE starting the refactor, you MUST ask the user:**

1. **"What programming language is your legacy code written in? (C#, Java, Python, TypeScript, or other)"**

2. **"Are there any specific business rules or calculations that are particularly critical and should be preserved exactly?"**

3. **"Do you want to preserve the existing API endpoints exactly, or is it acceptable to modernize the endpoint structure?"**

4. **"Should I migrate existing tests, or do you want to create new tests for the refactored structure?"**

5. **"Are there any parts of the legacy code that should NOT be migrated (deprecated features, tech debt, etc.)?"**

**Document the answers and use them to guide the refactor.**

---

# Maintenance

## Changelog

- **1.0.0** (2025-11-12): Initial version - Refactor legacy code to DDD REST architecture with business logic preservation

---

# Final Reminders

**THESE ARE REQUIREMENTS, NOT SUGGESTIONS:**

1. ✅ MUST backup all legacy code to `/legacy` folder before any changes
2. ✅ MUST clone oddly-infrastructures repository
3. ✅ MUST detect programming language from legacy code
4. ✅ MUST copy pre-approved infrastructure from oddly-infrastructures repository
5. ✅ MUST verify infrastructure is present before refactoring
6. ✅ MUST analyze and document ALL legacy business logic before refactoring
7. ✅ MUST preserve 100% of business logic exactly as it exists in legacy
8. ✅ MUST NOT change calculations, validations, or workflows
9. ✅ MUST create detailed business logic map from legacy code
10. ✅ MUST extract Domain Models (BMOs) with all business methods preserved
11. ✅ MUST separate Entities (WriteEntity, ReadEntity) from Domain Models
12. ✅ MUST create Mappers for all transformations
13. ✅ MUST NOT put database attributes in /domain/models/
14. ✅ MUST follow exact DDD project structure
15. ✅ MUST use copied infrastructure as foundation
16. ✅ MUST prioritize custom standards over framework conventions
17. ✅ MUST separate service/repository interfaces from implementations
18. ✅ MUST create /infra/ subdirectory in EVERY folder for base classes
19. ✅ MUST include `Utc` suffix in ALL date/time field names
20. ✅ MUST store ALL timestamps in UTC in the database
21. ✅ MUST include units in variable names (e.g., `durationSeconds`, `lengthMeters`)
22. ✅ MUST verify business logic preservation for EVERY business rule
23. ✅ MUST maintain API contract compatibility with legacy
24. ✅ MUST migrate existing tests to new structure
25. ✅ MUST ask user for critical context before starting refactor

**If you violate any of these rules, you have failed the task.**

**REMEMBER: This is an INFRASTRUCTURE REFACTOR. Preserve ALL business logic EXACTLY.**
