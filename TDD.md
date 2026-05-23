# Technical Design Document (TDD) - Screenshot Action Inbox

## Architecture Overview

Screenshot Action Inbox will use a feature-first layered architecture. The design should support manual import first, share-to-app second, and optional monitoring later. Automatic screenshot detection is not the only ingestion method and should not be the default MVP assumption.

## Layers

### Flutter Presentation Layer

- Inbox screen for imported screenshots.
- Review screen with OCR text, extracted fields, and action cards.
- Saved lists for books, places, receipts, and coupons.
- Local search UI.
- Permission education and request states.
- Cleanup prompt UI that leads into native OS deletion confirmation.

### Domain Use Cases

- `ImportScreenshot`
- `ReceiveSharedScreenshot`
- `RunOcr`
- `NormalizeExtractedText`
- `ExtractEntities`
- `ScoreCategory`
- `GenerateActionCards`
- `SaveExtractedAction`
- `SearchSavedItems`
- `ApplyUserCorrection`
- `RequestScreenshotDeletion`

### Data Repositories

- `ScreenshotRepository`
- `ExtractedActionRepository`
- `CollectionRepository`
- `UserCorrectionRepository`
- `EnrichmentCacheRepository`

### Platform Adapters

- Photo picker adapter.
- Share intent / share extension adapter.
- Selected-photo access adapter.
- OCR adapter.
- App launcher adapter for maps, browser, books, or other target apps.
- Native delete request adapter.
- Permission manager.

## Ingestion Methods

Priority order:

1. **Manual screenshot selection:** safest MVP path with clear user intent.
2. **Share-to-app flow:** user sends a screenshot directly into the inbox from the photo library or another app.
3. **Optional selected-photo access / screenshot folder monitoring:** useful after MVP validation, with careful permission messaging.
4. **Future background monitoring:** only where platform policies, battery rules, and store review constraints allow it.

Platform notes:

- Android should prefer Android Photo Picker and Selected Photos Access where available, with MediaStore used for deeper media workflows.
- iOS should use PhotoKit for selected assets and deletion workflows.
- Background monitoring must be treated as optional and platform-dependent.

## Core Services

### OCR Service

- Android: Google ML Kit text recognition.
- iOS: Apple Vision or equivalent platform OCR.
- Input: local image asset or temporary file.
- Output: raw text blocks, bounding boxes if available, confidence metadata if available.

### Rule-Based Classifier

- Uses normalized OCR text and simple metadata.
- Scores MVP categories:
  - Books
  - Places / Restaurants
  - Receipts / Coupons
- Does not require cloud AI.
- Should expose confidence scores and reasons for debugging.

### Action Card Generator

- Converts extracted entities and category scores into user-facing actions.
- Examples:
  - Save book to reading list.
  - Open place in maps.
  - Save receipt details.
  - Save coupon code and expiry.
- Must support low-confidence states and user correction.

### Local Database

Use Isar or Drift for local persistence. The database should store imported screenshot metadata, extracted text, structured actions, saved lists, user corrections, and optional enrichment cache.

### Permission Manager

- Requests permissions at the point of need.
- Keeps manual import available without broad library access where possible.
- Tracks denied, limited, and granted states.
- Provides copy suitable for App Store and Play Store review.

### Optional Enrichment Clients

- Google Books API client for book metadata.
- Google Places API client for place details.
- Disabled by default.
- Must only send extracted fields needed for enrichment.
- Must fail gracefully without blocking local save.

## Example Data Models

```dart
class ScreenshotItem {
  String id;
  String? platformAssetId;
  String? localThumbnailPath;
  DateTime importedAt;
  String source; // manual, share, selected_access, monitor
  String processingStatus; // pending, processed, failed
  String? extractedText;
  String? cleanupStatus; // not_prompted, prompted, deleted, declined, failed
}

class ExtractedAction {
  String id;
  String screenshotId;
  String category; // book, place, receipt, coupon
  String title;
  Map<String, Object?> fields;
  List<String> suggestedActions;
  double confidence;
  bool saved;
  DateTime createdAt;
}

class SavedList {
  String id;
  String name;
  String type; // reading, places, receipts, coupons
  DateTime createdAt;
  DateTime updatedAt;
}

class UserCorrection {
  String id;
  String screenshotId;
  String fieldName;
  String previousValue;
  String correctedValue;
  DateTime correctedAt;
}
```

## Processing Pipeline

Screenshot import
-> OCR
-> text normalization
-> entity extraction
-> category scoring
-> action card generation
-> user action
-> local persistence
-> optional cleanup prompt

Detailed flow:

1. User manually selects or shares a screenshot.
2. App creates a `ScreenshotItem` record.
3. OCR service extracts text locally.
4. Normalizer cleans whitespace, casing, URLs, currency, dates, and common OCR artifacts.
5. Entity extractor identifies candidate book titles, authors, places, addresses, merchants, totals, dates, and coupon codes.
6. Rule-based classifier scores the screenshot against supported MVP categories.
7. Action card generator creates one or more cards.
8. User saves a card, edits extracted fields, opens an external app, or dismisses the suggestion.
9. Saved details are persisted locally.
10. If appropriate, the app asks whether to delete the original screenshot.
11. Native OS confirmation handles deletion.

## Deletion Flow

- Android: use `MediaStore.createDeleteRequest` so the OS shows confirmation.
- iOS: use `PHAssetChangeRequest` through PhotoKit so deletion is confirmed by the OS.
- The app must never silently delete.
- If deletion is declined, the saved item remains intact.
- If deletion fails, the app records failure state and allows retry where appropriate.

## Local-only Mode

Default mode:

- No screenshot upload.
- OCR runs on-device.
- Classification uses deterministic rules.
- Action cards are generated locally.
- Search and saved lists are local.
- Enrichment clients are disabled.

## Optional Enrichment Mode

Opt-in mode:

- Google Books API can enrich book cards.
- Google Places API can enrich place cards.
- Only extracted fields required for lookup should be sent.
- The app must disclose network usage before enabling enrichment.
- Network failures should not block local processing.

## Error Handling

- OCR failure: keep the screenshot item and allow retry or manual entry.
- Low confidence: show editable action cards instead of asserting a result.
- Permission denied: keep manual alternatives available.
- Missing external app: fall back to browser links where available.
- API failure: save local extracted data and mark enrichment unavailable.
- Delete request failure: keep saved data and show a retryable cleanup status.

## Battery and Performance Considerations

- Run OCR only on explicitly imported/shared screenshots in MVP.
- Queue processing and avoid blocking the UI.
- Generate thumbnails for review instead of loading full-resolution images repeatedly.
- Cache OCR and classifier results.
- Defer background monitoring until after battery and policy validation.
- Batch optional processing only with user consent and clear progress states.

## Testing Strategy

- Unit tests for text normalization, entity extraction, category scoring, and action card generation.
- Repository tests for persistence and search.
- Platform adapter tests with mocked permissions, OCR results, app launches, and deletion responses.
- Golden/UI tests for action cards, low-confidence states, saved lists, and cleanup prompts.
- Manual device testing for Android Photo Picker, Selected Photos Access, MediaStore deletion, iOS PhotoKit import, and iOS deletion confirmation.
- Privacy review tests verifying local-only mode avoids enrichment network calls.
