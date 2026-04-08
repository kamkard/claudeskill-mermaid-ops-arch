# Decision Tree Examples

Worked examples showing how the skill routes user requests to the correct guides.

## Example 1: Workflow Diagram

**Input:** "Show the checkout process workflow"

**Decision Path:**
```
1. Analyze: workflow, process → ACTIVITY DIAGRAM
2. Load: references/guides/diagrams/activity-diagrams.md
3. Find pattern: E-commerce checkout template
4. Generate using template + Unicode symbols
```

**Output:** Activity diagram with Unicode symbols for cart, payment, order states.

---

## Example 2: Spring Boot Code

**Input:** "Here's my Spring Boot controller, create diagrams"

**Decision Path:**
```
1. Analyze: Spring Boot, code provided → CODE-TO-DIAGRAM + SPRING BOOT
2. Load:
   - examples/spring-boot/README.md
   - references/guides/diagrams/architecture-diagrams.md
   - references/guides/diagrams/sequence-diagrams.md
   - references/guides/diagrams/activity-diagrams.md
3. Generate:
   a. Architecture diagram from @RestController/@Service/@Repository annotations
   b. Sequence diagram from method call chain
   c. Activity diagram from business logic flow
```

**Output:** 3-4 diagrams showing different views of the application.

---

## Example 3: Infrastructure Documentation

**Input:** "Document my GCP Cloud Run deployment with AlloyDB"

**Decision Path:**
```
1. Analyze: infrastructure, GCP, Cloud Run → DEPLOYMENT DIAGRAM
2. Load:
   - references/guides/diagrams/deployment-diagrams.md
   - examples/spring-boot/ or examples/fastapi/ (if code provided)
3. Check for IaC files (Pulumi, Terraform, docker-compose)
4. Generate deployment diagram with:
   - Cloud Run services with specs
   - VPC connector, AlloyDB cluster
   - Security (IAM, Secret Manager), Monitoring
```

**Output:** Complete GCP deployment diagram with all resources labeled.

---

## Example 4: Resilient Diagram Generation

**Input:** "Create a sequence diagram and add it to the design doc"

**Decision Path:**
```
1. Analyze: diagram generation + markdown integration
2. Load: references/guides/resilient-workflow.md
3. Load: references/guides/diagrams/sequence-diagrams.md
4. Generate Mermaid code using templates
5. Run resilient_diagram.py with --json flag
6. If validation fails → apply troubleshooting fix → retry
7. On success → embed image reference in markdown
```

**Output:** Validated diagram file + image reference added to markdown.
