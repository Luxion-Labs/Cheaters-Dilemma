<p align="center">
  <img src="https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white" alt="Python" height="30"/>
  <img src="https://img.shields.io/badge/TypeScript-007ACC?style=for-the-badge&logo=typescript&logoColor=white" alt="TypeScript" height="30"/>
  <img src="https://img.shields.io/badge/Solidity-363636?style=for-the-badge&logo=solidity&logoColor=white" alt="Solidity" height="30"/>
  <img src="https://img.shields.io/badge/Next.js-000000?style=for-the-badge&logo=next.js&logoColor=white" alt="Next.js" height="30"/>
  <img src="https://img.shields.io/badge/React-61DAFB?style=for-the-badge&logo=react&logoColor=white" alt="React" height="30"/>
  <img src="https://img.shields.io/badge/Tailwind_CSS-06B6D4?style=for-the-badge&logo=tailwind-css&logoColor=white" alt="Tailwind CSS" height="30"/>
  <img src="https://img.shields.io/badge/Ethers.js-292929?style=for-the-badge&logo=ethers&logoColor=white" alt="Ethers.js" height="30"/>
  <img src="https://img.shields.io/badge/FastAPI-009688?style=for-the-badge&logo=fastapi&logoColor=white" alt="FastAPI" height="30"/>
  <img src="https://img.shields.io/badge/Foundry-151515?style=for-the-badge&logo=foundry&logoColor=white" alt="Foundry" height="30"/>
</p>

# The Cheater's Dilemma

## 1. Vision

The Cheater's Dilemma is a multi-agent governance war simulation designed to explore how autonomous AI agents compete for resources, capture governance through token accumulation, and how institutional corruption can emerge naturally within such systems.

This project investigates the inherent tension between cooperation and defection in complex adaptive systems. It provides a high-level, deterministic simulation environment where AI agents with distinct strategies interact within a token-based economy and a token-weighted governance structure. The ultimate goal is to understand the dynamics of power concentration, the evolution of strategies, and the implications for real-world socio-economic and political systems, especially in decentralized contexts.

The system comprises a powerful backend simulation engine, a reactive frontend visualization, and immutable smart contracts that encode the final distribution of power.

## 2. Motivation & Idea

Traditional game theory often simplifies agent interactions, focusing on equilibrium states in well-defined scenarios. However, real-world systems are dynamic, agents possess memory, and rules can be changed from within. The Cheater's Dilemma moves beyond these limits by simulating a complex environment where agents learn, adapt, and strategically manipulate the rules themselves.

The motivation stems from the observation that in many systems, particularly those involving tokens or voting power, the initial conditions and individual behaviors can lead to emergent properties like governance capture and institutional corruption. By creating a multi-agent simulation, we can observe these complex phenomena in a controlled, reproducible manner, offering insights into the mechanisms that drive such outcomes. This project aims to bridge the gap between theoretical game mechanics and the emergent complexity of adaptive multi-agent systems.

## 3. Core Concepts

### 3.1 Agents

Agents in the Cheater's Dilemma are autonomous entities endowed with memory, personality traits, and a utility-based decision-making process. Each agent assesses potential actions based on expected token gains, risks (like retaliation), governance influence, and reputation loss. Their "personalities" (e.g., Conservative, Aggressive, Politician, Opportunist) are defined by varying weightings of these utility factors, leading to diverse and dynamic behaviors. Agents are not static; their strategies can evolve based on the outcomes of their interactions.

### 3.2 Strategies

Agents employ various strategies, ranging from cooperative to highly self-serving:
*   **Cooperation**: Agents may work to generate tokens, benefiting all.
*   **Defection**: Agents might choose to steal from others, prioritizing individual gain.
*   **Conditional Behavior**: Strategies can be more nuanced, such as retaliating only when stolen from, or forming implicit alliances.
*   **Governance Manipulation**: Agents with sufficient token power can strategically vote to change rules to their advantage.

### 3.3 Interactions

The simulation models rich interactions between agents:
*   **Cooperation and Cheating**: Agents decide whether to contribute to the common good or exploit others' resources.
*   **Alliances**: Implicit alliances can form as agents learn to cooperate with or avoid certain peers.
*   **Conflict and Elimination**: Aggressive actions like 'attack' can lead to the elimination of other agents, concentrating power and resources.
*   **Token-Weighted Governance**: Agents use their accumulated tokens to vote on rule changes, directly influencing the game's mechanics.

## 4. System Architecture

The Cheater's Dilemma follows a clean architectural separation, ensuring modularity and clarity across its core components.

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

*   **Backend Simulation Engine**: Developed in Python, this is the core of the system, running deterministic multi-agent simulations. It handles agent decision-making, interactions, token economics, and governance mechanics, producing detailed JSON output.
*   **Frontend Visualization**: A Next.js (React) application that provides an interactive interface for configuring simulations, visualizing live events, and replaying past simulation outcomes. It consumes data from the backend API.
*   **Smart Contracts**: Written in Solidity and deployed using Foundry, these contracts secure the final state of the simulation on a blockchain (e.g., Monad Testnet). The `DilemmaToken` ERC20 contract encodes the immutable token distribution and simulation hash, representing the final power structure.

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

## 6. Documentation

Comprehensive documentation is available in the `docs/` directory, providing in-depth explanations of the project's vision, architecture, and technical details.

To get started, we recommend the following documents:
*   **[00_overview.md](docs/00_overview.md)**: A high-level introduction to the project.
*   **[01_vision_and_theory.md](docs/01_vision_and_theory.md)**: Delves into the theoretical underpinnings and core ideas.
*   **[02_system_architecture.md](docs/02_system_architecture.md)**: Detailed breakdown of the system's design.
*   **[06_codebase_guide.md](docs/06_codebase_guide.md)**: A guide for navigating and understanding the codebase.
*   **[08_extending_and_research.md](docs/08_extending_and_research.md)**: Notes on how to extend the system for research or new features.

For a complete overview, please explore the `docs/` directory.

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

## 8. Extending the System

The Cheater's Dilemma is designed for experimentation and research.

*   **Adding New Strategies**: Implement new `Agent` subclasses in `backend/app/agents/` to explore different behavioral models.
*   **Modifying Rules**: Adjust parameters in `backend/app/core/rules.py` or modify the governance mechanism in `backend/app/core/governance.py` to test various game mechanics.
*   **Experimentation and Research**: The deterministic nature of the simulation allows for reproducible experiments to study emergent phenomena, policy impacts, and the evolution of agent behaviors.

Refer to **[08_extending_and_research.md](docs/08_extending_and_research.md)** for more in-depth guidance.

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

The long-term vision includes developing a framework for agents to dynamically learn and adapt their strategies, exploring more complex governance structures (e.g., delegated voting, liquid democracy), and integrating with real-world decentralized protocols for live experimentation.

For a detailed roadmap, please consult **[09_limitations_and_roadmap.md](docs/09_limitations_and_roadmap.md)**.

## 10. License

This project is licensed under the MIT License. See the `LICENSE` file for more details.

## Acknowledgments

*   **OpenZeppelin**: For robust ERC20 implementations.
*   **Foundry**: For a powerful smart contract development toolkit.
*   **Monad**: For providing a high-performance testnet environment.
*   **Python**: The backbone of our simulation engine.
*   **Next.js & React**: For the flexible and performant frontend.
*   **FastAPI**: For the efficient and modern backend API.
