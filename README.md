# Cheaters Dilemma

> Multi-agent game theory simulation modeling competition, alliances, and defection in decentralized token economies.

![Cheaters Dilemma Banner](assets/images/banner.svg)

<p align="center">
  <img src="https://img.shields.io/badge/Python-3.11+-3776AB?style=for-the-badge&logo=python&logoColor=white" alt="Python">
  &nbsp;
  <img src="https://img.shields.io/badge/Solidity-0.8.20-363636?style=for-the-badge&logo=solidity&logoColor=white" alt="Solidity">
  &nbsp;
  <img src="https://img.shields.io/badge/Next.js_14-000000?style=for-the-badge&logo=nextdotjs&logoColor=white" alt="Next.js">
  &nbsp;
  <img src="https://img.shields.io/badge/Simulation-Mesa_Framework-F59E0B?style=for-the-badge" alt="Mesa">
  &nbsp;
  <img src="https://img.shields.io/badge/FastAPI-009688?style=for-the-badge&logo=fastapi&logoColor=white" alt="FastAPI">
  &nbsp;
  <img src="https://img.shields.io/badge/SLA-24_Hours-8B5CF6?style=for-the-badge" alt="SLA">
</p>

<p align="center">
  <a href="https://cheaters-dilemma.vercel.app"><img src="https://img.shields.io/badge/Live_Demo-Open_Simulation-F59E0B?style=flat-square" alt="Live App"></a>
  &nbsp;
  <a href="LICENSE"><img src="https://img.shields.io/badge/License-MIT-blue?style=flat-square" alt="MIT License"></a>
  &nbsp;
  <a href="CONTRIBUTING.md"><img src="https://img.shields.io/badge/Contributions-Welcome-blueviolet?style=flat-square" alt="Contributing"></a>
</p>

---

## Quick Start & Getting Started

Getting Cheaters Dilemma running locally takes only two steps:

```bash
git clone https://github.com/Luxion-Labs/Cheaters-Dilemma.git
cd Cheaters-Dilemma
pip install -r requirements.txt
```

Launch the simulation dashboard:
```bash
npm install && npm run dev
```

---

---

## 2. Motivation & Idea

Traditional game theory often simplifies agent interactions, focusing on equilibrium states in well-defined scenarios. However, real-world systems are dynamic, agents possess memory, and rules can be changed from within. The Cheater's Dilemma moves beyond these limits by simulating a complex environment where agents **learn, adapt, and strategically manipulate the rules themselves.**

The motivation stems from the observation that in many systems, particularly those involving tokens or voting power, the initial conditions and individual behaviors can lead to emergent properties like **governance capture** and **institutional corruption**. By creating a multi-agent simulation, we can observe these complex phenomena in a controlled, reproducible manner, offering insights into the mechanisms that drive such outcomes. This project aims to bridge the gap between theoretical game mechanics and the emergent complexity of adaptive multi-agent systems.

---

## 3. Core Concepts

### 3.1 Agents

Agents in the Cheater's Dilemma are **autonomous entities** endowed with:
*   **Memory**: Agents remember past interactions.
*   **Personality Traits**: Defined by varying weightings of utility factors (e.g., Conservative, Aggressive, Politician, Opportunist).
*   **Utility-Based Decision-Making**: Agents assess potential actions based on expected token gains, risks (like retaliation), governance influence, and reputation loss.

Their strategies can **evolve** based on the outcomes of their interactions, leading to diverse and dynamic behaviors.

### 3.2 Strategies

Agents employ various strategies, ranging from cooperative to highly self-serving:
*   **Cooperation**: Agents may work to generate tokens, benefiting all.
*   **Defection**: Agents might choose to steal from others, prioritizing individual gain.
*   **Conditional Behavior**: Nuanced strategies like retaliating only when stolen from, or forming implicit alliances.
*   **Governance Manipulation**: Agents with sufficient token power can strategically vote to change rules to their advantage.

### 3.3 Interactions

The simulation models rich interactions between agents:
*   **Cooperation and Cheating**: Agents decide whether to contribute to the common good or exploit others' resources.
*   **Alliances**: Implicit alliances can form as agents learn to cooperate with or avoid certain peers.
*   **Conflict and Elimination**: Aggressive actions like 'attack' can lead to the elimination of other agents, concentrating power and resources.
*   **Token-Weighted Governance**: Agents use their accumulated tokens to vote on rule changes, directly influencing the game's mechanics.

---

## 4. System Architecture

The Cheater's Dilemma follows a **clean architectural separation**, ensuring modularity and clarity across its core components.

```
┌─────────────────────────────────────────────────────────────┐
│                    SIMULATION LAYER                          │
│  (Pure Deterministic Python - Off-Chain)                     │
│                                                               │
│  • Token-based economics                                     │
│  • Utility-based agent decisions                             │
│  • Token-weighted governance voting                          │
│  • Deterministic RNG                                         │
│  • No blockchain calls                                       │
└─────────────────────────────────────────────────────────────┘
                            │
                            │ JSON Results
                            ▼
┌─────────────────────────────────────────────────────────────┐
│                   DEPLOYMENT LAYER                           │
│  (Post-Simulation - One-Time)                                │
│                                                               │
│  • Read simulation output                                    │
│  • Generate agent wallets                                    │
│  • Compute simulation hash                                   │
│  • Deploy ERC20 contract                                     │
│  • Mint token distribution                                   │
└─────────────────────────────────────────────────────────────┘
                            │
                            │ Contract Address
                            ▼
┌─────────────────────────────────────────────────────────────┐
│                   BLOCKCHAIN LAYER                           │
│  (Monad Testnet - Immutable)                                 │
│                                                               │
│  • DILEMMA (DLM) ERC20 Token                                 │
│  • Immutable simulation hash                                 │
│  • Non-transferable tokens                                   │
│  • Final political power encoded                             │
└─────────────────────────────────────────────────────────────┘
```

*   **Backend Simulation Engine**: Developed in **Python**, this is the core of the system, running deterministic multi-agent simulations. It handles agent decision-making, interactions, token economics, and governance mechanics, producing detailed JSON output.
*   **Frontend Visualization**: A **Next.js (React)** application that provides an interactive interface for configuring simulations, visualizing live events, and replaying past simulation outcomes. It consumes data from the backend API.
*   **Smart Contracts**: Written in **Solidity** and deployed using **Foundry**, these contracts secure the final state of the simulation on a blockchain (e.g., Monad Testnet). The `DilemmaToken` ERC20 contract encodes the immutable token distribution and simulation hash, representing the final power structure.

---

## 5. Codebase Structure

The project is organized into distinct directories for clarity and separation of concerns:

```
.
├── backend/                  # Python-based simulation engine and API
│   ├── app/                  # FastAPI application for simulation logic
│   │   ├── agents/           # Implementations of various AI agent strategies
│   │   ├── api/              # API routes and schemas for frontend interaction
│   │   ├── core/             # Core simulation mechanics (governance, rules, RNG)
│   │   ├── domain/           # Business logic (agent models, world state, actions)
│   │   └── services/         # Integration with blockchain, analytics, etc.
│   ├── data/                 # Example simulation results and seed data
│   ├── replays/              # Stored simulation replays
│   └── scripts/              # Utility scripts (deployment, wallet generation)
├── contracts/                # Smart contracts for on-chain token distribution
│   ├── DilemmaToken.sol      # The ERC20 contract encoding simulation results
│   └── foundry.toml          # Foundry configuration for contract development
└── frontend/                 # Next.js (React) application for visualization
    ├── public/               # Static assets
    ├── src/
    │   ├── app/              # Next.js pages and layouts
    │   ├── components/       # Reusable React components (charts, game board, UI)
    │   ├── hooks/            # Custom React hooks for data fetching, etc.
    │   └── lib/              # Utility functions, API clients, contract interactions
    └── package.json          # Frontend dependencies and scripts
```

---

## 6. Documentation

Comprehensive documentation is available in the `docs/` directory, providing in-depth explanations of the project's vision, architecture, and technical details.

To get started, we recommend the following documents:
*   **[00_overview.md](docs/00_overview.md)**: A high-level introduction to the project.
*   **[01_vision_and_theory.md](docs/01_vision_and_theory.md)**: Delves into the theoretical underpinnings and core ideas.
*   **[02_system_architecture.md](docs/02_system_architecture.md)**: Detailed breakdown of the system's design.
*   **[06_codebase_guide.md](docs/06_codebase_guide.md)**: A guide for navigating and understanding the codebase.
*   **[08_extending_and_research.md](docs/08_extending_and_research.md)**: Notes on how to extend the system for research or new features.

For a complete overview, please explore the `docs/` directory.

---

## 7. Running the Project

This section guides you through setting up and running the Cheater's Dilemma locally.

### Prerequisites

*   **Python 3.10+** (for the backend simulation)
*   **Node.js 18+** (for the frontend visualization)
*   **Foundry** (for smart contract development and deployment)

### Local Setup

1.  **Clone the repository:**
    ```bash
    git clone https://github.com/your-repo/cheaters-dilemma.git
    cd cheaters-dilemma
    ```
2.  **Backend Setup:**
    ```bash
    cd backend
    pip install -r requirements.txt # Or use your preferred Python package manager
    ```
3.  **Frontend Setup (Optional, for visualization):**
    ```bash
    cd frontend
    npm install # Or `yarn install` / `bun install`
    ```
4.  **Smart Contract Setup (Optional, for development/deployment):**
    ```bash
    cd contracts
    forge build
    ```

### Running Simulations

To run a basic simulation and deploy its results to a local blockchain (simulated):

```bash
# 1. Run the simulation
cd backend
python demo_flow.py

# 2. Deploy tokens (demo mode)
python scripts/deploy_token.py simulation_results_demo_seed42.json

# 3. Verify determinism
python verify_determinism.py
```
For more detailed instructions, refer to **[QUICK_START.md](QUICK_START.md)**.

### Running the Frontend

To start the frontend visualization application:

```bash
cd frontend
npm run dev
```
The application will typically be available at `http://localhost:3000`.

---

## 8. Extending the System

The Cheater's Dilemma is designed for **experimentation and research**.

*   **Adding New Strategies**: Implement new `Agent` subclasses in `backend/app/agents/` to explore different behavioral models.
*   **Modifying Rules**: Adjust parameters in `backend/app/core/rules.py` or modify the governance mechanism in `backend/app/core/governance.py` to test various game mechanics.
*   **Experimentation and Research**: The deterministic nature of the simulation allows for reproducible experiments to study emergent phenomena, policy impacts, and the evolution of agent behaviors.

Refer to **[08_extending_and_research.md](docs/08_extending_and_research.md)** for more in-depth guidance.

---

## 9. Limitations & Roadmap

### Known Limitations

*   **Computational Intensity**: Complex simulations with many agents or turns can be resource-intensive.
*   **Simplistic Agent Learning**: Current agents follow predefined personality traits; more advanced adaptive learning mechanisms could be explored.
*   **Frontend Scope**: The current frontend focuses on visualization; interactive agent creation or real-time rule modification is not yet implemented.

### Short-Term Improvements

*   Optimize simulation performance for larger scale runs.
*   Enhance frontend interactivity, allowing users to configure and launch simulations directly from the UI.
*   Integrate more sophisticated analytics and visualization tools.

### Long-Term Vision

The long-term vision includes developing a framework for agents to **dynamically learn and adapt their strategies**, exploring more complex governance structures (e.g., delegated voting, liquid democracy), and integrating with real-world decentralized protocols for live experimentation.

For a detailed roadmap, please consult **[09_limitations_and_roadmap.md](docs/09_limitations_and_roadmap.md)**.

---

## 10. License

```
MIT License

Copyright (c) [year] [fullname]

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```

---

## Acknowledgments

*   **OpenZeppelin**: For robust ERC20 implementations.
*   **Foundry**: For a powerful smart contract development toolkit.
*   **Monad**: For providing a high-performance testnet environment.
*   **Python**: The backbone of our simulation engine.
*   **Next.js & React**: For the flexible and performant frontend.
*   **FastAPI**: For the efficient and modern backend API.


---

## Maintainer Response SLA
Luxion Labs enforces a strict 24-hour response SLA on all community issues, feature proposals, and pull requests. See [CONTRIBUTING.md](CONTRIBUTING.md) for full development guidelines.

## License
Distributed under the MIT License. See [LICENSE](LICENSE) for details.
