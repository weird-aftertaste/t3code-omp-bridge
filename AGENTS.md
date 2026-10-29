# Agent instructions: T3 Code ↔ OMP Bridge

The user wants this **standalone experimental OMP RPC bridge** installed on the host where the official T3 Code server runs. Do not fork or rebuild T3 Code.

1. Verify that Node.js >=20 and npm are available, and that `omp --version` works under the T3 Code server's user/environment. Confirm that OMP authentication is configured without printing secrets.
2. Install directly from this small repository, without cloning T3 Code:

   ```sh
   npm install -g "https://github.com/weird-aftertaste/t3code-omp-bridge/archive/refs/heads/main.tar.gz"
   ```

   After an actual npm Registry release, use `npm install -g t3code-omp-bridge`.
3. Verify `omp-t3-bridge --version` (Windows: `omp-t3-bridge.cmd --version`), which should print the same OMP version and exit 0.
4. In T3 Code Settings → Providers, **add** a Pi instance named `Oh My Pi` with binary path `omp-t3-bridge`, enabled, no extra launch args. Leave the regular Pi instance unchanged.
5. For Docker, systemd or remote services, make sure Node.js, this bridge and OMP are installed **inside the T3 server execution environment**, and that PATH and credentials are visible to the service user. Restart only the relevant process when needed.
6. If `omp` is not discovered, configure provider environment variable `OMP_REAL_BINARY` with an absolute native OMP executable path. On Windows, prefer `omp.exe`, not an npm `.cmd` shim.
7. Refresh the Pi provider; if unavailable, gather nonsecret errors and diagnose. Avoid claiming success until an end-to-end interaction works.
8. **Security:** This bridge does not enforce approvals or OS-level sandboxing. Do not enable Full access or write into valuable projects until real OMP permission behavior is validated. Begin with a read-only prompt in a disposable project.
9. The standalone npm bridge does **not** import historical OMP sessions into T3 Code. Do not modify or move OMP session files.

Report installed versions, checks, service environment, provider status, and outstanding issues.
