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

❌ **Mixing ReadEntities and WriteEntities or putting them in wrong directories**
   - WriteEntities MUST be in `/infrastructure/persistence/write/`
   - ReadEntities MUST be in `/infrastructure/persistence/read/`
   - **WRONG**: `OrderEntity.cs` in `/infrastructure/persistence/`
   - **CORRECT**: `OrderWriteEntity.cs` in `/infrastructure/persistence/write/`, `OrderReadEntity.cs` in `/infrastructure/persistence/read/`

❌ **Using WriteEntities for queries or ReadEntities for commands**
   - WriteEntities are ONLY for commands (create, update, delete)
   - ReadEntities are ONLY for queries (read operations)
   - **WRONG**: Repository.FindById() returning WriteEntity
   - **CORRECT**: Query repos return ReadEntity, Command repos work with WriteEntity internally and return BMO

❌ **Not separating Command and Query repositories**
   - MUST have separate ICommandRepository and IQueryRepository
   - Command repos work with WriteEntities internally
   - Query repos return ReadEntities directly
   - **WRONG**: Single repository with mixed read/write methods
   - **CORRECT**: Separate CommandRepository and QueryRepository with clear responsibilities

❌ **Manual transaction management in services or controllers**
   - UnitOfWork middleware MUST handle all transactions
   - Services MUST NOT call BeginTransaction/Commit/Rollback directly
   - **WRONG**: Service manually managing transactions
   - **CORRECT**: UnitOfWork middleware wraps entire request/message

❌ **Skipping ownership/security checks**
   - MUST verify user owns resource before access (unless explicitly public)
   - Ownership middleware MUST execute before business logic
   - **WRONG**: Controller directly accessing any resource by ID
   - **CORRECT**: Middleware verifies ownership first

❌ **Domain events without proper naming convention**
   - MUST suffix with `Event` using pattern `{ObjectName}{EventName}Event`
   - **WRONG**: `OrderCreation`, `UserRegistered`, `PaymentEvent`
   - **CORRECT**: `OrderCreatedEvent`, `UserRegisteredEvent`, `PaymentProcessedEvent`

❌ **Making HTTP/REST calls between subdomains (backend services)**
   - REST APIs are ONLY for front-end interactions
   - Subdomain-to-subdomain communication MUST use domain events via message queue
   - **WRONG**: Payment service calling Order service's REST endpoint
   - **CORRECT**: Payment service subscribing to OrderCreatedEvent from message queue

❌ **Direct database access across subdomain boundaries**
   - Each subdomain MUST have its own database
   - MUST NOT query another subdomain's database directly
   - **WRONG**: Order service reading from Payment service's database
   - **CORRECT**: Order service subscribing to PaymentProcessedEvent

❌ **Returning non-standard HTTP error responses**
   - MUST follow standard error response contract with code, message, details, timestamp, path, requestId
   - MUST map ServiceException error codes to appropriate HTTP status codes
   - **WRONG**: Returning plain text error or custom format
   - **CORRECT**: Consistent ErrorResponse structure with proper HTTP status

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

## CQRS Pattern (MANDATORY - Separate Read/Write):

**MUST implement Command-Query Responsibility Segregation:**

### WriteEntity (Commands - Create, Update, Delete):
- MUST: Live in `/infrastructure/persistence/write/`
- MUST: Suffix with `WriteEntity`
- MUST: Used when business logic executes against data
- MUST: Contains all fields needed for business operations
- MUST: Updated by Command repositories
- Purpose: Transactional writes, data consistency, business rules

### ReadEntity (Queries - Read Operations):
- MUST: Live in `/infrastructure/persistence/read/`
- MUST: Suffix with `ReadEntity`
- MUST: Pre-rendered views optimized for front-end consumption
- MUST: Denormalized for query performance
- MAY: Aggregate data from multiple WriteEntities
- MAY: Point to materialized views, read replicas, or separate collections
- Purpose: Fast queries, optimized for specific UI needs

### Repository Separation:
- **CommandRepository**: 
  - Works with WriteEntity internally
  - Receives BMO from service
  - Maps BMO → WriteEntity
  - Returns void or ID after persistence
  - Handles: Create, Update, Delete operations

- **QueryRepository**: 
  - Returns ReadEntity directly to service
  - Service maps ReadEntity → Response DTO
  - No BMO involvement in read path (optimization)
  - Handles: Read operations, searches, lists

### Benefits:
1. **Performance**: Read models optimized independently from write models
2. **Scalability**: Separate read/write databases possible
3. **Flexibility**: Different schemas for different purposes
4. **Front-end Optimization**: ReadEntities pre-rendered for UI needs

## OBJECT TYPES (MANDATORY strict separation):

**DECISION TREE - Use this for EVERY class you create:**

### When creating a new class, ask:

1. **Is this for HTTP transport?** 
   → `/api/dto/` - suffix: Request | Response | Dto

2. **Does this contain business logic/behavior?** 
   → `/domain/models/` - suffix: Model | BMO
   → MUST NOT have `[BsonElement]`, `[Column]`, or any ORM attributes

3. **Does this map to a database table/document?** 
   → MUST determine if it's for Write or Read operations:
   - **For Write Operations (Commands)**:
     → `/infrastructure/persistence/write/` - suffix: WriteEntity
     → Used when business logic executes against it
     → Contains all fields for business operations
   - **For Read Operations (Queries)**:
     → `/infrastructure/persistence/read/` - suffix: ReadEntity
     → Pre-rendered views for front-end
     → Denormalized, optimized for queries
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
  - MUST: Have ORM/database attributes
  - MUST NOT: Have business behavior beyond identity/invariants for storage
  - MUST NOT: Have business methods
  - **TWO TYPES - CQRS Pattern**:
    - **WriteEntity**: For commands (create, update, delete)
      - MUST: Have suffix "WriteEntity"
      - MUST: Used for business logic execution and database writes
      - MUST: Live in `/infrastructure/persistence/write/`
      - Contains all fields needed for business operations
    - **ReadEntity**: For queries (read operations)
      - MUST: Have suffix "ReadEntity"
      - MUST: Pre-rendered views optimized for front-end consumption
      - MUST: Live in `/infrastructure/persistence/read/`
      - Denormalized, optimized for specific query patterns
      - May combine data from multiple write entities

- **Mappers**: 
  - MUST: Explicit transformers between DTO ↔ BMO ↔ Entity
  - MUST: Live in /application/mappers/
  - MUST NOT: Use implicit/magic mapping
  - All transformations are explicit

- **Errors**: 
  - MUST: Be typed, service-scoped
  - MUST NOT: Leak internal messages past controllers

- **Domain Events**: 
  - MUST: Represent significant domain occurrences
  - MUST: Live in `/domain/events/`
  - MUST: Have suffix `Event` with pattern: `{ObjectName}{EventName}Event`
  - MUST: Be immutable (all fields readonly/final)
  - MUST NOT: Contain business logic
  - Examples: `OrderCreatedEvent`, `UserRegisteredEvent`, `PaymentProcessedEvent`
  - Used for: Cross-subdomain communication via message queue
  
- **Queue Interfaces**: 
  - MUST: Abstract queue implementation details
  - MUST: Live in `/infrastructure/queues/`
  - MUST: Follow observer pattern for event subscriptions
  - MUST: Use `IEventPublisher` and `IEventSubscriber` interfaces
  - Each subdomain (repository) publishes and subscribes to domain events
  - Fan-out topics for multiple subscribers

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
    /middleware/           # Security, UnitOfWork, auth, logging, errors, rate limits
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
      /write/              # WriteEntities for commands (business logic execution)
      /read/               # ReadEntities for queries (pre-rendered views)
    /queues/               # Event publisher/subscriber; domain event handlers
      /middleware/         # Queue-specific middleware (UnitOfWork for messages)
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
- MUST: Write entities (commands) end with: `WriteEntity`
- MUST: Read entities (queries) end with: `ReadEntity`
- MUST: Business models end with: `Model` or `BMO`
- MUST: DTOs end with: `Request` | `Response` | `Dto`
- MUST: Domain events end with: `Event` using pattern `{ObjectName}{EventName}Event`
  - Examples: `OrderCreatedEvent`, `UserRegisteredEvent`, `PaymentProcessedEvent`
- MUST: Middleware end with: `Middleware`
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

// /infrastructure/persistence/write/ImageWriteEntity.cs - FOR WRITES (Commands)
namespace Infrastructure.Persistence.Write
{
    [BsonCollection("images")]
    public class ImageWriteEntity 
    {
        [BsonId]
        [BsonRepresentation(BsonType.ObjectId)]
        public string Id { get; set; }
        
        [BsonElement("checksum")]
        public string Checksum { get; set; }
        
        [BsonElement("created_at")]
        public DateTime CreatedAt { get; set; }
        
        [BsonElement("updated_at")]
        public DateTime UpdatedAt { get; set; }
        
        // All fields needed for business operations
    }
}

// /infrastructure/persistence/read/ImageReadEntity.cs - FOR READS (Queries)
namespace Infrastructure.Persistence.Read
{
    [BsonCollection("images_view")]  // May be a materialized view or separate collection
    public class ImageReadEntity 
    {
        [BsonId]
        [BsonRepresentation(BsonType.ObjectId)]
        public string Id { get; set; }
        
        [BsonElement("checksum")]
        public string Checksum { get; set; }
        
        [BsonElement("url")]
        public string Url { get; set; }
        
        [BsonElement("thumbnail_url")]
        public string ThumbnailUrl { get; set; }
        
        // Pre-rendered/denormalized fields optimized for front-end
        // May include data from multiple write entities
    }
}

// /application/mappers/ImageMapper.cs - Explicit transformations
namespace Application.Mappers
{
    public class ImageMapper 
    {
        // Map between BMO and WriteEntity for commands
        public ImageModel ToModel(ImageWriteEntity p_entity) { /* ... */ }
        public ImageWriteEntity ToWriteEntity(ImageModel p_model) { /* ... */ }
        
        // Map ReadEntity to Response DTO for queries
        public ImageResponse ToResponse(ImageReadEntity p_entity) { /* ... */ }
    }
}
```

---

# Abstract layer types (MANDATORY implementation)

**MUST implement these exact patterns:**

### Entity Patterns (CQRS - Separate Read/Write):

#### WriteEntity Pattern:
```
// For commands (create, update, delete)
// MUST: Live in /infrastructure/persistence/write/
// MUST: Have suffix "WriteEntity"
// MUST NOT: Have business behavior
// Used when business logic executes
abstract class BaseWriteEntity {
  id: Id
  version?: number
  createdAt: Date
  updatedAt: Date
  protected constructor(id) { this.id = id }
}
```

#### ReadEntity Pattern:
```
// For queries (read operations)
// MUST: Live in /infrastructure/persistence/read/
// MUST: Have suffix "ReadEntity"
// MUST NOT: Have business behavior
// Pre-rendered views for front-end
// May be denormalized/aggregated from multiple WriteEntities
abstract class BaseReadEntity {
  id: Id
  // Optimized fields for specific query patterns
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

### Repository Pattern (CQRS):
```
// MUST: Separate Command and Query repositories
// MUST: Command repos work with WriteEntities
// MUST: Query repos work with ReadEntities
// MUST: Return BMO to service layer for commands, NOT Entity

// Command Repository (for writes)
interface ICommandRepository<TModel, TId> {
  SaveAsync(model: TModel): Promise<TId>           // Maps BMO → WriteEntity
  UpdateAsync(model: TModel): Promise<void>        // Maps BMO → WriteEntity
  DeleteAsync(id: TId): Promise<void>              // Works with WriteEntity
}

// Query Repository (for reads)
interface IQueryRepository<TId> {
  FindByIdAsync(id: TId): Promise<ReadEntity|null>  // Returns ReadEntity
  ListByFilterAsync(filter): Promise<ReadEntity[]>  // Returns ReadEntity[]
  // These methods return ReadEntity for efficient queries
}
```

### Mapper Pattern (MANDATORY with Read/Write):
```
// MUST: Explicit transformations
// MUST: Live in /application/mappers/
interface IMapper<TDto, TModel, TWriteEntity, TReadEntity> {
  // Command path: Request → Model → WriteEntity
  ToModelFromRequest(dto: TDto): TModel
  ToWriteEntity(model: TModel): TWriteEntity
  
  // Query path: ReadEntity → Response
  ToResponseFromReadEntity(entity: TReadEntity): BaseResponseDto
  
  // Command response: Model → Response (after write)
  ToResponseFromModel(model: TModel): BaseResponseDto
}
```

---

# Middleware Patterns (MANDATORY)

**MUST implement these middleware layers in order:**

## 1. Security/Authorization Middleware (MANDATORY)

**MUST check object ownership before allowing access:**

- MUST: Verify user owns or has permission to access resource
- MUST: Execute BEFORE controller logic
- MUST: Live in `/api/middleware/`
- MUST: Use claims/tokens to identify current user
- MUST: Check ownership against resource in database
- MUST NOT: Allow access to resources user doesn't own unless explicitly public

**Pattern:**
```typescript
// /api/middleware/OwnershipMiddleware.ts
export class OwnershipMiddleware {
  async execute(request: Request, next: Next) {
    const userId = extractUserId(request);
    const resourceId = request.params.id;
    
    // Check if resource is public
    if (await isPublicResource(resourceId)) {
      return next();
    }
    
    // Verify ownership
    if (!await verifyOwnership(userId, resourceId)) {
      throw new ForbiddenException('Access denied');
    }
    
    return next();
  }
}
```

**C# Pattern:**
```csharp
// /api/middleware/OwnershipMiddleware.cs
public class OwnershipMiddleware
{
    public async Task InvokeAsync(HttpContext p_context, RequestDelegate p_next)
    {
        var userId = ExtractUserId(p_context);
        var resourceId = p_context.Request.RouteValues["id"];
        
        // Check if resource is public
        if (await IsPublicResourceAsync(resourceId))
        {
            await p_next(p_context);
            return;
        }
        
        // Verify ownership
        if (!await VerifyOwnershipAsync(userId, resourceId))
        {
            throw new ForbiddenException("Access denied");
        }
        
        await p_next(p_context);
    }
}
```

## 2. Unit of Work Middleware (MANDATORY)

**MUST encapsulate each request/queue message in a single transaction:**

- MUST: Start transaction at request/message start
- MUST: Commit transaction on success
- MUST: Rollback transaction on failure
- MUST: Live in `/api/middleware/` (for HTTP) and `/infrastructure/queues/middleware/` (for queue messages)
- MUST: Execute AFTER security middleware
- MUST NOT: Require manual transaction management in services/repositories
- Repositories use `Save()` functions, but transaction spans entire request

**HTTP Request Pattern:**
```typescript
// /api/middleware/UnitOfWorkMiddleware.ts
export class UnitOfWorkMiddleware {
  constructor(private _unitOfWork: IUnitOfWork) {}
  
  async execute(request: Request, response: Response, next: Next) {
    try {
      await this._unitOfWork.beginTransaction();
      
      await next();
      
      if (response.statusCode < 400) {
        await this._unitOfWork.commit();
      } else {
        await this._unitOfWork.rollback();
      }
    } catch (error) {
      await this._unitOfWork.rollback();
      throw error;
    }
  }
}
```

**Queue Message Pattern:**
```typescript
// /infrastructure/queues/middleware/QueueUnitOfWorkMiddleware.ts
export class QueueUnitOfWorkMiddleware {
  constructor(private _unitOfWork: IUnitOfWork) {}
  
  async execute(message: QueueMessage, handler: MessageHandler) {
    try {
      await this._unitOfWork.beginTransaction();
      
      await handler(message);
      
      await this._unitOfWork.commit();
      await message.ack(); // Acknowledge message
    } catch (error) {
      await this._unitOfWork.rollback();
      await message.nack(); // Negative acknowledgment
      throw error;
    }
  }
}
```

**C# Pattern:**
```csharp
// /api/middleware/UnitOfWorkMiddleware.cs
public class UnitOfWorkMiddleware
{
    private readonly IUnitOfWork _unitOfWork;
    
    public UnitOfWorkMiddleware(IUnitOfWork p_unitOfWork)
    {
        _unitOfWork = p_unitOfWork;
    }
    
    public async Task InvokeAsync(HttpContext p_context, RequestDelegate p_next)
    {
        try
        {
            await _unitOfWork.BeginTransactionAsync();
            
            await p_next(p_context);
            
            if (p_context.Response.StatusCode < 400)
            {
                await _unitOfWork.CommitAsync();
            }
            else
            {
                await _unitOfWork.RollbackAsync();
            }
        }
        catch
        {
            await _unitOfWork.RollbackAsync();
            throw;
        }
    }
}
```

**Unit of Work Interface:**
```typescript
// /application/persistence/IUnitOfWork.ts
export interface IUnitOfWork {
  beginTransaction(): Promise<void>;
  commit(): Promise<void>;
  rollback(): Promise<void>;
  saveChanges(): Promise<void>; // Called by repositories
}
```

## Middleware Order (MANDATORY)

**MUST apply in this exact order:**

1. **Correlation ID** - Generate/extract request ID
2. **Logging** - Log request start
3. **Authentication** - Verify identity
4. **Authorization/Ownership** - Verify permissions and ownership
5. **Unit of Work** - Begin transaction
6. **Controller** - Execute business logic
7. **Unit of Work** - Commit/rollback transaction
8. **Error Handling** - Catch and format errors
9. **Logging** - Log request end

**Pipeline Configuration Example:**
```typescript
app.use(correlationIdMiddleware);
app.use(loggingMiddleware);
app.use(authenticationMiddleware);
app.use(ownershipMiddleware);      // NEW: Check ownership
app.use(unitOfWorkMiddleware);     // NEW: Manage transactions
app.use(errorHandlingMiddleware);
app.use(controllers);
```

## Public Resources Pattern

**For resources that are publicly accessible:**

```typescript
// Mark resources as public in metadata
@Public() // Decorator
@Get('/posts/:id')
async getPost(id: string) {
  // Ownership check skipped
}

// Or check in service
class PostService {
  async getPostById(id: string, userId?: string) {
    const post = await this._repository.findById(id);
    
    if (!post.isPublic && post.authorId !== userId) {
      throw new ForbiddenException();
    }
    
    return post;
  }
}
```

---

# Domain Events & Queue Abstraction (MANDATORY)

**MUST implement observer pattern for cross-subdomain communication:**

## Domain Event Rules:

- MUST: Suffix with `Event` using pattern: `{ObjectName}{EventName}Event`
- MUST: Be immutable (readonly/final fields)
- MUST: Live in `/domain/events/`
- MUST: Contain only data, no behavior
- MUST: Include timestamp and correlation ID
- Examples: `OrderCreatedEvent`, `UserRegisteredEvent`, `PaymentProcessedEvent`

**Domain Event Pattern:**
```typescript
// /domain/events/OrderCreatedEvent.ts
export class OrderCreatedEvent {
  readonly eventId: string;
  readonly orderId: string;
  readonly userId: string;
  readonly timestamp: Date;
  readonly correlationId: string;
  
  constructor(data: OrderCreatedEventData) {
    this.eventId = generateId();
    this.orderId = data.orderId;
    this.userId = data.userId;
    this.timestamp = new Date();
    this.correlationId = data.correlationId;
    
    Object.freeze(this); // Immutable
  }
}
```

**C# Pattern:**
```csharp
// /domain/events/OrderCreatedEvent.cs
namespace Domain.Events
{
    public sealed class OrderCreatedEvent
    {
        public string EventId { get; }
        public string OrderId { get; }
        public string UserId { get; }
        public DateTime Timestamp { get; }
        public string CorrelationId { get; }
        
        public OrderCreatedEvent(
            string p_orderId,
            string p_userId,
            string p_correlationId
        )
        {
            EventId = Guid.NewGuid().ToString();
            OrderId = p_orderId;
            UserId = p_userId;
            Timestamp = DateTime.UtcNow;
            CorrelationId = p_correlationId;
        }
    }
}
```

## Queue Abstraction (Observer Pattern):

**MUST use these interfaces:**

```typescript
// /infrastructure/queues/IEventPublisher.ts
export interface IEventPublisher {
  publish<TEvent>(event: TEvent, topic: string): Promise<void>;
  publishBatch<TEvent>(events: TEvent[], topic: string): Promise<void>;
}

// /infrastructure/queues/IEventSubscriber.ts
export interface IEventSubscriber {
  subscribe<TEvent>(
    topic: string, 
    handler: (event: TEvent) => Promise<void>
  ): Promise<void>;
  
  unsubscribe(topic: string): Promise<void>;
}

// /infrastructure/queues/impl/EventBus.ts
export class EventBus implements IEventPublisher, IEventSubscriber {
  // Implementation uses RabbitMQ, Kafka, etc.
  // Topics fan out to multiple subscribers
}
```

**C# Pattern:**
```csharp
// /infrastructure/queues/IEventPublisher.cs
public interface IEventPublisher
{
    Task PublishAsync<TEvent>(TEvent p_event, string p_topic);
    Task PublishBatchAsync<TEvent>(IEnumerable<TEvent> p_events, string p_topic);
}

// /infrastructure/queues/IEventSubscriber.cs
public interface IEventSubscriber
{
    Task SubscribeAsync<TEvent>(
        string p_topic, 
        Func<TEvent, Task> p_handler
    );
    Task UnsubscribeAsync(string p_topic);
}
```

## Subdomain Communication Pattern:

**Each repository/subdomain:**

1. **Publishes** domain events when state changes
2. **Subscribes** to events from other subdomains
3. **Reacts** to events asynchronously

**Example:**
```typescript
// Order subdomain publishes event
class OrderService {
  async createOrder(request: CreateOrderRequest) {
    const order = await this._repository.save(orderModel);
    
    // Publish event for other subdomains
    await this._eventPublisher.publish(
      new OrderCreatedEvent({ 
        orderId: order.id,
        userId: order.userId,
        correlationId: this._correlationId 
      }),
      'order.created'
    );
    
    return order;
  }
}

// Inventory subdomain subscribes to event
class InventoryEventSubscriber {
  constructor(private _eventSubscriber: IEventSubscriber) {
    this.setupSubscriptions();
  }
  
  private setupSubscriptions() {
    this._eventSubscriber.subscribe<OrderCreatedEvent>(
      'order.created',
      async (event) => {
        await this.handleOrderCreated(event);
      }
    );
  }
  
  private async handleOrderCreated(event: OrderCreatedEvent) {
    // Reserve inventory for the order
    await this._inventoryService.reserveItems(event.orderId);
  }
}
```

## Topic Naming Convention:

- MUST: Use format `{subdomain}.{action}` (e.g., `order.created`, `payment.processed`)
- MUST: Be lowercase with dots as separators
- MUST: Be past tense for events that happened
- Examples: `order.created`, `user.registered`, `payment.failed`, `inventory.reserved`

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

# HTTP Error Response Standards (MANDATORY)

**ALL HTTP error responses MUST follow this contract:**

## Standard Error Response Structure:

```typescript
interface ErrorResponse {
  error: {
    code: string;           // Error code from enum (e.g., "ORDER_NOT_FOUND")
    message: string;        // Human-readable message
    details?: object;       // Optional structured details
    timestamp: string;      // ISO 8601 timestamp
    path: string;          // Request path that caused the error
    requestId: string;     // Correlation/request ID for tracing
  }
}
```

## HTTP Status Code Mapping (MANDATORY):

- MUST: Controllers catch ServiceException and map to appropriate HTTP status:
  - `NotFound` errors → `404 Not Found`
  - `Conflict` errors → `409 Conflict`
  - `ValidationFailed` errors → `400 Bad Request`
  - `Unauthorized` errors → `401 Unauthorized`
  - `Forbidden` errors → `403 Forbidden`
  - Unknown errors → `500 Internal Server Error`

- MUST NOT: Leak internal error details or stack traces in production
- MUST: Log full error details (including stack trace) server-side
- MUST: Return only safe, user-friendly messages to client

## Example Error Handler (MANDATORY pattern):

```typescript
// /api/middleware/ErrorHandlerMiddleware.ts
export class ErrorHandlerMiddleware {
  async execute(error: Error, request: Request, response: Response, next: Next) {
    const requestId = request.headers['x-request-id'] || generateId();
    
    // Log full error details
    logger.error('Request failed', {
      requestId,
      error: error.message,
      stack: error.stack,
      path: request.path
    });
    
    // Map to HTTP response
    if (error instanceof ServiceException) {
      const statusCode = this.mapErrorCodeToHttpStatus(error.ErrorCode);
      return response.status(statusCode).json({
        error: {
          code: error.ErrorCode.toString(),
          message: error.message,
          details: error.Details,
          timestamp: new Date().toISOString(),
          path: request.path,
          requestId
        }
      });
    }
    
    // Unknown errors - don't leak details
    return response.status(500).json({
      error: {
        code: 'INTERNAL_SERVER_ERROR',
        message: 'An unexpected error occurred',
        timestamp: new Date().toISOString(),
        path: request.path,
        requestId
      }
    });
  }
}
```

## Success Response Standards:

- MUST: Return consistent structure for successful operations
- MUST: Include appropriate HTTP status codes:
  - `200 OK` - Successful read or update
  - `201 Created` - Resource successfully created (include Location header)
  - `204 No Content` - Successful delete or operation with no body
- MUST: Return data in predictable format:

```typescript
// Single resource
interface ResourceResponse<T> {
  data: T;
  metadata?: {
    requestId: string;
    timestamp: string;
  };
}

// Collection
interface CollectionResponse<T> {
  data: T[];
  pagination?: {
    page: number;
    pageSize: number;
    total: number;
    hasMore: boolean;
  };
  metadata?: {
    requestId: string;
    timestamp: string;
  };
}
```

---

# Communication Architecture (MANDATORY - Subdomain Boundaries)

**CRITICAL: Understanding Domain vs Subdomain**

- **Domain**: The entire multi-repository business domain (e.g., e-commerce platform)
- **Subdomain**: Each individual repository/bounded context (e.g., Order service, Payment service, Inventory service)
- **NOTE**: Throughout this document, "domain" often refers to "subdomain" - each repo is one subdomain

## Communication Rules (MANDATORY):

### Rule 1: REST Endpoints are ONLY for Front-End Interactions

- MUST: REST/HTTP endpoints (`/api/controllers/`) are EXCLUSIVELY for front-end (UI) interactions
- MUST NOT: Call REST endpoints from one subdomain to another subdomain
- MUST NOT: Make HTTP calls between backend services
- Purpose: REST APIs are the external boundary for user-facing operations

**CORRECT Front-End Pattern:**
```
Front-End → REST API (Order Service) → Order Domain Logic
```

**WRONG Subdomain-to-Subdomain Pattern:**
```
❌ Order Service → HTTP Request → Payment Service REST API
```

### Rule 2: Subdomain-to-Subdomain Communication via Events ONLY

- MUST: Use domain events published to message queue for all cross-subdomain communication
- MUST: Subscribe to events from other subdomains via queue subscriptions
- MUST NOT: Make synchronous calls between subdomains
- Pattern: Observer pattern with fan-out topics

**CORRECT Subdomain-to-Subdomain Pattern:**
```typescript
// Order Service (Publisher)
class OrderService {
  async createOrder(p_order: OrderModel): Promise<string> {
    // 1. Create order in own database
    const orderId = await this._commandRepo.CreateAsync(p_order);
    
    // 2. Publish event to queue (async, non-blocking)
    await this._eventPublisher.Publish(
      new OrderCreatedEvent({
        orderId,
        customerId: p_order.CustomerId,
        totalAmount: p_order.TotalAmount,
        items: p_order.Items
      })
    );
    
    return orderId;
  }
}

// Payment Service (Subscriber in different repo)
class PaymentEventHandler implements IEventSubscriber<OrderCreatedEvent> {
  async handle(p_event: OrderCreatedEvent): Promise<void> {
    // React to order creation by processing payment
    await this._paymentService.ProcessPaymentAsync({
      orderId: p_event.orderId,
      amount: p_event.totalAmount
    });
  }
}
```

### Rule 3: Within Same Subdomain - Direct Service Calls OK

- MUST: Within the same subdomain (same repository), direct service-to-service calls are allowed
- MUST: Follow layer hierarchy: Controller → Service → Repository
- MUST NOT: Skip layers (e.g., Controller → Repository directly)

**CORRECT Intra-Subdomain Pattern:**
```typescript
// Within Order subdomain - direct calls OK
class OrderController {
  async createOrder(p_request: CreateOrderRequest): Promise<CreateOrderResponse> {
    // Direct service call within same subdomain
    const order = await this._orderService.CreateOrderAsync(p_request);
    return this._mapper.ToResponse(order);
  }
}

class OrderService {
  async createOrderAsync(p_request: CreateOrderRequest): Promise<OrderModel> {
    // Direct repository call within same subdomain
    const order = this._mapper.ToModel(p_request);
    return await this._orderCommandRepo.CreateAsync(order);
  }
}
```

## Anti-Patterns (FORBIDDEN):

❌ **HTTP calls between subdomains**
   - **WRONG**: Payment service calling Order service's REST API
   - **CORRECT**: Payment service subscribing to OrderCreatedEvent

❌ **Synchronous subdomain coupling**
   - **WRONG**: `await httpClient.post('http://inventory-service/api/reserve')`
   - **CORRECT**: Publish `InventoryReservationRequestedEvent`, subscribe to `InventoryReservedEvent`

❌ **Shared database between subdomains**
   - Each subdomain MUST have its own database
   - MUST NOT: Query another subdomain's database directly
   - MUST: Communicate via events only

❌ **Mixing REST and Events incorrectly**
   - **WRONG**: Front-end subscribing to message queue
   - **CORRECT**: Front-end uses REST API, backend uses events

## Architecture Summary:

```
┌─────────────┐
│  Front-End  │
└──────┬──────┘
       │ REST/HTTP ONLY
       │
┌──────▼──────────────────────────────────────────┐
│  API Gateway / REST Controllers                 │
│  (Front-end boundary)                           │
└──────┬──────────────────────────────────────────┘
       │
       │ Within Subdomain: Direct calls OK
       │
┌──────▼──────────────────────────────────────────┐
│  Subdomain Services & Repositories              │
│  (Order Service, Payment Service, etc.)         │
└──────┬─────────────────────┬────────────────────┘
       │                     │
       │ Events Only         │ Events Only
       │                     │
       ▼                     ▼
┌─────────────────────────────────────────────────┐
│       Message Queue (Fan-out Topics)            │
│  order.created, payment.processed, etc.         │
└─────────────────────────────────────────────────┘
       │                     │
       │ Subscribe           │ Subscribe
       │                     │
       ▼                     ▼
┌──────────────┐      ┌──────────────┐
│  Payment     │      │  Inventory   │
│  Subdomain   │      │  Subdomain   │
└──────────────┘      └──────────────┘
```

## When to Use What:

| Scenario | Communication Method | Reason |
|----------|---------------------|---------|
| Front-end needs data | REST API | External boundary, synchronous UX |
| Order created → Payment needed | Domain Event | Async, decoupled subdomains |
| Order created → Inventory check | Domain Event | Async, decoupled subdomains |
| Controller → Service (same repo) | Direct call | Same subdomain, same process |
| Service → Repository (same repo) | Direct call | Same subdomain, same process |
| Payment success → Order update | Domain Event | Cross-subdomain communication |

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

**RELATIONAL scaffold with CQRS (MUST follow):**
```
/src/infrastructure/persistence/
  - PostgresDbContext.(cs|ts|py)
  - migrations/
  - /write/
    - <Feature>WriteEntity.(cs|ts|py)  # MUST have suffix "WriteEntity" - for commands
  - /read/
    - <Feature>ReadEntity.(cs|ts|py)   # MUST have suffix "ReadEntity" - for queries

/src/domain/models/
  - <Feature>Model.(cs|ts|py)          # MUST have suffix "Model", NO DB attributes

/src/application/mappers/
  - <Feature>Mapper.(cs|ts|py)         # MANDATORY - maps between all types

/src/application/services/
  - I<Feature>Service.(cs|ts|py)       # Interface in parent directory
  - /impl/
    - <Feature>Service.(cs|ts|py)      # Implementation in /impl subdirectory

/src/infrastructure/repositories/
  - I<Feature>CommandRepository.(cs|ts|py)  # Command repo interface
  - I<Feature>QueryRepository.(cs|ts|py)    # Query repo interface
  - /impl/
    - <Feature>CommandRepository.(cs|ts|py) # Works with WriteEntity
    - <Feature>QueryRepository.(cs|ts|py)   # Returns ReadEntity

/src/api/dto/v1/
  - <Feature>Request.(cs|ts|py)
  - <Feature>Response.(cs|ts|py)

/src/api/controllers/
  - <Feature>Controller.(cs|ts|py)

/src/api/middleware/
  - OwnershipMiddleware.(cs|ts|py)     # Check resource ownership
  - UnitOfWorkMiddleware.(cs|ts|py)    # Manage transactions

/src/domain/events/
  - <Feature><Action>Event.(cs|ts|py) # Domain events: OrderCreatedEvent

/src/infrastructure/queues/
  - IEventPublisher.(cs|ts|py)         # Publisher interface
  - IEventSubscriber.(cs|ts|py)        # Subscriber interface
  - /impl/
    - EventBus.(cs|ts|py)              # Queue implementation
  - /middleware/
    - QueueUnitOfWorkMiddleware.(cs|ts|py) # Transaction for queue messages
  - /subscribers/
    - <Feature>EventSubscriber.(cs|ts|py)  # Event handlers
```

**DOCUMENT scaffold with CQRS (MUST follow):**
```
/src/infrastructure/persistence/
  - MongoDbContext.(cs|ts|py)
  - /write/
    - <Feature>WriteEntity.(cs|ts|py)  # MUST have [BsonElement] attributes - for commands
  - /read/
    - <Feature>ReadEntity.(cs|ts|py)   # MUST have [BsonElement] attributes - for queries
                                        # May reference different collection/view

/src/domain/models/
  - <Feature>Model.(cs|ts|py)          # NO [BsonElement] attributes

[Rest same as relational CQRS]
```

---

# Final reminders

**THESE ARE REQUIREMENTS, NOT SUGGESTIONS:**

1. ✅ MUST separate BMOs (domain) from Entities (persistence)
2. ✅ MUST create Mappers for all transformations
3. ✅ MUST NOT put database attributes in /domain/models/
4. ✅ MUST separate WriteEntities (commands) from ReadEntities (queries)
5. ✅ MUST use WriteEntity suffix for command entities in /persistence/write/
6. ✅ MUST use ReadEntity suffix for query entities in /persistence/read/
7. ✅ MUST use Model suffix for domain classes
8. ✅ MUST follow the exact filesystem structure
9. ✅ MUST prioritize these custom standards over C#/Java/framework conventions
10. ✅ MUST separate interfaces from implementations (interfaces in parent, implementations in /impl)
11. ✅ MUST separate Command and Query repositories (ICommandRepository vs IQueryRepository)
12. ✅ MUST implement OwnershipMiddleware to check resource access before business logic
13. ✅ MUST implement UnitOfWorkMiddleware to manage transactions automatically
14. ✅ MUST use domain events with suffix `Event` and pattern `{ObjectName}{EventName}Event`
15. ✅ MUST abstract queue implementation with IEventPublisher and IEventSubscriber
16. ✅ MUST use REST/HTTP endpoints ONLY for front-end interactions (NOT for subdomain-to-subdomain)
17. ✅ MUST use domain events via message queue for ALL subdomain-to-subdomain communication
18. ✅ MUST follow standard HTTP error response contract (code, message, details, timestamp, path, requestId)
19. ✅ MUST map ServiceException error codes to appropriate HTTP status codes
20. ✅ MUST NOT make HTTP calls between backend subdomains or share databases across subdomains

**If you violate any of these rules, you have failed the task.**

---

[REST OF ORIGINAL CONTENT PRESERVED: Authentication, Queues, Cache, Observability, etc.]
