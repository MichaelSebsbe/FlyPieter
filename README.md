# Fly Control App Documentation

## Overview
[Demo Video X/Tweet](https://x.com/michaelsebsbe/status/1898343622208115020?s=46)

This iOS application integrates a WKWebView to load a web-based flight simulator from "https://fly.pieter.com" and uses device motion data (CoreMotion) to control the simulation. It includes haptic feedback for missile firing events and a debug interface for calibration.

## Features
- **Web Integration**: Loads a flight simulator in a WKWebView.
- **Motion Control**: Uses CoreMotion to translate device roll and pitch into joystick controls.
- **Haptic Feedback**: Provides tactile feedback when missiles are fired using UIImpactFeedbackGenerator.
- **Debug Interface**: Displays motion data and calibration controls (visible when `debug = true`).

## Requirements
- iOS device with motion sensors
- Xcode with Swift support
- Internet connection to load "https://fly.pieter.com"

## Key Components

### Class: `ViewController`
Inherits from `UIViewController` and conforms to `WKScriptMessageHandler`.

#### Properties
- `webView`: WKWebView for displaying the flight simulator
- `motionManager`: CMMotionManager for device motion tracking
- `hapticFeedback`: UIImpactFeedbackGenerator for missile firing feedback (style: `.heavy`)
- Calibration variables:
  - `baseAmplification`: Roll amplification factor (default: 4.9)
  - `decayFactor`: Roll decay factor (default: 2.7)
  - `pitchBaseAmplification`: Pitch amplification factor (default: 4.9)
  - `pitchDecayFactor`: Pitch decay factor (default: 2.7)
- `debug`: Boolean to toggle debug UI (default: `true`)

#### UI Elements
- `overlayLabel`: Displays debug information
- `resetButton`: Resets motion baseline
- Calibration buttons: Adjust amplification and decay factors for roll and pitch

### Methods

#### Setup
- `viewDidLoad()`: Initializes UI, web view, and motion tracking
- `setupWebView()`: Configures WKWebView with JavaScript injection
- `loadWebContent()`: Loads the simulator URL
- `setupOverlayLabel()`: Sets up debug overlay
- `setupResetButton()`: Configures reset button
- `setupCalibrationButtons()`: Creates buttons for adjusting motion parameters

#### Motion Handling
- `calibrateMotion()`: Establishes initial roll baseline
- `setupMotionTracking()`: Continuously updates joystick data based on device motion
- `resetRollBaseline()`: Recalibrates motion baseline

#### Haptic Feedback
- `userContentController(_:didReceive:)`: Handles "missileFired" messages from JavaScript
  - Triggers haptic feedback with `hapticFeedback.impactOccurred()`
  - Prepares next feedback with `hapticFeedback.prepare()`

#### Calibration Actions
- Increase/decrease methods for `baseAmplification`, `decayFactor`, `pitchBaseAmplification`, and `pitchDecayFactor`

### JavaScript Integration
- Injects a script to override `window.shootMissile` and send "missileFired" messages
- Updates `leftJoystickData` with clamped roll and pitch values

## Usage
1. Launch the app on an iOS device.
2. Tilt the device to control the simulator (roll for left/right, pitch for up/down).
3. Fire missiles in the simulator to feel haptic feedback.
4. Use debug controls (if enabled) to:
   - Reset motion baseline
   - Adjust amplification and decay factors

## Notes
- Haptic feedback requires a compatible device.
- Motion tracking requires device motion availability.
- Debug UI is hidden when `debug = false`.
- Cleanup occurs in `deinit` to stop motion updates and remove message handlers.

## Potential Improvements
- Add cooldown for haptic feedback to prevent channel overload
- Implement error handling for web loading failures
- Add configuration options for haptic intensity
- Persist calibration settings

## Disclaimer
This application has only been tested on an iPhone 15 Pro. Compatibility with other devices is not guaranteed and may require adjustments to motion calibration or UI constraints.
