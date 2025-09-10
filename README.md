# PyQt5-rotating-tabs-widget

A custom multi-row, rotating tab widget for PyQt5 applications.

This widget provides a flexible and highly customizable tab interface that allows for multiple rows of tabs. When a tab in a back row is clicked, that row rotates to the front, providing an intuitive way to navigate a large number of tabs.

## Features

*   **Multi-row Tabs**: Automatically arranges tabs into multiple rows when they don't fit in a single row.
*   **Rotating UI**: Click a tab in a background row to bring it to the front.
*   **Justified Layout**: Tabs automatically stretch to fill the available width of the widget.
*   **Fixed Tab Layout**: Optionally, set a fixed number of tabs per row for a grid-like layout.
*   **Customizable Styles**: Comes with built-in styles (Default, Rounded, Dark Rounded) and can be easily extended with your own themes via a simple style file.
*   **Portable Logging**: Integrates with your application's existing logging system.
*   **Tab Management**: Access tab count and programmatically control tab selection.

## Installation

To use this widget in your project, simply copy the following two files into your project's source directory, making sure they are in the same location:

*   `rotating_tab_widget.py`
*   `tab_styles.py`

The widget requires `PyQt5` to be installed in your environment.

## How to Use

Here is a basic example of how to create and use the `RotatingTabWidget`.

```python
import sys
import logging
from PyQt5.QtWidgets import QApplication, QMainWindow, QLabel, QWidget, QVBoxLayout
from rotating_tab_widget import RotatingTabWidget
from tab_styles import STYLE_ROUNDED, STYLE_DARK_ROUNDED # Import styles

# 1. (Optional) Set up your application's logger
app_logger = logging.getLogger("my_app")
app_logger.setLevel(logging.DEBUG)
# ... configure your logger handlers ...

class MyMainWindow(QMainWindow):
    def __init__(self):
        super().__init__()
        self.setWindowTitle("RotatingTabWidget Example")
        self.setGeometry(100, 100, 600, 400)

        # 2. Create an instance of the widget, passing your logger
        #    If no logger is passed, it will work silently.
        self.tab_widget = RotatingTabWidget(self, logger=app_logger)

        # 3. Configure the widget's appearance
        self.tab_widget.setTabsPerRow(4) # Create a 4x4 grid
        self.tab_widget.setTabStyle(STYLE_ROUNDED) # Use the rounded style

        self.setCentralWidget(self.tab_widget)

        # 4. Add tabs to the widget
        for i in range(1, 17):
            # Each tab needs a content widget
            content = QWidget()
            layout = QVBoxLayout(content)
            layout.addWidget(QLabel(f"Content for Tab {i}"))
            
            self.tab_widget.addTab(content, f"Tab {i}")
        
        # 5. Access widget information
        print(f"Total tabs: {self.tab_widget.count()}")
        print(f"Current tab index: {self.tab_widget.currentIndex()}")

if __name__ == "__main__":
    app = QApplication(sys.argv)
    window = MyMainWindow()
    window.show()
    sys.exit(app.exec_())
```

## API Methods

The `RotatingTabWidget` provides the following methods:

*   **`addTab(widget, text, icon=None)`**: Add a new tab with the specified widget content, text, and optional icon.
*   **`setTabsPerRow(count)`**: Set a fixed number of tabs per row for a grid-like layout.
*   **`setTabStyle(style)`**: Apply a style (e.g., `STYLE_DEFAULT`, `STYLE_ROUNDED`, `STYLE_DARK_ROUNDED`).
*   **`currentIndex()`**: Get the index of the currently selected tab.
*   **`setCurrentIndex(index)`**: Programmatically select a tab by index.
*   **`count()`**: Get the total number of tabs in the widget.
*   **`widget(index)`**: Get the widget content of a tab by index.

## Customization

You can easily create your own themes by adding new style dictionaries to the `tab_styles.py` file. A style is a dictionary that defines colors, padding, border radius, and the drawing function to use.

Here is the structure of a style dictionary:

```python
MY_CUSTOM_STYLE = {
    "drawer": "_paint_rounded_tabs", # or "_paint_default_tabs"
    "padding": 15,
    "border_radius": 5,
    "y_offset_factor": 8,
    "colors": {
        "bg_selected": QColor("#ff0000"),
        "text_selected": QColor(Qt.white),
        "bg_back": QColor("#cccccc"),
        # ... and other colors
    }
}
```

You can then import and use this style in your application:

```python
from tab_styles import MY_CUSTOM_STYLE, STYLE_DARK_ROUNDED
# ...
my_tab_widget.setTabStyle(MY_CUSTOM_STYLE)
# Or use the built-in dark theme
my_tab_widget.setTabStyle(STYLE_DARK_ROUNDED)
```

## Available Built-in Styles

The widget comes with three built-in styles:

*   **`STYLE_DEFAULT`**: Basic tab appearance using system styling
*   **`STYLE_ROUNDED`**: Rounded tabs with custom colors and padding  
*   **`STYLE_DARK_ROUNDED`**: Dark theme with rounded tabs, perfect for dark mode applications

## License

This project is licensed under the **GPL v3 License** for non-commercial use, as required by the PyQt5 dependency. 

**Important Licensing Notes:**
- PyQt5 is dual-licensed under GPL v3 and a commercial license from Riverbank Computing
- For **non-commercial use**: This project and PyQt5 can be used under GPL v3
- For **commercial use**: You must obtain a commercial license for PyQt5 from Riverbank Computing

If you plan to use this widget in a commercial application, please ensure you have the appropriate PyQt5 commercial license or consider using PySide2/PySide6 (Qt for Python) which is available under LGPL license.
