## Technical Overview 
- MatchmakingManager:
1. Handle incoming players
2. Update MatchmakingGames time in queue and ELO Band growth
3. Assign players to existing games OR Create new games to house incoming players
4. Attempt to fill non-full games by cannibalizing Partially Full games by Almost Full games.

- MatchmakingGame
1. Balances teams based on assigned players from MatchmakingManager

- MatchmakingSettings
1. Global Settings from the MatchmakingSettings.ini

## Basic Flow
- Program will process "incoming" players from .json file
- Matchmaking service can choose to Create 1 Pending game and fill that game OR process the whole queue leading to 3 states of games
1. Full
2. Almost Full (capacity default is 75% can be changed in MatchmakingSettings.ini)
3. Partially full
- Balance Full Games (if balanced, team will be printed, you can inspect the output from the console)

## Improvements (Didn't have time to implement)
- Kick back players in Partially Full games into the Queue if past a certain time threshold
- As part of the requirement "Matchmaking system should get better over time in matching people"
1. Multipliers to the KDELO / TeamScoreELO to artificially boost portions of the combined ELO. This could be a result of machine learning data that may have lead to more accurate representation of each of the components of ELO
2. Streak system, streak should act as a modifier on gained / lost ELO from a game
- FOLDER STRUCTURE (my bad, I completely overlooked this)
- As another step of proof of concept, I wanted a simple win/lose "game result" from balanced games to write back into the json file, so you could repeat the process and see people's ELO be adjusted.

## Assumptions made
- This test allows external libraries
1. JSONParser (https://github.com/nlohmann/json)
2. INIReader (https://github.com/benhoyt/inih)
- Invalid user data in json is ignored, eg. values > 32bit, wins/losses < 0
- As a first initialization of the userdata, ELO is introduced and calculated via the formula (1500 + wins - losses) as a default.

## Things not thoroughly tested & may be erronous
- Memory ownership, ideally would like to use std::weak_ptr or an equivalent system of Weak & Strong References for a lot of these dynamically allocated data.

## Thoughts to consider
- I think it might be a better option to leave a portion of players in the queue and not have them all in PartiallyFull games. However I was too committed to the AlmostFull consuming PartiallyFull design and it was too late to go back.
