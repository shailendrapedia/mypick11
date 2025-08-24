# MyPick11 - Cricket Fantasy & Typing Trainer

A comprehensive web application featuring cricket stadium statistics and an interactive typing trainer similar to keybr.com.

## Features

### Cricket Fantasy Section
- **Stadium Statistics**: Detailed T20 stats for Bharat Ratna Shri Atal Bihari Vajpayee Ekana Cricket Stadium
- **Match Insights**: Average scores, wicket analysis, and fantasy points
- **IPL 2025 Schedule**: Upcoming matches and tournament information
- **Fantasy Points Comparison**: Visual charts for batting, bowling, pace, and spin performance

### Typing Trainer Section ⌨️
Inspired by keybr.com, our typing trainer includes:

- **Real-time Typing Practice**: Interactive text with character-by-character feedback
- **Performance Metrics**: 
  - WPM (Words Per Minute) calculation
  - Accuracy percentage tracking
  - Error counting
  - Time tracking
- **Visual Feedback**: Color-coded characters (correct/incorrect/current)
- **Test Controls**: Start new test, reset, and pause/resume functionality
- **Multiple Practice Texts**: Variety of sentences for diverse typing practice

## For MIT App Inventor / Kodular / Niotron Developers

### Using This as Reference for Your Typing App

This web implementation provides a complete reference for building a typing trainer extension or app in MIT App Inventor, Kodular, or Niotron:

#### Key Components to Implement:

1. **Text Display Component**
   - Use Label components to show text to type
   - Implement character-by-character highlighting
   - Color coding: Green (correct), Red (incorrect), Yellow (current)

2. **Input Handling**
   - TextBox component for user input
   - Real-time text change events
   - Character comparison logic

3. **Statistics Tracking**
   - Timer component for time tracking
   - Mathematical calculations for WPM
   - Accuracy percentage calculations
   - Error counting logic

4. **User Interface Elements**
   - Buttons for: New Test, Reset, Pause/Resume
   - Labels for displaying: WPM, Accuracy, Time, Errors
   - Progress indicators

#### Implementation Steps:

1. **Create the Layout**
   ```
   - Vertical Arrangement (main container)
     - Horizontal Arrangement (stats display)
       - Label (WPM) | Label (Accuracy) | Label (Time) | Label (Errors)
     - Label (text to type with formatting)
     - TextBox (typing input)
     - Horizontal Arrangement (control buttons)
       - Button (New Test) | Button (Reset) | Button (Pause)
   ```

2. **Add Logic Blocks**
   - Timer for tracking typing time
   - Text comparison for accuracy
   - WPM calculation: (characters typed / 5) / (time in minutes)
   - Color formatting for feedback

3. **Extension Development**
   If creating a custom extension:
   - Define properties: texts array, current stats
   - Define methods: startTest(), resetTest(), calculateWPM()
   - Define events: onTextTyped, onTestComplete
   - Return values: WPM, accuracy, errors, time

### Sample Block Logic Concepts:

```
When TextBox.TextChanged:
  - Calculate current position
  - Compare typed character with target character
  - Update visual feedback
  - Calculate and update statistics
  - Check if test is complete

When Timer.Timer:
  - Update time display
  - Recalculate WPM
  - Update accuracy percentage

When NewTestButton.Click:
  - Reset all variables
  - Load new text
  - Clear input
  - Restart timer
```

## Installation & Usage

1. Clone this repository
2. Open `index.html` in a web browser
3. Navigate to the Typing Trainer section
4. Click "Start typing here..." and begin practicing
5. Use the control buttons to manage your typing session

## Technologies Used

- HTML5
- CSS3 (with animations and responsive design)
- Vanilla JavaScript (ES6+ features)
- No external dependencies

## Browser Compatibility

- Chrome (recommended)
- Firefox
- Safari
- Edge

## Contributing

Feel free to contribute by:
- Adding more practice texts
- Improving the UI/UX
- Creating MIT App Inventor extension templates
- Adding new typing exercises

## License

Open source - feel free to use this code as reference for your MIT App Inventor, Kodular, or Niotron projects!