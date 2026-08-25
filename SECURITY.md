# Security policy

    Report vulnerabilities privately to the `led-dynamo` maintainers. Never include secrets, customer data, source payloads, or exploit material in a public issue.

    ## Runtime boundary

    - stdio is the only transport and stdout is the MCP wire;
    - tools are deterministic, read-only, and fail closed on unknown fields or out-of-range numbers;
    - no tool accepts arbitrary URLs, commands, source payloads, credentials, or mutation instructions;
    - readiness exposes presence booleans only;
    - telemetry excludes arguments, results, identities, secrets, and high-cardinality values.

    - The MCP server never publishes, clears, or renders a live device command.
- Device identifiers and message contents are excluded from telemetry.
- Power and capacity planning requires hardware-specific review outside MCP.
