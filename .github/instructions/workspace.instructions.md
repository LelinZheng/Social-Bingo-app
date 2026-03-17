---
applyTo: "**"
description: Comprehensive workspace setup, development conventions, and project structure for the Social Bingo application.
---

# Workspace Instructions

## Project Overview

**Social Bingo** is an interactive web game for in-person mixers. Players find people matching bingo card questions and try to achieve 5 in a row. The application is built with FastAPI (backend) and Jinja2 + HTMX (frontend).

### Quick Links
- **Play**: https://madebygps.github.io/vscode-github-copilot-agent-lab/
- **Lab Guide**: https://madebygps.github.io/vscode-github-copilot-agent-lab/docs/
- **Local Docs**: See `/workshop` folder for offline guides

---

## Environment Setup

### Prerequisites
- Python 3.13+ (verify: `python --version`)
- uv package manager (install: `curl -LsSf https://astral.sh/uv/install.sh | sh`)

### Initial Setup
```bash
# Sync dependencies
uv sync

# Run tests (should pass)
uv run pytest

# Start dev server (http://localhost:8000)
uv run uvicorn app.main:app --reload --port 8000
```

**Important**: Do NOT use VS Code Simple Browser. Always use an external browser (Chrome, Safari, Firefox) as HTMX requires full browser functionality.

---

## Project Structure

```
app/
├── main.py              # FastAPI application entry point
├── models.py            # Pydantic data models
├── game_logic.py        # Core bingo game mechanics
├── game_service.py      # Game state management
├── data.py              # Static game data (questions, themes)
├── static/
│   ├── css/app.css      # Custom utility CSS classes
│   └── js/htmx.min.js   # HTMX library
└── templates/
    ├── base.html        # Main layout wrapper
    ├── home.html        # Home page
    └── components/      # Reusable HTML components
        ├── bingo_board.html
        ├── bingo_modal.html
        ├── game_screen.html
        └── start_screen.html

tests/
├── test_api.py          # API endpoint tests
└── test_game_logic.py   # Game logic unit tests

docs/                    # Static documentation site
workshop/               # Lab guide markdown files
```

---

## Development Commands

### Running the Application
```bash
# Development server with auto-reload
uv run uvicorn app.main:app --reload --port 8000

# Using VS Code task (Cmd+Shift+B)
Task: "python: run"
```

### Testing
```bash
# Run all tests
uv run pytest

# Run specific test file
uv run pytest tests/test_game_logic.py

# Run with verbose output
uv run pytest -v

# Using VS Code task
Task: "python: test"
```

### Code Quality
```bash
# Lint code (Ruff)
uv run ruff check .

# Auto-fix linting issues
uv run ruff check . --fix

# Using VS Code task
Task: "python: lint"
```

---

## Development Conventions

### Python/Backend

#### Code Style
- **Line length**: 88 characters (enforced by Ruff)
- **Target version**: Python 3.13
- **Linting**: Ruff checks E (errors), F (pyflakes), I (imports), N (naming), W (warnings)

#### FastAPI Routes
- Place route handlers in `app/main.py`
- Return appropriate HTTP status codes
- Use Pydantic models for request/response validation
- Document complex endpoints with docstrings

#### Game Logic
- Core mechanics live in `app/game_logic.py`
- Keep logic pure and testable (minimal side effects)
- Game state management in `app/game_service.py`
- Add comprehensive tests for new game rules

### Frontend/Templates

#### HTML Structure
- Use Jinja2 templates in `app/templates/`
- Organize reusable components in `components/` subfolder
- Use `base.html` as the main layout template
- Keep components modular and focused on a single responsibility

#### HTMX Usage
- Use HTMX for dynamic interactions without page reloads
- Leverage `hx-get`, `hx-post` for server communication
- Use `hx-trigger` for custom events
- Always test in full browsers (Firefox, Chrome, Safari)

#### Styling
- Use custom CSS utility classes from `app/static/css/app.css`
- Follow the Tailwind-like naming convention (.flex, .p-4, .bg-accent, etc.)
- See `css-utilities.instructions.md` for available utilities
- Avoid inline styles; use utility classes instead
- CSS variables for theme colors:
  - `--color-accent`: Primary blue
  - `--color-marked`: Green (selected state)
  - `--color-bg-*`: Background colors

#### Design Philosophy
- See `frontend-design.instructions.md` for creative design practices
- Avoid generic "AI aesthetics" — make distinctive choices
- Commit to cohesive color themes and typography
- Use animations sparingly but effectively
- Create atmosphere through gradients and contextual effects

---

## Testing Guidelines

### Writing Tests
- Place tests in `tests/` folder
- Use pytest framework
- Name test functions: `test_<feature>_<scenario>`
- Test both happy paths and edge cases
- Mock external dependencies when needed

### Example Test Structure
```python
def test_bingo_board_marks_correct_cell():
    # Setup
    board = create_test_board()
    
    # Action
    board.mark(row=0, col=0)
    
    # Assert
    assert board.is_marked(row=0, col=0)
```

### Coverage Expectations
- Aim for >80% coverage on critical game logic
- Test API endpoints with `TestClient` from `httpx`
- Include integration tests for user workflows

---

## Working with Git

### Branch Naming
- `feature/description` for new features
- `fix/description` for bug fixes
- `docs/description` for documentation
- `refactor/description` for code improvements

### Commit Messages
- Start with verb: "Add", "Fix", "Update", "Refactor"
- Reference issue numbers if applicable: `Fixes #123`
- Keep first line ≤50 characters

### Pull Requests
- Include description of changes
- Reference related issues
- Link to relevant documentation
- Ensure all tests pass before requesting review

---

## Common Tasks

### Adding a New Game Question
1. Edit `app/data.py` to add question to appropriate theme
2. Run tests to ensure data integrity
3. Test in browser to verify UI displays correctly

### Creating a New Component
1. Create file in `app/templates/components/`
2. Use semantic HTML with utility classes
3. Test HTMX interactions in full browser
4. Document component purpose in comments

### Modifying Game Rules
1. Update logic in `app/game_logic.py`
2. Add/update corresponding tests
3. Run full test suite: `uv run pytest`
4. Test manually in browser with edge cases

### Debugging
- Check browser console for JavaScript errors
- Use FastAPI docs at http://localhost:8000/docs for API testing
- Add `print()` statements in Python (appear in terminal)
- Use VS Code debugger with breakpoints if needed

---

## Performance Considerations

- Keep game state minimal to reduce payload size
- Use HTMX swaps efficiently (only update changed elements)
- Cache static assets (CSS, JS, images)
- Minimize template rendering overhead
- Profile with browser DevTools if performance issues arise

---

## Additional Resources

- **FastAPI Docs**: https://fastapi.tiangolo.com/
- **Jinja2 Docs**: https://jinja.palletsprojects.com/
- **HTMX Docs**: https://htmx.org/
- **Pytest Docs**: https://docs.pytest.org/
- **Ruff Linter**: https://docs.astral.sh/ruff/
- **Lab Guide**: `/workshop/GUIDE.md` (detailed project walkthrough)

---

## Troubleshooting

### Server won't start
```bash
# Clear cache and reinstall
rm -rf .venv
uv sync
uv run uvicorn app.main:app --reload --port 8000
```

### Tests fail after changes
```bash
# Run with verbose output to see errors
uv run pytest -v
```

### HTMX not working
- Ensure you're using a full browser (not Simple Browser)
- Check browser console for JavaScript errors
- Verify server is running (check port 8000)
- Use browser DevTools Network tab to inspect requests

### Port 8000 already in use
```bash
# Use a different port
uv run uvicorn app.main:app --reload --port 8001
```

---

## Questions or Issues?

Refer to:
- Project documentation in `/docs` and `/workshop`
- GitHub issues for known problems
- Lab guide for step-by-step guidance
