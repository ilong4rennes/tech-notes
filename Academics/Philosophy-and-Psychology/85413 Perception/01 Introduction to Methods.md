---
tags: [perception]
---

# Part 1: Measuring neurons and the brain

## What do we learn from studying perception?
- **Basic science**: explains how mental representations arise from the physical world.
- **Quantitative data**: measures human abilities (normal range, age differences, impairments) with applications.
- **Neuroscience**: shows how perception links neural processes with mental functions.
- **Broader understanding**: perception underlies almost all behavior (action, emotion, thought).

## Neurons and Neural Firing
- **Neuron structure**: dendrites, soma, axon, terminals; many structural variations exist.![[Screenshot 2025-08-27 at 6.53.47 PM.png]]
- **Neural firing (spikes)**:
    - Triggered by chemical stimulation at receptors (感觉器官).
    - Positive ions enter, neuron reaches threshold, fires an action potential.
    - Brief electrical signal travels down axon; ions pumped out to reset.
- **Information flow between neurons**: Electrical signals produce chemical changes that allows them to cross the synapse (gap) between neurons, creating neural networks.![[Screenshot 2025-08-27 at 7.16.04 PM.png]]

## How does neural firing signal perceptual information?

- Neurons communicate information through **action potentials** (spikes).

Two types of information are conveyed (Two main coding strategies):

1. **Total spike activity**
    - count of spikes within a time window.
    - Stronger stimulus → more spikes, faster accumulation. ![[Screenshot 2025-08-27 at 7.23.44 PM.png]]
	    - **Top Graph (weak vs strong stimulus, spikes over time):**
		    - Weak stimulus → few spikes, spaced out.
		    - Strong stimulus → many spikes, closely packed.
		    - Shows stimulus strength is encoded in spike frequency.
		- **Bottom Graph (summed spikes):**
		    - Y-axis: total spike number.
		    - X-axis: time.
		    - Strong stimulus curve rises steeply → more spikes accumulate faster.
		    - Weak stimulus curve is shallower → fewer spikes over same period.
		    - Interpretation: stimulus intensity is reflected in how quickly spike counts build up.

2. **Precise spike timing**
    - Timing patterns of firing relative to input. Rate of firing over time.
    - Example: auditory neurons fire in sync with sound wave frequency (“neural entrainment”).![[Screenshot 2025-08-27 at 7.39.51 PM.png]]

- **Spike count encodes intensity:** more spikes = stronger stimulus.
- **Spike timing encodes rhythm/frequency:** neurons lock onto temporal patterns, especially in auditory and touch systems.
- Both mechanisms work together: **intensity (how much)** + **timing (when)** → richer encoding of perception.
# Methods to Study Perception

- **Controlled behavior**: accuracy, speed 
	- e.g., visual search → response time differences for target present vs absent
	- It takes longer to find the target (red triangle) in conjunction search.
- **Natural observational measures**: eye fixation, body/eye/face movements
- **Neural measures**:
	- Invasive: single neuron recordings
	- Noninvasive: fMRI (BOLD signal), DTI (white matter tracts), EEG, fNIRS, MEG

# Psychophysics: Linking Physical & Psychological

- Vary physical stimulus, measure psychological response
- Two types:
	- **Threshold** (responses just at the limits of perception)
	- **Supra-threshold** (responses to clearly perceivable events)

# Measuring thresholds: absolute and difference

- **Fundamental Abilities Measured**
    - **Detection threshold**: is something there?
    - **Discrimination threshold**: minimal detectable difference (JND)
    - **Binary choice**: A vs A’ discrimination
    - **Identification**: accuracy of identifying a stimulus
## Thresholds
- **Absolute threshold**: How much input do we need to tell something is happening?
- **Difference threshold (JND)**: How much of a diﬀerence do we need to tell one input from another?

- **Methods to measure absolute threshold**:
	- Method of Limits (ascending/descending series)
	- Method of Adjustment (subject freely adjusts stimulus)
	- Method of Constant Stimuli (fixed set, % detection across trials, fit function for threshold)
- **Methods to measure difference threshold**:
	- JND = “just noticeable difference”
	- The Weber fraction = $\frac{JND value}{standard value}$
    - **Weber’s Law**: JND is a constant fraction of baseline stimulus; smaller Weber fraction = greater sensitivity
    - **Applications**: measure perceptual ability, effects of age/impairment, practical uses (hearing loss, interface design)

# Supra-threshold Measures: Magnitude estimation

- Focus on stimuli clearly detectable
- Questions: How does perceived intensity scale with physical variation? Does scaling differ by dimension (brightness, weight, taste, etc.)?

- **Magnitude Estimation**
    - Subjects assign numbers proportional to perceived intensity (free scale, above threshold)
    - Produces **power functions**:
        - Perceived magnitude = $$K \times Stimulus^{n}$$
        - n < 1: means that each increase in stimulus magnitude counts **less** - diminishing returns (e.g., brightness, loudness)
        - n > 1: means that each increase in stimulus magnitude counts **more** - increasing returns (e.g., shock, heaviness)![[Screenshot 2025-09-02 at 2.32.20 AM.png]]
    - Different dimensions have characteristic exponents (e.g., brightness ≈ 0.5, heaviness ≈ 1.45, shock ≈ 3.5)
    - Data normalization needed due to individual scaling differences
    - Log-log transformation linearizes data, slope = exponent (n)
# Binary Discrimination (Supra-threshold)
- Different from threshold discrimination
- Signal detection approach: measure accuracy distinguishing between two above-threshold stimuli
- Vary stimulus differences to find % accuracy at target level (not 100%)

- **Applications of Psychophysics**
    - Provides core sensory metrics (e.g., Weber fractions)
    - Assesses sensory function and decline (aging, disease)
    - Tests human–computer interfaces and VR systems
    - Goals: maximize detection/discrimination, ensure wide perceptual range

## Signal Detection Basics

- Signal detection theory measures how well people discriminate between two conditions (signal vs. noise).
- On each trial, a signal or no-signal is presented, and the observer responds “signal” or “no signal.”
- Four possible outcomes: **Hit, Miss, False Alarm, Correct Rejection.**

## Outcomes and Measures

- Outcomes can be expressed as proportions (% hit, % miss, % false alarm, % correct rejection).
- Key measures:
    - **d′ (sensitivity):** Distance between signal and noise distributions in SD units.
    - **Criterion (c or β):** The threshold for deciding “signal.”
![[Screenshot 2025-09-02 at 10.06.18 AM.png]]
## Theoretical Model

- Internal responses to signal and noise are modeled as normal distributions.
- Mean response is higher for signal than for noise.
- Criterion determines boundary between “signal” and “no signal” judgments.
- **d′ increases** → distributions separate more → better sensitivity.
- **Shifting criterion** changes trade-off between hits and false alarms.

## Visualizations

- Probability curves illustrate how hits and false alarms depend on distributions and criterion.
    ![[Screenshot 2025-09-02 at 10.11.07 AM.png]]
- Examples:
    - High d′ → high hits.
    - Liberal criterion → more hits but also more false alarms.
    - Conservative criterion → fewer hits, fewer false alarms.
- $d'= z_1 + z_2$
## ROC Curves (Receiver Operating Characteristic)

- Moving criterion shifts point along the curve.
- Higher sensitivity (larger d′) shifts the ROC curve away from the diagonal.
- **Area under the curve (AUC)** is another measure of sensitivity (A = 0.5 = chance, A close to 1 = high sensitivity).
- ROC slope relates to β (criterion).
- confidence???

### Extensions to Machine Learning

- ML classifiers also make binary decisions (e.g., cancer detection).
- Similar measures apply:
    - **Precision:** Hits / (Hits + False Alarms).
    - **Recall:** Same as hit rate.
    - **Specificity:** Correct rejection rate.
    - **Accuracy:** Overall proportion correct.
- Precision/Recall curves are especially useful when signals are rare.

### Computation

- d’ (sensitivity in terms of distance between curves) =NORMSINV(HIT_PROP)-NORMSINV(FA_PROP) 
- Beta (criterion expressed by ratio of height of curves) =EXP((NORMSINV(FA_PROP)^2)- NORMSINV(HIT_PROP)^2)/2) 
- C (criterion expressed by x axis position, relative to point where curves cross, ie c = 0 where beta = 1) =-(NORMSINV(HIT_PROP)+NORMSINV(FA_PROP))/2 
- A (area under ROC curve measure of sensitivity) =NORMSDIST(D_PRIME/SQRT(2))
- Example worksheet shows calculations with given hit/false alarm rates.
