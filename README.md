# SmartFlow - AI-Powered Traffic Signal Controller

> A browser-based intelligent traffic intersection simulator that uses a **Priority Queue / Max-Heap scheduling strategy** to give the green signal to the lane that needs it most.

![SmartFlow Banner](https://img.shields.io/badge/SmartFlow-AI%20Traffic%20Controller-00e5ff?style=for-the-badge)
![Made With HTML](https://img.shields.io/badge/HTML5-Structure-e34f26?style=for-the-badge&logo=html5&logoColor=white)
![Made With CSS](https://img.shields.io/badge/CSS3-Glassmorphism-1572b6?style=for-the-badge&logo=css3&logoColor=white)
![Made With JavaScript](https://img.shields.io/badge/JavaScript-Algorithm%20Engine-f7df1e?style=for-the-badge&logo=javascript&logoColor=black)
![No Backend](https://img.shields.io/badge/Backend-Not%20Required-00e676?style=for-the-badge)

---

## Overview

**SmartFlow** is a premium browser-based traffic intersection simulator built with **HTML, CSS, and Vanilla JavaScript**.

Unlike traditional fixed-timer traffic lights, SmartFlow makes signal decisions dynamically. Every lane is scored in real time based on congestion, vehicle type, queue length, and emergency presence. The lane with the highest priority gets the green signal.

The entire project runs offline in the browser. No server. No database. No installation.

---

## Why SmartFlow?

Modern traffic systems need more than static timers. A lane with five vehicles should not wait behind an empty road just because a timer says so. SmartFlow demonstrates how a smarter signal controller can reduce waiting time by using weighted lane priority.

It is designed as an educational and visual project for:

- Data Structures and Algorithms demonstrations
- AI-inspired traffic management concepts
- Browser-based simulation projects
- Smart city and intelligent transportation showcases
- Portfolio and GitHub project presentation

---

## Core Idea

Each lane receives a priority score:

```txt
Score = [hasEmergency, totalWeight, queueLength]
```

The score is compared lexicographically:

| Priority Level | Rule | Meaning |
| --- | --- | --- |
| 1 | Emergency vehicle present | Ambulance, Fire Truck, or Police gets instant priority |
| 2 | Highest total vehicle weight | Heavy traffic wins over lighter traffic |
| 3 | Longest queue | If scores tie, the longer queue wins |

---

## Vehicle Priority System

| Vehicle | Icon | Weight | Priority Type |
| --- | --- | ---: | --- |
| Car | 🚗 | 1 | Normal |
| Motorcycle | 🏍️ | 1 | Normal |
| Truck | 🚛 | 3 | Heavy |
| Bus | 🚌 | 5 | Heavy |
| Ambulance | 🚑 | 100 | Emergency |
| Fire Truck | 🚒 | 100 | Emergency |
| Police | 🚓 | 100 | Emergency |

Emergency vehicles override all regular scoring and immediately push their lane to the top.

---

## Features

### Intelligent Signal Engine

- Priority Queue / Max-Heap style lane ranking
- Emergency vehicle override
- Weighted scoring for heavy vehicles
- Queue-length tie breaking
- Automatic lane clearing after every green signal

### Interactive Traffic Simulator

- Four-way intersection: North, South, East, and West
- Vehicle queue visualization using icons
- Live signal states: red, green, and emergency
- Dynamic lane statistics
- Real-time system activity log

### Control Panel

- Select lane direction
- Choose vehicle type
- Add vehicles in batches
- Fill all lanes with random traffic
- Run one signal cycle manually
- Auto-simulate multiple cycles
- Reset the whole system

### Live Dashboard

- Total cycles completed
- Vehicles cleared
- Emergency overrides
- Total queued vehicles
- Current active green lane

---

## Project Structure

```txt
Traffic-Optimizer-main/
|
|-- index.html    # Main UI layout
|-- style.css     # Dark glassmorphism theme and responsive styling
|-- app.js        # Priority scheduling algorithm and UI logic
|-- README.md     # Project documentation
```

---

## Algorithm

SmartFlow ranks active lanes using a max-priority strategy. The highest ranked lane gets the green signal for the current cycle.

```javascript
function laneScore(dir) {
  const q = lanes[dir];
  const hasEmergency = q.some(v => EMERGENCY_TYPES.has(v)) ? 1 : 0;
  const totalWeight = q.reduce((sum, v) => sum + VEHICLE_WEIGHTS[v], 0);

  return [hasEmergency, totalWeight, q.length];
}

function compareScores(a, b) {
  for (let i = 0; i < a.length; i++) {
    if (b[i] !== a[i]) return b[i] - a[i];
  }
  return 0;
}

function getRankedLanes() {
  return DIRECTIONS
    .filter(dir => lanes[dir].length > 0)
    .sort((a, b) => compareScores(laneScore(a), laneScore(b)));
}
```

### Cycle Clearing

After a lane receives the green signal, SmartFlow clears:

```txt
50% of the winning lane queue
minimum cleared vehicles = 1
```

This simulates realistic traffic movement through an intersection.

---

## Complexity Analysis

| Operation | Complexity |
| --- | --- |
| Lane scoring | O(k) per lane |
| Lane ranking | O(n log n) |
| Queue storage | O(v) |

Where:

- `n` = number of active lanes, maximum 4
- `k` = vehicles in a lane
- `v` = total vehicles in all lanes

Because the simulation has only four directions, runtime is extremely fast in practice.

---

## How to Run

No setup is required.

```txt
1. Download or clone the repository
2. Open index.html in any modern browser
3. Start simulating traffic
```

You can run it with:

```bash
git clone https://github.com/your-username/Traffic-Optimizer-main.git
cd Traffic-Optimizer-main
```

Then open:

```txt
index.html
```

---

## How to Use

### Quick Demo

1. Open `index.html`
2. Click **Fill All 4 Lanes**
3. Click **Run 1 Signal Cycle**
4. Watch the highest-priority lane turn green
5. Use **Auto Simulate** to run multiple cycles

### Emergency Test

1. Select any direction
2. Add an Ambulance, Fire Truck, or Police vehicle
3. Run a signal cycle
4. The emergency lane receives instant green priority

---

## Keyboard Shortcuts

| Key | Action |
| --- | --- |
| `Enter` | Run one signal cycle |
| `Space` | Run one signal cycle |
| `A` | Add selected vehicle |
| `S` | Start auto simulation |
| `R` | Reset system |

---

## UI Design

SmartFlow uses a modern dark interface with glassmorphism-inspired panels, animated status indicators, and clean traffic dashboard styling.

| Element | Design Style |
| --- | --- |
| Theme | Dark glassmorphism |
| Accent | Cyan, green, red, amber, purple |
| Typography | Inter and JetBrains Mono |
| Signals | Glow states and animated feedback |
| Logs | Timestamped and color-coded |

---

## Example Scenario

Suppose the lanes contain:

| Lane | Vehicles | Score |
| --- | --- | --- |
| North | 3 Cars | `[0, 3, 3]` |
| South | 1 Bus + 1 Truck | `[0, 8, 2]` |
| East | 1 Ambulance | `[1, 100, 1]` |
| West | 5 Motorcycles | `[0, 5, 5]` |

SmartFlow chooses **East** because it contains an emergency vehicle.

If no emergency vehicle exists, SmartFlow chooses the lane with the highest total weight.

---

## Potential Enhancements

- Real-time random traffic arrival using probability distributions
- Pedestrian crossing signals
- Rush-hour traffic mode
- Multi-intersection smart network
- Analytics dashboard with throughput charts
- Live traffic API integration
- Mobile-first touch controls
- Exportable simulation reports

---

## Tech Stack

| Technology | Purpose |
| --- | --- |
| HTML5 | Semantic structure |
| CSS3 | Responsive glassmorphism UI |
| JavaScript | Scheduling algorithm and simulation logic |
| Google Fonts | Professional typography |

---

## Learning Outcomes

This project demonstrates:

- Priority Queue scheduling
- Max-Heap inspired decision making
- Lexicographic score comparison
- DOM manipulation
- Event-driven JavaScript
- UI state management
- Simulation design
- Emergency override logic

---

## License

This project is open-source and available for educational, portfolio, and demonstration use.

---

## Author

Built with focus on intelligent transportation, clean UI design, and practical algorithm visualization.

If you use this project, consider giving the repository a star.

---

<p align="center">
  <strong>SmartFlow v2.0</strong><br>
  Priority Queue Scheduling Engine for Intelligent Traffic Control
</p>
