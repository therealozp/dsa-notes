
```python
white = 1
black = 2
empty = 0

dirs = [(-1, -1), (-1, 0),  (-1, 1), (0, -1), (0, 1), (1, -1), (1, 0), (1, 1)]
# in the case of infinite grid size
board = defaultdict(lambda: defaultdict(int))
# print(board[1][1])

def play(board, move, row, column): 
    board[row][column] = move

def check(board, move, row[[]()](), column):
    scores = [0] * len(dirs)
    for diri in range(len(dirs)): 
        x, y = dirs[diri]
        i, j = row + x, column + y
        while (board[i][j] == move and i > -1 and j > -1 and scores[diri] < 5):
            print('visiting', i, j)
            scores[diri] += 1
            i += x
            j += y
    
    print(scores)

    for i in range(len(scores) // 2): 
        if scores[i] + scores[len(scores) - i - 1] >= 5: 
            return True
    else: 
        return False
```
