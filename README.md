
### **Adaptive Retaliator – Iterated Prisoner’s Dilemma Strategy**  

#### **Overview**  
Adaptive Retaliator is a dynamic strategy designed for the Iterated Prisoner's Dilemma tournament. It balances cooperation and retaliation by analyzing the opponent's past behavior and adjusting its responses accordingly.  

#### **How It Works**  
- **Starts with cooperation** to test the opponent.  
- **Retaliates if the opponent defects twice in a row.**  
- **Monitors opponent’s defection rate** – if they defect more than 50% of the time, we also defect.  
- **Uses strategic deception** – after 10 defections, it cooperates briefly to trick exploiters.  
- **Defects in the last round** to maximize points.  

#### **Code Implementation**  
```python
def strategy(my_history: list[int], opponent_history: list[int], rounds: int | None) -> int:
    if not opponent_history:  
        return 1  # Cooperate first move

    total_rounds = len(opponent_history)
    opponent_defects = opponent_history.count(0)
    my_defects = my_history.count(0)

    # If the opponent defected in the last two rounds, retaliate
    if total_rounds >= 2 and opponent_history[-1] == 0 and opponent_history[-2] == 0:
        return 0  

    # If the opponent defects more than 50% of rounds, increase defections
    if opponent_defects / total_rounds > 0.5:
        return 0  

    # If we have defected 10 times, briefly cooperate to reset expectations
    if my_defects >= 10 and my_history[-2:].count(0) < 2:
        return 1  

    # If it's the last round, defect for maximum gain
    if rounds is not None and total_rounds == rounds - 1:
        return 0  

    return 1  # Default to cooperation
```

#### **Why This Strategy Works**  
✅ **Adaptive** – It changes based on opponent behavior.  
✅ **Unpredictable** – The mix of cooperation and defection prevents easy exploitation.  
✅ **Strategically aggressive** – Punishes frequent defectors while rewarding cooperators.  
✅ **Tournament-ready** – Optimized for long-term scoring, not just short-term revenge.  

#### **Submission Guidelines**  
- Ensure the file is named **adaptive_retaliator.py** before submission.  
- No debugging prints or extra comments should be included in the final version.  
- Test thoroughly before submitting!  
