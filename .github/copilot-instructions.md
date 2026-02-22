# Guidance for AI coding agents

Purpose: Help an AI agent be immediately productive in this educational network-lab repository.

- Big picture: This repo is a collection of network lab scenarios and server automation scripts, not a running microservice. Primary artifacts live under `scenarios/*/commands` (Cisco-style `.cfg` files and scenario READMEs) and `servers/` (server scripts and notes). See [README.md](README.md) and [scenarios/TecSol/README.md](scenarios/TecSol/README.md) for canonical examples.

- Primary workflows:
  - There is no build/CI system. Testing and validation are manual using Packet Tracer or by running the provided scripts.
  - To provision example server users and ACLs run the Linux script: `bash servers/linux/linux.sh`.
  - To create the bare `.cfg` files used in exercises, see `scenarios/fankatech/commands/script.sh` which touches the expected config filenames.

- Project conventions and patterns (important):
  - VLAN naming/IDs are explicit in scenario READMEs (e.g., VLANs 10/20/30/50/99 and VLAN 999 used as native in [scenarios/TecSol/README.md](scenarios/TecSol/README.md)).
  - Router-on-a-stick is used: router subinterfaces with `dot1Q` tagging for VLAN gateways (SVIs described in scenario READMEs).
  - File naming pattern for device configs: `Switch-1.cfg`, `Router-Borda.cfg`, `Router-Interno.cfg` (see `scenarios/fankatech/commands`).
  - Scripts use a simple Bash style: shebang, comments, `sudo` where necessary — follow `servers/linux/linux.sh` when adding new scripts.

- Editing guidance (what an AI agent should do and avoid):
  - When modifying Cisco `.cfg` files, preserve CLI structure and command ordering. Do not reformat to JSON/YAML. Example targets: `scenarios/*/commands/*.cfg`.
  - Keep scenario README content authoritative. If you add or change configuration snippets, update the matching scenario README (e.g., [scenarios/TecSol/README.md](scenarios/TecSol/README.md)).
  - Avoid editing author/contact metadata unless asked. This repo is educational — prefer additive changes (new sample configs, extra comments) over destructive edits.
  - For server scripts: keep the same style (comments, echo messages, idempotent where reasonable), and use `apt` before installing packages as shown in `servers/linux/linux.sh`.

- Integration points and assumptions:
  - No external CI or package manifests are present. The main external dependency is the human-run Packet Tracer simulator for network validation.
  - Scripts assume a Debian/Ubuntu environment (uses `apt` and typical Linux utilities).

- Examples of useful edits an AI agent can perform:
  - Add a short, copyable `show` checklist or verification commands to a scenario README (e.g., ping tests and SVI addresses listed in [scenarios/TecSol/README.md](scenarios/TecSol/README.md)).
  - Add small, well-documented config snippets (ACL entries, `interface`/`switchport` examples) into the matching `*.cfg` file.
  - Create helper scripts mirroring `servers/linux/linux.sh` for repeatable local setup tasks.

If anything here is unclear or you'd like more/less detail on a specific area (config conventions, script patterns, or recommended safe edits), tell me which section to expand and I will iterate.
