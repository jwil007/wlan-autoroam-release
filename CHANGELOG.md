# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

## [1.0.0] - 2025-10-23

### Changed
- **BREAKING**: `/api/start_roam` now blocks until completion and returns full results (previously returned immediately)
- Replaced all polling architecture with Server-Sent Events (SSE) for real-time updates
- Simplified MCP workflow - `start_roam()` now auto-broadcasts UI updates
- Overhauled prompt engineering rules to remove confusing MANDATORY language
- Simplified path handling - backend now accepts both full paths and basenames

### Added
- `/api/start_roam_stream` endpoint for SSE-based streaming (preferred for UI)
- `/api/ui_events` SSE endpoint for push notifications to connected clients
- `/api/notify_ui` endpoint for MCP tools to push notifications
- `/api/mobility_score` endpoint for mobility scoring
- `/api/user_prefs` endpoint for user preferences
- `/api/chat` MCP-based chat endpoint
- `/api/ai_settings` GET/POST endpoints for LLM configuration
- `/api/ai_test` endpoint for LLM health checks
- `broadcast_ui_event()` function for SSE push notifications
- Interface validation in `/api/start_roam` (validates with `iw dev`)

### Removed
- `wait_for_roam_completion()` MCP tool (polling-based, no longer needed)
- UI notification polling (replaced with SSE push)
- `roam_done.flag` file mechanism
- Deprecated `/server/{filename}` endpoint

### Fixed
- `user_prefs.json` now saved in correct location (user's home directory)
- Fixed double-nested `runs/runs/` directory bug
- Corrected all MCP tool docstrings to reflect new blocking workflow

### Documentation
- Updated `docs/MCP_EXPLAINER.md` with new single-call workflow
- Updated `docs/openapi.yaml` to v1.1.0 with all new endpoints
- Added architecture notes explaining polling elimination

## [1.0.0] - 2025-10-14

### Added
- Initial release
- Web UI for WiFi roaming tests
- CLI tool for automated roaming
- MCP server integration
- Basic API endpoints
- Roam analysis and visualization
