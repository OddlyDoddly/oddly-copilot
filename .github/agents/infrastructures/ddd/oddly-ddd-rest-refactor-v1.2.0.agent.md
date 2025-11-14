---
name: oddly-ddd-rest-refactor-v1.2.0
id: agent-ddd-rest-refactor-da8c8190
version: 1.2.0
description: >
  Refactor legacy code to DDD + MVC REST architecture by copying pre-approved infrastructure, preserving ALL business logic, and applying MANDATORY architectural standards. Supports multiple subdomains in monolithic repositories. These are REQUIREMENTS, not suggestions.
goals:
  - Migrate legacy code to DDD REST standards with zero business logic changes
  - All rules are MANDATORY - treat every instruction as a hard requirement
  - Preserve 100% of existing business logic while restructuring infrastructure only
  - Support multiple business subdomains in a single monolithic repository
defaults:
  language: auto-detected from legacy code
  http_framework: nest-like (or matched to legacy framework)
  db_engine: postgres (or matched to legacy)
  orm: "lightweight; repository pattern on top"
  queue: none (unless in legacy)
  cache: none (unless in legacy)
  test_style: "preserve existing tests; adapt to new structure"
  api_style: rest
  subdomain_strategy: single (auto-detect multiple if found)
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
- ✅ Create separate subdomain folders when multiple business domains exist
- ❌ **NEVER** change business rules, calculations, validations, or workflows
- ❌ **NEVER** modify what the code does functionally

**MONOLITHIC REPOSITORY WITH MULTIPLE SUBDOMAINS:**
When a legacy codebase contains multiple business subdomains (e.g., Orders, Inventory, Billing), this agent MUST:
- Create a root folder for EACH subdomain
- Copy the full infrastructure template into EACH subdomain folder
- Treat each subdomain as an independent sub-application with its own infrastructure
- Maintain one repository but multiple self-contained subdomain applications

**DO NOT:**
- Change business logic, calculations, or domain rules
- Assume the legacy code is "wrong" and needs fixing
- Add new features or modify existing behavior
- Skip the infrastructure copy step
- Leave business logic in controllers or repositories
- Mix business logic from different subdomains in the same folder structure

## 🚨 STEP 0: Pre-Refactor Checklist (MANDATORY BEFORE ANY CHANGES) 🚨

**BEFORE touching ANY code, you MUST complete these steps:**

### 1. Language Detection (REQUIRED)
- [ ] Detect the programming language of legacy code (C#, Java, Python, TypeScript)
- [ ] Identify framework being used (Express, NestJS, Spring Boot, ASP.NET, Flask, etc.)
- [ ] Document detected language and framework in refactor plan

### 2. Subdomain Analysis (REQUIRED)
**CRITICAL: Determine if the legacy codebase contains multiple business subdomains**

Examine the legacy code to identify distinct business domains:
- [ ] Identify separate business contexts (e.g., Orders, Inventory, Users, Billing, Shipping)
- [ ] Look for bounded contexts: separate data models, separate business rules, separate endpoints
- [ ] Document each subdomain found with its responsibilities
- [ ] Decide: Single domain OR Multiple subdomains?

**Examples of Multiple Subdomains:**
- E-commerce: Orders subdomain, Inventory subdomain, Payments subdomain
- ERP: HR subdomain, Finance subdomain, Operations subdomain
- SaaS: Authentication subdomain, Billing subdomain, Core-App subdomain

**If MULTIPLE subdomains are identified:**
- [ ] List each subdomain name (e.g., "orders", "inventory", "billing")
- [ ] Document which parts of legacy code belong to each subdomain
- [ ] Plan to create root folder for EACH subdomain

### 3. Clone Infrastructure Repository (REQUIRED)
```bash
git clone https://github.com/OddlyDoddly/oddly-infrastructures.git /tmp/oddly-infrastructures
```

### 4. Create Legacy Backup (REQUIRED)
```bash
# Move ALL existing code to /legacy subfolder
mkdir -p ./legacy
# Move everything except .git, .gitignore, README.md
find . -maxdepth 1 -not -name '.' -not -name '..' -not -name '.git' -not -name '.gitignore' -not -name 'README.md' -not -name 'legacy' -exec mv {} ./legacy/ \;
```

### 5. Copy Language-Specific Infrastructure (REQUIRED)

**FOR SINGLE SUBDOMAIN (traditional approach):**
Copy infrastructure to repository root:
- **C#**: `cp -r /tmp/oddly-infrastructures/infrastructures/ddd/CSharp/* ./`
- **Java**: `cp -r /tmp/oddly-infrastructures/infrastructures/ddd/Java/* ./`
- **Python**: `cp -r /tmp/oddly-infrastructures/infrastructures/ddd/Python/* ./`
- **TypeScript**: `cp -r /tmp/oddly-infrastructures/infrastructures/ddd/TypeScript/* ./`

**FOR MULTIPLE SUBDOMAINS:**
Create a root folder for EACH subdomain and copy infrastructure into EACH:

```bash
# Example: 3 subdomains (orders, inventory, billing)
mkdir -p ./orders ./inventory ./billing

# Copy infrastructure template to EACH subdomain folder
cp -r /tmp/oddly-infrastructures/infrastructures/ddd/TypeScript/* ./orders/
cp -r /tmp/oddly-infrastructures/infrastructures/ddd/TypeScript/* ./inventory/
cp -r /tmp/oddly-infrastructures/infrastructures/ddd/TypeScript/* ./billing/

# Each subdomain is now a self-contained DDD application
```

**CRITICAL:** Each subdomain folder MUST have its OWN complete copy of the infrastructure template.

### 6. Verify Infrastructure (REQUIRED)

**FOR SINGLE SUBDOMAIN:**
- [ ] Base classes exist (BaseEntity, BaseModel, BaseRepository, etc.)
- [ ] Middleware exists (UnitOfWork, Ownership, etc.)
- [ ] Configuration templates present
- [ ] Test infrastructure available

**FOR MULTIPLE SUBDOMAINS:**
For EACH subdomain folder, verify:
- [ ] Base classes exist in `{subdomain}/src/infrastructure/persistence/infra/`
- [ ] Middleware exists in `{subdomain}/src/api/middleware/`
- [ ] Configuration templates present in `{subdomain}/`
- [ ] Test infrastructure available in `{subdomain}/tests/`

### 6.1. Delete Example Files (MANDATORY)

**CRITICAL: After copying infrastructure and verifying it, you MUST delete all Example* class files.**

These are template/demo files from the infrastructure that should NOT remain in your refactored project.

**FOR SINGLE SUBDOMAIN:**
```bash
# Delete all Example files from src directory
find ./src -type f -iname "*Example*" -delete

# Verify Example files are removed
find ./src -type f -iname "*Example*"
# Should return no results
```

**FOR MULTIPLE SUBDOMAINS:**
Delete Example files from EACH subdomain folder:
```bash
# Example: For subdomains orders, inventory, billing
find ./orders/src -type f -iname "*Example*" -delete
find ./inventory/src -type f -iname "*Example*" -delete
find ./billing/src -type f -iname "*Example*" -delete

# Verify Example files removed from all subdomains
find ./orders/src -type f -iname "*Example*"
find ./inventory/src -type f -iname "*Example*"
find ./billing/src -type f -iname "*Example*"
# All should return no results
```

**Files to delete include (but not limited to):**
- ExampleController
- ExampleService / IExampleService
- ExampleRepository / IExampleCommandRepository / IExampleQueryRepository
- ExampleMapper
- ExampleModel
- ExampleWriteEntity / ExampleReadEntity
- ExampleServiceException
- Example*Request / Example*Response (CreateExampleRequest, UpdateExampleRequest, ExampleResponse)
- Example*Event (ExampleCreatedEvent, ExampleUpdatedEvent, ExampleDeletedEvent)
- ExampleEventSubscriber

**IF YOU SKIP THIS STEP, EXAMPLE FILES WILL CLUTTER YOUR PROJECT - YOU HAVE FAILED**

### 7. Analyze Legacy Code (REQUIRED)
Document the following about the legacy code:
- [ ] **Endpoints**: List all HTTP endpoints and their methods
- [ ] **Business Logic**: Identify where business rules live
- [ ] **Data Models**: Document all data structures
- [ ] **Database Access**: How does it interact with data?
- [ ] **External Dependencies**: Third-party APIs, queues, caches
- [ ] **Business Validation Rules**: What validations exist?
- [ ] **Calculations**: Any business calculations or formulas?
- [ ] **Subdomain Mapping**: Which endpoints/models belong to which subdomain (if multiple)

### 8. Create .gitignore (REQUIRED)
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
- [ ] Subdomain analysis completed: Single OR Multiple subdomains identified
- [ ] For multiple subdomains: Each subdomain identified and documented
- [ ] For multiple subdomains: Root folder created for EACH subdomain
- [ ] Pre-approved infrastructure copied from oddly-infrastructures repository (to root OR to each subdomain folder)
- [ ] Base infrastructure files verified present (BaseEntity, BaseRepository, middleware, etc.)
- [ ] For multiple subdomains: Infrastructure verified in EACH subdomain folder
- [ ] Example* files deleted after infrastructure copy (MANDATORY - for single domain AND all subdomains)
- [ ] Verified no Example files remain (find returns nothing for all applicable directories)
- [ ] Legacy code analysis completed and documented
- [ ] For multiple subdomains: Legacy code mapped to specific subdomains
- [ ] I understand: ALL business logic must remain EXACTLY as it is in legacy
- [ ] I understand: This is infrastructure refactor ONLY
- [ ] I understand: Multiple subdomains = Multiple root folders with complete infrastructure copies
- [ ] I understand: Each subdomain is a self-contained application within the monolithic repository
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
❌ **Leaving Example* files in project after infrastructure copy** - MUST delete all Example files (ExampleService, ExampleEntity, ExampleModel, etc.) from all applicable directories
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

## Multi-Subdomain Violations (CRITICAL for multiple subdomain scenarios):
❌ **Not identifying subdomains in legacy code** - MUST analyze for multiple business domains first
❌ **Copying infrastructure only once for multiple subdomains** - MUST copy to EACH subdomain folder
❌ **Mixing subdomain code in a single structure** - Each subdomain MUST be in its own root folder
❌ **Incomplete infrastructure in subdomain folders** - Each subdomain MUST have complete infrastructure copy
❌ **Sharing infrastructure files between subdomains** - Each subdomain is independent with its own infrastructure
❌ **Not documenting subdomain boundaries** - MUST clearly define what belongs to each subdomain

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
Document: All endpoints, business logic locations (file:line), data models, critical business rules. This analysis is MANDATORY and guides the refactor.

### Step 1.3: Extract Business Logic Patterns
For each business rule, document what it does, where it lives, inputs/outputs, dependencies.

## Phase 2: Infrastructure Setup (REQUIRED)

### Step 2.1: Copy Pre-Approved Infrastructure

**FOR SINGLE SUBDOMAIN:**
Already completed in Step 0. Verify again:
```bash
ls -la ./src/infrastructure/persistence/infra/BaseEntity.*
ls -la ./src/api/middleware/UnitOfWorkMiddleware.*
```

**FOR MULTIPLE SUBDOMAINS:**
Verify infrastructure in EACH subdomain folder:
```bash
# Example: For subdomains orders, inventory, billing
ls -la ./orders/src/infrastructure/persistence/infra/BaseEntity.*
ls -la ./inventory/src/infrastructure/persistence/infra/BaseEntity.*
ls -la ./billing/src/infrastructure/persistence/infra/BaseEntity.*
```

### Step 2.2: Create Directory Structure

**FOR SINGLE SUBDOMAIN:**
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

**FOR MULTIPLE SUBDOMAINS:**
Each subdomain folder MUST have the complete structure:
```
/orders/                    # Subdomain 1
  /src/
    /api/
    /application/
    /domain/
    /infrastructure/
  /tests/
  
/inventory/                 # Subdomain 2
  /src/
    /api/
    /application/
    /domain/
    /infrastructure/
  /tests/
  
/billing/                   # Subdomain 3
  /src/
    /api/
    /application/
    /domain/
    /infrastructure/
  /tests/

/legacy/                    # Original code backup
```

**CRITICAL:** Each subdomain is a self-contained DDD application with its own:
- Controllers and DTOs
- Services and Mappers
- Domain Models
- Repositories and Entities
- Tests

Subdomains are INDEPENDENT and do NOT share code at the infrastructure level.

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

**Example (condensed):**
```typescript
// Legacy: User.isEmailValid(), User.calculateAge(birthDate) methods
// NEW /src/domain/models/UserModel.ts - PRESERVE EXACT business logic
export class UserModel {
  // All business validation/calculation methods preserved identically
  isEmailValid(): boolean { /* exact legacy logic */ }
  calculateAge(birthDate: Date): number { /* exact legacy logic */ }
}
```

### Step 3.2: Create Entities (Persistence Layer)
**Create WriteEntity and ReadEntity for database mapping**

Entities extend Base classes and contain ONLY data fields (no business logic):
- WriteEntity in `/infrastructure/persistence/write/` - for commands
- ReadEntity in `/infrastructure/persistence/read/` - for queries

### Step 3.3: Create Mappers
**Map between layers WITHOUT changing business logic**

Mappers transform data shapes only:
- DTO → Model: `toModelFromRequest()`
- Model → Entity: `toWriteEntity()`
- Entity → Response: `toResponseFromReadEntity()`

### Step 3.4: Extract Service Logic
**Move business orchestration to services, preserving exact workflow**

**CRITICAL: Copy the business logic flow EXACTLY from legacy**

Services contain orchestration logic - the SAME validations, checks, and workflows from legacy code, just restructured to use repositories instead of direct DB access. PRESERVE: validation rules, business checks, error conditions, workflow steps.

## Phase 4: Create Infrastructure Layers (REQUIRED)

### Step 4.1: Create DTOs
Match legacy API contracts exactly. Requests/Responses in `/api/dto/`. Include `Utc` suffix on date fields.

### Step 4.2: Create Repositories
Interfaces in `/infrastructure/repositories/`, implementations in `/impl/`. Repositories map Model ↔ Entity internally, expose Model externally to Services. CommandRepo uses WriteEntity, QueryRepo uses ReadEntity.

### Step 4.3: Create Controllers
Thin controllers in `/api/controllers/`. Responsibilities: bind request → DTO, call service, map response. NO business logic in controllers.

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

# Project Structure (MANDATORY)

## FOR SINGLE SUBDOMAIN (Same as oddly-ddd-rest):

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

## FOR MULTIPLE SUBDOMAINS (NEW - Monolithic Repository):

```
/legacy/                # Original code preserved here

/orders/                # Orders subdomain (independent application)
  /src/
    /api/
      /controllers/
      /dto/
      /middleware/
    /application/
      /services/
      /mappers/
    /domain/
      /models/
    /infrastructure/
      /repositories/
      /persistence/
  /tests/

/inventory/             # Inventory subdomain (independent application)
  /src/
    /api/
    /application/
    /domain/
    /infrastructure/
  /tests/

/billing/               # Billing subdomain (independent application)
  /src/
    /api/
    /application/
    /domain/
    /infrastructure/
  /tests/
```

**CRITICAL RULES FOR MULTIPLE SUBDOMAINS:**
1. Each subdomain folder = complete DDD application
2. Each subdomain has its own infrastructure copy (no sharing)
3. Subdomains are independent and self-contained
4. Legacy code is split into subdomain-specific implementations
5. One monolithic repository, multiple independent subdomain applications

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
4. ✅ **Analyze Subdomains**: Determine if single or multiple business subdomains exist
5. ✅ **Create Folders**: For multiple subdomains, create root folder for each subdomain
6. ✅ **Copy Infrastructure**: Copy language-specific infrastructure (to root OR each subdomain folder)
7. ✅ **Analyze**: Document all legacy business logic (map to subdomains if multiple)
8. ✅ **Extract**: Create Domain Models with preserved business logic (in appropriate subdomain)
9. ✅ **Create**: Build Entities (WriteEntity, ReadEntity) per subdomain
10. ✅ **Map**: Create Mappers (DTO ↔ Model ↔ Entity) per subdomain
11. ✅ **Service**: Extract business orchestration to Services (preserve workflow) per subdomain
12. ✅ **Repository**: Implement Repositories per subdomain
13. ✅ **Controller**: Create thin Controllers per subdomain
14. ✅ **Verify**: Confirm business logic preserved exactly in all subdomains
15. ✅ **Test**: Migrate and run all tests for all subdomains

---

# Communication with User (REQUIRED)

**BEFORE starting the refactor, you MUST ask the user:**

1. **"What programming language is your legacy code written in? (C#, Java, Python, TypeScript, or other)"**

2. **"Does your legacy codebase contain multiple distinct business subdomains? (e.g., Orders, Inventory, Billing, Users, etc.)"**
   - If YES: **"Please list the subdomain names and their responsibilities"**
   - If NO: Proceed with single-domain refactor

3. **"Are there any specific business rules or calculations that are particularly critical and should be preserved exactly?"**

4. **"Do you want to preserve the existing API endpoints exactly, or is it acceptable to modernize the endpoint structure?"**

5. **"Should I migrate existing tests, or do you want to create new tests for the refactored structure?"**

6. **"Are there any parts of the legacy code that should NOT be migrated (deprecated features, tech debt, etc.)?"**

**Document the answers and use them to guide the refactor.**

**FOR MULTIPLE SUBDOMAINS:**
After identifying subdomains, you MUST:
- Confirm the subdomain list with the user
- Verify which parts of legacy code belong to each subdomain
- Plan the subdomain folder structure before copying infrastructure

---

# Maintenance

## Changelog

- **1.2.0** (2025-11-14): Added mandatory Example* file cleanup step after infrastructure copy - agents must now delete all Example class files (ExampleService, ExampleEntity, ExampleModel, etc.) from all applicable directories (single domain AND all subdomains) to prevent clutter in refactored projects
- **1.1.0** (2025-11-12): Added multi-subdomain support - Agent now detects and handles multiple business subdomains in legacy monolithic applications, creating separate root folders with complete infrastructure copies for each subdomain
- **1.0.0** (2025-11-12): Initial version - Refactor legacy code to DDD REST architecture with business logic preservation

---

# Final Reminders

**THESE ARE REQUIREMENTS, NOT SUGGESTIONS:**

1. ✅ MUST backup all legacy code to `/legacy` folder before any changes
2. ✅ MUST clone oddly-infrastructures repository
3. ✅ MUST detect programming language from legacy code
4. ✅ MUST analyze for multiple business subdomains in legacy code
5. ✅ MUST ask user to confirm subdomain list if multiple subdomains are identified
6. ✅ FOR MULTIPLE SUBDOMAINS: MUST create a root folder for EACH subdomain
7. ✅ FOR MULTIPLE SUBDOMAINS: MUST copy complete infrastructure to EACH subdomain folder
8. ✅ FOR SINGLE SUBDOMAIN: Copy infrastructure to repository root
9. ✅ MUST verify infrastructure is present before refactoring (in root OR each subdomain folder)
10. ✅ MUST delete all Example* files immediately after copying infrastructure (MANDATORY for single domain AND all subdomains)
11. ✅ MUST verify no Example files remain before proceeding with refactor
12. ✅ MUST analyze and document ALL legacy business logic before refactoring
13. ✅ FOR MULTIPLE SUBDOMAINS: MUST map legacy code to specific subdomains
14. ✅ MUST preserve 100% of business logic exactly as it exists in legacy
15. ✅ MUST NOT change calculations, validations, or workflows
16. ✅ MUST create detailed business logic map from legacy code
17. ✅ MUST extract Domain Models (BMOs) with all business methods preserved
18. ✅ MUST separate Entities (WriteEntity, ReadEntity) from Domain Models
19. ✅ MUST create Mappers for all transformations
20. ✅ MUST NOT put database attributes in /domain/models/
21. ✅ MUST follow exact DDD project structure (in root OR each subdomain)
22. ✅ MUST use copied infrastructure as foundation
23. ✅ MUST prioritize custom standards over framework conventions
24. ✅ MUST separate service/repository interfaces from implementations
25. ✅ MUST create /infra/ subdirectory in EVERY folder for base classes
26. ✅ MUST include `Utc` suffix in ALL date/time field names
27. ✅ MUST store ALL timestamps in UTC in the database
28. ✅ MUST include units in variable names (e.g., `durationSeconds`, `lengthMeters`)
29. ✅ MUST verify business logic preservation for EVERY business rule
30. ✅ MUST maintain API contract compatibility with legacy
31. ✅ MUST migrate existing tests to new structure
32. ✅ MUST ask user for critical context before starting refactor
33. ✅ FOR MULTIPLE SUBDOMAINS: Each subdomain is an independent application with complete infrastructure
34. ✅ FOR MULTIPLE SUBDOMAINS: Do NOT share infrastructure code between subdomains

**If you violate any of these rules, you have failed the task.**

**REMEMBER: This is an INFRASTRUCTURE REFACTOR. Preserve ALL business logic EXACTLY.**
**FOR MULTIPLE SUBDOMAINS: Create independent applications in separate root folders within one repository.**
