# BridgePattern

**Assignment #3: Bridge Pattern**
Course: Software Design Patterns 
Astana IT University, School of Computer Engineering
Student: Mukhametsharip Aibyn, group SE-2504

## Description

A small drawing program in Java that demonstrates the Bridge structural design pattern. Geometric shapes can be drawn using interchangeable rendering techniques, and the renderer of a shape can be switched at runtime without changing the shape's code.

### Why Bridge fits this problem

There are two independent dimensions of variation: *what* is drawn (the shape's geometry) and *how* it is drawn (the rendering technique). Without Bridge, adding a new shape would require implementing it for every rendering technique, and adding a new technique would require touching every shape. Bridge decouples the two hierarchies through composition: `Shape` holds a reference to a `Renderer` interface, so either side can be extended independently.

| Role | Classes |
| --- | --- |
| Abstraction | `Shape` (holds a reference to `Renderer`) |
| Refined Abstraction | `Circle`, `Square`, `Triangle` |
| Implementor | `Renderer` |
| Concrete Implementor | `VectorRenderer`, `RasterRenderer`, `AsciiRenderer` |
| Client | `Main` (composes a shape with a renderer and switches renderers at runtime) |

## Project structure

```
BridgePattern/
├── README.md
├── docs/                      # UML diagram (PNG) and report
└── src/
    └── com/bridge/
        ├── Main.java
        ├── abstraction/
        │   ├── Shape.java
        │   ├── Circle.java
        │   ├── Square.java
        │   └── Triangle.java
        └── implementor/
            ├── Renderer.java
            ├── VectorRenderer.java
            ├── RasterRenderer.java
            └── AsciiRenderer.java
```

## How to run

Requirements: JDK 17 or newer.

**IntelliJ IDEA:** open the project, mark `src` as Sources Root, open `Main.java` and click Run.

**Command line:**

```bash
javac -d out $(find src -name "*.java")
java -cp out com.bridge.Main
```

## Expected output

```
##### Different shapes, same renderer #####
[Vector] Drawing a circle as an SVG path: center=(0.0, 0.0), radius=5.0
[Vector] Drawing a square as an SVG path: top-left=(1.0, 1.0), side=4.0
[Vector] Drawing a triangle as an SVG path: origin=(2.0, 2.0), base=6.0, height=3.0

##### Same shape, switching renderer at runtime #####
[Vector] Drawing a circle as an SVG path: center=(0.0, 0.0), radius=5.0
[Raster] Drawing a circle as a bitmap at (0.0, 0.0), approx. 79 pixels
[ASCII] Circle:
   ***
  *   *
   ***

##### Resize is independent of the renderer #####
[Vector] Drawing a circle as an SVG path: center=(0.0, 0.0), radius=10.0
```

## UML diagram

shows both hierarchies (`Shape` and `Renderer`) and the bridge composition link between them.

Created with PlantUML.

## Clean Code principles applied

1. Clear separation of abstraction-side vs. implementation-side responsibilities — `Shape` never knows about SVG, pixels, or ASCII; only `Renderer` does.
2. Meaningful names distinguishing the Abstraction side (`Shape`, `Circle`, `Square`) from the Implementor side (`Renderer`, `VectorRenderer`, `RasterRenderer`).
3. Small, focused classes — each Concrete Implementor implements exactly three short methods and holds no shape state.
4. No duplicated logic between Concrete Implementors — the renderer reference and null-check live once, in the `Shape` constructor.
5. Backward-compatible design — adding `AsciiRenderer` required zero changes to `Shape`, `Circle`, `Square`, or `Triangle`.

## Extending the project

- New shape: add a class extending `Shape` that calls the matching `Renderer` method. No changes needed in any `Renderer` implementation.
- New rendering technique: add a class implementing `Renderer`. No changes needed in `Shape` or its subclasses.

This independence between the two hierarchies is the core benefit of the Bridge pattern.
