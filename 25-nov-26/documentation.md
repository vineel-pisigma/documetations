# Smizzer – Parsing Feature Improvements & OCR Research

**Name:** K. Vineel  
**Work:** Improving the parsing feature of Smizzer  
**Project:** Smishing Detector

## 1. Package and Environment Updates

- Updated project packages to support the latest compatible environments.
- Investigated issues in the existing OCR preprocessing and text parsing pipeline.

## 2. Issues Identified in the Existing Text Preprocessing Pipeline

### 2.1 Text Correction Can Modify Genuine Message Content

**File:** `backend/app/services/text_corrector.py`

**Finding:**

The raw OCR-extracted text is passed directly into the text correction module. The correction logic makes assumptions about OCR mistakes and automatically replaces certain character combinations.

Examples:
- `rn` → `m`
- `0` → `o`
- `1` → `i`

**Problems:**
- Correction rules cannot reliably distinguish OCR errors from genuine message content.
- Valid words and message content can be unintentionally modified.
- URLs, banking terminology, transaction identifiers, and other important text may be affected.

**Potential impact:** Loss of original message information and incorrect smishing classification.

### 2.2 Whitespace Normalization Destroys Message Layout

**Affected files:**
- `backend/app/services/text_corrector.py`
- `backend/app/utils/text_analyzer.py`

**Finding:**

The implementation collapses repeated whitespace and joins extracted text using whitespace splitting.

For example, an OCR result containing separate lines:

```text
HDFC BANK

Your account will be blocked.

Click here:
https://example.com/verify
```

Can become:

```text
HDFC BANK Your account will be blocked. Click here: https://example.com/verify
```

**Problems:**
- Original line breaks and paragraph boundaries are lost.
- Message structure and contextual relationships are affected.
- Important formatting information may be lost during analysis.

**Potential impact:** Reduced parsing accuracy and loss of contextual information.

### 2.3 URL Correction Can Modify Genuine Domains

**File:** `backend/app/services/text_corrector.py`

**Finding:**

The URL correction logic replaces character combinations such as:
- `.corn` → `.com`
- `.c0m` → `.com`
- `.orq` → `.org`

**Problems:**
- A genuine domain may be modified unintentionally.
- The corrected URL may not match the actual URL displayed in the screenshot.
- Original suspicious domains may be lost.

**Potential impact:** Incorrect URL extraction and potentially unreliable domain-based threat analysis.

## 3. Research on Alternative OCR Engines

Investigated alternative OCR solutions that could improve text extraction while supporting CPU-based and Docker deployments.

### 3.1 OCR Solutions Evaluated

| OCR Engine | Key Consideration |
|---|---|
| PaddleOCR | Existing solution; good text and layout extraction capabilities |
| EasyOCR | Alternative OCR engine with relatively straightforward integration |
| Surya OCR | Document-oriented OCR; investigate model-weight licensing |
| DeepSeek-OCR | Vision-language-based document OCR approach |
| Tesseract 5 | Lightweight OCR engine suitable for CPU deployment |
| docTR | Deep-learning-based OCR framework |

DeepSeek-OCR repository:

https://github.com/deepseek-ai/DeepSeek-OCR

### 3.2 Preliminary Resource Comparison

The following are approximate research estimates, not measurements from a controlled benchmark.

| OCR Engine | Model Files | RAM After Loading | Peak RAM |
|---|---:|---:|---:|
| PaddleOCR PP-OCRv5 Mobile | 15–30 MB | 300–700 MB | 400–900 MB |
| PaddleOCR PP-OCRv5 Server | 100–200 MB | 700 MB–1.5 GB | 1–2 GB |
| PaddleOCR PP-OCRv6 Tiny | Variant-dependent | 400–800 MB | 500 MB–1 GB |
| PaddleOCR PP-OCRv6 Small | Variant-dependent | 600 MB–1.2 GB | 800 MB–1.5 GB |
| PaddleOCR PP-OCRv6 Medium | 1–2 GB | 1–2 GB | 1.5–3 GB |
| EasyOCR | 100–200 MB | 500 MB–1.2 GB | 700 MB–1.8 GB |
| Tesseract 5 | 10–50 MB per language | 50–200 MB | 100–400 MB |
| docTR | Model-dependent | 500 MB–1.5 GB | 800 MB–2 GB |

### 3.3 OCR Capability Comparison

| OCR Engine | Text Accuracy Potential | URL Extraction Suitability | CPU Support | Docker |
|---|---|---|---|---|
| PP-OCRv6 | High | Very good | Yes | Yes |
| PP-OCRv5 | High | Very good | Yes | Yes |
| EasyOCR | Good | Good | Yes | Yes |
| Tesseract 5 | Good on clean text | Moderate | Yes | Yes |
| docTR | High | Good | Yes | Yes |

These ratings are preliminary assessments. Actual performance needs to be validated using the same screenshot dataset.

## 4. Research on Smishing and Spam Detection Models

Investigated existing machine learning models that could help identify suspicious messages.

### 4.1 OpenTextShield

https://huggingface.co/telecomsxchange/OpenTextShield

An existing text-based security model investigated as a potential candidate for suspicious message detection.

### 4.2 Scam Shield DistilBERT

https://huggingface.co/rasheq/scam-shield-distilbert

A DistilBERT-based model investigated for scam and suspicious message classification.

### 4.3 Qwen2.5-VL-3B Email/SMS Spam Model

https://huggingface.co/SourinC/qwen2.5-vl-3b-email-sms-spam-rslora-4bit

Investigated a vision-language model that accepts screenshots directly.

**Potential advantages:**
- Can process visual input and text together.
- May identify suspicious message content directly from screenshots.
- Could reduce dependence on a separate OCR pipeline for certain use cases.

Further evaluation is required for inference speed, memory requirements, detection accuracy, and CPU compatibility.

## 5. Research on Existing Smishing Detection Solutions

Reviewed existing projects and solutions to understand their approaches to screenshot-based phishing and smishing detection.

1. **GemmaGuard Android:**  
   https://github.com/GemmaGuard/gemma-guard-android

2. **GemScan:**  
   https://gemscan.github.io/GemScan/

3. **LureLit AI:**  
   https://lurelit.ai/

4. **Research Paper:**  
   https://www.mdpi.com/1424-8220/24/18/6084

## 6. Planned Next Steps

1. Remove the existing automatic text correction logic from the preprocessing pipeline.
2. Preserve the original OCR output without modifying genuine message content.
3. Benchmark EasyOCR for raw text extraction using the same screenshot dataset.
4. Compare OCR engines based on:
   - Text extraction accuracy.
   - URL extraction accuracy.
   - Preservation of line breaks and message layout.
   - CPU inference time.
   - RAM consumption.
   - Docker deployment requirements.
5. Investigate existing screenshot-based smishing detection solutions.
6. Evaluate suitable text classification and vision-language models for suspicious message detection.

**Current Focus:** Improving the reliability of OCR text extraction and parsing before introducing additional correction or classification mechanisms.