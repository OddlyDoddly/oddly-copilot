# Advanced Agent Template

Use this template for creating sophisticated agents with complex behaviors and decision-making capabilities.

## Agent Metadata

```yaml
name: [your-agent-name]
version: 1.0.0
category: [development|testing|documentation|security|devops]
description: [Brief one-line description]
complexity: advanced
requires: [List any prerequisite agents or tools]
```

## Purpose

**Problem Statement:**
[Detailed description of the complex problem this agent solves]

**Scope:**
- **In Scope**: [What this agent handles]
- **Out of Scope**: [What this agent does not handle]
- **Boundaries**: [Where this agent's responsibility ends]

**When to Use:**
- [Complex scenario 1]
- [Complex scenario 2]
- [Complex scenario 3]

**When NOT to Use:**
- [Scenario where another approach is better]
- [Scenario outside agent's expertise]

**Expected Outcomes:**
- [Detailed outcome 1]
- [Detailed outcome 2]
- [Detailed outcome 3]

## Expertise Domain

This agent specializes in:

- **Primary Domain**: [Advanced topic area]
- **Secondary Domains**: [Related areas]
- **Technologies**: 
  - [Technology stack with versions]
  - [Frameworks and libraries]
  - [Tools and platforms]
- **Skills**: 
  - [Advanced capability 1]
  - [Advanced capability 2]
  - [Advanced capability 3]
- **Methodologies**:
  - [Design patterns used]
  - [Best practices followed]
  - [Standards compliance]

## Architecture

### Agent Workflow

```
┌─────────────┐
│   Input     │
│ Processing  │
└──────┬──────┘
       │
       ▼
┌─────────────┐
│  Analysis   │
│   & Plan    │
└──────┬──────┘
       │
       ▼
┌─────────────┐
│  Execution  │
│   Pipeline  │
└──────┬──────┘
       │
       ▼
┌─────────────┐
│ Validation  │
│  & Report   │
└─────────────┘
```

### Decision Tree

```
START
  │
  ├─ [Condition 1]? ──YES→ [Action A]
  │                    NO↓
  ├─ [Condition 2]? ──YES→ [Action B]
  │                    NO↓
  └─ [Default] ────────────→ [Action C]
```

### State Management

The agent maintains state across:
- [State category 1]: [Purpose]
- [State category 2]: [Purpose]
- [State category 3]: [Purpose]

## Instructions

### Role Definition

You are an advanced [ROLE] agent with deep expertise in [DOMAIN]. You are responsible for [PRIMARY RESPONSIBILITY] and have authority to [DECISION-MAKING AUTHORITY].

Your advanced capabilities include:
- [Capability 1]
- [Capability 2]
- [Capability 3]

### Core Objectives

Your objectives, in priority order:

1. **[High Priority Objective]**: [Detailed description]
   - Sub-goal 1.1: [Details]
   - Sub-goal 1.2: [Details]

2. **[Medium Priority Objective]**: [Detailed description]
   - Sub-goal 2.1: [Details]
   - Sub-goal 2.2: [Details]

3. **[Lower Priority Objective]**: [Detailed description]
   - Sub-goal 3.1: [Details]
   - Sub-goal 3.2: [Details]

### Advanced Approach

#### Phase 1: Deep Analysis

1. **Context Gathering**
   ```
   - Analyze [aspect 1]
   - Review [aspect 2]
   - Understand [aspect 3]
   - Map dependencies
   - Identify constraints
   ```

2. **Problem Decomposition**
   ```
   - Break down into sub-problems
   - Identify patterns
   - Determine complexity
   - Assess risks
   - Plan mitigation strategies
   ```

3. **Strategy Selection**
   ```
   IF [complex_pattern_detected]:
       USE strategy_A with parameters X, Y
   ELIF [simple_pattern_detected]:
       USE strategy_B with parameters Z
   ELSE:
       USE adaptive_strategy with auto-tuning
   ```

#### Phase 2: Intelligent Execution

1. **Planning**
   - Create detailed execution plan
   - Identify critical paths
   - Set checkpoints
   - Define rollback points
   - Estimate resources needed

2. **Execution with Monitoring**
   ```python
   for step in execution_plan:
       pre_condition_check()
       execute_step(step)
       validate_step(step)
       if not valid:
           rollback_if_needed()
           adjust_strategy()
       checkpoint()
       update_progress()
   ```

3. **Adaptive Behavior**
   - Monitor execution quality
   - Adjust approach based on results
   - Learn from failures
   - Optimize for efficiency

#### Phase 3: Comprehensive Validation

1. **Multi-Level Validation**
   - Syntactic validation
   - Semantic validation
   - Integration validation
   - Performance validation
   - Security validation

2. **Quality Assurance**
   - Run automated checks
   - Verify against requirements
   - Test edge cases
   - Validate error handling
   - Measure against KPIs

3. **Reporting**
   - Summarize actions taken
   - Report metrics and measurements
   - Document decisions made
   - Highlight any issues
   - Provide recommendations

### Advanced Guidelines

#### Decision Making Framework

Use this framework for complex decisions:

```
EVALUATE decision_context:
  GATHER relevant_data
  ANALYZE trade-offs
  CONSIDER:
    - Performance impact
    - Maintainability
    - Security implications
    - Scalability concerns
    - Team expertise
    - Time constraints
    
  RANK options BY:
    1. Correctness (highest priority)
    2. Security
    3. Performance
    4. Maintainability
    5. Simplicity
    
  SELECT best_option
  DOCUMENT rationale
  IMPLEMENT with monitoring
```

#### Error Recovery Strategies

Advanced error handling:

```
TRY:
    execute_primary_strategy()
CATCH [ErrorType1]:
    log_detailed_error()
    TRY fallback_strategy_1()
    CATCH: escalate_to_fallback_2()
CATCH [ErrorType2]:
    analyze_root_cause()
    IF recoverable: attempt_recovery()
    ELSE: graceful_degradation()
CATCH [UnexpectedError]:
    full_diagnostic_report()
    safe_rollback()
    request_human_intervention()
FINALLY:
    cleanup_resources()
    update_state()
    report_outcome()
```

#### Performance Optimization

Optimization strategies:

1. **Parallelization**
   - Identify independent operations
   - Execute in parallel when safe
   - Manage dependencies carefully
   - Monitor resource usage

2. **Caching**
   - Cache expensive operations
   - Invalidate appropriately
   - Balance memory vs. speed

3. **Incremental Processing**
   - Process in chunks
   - Provide progress updates
   - Allow interruption/resumption

### Quality Standards

#### Code Quality
- [ ] Follows language best practices
- [ ] Passes all linters
- [ ] Has comprehensive tests
- [ ] Is well-documented
- [ ] Handles errors gracefully
- [ ] Is maintainable
- [ ] Is performant

#### Security Standards
- [ ] No hardcoded secrets
- [ ] Input validation
- [ ] Proper authentication
- [ ] Authorization checks
- [ ] Secure dependencies
- [ ] Audit logging
- [ ] Vulnerability scanning

#### Documentation Standards
- [ ] Clear purpose statement
- [ ] Usage examples
- [ ] API documentation
- [ ] Edge cases documented
- [ ] Troubleshooting guide
- [ ] Performance characteristics
- [ ] Security considerations

## Tool Configuration

### Required Tools

**Core Tools:**
- **[tool-1]**: [Detailed usage requirements]
  - Configuration: [specific config]
  - Permissions needed: [list]
  - Usage patterns: [how this agent uses it]

### Advanced Tool Usage

**Orchestration Pattern:**
```
1. Use [tool-A] to gather information
2. Use [tool-B] to analyze results
3. Use [tool-C] in parallel with [tool-D]
4. Use [tool-E] for validation
5. Use [tool-F] for reporting
```

**Tool Combination Strategies:**
- [Strategy 1]: When to use [tool-A] + [tool-B]
- [Strategy 2]: When to use [tool-C] alone
- [Strategy 3]: When to orchestrate [tool-D], [tool-E], [tool-F]

### Tool Restrictions

Strict limitations:
- [Restriction 1 with rationale]
- [Restriction 2 with rationale]
- [Restriction 3 with rationale]

## Advanced Examples

### Example 1: Complex Multi-Step Scenario

**Context:**
[Detailed context of a complex scenario]

**Input:**
```
[Complex input with multiple components]
```

**Expected Behavior:**
```
Step 1: [Analysis phase details]
  - Sub-step 1.1: [Details]
  - Sub-step 1.2: [Details]

Step 2: [Execution phase details]
  - Sub-step 2.1: [Details]
  - Sub-step 2.2: [Details]
  
Step 3: [Validation phase details]
  - Sub-step 3.1: [Details]
  - Sub-step 3.2: [Details]
```

**Expected Output:**
```
[Detailed expected output with explanations]
```

**Decision Rationale:**
[Explanation of why this approach was chosen]

### Example 2: Edge Case Handling

**Context:**
[Description of challenging edge case]

**Input:**
```
[Edge case input]
```

**Expected Behavior:**
```
1. Detect edge case: [how]
2. Apply special handling: [what]
3. Validate carefully: [how]
4. Report with caveats: [what to report]
```

**Expected Output:**
```
[Expected output for edge case]
```

## Testing Strategy

### Comprehensive Test Plan

1. **Unit Testing**
   - Test individual components
   - Mock dependencies
   - Cover all branches

2. **Integration Testing**
   - Test component interactions
   - Verify data flow
   - Check error propagation

3. **End-to-End Testing**
   - Test complete workflows
   - Use realistic scenarios
   - Measure performance

4. **Stress Testing**
   - Large inputs
   - High concurrency
   - Resource constraints

5. **Chaos Testing**
   - Random failures
   - Network issues
   - Resource exhaustion

### Validation Checklist

**Functional Validation:**
- [ ] Meets all requirements
- [ ] Handles all use cases
- [ ] Processes edge cases correctly
- [ ] Error handling works
- [ ] Recovery mechanisms work

**Non-Functional Validation:**
- [ ] Performance meets targets
- [ ] Scalability is adequate
- [ ] Security is robust
- [ ] Reliability is high
- [ ] Maintainability is good

## Monitoring and Observability

### Metrics to Track

- **Performance Metrics**:
  - Execution time
  - Resource usage
  - Throughput
  - Latency

- **Quality Metrics**:
  - Success rate
  - Error rate
  - Retry rate
  - Rollback rate

- **Business Metrics**:
  - Tasks completed
  - Value delivered
  - User satisfaction

### Logging Strategy

```
INFO: High-level progress updates
DEBUG: Detailed execution information
WARN: Recoverable issues
ERROR: Failures requiring attention
CRITICAL: Severe failures
```

## Maintenance

### Changelog

- **1.0.0** (YYYY-MM-DD): Initial version
  - [Feature list or description of what was added]
  
### Upgrade Path

When updating this agent:
1. Review breaking changes
2. Update dependencies
3. Run full test suite
4. Update documentation
5. Notify users

### Known Limitations

- [Technical limitation 1]
- [Technical limitation 2]
- [Design trade-off 1]

### Future Enhancements

- **Short-term** (Next release):
  - [Enhancement 1]
  - [Enhancement 2]

- **Long-term** (Future releases):
  - [Enhancement 3]
  - [Enhancement 4]

## Integration

### With Other Agents

This agent can:
- **Call**: [List of agents this can invoke]
- **Be called by**: [List of agents that can invoke this]
- **Coordinate with**: [List of peer agents]

### API Contract

Input contract:
```yaml
required:
  - field1: type1
  - field2: type2
optional:
  - field3: type3
```

Output contract:
```yaml
success:
  - result: type
  - metadata: type
failure:
  - error: type
  - context: type
```

## Security Considerations

- [Security concern 1 and mitigation]
- [Security concern 2 and mitigation]
- [Security concern 3 and mitigation]

## Performance Characteristics

- **Best Case**: [Performance under ideal conditions]
- **Average Case**: [Typical performance]
- **Worst Case**: [Performance under worst conditions]
- **Space Complexity**: [Memory usage]
- **Time Complexity**: [Execution time]

## Troubleshooting Guide

### Common Issues

**Issue 1: [Problem]**
- Symptoms: [What you observe]
- Cause: [Root cause]
- Solution: [How to fix]
- Prevention: [How to avoid]

**Issue 2: [Problem]**
- Symptoms: [What you observe]
- Cause: [Root cause]
- Solution: [How to fix]
- Prevention: [How to avoid]

## Additional Notes

[Any additional important information]

---

## Usage Instructions

1. Copy this template to `.github/agents/[your-agent-name].md`
2. Carefully fill in all sections
3. Remove this usage instructions section
4. Test extensively with various scenarios
5. Iterate based on real-world usage
6. Keep documentation updated
