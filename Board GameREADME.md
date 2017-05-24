# Winnie-
Board Game
import check

## Board Game: mutate the assigned position in the square board into the desired
##   "X" or "O", and the up and down and adjacent positions are also changed from
##   'X' to 'O' and 'O' to 'X', or space ' ' stays the same

## A Board is a (listof (listof (anyof 'X' 'O' ' ')) 
## requires: 
## * each list in a Board corresponds to a row of the
##   grid, and all lists are the same length,
## * Board is square,
## * Board contains at least one row.
b1x1 = [[' ']]
b2x2 = [['X',' '], [' ','X']]
b4x4 = [['X','O','X','X'], ['O','X','X','O'],   
        ['X','X','O','X'], ['X','X','X','O']]
b5x5 = [['X','O','X','O','X'], ['O','X',' ','X',' '],
       ['O',' ','O',' ','O'], ['X','X',' ','X',' '], 
       ['X','O','X','O','X']]
b6x6 = [['X','O',' ',' ',' ',' '],  ['X','X',' ','O',' ',' '],  
        [' ',' ','O','X','X',' '],  [' ',' ','X',' ',' ',' '],
        [' ',' ',' ',' ','O','X'],  [' ',' ',' ',' ',' ','O']]


# Prompts for input function (note: initialized to work with format)
row_prompt = "Enter row to play (0=top, {0}=bottom): "
column_prompt = "Enter column to play (0=left, {0}=right): "

## dash(s) consumes a str, s, if it's a space,then produces'-'
##     otherwise dosen't change
## dash: Str -> Str

def dash(s):
    if s==' ':
        return '-'
    else:
        return s
    
## mutate_empty(b,posn) consumes a board, b, and a position,posn
##     changes each value in b if it's a space
## Effect: mutates vals
## mutate_empty: Board Int -> None
    
def mutate_empty(b,posn):
    if posn<len(b):
        print (''.join(list(map(dash,b[posn]))))
        mutate_empty(b,posn+1)
    
## turn(b,piece) consumes a board,b and a string, piece
##   mutates the adjacent and up and down string of the piece
## Effects: 
## * reads in two Nat, 
## respectively represents the row and cloumn of a value need to change 
## *mutates vals arround that specific value
## turn: Board Str -> None
## Example:
##turn(b2x2,'X'),read in row 1,column 1=>
##      =>[['X',' '], [' ','X']]

def turn(b, piece):
    mutate_empty(b,0)
    row=int(input(row_prompt.format(len(b)-1)))
    column=int(input(column_prompt.format(len(b[0])-1)))
    b[row][column]=piece
    if len(b)==1: b=b
    if b[row-1][column]=='X':
        b[row-1][column]='O'
    elif b[row-1][column]=='O':
        b[row-1][column]='X'
    
    if  b[row][column-1]=='X':
        b[row][column-1]='O'
    elif b[row][column-1]=='O':
        b[row][column-1]='X'  
    if  len(b)>(row+1):
        if b[row+1][column]=='X':
            b[row+1][column]='O'
        elif b[row+1][column]=='O':
            b[row+1][column]='X'
    if len(b)>(column+1):
        if b[row][column+1]=='X':
            b[row][column+1]='O'
        elif b[row][column+1]=='O':
            b[row][column+1]='X'
    mutate_empty(b,0)
## Test1: b=b4x4,piece='O',read in row 2, column 2
check.set_input(["2","2"])
check.expect("Q1T1",turn(b4x4,'O'),None)
check.expect("QIT1{b4X4}",b4x4,[['X','O','X','X'], ['O','X','O','O'],['X','O','O','O'], ['X','X','O','O']])
## Test2:b=b6x6,piece='X',read in row 3,column 4
check.set_input(["3","4"])
check.expect("Q1T2",turn(b6x6,'X'),None)
check.expect("QIT2{b6X6}",b6x6,[['X','O',' ',' ',' ',' '],  ['X','X',' ','O',' ',' '],  
        [' ',' ','O','X','O',' '],  [' ',' ','X',' ','X',' '],
        [' ',' ',' ',' ','X','X'],  [' ',' ',' ',' ',' ','O']])
## Test3: b=b5x5,piece='X',read in row 1, column 0
check.set_input(["1","0"])
check.expect("Q1T2",turn(b5x5,'X'),None)
check.expect("QIT3{b5X5}",b5x5,[['O','O','X','O','X'], ['X','O',' ','X',' '],
       ['X',' ','O',' ','O'], ['X','X',' ','X',' '], 
       ['X','O','X','O','X']])

