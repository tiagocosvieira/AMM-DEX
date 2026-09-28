# AMM DEX (Uniswap V1-style)

**Educational Automated Market Maker** built with **Solidity**, **TypeScript**, and **Next.js**.

> Invariant `x * y >= k` rigorously verified via fuzz testing.

---

## 🎯 What is This Project?

A functional **Decentralized Exchange (DEX)** implementing an **Automated Market Maker (AMM)** model inspired by **Uniswap V1**. The project is structured as a professional **Monorepo** to demonstrate clean software architecture, strict domain separation, shared business logic, and advanced invariant testing in Smart Contracts.

- **Live App:** [amm-dex.vercel.app](https://amm-dex.vercel.app) _(example)_
- **Sepolia Contract:** `0x...` _(example)_

---

## 🏛️ Software Architecture (Monorepo)

The project adopts a **Monorepo** standard managed via **pnpm workspaces** and optimized with **Turborepo**, ensuring cached builds and instant onboarding.

```
amm-dex/
├── .github/workflows/         # CI pipelines (Contracts and Frontend)
├── packages/
│   ├── contracts/             # Smart Contracts (Hardhat + Solidity + Foundry)
│   ├── abi/                   # Automatically generated typed ABIs
│   └── shared/                # Pure shared logic (math.ts, constants)
├── apps/
│   └── web/                   # Web interface (Next.js + Tailwind + Viem/Wagmi)
└── docs/                      # Detailed technical documentation (Math, Architecture, Security)
```

### 🔍 Layer Breakdown & Justifications

| Layer / Folder | Architectural Role |
| --- | --- |
| `packages/contracts` | Isolates the Web3 domain and smart contracts in Solidity (`AMM.sol`, `LPToken.sol`), containing unit tests and invariant fuzzing. |
| `packages/shared` | **Architectural Gold:** Centralizes pure AMM mathematical rules (`math.ts`). It is consumed by both tests and the frontend to guarantee exact calculation parity. |
| `packages/abi` | Guarantees end-to-end (E2E) type safety between smart contracts and the web application, entirely eliminating the use of `any`. |
| `apps/web` | User interface built with Next.js (App Router), consuming reactive hooks powered by Viem and Wagmi v2. |

---

## 🧮 The Mathematics Behind the AMM

The protocol is governed by the classic **constant product invariant**:

$$
\text{Invariant: } x \cdot y = k
$$

**Swap Fee:** Fixed `0.3%` per swap embedded directly into virtual reserves.

**Output Formula:**

$$
\Delta y = \frac{y \cdot \Delta x_{real}}{x + \Delta x_{real}}
$$

**Impermanent Loss (IL):**

$$
IL = \frac{2\sqrt{r}}{1 + r} - 1 \quad (\text{where } r = \text{price ratio})
$$

Complete details and algebraic proofs can be found in [`docs/MATH.md`](docs/MATH.md).

---

## 🛠️ Technology Stack

| Domain | Technology / Tool |
| --- | --- |
| **Smart Contracts** | Solidity 0.8.24, OpenZeppelin Contracts, Hardhat |
| **Testing & Fuzzing** | Hardhat Test, Foundry (`forge-std`) for invariant tests |
| **Frontend** | Next.js 14 (App Router), TypeScript, Tailwind CSS |
| **Web3 Client** | Viem, Wagmi v2, TanStack React Query |
| **Monorepo** | pnpm Workspaces, Turborepo |
| **CI/CD** | GitHub Actions |

---

## 📦 How to Run Locally

Make sure you have **Node.js (>=18)** and **pnpm** installed on your machine.

### 1. Clone the repository and install dependencies

```bash
git clone https://github.com/your-username/amm-dex.git
cd amm-dex
pnpm install
```

### 2. Compile and run contract tests

```bash
pnpm contracts:compile
pnpm contracts:test
```

### 3. Run the Frontend in development mode

```bash
pnpm web:dev
```

Open your browser and navigate to [http://localhost:3000](http://localhost:3000).

---

## 📊 Quality and Security Guarantees

The repository includes engineering benchmarks targeted at production-grade environments and audits:

- **Advanced Fuzz Testing:** The contract goes through thousands of randomized runs ensuring the invariant `x * y >= k` never decreases post-fees.
- **Reentrancy Protection:** Utilization of OpenZeppelin's `ReentrancyGuard` across all critical fund transfer functions.
- **Automated CI:** Independent workflows validate contract integrity and frontend lint/build status on every commit.