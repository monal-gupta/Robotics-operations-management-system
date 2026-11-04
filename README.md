# RoboFleet Operations Management System (ROMS)

A practical **Robotics Operations Management System (ROMS)** designed for a robotics startup operating autonomous mobile robots (AMRs) in a fulfillment warehouse facility.

This system transforms a robot telemetry viewer into a full operations command center, demonstrating workforce coordination, mission task dispatching, incident reliability management, and high-level fleet performance monitoring.

---

## 🎯 Executive Overview & Purpose

In commercial robotics deployments, hardware telemetry (position, battery, and speed) is only half the operational equation. The other critical half is **operations management**:
- Which field technician is on duty to unblock a stalled robot?
- Are fulfillment picking and hauling missions being completed before their customer SLA deadlines?
- When a robot triggers an emergency stop, how quickly is it triaged and resolved?
- What is the overall fleet utilization percentage across shifts?

This project is tailored to demonstrate real-world operations management skills for a **Robotics Operations Intern / Fleet Operations Specialist** role.

---

## 🌐 Live Architecture & Module Overview

```
                      ┌───────────────────────────────────────────────┐
                      │          Top Navigation & Workspace           │
                      │  (Simulated AMR Warehouse Telemetry Badge)   │
                      └───────┬───────────────────────────────┬───────┘
                              │                               │
            ┌─────────────────┴─────────────┐   ┌─────────────┴─────────────────┐
            │   Fleet Monitoring Layer      │   │   Operations Management Layer │
            │   (Original Core Engine)      │   │   (Workforce, Tasks, Faults)  │
            ├───────────────────────────────┤   ├───────────────────────────────┤
            │ • 8-Robot Map Layout (r1-r8)  │   │ • 👷 Operator Management      │
            │ • Recorded Log Replay (1x-10x)│   │ • 📋 Task & Mission Dispatch  │
            │ • Synthetic Live Simulation   │   │ • 🚨 Incident Management      │
            │ • Robot Details & Search      │   │ • 📊 Operations Dashboard     │
            │ • Trend Chart History         │   │ • LocalStorage Demo State     │
            └───────────────────────────────┘   └───────────────────────────────┘
```

---

## 🚀 Key Modules & Implemented Features

### 1. 👷 Operator Management (Workforce Coordination)
- **Operator Profiles**: Detailed profiles with names, roles (Lead Fleet Supervisor, Autonomous Systems Specialist, Hardware Field Tech, Dispatcher), contact info, and assigned robots.
- **Availability Tracking**: Real-time status tracking (`Available`, `Busy`, `On Break`, `Off Duty`) with color-coded status badges.
- **Shift Allocation**: Workforce shift management across **Morning** (06:00–14:00), **Evening** (14:00–22:00), and **Night** (22:00–06:00) shifts.
- **Robot Assignment**: Multi-robot assignment chips with quick unassign (`×`) and dynamic dropdown to assign robots `r1` through `r8`.
- **+ Add Operator**: Modal to dynamically register new operators into the active roster.

### 2. 📋 Task Management (Mission Planning & Dispatch)
- **Task Lifecycle**: Full operational lifecycle tracking (`Pending` ➔ `In Progress` ➔ `Completed` / `Failed`).
- **Priority & Deadlines**: Priority classification (`Low`, `Medium`, `High`, `Critical`) and customer SLA deadlines (e.g. `"12:30 PM"`).
- **Task Assignment**: Tasks assigned directly to both a specific AMR (`r1`–`r8`) and an active supervising operator.
- **Completion Statistics Bar**: Real-time calculation of Total Missions, Completed, In Progress, Pending, Failed, and overall **Task Completion Rate %** with a visual progress bar.
- **+ Create & Edit Tasks**: Modal form to dispatch new warehouse missions or update priorities on the fly.

### 3. 🚨 Incident Management (Reliability & Fault Triage)
- **Manual Incident Logging**: Log robot stoppages, sensor degradation, charging misalignment, and obstacle lockouts.
- **Severity Categorization**: 4-tier operational severity rating:
  - **Minor**: Non-blocking alerts (e.g. dusty LiDAR casing).
  - **Moderate**: Degraded performance or dock charging realignment.
  - **Major**: Hardware or navigation malfunction requiring intervention.
  - **Critical**: Safety-critical stoppages or active E-stops.
- **Technician Assignment**: Directly assigns the responsible operator to investigate.
- **Status Workflow**: Tracks `Open` ➔ `In Progress` ➔ `Resolved` with resolution timestamps and post-incident resolution notes.
- **Audit History**: Complete historical log for continuous reliability improvements (MTTR).

### 4. 📊 Operations Dashboard (Executive Command Center)
- **Unified Fleet KPIs**:
  - **Fleet Utilization %**: Dynamically calculated percentage of working AMRs over total fleet:
    $$\text{Utilization \%} = \left(\frac{\text{Working Robots}}{\text{Total Robots}}\right) \times 100$$
  - **Robot Availability**: Available (working + idle) vs. Unavailable (faulted/attention).
  - **Active Workforce**: On-duty operators count with shift breakdown.
  - **Task Completion %**: Fulfillment mission SLA throughput percentage.
  - **Active Incidents**: Open alerts and active critical safety stoppages.
- **Visual Analytics & Lightweight Charts**:
  - **Fleet Status Distribution Bar**: Stacked visual chart showing Working (green), Idle (sky blue), and Needs Attention (red).
  - **Mission Pipeline Breakdown**: Stacked visual chart of Completed, In Progress, Pending, and Failed tasks.
  - **Robot Operational Summary Table**: Live table showing all 8 robots, their current battery %, active missions, and supervising operator.

### 5. 🤖 Fleet Telemetry & Monitoring (Preserved Core)
- The entire original **RoboFleet Monitor** remains 100% operational in the **"🤖 Fleet Monitoring"** tab:
  - Warehouse map layout with coordinates.
  - 1x, 2x, 5x, 10x replay playback engine.
  - Client-side Markovian synthetic live simulation.
  - Robot search and "Needs attention" filters.
  - Fleet working percentage trend chart over time.

---

## 🔬 Simulated vs. Real Robot Data: What’s the Difference?

| Feature | This Simulation (ROMS) | Real-World Robotics Production System |
|---|---|---|
| **Data Transport** | Client-side event replay and Markovian simulation in browser memory | ROS 2 (`rclpy`/`rclcpp`), MQTT broker, or WebSocket telemetry daemon |
| **Telemetry Ingestion** | Pre-recorded JSONL log / browser timer | Micro-ROS / edge telemetry agent running on robot compute (NVIDIA Jetson / x86) |
| **State Persistence** | React state + browser `localStorage` | Distributed PostgreSQL database, Redis state cache, or time-series DB (InfluxDB) |
| **Task Dispatch** | Client-side task state machine | Fleet Management System (FMS) / Warehouse Management System (WMS) API |
| **Safety E-Stops** | Manually logged / simulated fault transitions | Hardware-rated safety relays, SIL-2 certified LiDAR scanners, physical e-stop buttons |

---

## 🛠️ Technology Stack

- **Framework**: React 19 + TypeScript
- **Bundler**: Vite 8
- **Testing**: Vitest 4 + React Testing Library + JSDOM
- **Styling**: Modern, semantic CSS with responsive CSS Grid and Flexbox (zero heavy external UI/chart dependencies)
- **Architecture**: Context API + LocalStorage state persistence + Pure domain mathematical modeling

---

## 📦 Running Locally

### 1. Install Dependencies
```bash
cmd /c npm install
```

### 2. Start the Development Server
```bash
cmd /c npm run dev
```
Open [http://localhost:5173](http://localhost:5173) in your browser.

### 3. Run Automated Tests
```bash
cmd /c npm run test
```
Runs the full Vitest suite (17 test files, 121 tests passing).

### 4. Build for Production
```bash
cmd /c npm run build
```
Type checks with `tsc -b` and builds production bundle into `dist/`.

---

## 💡 Operations Intern Interview Guide

When presenting this project in an interview, here are key operational talking points:

1. **Why Track Robot Utilization?**
   > *"Robots are capital assets (CapEx). If a warehouse owns 8 autonomous mobile robots costing \$30,000 each, an idle robot sitting in a corner is lost throughput. Tracking utilization % ensures the fleet is actively delivering value and reveals if order dispatching is lagging."*

2. **Why Shift Allocation & Availability Matter?**
   > *"A common mistake in robotics operations is treating autonomy as 'zero human touch'. When a robot stops in an aisle due to shrink wrap caught in its wheels, operations needs to immediately know which technician is on duty right now, on which shift, and whether they are available or on break."*

3. **Why Categorize Incidents by Severity?**
   > *"Not all robot faults are equal. A dusty LiDAR casing is a Minor issue that can wait for scheduled shift maintenance. A triggered physical E-Stop or navigation lockout in an active forklift corridor is Critical and requires immediate intervention. Triage ensures high-priority safety incidents are cleared first to minimize Mean Time to Resolution (MTTR)."*
