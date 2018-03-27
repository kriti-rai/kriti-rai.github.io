---
layout: post
title:      "Tic-Tac-Toe with AI"
date:       2018-03-27 05:44:58 +0000
permalink:  tic-tac-toe_with_ai
---

![img](https://i.imgur.com/uodqzo4.png)

While I don't think that I have built the ultimate TicTacToe AI, I did fairly succeed in making my computer know how to make logical moves. To be honest, this part of the lab was challenging-- not the idea of coming up with what moves to make, but the idea of putting that knowledge in code. So I started small. I first made my computer make simple moves. For instance, I hardcoded the block numbers, like "1" if I wanted to make a move to the first box or "2" for the second box and so on. Once I sort of got the CLI working, I looked up the strategies online and started playing around with the logics to make my computer smarter.  Finally, after playing the game numerous times on a piece of paper [(see here)](https://photos.google.com/share/AF1QipPDXFHDUL3HkFMZCBK_zNVrEeMg4M7c5xBbct5VQBTS4zVpgELnihYERR38WWzYTw/photo/AF1QipMuBzvdkBn8Lo7KnUMRf0RkqlGd_8uXKNnHNkYg?key=MW8zUFVBdFB5REtpWUxXRlBGZ25sVnZ6cUpTT2hn) and scribbling down the possible moves, I came up with the strategies which are based on two scenarios--1) where the computer is Player 1 and 2) where the computer is Player 2.

**STRATEGIES**
> As player 1:

>  1. The first move is a `corner_move`, i.e. one of the corners -` "1", "3", "7" or "9"`.
>  2. The second move is an `opposite_corner`. So, if `"1"` was the first move, the second move is `"9"`, if the first move was `"3"`, the  second move is `"7"`, and so on. In case the desired block is taken, the computer then takes up one of the other available opposite corners. For example, if the first move was` "1"` but `"9"` is taken, the computer takes up either `"3"` or `"7"`.
>  3. On its third turn and onward, the computer will try to either make a `win_move`, `defend` or make any other `valid_move`.
 
> As player 2:

> 1. Since we know that as player 1, the computer will always make a `corner_move`, so while playing a computer as player 2, the computer will always make a `center_move`, i.e.` "5"`. However, if `"5"` is taken, then the computer will make a valid `corner_move`.
> 2. On its second turn, the computer will either `defend` or make an `edge_move`, the latter being `"2", "4", "6" or "8"`.
> 3. On its third turn and onward, the computer will try to make a `win_move`, `defend` or make a `valid_move`.

While `corner_move, center_move, edge_move` and `valid_move` were fairly easy to code, it took me some time to code the `win_move` and `defend`. I finally came up with an idea that since we already had access to winning combinations, i.e. `WIN_COMBINATIONS`, it made sense to use it to figure out the `win_move` and `defend` methods.

**WIN_MOVE**

So to make a winning move, the computer would simply iterate over `WIN_COMBINATIONS` and look to see if any of the two indices of a combination are occupied by its token. If that is the case, the computer would then try to place its token in the available index of the combination, therefore, making a winning move. 

**DEFEND**

The same idea goes for defending or blocking move. The computer would iterate over `WIN_COMBINATIONS` and look to see if any of the two indices of a combination are occupied by its opponent's token. If that is the case, the computer would then try to place its token in the available index of the combination, thereby blocking the win for its opponent.

**PUTTING IT ALL TOGETHER**

Lastly, I encapsulated all these possible moves into the main method, i.e. the `move` method. Up until the turn count of 3 the computer will try to create a winning scenario, and then afterwards it will try to make a `win_move`. If that is not possible, or a defense is needed, it will try to `defend`. And, if neither of the former moves are valid or possible it will make a random `valid_move`.

**CONCLUSION**

Finally, I ran some test games and watched the computer versus computer play and also played against the computer. It seems like my AI is working although after a while you might be able to predict the computer's moves so that is an area in which I could improve. Similarly, in the process of writing this blog I went back to the lab and realized I had some unnecessary move methods that I had defined but did not use, and some redundant codes which I could have encapsulated into one, and so I did. I also added `colorize gem` to distinguish between the board status after it has been updated with the player's move.  However, I am not very proud of my bin file as all of my CLI clutter resides there. So, a note to my future-self: *keep your CLI in a separate file and simply call it in the bin file. *
