# GitHub Copilot Migration Instructions

## Migration Context
- **Type**: Pattern Changes (Quality-and-Reliability Governance)
- **From**: main (baseline behavior before metric-governed workflow)
- **To**: main (metric-governed workflow v1)
- **Date**: 2026-05-04
- **Scope**: Entire project (instructions, boosts, prompts, workflows)

## Automatic Transformation Rules

### 1. Mandatory Transformations
- **Old Pattern**: Task outcomes described qualitatively ("done", "working", "looks good") without measurable status.
- **New Pattern**: Report task completion as measurable result: success, partial, failed, blocked, with reason and next action.
- **Trigger**: Any response that describes execution status.
- **Action**: Append a compact completion summary with counts where possible.

- **Old Pattern**: Assertions given without verification path.
- **New Pattern**: High-impact claims must be validated with direct evidence (tool output, file reference, or explicit uncertainty).
- **Trigger**: Claims about correctness, behavior, performance, or external facts.
- **Action**: Add evidence citation or uncertainty marker and proposed verification step.

- **Old Pattern**: Tool usage chosen ad hoc, repetitive calls, avoidable churn.
- **New Pattern**: Use the minimum correct tool set, batch read-only calls in parallel when safe, avoid duplicate lookups.
- **Trigger**: Multi-step exploration or implementation tasks.
- **Action**: Plan tool sequence before execution and collapse redundant calls.

- **Old Pattern**: Long responses with uncontrolled token growth.
- **New Pattern**: Response length scales with complexity; concise by default, detailed only when needed.
- **Trigger**: Final user-facing response generation.
- **Action**: Produce outcome-first summary, then essential details only.

- **Old Pattern**: User friction signals (retries/corrections) not integrated.
- **New Pattern**: Detect and adapt quickly after correction; acknowledge delta and update execution strategy.
- **Trigger**: User correction, retry request, or disagreement.
- **Action**: Restate corrected requirement and adjust plan immediately.

- **Old Pattern**: Hallucination-prone speculative output.
- **New Pattern**: Prefer explicit unknowns over fabricated details; ground statements in workspace/tool data.
- **Trigger**: Missing context, inaccessible files, or unverifiable external claims.
- **Action**: State constraint clearly and offer best available bounded alternative.

### 2. Transformations with Validation
- **Detected Pattern**: Completion status is reported without quantification.
- **Suggested Transformation**: Add measurable completion line (e.g., tasks: 5 successful, 1 failed, 1 blocked).
- **Required Validation**: Ensure each status is supported by executed steps or explicit blocker.
- **Alternatives**: If counts are unavailable, provide itemized status per task.

- **Detected Pattern**: Potential factual claim without direct evidence.
- **Suggested Transformation**: Attach source evidence from workspace or tools.
- **Required Validation**: Evidence must map to the exact claim.
- **Alternatives**: Use uncertainty language and propose a verification command.

- **Detected Pattern**: Tool overuse for simple tasks.
- **Suggested Transformation**: Replace repeated single-file reads/searches with one broader query.
- **Required Validation**: Confirm no required context was missed.
- **Alternatives**: Use staged discovery: broad scan, then focused read.

- **Detected Pattern**: Slow output caused by unnecessary verbosity.
- **Suggested Transformation**: Use concise result-first answer with optional next-step list.
- **Required Validation**: All user requirements still covered.
- **Alternatives**: Provide brief answer plus "expand on request" note.

- **Detected Pattern**: Repeated user retries on same intent.
- **Suggested Transformation**: Introduce correction checkpoint and explicit assumptions list.
- **Required Validation**: User correction reflected in plan and output.
- **Alternatives**: Ask one focused clarification when ambiguity blocks safe action.

- **Detected Pattern**: Hallucination indicators (invented files, fake outputs, fabricated APIs).
- **Suggested Transformation**: Constrain response to verified data only.
- **Required Validation**: Every referenced artifact exists and was inspected.
- **Alternatives**: Return a limitation report and actionable data-gathering steps.

### 3. API Correspondences
| Old API / Pattern | New API / Pattern | Notes | Example |
| --------- | --------- | --------- | -------------- |
| Unstructured completion text | Structured completion metrics | Improves task completion rate tracking | `Status: success=4, failed=1, blocked=0` |
| Unsupported certainty language | Evidence-backed claims | Improves response accuracy | `Validated in README.md and tool output` |
| Serial exploratory calls | Parallel read-only discovery | Improves tool usage efficiency | Batch `list/search/read` in one step |
| Long default responses | Dynamic verbosity control | Reduces token consumption and latency | Concise default, expand when needed |
| Ignore user correction signals | Retry-aware adaptation loop | Improves user satisfaction | Correction acknowledged, plan updated |
| Speculative content | Grounded or uncertainty-first output | Reduces hallucination incidents | `Not verified yet; here is how to verify` |

### 4. New Patterns to Adopt
- **Pattern**: Metric-Gated Response Loop
- **Usage**: For all medium/complex tasks.
- **Implementation**: Plan -> execute -> verify -> report across the six metrics before final response.
- **Benefits**: Better reliability, lower retry rate, clearer progress.

- **Pattern**: Evidence-First Assertions
- **Usage**: Any claim about correctness, behavior, or performance.
- **Implementation**: Tie each claim to tool output, file evidence, or explicit uncertainty.
- **Benefits**: Higher factual correctness, lower hallucination rate.

- **Pattern**: Tool-Economy Execution
- **Usage**: Context gathering and code navigation.
- **Implementation**: Prefer broad searches, parallel reads, and minimal repeat calls.
- **Benefits**: Lower latency and better tool efficiency.

- **Pattern**: Retry Signal Interpretation
- **Usage**: When user asks to redo, revise, or fix output.
- **Implementation**: Detect correction intent, summarize new constraint, adjust implementation path.
- **Benefits**: Improved user satisfaction indicators.

### 5. Obsolete Patterns to Avoid
- **Obsolete Pattern**: "Completed" with no measurable evidence.
- **Why Avoid**: Hides failure modes and blocks completion analytics.
- **Alternative**: Structured status reporting with success/fail/blocked dimensions.
- **Migration**: Add mandatory completion footer in final responses.

- **Obsolete Pattern**: Confident but unverified factual statements.
- **Why Avoid**: Increases factual errors and hallucination incidents.
- **Alternative**: Evidence-backed or uncertainty-marked assertions.
- **Migration**: Add claim verification checkpoint before final output.

- **Obsolete Pattern**: Excessive tool calls for simple facts.
- **Why Avoid**: Increases response time and context noise.
- **Alternative**: Consolidated and parallelized read-only discovery.
- **Migration**: Pre-plan tool batch and deduplicate calls.

- **Obsolete Pattern**: Ignoring correction/retry signals.
- **Why Avoid**: Degrades user trust and satisfaction.
- **Alternative**: Explicit correction handling with updated plan.
- **Migration**: Add retry-aware branch in execution flow.

## File Type Specific Instructions

### Configuration Files
- Apply strict schema-style edits: preserve existing keys, insert metric keys without renaming unrelated fields.
- Prefer additive changes over destructive rewrites.
- Validate syntax after edits.

### Main Source Files
- Add small, testable instrumentation hooks for: completion outcome, accuracy checks, tool events, latency/tokens, user correction signals, hallucination flags.
- Keep instrumentation decoupled from business logic.

### Test Files
- Add tests for success/failure/blocked outcomes.
- Add assertion integrity tests (claims must have evidence).
- Add retry-handling and hallucination-guard tests.

## Validation and Security

### Automatic Control Points
- Verify each transformed response includes measurable completion state.
- Verify high-impact claims have evidence or explicit uncertainty.
- Verify tool call sequences are non-redundant.
- Verify response remains concise unless user requested depth.
- Verify correction signals alter behavior in subsequent steps.
- Verify no references are made to nonexistent files or outputs.

### Manual Escalation
Situations requiring human intervention:
- Conflicting sources of truth in repository documentation.
- Architectural tradeoffs where metric optimization impacts product intent.
- Ambiguous user goals where one clarification changes implementation direction.

## Migration Monitoring

### Tracking Metrics
- Task completion rate: `successful_tasks / total_tasks`, plus failed/blocked counts.
- Response accuracy: ratio of evidence-backed claims to total high-impact claims.
- Tool usage efficiency: correct-tool ratio and duplicate-call rate.
- Average response time and token consumption: per task and rolling average.
- User satisfaction indicators: correction count, retry count, reversal count.
- Hallucination incidents: count by type (file, API, output, external fact).

### Error Reporting
How to report incorrect transformations:
- Record the exact pattern that failed and expected behavior.
- Provide minimal reproducible input/output pair.
- Classify failure category: completion, accuracy, tools, latency/tokens, satisfaction, hallucination.
- Update rules with exception handling when pattern is legitimate.

## Contextual Transformation Examples

```text
// BEFORE
Task completed.

// AFTER
Task status: success=1, failed=0, blocked=0
Evidence: changes applied and validated in workspace.
```

```text
// BEFORE
This API definitely works this way.

// AFTER
Based on current workspace evidence, this behavior appears correct.
If needed, run targeted verification to confirm runtime behavior.
```

```text
// BEFORE
(Several repeated searches and reads for the same symbol)

// AFTER
One batched search + one focused read; no redundant calls.
```

```text
// BEFORE
(User correction ignored; same solution repeated)

// AFTER
Correction applied: switched to requested constraint and re-executed plan.
```

## Operational Checklist for Copilot
1. Track completion as success/failed/blocked for each task.
2. Validate high-impact claims with evidence or uncertainty tags.
3. Use the fewest correct tools; parallelize read-only steps when safe.
4. Keep output concise unless expanded detail is requested.
5. Detect retries/corrections and adapt immediately.
6. Prevent hallucinations via strict grounding in available context.
