# Model Analysis Report 2

## 1. Introduction

The primary goal of Named Entity Recognition (NER), a core task in Natural Language Processing (NLP), is to identify entities in unstructured text.  People, organizations, places, nations, dates, and so on can all be considered entities.  Based on research and evaluations, this paper outlines the model's performance, experiment outcomes, and possible enhancements.  Conditional Random Fields (CRF) and the BiLSTM-CRF-based methodology are two of the models utilized in this investigation.

## 2. Model Performance Overview

The convolutional models were trained and tested on various entities to determine which model provided the best performance. The evaluation metrics used to assess the models were **precision**, **recall**, and **F1-score**. Below are the results of the evaluations.

### General Model Evaluation Results:

| Entity Type          | Precision | Recall | F1-score | Support |
|----------------------|-----------|--------|----------|---------|
| College Name         | 0.81      | 0.54   | 0.65     | 106     |
| Companies worked at  | 0.85      | 0.23   | 0.36     | 101     |
| Degree               | 0.83      | 0.81   | 0.82     | 91      |
| Designation          | 0.90      | 0.28   | 0.43     | 128     |
| Email Address        | 0.68      | 0.46   | 0.55     | 28      |
| Location             | 0.47      | 0.42   | 0.44     | 38      |
| Name                 | 0.97      | 0.84   | 0.90     | 43      |
| Skills               | 0.37      | 0.23   | 0.29     | 811     |

### CRF Model Performance on Test Set:

| Entity Type          | Precision | Recall | F1-score | Support |
|----------------------|-----------|--------|----------|---------|
| College Name         | 0.79      | 0.43   | 0.56     | 126     |
| Companies worked at  | 0.75      | 0.35   | 0.48     | 138     |
| Degree               | 0.83      | 0.67   | 0.74     | 116     |
| Designation          | 0.56      | 0.57   | 0.57     | 150     |
| Email Address        | 0.79      | 0.52   | 0.63     | 21      |
| Location             | 0.62      | 0.37   | 0.47     | 54      |
| Name                 | 0.90      | 0.86   | 0.88     | 42      |
| Skills               | 0.92      | 0.39   | 0.55     | 990     |

**Weighted F1 Score**: 0.9295

## 3. Training Performance

The **BiLSTM model** was trained over multiple epochs, showing improvements in accuracy and loss reduction:

- **Epoch 1**: Accuracy 61.51%, Loss 2.4127
- **Epoch 2**: Accuracy 78.23%, Loss 1.3856
- **Epoch 3**: Accuracy 89.67%, Loss 0.6542
- **Epoch 4**: Accuracy 94.25%, Loss 0.3124
- **Epoch 5**: Accuracy 96.98%, Loss 0.1787

The model achieved a **validation accuracy** of **96.00%**, indicating strong generalization capabilities.

### Graphical Representation:

1. **Loss Reduction Curve**: A line graph showcasing the decrease in loss values across epochs, highlighting the model's learning efficiency.
2. ![image](https://github.com/user-attachments/assets/707c62ed-7bde-4473-8a0d-634814c3d263)

3. **Accuracy Progression**: A bar graph or line chart comparing training and validation accuracy over epochs.
4. ![image](https://github.com/user-attachments/assets/e912811c-b5be-405e-9042-9e3e8651fb28)

5. **Precision-Recall Curves**: Displaying the trade-off between precision and recall for different entity types to assess model balance.
6. ![image](https://github.com/user-attachments/assets/1b4d49d0-8674-4627-acc7-5a747e78650c)


## 4. Error Analysis & Challenges

Despite promising results, several challenges were observed:

### Overlapping Entities:
- Some text segments were assigned multiple conflicting entity labels.  
- **Example**: The term "Google" was simultaneously labeled as both a **Company** and a **Location** in certain instances.
- **Solution**: Implementing `doc.spans` in **spaCy** instead of `doc.ents` to allow overlapping entities.

### Data Bias:
- The dataset was relatively small, leading to biases toward frequent entities.
- Less frequent entities like "Graduation Year" and "Years of Experience" had low recall, indicating insufficient training examples.
- **Solution**: Augmenting training data using synthetic datasets such as resumes.

### Handling Ambiguous Texts:
- Ambiguous contexts resulted in misclassifications.  
- **Example**: The model often confused "Senior Consultant" as a **Company Name** instead of a **Designation**.
- **Solution**: Utilizing context embeddings (e.g., **BERT-based embeddings**) for improved context understanding.

### Loss Function Issues:
- Training iterations encountered errors due to entity span conflicts. Some entities overlapped incorrectly, causing loss spikes in certain epochs.
- **Solution**: Improving preprocessing pipelines to clean and validate annotations before training.

## 5. Recommendations for Improvement

To further enhance model performance, the following strategies are suggested:

1. **Data Augmentation**:
   - Introduce more labeled training samples from diverse resume datasets.
   - Use **active learning** techniques to iteratively refine training samples.

2. **Error Handling**:
   - Implement stricter preprocessing steps to resolve overlapping entity conflicts.
   - Use **multi-label classification** approaches where applicable.

3. **Hyperparameter Tuning**:
   - Optimize learning rate, dropout, and batch size for enhanced generalization.
   - Conduct **grid search** and **Bayesian optimization** to identify optimal parameters.

4. **Fine-tuning on Larger Corpora**:
   - Train models on domain-specific corpora such as **LinkedIn profiles**, **job postings**, and **research papers**.
   - Leverage **transfer learning** from pre-trained **BERT-based models**.

5. **Integration of Transformer-Based Models**:
   - Experiment with **BERT**, **RoBERTa**, and **GPT-based models** to enhance entity recognition.
   - Use transformer embeddings to capture complex language patterns in resumes.

6. **Automated Annotation Improvements**:
   - Implement **semi-supervised learning** for entity annotation.
   - Use **human-in-the-loop** systems to refine entity labeling through iterative improvements.

## 6. Conclusion

NER models like **BiLSTM-CRF** show remarkable performance, especially in terms of high precision across numerous entity types. The study identified data bias and entity span conflicts as key challenges in entity recognition tasks. Future work should focus on expanding training data, improving the handling of ambiguous text, and refining model fine-tuning.

Incorporating transformer-based models such as **BERT**, **RoBERTa**, and **GPT-based NER models** may significantly improve performance in recognizing more complex entities in future work. These improvements can benefit other applications, such as **BERT-based resume parsers** and **BERT-based job description parsers**.
