---
applyTo: "**"
description: Comprehensive workspace setup, development conventions, and project structure for the Social Bingo application.
---

# Workspace Instructions

## 🚀 Development Checklist (Before Any Commit)

- [ ] **Lint**: `uv run ruff check . --fix`
- [ ] **Test**: `uv run pytest` (all tests passing)
- [ ] **Run**: `uv run uvicorn app.main:app --reload --port 8000` (starts without errors)

---

## Project Overview

**Social Bingo** is an interactive web game for in-person mixers. Players find people matching bingo card questions and try to achieve 5 in a row. Built with FastAPI (backend) and Jinja2 + HTMX (frontend).

- **Play**: https://madebygps.github.io/vscode-github-copilot-agent-lab/
- **Lab Guide**: https://madebygps.github.io/vscode-github-copilot-agent-lab/docs/
- **Local Docs**: `/workshop` folder

---

## Setup

### Prerequisites
- Python 3.13+ 
- uv package manager: `curl -LsSf https://astral.sh/uv/install.sh | sh`

### Quick Start
```bash
uv sync
uv run pytest
uv run uvicorn app.main:app --reload --port 8000
```

**Important**: Use a full browser (Chrome, Safari, Firefox) — NOT VS Code Simple Browser. HTMX requires full browser functionality.

---

## Project Structure

```
app/
├── main.py              # FastAPI entry point & routes
├── game_logic.py        # Core bingo mechanics
├── game_service.py      # Game state management
├── models.py            # Pydantic models
├── data.py              # Game questions & themes
├── static/css/app.css   # Utility classes
└── templates/           # Jinja2 templates + HTMX
    ├── base.html
    ├── home.html
    └── components/

tests/
├── test_api.py
└── test_game_logic.py
```

---

## Development Conventions

### Python/Backend
- **Line length**: 88 chars (Ruff enforced)
- **Target**: Python 3.13
- **Linting**: E, F, I, N, W rules
- Keep game logic pure and testable in `game_logic.py`
- Use Pydantic models for validation
- Document complex endpoints with docstrings

### Frontend/Templates
- Use Jinja2 in `app/templates/`
- Reusable components in `components/` subfolder
- HTMX for dynamic interactions: `hx-get`, `hx-post`, `hx-trigger`
- Style with utility classes from `app/static/css/app.css`
- Avoid inline styles
- CSS variables: `--color-accent`, `--color-marked`, `--color-bg-*`
- Refer to `frontend-design.instructions.md` for creative design practices

---

## Testing

### Writing Tests
- Place in `tests/` folder
- Name: `test_<feature>_<scenario>`
- Test happy paths + edge cases
- Aim for >80% coverage on game logic

### Example
```python
def test_bingo_board_marks_correct_cell():
    board = create_test_board()
    board.mark(row=0, col=0)
    assert board.is_marked(row=0, col=0)
```

---

## Git Workflow

### Branch Names
- `feature/description`
- `fix/description`
- `docs/description`
- `refactor/description`

### Commits
- Start with verb: "Add", "Fix", "Update", "Refactor"
- Reference issues: `Fixes #123`
- Keep first line ≤50 characters

---

## Common Tasks

### Add a Game Question
1. Edit `app/data.py`
2. Run `uv run pytest`
3. Test in browser

### Create a Component
1. Create file in `app/templates/components/`
2. Use semantic HTML + utility classes
3. Test HTMX in full browser
4. Add comments

### Modify Game Rules
1. Update `app/game_logic.py`
2. Add/update tests
3. Run full test suite
4. Test manually with edge cases

### Debug
- Browser console for JS errors
- FastAPI docs: http://localhost:8000/docs
- Terminal prints from Python code
- VS Code debugger with breakpoints

---

## Resources

- **FastAPI**: https://fastapi.tiangolo.com/
- **Jinja2**: https://jinja.palletsprojects.com/
- **HTMX**: https://htmx.org/
- **Pytest**: https://docs.pytest.org/
- **Ruff**: https://docs.astral.sh/ruff/
- **Lab Guide**: `/workshop/GUIDE.md`

---

## Troubleshooting

| Issue | Solution |
|-------|----------|
| Server won't start | `rm -rf .venv && uv sync` |
| Tests fail | `uv run pytest -v` |
| HTMX not working | Use full browser (not Simple Browser); check console |
| Port 8000 in use | `uv run uvicorn app.main:app --reload --port 8001` |
