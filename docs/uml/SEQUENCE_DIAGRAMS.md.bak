# Sequence Diagrams - Key Workflows

This document contains sequence diagrams for the main workflows in the Cloud-Intelligent-Jira-Ticket-Resolution-System.

## Compilation Instructions

All diagrams are provided as PlantUML source files (`.puml`) and can be compiled using:

```bash
# Compile all sequence diagrams
plantuml -tpng docs/uml/sequence_*.puml

# Or compile a specific diagram
plantuml -tpng docs/uml/sequence_1_ingestion.puml
```

## 1. Ticket Ingestion and Processing Flow

**Source:** [sequence_1_ingestion.puml](sequence_1_ingestion.puml)

**Generated Image:** ![Ticket Ingestion](TicketIngestionSequence.png)

This sequence diagram shows how tickets are ingested through Kafka, enriched with NLP processing, and stored in multiple data stores.

### Key Steps:
1. Kafka Producer publishes raw ticket to Amazon MSK
2. Stream Consumer receives and processes the ticket
3. Enrichment Service performs NLP analysis
4. Parallel storage operations to S3 and DynamoDB
5. Enriched ticket published to downstream topic

### Participants:
- Kafka Producer
- Amazon MSK
- Stream Consumer
- Enrichment Service
- NLP Processor
- S3 Storage
- DynamoDB
- Kafka (enriched topic)

---

## 2. RAG (Retrieval-Augmented Generation) Flow

**Source:** [sequence_2_rag.puml](sequence_2_rag.puml)

**Generated Image:** ![RAG Processing](RAGProcessingSequence.png)

This diagram illustrates the RAG pipeline for generating intelligent responses using vector search and LLM.

### Key Steps:
1. API Gateway receives new ticket request
2. Response Agent initiates RAG processing
3. Embedding Service generates query vector
4. Vector Store performs similarity search
5. RAG Service constructs prompt with context
6. LLM generates response
7. Quality Agent validates the response

### Decision Points:
- Quality Score determines auto-approval vs manual review
- Confidence threshold affects routing

---

## 3. Multi-Agent Workflow Orchestration

**Source:** [sequence_3_agent_workflow.puml](sequence_3_agent_workflow.puml)

**Generated Image:** ![Multi-Agent Workflow](MultiAgentWorkflow.png)

This sequence shows how multiple specialized agents collaborate to process tickets.

### Agent Roles:
- **Triage Agent:** Categorizes and prioritizes tickets
- **Response Agent:** Generates responses using RAG
- **Quality Agent:** Validates response quality
- **Escalation Agent:** Routes complex issues
- **Automation Engine:** Auto-approves high-confidence responses

### Workflow Branches:
1. **P0 Critical Path:** Immediate escalation to on-call team
2. **P1-P3 Standard Path:** RAG processing with quality checks
3. **Auto-Approval Path:** High confidence tickets auto-resolved
4. **Manual Review Path:** Low confidence tickets queued for humans
5. **Escalation Path:** Complex issues routed to specialists

---

## 4. Feedback Loop and Retraining Pipeline

**Source:** [sequence_4_mlops.puml](sequence_4_mlops.puml)

**Generated Image:** ![Feedback and Retraining](FeedbackAndRetrainingPipeline.png)

This diagram shows the MLOps pipeline for continuous learning and model improvement.

### Pipeline Stages:
1. **Feedback Collection:** Agent dashboard submits feedback
2. **Data Preparation:** Load and preprocess feedback data
3. **Training:** Fine-tune embedding and LLM models
4. **Evaluation:** Calculate metrics and compare with baseline
5. **Deployment:** Staged rollout with canary testing
6. **Monitoring:** Track performance and detect issues

### Deployment Strategy:
- Blue-green deployment to staging
- Integration testing
- Canary deployment (10% → 50% → 100%)
- Automatic rollback on failure

---

## Key Observations

### Performance Optimizations
1. **Parallel Processing:** Storage operations happen concurrently
2. **Caching:** Redis cache reduces database load
3. **Batch Operations:** Embedding models process in batches
4. **Async Processing:** Kafka enables fire-and-forget patterns

### Resilience Patterns
1. **Circuit Breaker:** External service calls have timeouts
2. **Retry Logic:** Failed operations retry with exponential backoff
3. **Fallback Mechanisms:** Quality checks prevent poor responses
4. **Graceful Degradation:** System works with reduced functionality

### Security Measures
1. **Authentication:** JWT tokens verified by Cognito
2. **Authorization:** Lambda authorizers check permissions
3. **Rate Limiting:** Prevents API abuse
4. **PII Removal:** Sensitive data cleaned early in pipeline

### Observability
1. **Distributed Tracing:** Each interaction is logged
2. **Metrics Collection:** CloudWatch captures all metrics
3. **Alert System:** SNS notifications for anomalies
4. **Audit Trail:** All decisions are recorded

---

## Compiling the Diagrams

To regenerate all sequence diagrams:

```bash
# Generate PNG images
plantuml -tpng docs/uml/sequence_*.puml

# Generate SVG images (scalable)
plantuml -tsvg docs/uml/sequence_*.puml

# Generate both
plantuml -tpng -tsvg docs/uml/sequence_*.puml
```

### PlantUML Installation

If PlantUML is not installed:

```bash
# Ubuntu/Debian
sudo apt-get install plantuml graphviz

# macOS
brew install plantuml

# Or use Docker
docker run -v $(pwd):/data plantuml/plantuml:latest -tpng /data/docs/uml/*.puml
```
