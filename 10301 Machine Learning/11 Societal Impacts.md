# Individual Fairness
**Statistical/Demographic Parity**
- Equal selection rate across different groups: $P(\hat{Y} = 1 | S = s_1) = P(\hat{Y} = 1 | S = s_2)$
**Equality of Accuracy**
- Equality of the prediction accuracy (L) across groups: $E[L(\hat{y}, y) | S = s_1] = E[L(\hat{y}, y) | S = s_2]$
**Equality of FPR/FNR** 
- Equality of the False Positive Rate (FPR) across groups: $P[\hat{Y}=1|Y =0, S = s1]=P[\hat{Y}=1|Y =0, S = s2]$
- Equality of the False Negative Rate (FNR) across groups: $P[\hat{Y}=0|Y =1, S = s1]=P[\hat{Y}=0|Y =1, S = s2]$
- Equality of Odds: equal FNR and FPR simultaneously
**Equality of PPV/NPV** 
- Equality of the Positive Predictive Value (PPV) $P[Y =1|\hat{Y}=1, S = s1]=P[Y=1|\hat{Y}=1, S = s2]$
- Equality of the Negative Predictive Value (NPV) $P[Y =0|\hat{Y}=0, S = s1]=P[Y =0|\hat{Y}=0, S = s2]$ 
- Predictive Value Parity (PVP): equal PPV and NPV simultaneously
**Common Pros and Cons**
- Ignoring possible correlation between Y and S. 
- Allowing for trading off different types of error. 
- Not considering practical considerations. 
	- e.g., High accuracy difficult to attain for small groups

![[Screen Shot 2024-03-27 at 19.51.20.png]]
## Summary of Fairness Notions w. Confusion Matrix

![[Screen Shot 2024-03-27 at 19.30.21.png]]
