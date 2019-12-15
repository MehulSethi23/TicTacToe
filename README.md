# TicTacToe
# A simple TicTacToe Game

import itertools

from colorama import Fore, Back, Style, init
init()

game = [[0, 0, 0],
        [0, 0, 0],
        [0, 0, 0]]


def win(current_game):

    def all_same(l):
        if l.count(l[0]) == len(l) and l[0] != 0:
            return True
        else:
            return False

    # horizontal
    for row in game:
        print(row)
    if all_same(row):
        print(f"Player {row[0]} is the winner horizontally!")
        return True


    # vertical
    for col in range(len(game[0])):
        check = []
        for row in game:
            check.append(row[col])
        if all_same(check):
              print(f"Player {check[0]} is the winner vertically!")
              return True

    # / diagonal
    diags = []
    for idx, reverse_idx in enumerate(reversed(range(len(game)))):
        diags.append(game[idx][reverse_idx])
    if all_same(diags):
        print(f"Player {diags[0]} has won Diagonally (/)")
        return True

    # \ diagonal
    diags = []
    for ix in range(len(game)):
        diags.append(game[ix][ix])
    if all_same(diags):
        print(f"Player {diags[0]} has won Diagonally (\\)")
        return True


    return False


def game_board(game_map, player=0, row=0, column=0, just_display = False):

    try:
        if game_map[row][column] !=0 :
            print("\n Place Occupied!  \nTry Another Location \n")
            return game_map,False
        print("   " + "   ".join([str(i) for i in range(len(game_map))]))

        if not just_display:
            game_map[row][column] = player
        for count,row in enumerate(game_map):
            colored_row = ""
            for item in row:
                if item == 0:
                    colored_row +="   "+"|"
                elif item == 1:

                    colored_row += Fore.GREEN + ' X ' + Style.RESET_ALL
                    colored_row += "|"


                elif item == 2:

                    colored_row += Fore.LIGHTRED_EX + ' O ' + Style.RESET_ALL
                    colored_row += "|"


            print(count,colored_row)
        return game_map,True


    except Exception as e:
        print("ERROR! , Call SWAGSY now!",e)
        return game_map,False




play = True
players = [1,2]
while play:
    print("Enter Game Size ! ")
    game_size = int(input())
    print(" \n  Player 1 is  X  \n  Player 2 is  0 \n  Good Luck Players !\n")
    game = [[ 0 for i in range(game_size)] for i in range(game_size)]

    game_won = False
    game, _ = game_board(game, just_display=True)
    player_cycle = itertools.cycle([1, 2])

    while not game_won:
        current_player = next(player_cycle)
        print(f"\nPlayer: {current_player}")
        played = False

        while not played:
           column_choice = int(input("What Column do you want to play? (0,1,2): "))
           row_choice = int(input("What Row do you want to play? (0,1,2): "))
           game, played = game_board(game, current_player, row_choice, column_choice)

        if win(game):
            game_won = True
            again = input(" Game Over ! \n Wanna Play Again ? (y/n) ")
            if again.lower() == "y":
                print("Restarting !")
            elif again.lower() == "n":
                print("Bye , Thank You for Playing ! ")
                play = False
            else :
                print(" Sorry , Not a Valid Answer !")
                play = False

