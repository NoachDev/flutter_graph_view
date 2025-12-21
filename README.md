<!--
  Copyright (c) 2023- All flutter_graph_view authors. All rights reserved.

  This source code is licensed under Apache 2.0 License.
 -->

<h1 align="center"> Flutter Graph View </h1>

<p align="center">
  <a title="Pub" href="https://pub.dev/packages/flutter_graph_view" >
      <img src="https://img.shields.io/badge/Pub-v2.x-red?style=popout" />
  </a>
  <a href="https://github.com/dudu-ltd/flutter_graph_view/stargazers">
      <img src="https://img.shields.io/github/stars/dudu-ltd/flutter_graph_view" alt="GitHub stars" />
  </a>
  <a href="https://github.com/dudu-ltd/flutter_graph_view/network/members">
      <img src="https://img.shields.io/github/forks/dudu-ltd/flutter_graph_view" alt="GitHub forks" />
  </a>
</p>

Widgets for beautiful graphic data structures, such as force-oriented diagrams.

![image](https://github.com/graph-cn/flutter_graph_view/assets/15630211/0d368544-ad0b-441a-9bbc-a1d0ccdc0fd6)
![image](https://github.com/graph-cn/flutter_graph_view/assets/15630211/dae205f3-6706-44c6-acd6-83a86140a955)

## Features

- [x] Data converter: convert business data into graphic view data.
- [x] Multiple Layout Algorithms: calculate vertex layout.

  - [x] Force directed algorithm ( Simulates physical forces. Great for visualizing complex networks )
  - [x] Circle Layout ( Positions nodes in a perfect circle around a center point )
  - [x] Custom Algorithms ( A flexible way to free your creativity).
  - [x] Support algorithm decorator.
    - [x] Provide decorators that follow Hooke's Law for related nodes
    - [x] Provide a decorator for Hooke's Law from the center outward for all nodes
    - [x] Provide mutually exclusive Coulomb's law decorators for subgraph root nodes
    - [x] Hooke's Law decorator that provides border buffering collisions for all nodes
    - [x] Add a counter decorator in the figure to convert node forces into motion
    - [x] anothers decorators

- [x] Data panel embedding.
- [x] Style configuration:

  - [x] Custom vertex shapes (circle, diamond, rectangle, or fully custom shapes)
  - [x] Configurable colors per tag or by index with fallback mechanisms
  - [x] Customizable text rendering (size, style, color, font) per vertex or globally
  - [x] Edge decorators for special edge visual styles

- [x] Rich Interactivity

  - [x] Automatic hover detection for nodes and edges
  - [x] Smooth zoom (mouse wheel / pinch ) and pan (drag) with configurable limits
  - [x] Multi-node selection with visual feedback
  - [x] Floating data panels showing node/edge information
  - [x] Customizable event callbacks (tap down, tap up, tap cancel, drag, etc.)

- [ ] More graphical interactions.

---
# Getting started

```sh
flutter pub add flutter_graph_view
```

to start an application view the first [configuration](https://github.com/graph-cn/flutter_graph_view/blob/main/doc/en/quick_start.md)

Or explore the [exemples](https://github.com/graph-cn/flutter_graph_view/tree/main/example)

---
# Overview

**Flutter Graph View** is a powerful Dart/Flutter library designed to create interactive and animated visualizations of graph data structures. It provides all the tools needed to render, layout, and interact with complex networks of nodes and edges in your Flutter applications.

Whether you're building organizational charts, knowledge graphs, social network visualizations, dependency diagrams, or any other graph-based UI, Flutter Graph View delivers smooth animations, responsive interaction, and extensive customization options.

## What is a Graph?

A graph is a fundamental data structure composed of:

- **Vertices (Nodes)**: Individual entities or data points that represent objects in your domain
- **Edges (Connections)**: Lines or links between vertices that represent relationships or dependencies between those entities

```
    [Vertex A] ─── [Vertex B]
          │              │
          └──────────────┘
```

In this simple example, Vertex A and Vertex B are connected by edges, forming a basic graph structure.

## Three-Layer Architecture

Flutter Graph View is organized into three cohesive layers that work together seamlessly:

### 1. **Data Layer** (`model/`)

Fundamental data structures that represent graph information:

- **`Graph<ID>`** — The container that holds all vertices, edges, and metadata. Acts as the central repository for your entire graph structure, managing collections, caches, and relationships.

- **`Vertex<ID>`** — Represents a single node in the graph with properties such as position, visual style (color, size), connections to other nodes, and associated business data.

- **`Edge`** — Represents a directed or undirected connection between two vertices with configurable properties like weight, ranking, type, and custom styling.

### 2. **Logic Layer** (`core/`)

Processing, computation, and transformation of graph data:

- **`DataConvertor`** — Transforms your business data (from JSON, REST APIs, databases, CSV, etc.) into the Graph, Vertex, and Edge models that the library understands. Handles data validation and mapping.

- **`GraphAlgorithm`** — Interface and implementations for layout algorithms (force-directed, circle, random) and decorator-based extensibility for advanced behaviors like physics simulation, persistence, and interaction handling.

- **`Options`** — Centralized configuration hub for behaviors, callbacks, event handlers, zoom/pan settings, scale ranges, and visual options. Acts as the main configuration object.

- **`GraphStyle`** — Manages all visual styling aspects including colors per tag, stroke widths, edge appearances, text styles, and rendering preferences.

### 3. **Presentation Layer** (`widgets/`)

Flutter widgets and rendering components that display the graph on screen:

- **`FlutterGraphWidget`** — The main widget you add to your Flutter app; orchestrates all layers and handles the UI lifecycle, state management, and widget composition.

- **`GraphComponentCanvas`** — A custom canvas widget that manages rendering, animation, and user input gestures (zoom, pan, tap detection, drag interactions).

- **`GraphPainter`** — The low-level painter that draws vertices and edges on the canvas using Dart's drawing APIs, handling per-frame rendering and visual updates.

```
┌─────────────────────────────────────────────────────────────────────┐
│        FlutterGraphWidget (Main Widget Entry Point)                 │
├─────────────────────────────────────────────────────────────────────┤
│  Presentation Layer                                                 │
│  - GraphComponentCanvas (canvas & input management)                 │
│  - GraphPainter (renders vertices & edges each frame)               │
├─────────────────────────────────────────────────────────────────────┤
│  Logic Layer                                                        │
│  - GraphAlgorithm (computes positions via layout logic)             │
│  - DataConvertor (transforms business data → Graph models)          │
│  - Options (centralized configuration)                              │
├─────────────────────────────────────────────────────────────────────┤
│  Data Layer                                                         │
│  - Graph, Vertex, Edge (core immutable/mutable models)              │
├─────────────────────────────────────────────────────────────────────┤
│  Your Business Data (JSON, API, Database, CSV, Objects, etc.)       │
└─────────────────────────────────────────────────────────────────────┘
```

## Processing Flow

Here's how data flows through the library from your input to the rendered graph:

1. **Input Data** — You provide your raw data (JSON, Map, objects, etc.) in any format your application uses.
2. **Conversion** — A `DataConvertor` validates and transforms your data into `Graph`, `Vertex`, and `Edge` models the library understands.
3. **Layout Computation** — A `GraphAlgorithm` iteratively calculates optimal positions for each node based on the graph topology.
4. **Rendering** — The `GraphPainter` draws vertices and edges on the canvas at their computed positions, with colors, styles, and text from the configuration.
5. **User Interaction** — Mouse/touch events (hover, tap, drag, zoom, pan) trigger callbacks and update the graph state in real time.
6. **Animation** — Position changes and style updates animate smoothly across frames using Flutter's animation system.

```
Your Data → Converter → Graph Model → Algorithm Layout → Canvas Draw → Screen
                                               ↓
                       User Events: Hover, Tap, Drag, Zoom, Pan
                                               ↓
                       Callback Handlers → Update State → Re-render
```


---

## Licence

flutter_graph_view is under the [Apache License, Version 2.0](https://www.apache.org/licenses/LICENSE-2.0).
