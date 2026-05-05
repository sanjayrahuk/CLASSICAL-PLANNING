## Implementation of Classical Planning Algorithm
### AIM
To implement a Classical Planning Algorithm using search techniques (BFS) to find a sequence of actions hat transforms an initial state into a goal state.
### ALGORITHM
```
1. Define the initial state and goal state.
2. Define the set of actions with their preconditions and effects.
3. Initialize a queue with the initial state and an empty plan.
4. Initialize an empty set for visited states.
5. Repeat until the queue is empty:
   a. Remove the first state from the queue.
   b. If the current state is equal to the goal state:
        return the plan.
   c. Mark the state as visited.
   d. For each action:
        i. Check if preconditions are satisfied.
        ii. Apply the action to generate a new state.
        iii. Add the new state and updated plan to the queue.
6. If no solution is found, return failure.
```
### PROGRAM
```python
# -------- GOAL TEST --------
def is_goal_state(current_state, goal_state):
    return current_state == goal_state


# -------- APPLY ACTION --------
def apply_action(current_state, action_effect):
    new_state = current_state.copy()
    new_state.update(action_effect)
    return new_state


# -------- CHECK PRECONDITION --------
def is_applicable(current_state, precondition):
    return all(current_state.get(k) == v for k, v in precondition.items())


# -------- FIND PLAN (BFS) --------
def find_plan(initial_state, goal_state, actions):
    queue = [(initial_state, [])]   # (state, plan)
    visited = set()

    while queue:
        current_state, plan = queue.pop(0)

        # Goal check
        if is_goal_state(current_state, goal_state):
            return plan

        state_tuple = tuple(current_state.items())
        if state_tuple in visited:
            continue
        visited.add(state_tuple)

        # Try all actions
        for action in actions:
            if is_applicable(current_state, actions[action]['precondition']):
                next_state = apply_action(current_state, actions[action]['effect'])
                queue.append((next_state, plan + [action]))

    return None


# EXAMPLE 1

initial_state1 = {'A': 'Table', 'B': 'Table'}
goal_state1 = {'A': 'B', 'B': 'Table'}

actions1 = {
    'move_A_to_B': {'precondition': {'A': 'Table', 'B': 'Table'}, 'effect': {'A': 'B'}},
    'move_B_to_Table': {'precondition': {'A': 'Table', 'B': 'B'}, 'effect': {'B': 'Table'}}
}

plan1 = find_plan(initial_state1, goal_state1, actions1)
print("Example 1 Output:", plan1)



# EXAMPLE 2
initial_state2 = {'A': 'Table', 'B': 'Table', 'C': 'Table'}
goal_state2 = {'A': 'B', 'B': 'C', 'C': 'Table'}

actions2 = {
    'move_A_to_B': {'precondition': {'A': 'Table', 'B': 'Table'}, 'effect': {'A': 'B'}},
    'move_B_to_C': {'precondition': {'A': 'B', 'B': 'Table', 'C': 'Table'}, 'effect': {'B': 'C'}},
    'move_C_to_Table': {'precondition': {'A': 'B', 'B': 'C', 'C': 'C'}, 'effect': {'C': 'Table'}}
}

plan2 = find_plan(initial_state2, goal_state2, actions2)
print("Example 2 Output:", plan2)


#  EXAMPLE 3 (Goal already reached)

initial_state3 = {'A': 'Table', 'B': 'Table'}
goal_state3 = {'A': 'Table', 'B': 'Table'}

actions3 = {
    'move_A_to_B': {'precondition': {'A': 'Table', 'B': 'Table'}, 'effect': {'A': 'B'}}
}

plan3 = find_plan(initial_state3, goal_state3, actions3)
print("Example 3 Output:", plan3)
```
### OUTPUT
<img width="925" height="126" alt="image" src="https://github.com/user-attachments/assets/b02d3ec7-cebd-429a-940f-b0971570c490" />



### RESULT
Thus, the planning algorithm efficiently finds a sequence of actions to achieve the desired goal state.
