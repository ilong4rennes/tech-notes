#### Traditional vs. ML Development
- **Traditional Development**: Define rules explicitly.
- **ML Development**: Cyclical process involving observation, hypothesis, testing, and refining.

#### Black-box View of ML
- Simplified view where input is processed by the model to yield an output, without visibility into internal workings.

#### Software Engineering for ML (Microsoft View)
![[Screenshot 2024-11-19 at 5.21.27 PM.png]]
- 3 Key differences btw Machine Learning development and Traditional Software Development:
	- 1. **Data Discovery and Management**:
	    - Traditional software relies on predefined logic and algorithms.
	    - ML systems heavily depend on data for training and predictions, requiring extensive processes for data collection, cleaning, labeling, and management
	- 2. **Customization and Reuse**:
	    - Traditional software components can often be reused across different projects with minimal customization.
	    - ML systems often require models to be retrained or tuned with task-specific data, limiting reuse.
	- 3. **No Modular Development of the Model Itself**
	    - Traditional software is typically modular, enabling separation of concerns and easier maintenance.
	    - ML models are holistic and interdependent, making modular development challenging. The model is treated as a single block, where changes often require retraining the entire system.

#### Typical ML Pipeline
- Static
	- Get labeled data (data collection, cleaning and, labeling)
	- Identify and extract features (feature engineering)
	- Split data into training and evaluation set
	- Learn model from training data (model training)
	- Evaluate model on evaluation data (model evaluation)
	- Repeat, revising features
- with production data
	- Evaluate model on production data; monitor (model monitoring)
	- Select production data for retraining (model training + evaluation)
	- Update model regularly (model deployment)

#### **Feature Engineering**
- Includes:
  - Normalization, context inclusion, and removing misleading features.
  - Examples: time, location, weather, demand patterns.

#### **Data Cleaning**
- Handles outliers, missing values, and ensures normalized, high-quality data.

#### **Learning & Evaluation**
- Evaluates models on:
  - Prediction accuracy.
  - Precision/recall for classification.
  - Top-K accuracy for ranking.

#### **ML Model Tradeoffs**
- Balances:
  - Accuracy, latency, scalability, and explainability.
  - Model size, robustness, and energy consumption.

#### **System Architecture**
- Decides where the model resides:
  - **Cloud**: Easy updates but requires connectivity.
  - **Edge (e.g., phones)**: Lower latency, offline use.

#### **Updating Models**
- Models need frequent updates due to:
  - Data drift.
  - Feedback loops.
  - New requirements.

#### **Mistakes in ML**
- Types: System outages, model degradation, and errors.
- Mitigation strategies include better data, guardrails, and manual overrides.

#### **QA in ML**
- Encompasses:
  - Data validation (schemas, distributions).
  - Model testing (unit, integration).
  - Continuous monitoring for input drift.

#### **Software Qualities of ML Systems**
- Attributes: Fairness, scalability, robustness, energy efficiency, interpretability.

#### **Fairness in ML**
- Types of fairness:
  - Group unaware, demographic parity, equal opportunity.
- Challenges:
  - Defining "fair" and addressing biases.

#### **LLMs (Large Language Models)**
- Overview:
  - LLMs predict text sequences.
  - Examples: GPT-3 (175B parameters), GPT-4 (~1.24T parameters).
  - Challenges: Hallucinations, high latency.

#### **Evaluating LLMs**
- Compare generated output against labeled datasets using:
  - Textual comparisons (exact match, cosine similarity).
  - Metrics for creativity and diversity.

#### **Improving LLM Performance**
- Techniques:
  - **Prompt Engineering**: Reword prompts for better outputs.
  - **Chain of Thought Prompting**: Include reasoning examples in prompts.
  - **Fine-Tuning**: Adapt models to specific tasks with additional data.

#### **Productionizing LLMs**
- Consider:
  - Operational costs (e.g., token-based pricing).
  - Latency improvements (e.g., caching, streaming responses).
  - Reinforcement learning with user feedback.

#### **Intellectual Property & Security**
- Concerns:
  - Ownership of LLM-generated content.
  - Security risks like prompt injection.

#### **Retrospectives**
- Aims to improve workflows by reflecting on:
  - Start, stop, and keep doing practices.

This summary provides a detailed breakdown of each slide in the presentation. Let me know if you need further insights!