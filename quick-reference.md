# AlibreScript Quick Reference

A comprehensive reference guide for the AlibreScript API.

---

## Keyboard Shortcuts

| Shortcut | Action |
|----------|--------|
| `Ctrl+Space` | Trigger autocomplete |
| `Ctrl+Shift+C` | Clear editor |
| `F1` | Command palette |
| Right-click | Context menu with templates |

---

## Getting Started

```python
from AlibreScript import *

# Get the currently open part
MyPart = CurrentPart()

# Get the currently open assembly
MyAssembly = CurrentAssembly()
```

---

## Creating Parts

```python
# Create new part
NewPart = Part('My New Part')

# Open existing part
ExistingPart = Part('C:\\Parts', 'MyPart')
```

---

## Reference Planes

Every part has built-in reference geometry:

```python
MyPart.XYPlane    # Front plane
MyPart.YZPlane    # Right plane
MyPart.ZXPlane    # Top plane

MyPart.XAxis      # X reference axis
MyPart.YAxis      # Y reference axis
MyPart.ZAxis      # Z reference axis
```

---

## Sketches - 2D Geometry

### Creating a Sketch

```python
# Create a sketch on a plane
sketch = MyPart.AddSketch('MySketch', MyPart.XYPlane)
```

### Adding Geometry

```python
# Circle: centerX, centerY, diameter, isReference
sketch.AddCircle(0, 0, 50, False)

# Rectangle: x1, y1, x2, y2, isReference
sketch.AddRectangle(-25, -25, 25, 25, False)

# Line: x1, y1, x2, y2, isReference
sketch.AddLine(0, 0, 50, 50, False)

# Arc: centerX, centerY, startX, startY, endX, endY
sketch.AddArcCenterStartEnd(0, 0, 25, 0, 0, 25)

# Polygon: centerX, centerY, diameter, sides, isReference
sketch.AddPolygon(0, 0, 40, 6, False)  # Hexagon
```

---

## Features - 3D Operations

### Extrude

```python
# Extrude Boss (adds material)
feature = MyPart.AddExtrudeBoss('Extrude1', sketch, 25, False)

# Extrude Cut (removes material)
feature = MyPart.AddExtrudeCut('Cut1', sketch, 10, False)
```

### Revolve

```python
# Revolve around an axis
feature = MyPart.AddRevolveBoss('Revolve1', sketch, MyPart.YAxis, 360)
```

### Fillet & Chamfer

```python
edge = MyPart.GetEdge('Edge<1>')

# Fillet: edge, radius, tangentPropagate
MyPart.AddFillet('Fillet1', edge, 5, True)

# Chamfer: edge, distance, tangentPropagate
MyPart.AddChamfer('Chamfer1', edge, 3, True)
```

---

## Reference Geometry

```python
# Offset plane from existing plane
plane = MyPart.AddPlane('OffsetPlane', MyPart.XYPlane, 50)

# Point at coordinates
point = MyPart.AddPoint('Point1', 10, 20, 30)

# Axis from two points
axis = MyPart.AddAxis('Axis1', [0,0,0], [0,0,100])
```

---

## Parameters

```python
# Add parameters
width = MyPart.AddParameter('Width', ParameterTypes.Distance, 100)
height = MyPart.AddParameter('Height', ParameterTypes.Distance, 50)

# Get existing parameter
param = MyPart.GetParameter('Width')
print(param.Value)
```

---

## Topology Access

### Get All Elements

```python
faces = MyPart.GetFaces()
edges = MyPart.GetEdges()
vertices = MyPart.GetVertices()
```

### Get by Name

```python
face = MyPart.GetFace('Face<1>')
edge = MyPart.GetEdge('Edge<1>')
```

### Iterate

```python
for face in MyPart.GetFaces():
    print(face.Name)
```

---

## User Dialogs

### Simple Dialogs

```python
# Information dialog
Windows.InfoDialog('Operation complete!', 'Info')

# Error dialog
Windows.ErrorDialog('Something went wrong', 'Error')

# Question dialog (returns True/False)
if Windows.QuestionDialog('Continue?', 'Confirm'):
    print('User clicked Yes')
```

### Input Dialog

```python
inputs = [
    ['Width', WindowsInputTypes.Float, 100],
    ['Height', WindowsInputTypes.Float, 50],
    ['Name', WindowsInputTypes.String, 'Part1']
]
values = Windows.OptionsDialog('Enter Values', inputs)
width, height, name = values
```

### File Dialogs

```python
# Open file dialog
file = Windows.OpenFileDialog('Select Part', 'Part Files|*.AD_PRT')

# Select folder dialog
folder = Windows.SelectFolderDialog('', 'Select Output Folder')
```

---

## Save & Export

### Saving

```python
MyPart.Save()
MyPart.Save('C:\\Output')
MyPart.SaveAs('C:\\Output', 'NewName')
```

### Exporting

```python
MyPart.ExportSTL('C:\\Output\\part.stl')
MyPart.ExportSTEP214('C:\\Output\\part.step')
MyPart.ExportIGES('C:\\Output\\part.igs')
```

---

## Assemblies

### Creating

```python
assy = Assembly('MyAssembly')
assy = CurrentAssembly()
```

### Adding Parts

```python
part1 = assy.AddPart('C:\\Parts\\Part1.AD_PRT', 0, 0, 0)
part2 = assy.AddPart('C:\\Parts\\Part2.AD_PRT', 100, 0, 0)
```

### Constraints

```python
assy.AddMateConstraint(0, part1, part1.XYPlane, part2, part2.XYPlane)
assy.AddAlignConstraint(0, part1, part1.YZPlane, part2, part2.YZPlane)
```


