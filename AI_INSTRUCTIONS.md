# AI Assistant Instructions & Project Guidelines (Template Core Repository)

## 1. Repository Purpose & Architecture Model
- **This Repository (`mtEcomWhite`) is the MASTER TEMPLATE CORE**:
  - Contains strictly the core engine: `backend/` and `dashboard/`.
  - Serves as the upstream source for all white-label client instances.
  - Zero hardcoded client secrets, domains, or tenant identifiers are tracked in this repository.
  - All tenant-specific configurations remain isolated in their respective client repositories.
  - Development and testing inside this template repository use the **`demo`** configuration (`configs/demo.json`).

---

## 2. Poly-Repo Architecture & Upstream Sync Model

### A. Client Repository Structure & Separation
- Each client has their own dedicated private GitHub repository (e.g. `client-toyoland`, `client-surokkha`, `client-decantre`, etc.).
- Client repositories contain:
  1. Core `backend/` and `dashboard/` (derived from this template).
  2. Client-specific `frontend/` codebase (unique UI/UX, styles, landing pages).
  3. Client-specific configuration and environment (`configs/<clientKey>.json`, `.env`).

### B. Upstream Update Synchronization Flow
When core features or bug fixes are developed in this master template:
1. Changes are tested and committed to this template repository (`dev` $\rightarrow$ `Live` / `master`).
2. To propagate updates to a specific client repository:
   ```bash
   cd /path/to/client-repo
   git remote add upstream https://github.com/ikram3031/mtEcomWhite.git  # (Once per repo)
   git fetch upstream
   git pull upstream Live                                              # Pulls verified core updates
   ```
3. Merge conflicts (if any, typically localized to theme/branding) are resolved and tested.
4. Client repo pushes changes to its own origin (`origin/dev` $\rightarrow$ `origin/Live`).

---

## 3. Client Onboarding Workflow
1. **Create New Client Repository**:
   - Clone or initialize a new private repository for the tenant.
2. **Copy Client Configuration Template**:
   - Copy `configs/client.template.json` to `configs/<clientKey>.json`.
   - Populate tenant credentials, brand name, domain, API URLs, policies, and color schemes.
3. **Setup Environment**:
   - Configure `.env` with tenant's MongoDB credentials, JWT secret, and integration keys.
4. **Deploy via Git-Based Agent / Panel**:
   - Production VPS pulls from the client's own `origin/Live` branch and executes `make deploy`.

---

## 4. Git Branching, Push & Merge Standards (STRICT)
- **`master` (Gold Standard / Safe Vault)**:
  - Ultimate backup branch containing only fully verified, production-stable code. Never push experimental or unverified commits directly to `master`.
- **`dev` (Active Working Branch)**:
  - All day-to-day coding, feature implementations, and bug fixes must be committed and pushed to **`dev`**.
- **`Live` (Production Deployment Source)**:
  - Deployments on production servers strictly pull from **`Live`**.
  - Merging from `dev` to `Live` is done once changes are verified and ready for deployment.
- **Explicit Deployment Command Required**:
  - Never execute production deploy scripts, container rebuilds, or VPS service restarts without explicit user instruction ("ডিপ্লয় দাও" or direct command).

---

## 5. Commit Message Format
Every git commit must strictly adhere to the format:
`<LogID>(<type>): <description>`

- **LogID Examples**: `AA01`, `TC01`, `TC02`, `DEP01`, etc.
- **Allowed Types (4 letters max)**:
  - `feat` (New feature / capability)
  - `fix`  (Bug fix)
  - `refc` (Refactoring / code cleanup)
  - `docs` (Documentation updates)
  - `perf` (Performance improvements)
  - `chor` (Chores / dependency updates)
  - `styl` (Styling / CSS / theme UI changes)
  - `test` (Testing / test scripts)
- **No Lazy Commits**: Never use generic messages like `wip`, `update`, `fix`, `changes`.

---

## 6. Code Style & Technical Guardrails
- **Dual-Backend Instance Standard**:
  - `server.domain.com` $\rightarrow$ Storefront Public API (Port 4001, lean, cached, public routes only).
  - `service.domain.com` $\rightarrow$ Dashboard Business API (Port 4002, admin routes, WebSockets, background schedulers).
- **Arrow Functions Only**: Use `const myFunc = () => {}` for functional components and custom functions. Avoid standard `function` keyword declarations.
- **No Inline Comments**: Do not place inline comments inside function bodies, conditionals, or JSX.
- **Single-Line Preceding Function Comment**: Put exactly one clean single-line comment on the line immediately preceding the function.
- **Zero Client Bleed**: Never hardcode tenant-specific keys, credentials, or custom domain checks in core template code.

---

## 7. Documentation Location Policy
- All central operational records, tenant credentials, and VPS access documentation are securely maintained in Google Drive:
  `J:\My Drive\A.EcomWhiteLabel\`
- Never commit private tenant passwords, private SSH keys, or cloud provider tokens into git repositories.
