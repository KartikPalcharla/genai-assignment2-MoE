# Unit 2 Assignment: MoE Router Observations

## Routing Accuracy
Across all test cases, the **Llama-3.1-8b-instant** router achieved 100% accuracy. By using `temperature=0`, the router consistently returned the exact category string without conversational filler, which is critical for the orchestrator's logic to function.

## Expert Persona Analysis

### 1. Technical Expert
- **Observation:** Successfully identified technical keywords ("Python", "IndexError").
- **Behavior:** The response was structured and logical. It didn't just provide a solution but explained the underlying cause and offered two distinct coding patterns (Input Validation and Exception Handling), fulfilling the "Senior Engineer" persona.

### 2. Billing Expert
- **Observation:** Correctly triggered by financial intent ("charged twice", "subscription").
- **Behavior:** This expert displayed the highest shift in "voice." It transitioned from the cold, logic-based tone of the Technical Expert to an empathetic, customer-first tone. It strictly adhered to the business constraint of the "30-day refund window."

### 3. Tool Use (Bonus)
- **Observation:** The router correctly identified the request for real-time data (`crypto_tool`).
- **Behavior:** Instead of the LLM "hallucinating" a price, the orchestrator intercepted the request and routed it to a deterministic Python function. This demonstrates a hybrid MoE architecture where "Experts" can be either LLMs or hard-coded tools.

### 4. General Expert
- **Observation:** Acted as an effective catch-all for non-specific queries ("How's your day?").
- **Behavior:** Maintained a brief and friendly tone, preventing the system from over-analyzing casual social interactions.

---

## Architectural Benefits
* **Latency Optimization:** Using a smaller, faster model for the Router minimizes the overhead of the classification step.
* **Specialization:** By separating system prompts, we can fine-tune the "creativity" (temperature) for each expert individually—keeping technical answers precise ($0.2$) and general answers fluid ($0.7$).
* **Scalability:** The system is modular; new experts or specialized tools can be added to the `MODEL_CONFIG` without rewriting the core orchestration logic.