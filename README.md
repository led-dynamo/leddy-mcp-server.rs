# Leddy MCP Server

    Read-only MCP diagnostics for LED displays, framebuffers, commands, telemetry, and fleet topology. The server is a Rust MCP process over stdio. Stdout is exclusively the JSON-RPC wire; structured diagnostics go to stderr and optional OTLP.

    ## Tools

    - `leddy_fleet_map`
- `leddy_plan`
- `leddy_runtime_readiness`
- `leddy_shared_platform`
- `leddy_lifecycle_state`
- `leddy_safety_boundary`

    Every tool is read-only. Planning accepts a closed workload enum plus bounded numeric fields. The server has no arbitrary URL, command, filesystem, database, GitHub mutation, cluster mutation, or secret-value input.

    ## Product topology

    - `leddy-api-server.rs` — message, display, and telemetry command plane
- `leddy-interfaces` — display, command, message, and telemetry contracts
- `leddy-lib` — framebuffer, font, layout, and scrolling renderer
- `leddy-rasp-pi` — Raspberry Pi display agent
- `leddy-arduino` — Arduino and ESP32 firmware

    ## Security boundary

    - The MCP server never publishes, clears, or renders a live device command.
- Device identifiers and message contents are excluded from telemetry.
- Power and capacity planning requires hardware-specific review outside MCP.

    The shared core is pinned at `c6101656c8227251d1dbd61df54f03a186b42ade`. It provides bounded MCP framing, explicit OTLP/gRPC traces, metrics and logs, JSON stderr diagnostics, redaction, low-cardinality tool metrics, and the formal runtime lifecycle. Each tool also owns an explicit span with `skip_all`; arguments and results are never recorded. Configuration readiness reports environment-variable presence only and performs no authentication or network request.

    This server contains no authenticated HTTP client. If a future tool adds one, it must use fixed or strictly validated HTTP(S) origins, reject credentials/query/fragment/private/metadata targets, disable redirects and ambient proxies, keep credentials in sensitive headers, cap every response, and add adversarial tests before merge.

    ## Shared platform knowledge

    The bounded `shared_platform` tool documents ORE Kubernetes, shared definitions, dpm, Cloudflare/Squarespace, Supabase, and Fiducia without exposing a mutation or credential surface.

    ## Validate

    ```sh
    cargo fmt --all -- --check
    cargo clippy --locked --all-targets --all-features -- -D warnings
    cargo test --locked --all-targets --all-features
    cargo build --locked --release
    cargo audit --deny warnings
    ```
