# Extractive-QA
Build a transformer based pipeline for context based question answering
Here's a draft for the README file you can use for your GitHub project:

---

# **Product Attribute Extraction with ByT5 and BART Models**

This project involves fine-tuning ByT5 and BART models for extracting product properties such as height, weight, volume, and other attributes from OCR-generated text. The fine-tuned models are evaluated using metrics such as BLEU score, BERT score, and exact match in SI units.

## **Table of Contents**
- [Project Overview](#project-overview)
- [Datasets](#datasets)
- [Preprocessing](#preprocessing)
- [Modeling](#modeling)
- [Evaluation](#evaluation)
- [Postprocessing](#postprocessing)
- [Results](#results)
- [Installation](#installation)
- [Usage](#usage)

## **Project Overview**

The aim of this project is to extract numeric product attributes from textual data (OCR outputs) using fine-tuned transformer models. By leveraging the ByT5 (Byte-to-Byte) and BART (Bidirectional and Auto-Regressive Transformers) models, we can efficiently handle varying input lengths and improve prediction accuracy for product attributes.

Both models were fine-tuned using a custom attention mechanism to emphasize the target entity in the input sequence, ensuring better performance for the attribute extraction task.

## **Datasets**

The dataset includes:
1. **OCR Text**: Extracted text from product images using an OCR engine.
2. **Target Values**: The actual product attribute values that need to be predicted.

For ByT5, shorter numeric sequences are extracted to match its byte-based tokenizer. BART uses longer sequences as it can handle larger inputs.

## **Preprocessing**

The OCR text is preprocessed to extract relevant numeric substrings using regex. These substrings focus on the context around numeric values within the text.

- **For ByT5**: A substring of 2 characters before and 10 after any numeric character.
- **For BART**: A substring of 50 characters before and after any numeric character.

In addition, a prefix is added to the input, indicating the specific entity being extracted (e.g., `extract weight` or `extract height`).

## **Modeling**

### Fine-Tuning ByT5 and BART
- **ByT5**: The fine-tuned ByT5 model handles byte-level tokenization, with custom attention mechanisms to focus more on the start of the sequence where the entity name resides.
- **BART**: The BART model is trained using default attention masks.

Tokenization and sequence truncation are applied based on the input sequence lengths, with a maximum length of 200 tokens.

## **Evaluation**

The models are evaluated using the following metrics:
- **BLEU Score**: To measure the similarity between predicted and target sequences.
- **BERT Score**: To compute the semantic similarity using the BERT embedding space.
- **Exact Match Score in SI Units**: Measures the accuracy of the extracted numeric value and unit, after converting the predicted and target values to SI units.

## **Postprocessing**

The predicted attribute values are converted to SI units using the `pint` library, ensuring consistency in unit measurements. A custom function checks for both numeric and unit matching between the predicted and target values.

## **Results**

The fine-tuned ByT5 model yielded the best results with:
- **BLEU Score**: 50.51
- **Exact Match Score (in SI Units)**: 0.65
- **BERT f1 Score**: 0.94

## **Installation**

To set up this project locally, follow these steps:

1. Clone the repository:
   ```bash
   git clone https://github.com/your-username/product-attribute-extraction.git
   cd product-attribute-extraction
   ```

2. Install the necessary dependencies:
   ```bash
   pip install -r requirements.txt
   ```

## **Usage**

To fine-tune and evaluate the models, follow these steps:

1. **Preprocessing**: Load the dataset and preprocess the OCR text.
   ```bash
   python preprocess.py
   ```

2. **Model Fine-Tuning**: Train the ByT5 and BART models by running the fine-tuning script.
   ```bash
   python fine_tune.py
   ```

3. **Evaluation**: Evaluate the models on the test dataset.
   ```bash
   python evaluate.py
   ```

4. **Inference**: Generate predictions for new product descriptions.
   ```bash
   python inference.py
   ```

## **Contributing**

Contributions are welcome! If you find any issues or have suggestions for improvement, feel free to open a pull request or an issue.

## **License**

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

This README provides an overview of the project, guides users on how to set it up, and outlines the steps to fine-tune and evaluate the models. Feel free to customize it further to fit your specific project details!
