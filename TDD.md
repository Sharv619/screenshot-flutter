# Technical Design Document (TDD)

## 1. Architecture Overview
The application will follow a **Feature-First Layered Architecture**:
- **Presentation:** Flutter Widgets, State Management (Riverpod).
- **Domain:** Entities, Use Cases (AnalyzeScreenshot, CategorizeImage).
- **Data:** Repositories, Data Sources (Local DB, ML Kit API, File System).

## 2. Detection Mechanism
- **Android:** Uses a `ContentObserver` on `MediaStore.Images` to detect new files in the `Screenshots` bucket.
- **iOS:** Uses `PHPhotoLibraryChangeObserver` to detect new additions to the Photo Library with specific metadata filters.

## 3. ML Pipeline
1. **Input:** Newly detected image file path.
2. **Preprocessing:** Resize/Compress for faster ML analysis (if needed).
3. **Inference:**
   - **Text Recognition:** Extract all visible text.
   - **Image Labeling:** Generate top 5 confidence labels.
4. **Classification:**
   - Text is passed through a keyword extractor.
   - Labels and Keywords are weighted.
   - Output: Most probable category.

## 4. Database Schema (Isar)
```dart
@collection
class Screenshot {
  Id id = Isar.autoIncrement;
  late String path;
  late DateTime createdAt;
  late String category;
  List<String>? tags;
  String? extractedText;
}
```

## 5. Security & Privacy
- Zero Network Usage: The app will not have the `INTERNET` permission in production unless strictly necessary for crash reporting (and even then, opt-in).
- Local-only processing: All ML models are bundled or downloaded once (ML Kit).
