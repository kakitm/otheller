# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Development Setup

### Python Environment
- Uses `uv` for dependency management (preferred) or standard `pip` with `requirements.lock`
- Python 3.12+ required
- Install dependencies: `uv sync` or `pip install -r requirements.lock`

### Web Application
- Flask application entry point: `flask --app otheller run`
- Access at: `http://127.0.0.1:5000`
- Static files in `otheller/static/` (JS/CSS)
- Templates in `otheller/templates/`

## Development Commands

### Python
- **Lint**: `ruff check` (configured in `ruff.toml`)
- **Format**: `ruff format`
- **Type check**: `mypy otheller/`
- **Fix linting issues**: `ruff check --fix`

### JavaScript
- **Lint**: `npm run lint` or `pnpm lint`
- **Format**: `npm run format` or `pnpm format`
- Package manager: `pnpm` (preferred, v10.13.1)

## Architecture Overview

### Core Game Engine (`otheller/core/`)
- **Facade Pattern**: Primary interface through `Board` class in `board.py`
- **State Management**: `BoardState` handles 8x8 grid state
- **Game Logic**: `GameManager` controls turn flow and stone placement
- **Move Validation**: `Placement` validates legal moves and flipping
- **Simulation**: `GameSimulator` provides move preview capabilities

### Service Layer (`otheller/service/`)
- **GameController**: Main orchestrator for web API interactions
- **State Persistence**: JSON-based game state saving/loading
- **Strategy Management**: Dynamic loading of uploaded Python strategies

### Strategy System
- Base class: `StrategyBase` in `examples/base.py`
- Custom strategies must implement `choose_move()` method
- Class name must be `MyStrategy` for dynamic loading
- Uploaded files executed in sandboxed environment (uploads folder)

### Web Interface
- **Frontend**: Vanilla JavaScript with modular architecture
  - `gameOrchestrator.js`: Main game flow controller
  - `boardRenderer.js`: Visual board representation
  - `apiClient.js`: Server communication
  - `playerManager.js`: Strategy file handling
- **Backend**: Flask REST API with JSON responses

## File Structure Context
- `examples/`: Strategy implementation templates and samples
- `uploads/`: Runtime storage for uploaded strategy files and game state
- `otheller/`: Main application package
  - `core/`: Game engine (pure logic, no I/O)
  - `service/`: Web service layer
  - `static/js/`: Frontend JavaScript modules
  - `app.py`: Flask application and API endpoints

## Key Design Patterns
- **Strategy Pattern**: Pluggable AI implementations via `StrategyBase`
- **Facade Pattern**: Simplified `Board` interface for external consumers
- **Controller Pattern**: `GameController` manages game lifecycle
- **Module Pattern**: Frontend organized into focused JavaScript modules

## Security Notes
- Strategy files are executed dynamically without sandboxing
- Application designed for local development only
- File uploads stored in `uploads/` directory
- No authentication or access controls implemented