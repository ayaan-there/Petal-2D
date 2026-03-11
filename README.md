# 💐 Petal

> Capture real flowers, craft digital bouquets, and share them with the people you love.

**Petal** is an iOS app that lets you photograph real flowers, automatically remove their backgrounds using on-device AI, and arrange them into beautiful digital bouquets — complete with wrappers, ribbons, and ferns. Built entirely with SwiftUI and the Vision framework.

`swift` `swiftui` `swift-student-challenge` `ios` `vision-framework` `apple`

---

## Demonstration

<video src="https://github.com/user-attachments/assets/2f4acbf5-6542-4047-850a-bb1f5483fe0c" controls width="300"></video>

---

## Features

- **AI Background Removal** — Uses Apple's Vision framework (`VNGenerateForegroundInstanceMaskRequest`) to instantly segment flowers and ferns from any photo, producing clean transparent cutouts — all on-device, no server needed.
- **Camera with Guide Overlay** — A live camera view with a circular guide overlay, pinch-to-zoom, and haptic feedback to help you frame the perfect shot.
- **Smart Validation** — The segmentation pipeline checks that your subject is centered, large enough, and not cluttered with multiple objects before saving.
- **Bouquet Builder** — A step-by-step builder where you pick flowers (up to 10), ferns (up to 8), a wrapper, and a ribbon. Built-in assets are included alongside your own custom captures.
- **Craft Room Canvas** — A fully interactive canvas where every element can be dragged, scaled, rotated, flipped, duplicated, and re-layered. A unique dial selector at the bottom lets you browse and place elements by category.
- **Template Layout Engine** — When you hit "Craft", the app auto-arranges all your selections into a natural-looking triangular bouquet layout, which you can then fine-tune by hand.
- **Save & Edit** — Bouquets are saved as full-resolution images and as editable compositions (JSON), so you can reopen and tweak them anytime.
- **Share** — Share your finished bouquets directly from the app via the iOS share sheet.
- **Attach Notes** — Write a personal note to go with each bouquet. The detail screen features a card-flip animation to reveal it.
- **Animated Onboarding** — A three-page onboarding experience with smooth transitions that introduces the app's concept.

---

## Screens

### Onboarding
The very first time you open Petal, you're greeted by a three-page onboarding flow. Each page fades in with a short animation and introduces one idea: **Capture Forever** (photograph your flowers), **Create Bouquets** (arrange them), and **Share Love** (give them to someone). A single "Get Started" button takes you into the app.

### Bouquet Builder (Craft Tab)
This is the home screen and your starting point. It's a scrollable page broken into four sections:

1. **Flowers** — A horizontal strip of built-in flower illustrations (Flower1–3) plus any flowers you've captured yourself. Tap a flower to add one to the bouquet; tap again to add more (up to 10 total). Long-press to delete a custom flower. A badge on each card shows how many you've selected.
2. **Ferns** — Same idea, but for ferns (up to 8). Includes four built-in fern assets and your own custom-captured ferns.
3. **Wrapper** — A single-select row of five decorative wrapper styles. Tap to select, tap again to deselect.
4. **Ribbon** — Five ribbon styles, same single-select behavior.

A summary bar at the top shows your current selections at a glance. Once you have at least one flower, a wrapper, and a ribbon, the **"Craft Bouquet"** button lights up and takes you to the Craft Room.

### Camera
Tapping the **+** button in the Flower or Fern section opens the camera. A dark overlay with a circular cutout helps you frame the subject. Pinch to zoom (displayed as a pill like "2.3×"). Press the large white capture button to take the photo. The app then pushes to the Cutout Preview.

### Cutout Preview
After capturing, the screen shows a loading spinner while the Vision framework removes the background. Once done, the clean cutout is displayed on a black background. You get two choices:

- **Retake** — Go back to the camera and try again.
- **Save** — An alert pops up with a default name (e.g. "Flower 4") that you can customize. The cutout and a thumbnail are saved to the app's Documents directory, and you're returned to the Builder with the new item ready to use.

If segmentation fails (subject not centered, too small, multiple objects detected), a friendly error message explains what went wrong and offers to retake.

### Craft Room
This is where the magic happens. The app's template layout engine automatically places your selected elements on a canvas in a natural triangular bouquet arrangement:

- **Wrapper** sits at the back, scaled up.
- **Ferns** fan out from the left and right edges.
- **Flowers** fill the upper triangle region.
- **Ribbon** ties it all together at the stem point.

From here, you can:

- **Drag** any element to reposition it.
- **Pinch** to scale elements up or down.
- **Rotate** using the circular handle below each selected element.
- **Flip, duplicate, or delete** elements via the action bar.
- **Reorder layers** (move forward, backward, to front, to back).
- **Switch categories** using the semicircular dial at the bottom to add more elements.

When you're happy, tap **Save**. Name your bouquet, optionally write a note, and it gets saved as a full-resolution PNG, a preview thumbnail, and an editable JSON composition.

### My Bouquets (Bouquets Tab)
A grid of all your saved bouquets, displayed as preview cards. Tap one to open the detail screen. Long-press (context menu) gives you options to **Edit**, **Share**, or **Delete** a bouquet.

### Bouquet Detail
A full-screen view of your bouquet image against a soft pink background. If you attached a note, tapping the bouquet triggers a card-flip animation that reveals a hand-lettered-style note card. You can edit the note inline and tap to flip back. Toolbar buttons let you share the image or re-open the bouquet in the Craft Room for editing.

---

## Tech Stack

| Layer | Technology |
|---|---|
| UI | SwiftUI, UIKit (Camera) |
| AI / Vision | `VNGenerateForegroundInstanceMaskRequest`, `VNImageRequestHandler` |
| Image Processing | Core Image, Core Graphics |
| Persistence | FileManager (Documents directory), JSON, PNG/JPEG |
| Architecture | MVVM + shared `AppState` via `@EnvironmentObject` |
| Platform | iOS 17+, Swift Playgrounds App Project |

---

## Project Structure

```
Petal.swiftpm/
├── PetalApp.swift              # App entry point, tab navigation
├── AppState.swift              # Central observable state
├── AppDelegate.swift
├── Models/
│   ├── Bouquet.swift           # Bouquet data model
│   ├── Flower.swift            # Flower data model
│   ├── CanvasElement.swift     # Canvas element (position, scale, rotation)
│   └── Enums.swift             # Tab, DialMode, ElementKind, PetalError
├── ViewModels/
│   ├── BouquetBuilderViewModel.swift
│   └── CraftRoomViewModel.swift
├── Views/
│   ├── OnboardingView.swift
│   ├── BouquetBuilderScreen.swift
│   ├── CameraScreen.swift
│   ├── CutoutPreviewScreen.swift
│   ├── CraftRoomScreen.swift
│   ├── BouquetListScreen.swift
│   ├── BouquetDetailScreen.swift
│   └── Components/
│       ├── CanvasView.swift
│       ├── CameraPreview.swift
│       ├── DialSelector.swift
│       └── DraggableElement.swift
├── Services/
│   ├── ImageProcessor.swift        # Vision-based background removal
│   ├── CameraService.swift         # AVCaptureSession management
│   ├── StorageService.swift        # File I/O for flowers, ferns, bouquets
│   └── TemplateLayoutService.swift # Auto-layout engine for bouquet arrangement
├── Helpers/
│   ├── Constants.swift
│   └── Extensions.swift
└── Assets.xcassets/            # Built-in flowers, ferns, wrappers, ribbons
```

---

## Requirements

- iOS 17.0+
- Xcode 15+ or Swift Playgrounds 4.4+
- A physical device is recommended (camera + Vision framework)

---

This project was built for the **Apple Swift Student Challenge**.
