# Screenshot Action Inbox

A privacy-first screenshot action inbox that extracts useful information from screenshots, saves it into lists, opens the right app, and helps delete the original clutter.

## Product Vision

Screenshot Action Inbox turns screenshots from passive image clutter into searchable, structured, actionable items. Instead of asking users to manually sort screenshots into folders, the app extracts useful details locally, suggests simple action cards, saves important information into lists, and offers a cleanup prompt when the original screenshot is no longer needed.

Categorization remains part of the engine, but the product value is extraction, action cards, local search, saved lists, and cleanup.

## Project Status

This repository currently contains planning documentation only. Flutter source code has not been initialized.

## Core Features

- Manual screenshot import from the photo library.
- Share-to-app flow for sending screenshots into the inbox.
- On-device OCR for extracting visible text.
- Rule-based entity extraction for books, places/restaurants, receipts, and coupons.
- Action cards that suggest useful next steps.
- Saved lists and collections for extracted details.
- Local search across saved text, entities, and actions.
- Optional cleanup prompt after information is saved or acted on.
- Native OS-level confirmation before deleting any original screenshot.

## Planned Tech Stack

- **Framework:** Flutter
- **State management:** Riverpod
- **Local persistence:** Isar or Drift
- **Android OCR:** Google ML Kit
- **iOS OCR:** Apple Vision or equivalent platform OCR
- **Android media access:** Android Photo Picker, Selected Photos Access, MediaStore
- **iOS media access:** PhotoKit
- **Optional enrichment APIs:** Google Books API, Google Places API

## Architecture Overview

The planned app uses a feature-first layered architecture:

- **Presentation:** Flutter screens, action cards, list views, permission states, and cleanup prompts.
- **Domain:** use cases for importing screenshots, running OCR, extracting entities, generating action cards, saving details, searching locally, and requesting cleanup.
- **Data:** repositories for screenshots, extracted actions, collections, user corrections, and enrichment cache.
- **Platform adapters:** photo picker, share extension/intent handling, OCR, app links, and native delete requests.

## Processing Pipeline

1. User imports or shares a screenshot.
2. The app stores local metadata for the imported item.
3. OCR extracts visible text on-device.
4. Text is normalized for matching and search.
5. Entity extraction identifies books, places/restaurants, receipts, coupons, dates, prices, URLs, and addresses where possible.
6. A rule-based classifier scores likely categories.
7. The app generates action cards such as save to reading list, open maps, save receipt details, or add coupon.
8. The user chooses an action.
9. Structured details are saved locally.
10. The app may offer a cleanup prompt.
11. Deletion, if requested, is handled through native OS-level confirmation.

## Example Data Model

```dart
class ScreenshotItem {
  String id;
  String localAssetId;
  DateTime importedAt;
  String? thumbnailPath;
  String? extractedText;
  String processingStatus;
}

class ExtractedAction {
  String id;
  String screenshotId;
  String category; // books, places, receipts, coupons
  String title;
  Map<String, Object?> structuredData;
  double confidence;
  bool saved;
}

class SavedList {
  String id;
  String name;
  String type;
  DateTime updatedAt;
}
```

## Privacy Direction

Core processing is local-only. OCR, entity extraction, classification, action card generation, local search, and saved lists should work without sending screenshot content to external services.

The app should avoid positioning itself as a heavy AI assistant. The MVP foundation is platform OCR plus deterministic and rule-based classification.

## Network Usage

### Local-only mode

- Default mode.
- No screenshot content is uploaded.
- OCR and classification run on-device.
- Saved lists and search remain local.
- Cleanup prompts use native OS workflows.

### Optional enrichment mode

- User-enabled only.
- May use external APIs to enrich already extracted details.
- Candidate APIs include Google Books API for book metadata and Google Places API for place details.
- The app should clearly disclose what data is sent and why.
- Enrichment should never be required for the core inbox workflow.

## Permission Strategy

- Start with manual import and share-to-app to keep permissions narrow.
- Prefer Android Photo Picker and iOS selected-photo access over broad library access.
- Request expanded photo permissions only when the user opts into workflows that need them.
- Treat automatic screenshot monitoring as a later optional feature, subject to platform policy and store review.
- Never silently delete screenshots.
- Use `MediaStore.createDeleteRequest` on Android and `PHAssetChangeRequest` on iOS so the OS confirms deletion.

## MVP Scope

- Manual screenshot selection.
- Share-to-app ingestion.
- Local OCR.
- Rule-based extraction for:
  - Books
  - Places / Restaurants
  - Receipts / Coupons
- Action cards for saving details and opening the relevant app.
- Local saved lists.
- Local search.
- Cleanup prompt with native delete confirmation.

## Roadmap

- Validate import, OCR, and action-card flows with sample screenshots.
- Build the Flutter app shell and inbox UI.
- Implement local OCR and rule-based extraction.
- Add saved lists, search, and user corrections.
- Implement cleanup/delete confirmation flows.
- Add optional enrichment for books and places.
- Evaluate selected-photo access and screenshot folder monitoring.
- Explore background monitoring only where platform policies allow it.
- Prepare App Store and Play Store permission disclosures.

## Documentation

- [Product Requirements Document](PRD.md)
- [Technical Design Document](TDD.md)
- [Work Breakdown Structure](WBS.md)
- [Use-Case Journey](20260504121345_wordkcase.md)

## Development Principles

- Keep the default experience private and local-first.
- Prefer explicit user actions over broad background permissions.
- Use deterministic extraction before adding heavier AI features.
- Make cleanup reversible at the decision point by relying on native OS confirmation.
- Design for store review clarity from the beginning.
- Preserve user trust by explaining permissions and optional network enrichment plainly.
