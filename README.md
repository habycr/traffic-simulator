# Traffic Simulator

> An interactive traffic network simulator built with Python and Pygame, using graphs, custom data structures, dynamic edge weights, and Dijkstra's shortest-path algorithm.

This project was developed collaboratively for **CE 1103 — Algoritmos y Estructuras de Datos I** at the **Instituto Tecnológico de Costa Rica**.

The simulator models a road network as an undirected weighted graph. Cities are represented as nodes, roads as edges, and vehicles move along routes calculated from the current state of the network. Accidents, construction work, police operations, adverse weather, congestion, and blocked roads can change the cost of a route and affect the path selected by the algorithm.

## Table of Contents

- [Overview](#overview)
- [Main Features](#main-features)
- [How the Simulation Works](#how-the-simulation-works)
- [Graph and Routing Model](#graph-and-routing-model)
- [Dynamic Route Weights](#dynamic-route-weights)
- [Custom Data Structures](#custom-data-structures)
- [Software Design](#software-design)
- [Controls](#controls)
- [Installation](#installation)
- [Running the Project](#running-the-project)
- [Project Structure](#project-structure)
- [Algorithmic Complexity](#algorithmic-complexity)
- [Known Limitations](#known-limitations)
- [Development Team](#development-team)
- [Project Origin](#project-origin)

## Overview

Traffic Simulator provides a visual environment for building and analyzing a road network.

The user can:

- create cities;
- connect cities with roads;
- calculate shortest paths from a selected origin;
- generate vehicles with different routes;
- inspect individual vehicles;
- modify road conditions;
- pause or accelerate the simulation;
- view congested or important routes.

The project focuses on the practical application of graphs, linked structures, pathfinding algorithms, object-oriented design, and interactive visualization.

## Main Features

- Interactive road-network editor.
- Undirected weighted graph represented with adjacency lists.
- Shortest-path calculation with Dijkstra's algorithm.
- Dynamic road costs based on traffic and external events.
- Automatic and manual vehicle generation.
- Multiple vehicle categories.
- Road events:
  - accidents;
  - construction work;
  - police operations;
  - adverse weather;
  - blocked roads.
- Vehicle selection and route inspection.
- Detection of frequently used or congested routes.
- Simulation speed controls and pause support.
- Graphical interface developed with Pygame.
- Custom implementations of dictionaries and linked lists.
- Use of several object-oriented design patterns.

## How the Simulation Works

The application starts with an example network of cities and roads.

During execution:

1. The road network is stored as a weighted graph.
2. The simulator calculates the current cost of each road.
3. Dijkstra's algorithm finds the shortest available route.
4. Vehicles are assigned a route between an origin and destination.
5. Each vehicle moves through the sequence of nodes in its route.
6. Changes in congestion or road events modify edge weights.
7. New route calculations use the updated network state.

A road marked as blocked is excluded from valid neighbor traversal, preventing the routing algorithm from using it.

## Graph and Routing Model

### Nodes

Each city is represented by a node containing:

- a unique identifier;
- a display name;
- screen coordinates;
- metadata used during route calculation.

### Edges

Each road is represented by an edge connecting two nodes.

The graph is undirected, so every connection is stored in both directions. Each edge contains information such as:

- base weight;
- dynamic weight;
- current vehicle count;
- capacity;
- accident count;
- construction events;
- police operations;
- adverse weather;
- blocked or active state.

### Dijkstra's Algorithm

The simulator uses Dijkstra's algorithm to calculate the shortest route between cities.

For every route calculation, the algorithm:

1. initializes all node distances to infinity;
2. assigns distance `0` to the origin;
3. repeatedly selects the unvisited node with the smallest known distance;
4. relaxes the edges connected to that node;
5. stores predecessor references;
6. reconstructs the final route from destination to origin.

The implementation uses the project's custom dictionary structure to store the unvisited nodes and their current distances.

## Dynamic Route Weights

The base weight of a road is calculated from the Euclidean distance between its endpoints:

```text
base weight = max(1, integer distance / 10)
```

The dynamic weight adds congestion and event penalties:

```text
dynamic weight = base weight × congestion factor + event penalties
```

The congestion factor increases according to the ratio between the current number of vehicles and the road capacity.

Additional penalties are applied for:

| Event | Added cost |
|---|---:|
| Accident | `15` per event |
| Construction | `8` per event |
| Police operation | `5` per event |
| Adverse weather | `10` |
| Blocked road | Infinite cost |

Because weights are recalculated before route searches, the preferred route can change while the simulation is running.

## Custom Data Structures

The project implements its own structures instead of relying exclusively on Python's built-in containers.

### Custom dictionary

`DiccionarioPersonalizado` is implemented with a circular doubly linked list. It supports:

- insertion;
- search by key;
- update;
- deletion;
- iteration over key-value pairs;
- selection of the entry with the smallest value.

### Linked adjacency lists

Each graph node stores its outgoing edges in a custom linked list. This representation is appropriate for sparse graphs because it stores only existing connections.

These structures were developed as part of the academic objective of the course.

## Software Design

The application separates the simulation logic from the graphical interface.

The main responsibilities are divided into:

- **Models:** nodes, edges, vehicles, graph, linked structures, and custom dictionary.
- **Services:** route calculation, dynamic weight calculation, traffic analysis, and simulation coordination.
- **Controllers:** user input and interaction between the simulation and the interface.
- **Views:** rendering of the map, controls, status information, vehicles, and recommendations.

The code also applies several design patterns:

| Pattern | Purpose |
|---|---|
| MVC | Separates model, controller, and interface responsibilities |
| Strategy | Encapsulates the route-calculation algorithm |
| Facade | Provides a simplified interface to the simulation subsystem |
| Observer | Propagates changes in simulation state |
| Factory | Creates the different vehicle types |

## Controls

| Key or action | Function |
|---|---|
| `1` | Add cities |
| `2` | Connect cities |
| `3` | Select Dijkstra mode |
| `4` | Select road-event mode |
| `5` | Accident |
| `6` | Construction |
| `7` | Police operation |
| `8` | Adverse weather |
| `9` | Blocked road |
| `R` | Show or hide calculated routes |
| `S` | Generate random vehicles |
| `P` | Start automatic vehicle generation |
| `I` | Inspect vehicles |
| `T` | Show or hide route recommendations |
| `A` | Increase simulation speed |
| `D` | Decrease simulation speed |
| `Space` | Pause or resume |
| `C` | Reset the simulation |
| `Esc` | Exit |
| Left click | Select or modify an element according to the current mode |
| Right click | Add a vehicle toward a destination while using Dijkstra mode |

## Installation

### Requirements

- Python 3.
- Pygame.

Clone the repository:

```bash
git clone https://github.com/habycr/traffic-simulator.git
cd traffic-simulator
```

Create a virtual environment:

```bash
python -m venv .venv
```

Activate it on Windows PowerShell:

```powershell
.venv\Scripts\Activate.ps1
```

Activate it on Linux or macOS:

```bash
source .venv/bin/activate
```

Install Pygame:

```bash
python -m pip install pygame
```

## Running the Project

From the repository root, run:

```bash
python traffic_simulator/main.py
```

The entry point adds the repository root to Python's import path before loading the application, so the project can be started directly from the root directory.

## Project Structure

```text
traffic_simulator/
├── backend/
│   ├── factories/
│   ├── interfaces/
│   ├── models/
│   ├── services/
│   └── utils/
├── frontend/
│   ├── controllers/
│   ├── views/
│   └── app.py
└── main.py
```

Important files include:

| File | Responsibility |
|---|---|
| `traffic_simulator/main.py` | Application entry point |
| `frontend/app.py` | Pygame loop and input handling |
| `frontend/controllers/simulacion_controller.py` | Interaction and simulation control |
| `backend/services/simulacion_facade.py` | Main simulation interface |
| `backend/services/dijkstra_strategy.py` | Shortest-path algorithm |
| `backend/services/calculador_peso.py` | Base and dynamic edge weights |
| `backend/models/grafo_lista_adyacencia.py` | Graph representation |
| `backend/models/diccionario.py` | Custom dictionary |
| `backend/models/lista_enlazada.py` | Custom linked list |
| `backend/services/analizador_critico.py` | Frequently used and critical-route analysis |

## Algorithmic Complexity

Let:

- `V` be the number of cities;
- `E` be the number of roads.

The graph uses adjacency lists, so traversing all neighbors across the graph requires `O(V + E)` time.

The current Dijkstra implementation selects the minimum-distance node by scanning the custom dictionary instead of using a binary heap. Its expected complexity is therefore approximately:

```text
O(V² + E)
```

Route reconstruction requires at most `O(V)` additional time.

A future version could use a heap-based priority queue to reduce shortest-path calculation to approximately:

```text
O((V + E) log V)
```

## Known Limitations

- The simulator uses a custom linear-search dictionary rather than a hash table.
- Dijkstra's algorithm does not use a binary heap.
- The simulation is intended for educational use and is not a real traffic-prediction system.
- Vehicle behavior is simplified and does not model lanes, traffic lights, overtaking, or collisions.
- Dynamic weights are based on predefined formulas rather than live traffic data.
- The interface is designed for keyboard and mouse interaction on a desktop computer.
- Some application layers still access internal objects directly and could be further encapsulated.
- Automated tests and dependency packaging can be expanded.

## Development Team

Developed collaboratively by:

- Iván Ignacio Brito Medina
- Jeison Johel Picado Picado
- José Fabio Ruiz Morales
- Antony Javier Hernández Castillo

**Course:** CE 1103 — Algoritmos y Estructuras de Datos I  
**Institution:** Instituto Tecnológico de Costa Rica

## Project Origin

This repository is a portfolio fork of the original collaborative project:

- Original repository: `ibritom/TrafficSimulator`
- Portfolio fork: `habycr/traffic-simulator`

The original commit history and authorship have been preserved.
