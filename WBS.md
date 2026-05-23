# Work Breakdown Structure (WBS) - Screenshot Action Inbox

## Phase 1: Validation and Research

- Define MVP journeys for books, places/restaurants, receipts, and coupons.
- Collect representative screenshot samples for each MVP category.
- Validate manual import and share-to-app flows on Android and iOS.
- Research Android Photo Picker, Selected Photos Access, MediaStore, and deletion confirmation.
- Research iOS PhotoKit selected access, share extension constraints, and deletion confirmation.
- Draft App Store and Play Store permission explanations.
- Define local-only mode and optional enrichment mode boundaries.

## Phase 2: Core Flutter App Shell

- Initialize the Flutter project after planning approval.
- Set up feature-first project structure.
- Add Riverpod state management.
- Build inbox, review, saved lists, search, and settings navigation.
- Create reusable action card UI components.
- Build permission education screens and empty states.
- Add local theme and accessibility baseline.

## Phase 3: OCR and Local Processing

- Implement manual screenshot import.
- Implement share-to-app ingestion.
- Create local screenshot metadata records.
- Integrate Android OCR with Google ML Kit.
- Integrate iOS OCR with Apple Vision or equivalent platform OCR.
- Build text normalization utilities.
- Implement rule-based entity extraction for books, places/restaurants, receipts, and coupons.
- Implement category scoring and confidence output.
- Add processing queue and retry handling.

## Phase 4: Action Cards and Saved Lists

- Generate action cards from extracted entities and category scores.
- Build book action card and reading list save flow.
- Build place/restaurant action card and maps open flow.
- Build receipt action card and receipt details save flow.
- Build coupon action card and coupon save flow.
- Implement saved lists or collections.
- Implement local search across OCR text and structured fields.
- Add user correction flow for extracted fields.

## Phase 5: Cleanup / Delete Flow

- Add cleanup prompt after a user saves details or completes an action.
- Implement Android deletion via `MediaStore.createDeleteRequest`.
- Implement iOS deletion via `PHAssetChangeRequest`.
- Handle declined, failed, and completed deletion states.
- Make deletion copy explicit that the OS will confirm removal.
- Test no silent deletion paths exist.
- Document cleanup permission and store review messaging.

## Phase 6: Optional Enrichment

- Add settings toggle for optional enrichment mode.
- Implement disclosure copy explaining what extracted fields may be sent.
- Integrate Google Books API for opt-in book metadata.
- Integrate Google Places API for opt-in place details.
- Cache enrichment results locally.
- Add API failure and quota handling.
- Verify local-only mode makes no enrichment network calls.

## Phase 7: Optional Background Detection

- Evaluate selected-photo access and screenshot folder monitoring after MVP flows are stable.
- Prototype Android MediaStore screenshot folder monitoring if permission and policy review supports it.
- Prototype iOS PhotoKit change observation where selected access and policy allow it.
- Measure battery impact and processing frequency.
- Add user-facing opt-in controls for any monitoring feature.
- Keep manual import and share-to-app as fallback paths.

## Phase 8: Testing, Store Readiness, and Launch

- Write unit tests for normalization, extraction, category scoring, and action generation.
- Write persistence and search tests.
- Run device tests for import, share, OCR, action cards, and delete confirmation.
- Run privacy tests for local-only mode.
- Test permission denial and limited-access states.
- Profile OCR speed, memory, and battery use.
- Prepare App Store and Play Store privacy labels and permission explanations.
- Create demo dataset and review script.
- Prepare launch checklist and known limitations.
