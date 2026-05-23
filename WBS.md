# Work Breakdown Structure (WBS) - 2-Week Sprint

## Phase 1: Foundation (Week 1)

### Day 1-2: Project Initialization & UI Core
- [ ] Initialize Flutter project (`flutter create`).
- [ ] Set up project architecture (Clean Architecture or Layered).
- [ ] Design and implement basic Theme and UI Skeleton (Home, Gallery, Settings).
- [ ] Implement Permission Handling (Storage, Photos, Notifications).

### Day 3-4: Background Services & Detection
- [ ] Implement File System Watcher for screenshot directories.
- [ ] Create Method Channels for platform-specific screenshot detection (Android `ContentObserver`, iOS `UIApplicationUserDidTakeScreenshotNotification`).
- [ ] Set up background service/work manager.

### Day 5: Data Persistence
- [ ] Set up local database (Isar or SQLite).
- [ ] Define schemas for Screenshots, Categories, and Tags.
- [ ] Implement CRUD operations for screenshot metadata.

---

## Phase 2: Intelligence & Refinement (Week 2)

### Day 6-7: ML Kit Integration
- [ ] Integrate Google ML Kit (Android) / Apple Vision (iOS) via plugins.
- [ ] Implement OCR pipeline for text extraction.
- [ ] Implement Image Labeling for visual context.

### Day 8-9: Categorization Engine
- [ ] Develop logic to map OCR/Labels to Categories.
- [ ] Implement a lightweight classification model (e.g., KNN on text embeddings or Keyword Matching).
- [ ] Enable auto-folder creation based on category results.

### Day 10-12: Advanced UI & Search
- [ ] Build Category-based Gallery views.
- [ ] Implement full-text search across all screenshots.
- [ ] Add manual categorization and tagging UI.

### Day 13-14: Testing & Deployment Prep
- [ ] Unit and Integration testing for the categorization logic.
- [ ] Performance profiling (Battery & Memory).
- [ ] Final UI Polish and documentation.
- [ ] Build generation (APK/IPA).
