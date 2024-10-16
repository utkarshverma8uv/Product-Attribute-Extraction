# **Product Attribute Extraction with ByT5 and BART Models**

This project involves fine-tuning ByT5 and BART models for extracting product properties such as height, weight, volume, and other attributes from OCR-generated text extracted using paddleOCR. The fine-tuned models are evaluated using metrics such as BLEU score, BERT score, and exact match in SI units.

![tasveer.png](https://github.com/utkarshverma8uv/Product-Attribute-Extraction/blob/main/tasveer.png)

## **Table of Contents**
- [Project Overview](#project-overview)
- [Datasets](#datasets)
- [Notebooks](#notebooks)
- [Preprocessing](#preprocessing)
- [Modeling](#modeling)
- [Evaluation](#evaluation)
- [Postprocessing](#postprocessing)
- [Results](#results)

## **Project Overview**

This project uses paddleOCR to extract textual data from product images. Then, transformer models are fine-tuned on extracted text to extract numeric product attributes from textual data (OCR outputs). By leveraging the ByT5 (Byte-to-Byte) and BART (Bidirectional and Auto-Regressive Transformers) models, we can efficiently handle varying input lengths and improve prediction accuracy for product attributes.

Both models were fine-tuned using a custom attention mechanism to emphasize the target entity in the input sequence, ensuring better performance for the attribute extraction task.

## **Datasets**

The dataset includes:
1. **train.cs**: The original Amazon ML challenge25' dataset
2. **sample_1000**: A sampled dataset from the training dataset containing 8000 rows, 1000 rows for each type of entity type.
3. **sample_1000_ocr.csv**: Extracted texts for sample_1000 dataset.
4. **byt5_data_1000_att.csv**: Preprocessed dataset used for finetuning-ByT5-small.
5. **bart_data_1000.csv**: Preprocessed dataset used for finetuning-BART-small.
6. **byt5_output.csv**: Output generated by finetuned-byt5 model.
7. **bart_output.csv**: Output generated by finetuned-bart model.

For ByT5, shorter numeric sequences are extracted to match its byte-based tokenizer. BART uses longer sequences as it can handle larger inputs.

## **Notebooks**

### 1. Text Extraction Notebook
This notebook focuses on extracting text from images using OCR techniques. The extracted text is preprocessed and cleaned for further use. Key steps include:
- Random sampling of the training dataset.
- Applying OCR to images.

### 2. Fine-Tuning Notebook
This notebook is dedicated to fine-tuning BART and ByT5 models for extracting product attributes like weight, height, and volume from the preprocessed text. Key steps include:
- Cleaning and filtering the text using regular expressions.
- Preparing the data for the fine-tuning process.
- Preprocessing and tokenizing the dataset.
- Modifying the attention mechanism for the ByT5 model.
- Fine-tuning both models and evaluating their performance using metrics like BLEU, BERT, and Exact Match scores.

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

The predicted attribute values are converted to SI units using the `pint` library, ensuring consistency in unit measurements. A custom function checks for numeric values and unit matching between the predicted and target values.

## **Results**

The fine-tuned ByT5 model yielded the best results with:
- **BLEU Score**: 50.51
- **Exact Match Score (in SI Units)**: 0.65
- **BERT f1 Score**: 0.94

---
## **License**

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---
