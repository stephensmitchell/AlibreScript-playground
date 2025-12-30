# Welcome to AlibreScript Playground
# 
# Right-click for templates, Quick Reference and About information.
#
# Keyboard Shortcuts:
#   Ctrl+Space     - Trigger autocomplete
#   Ctrl+Shift+C   - Clear editor
#   F1             - Command palette

from AlibreScript import *

# Get the current part
MyPart = CurrentPart()

# Create a sketch
sketch = MyPart.AddSketch('MySketch', MyPart.XYPlane)

# Add geometry
sketch.AddCircle(0, 0, 50, False)

# Extrude
MyPart.AddExtrudeBoss('Extrude1', sketch, 25, False)

# Show dialog
Windows.InfoDialog('Done!', 'AlibreScript')

