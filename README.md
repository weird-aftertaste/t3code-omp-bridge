# T3 Code ↔ OMP Bridge

Experimental standalone RPC compatibility shim for using [Oh My Pi (OMP)](https://github.com/can1357/oh-my-pi) as an additional **Pi** provider instance in the **official [T3 Code](https://github.com/pingdotgg/t3code)** application.

**No T3 Code fork or build is needed.** This package does not import old OMP sessions into the T3 Code interface.

> **Experimental / safety:** Protocol translation has mock integration tests, but real OMP ↔ T3 Code integration, approval enforcement, MCP injection and session lifecycle have not been fully validated. Do not use Full access or important projects until permissions are confirmed.

## Install

Requires Node.js 20+, npm, and a working authenticated OMP executable **on the same machine/container and under the same user as the T3 Code server**.

### Before npm registry release

Install in one command from this standalone GitHub repository (no manual clone):

```sh
npm install -g "https://github.com/weird-aftertaste/t3code-omp-bridge/archive/refs/heads/main.tar.gz"
```

Or install using Git (requires Git installed):

```sh
npm install -g "github:weird-aftertaste/t3code-omp-bridge"
```

### After npm registry release

```sh
npm install -g t3code-omp-bridge
```

The short registry command works **only after the package has actually been published** to npmjs.com.

Verify:

```sh
omp --version
omp-t3-bridge --version
```

On Windows PowerShell, use `omp-t3-bridge.cmd --version` if needed. Prefer native `omp.exe` over an npm `.cmd` shim for OMP itself.

## Configure official T3 Code

Under **Settings → Providers**, add another **Pi** instance while keeping any existing Pi provider:

- **Display name:** `Oh My Pi`
- **Binary path:** `omp-t3-bridge` (or absolute path / Windows `.cmd` path as necessary)
- **Launch arguments:** leave empty
- **Enabled:** yes

Refresh providers. If OMP is not available on T3's PATH, set an instance environment variable `OMP_REAL_BINARY` to the absolute path of the native OMP executable. Provider credentials come from the ordinary process environment.

The bridge can be used with the **official release of T3 Code**. It does not alter T3 Code files and does not add the separate fork's session-history importer.

## Update

Until npm publication, repeat the GitHub archive installation command. After publication, use:

```sh
npm update -g t3code-omp-bridge
```

## Features

- Runs OMP's JSONL RPC mode behind T3 Code's existing Pi provider.
- Maps `--session` to `--resume`.
- Maps Pi's `get_commands` request to OMP's `get_available_commands`.
- Converts `session_settled` to Pi's `agent_settled`.
- Negotiates RPC v2 and reconstructs chunked responses.
- Forwards plain-text health-check output such as `--version`.

## Develop / publish

```sh
npm test
npm pack --dry-run
npm login
npm publish --access public
```

The publish step requires an npm account with permission to publish the package and any required 2FA confirmation. Check that the `t3code-omp-bridge` registry name is available. This repository being public **does not** mean it is already published on npm.

See [AGENTS.md](./AGENTS.md) for agent-friendly installation guidance.

Licensed MIT.
