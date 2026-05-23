# Product Requirements Document (PRD) - Screenshot Auto-Categorizer

## 1. Introduction
The Screenshot Auto-Categorizer is a mobile application built with Flutter that automatically organizes screenshots taken on a device. It uses on-device machine learning (OCR and Image Classification) to understand the content of screenshots and categorize them into meaningful folders.

## 2. Target Audience
- Users who take frequent screenshots for research, shopping, work, or personal memory.
- Users looking for a better way to organize their mobile captures without manual sorting.

## 3. Goals
- **Automation:** Detect screenshots and categorize them without user intervention.
- **Privacy:** Perform all analysis on-device (no cloud uploads).
- **Usability:** Provide a clean, intuitive gallery interface.

## 4. Key Features
### 4.1. Screenshot Detection
- Listen for new images added to the system's screenshot directory.
- Background service to handle detection even when the app is closed.

### 4.2. Content Analysis
- **OCR (Optical Character Recognition):** Extract text from screenshots.
- **Image Labeling:** Identify objects or scenes (e.g., "Food", "Text", "Clothing").
- **Metadata Extraction:** Date, time, and source (if available).

### 4.3. Categorization Engine
- Use extracted text and labels to assign categories.
- Primary Categories: Finance, Social Media, Shopping, Work, Personal, Education.
- Support for KNN or rule-based logic for improved accuracy.

### 4.4. Gallery & Organization
- Custom gallery view organized by folders/categories.
- Search functionality based on extracted text.
- Manual move/tagging capabilities.

## 5. Technical Stack
- **Framework:** Flutter
- **ML Engine:** Google ML Kit (Android) & Apple Vision Framework (iOS).
- **Database:** Isar or SQLite for local metadata storage.
- **State Management:** Provider or Riverpod.

## 6. Constraints & Risks
- **Permissions:** High dependency on "Storage" and "Background Execution" permissions.
- **Platform Limitations:** iOS background execution constraints compared to Android.
- **Performance:** On-device ML analysis must be battery-efficient.
