# Python Sudoku Solver

Fast, configurable Sudoku solver in Python. Uses backtracking with pluggable heuristics for **next-cell** and **next-choice** selection. Validates boards, solves from files or stdin, and can print timing and iteration stats.

## Features
- Backtracking engine with heuristic weighting (cell and digit strategies)
- Board validator (rows, columns, 3×3 boxes)
- Works with zeros for blanks
- Optional prefill of single-choice cells
- CLI and importable API
- Colorized interactive input (Windows-friendly via `colorama`)

## Install
```bash
python -m venv .venv && . .venv/bin/activate  # on Windows: .venv\Scripts\activate
pip install -r requirements.txt  # colorama only
```

## Usage

### Solve an example
```bash
python sudoku.py --example
```

### Enter your own puzzle interactively
```bash
python sudoku.py --interactive
```

### Solve from JSON/CSV file or stdin (recommended)
Input format: 9 lines of 9 integers each, zeros for blanks.
```bash
cat puzzle.txt | python sudoku.py --solve --cell-heuristic A --choice-heuristic 0
```

### Heuristics
- **Cell**: `A..L` (e.g., `A` = first empty cell; `E` = fewest choices; `D` = closest to center, etc.)
- **Choice**: `0..9,a` (e.g., `0` = lowest digit; `3` = least-used digit overall)

Example:
```bash
python sudoku.py --solve --cell-heuristic E --choice-heuristic 3 --prefill
```

## Library API
```python
from sudoku import Sudoku

grid = [
  [8,0,0, 0,0,0, 0,0,0],
  [0,0,3, 6,0,0, 0,0,0],
  [0,7,0, 0,9,0, 2,0,0],
  [0,5,0, 0,0,7, 0,0,0],
  [0,0,0, 0,4,5, 7,0,0],
  [0,0,0, 1,0,0, 0,3,0],
  [0,0,1, 0,0,0, 0,6,8],
  [0,0,8, 5,0,0, 0,1,0],
  [0,9,0, 0,0,0, 4,0,0],
]

s = Sudoku(grid)
ok = s.solve(_nextCellMethod="E", _nextChoiceMethod="3", _start_time=0.0, _prefillSingleChoiceCells=True)
assert ok and s.validate()
print(s.grid)
```

## Performance
Backtracking with strong heuristics (`E` + `3`) typically solves hard puzzles in milliseconds–seconds on a modern CPU. Your results will vary by puzzle and heuristic.

## Repo structure
```
.
├── sudoku.py              # Solver + CLI
├── puzzles/               # Sample inputs
├── requirements.txt
└── README.md
```

## License
MIT
