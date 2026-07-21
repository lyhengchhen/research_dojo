# Cross Entropy
Cross Entropy loss measure how far a predicted probability distribution is from the true distribution. 
- Standard loss function for classification problems 
$$L = -\sum_{i=1}^{C}y_{i}log(\hat{y_{i}})$$
Where the $y$ is the true label (one hot encoded) and $\hat{y}$ is the predicted probability vector
and since $y$ is one-hot (a 1 for the correct class, 0 elsewhere), this gives us:
$$L = -ln(\hat{y}_{\text{correct class}})$$
because
- If the model predicts probability to close to 1 for the correct class -> $-log(1) = 0$ almost no loss (ideal prediction)
- If the model predicts probability close to 0 the for correct class -> $-log(0) = -\infty$ which is a huge loss. 

This create a strong pressure to push **predicted probability mass** toward the correct answer, and the penalty grows sharply as confidence in the wrong answer increases. 

### Cross Entropy + Softmax + BackProp 
```
import numpy as np

def softmax(z):
    # subtract max for numerical stability (log-sum-exp trick)
    exp_z = np.exp(z - np.max(z))
    return exp_z / np.sum(exp_z)

def cross_entropy_loss(z, true_class_idx):
    probs = softmax(z)
    loss = -np.log(probs[true_class_idx])
    return loss, probs

def gradient(probs, true_class_idx, num_classes):
    y_onehot = np.zeros(num_classes)
    y_onehot[true_class_idx] = 1
    return probs - y_onehot   # this is the (ŷ - y) gradient!

# --- Example ---
logits = np.array([2.0, 0.5, -1.0])   # raw scores for 3 classes
true_class = 0                        # correct class is class 0

loss, probs = cross_entropy_loss(logits, true_class)
grad = gradient(probs, true_class, num_classes=3)

print("Probabilities:", probs)
print("Loss:", loss)
print("Gradient wrt logits:", grad)
```