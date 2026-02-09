# mlb_frankenstein_player_builder
This project builds customizable “Frankenstein” player profiles by combining the skill sets of multiple MLB players and identifying real players who most closely match that composite archetype.
The goal is to help front offices, analysts, and fans explore player similarity across multiple dimensions such as outcomes, mechanics, and defensive value and to identify both superstar and “budget” alternatives with comparable profiles.

## How It Works

The core functions allow you to construct a hypothetical player by blending specific skill sets from up to three real players.

You can, for example:

  Create a pitcher with:
  
    Outcomes like one player
    
    Command like another
    
    Pitch arsenal like a third

  Or create a hitter who:
  
    Hits like one player
    
    Fields like another
    
    Swings like a third

For each skill set, the model computes cosine similarity between the selected prototype and every other player in the dataset. These similarity scores are then weighted and aggregated into a final composite similarity score.

The output is the top n players whose overall profile most closely matches the Frankenstein player you constructed.

## Pitcher Model

  The pitcher version allows you to combine:

### Outcomes (pitcher_like)

  Includes performance metrics such as:
  
    xERA
    
    Strikeout percentage
    
    Whiff rate
    
    Batted-ball outcomes

### Command (command_like)

  Measures how a pitcher performs within specific Baseball Savant strike zone regions, including:
  
    runs_heart
    
    runs_shadow
    
    runs_chase
    
    runs_waste

### Arsenal (arsenal_like)

  Captures pitch usage and pitch characteristics, including:
  
    Pitch type usage rates
    
    Spin rates
    
    Velocity
  
  Each skill block contributes to the final score using adjustable weights.

#### Default weights:

  Outcomes: 0.55
  
  Batted-ball profile: 0.20
  
  Arsenal: 0.20
  
  Command: 0.05

#### The function:

  Computes similarity within each skill block.
  
  Multiplies each similarity by its weight.
  
  Aggregates all contributions into a final similarity score.
  
  Returns the top n most similar pitchers.

## Hitter Model

  The hitter version allows you to combine:

### Offense / Batted Ball (hit_like)

  Includes:
  
    xwOBA
    
    xOBP
    
    xSLG
    
    Barrel rate
    
    Hard-hit rate
    
    Batted-ball distribution

### Defense (field_like)

  Includes:
  
    Fielding run value
    
    Arm strength (role-normalized)

### Swing Mechanics (swing_like)

  Includes:
  
    Attack angle
    
    Attack direction
    
    Swing tilt
    
    Bat speed

  Each block contributes to the total similarity score via adjustable weights.

#### Default weights:

  Offense: 0.60
  
  Batted-ball profile: 0.20
  
  Swing mechanics: 0.10
  
  Defense: 0.10
  
  If field_like or swing_like is not specified, the function defaults to using the hit_like player for those dimensions.

## Customization Options
  ### Adjustable Weights

    You can adjust how much each skill set contributes to the final similarity score.
    
    This allows you to emphasize:
    
    Pure performance
    
    Mechanics
    
    Defense
    
    Arsenal shape
    
    Command profile

## Minimum Shared Feature Threshold

  Because the dataset contains some missing values, the model allows you to specify a minimum number of shared (non-NA) features required for similarity to be calculated within a block.

## Handedness Filter (Pitchers)

  You can optionally filter results to:
  
    Left-handed pitchers
    
    Right-handed pitchers

## Counterpart Mode (Budget Alternatives)

  The model includes a counterpart filter, designed to identify players with similar profiles who are not elite superstars.
  
  When enabled:
  
    You choose a performance metric (e.g., xERA or xwOBA).
    
    You specify a percentile threshold (e.g., 0.05).
    
    Players above the top percentile cutoff are excluded.
  
  Example:
  
    Setting counterpart_top_pct = 0.05 filters out the top 5% performers, returning players below the 95th percentile for the chosen metric.
    
    This can help identify lower-cost alternatives with similar skill compositions.

## Output

  The function returns the top n most similar players, along with:
  
  Individual skill block contributions
  
  Total similarity score
  
  Visualization tools are included to show how much each skill component contributes to the overall similarity score.

## Use Case

  This framework is designed to support:
  
    Player acquisition and scouting comparisons
    
    Archetype analysis
    
    Contract value exploration
    
    Identifying stylistic player substitutes
    
    Exploring “what if” player constructions
