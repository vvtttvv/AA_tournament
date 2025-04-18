
### **Adaptive Retaliator – Iterated Prisoner’s Dilemma Strategy**  

#### **Overview**  
Adaptive Retaliator is a dynamic strategy designed for the Iterated Prisoner's Dilemma tournament. It balances cooperation and retaliation by analyzing the opponent's past behavior and adjusting its responses accordingly.  

#### **How It Works (Round 1)**  
- **Starts with cooperation** to test the opponent.  
- **Retaliates if the opponent defects twice in a row.**  
- **Monitors opponent’s defection rate** – if they defect more than 50% of the time, we also defect.  
- **Uses strategic deception** – after 10 defections, it cooperates briefly to trick exploiters.  
- **Defects in the last round** to maximize points.  

#### **Round 2 Enhancements**  
In the second part of the tournament, Adaptive Retaliator also **chooses who to play with next**, using smart heuristics:  
- **Avoids opponents with whom we've already played 200 rounds.**  
- **Prefers opponents with high cooperation rates.**  
- **If no better choice is available, continues playing with the current opponent.**

#### **Code Implementation (Round 2)**  
```python
def strategy_round_2(opponent_id: int, my_history: dict[int, list[int]], opponents_history: dict[int, list[int]]) -> tuple[int, int]:
    my_moves = my_history.get(opponent_id, [])
    opponent_moves = opponents_history.get(opponent_id, [])
    
    if not opponent_moves:
        move = 1
    else:
        total_rounds = len(opponent_moves)
        opponent_defects = opponent_moves.count(0)
        my_defects = my_moves.count(0)

        if total_rounds >= 2 and opponent_moves[-1] == 0 and opponent_moves[-2] == 0:
            move = 0
        elif opponent_defects / total_rounds > 0.5:
            move = 0
        elif my_defects >= 10 and my_moves[-2:].count(0) < 2:
            move = 1
        elif total_rounds == 199:
            move = 0
        else:
            move = 1

    available_opponents = [pid for pid in opponents_history if len(my_history.get(pid, [])) < 200]
    
    def cooperation_score(pid):
        opp = opponents_history[pid]
        return opp.count(1) / len(opp) if opp else 1.0

    available_opponents.sort(key=cooperation_score, reverse=True)

    next_opponent = available_opponents[0] if available_opponents else opponent_id

    return move, next_opponent
```

#### **Why This Strategy Works**  
✅ **Adaptive** – It changes based on opponent behavior.  
✅ **Unpredictable** – The mix of cooperation and defection prevents easy exploitation.  
✅ **Opponent-selective** – In Round 2, it avoids toxic players and seeks the most beneficial ones.  
✅ **Tournament-ready** – Optimized for long-term scoring and multi-agent dynamics.

#### **Submission Guidelines**  
- Ensure the file is named:  
  **`titerez_vladislav_adaptive_retaliator_round_2.py`**  
- No debugging prints or extra comments should be included in the final version.  
- Submit to the **same GitHub repo as Round 1**, and commit before **April 27, 23:59**.  
- Include this README in the repo to explain your logic.
