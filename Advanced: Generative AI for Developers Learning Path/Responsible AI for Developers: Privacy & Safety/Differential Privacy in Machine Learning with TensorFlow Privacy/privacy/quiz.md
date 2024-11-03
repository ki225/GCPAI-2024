1. In machine learning, privacy measures often introduce trade-offs with other important factors. Which of the following does NOT represent a typical trade-off?
   - [x] Privacy and Transparency
   - [ ] Privacy and Fairness
   - [ ] Privacy and Model Performance
   - [ ] Privacy and Training Efficiency

2. You've applied various de-identification techniques to a customer dataset containing sensitive information before using it to train an AI model. Which of the following concepts should you use to evaluate the suitability of your de-identification techniques?
   - [ ] Productivity and efficiency.
   - [x] Reversibility and referential integrity.
   - [ ] Scalability and efficiency.
   - [ ] Complexity and time consumption.

3. A customer is working with a large dataset of healthcare records to develop a diagnostic AI model. They want to ensure privacy of sensitive data, by restricting the contribution of specific data on the model during training. Which privacy-focused technique would be best for the customer?
   - [ ] TFF (TensorFlow Federated)
   - [x] DP-SGD (Differentially Private - Stochastic Gradient Descent)
   - [ ] Data perturbation
   - [ ] Federated Learning
> DP-SGD carefully adds calculated noise into the training process to protect individual contributions
