## EX NO:06
## DATE:07.06.2022
# <p align="center">Sudoku

## Aim:
To develop a code to solve a sudoku puzzle using contraint propagation

## Theory:
Sudoku consists of a 9x9 grid, and the objective is to fill the grid with digits in such a way that each row, each column, and each of the 9 principal 3x3 subsquares contains all of the digits from 1 to 9.

## Sudoku puzzle:

![sudoku](https://user-images.githubusercontent.com/75234588/173184334-8ca9ed22-748a-41e2-acaf-75b591627378.jpg)




## Design Steps:
### Step 1:
Take an unsolved sudoku puzzle and make it has a single string format.
### Step 2:
Convert given string format into dictionary format.
### Step 3:
Eliminate possible values for a box by looking at its peers.
### Step 4:
Check whether any box which allows only a certain digit in the unit after elimination.
### Step 5:
Repeat 3 and 4 until we get the solved puzzle.
### Step 6:
Calculate the time taken to solve the sudoku.

## Program:
```
Developed by: Vincent isaac jeyaraj J
Register  No: 212220230060

```
```python

%matplotlib inline
import matplotlib.pyplot as plt
import random
import math
import sys
puzzle='...8.....789.1...6.....61....7....5.5.87.93.4.4....2....32.....8...7.439.....1...'
rows = 'ABCDEFGHI'
cols = '123456789'

def cross(a,b):
    return [s+t for s in a for t in b]
boxes= cross(rows, cols)
row_units = [cross(r,cols) for r in rows]
column_units = [cross(rows,c) for c in cols]
square_units = [cross(rs,cs) for rs in ('ABC','DEF','GHI') for cs in ('123','456','789')]

unitlist = row_units + column_units + square_units
units = dict((s,[u for u in unitlist if s in u]) for s in boxes)
peers = dict((s,set(sum(units[s],[]))-set([s])) for s in boxes)
def display(values):
    width=1+max(len(values[s])for s in boxes)
    line = '+'.join(['-'*(width*3)]*3)
    for r in rows:
        print(''.join(values[r+c].center(width)+('|'if c in '36' else '')for c in cols))
        if r in 'CF':print(line)
    return
def elimination(values):
    solved_values = [box for box in values.keys() if len(values[box])==1]
    for box in solved_values:
        digit = values[box]
        for peer in peers[box]:
            values[peer] = values[peer].replace(digit,'')
    return values
def only_choice(values):
    for unit in unitlist:
        for digit in '123456789':
            dplaces = [box for box in unit if digit in values[box]]
            if len(dplaces) == 1:
                values[dplaces[0]] = digit
    return values
def grid_values(grid):
    assert len(grid) == 81, "Input grid must be a string length of 81 (9x9)"
    boxes = cross(rows,cols)
    return dict(zip(boxes,grid))

def grid_values_improved(grid):
    values = []
    all_digits = '123456789'
    for c in grid:
        if c == '.':
            values.append(all_digits)
        elif c in all_digits:
                values.append(c)
    assert len(values) == 81
    boxes = cross(rows,cols)
    return dict(zip(boxes,values))
def reduce_puzzle(values):
    stalled =False
    while not stalled:
        solved_values_before = len([box for box in values.keys() if len(values[box])==1])
        elimination(values)
        only_choice(values)
        solved_values_after = len([box for box in values.keys() if len(values[box])==1])
        stalled = solved_values_after == solved_values_before
        if len([box for box in values.keys() if len(values[box])==1])==0:
            return False
    return values
def search(values):
    values_reduced = reduce_puzzle(values)
    if not values_reduced:
        return False
    else:
        values=values_reduced
    if len([box for box in values.keys() if len(values[box])==1])==81:
        return values
    
    possibility_count_list = [(len(values[box]),box) for box in values.keys() if len(values[box])>1]
    
    possibility_count_list.sort()
    for(_,t_box_min) in possibility_count_list:
        for i_digit in values[t_box_min]:
            new_values = values.copy()
            new_values[t_box_min]=i_digit
            new_values = search(new_values)
            if new_values:
                return new_values
            
    return values
p1=grid_values_improved(puzzle)
result = search(p1)
display(result)
```

## Output:
![Capture68](https://user-images.githubusercontent.com/75234588/173184203-a8166d05-ee6a-48b8-905a-d7efa376b161.PNG)



## Result:
Thus, a program to solve sudoku puzzle using constraint propagation is implemented successfully.
