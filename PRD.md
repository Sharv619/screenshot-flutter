# Product Requirements Document (PRD) - Screenshot Action Inbox

## Introduction

Screenshot Action Inbox is a planned Flutter mobile app that turns screenshots into searchable, structured, actionable items. The app extracts useful information locally, generates action cards, saves details into lists, opens the right destination app when useful, and helps users clean up the original screenshot with explicit OS-level confirmation.

This is not a cloud AI assistant and not primarily a folder sorter. Categorization is an engine capability that supports a broader workflow: extraction, action, local search, saved information, and cleanup.

## Problem Statement

People take screenshots to remember books, restaurants, coupons, receipts, product ideas, addresses, and recommendations. Most screenshots then sit in the photo library as visual clutter. Users often forget why they captured them, cannot search them reliably, and hesitate to delete them because the useful information has not been saved elsewhere.

## Product Vision

Build a privacy-first screenshot inbox where users can quickly convert screenshots into structured information and actions. A screenshot should become a reading-list item, place to visit, saved receipt, coupon reminder, or searchable note, then the app should help remove the original image when the user confirms it is no longer needed.

## Target Users

- People who frequently save screenshots of recommendations, places, receipts, coupons, and shopping ideas.
- Users who want private, local organization without sending screenshots to cloud services.
- Users who want quick next actions such as opening maps, saving a book, or keeping receipt details.
- Users with cluttered photo libraries who want a safer cleanup workflow.

## Core User Journeys

### Import a Screenshot

1. User selects a screenshot manually or shares one into the app.
2. The app imports metadata and runs local OCR.
3. The app extracts likely entities and generates action cards.
4. User saves useful details into a list or opens the relevant app.
5. The app offers an optional cleanup prompt.

### Save a Book

1. User imports a screenshot containing a book title, author, cover, or recommendation.
2. OCR and rules identify likely book information.
3. The app shows a book action card.
4. User saves it to a reading list.
5. Optional enrichment can add metadata if enabled.

### Save a Place or Restaurant

1. User imports a screenshot containing a restaurant name, address, map snippet, or travel recommendation.
2. OCR and rules identify location-like text.
3. The app shows a place action card.
4. User saves it to a places list or opens a maps app.
5. Optional enrichment can add place details if enabled.

### Save Receipt or Coupon Details

1. User imports a receipt, order confirmation, promo code, or coupon screenshot.
2. OCR identifies amounts, dates, merchant names, and codes.
3. The app shows a receipt or coupon action card.
4. User saves structured details for search and later reference.

## MVP Scope

- Manual screenshot selection.
- Share-to-app ingestion.
- On-device OCR.
- Text normalization.
- Rule-based classification and entity extraction.
- Action cards for Books, Places / Restaurants, and Receipts / Coupons.
- Local saved lists and collections.
- Local search across extracted text and saved details.
- User corrections for improving extracted fields.
- Cleanup prompt after save or action.
- Native OS-level deletion confirmation.

## Out of Scope for MVP

- Silent deletion of screenshots.
- Cloud AI dependency.
- Fully automatic background screenshot monitoring as the default flow.
- Broad photo library scanning by default.
- Social sharing features.
- Complex personal assistant behavior.
- Cross-device sync.
- Full expense management or accounting workflows.

## Core Features

- **Screenshot Inbox:** A queue of imported screenshots awaiting review or action.
- **Local OCR:** Platform OCR extracts visible text on-device.
- **Entity Extraction:** Rules identify books, places, receipts, coupons, dates, prices, URLs, addresses, and codes.
- **Category Scoring:** Lightweight scoring ranks likely screenshot types.
- **Action Cards:** Structured prompts turn extracted content into useful actions.
- **Saved Lists:** Users save books, places, receipts, and coupons into local collections.
- **Local Search:** Search works across OCR text, structured entities, tags, and lists.
- **User Corrections:** Users can fix extracted fields and improve future rules.
- **Cleanup Flow:** After saving useful information, the app can ask whether to delete the original screenshot.

## Intelligent Action Cards

Action cards should be direct and practical. They should not imply the app has perfect understanding.

Recommended MVP cards:

- **Book Card:** title, author if detected, save to reading list, optional metadata enrichment.
- **Place / Restaurant Card:** name, address or location clue, save to places list, open maps.
- **Receipt Card:** merchant, date, amount, save receipt details.
- **Coupon Card:** merchant, promo code, expiry date if detected, save coupon.

More categories can be added later, including shopping, events, movies, recipes, finance, travel, and contacts.

## Screenshot Cleanup Prompt

The cleanup prompt appears only after useful information is saved or the user completes an action. It should explain that the structured details have been saved locally and ask whether the user wants to remove the original screenshot.

Rules:

- No silent deletion.
- No automatic deletion without confirmation.
- Use native OS-level confirmation.
- Android deletion should use `MediaStore.createDeleteRequest`.
- iOS deletion should use `PHAssetChangeRequest`.
- If deletion fails or permission is denied, keep the saved item and show a clear recoverable state.

## Privacy and Trust Requirements

- Core processing is local-only.
- Screenshot content should not be uploaded in default mode.
- OCR, rules, action-card generation, saved lists, and search should work offline.
- Permissions should be requested only when needed.
- Manual import and share-to-app are preferred for MVP.
- Any optional network request must be disclosed before use.
- User data should be stored locally unless a future sync feature is explicitly designed and accepted.

## Optional Enrichment Mode

Optional enrichment mode can improve extracted results with external APIs, but must be user-enabled.

Possible enrichment:

- Google Books API for book metadata.
- Google Places API for restaurant or place details.

Requirements:

- The app must explain what extracted text or fields are sent.
- Enrichment cannot be required for core functionality.
- Users must be able to keep local-only mode.
- API failures should not block saving local extracted details.

## Success Metrics

- Percentage of imported screenshots that generate a useful action card.
- Percentage of action cards saved or opened by users.
- Percentage of reviewed screenshots cleaned up after OS confirmation.
- OCR and extraction correction rate.
- Time from screenshot import to saved structured item.
- Permission denial rate.
- Optional enrichment opt-in rate.
- App Store / Play Store review approval without permission-related rework.

## Risks and Constraints

- OCR accuracy varies by screenshot quality, language, and layout.
- iOS and Android differ significantly in media permissions and deletion workflows.
- Background monitoring may be limited by platform policy and battery rules.
- Broad photo library access can reduce trust and complicate store review.
- Optional API costs and quotas must be managed if enrichment is enabled.
- False positives in action cards could reduce confidence.

## Future Roadmap

- More categories such as events, movies, recipes, products, travel, contacts, and finance.
- Selected-photo access or screenshot folder monitoring for lower-friction import.
- Background monitoring only where platform policies allow it.
- Smarter user-correction feedback loops.
- Optional encrypted backup or sync.
- Reminder flows for coupons, events, and reservations.
- Export saved lists to notes, calendar, maps, or spreadsheets.
