# Typing Trainer Extension Template for MIT App Inventor

This document provides a template and guidelines for creating a Typing Trainer extension for MIT App Inventor, Kodular, or Niotron based on the web implementation in this repository.

## Extension Structure

### Extension Properties

```java
// Extension Properties (Designer Properties)
@SimpleProperty(category = PropertyCategory.BEHAVIOR)
public String TextToType() {
    return textToType;
}

@DesignerProperty(editorType = PropertyTypeConstants.PROPERTY_TYPE_STRING,
                  defaultValue = "The quick brown fox...")
@SimpleProperty
public void TextToType(String text) {
    this.textToType = text;
    this.currentIndex = 0;
    FireTextChanged();
}

@SimpleProperty(category = PropertyCategory.BEHAVIOR)
public int WPM() {
    return wpm;
}

@SimpleProperty(category = PropertyCategory.BEHAVIOR)
public int Accuracy() {
    return accuracy;
}

@SimpleProperty(category = PropertyCategory.BEHAVIOR)
public int Errors() {
    return errors;
}

@SimpleProperty(category = PropertyCategory.BEHAVIOR)
public long TimeElapsed() {
    return timeElapsed;
}
```

### Extension Methods

```java
// Main Methods
@SimpleFunction(description = "Start a new typing test")
public void StartNewTest() {
    LoadRandomText();
    ResetStats();
    FireTestStarted();
}

@SimpleFunction(description = "Reset current test")
public void ResetTest() {
    this.currentIndex = 0;
    this.errors = 0;
    this.startTime = 0;
    this.wpm = 0;
    this.accuracy = 100;
    FireTestReset();
}

@SimpleFunction(description = "Process typed character")
public void ProcessInput(String typedText) {
    if (startTime == 0) {
        startTime = System.currentTimeMillis();
    }
    
    CalculateStats(typedText);
    CheckCompletion(typedText);
    FireStatsUpdated();
}

@SimpleFunction(description = "Get character status at position")
public String GetCharacterStatus(int position) {
    // Returns: "correct", "incorrect", "current", "pending"
    if (position < currentIndex) {
        return typedCharacters.charAt(position) == textToType.charAt(position) 
               ? "correct" : "incorrect";
    } else if (position == currentIndex) {
        return "current";
    } else {
        return "pending";
    }
}
```

### Extension Events

```java
// Events
@SimpleEvent(description = "Fired when typing test starts")
public void TestStarted() {
    EventDispatcher.dispatchEvent(this, "TestStarted");
}

@SimpleEvent(description = "Fired when test is completed")
public void TestCompleted(int finalWPM, int finalAccuracy, long totalTime) {
    EventDispatcher.dispatchEvent(this, "TestCompleted", finalWPM, finalAccuracy, totalTime);
}

@SimpleEvent(description = "Fired when stats are updated")
public void StatsUpdated(int currentWPM, int currentAccuracy, int currentErrors) {
    EventDispatcher.dispatchEvent(this, "StatsUpdated", currentWPM, currentAccuracy, currentErrors);
}

@SimpleEvent(description = "Fired when text changes")
public void TextChanged(String newText) {
    EventDispatcher.dispatchEvent(this, "TextChanged", newText);
}
```

## App Inventor Block Structure

### Screen Layout Components

```
Screen1
├── VerticalArrangement1 (Main Container)
│   ├── HorizontalArrangement1 (Stats Panel)
│   │   ├── Label_WPM
│   │   ├── Label_Accuracy  
│   │   ├── Label_Time
│   │   └── Label_Errors
│   ├── Label_TextDisplay (Shows text with formatting)
│   ├── TextBox_Input (User typing input)
│   └── HorizontalArrangement2 (Control Buttons)
│       ├── Button_NewTest
│       ├── Button_Reset
│       └── Button_Pause
└── TypingTrainerExtension1 (Your Extension)
```

### Block Logic Example

#### When Screen Initializes
```blocks
when Screen1.Initialize
do set TypingTrainerExtension1.TextToType to "Sample typing text here..."
   call TypingTrainerExtension1.StartNewTest
   call UpdateDisplay
```

#### When User Types
```blocks
when TextBox_Input.TextChanged
do call TypingTrainerExtension1.ProcessInput(TextBox_Input.Text)
```

#### When Stats Update
```blocks
when TypingTrainerExtension1.StatsUpdated(currentWPM, currentAccuracy, currentErrors)
do set Label_WPM.Text to join(currentWPM, " WPM")
   set Label_Accuracy.Text to join(currentAccuracy, "%")
   set Label_Errors.Text to join(currentErrors, " Errors")
```

#### Text Display with Color Formatting
```blocks
to UpdateDisplay
do set displayText to ""
   for each position from 1 to length(TypingTrainerExtension1.TextToType)
   do set charStatus to call TypingTrainerExtension1.GetCharacterStatus(position - 1)
      set char to select list item(split TypingTrainerExtension1.TextToType at ""), position
      if charStatus = "correct"
      then set displayText to join(displayText, colorize char with GREEN)
      else if charStatus = "incorrect"  
      then set displayText to join(displayText, colorize char with RED)
      else if charStatus = "current"
      then set displayText to join(displayText, colorize char with YELLOW)
      else set displayText to join(displayText, char)
   set Label_TextDisplay.HTMLText to displayText
```

## Alternative: Pure App Inventor Implementation

If you prefer not to create an extension, you can implement the typing trainer using only built-in App Inventor components:

### Global Variables
- `TextToType` (text)
- `CurrentPosition` (number) 
- `StartTime` (number)
- `ErrorCount` (number)
- `TestTexts` (list)

### Procedures

#### StartNewTest
```blocks
to StartNewTest
do set global TextToType to select list item(global TestTexts, random 1 to length(global TestTexts))
   set global CurrentPosition to 0
   set global ErrorCount to 0
   set global StartTime to 0
   set TextBox_Input.Text to ""
   call UpdateDisplay
```

#### CalculateWPM
```blocks
to CalculateWPM returns number
do if global StartTime > 0
   then set timeInMinutes to (current time in milliseconds - global StartTime) / 60000
        set wordsTyped to length(TextBox_Input.Text) / 5
        return round(wordsTyped / timeInMinutes)
   else return 0
```

## CSS Styling for Web Components

If you want to integrate web views or HTML components:

```css
.typing-container {
    font-family: 'Courier New', monospace;
    padding: 20px;
}

.char-correct {
    background-color: #22c55e;
    color: white;
}

.char-incorrect {
    background-color: #ef4444;
    color: white;
}

.char-current {
    background-color: #ffcc00;
    color: black;
    animation: blink 1s infinite;
}

@keyframes blink {
    0%, 50% { opacity: 1; }
    51%, 100% { opacity: 0.3; }
}
```

## Testing Your Extension

1. **Unit Testing**: Test each method individually
2. **Integration Testing**: Test the complete typing flow
3. **Performance Testing**: Ensure smooth real-time updates
4. **User Experience Testing**: Test on different devices and screen sizes

## Distribution

1. **Compile Extension**: Use App Inventor Extension Template
2. **Create AIX File**: Package your extension
3. **Documentation**: Provide clear usage instructions
4. **Examples**: Include sample projects

## Advanced Features to Consider

- **Multiple Difficulty Levels**: Beginner, intermediate, advanced texts
- **Keyboard Layout Support**: QWERTY, Dvorak, Colemak
- **Progress Tracking**: Save user statistics locally
- **Lessons**: Structured typing lessons
- **Themes**: Multiple UI themes
- **Sound Effects**: Audio feedback for typing
- **Multiplayer**: Competitive typing races

This template provides a solid foundation for creating a professional typing trainer extension that matches the functionality of the web implementation in this repository.