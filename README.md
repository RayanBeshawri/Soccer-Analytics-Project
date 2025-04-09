# Soccer Analytics Project

A professional soccer analytics engine that processes match data and generates tactical insights with MEOW Score match intensity summaries.

## Overview

This project implements a robust soccer analytics engine that:
- Loads match data (events, players, teams, matches) from CSV files
- Processes the data to identify patterns and key moments
- Generates tactical insights in plain English for each match
- Outputs a "MEOW Score" summarizing match intensity

## Requirements

### Python Packages
- pandas
- numpy
- ast
- torch
- random (standard library)
- dataclasses (Python 3.7+)
- typing (Python 3.5+)
- logging (standard library)
- enum (standard library)

### Data Files (CSV)
- events_World_Cup.csv
- players.csv
- teams.csv
- matches_World_Cup.csv

## Installation & Setup

1. **Install Python Packages**:
   ```bash
   pip install pandas numpy torch
   ```

2. **Data Location**:
   Place the CSV files in the specified directory (e.g., `/kaggle/input/soccer-match-event-dataset/` or modify the path in the code)

## How to Run the Project

Run the script as you would any Python script:

```bash
python soccer_analytics.py
```

This will start the soccer analytics pipeline and print insights and summary (including the "MEOW Score") for each match.

## Code Documentation

### 1. Configuration & Logging

**Logging Setup:**
- Configured at the beginning for INFO-level output

**AnalyticsConfig Dataclass:**
- Holds configurable thresholds and probabilities for event filtering

```python
@dataclass
class AnalyticsConfig:
    long_range_threshold: float = 25.0      # Minimum distance for long-range shots
    exceptional_threshold: float = 35.0      # Distance for exceptional shots
    aggregation_window: int = 15             # Minimum seconds gap between insights
    match_limit: int = 10                    # Number of matches to analyze
    event_probabilities: Dict[str, float] = None
```

The `__post_init__` method sets default filtering probabilities.

### 2. Data Models

**EventTag (IntEnum):**
- Defines various event types (Goal, Miss, Save, etc.) for clarity in the code

**Data Classes:**
- `Player`: Contains player ID, name, role, and team information
- `Team`: Contains team ID and name
- `Position`: Represents a field position (x, y) and provides a function to calculate distance to the goal
- `MatchEvent`: Represents an event in a match (e.g., shot, pass) with properties for insight generation

### 3. SoccerAnalyticsEngine Class

Handles the core functionality:

**Initialization:**
- Sets up data containers and loads the processing device (CPU or GPU)

**Data Loading:**
- `load_data`: Loads CSV files into Pandas DataFrames and processes players, teams, and events
- `_parse_tags`: Safely converts tag data from a string to a list of integers

**Utility Methods:**
- `get_player_name` and `get_team_name`: Return names based on an ID
- `_should_analyze_event`: Uses probabilities to decide whether to output an insight
- `calculate_expected_goals`: Computes a simple xG (expected goals) metric

**Tactical Analysis:**
- `detect_tactical_pattern`: Analyzes recent events to detect patterns such as counter-attacks, possession buildup, or high pressing
- `analyze_match`: Iterates through match events, generating insights for shots, passes, tactical patterns, and disciplinary events
- `run_analysis`: Processes multiple matches and collects insights for each

### 4. Main Execution Function

**run_soccer_analytics:**
- Configures the analytics engine
- Runs the analysis
- Prints insights along with a summary including the custom "MEOW Score"

## Additional Notes

**MEOW Score:**
- A metric computed by summing the number of shot events, tactical patterns, and key passes
- Serves as a quick professional insight indicator

**Customization:**
- Adjust thresholds and probabilities in the `AnalyticsConfig` to tune the sensitivity of insights

## License

[Specify license here]

## Contributors

[Add contributor information here]
