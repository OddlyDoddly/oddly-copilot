---
name: Oddly's DDD-MVC Backend Agent (Strict Mode)
description: >
  Build REST backends using DDD + MVC with MANDATORY separation of layers and object types.
  These are REQUIREMENTS, not suggestions. Custom standards override ALL language conventions.
goals:
  - Generate services following this architecture with zero deviation
  - All rules are MANDATORY - treat every instruction as a hard requirement
  - Custom standards ALWAYS override language/framework conventions (C#, Java, etc.)
defaults:
  language: typescript
  http_framework: nest-like
  db_engine: postgres
  orm: "lightweight; repository pattern on top"
  queue: none
  cache: none
  test_style: "unit-first; e2e for risky paths"
  api_style: rest
style:
  lines_max: 120
  prefer_vertical_space: true
  comments: concise, technical, no filler
  naming:
    - "Types PascalCase; methods camelCase. REST paths kebab-case."
  commits:
    - "Conventional: feat|fix|refactor|docs|test|build(scope): message"
    - "PR must include: intent, approach, decisions, risks, test plan"
---

# ⚠️ CRITICAL: READ THIS FIRST ⚠️

**THESE ARE MANDATORY REQUIREMENTS, NOT OPTIONAL GUIDELINES**

Every rule in this document is a **MUST** unless explicitly marked optional.
These custom standards OVERRIDE all language/framework conventions (C#, Java, TypeScript, etc.).

**DO NOT:**
- Follow C# standard naming conventions if they conflict with these rules
- Follow Java/Spring conventions if they conflict with these rules  
- Follow any framework's "best practices" that violate these rules
- Assume you know better than these specifications

## 🚨 STEP 0: .gitignore (BEFORE ANY CODE) 🚨

**MANDATORY FIRST STEP - DO THIS BEFORE WRITING ANY CODE:**

1. **CREATE OR UPDATE .gitignore AT PROJECT ROOT** - This is NOT optional
2. **VERIFY .gitignore includes ALL of the following patterns:**
   ```
   # Build outputs (MANDATORY)
   bin/
   obj/
   dist/
   out/
   build/
   target/
   
   # Binaries (MANDATORY) 
   *.dll
   *.exe
   *.so
   *.dylib
   *.pdb
   *.class
   *.pyc
   *.pyo
   *.o
   *.a
   *.lib
   
   # IDE/Editor (MANDATORY)
   .vs/
   .vscode/
   .idea/
   *.suo
   *.user
   *.sln.docstates
   
   # Dependencies (MANDATORY)
   node_modules/
   packages/
   __pycache__/
   *.egg-info/
   
   # OS files (MANDATORY)
   .DS_Store
   Thumbs.db
   Desktop.ini
   
   # Caches (MANDATORY)
   *.cache
   .nuget/
   ```

3. **AFTER creating .gitignore, you MUST:**
   - Verify no bin/ or obj/ directories will be committed
   - Verify no .dll, .exe, .pdb files will be committed
   - If these files exist, they MUST be in .gitignore before any `git add`

**IF YOU COMMIT BINARIES/BUILD ARTIFACTS, YOU HAVE FAILED THE TASK**

**VALIDATION CHECKLIST - READ BEFORE CODING:**

Before writing any code (after .gitignore is ready), you MUST confirm:
- [ ] .gitignore exists and includes bin/, obj/, *.dll, *.exe, *.pdb, .vs/, .idea/
- [ ] I have read the OBJECT TYPES section (BMOs vs Entities vs DTOs)
- [ ] I have read the Naming conventions (Entity suffix vs Model/BMO suffix)
- [ ] I understand: Domain models in /domain/models/ MUST NOT have database attributes
- [ ] I understand: Persistence classes in /infrastructure/persistence/ MUST end with "Entity"
- [ ] I understand: I MUST create Mappers in /application/mappers/ for all transformations
- [ ] I understand: Repositories work with Entities internally, return BMOs externally
- [ ] I understand: These custom standards override C#/Java/framework conventions

---

# What this agent does

- Generates a REST service using DDD + MVC with **strict** layer separation
- Creates minimal vertical slices: DTO ↔ BMO ↔ Entity ↔ Repository ↔ Service ↔ Controller
- Writes small PRs (≈ ≤400 LOC when possible); chains follow-ups for refactors
- Adds tests alongside code; keeps CI green
- **NEVER mixes concerns or skips layers**

---

# ❌ ANTI-PATTERNS (NEVER DO THESE) ❌

The following are **FORBIDDEN**. If you do any of these, you have failed:

❌ **Putting database/ORM attributes on classes in /domain/models/**
   - Example: `[BsonElement]`, `[Column]`, `@Entity`, `data class` with JPA annotations
   - Domain models MUST be pure business logic

❌ **Returning Entity classes from Repository to Service/Controller**
   - Repositories MUST map Entity → BMO before returning

❌ **Skipping the Mapper layer**
   - ALL transformations require explicit mappers: DTO ↔ BMO ↔ Entity

❌ **Mixing BMOs and Entities in one class**
   - A class with `[BsonElement]` belongs in /infrastructure/persistence/, NOT /domain/models/

❌ **Following C#/Java standard conventions when they conflict with these rules**
   - Example: C# would put `ImageModel` with Entity Framework attributes in /Models/
   - **WRONG**: We separate ImageModel (BMO) from ImageEntity (persistence)

❌ **Putting business logic in Controllers or Repositories**
   - Controllers: only bind, validate, authorize, map DTOs
   - Repositories: only persistence operations

❌ **Using language framework naming when it conflicts**
   - Example: Entity Framework wants "Models", we want "Entities" suffix
   - **OUR standard wins**, not the framework's

❌ **Putting ServiceException classes in /application/services/**
   - ServiceExceptions MUST go in `/application/errors/` directory
   - Example: `ImageServiceException.cs` MUST be at `/application/errors/ImageServiceException.cs`
   - **WRONG**: `/application/services/ImageServiceException.cs`
   - **CORRECT**: `/application/errors/ImageServiceException.cs`

❌ **Committing build artifacts, binaries, or IDE files to git**
   - **FORBIDDEN**: bin/, obj/, *.dll, *.exe, *.pdb, .vs/, .idea/, node_modules/
   - .gitignore MUST exist BEFORE any code generation
   - If build artifacts appear in PR, you have FAILED

❌ **Mixing interfaces and implementations in the same directory**
   - Interfaces MUST be in parent directory (e.g., `/application/services/`)
   - Implementations MUST be in `/impl` subdirectory (e.g., `/application/services/impl/`)
   - **WRONG**: `OrderService.cs` and `IOrderService.cs` both in `/application/services/`
   - **CORRECT**: `IOrderService.cs` in `/application/services/`, `OrderService.cs` in `/application/services/impl/`

---

# Architectural rules (MANDATORY)

## LAYERS (outer → inner):

**MUST follow this exact flow:**

1. **Web Controller Layer (HTTP)**: 
   - Controllers/routers ONLY
   - NO business logic allowed
   - Responsibilities: Bind, validate, authorize, map DTOs

2. **Application/Service Layer**: 
   - Orchestrates use-cases
   - Transaction boundaries
   - Policies
   - Calls repositories and domain services
   - MUST NOT contain persistence logic

3. **Domain (Business Logic) Layer**: 
   - Business Model Objects (BMOs) + domain services
   - Pure logic, side-effect free where possible
   - MUST NOT have persistence annotations
   - MUST NOT reference database types

4. **Data Access Layer**: 
   - Repositories ONLY
   - Persistence logic
   - Entities map to tables/documents
   - NO business logic allowed
   - Maps Entity ↔ BMO internally

## OBJECT TYPES (MANDATORY strict separation):

**DECISION TREE - Use this for EVERY class you create:**

### When creating a new class, ask:

1. **Is this for HTTP transport?** 
   → `/api/dto/` - suffix: Request | Response | Dto

2. **Does this contain business logic/behavior?** 
   → `/domain/models/` - suffix: Model | BMO
   → MUST NOT have `[BsonElement]`, `[Column]`, or any ORM attributes

3. **Does this map to a database table/document?** 
   → `/infrastructure/persistence/` - suffix: Entity
   → MUST have ORM/database attributes
   → MUST NOT have business methods

4. **Does this transform between types?** 
   → `/application/mappers/` - suffix: Mapper

**Type Definitions:**

- **DTOs (Data Transfer Objects)**: 
  - MUST: Transport-only
  - MUST: Live in /api/dto/
  - MUST NOT: Be used as BMOs or Entities
  - MUST NOT: Contain business logic

- **BMOs (Business Model Objects)**: 
  - MUST: Contain behavior + invariants
  - MUST: Live in /domain/models/
  - MUST: Have suffix "Model" or "BMO"
  - MUST NOT: Have persistence concerns
  - MUST NOT: Have database attributes ([BsonElement], [Column], etc.)
  - Methods allowed and encouraged

- **Entities (Persistence Models)**: 
  - MUST: Represent persistence shape ONLY
  - MUST: Live in /infrastructure/persistence/
  - MUST: Have suffix "Entity"
  - MUST: Have ORM/database attributes
  - MUST NOT: Have business behavior beyond identity/invariants for storage
  - MUST NOT: Have business methods

- **Mappers**: 
  - MUST: Explicit transformers between DTO ↔ BMO ↔ Entity
  - MUST: Live in /application/mappers/
  - MUST NOT: Use implicit/magic mapping
  - All transformations are explicit

- **Errors**: 
  - MUST: Be typed, service-scoped
  - MUST NOT: Leak internal messages past controllers

## RULES (MANDATORY):

- MUST: Controllers → Services → Repositories → DB. No sideways calls
- MUST NOT: Put business logic in controllers or repositories
- MUST: Validate at the edge (DTOs). Re-validate invariants inside domain when needed
- MUST: Add Idempotency-Key on mutating endpoints when reasonable
- MUST: Propagate correlation/request IDs across layers and queues

---

# Project structure (MANDATORY - exact paths)

**MUST match this structure exactly:**

```
/src/
  /api/
    /controllers/          # HTTP endpoints (REST). Map DTOs. No business logic
    /dto/                  # Request/Response DTOs; versioned by API version
    /middleware/           # auth, logging, errors, rate limits
  /application/
    /services/             # use-cases; transaction boundaries
    /mappers/              # DTO↔BMO↔Entity transformers (MANDATORY)
    /policies/             # authorization and domain policies
    /errors/               # service-scoped error types
  /domain/
    /models/               # BMOs with behavior; pure logic (NO DB ATTRIBUTES)
    /events/               # domain events (in-process)
  /infrastructure/
    /repositories/         # DB access via repository interfaces
    /persistence/          # Entities with DB attributes, contexts, migrations
    /queues/               # producers/consumers; job schemas
    /integrations/         # external HTTP clients with strict interfaces
    /cache/                # cache client/policies
/tests/
  /unit/                   # domain + services + mappers
  /e2e/                    # controllers via HTTP
/docs/
  /openapi/                # generated openapi.yaml and changelog
  /adrs/                   # architecture decision records
```

**Namespace MUST follow folder structure:**
- /api/controllers/ → Api.Controllers
- /application/services/ → Application.Services
- /domain/models/ → Domain.Models
- /infrastructure/persistence/ → Infrastructure.Persistence

## Interface/Implementation Separation (MANDATORY)

**MUST separate interfaces from implementations:**

- **Interfaces**: Place in the main directory (e.g., `/application/services/`, `/infrastructure/repositories/`)
- **Implementations**: Place in `/impl` subdirectory (e.g., `/application/services/impl/`, `/infrastructure/repositories/impl/`)

**Example structure:**
```
/src/application/
  /services/
    IOrderService.cs              # Interface
    /impl/
      OrderService.cs              # Implementation

/src/infrastructure/
  /repositories/
    IOrderRepository.cs           # Interface
    /impl/
      OrderRepository.cs           # Implementation
```

**Rules:**
- MUST: Interface files in parent directory
- MUST: Implementation files in `/impl` subdirectory
- MUST: Interface names start with `I` (e.g., `IOrderService`, `IOrderRepository`)
- MUST: Implementation names match interface without `I` (e.g., `OrderService`, `OrderRepository`)
- MUST NOT: Mix interfaces and implementations in the same directory
- MUST: Follow this pattern for Services, Repositories, and any other abstraction layers

**C# Example:**
```csharp
// /application/services/IOrderService.cs
namespace Application.Services
{
    public interface IOrderService
    {
        Task<OrderModel> CreateOrderAsync(CreateOrderRequest p_request);
    }
}

// /application/services/impl/OrderService.cs
namespace Application.Services.Impl
{
    public class OrderService : IOrderService
    {
        public async Task<OrderModel> CreateOrderAsync(CreateOrderRequest p_request)
        {
            // Implementation
        }
    }
}
```

**TypeScript Example:**
```typescript
// /application/services/IOrderService.ts
export interface IOrderService {
  createOrderAsync(request: CreateOrderRequest): Promise<OrderModel>;
}

// /application/services/impl/OrderService.ts
import { IOrderService } from '../IOrderService';

export class OrderService implements IOrderService {
  async createOrderAsync(request: CreateOrderRequest): Promise<OrderModel> {
    // Implementation
  }
}
```

---

# Naming conventions (MANDATORY - override language defaults)

**These rules OVERRIDE C#/Java/TypeScript standard conventions:**

- MUST: Controllers end with: `Controller`
- MUST: Services end with: `Service`
- MUST: Repositories end with: `Repository`
- MUST: Entities (persistence) end with: `Entity`
- MUST: Business models end with: `Model` or `BMO`
- MUST: DTOs end with: `Request` | `Response` | `Dto`
- MUST: Async functions end with `Async` (e.g., `FindOrderByIdAsync`)
- MUST: Repository methods describe intent verbosely:
  - `FindObjectByIdAndNameAsync`, `DeleteObjectByIdAsync`, `ListOrdersByStatusAndDateRangeAsync`

**Member fields:**
- MUST (default): `_variable` (underscore prefix)
- MUST (C++ or langs that disallow underscore): `m_variable`

**Parameters:**
- MUST: `p_variable` (p_ prefix)

**Local variables:**
- MUST: `camelCase`

**Example (CORRECT):**
```csharp
// /domain/models/ImageModel.cs - NO database attributes
namespace Domain.Models
{
    public class ImageModel 
    {
        private string _id;
        public string Id => _id;
        
        public void ValidateChecksum(string p_checksum) 
        {
            // business logic here
        }
    }
}

// /infrastructure/persistence/ImageEntity.cs - WITH database attributes
namespace Infrastructure.Persistence
{
    [BsonCollection("images")]
    public class ImageEntity 
    {
        [BsonId]
        [BsonRepresentation(BsonType.ObjectId)]
        public string Id { get; set; }
        
        [BsonElement("checksum")]
        public string Checksum { get; set; }
    }
}

// /application/mappers/ImageMapper.cs - Explicit transformations
namespace Application.Mappers
{
    public class ImageMapper 
    {
        public ImageModel ToModel(ImageEntity p_entity) { /* ... */ }
        public ImageEntity ToEntity(ImageModel p_model) { /* ... */ }
    }
}
```

---

# Abstract layer types (MANDATORY implementation)

**MUST implement these exact patterns:**

### Entity Pattern:
```
// Persistence-only representation
// MUST: Live in /infrastructure/persistence/
// MUST: Have suffix "Entity"
// MUST NOT: Have business behavior
abstract class BaseEntity {
  id: Id
  version?: number
  protected constructor(id) { this.id = id }
}
```

### BMO Pattern:
```
// Business Model Object
// MUST: Live in /domain/models/
// MUST: Have suffix "Model" or "BMO"
// MUST NOT: Have database attributes
abstract class BaseModel { 
  /* business-only fields + behavior */ 
  /* methods encouraged */
}
```

### DTO Pattern:
```
// Transport only
// MUST: Live in /api/dto/
// MUST NOT: Be used as BMOs or Entities
abstract class BaseRequestDto { 
  /* input; validation annotations allowed */ 
}
abstract class BaseResponseDto { 
  /* output; never leak internal fields */ 
}
```

### Repository Pattern:
```
// MUST: Map Entity ↔ BMO internally
// MUST: Return BMO to service layer, NOT Entity
interface IRepository<TModel, TId> {
  SaveAsync(model: TModel): Promise<TId>
  FindByIdAsync(id: TId): Promise<TModel|null>  // Returns BMO, not Entity!
  ListByFilterAsync(filter): Promise<TModel[]>
}
```

### Mapper Pattern (MANDATORY):
```
// MUST: Explicit transformations
// MUST: Live in /application/mappers/
interface IMapper<TDto, TModel, TEntity> {
  ToModelFromRequest(dto: TDto): TModel
  ToResponseFromModel(model: TModel): BaseResponseDto
  ToEntity(model: TModel): TEntity
  ToModel(entity: TEntity): TModel
}
```

---

# Service Exception policy (MANDATORY enum-based)

**ALL service-level exceptions MUST follow this pattern:**

- MUST: Follow pattern `{ObjectName}ServiceException`
- MUST: Define own `{ObjectName}ErrorCode` enum (NOT strings)
- MUST: Inherit from `ServiceException<TErrorCode>`
- MUST: Place ALL exception classes in `/application/errors/` directory (NOT in `/application/services/`)
- MUST: Services throw typed exceptions; controllers ONLY map to HTTP
- MUST NOT: Repositories throw ServiceExceptions
- MUST: Message text from templates dictionary keyed by enum

**C# Base Template (MANDATORY):**
```csharp
namespace Application.Errors
{
    // Base class for catching all service exceptions
    public abstract class ServiceException : Exception
    {
        internal sealed class ServiceExceptionInheritanceLock { }

        public abstract Enum GenericErrorCode { get; }
        public abstract Type ErrorCodeType { get; }

        private protected ServiceException(
            string message, 
            ServiceExceptionInheritanceLock _
        ) : base(message) { }
    }

    // Generic typed base class
    public abstract class ServiceException<TErrorCode> : ServiceException
        where TErrorCode : Enum
    {
        public readonly TErrorCode ErrorCode;
        public readonly IReadOnlyDictionary<string, object>? Details;

        public override Enum GenericErrorCode => ErrorCode;
        public override Type ErrorCodeType => typeof(TErrorCode);

        protected ServiceException(
            TErrorCode p_code,
            IReadOnlyDictionary<string, string> p_messageTemplates,
            IReadOnlyDictionary<string, object>? p_details = null
        ) : base(FormatMessage(p_code, p_messageTemplates, p_details), new ServiceExceptionInheritanceLock())
        {
            ErrorCode = p_code;
            Details = p_details;
        }

        private static string FormatMessage(
            TErrorCode p_code,
            IReadOnlyDictionary<string, string> p_templates,
            IReadOnlyDictionary<string, object>? p_details
        )
        {
            var template = p_templates.TryGetValue(p_code.ToString(), out var msg) 
                ? msg 
                : "Unknown error";
            
            if (p_details == null) return template;
            
            foreach (var kvp in p_details)
            {
                template = template.Replace($"{{{kvp.Key}}}", kvp.Value?.ToString() ?? "");
            }
            return template;
        }
    }
}
```

**Usage Example (MANDATORY pattern):**
```csharp
// /application/errors/ImageServiceException.cs
public enum ImageErrorCode
{
    NotFound,
    ValidationFailed,
    Conflict
}

public class ImageServiceException : ServiceException<ImageErrorCode>
{
    private static readonly IReadOnlyDictionary<string, string> _messageTemplates = 
        new Dictionary<string, string>
        {
            { nameof(ImageErrorCode.NotFound), "Image '{id}' was not found" },
            { nameof(ImageErrorCode.ValidationFailed), "Validation failed: {reason}" },
            { nameof(ImageErrorCode.Conflict), "Image '{id}' already exists" }
        };

    public ImageServiceException(
        ImageErrorCode p_code, 
        IReadOnlyDictionary<string, object>? p_details = null
    ) : base(p_code, _messageTemplates, p_details) { }
}
```

---

# Filesystem contract (MANDATORY - no deviations)

**Root MUST be:** `/src/`

**All paths MUST match exactly:**
- `/src/api/controllers/`
- `/src/api/dto/`
- `/src/api/middleware/`
- `/src/application/services/`
- `/src/application/mappers/` (MANDATORY - never skip)
- `/src/application/policies/`
- `/src/application/errors/`
- `/src/domain/models/` (BMOs only - NO database attributes)
- `/src/domain/events/`
- `/src/infrastructure/repositories/`
- `/src/infrastructure/persistence/` (Entities only - WITH database attributes)
- `/src/infrastructure/queues/`
- `/src/infrastructure/integrations/`
- `/src/infrastructure/cache/`
- `/tests/unit/`
- `/tests/e2e/`
- `/docs/openapi/`
- `/docs/adrs/`

---

# Pre-flight checklist (MANDATORY before any code)

**BEFORE creating any Model/Entity/DTO, answer these:**

1. **Where does this class live?**
   - Contains business logic? → `/domain/models/` (suffix: Model/BMO)
   - Maps to database? → `/infrastructure/persistence/` (suffix: Entity)
   - For HTTP transport? → `/api/dto/` (suffix: Request/Response/Dto)

2. **Does it have the right suffix?**
   - Business logic class → `Model` or `BMO`
   - Persistence class → `Entity`
   - Transport class → `Request` | `Response` | `Dto`

3. **Does it have database attributes?**
   - YES → MUST be in `/infrastructure/persistence/` with suffix `Entity`
   - NO → MUST be in `/domain/models/` with suffix `Model`

4. **Have I created the mapper?**
   - For every Entity/Model pair → MUST create Mapper in `/application/mappers/`

5. **Am I following custom standards or language standards?**
   - MUST: Follow custom standards (this document)
   - MUST NOT: Follow C#/Java/framework conventions if they conflict

---

# Storage recommendation policy

Same as original (document vs relational rubric)

[Content preserved from original lines 161-204]

---

# Scaffolding plan

**MANDATORY: Print plan BEFORE any edits**

Must include:
1. Summary: stack + rationale (≤8 lines)
2. File tree: new/changed paths ONLY
3. Per-file intent: 1–2 lines each
4. DI/wiring notes
5. Config/env additions
6. Test plan outline
7. Rollback plan

**RELATIONAL scaffold (MUST follow):**
```
/src/infrastructure/persistence/
  - PostgresDbContext.(cs|ts|py)
  - migrations/
  - entities/
    - <Feature>Entity.(cs|ts|py)  # MUST have suffix "Entity"

/src/domain/models/
  - <Feature>Model.(cs|ts|py)      # MUST have suffix "Model", NO DB attributes

/src/application/mappers/
  - <Feature>Mapper.(cs|ts|py)     # MANDATORY - never skip

/src/application/services/
  - I<Feature>Service.(cs|ts|py)   # Interface in parent directory
  - /impl/
    - <Feature>Service.(cs|ts|py)  # Implementation in /impl subdirectory

/src/infrastructure/repositories/
  - I<Feature>Repository.(cs|ts|py)  # Interface in parent directory
  - /impl/
    - <Feature>Repository.(cs|ts|py)  # Implementation in /impl subdirectory

/src/api/dto/v1/
  - <Feature>Request.(cs|ts|py)
  - <Feature>Response.(cs|ts|py)

/src/api/controllers/
  - <Feature>Controller.(cs|ts|py)
```

**DOCUMENT scaffold (MUST follow):**
```
/src/infrastructure/persistence/
  - MongoDbContext.(cs|ts|py)
  - entities/
    - <Feature>Entity.(cs|ts|py)  # MUST have [BsonElement] attributes

/src/domain/models/
  - <Feature>Model.(cs|ts|py)      # NO [BsonElement] attributes

[Rest same as relational]
```

---

# Final reminders

**THESE ARE REQUIREMENTS, NOT SUGGESTIONS:**

1. ✅ MUST separate BMOs (domain) from Entities (persistence)
2. ✅ MUST create Mappers for all transformations
3. ✅ MUST NOT put database attributes in /domain/models/
4. ✅ MUST use Entity suffix for persistence classes
5. ✅ MUST use Model suffix for domain classes
6. ✅ MUST follow the exact filesystem structure
7. ✅ MUST prioritize these custom standards over C#/Java/framework conventions
8. ✅ MUST separate interfaces from implementations (interfaces in parent, implementations in /impl)

**If you violate any of these rules, you have failed the task.**

---

[REST OF ORIGINAL CONTENT PRESERVED: Authentication, Queues, Cache, Observability, etc.]
