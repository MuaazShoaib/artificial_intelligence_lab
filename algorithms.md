### Uniform Cost Search
```
# Uniform Cost Search

import math


class Node:
    def __init__(self, state, parent, actions, total_cost):
        self.state = state
        self.parent = parent
        self.actions = actions
        self.total_cost = total_cost


def find_min(frontier):
    min_value = math.inf
    node = ''
    for i in frontier:
        if min_value > frontier[i][1]:
            min_value = frontier[i][1]
            node = i
    return node


def action_sequence(graph, initial_state, goal_state):
    solution = [goal_state]
    current_parent = graph[goal_state].parent
    while current_parent is not None:
        solution.append(current_parent)
        current_parent = graph[current_parent].parent
    solution.reverse()
    return solution


def uniform_cost_search(graph, initial_state, goal_state):
    frontier = dict()
    frontier[initial_state] = (None, 0)
    explored = []

    while len(frontier) != 0:
        current_node = find_min(frontier)
        del frontier[current_node]
        if graph[current_node].state == goal_state:
            return action_sequence(graph, initial_state, goal_state)
        explored.append(current_node)

        for child in graph[current_node].actions:
            current_cost = child[1] + graph[current_node].total_cost
            if child[0] not in frontier and child[0] not in explored:
                graph[child[0]].parent = current_node
                graph[child[0]].total_cost = current_cost
                frontier[child[0]] = (graph[child[0]].parent, graph[child[0]].total_cost)
            elif child[0] in frontier:
                if frontier[child[0]][1] < current_cost:
                    graph[child[0]].parent = frontier[child[0]][0]
                    graph[child[0]].total_cost = frontier[child[0]][1]
                else:
                    frontier[child[0]] = (current_node, current_cost)
                    graph[child[0]].total_cost = frontier[child[0]][1]


INITIAL_STATE = 'C'
GOAL_STATE = 'B'

GRAPH = {
    'A': Node('A', None, [('B', 6), ('C', 9), ('E', 1)], 0),
    'B': Node('B', None, [('A', 6), ('D', 3), ('E', 4)], 0),
    'C': Node('C', None, [('A', 9), ('F', 2), ('G', 3)], 0),
    'D': Node('D', None, [('B', 3), ('E', 5), ('F', 7)], 0),
    'E': Node('E', None, [('A', 1), ('B', 4), ('D', 5), ('F', 6)], 0),
    'F': Node('F', None, [('C', 2), ('E', 6), ('D', 7)], 0),
    'G': Node('G', None, [('C', 3)], 0),
}

solution = uniform_cost_search(graph=GRAPH, initial_state=INITIAL_STATE, goal_state=GOAL_STATE)
print(solution)
```

### Iterative Deepening Search
```
# iterative deepening search

def iterative_deepening_dfs(start, target):
    depth = 1
    bottom_reached = False
    while not bottom_reached:
        result, bottom_reached = iterative_deepening_dfs_rec(start, target, 0, depth)
        if result is not None:
            return result
        depth *= 2
        print("Increasing depth to " + str(depth))
    return None


def iterative_deepening_dfs_rec(node, target, current_depth, max_depth):
    print("Visiting Node " + str(node["value"]))
    if node["value"] == target:
        print("Found the node we are looking for!")
        return node, True
    if current_depth == max_depth:
        print("Current maximum depth reached, returning...")
        if len(node["children"]) > 0:
            return None, False
        else:
            return None, True

    bottom_reached = True
    for i in range(len(node["children"])):
        result, bottom_reached_rec = iterative_deepening_dfs_rec(node["children"][i], target, current_depth + 1,
                                                                 max_depth)

        if result is not None:
            return result, True
        bottom_reached = bottom_reached and bottom_reached_rec
    return None, bottom_reached


start = {
    "value": 0, "children": [
        {"value": 1, "children": [
            {"value": 3, "children": []},
            {"value": 4, "children": []}
        ]}, {
            "value": 2, "children": [
                {"value": 5, "children": []},
                {"value": 6, "children": []}
            ]
        }
    ]
}

print(iterative_deepening_dfs(start, 6)["value"])
```

### Depth First Search
```
# depth first search

class Node:
    def __init__(self, state, parent, actions, total_cost):
        self.state = state
        self.parent = parent
        self.actions = actions
        self.total_cost = total_cost


def depth_first_search(graph, initial_state, goal_state):
    frontier = [initial_state]
    explored = []

    while len(frontier) != 0:
        current_node = frontier.pop(len(frontier) - 1)
        explored.append(current_node)

        for child in graph[current_node].actions:
            if child not in frontier and child not in explored:
                graph[child].parent = current_node
                if graph[child].state == goal_state:
                    print("Explored Node List:", explored)
                    return action_sequence(graph, initial_state, goal_state)
                frontier.append(child)


def action_sequence(graph, initial_state, goal_state):
    solution = [goal_state]
    current_parent = graph[goal_state].parent

    while current_parent is not None:
        solution.append(current_parent)
        current_parent = graph[current_parent].parent
        solution.reverse()
        return solution


GRAPH = {'A': Node('A', None, ['B', 'E', 'C'], None),
         'B': Node('B', None, ['D', 'E', 'A'], None),
         'C': Node('C', None, ['A', 'F', 'G'], None),
         'D': Node('D', None, ['B', 'E'], None),
         'E': Node('E', None, ['A', 'B', 'D'], None),
         'F': Node('F', None, ['C'], None),
         'G': Node('G', None, ['C'], None)}

INITIAL_STATE = "A"
GOAL_STATE = "D"

sol = depth_first_search(graph=GRAPH, initial_state=INITIAL_STATE, goal_state=GOAL_STATE)
print(sol)
```

### Breadth First Search
```
# breadth first search

class Node:
    def __init__(self, state, parent, actions, total_cost):
        self.state = state
        self.parent = parent
        self.actions = actions
        self.total_cost = total_cost


def action_sequence(graph, initial_state, goal_state):
    solution = [goal_state]
    current_parent = graph[goal_state].parent
    while current_parent is not None:
        solution.append(current_parent)
        current_parent = graph[current_parent].parent
    solution.reverse()
    return solution


def breadth_first_search(graph, initial_state, goal_state):
    queue = [initial_state]
    visited = []

    while len(queue) != 0:
        current_node = queue.pop(0)
        visited.append(current_node)
        for child in graph[current_node].actions:
            if child not in queue and child not in visited:
                graph[child].parent = current_node
                if graph[child].state == goal_state:
                    return action_sequence(graph, initial_state, goal_state)
                queue.append(child)


GRAPH = {'A': Node('A', None, ['B', 'C', 'E'], None),
         'B': Node('B', None, ['A', 'D', 'E'], None),
         'C': Node('C', None, ['A', 'F', 'G'], None),
         'D': Node('D', None, ['B', 'E'], None),
         'E': Node('E', None, ['A', 'B', 'D'], None),
         'F': Node('F', None, ['C'], None),
         'G': Node('G', None, ['C'], None)}

INITIAL_STATE = 'F'
GOAL_STATE = 'A'

sol = breadth_first_search(graph=GRAPH, initial_state=INITIAL_STATE, goal_state=GOAL_STATE)
print(sol)
```
