# Precision

$$
Pr=\frac{TP}{TP+FP}=\frac{TP}{\text{All Positive Predictions of the Classifier}}
$$

Out of all the times that you predicted positive, how many times were you correct about it?

Out of all the ones selected, how many were relevant.

# Recall

$$
Re=\frac{TP}{TP+FN}=\frac{TP}{\text{All Positive Samples Tested on}}
$$

Out of all the positive classes that you were tested on, how many were correctly predicted?

Out of all the ones that are relevant, how many were selected?

# Precision Recall Curve

* A true positive is returned only if the classifier is *confident*. Confidence is a threshold $\in [0,1]$. If the classifier output is a number greater than the threshold, it predicts the class as true, or else false. 
* Increasing the threshold means that the classifier has to be more certain, before it makes a prediction. 
* This will reduce the number of times it misclassifies a true class, i.e. reduce $FP$, thereby increasing precision.
* Reducing the threshold means that the classifier can output almost always, the true class.
* This will reduce the number of times it misclassifies a askdfas;d ;d