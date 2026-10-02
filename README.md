# CarbonIQ – Industrial Carbon Intelligence & Reduction Engine

> An enterprise-grade, AI-powered industrial carbon management platform designed for factories, manufacturing facilities, logistics enterprises, sustainability managers, and environmental engineers.

---

## 🌟 Overview

**CarbonIQ** automates Scope 1, Scope 2, and Scope 3 greenhouse gas (GHG) accounting, pinpoints top emitting activities, dynamically generates AI insights, optimizes sustainability budget allocations using a multi-objective Knapsack solver, and enables real-time "What-If" decarbonization scenario modeling.

Built as a full-stack production React + Vite application with TypeScript, Tailwind CSS, and Recharts, CarbonIQ complies with ISO 14064 and GHG Protocol guidelines.

---

## 🚀 Key Features

1. **Executive Dashboard**: Real-time KPI cards for Total CO₂e, Monthly Emissions, Source Breakdown, Sustainability Budget, Potential CO₂ Reduction, Net ROI, Annual Savings, and Carbon Intensity per unit.
2. **Itemized Emission Calculator**: Transparent forms for Electricity, Fuel Combustion, Logistics Transport, Production Processes, and Waste Disposal with live formula cards.
3. **Emissions Analysis & Breakdown**: Interactive pie/donut charts, monthly stacked bar charts, Scope 1/2/3 breakdown cards, and category source ranking matrix.
4. **AI Carbon Reduction Engine**: 10+ pre-evaluated decarbonization projects (Solar PV, VFDs, Biomass Boilers, Freight Rail, Waste Heat Recovery) with ROI, payback years, implementation time, and difficulty rating.
5. **AI Sustainability Budget Optimizer** *(Core Feature)*: Multi-objective Knapsack dynamic programming solver that allocates a user's fixed budget (e.g., ₹10,00,000) to maximize total annual CO₂ reduction while considering ROI and payback.
6. **Interactive "What-If" Simulator**: Live sliders for electricity usage, renewable energy share, fuel consumption, logistics route optimization, waste recycling, and machine efficiency with immediate before-vs-after comparison graphs.
7. **Strategic Scenario Matrix**: Side-by-side comparison of Business as Usual (BAU), Solar + Energy Efficiency, Logistics Optimization, and Maximum Decarbonization.
8. **Dynamic AI Insights**: Natural language summary automatically generated from company baseline data, highlighting top emission drivers and high-ROI quick wins.
9. **Phase-wise Implementation Roadmap**: 4-phase Gantt execution timeline (Immediate, 3–6m, 6–12m, 12m+).
10. **Executive Report Generation**: ISO-compliant report view with print-to-PDF support (`window.print()`).

---

## 📐 Project Architecture

```
c:\Users\HP ELITEBOOK\Documents\github project\
├── index.html                  # HTML5 Entry Point
├── package.json                # Project Dependencies & Scripts
├── tailwind.config.js          # Industrial Sustainability Theme & Dark Mode
├── tsconfig.json               # TypeScript Configuration
├── vite.config.ts              # Vite Bundler Setup & Path Aliases
└── src/
    ├── App.tsx                 # Main Module Router & Layout Shell
    ├── main.tsx                # React DOM Mount Entry Point
    ├── types/
    │   └── index.ts            # Domain Entities, Database Schemas & Types
    ├── engine/                 # Decoupled Business & Optimization Engine
    │   ├── emissionFactors.ts  # Standard Factor Database (IPCC, CEA, EPA, DEFRA)
    │   ├── calculator.ts       # Scope 1, 2 & 3 Mathematical Calculations
    │   ├── budgetOptimizer.ts  # Multi-Objective Knapsack Solver Algorithm
    │   ├── scenarioSimulator.ts# What-If Sensitivity & Scenario Solver
    │   └── aiInsights.ts       # Dynamic Natural Language Insight Generator
    ├── data/
    │   └── demoData.ts         # 12 Months Realistic Data (Apex Manufacturing Solutions)
    ├── context/
    │   └── CarbonContext.tsx   # Persistent State Manager (Auth, Budget, Currency, Records)
    └── components/             # Modular UI Views
        ├── layout/             # Sidebar, Header, Theme Switcher
        ├── dashboard/          # Dashboard View & KPI Cards
        ├── calculator/         # Electricity, Fuel, Logistics, Production, Waste Forms
        ├── breakdown/          # Scope 1/2/3 Breakdown & Recharts Stacked Charts
        ├── aiEngine/           # AI Recommendation Cards & Detail Modal
        ├── optimizer/          # Budget Input Slider & Solver Results
        ├── simulator/          # Interactive Sliders & Live Comparison Graph
        ├── scenarios/          # Strategic Comparison Matrix Table
        ├── insights/           # Dynamic NLP Takeaways
        ├── roadmap/            # 4-Phase Implementation Timeline
        ├── reports/            # PDF Executive Report Preview
        ├── settings/           # Company Profile & Currency Switcher
        └── auth/               # Login Screen & Demo Shortcuts
```

---

## 🗄️ Database Schema & Entities

The application models data structures ready for database persistence (e.g., PostgreSQL / SQLite / MongoDB):

```typescript
// Users & Company Entities
User { id, name, email, role, companyId, avatar }
Company { id, name, industry, location, currency, targetYear, reductionTargetPct }

// Activity Records
ElectricityRecord { kwh, gridFactor, renewablePct, cost }
FuelRecord { fuelType, quantity, unit, emissionFactor, cost }
LogisticsRecord { mode, distanceKm, weightTonnes, trips, emissionFactor }
ProductionRecord { quantityUnits, rawMaterialTonnes, machineKwh, productFactor }
WasteRecord { wasteType, quantityTonnes, recyclingPct, disposalMethod, emissionFactor }

// Aggregate Monthly Baseline Record
EmissionRecord {
  id, month, year, electricity, fuel, logistics, production, waste,
  totalElectricityCo2e, totalFuelCo2e, totalLogisticsCo2e, totalProductionCo2e, totalWasteCo2e,
  totalCo2eTonnes, carbonIntensity
}

// Decarbonization Action Entity
ReductionAction {
  id, name, category, co2ReductionTonnes, cost, annualSavings,
  roi, paybackYears, difficulty, implementationWeeks, description, isAllocated
}
```

---

## 🧮 How Carbon Calculations Work

All calculations follow standard IPCC / GHG Protocol methodologies:

### 1. Scope 2 – Electricity Grid Power
$$\text{CO}_2\text{e (t)} = \frac{\text{kWh} \times (1 - \frac{\text{Renewable\%}}{100}) \times \text{Grid Emission Factor}}{1000}$$
*Default Grid Factor: 0.716 kg CO₂e/kWh (India National Grid Average)*

### 2. Scope 1 – Fuel Combustion
$$\text{CO}_2\text{e (t)} = \frac{\text{Fuel Quantity} \times \text{Fuel Factor}}{1000}$$
*Examples: Diesel = 2.68 kg CO₂e/L, Petrol = 2.31 kg CO₂e/L, Natural Gas = 2.02 kg CO₂e/m³*

### 3. Scope 3 – Freight Logistics
$$\text{CO}_2\text{e (t)} = \frac{\text{Distance (km)} \times \text{Weight (t)} \times \text{Trips} \times \text{Transport Factor}}{1000}$$
*Factors: Heavy Truck = 0.12 kg CO₂e/t-km, Rail = 0.03 kg CO₂e/t-km, Ship = 0.015 kg CO₂e/t-km*

### 4. Scope 1 & 2 Blend – Manufacturing Process
$$\text{CO}_2\text{e (t)} = \frac{(\text{Units} \times \text{Process Factor}) + (\text{Machine kWh} \times \text{Grid Factor})}{1000}$$

### 5. Scope 3 – Waste Disposal
$$\text{CO}_2\text{e (t)} = \frac{(\text{Waste Weight (t)} \times 1000) \times (1 - \frac{\text{Recycling\%}}{100}) \times \text{Disposal Factor}}{1000}$$

---

## 💡 How the Budget Optimization Algorithm Works

The optimization algorithm maximizes total decarbonization within a user's budget constraint:

$$\max \sum_{i \in S} \text{CO}_2\text{Reduction}_i \quad \text{subject to} \quad \sum_{i \in S} \text{Cost}_i \le \text{Budget}$$

### Algorithm Execution Steps:
1. **Decarbonization Efficiency Ranking**: Evaluates each project's Decarbonization Efficiency Index ($E_i$):
   $$E_i = \left(\frac{\text{CO}_2\text{Reduction}_i}{\text{Cost}_i}\right) \times \left(1 + \frac{\text{ROI}_i}{100}\right)$$
2. **Knapsack Greedy Allocation**: Iterative dynamic allocation selecting projects with highest $E_i$ until available budget is consumed.
3. **Portfolio Analytics**: Computes total investment spent, remaining unallocated budget, net annual OPEX savings, total CO₂ reduction achieved, and portfolio weighted payback period.

---

## 💻 How to Run the Project

### Prerequisites
- **Node.js**: v18.x or higher installed.

### Installation & Launch

```bash
# 1. Clone the repository
git clone https://github.com/your-username/carboniq-industrial-sustainability.git
cd carboniq-industrial-sustainability

# 2. Install dependencies
npm install

# 3. Start the development server
npm run dev
```

Open your browser at `http://localhost:5173`.

### Building for Production

```bash
npm run build
```

---

## 🔑 Demo Login Credentials

You can log in directly using the pre-configured accounts:

| Role | Email | Password |
| :--- | :--- | :--- |
| **Sustainability Manager (Admin)** | `admin@carboniq.io` | `password123` |
| **Environmental Engineer** | `manager@apexindustrial.com` | `sustain2026` |

---

## 🔮 Future Improvements

1. **IoT Sensor & Smart Meter Integration**: Connect via Modbus / MQTT for real-time telemetry from factory sub-meters.
2. **Carbon Offset Marketplace API**: Integrate API connectors for verified Gold Standard / Verra carbon credits.
3. **Multi-Facility Enterprise Rollup**: Support multi-site global corporate hierarchies with regional grid emission factor auto-lookup.
4. **Supply Chain Portal**: Scope 3 Tier 1 supplier portal for automated supplier carbon scorecards.

---

## 📄 License

Distributed under the MIT License.
