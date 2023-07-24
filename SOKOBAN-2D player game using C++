#include <conio.h>
#include <iostream>
#include <queue>
#include <vector>
using namespace std;
#define mapSize 40

class Game;
class Map;
class Player;
class Box;
class Placeholder;

class Player
{
public:
    string top, bottom;
    int x, y;
    int left, right, up, down;

    Player(int xcor, int ycor)
    {
        //Current position of player
        x = xcor;
        y = ycor;

        // This is how the player looks
        top = "/\\";
        bottom = "\\/";

        // ASCII values of arrow keys
        left = 75;
        right = 77;
        up = 72;
        down = 80;
    }
};

class Box
{
public:
    //Skins of the box
    string placedSkin = "XX";
    string unplacedSkin = "OO";

    int x, y, placed = 0;

    Box(int x, int y)
    {
        this->x = x;
        this->y = y;
    }

    //If the box is placed or not placed then change the skin accordingly
    string skin()
    {
        return (placed) ? placedSkin : unplacedSkin;
    }
};

class Placeholder
{
public:
    int x, y;

    Placeholder(int x, int y)
    {
        this->x = x;
        this->y = y;
    }
};

class Map
{
private:
    int solved = 0;

    //Number of boxes and placeholders
    int boxCount = 0;
    int plhCount = 0;

    //Information about the boxes, their positions and their skins
    Box *boxes[10];
    //Information about the placeholders and their positions
    Placeholder *placeholders[10];

public:

    //Checks if all the boxes are placed int the map   
    int checkAllBoxPlaced()
    {
        for (int i = 0; i < boxCount; i++)
            if (boxes[i]->placed == 0)
                return 0;
        return 1;
    }

    //Update the loaction of player according to the direction  
    void resetAndUpdatePlayer(Player *player, string dir)
    {
        if(dir=="left")
        {
            map[player->x][player->y] = "  ";
            map[player->x + 1][player->y] = "  ";
            player->y -= 1;
        }
        else if(dir=="right")
        {
            map[player->x][player->y] = "  ";
            map[player->x + 1][player->y] = "  ";
            player->y += 1;
        }
        else if(dir=="up")
        {
            map[player->x][player->y] = "  ";
            map[player->x + 1][player->y] = "  ";
            player->x -= 2;
        }
        else if(dir=="down")
        {
            map[player->x][player->y] = "  ";
            map[player->x + 1][player->y] = "  ";
            player->x += 2;
        }
    }
    
    //----------------------------------------------------------------
    // 1. Check whether collision occured 
    // 2. If not collided with box or placeholders then update player position
    // 3. Injest placeholders position in the map
    // 4. Inject player position in the map
    // 5. Inject boxes positions in the map
    // 6. Finally display the map 
    void moveUp(Player *player, int rows, int cols)
    {
        if ((!(checkCollision(player->x - 2, player->y, "up"))) && ((!(checkCollision(player->x - 1, player->y, "up")))))
        {
            resetAndUpdatePlayer(player,"up");
            injestPlaceholders();
            injestPlayer(player, player->x, player->y);
            injestBoxes();
            render(rows, cols);
        }
    }

    void moveDown(Player *player, int rows, int cols)
    {
        if (!(checkCollision(player->x + 2, player->y, "down")))
        {
            resetAndUpdatePlayer(player,"down");
            injestPlaceholders();
            injestPlayer(player, player->x, player->y);
            injestBoxes();
            render(rows, cols);
        }
    }

    void moveLeft(Player *player, int rows, int cols)
    {
        if (!(checkCollision(player->x, player->y - 1, "left")))
        {
            resetAndUpdatePlayer(player,"left");
            injestPlaceholders();
            injestPlayer(player, player->x, player->y);
            injestBoxes();
            render(rows, cols);
        }
    }

    void moveRight(Player *player, int rows, int cols)
    {
        if (!(checkCollision(player->x, player->y + 1, "right")))
        {
             resetAndUpdatePlayer(player,"right");
            injestPlaceholders();
            injestPlayer(player, player->x, player->y);
            injestBoxes();

            render(rows, cols);
        }
    }
    //----------------------------------------------------------------

    // Run the function of left, right, up and down according to the user input
    void movement(int key, Player *player, int rows, int cols)
    {
        switch (key)
        {
        case 75:
            moveLeft(player, rows, cols);
            break;
        case 77:
            moveRight(player, rows, cols);
            break;
        case 72:
            moveUp(player, rows, cols);
            break;
        case 80:
            moveDown(player, rows, cols);
            break;
        }
    }

    // Check whether the player collides with game objects and move/stay accordingly
    int checkCollision(int f_xcor, int f_ycor, string dir)
    {
        if (
            map[f_xcor][f_ycor] == "OO" ||
            map[f_xcor][f_ycor] == "XX")
        {
            if (dir == "up")
            {
                if (
                    (map[f_xcor - 2][f_ycor] == "++" ||
                     map[f_xcor - 2][f_ycor] == "  ") &&
                    (map[f_xcor - 1][f_ycor] != "WW"))
                {
                    for (int i = 0; i < boxCount; i++)
                    {
                        if (boxes[i]->x == f_xcor && boxes[i]->y == f_ycor)
                        {
                            boxes[i]->x -= 2;
                            if (map[f_xcor - 1][f_ycor] == "++")
                                boxes[i]->placed = 1;
                            else
                                boxes[i]->placed = 0;
                        }
                    }
                    return 0;
                }
                else
                {
                    return 1;
                }
            }
            else if (dir == "down")
            {
                if (
                    (map[f_xcor + 2][f_ycor] == "++" ||
                     map[f_xcor + 2][f_ycor] == "  ") &&
                    (map[f_xcor + 1][f_ycor] != "WW"))
                {

                    for (int i = 0; i < boxCount; i++)
                    {
                        if (boxes[i]->x == f_xcor && boxes[i]->y == f_ycor)
                        {
                            boxes[i]->x += 2;
                            if (map[f_xcor + 2][f_ycor] == "++")
                                boxes[i]->placed = 1;
                            else
                                boxes[i]->placed = 0;
                        }
                    }
                    return 0;
                }
                else
                {
                    return 1;
                }
            }
            else if (dir == "right")
            {
                if (
                    (map[f_xcor][f_ycor + 1] == "++" ||
                     map[f_xcor][f_ycor + 1] == "  ") &&
                    (map[f_xcor][f_ycor + 1] != "WW"))
                {

                    for (int i = 0; i < boxCount; i++)
                    {
                        if (boxes[i]->x == f_xcor && boxes[i]->y == f_ycor)
                        {
                            boxes[i]->y += 1;
                            if (map[f_xcor][f_ycor + 1] == "++")
                                boxes[i]->placed = 1;
                            else
                                boxes[i]->placed = 0;
                        }
                    }
                    return 0;
                }
                else
                {
                    return 1;
                }
            }
            else if (dir == "left")
            {
                if (
                    (map[f_xcor][f_ycor - 1] == "++" ||
                     map[f_xcor][f_ycor - 1] == "  ") &&
                    (map[f_xcor][f_ycor - 1] != "WW"))
                {

                    for (int i = 0; i < boxCount; i++)
                    {
                        if ((boxes[i]->x == f_xcor && boxes[i]->y == f_ycor))
                        {
                            boxes[i]->y -= 1;
                            if (map[f_xcor][f_ycor - 1] == "++")
                            {
                                boxes[i]->placed = 1;
                            }
                            else
                                boxes[i]->placed = 0;
                        }
                    }
                    return 0;
                }
                else
                {
                    return 1;
                }
            }
        }
        else if ((map[f_xcor][f_ycor] == "  " || map[f_xcor][f_ycor] == "++"))
            return 0;
        else
            return 1;
    }

    string map[mapSize][mapSize];

    Map(string _map[mapSize][mapSize], int rows, int cols)
    {
        for (int i = 0; i < rows; i++)
            for (int j = 0; j < cols; j++)
                if (_map[i][j] == "OO")
                {
                    Box *box = new Box(i, j);
                    boxes[boxCount++] = box;
                }
                else if (_map[i][j] == "++")
                {
                    Placeholder *place = new Placeholder(i, j);
                    placeholders[plhCount++] = place;
                }
                else
                    map[i][j] = _map[i][j];
    }

    //Display the final map
    void render(int rows, int cols)
    {
       cout << "\033[2J\033[1;1H";
        for (int i = 0; i < rows; i++)
        {
            for (int j = 0; j < cols; j++)
            {
                if (map[i][j] == "WW")
                   cout << "\033[1;4;44m  \033[0m";
                else if (map[i][j] == "/\\")
                    cout << "\033[1;31m/\\\033[0m";
                else if (map[i][j] == "\\/")
                    cout << "\033[1;31m\\/\033[0m";
                else if (map[i][j] == "OO" || map[i][j] == "OO ")
                    cout << "\033[1;33mOO\033[0m";
                else if (map[i][j] == "XX")
                    cout << "\033[1;35mXX\033[0m";
                else if (map[i][j] == "++" || map[i][j] == " ++ ")
                    cout << "\033[1;32m++\033[0m";
                else if(map[i][j] == "  ")
                    cout << "\033[1;m  \033[0m";
                else if(map[i][j] == "SOKOBAN ")
                    cout<< "\033[1;41mSOKOBAN \033[0m";
                else if(map[i][j] == "GET INTO THE OLD WORLD OF GAME WITH SOKOBAN ")
                    cout<< "\033[1;35mGET INTO THE OLD WORLD OF GAME WITH SOKOBAN \033[0m";
                else if(map[i][j] == "HERE ARE THE RULES HOW TO PLAY:-- ")
                    cout<< "\033[1;35mHERE ARE THE RULES HOW TO PLAY:-- \033[0m";
                else if(map[i][j] == "1.THE PLAYER NEEDS TO MOVE THE BOX TO PLACE IT ON THE PLACE HOLDER. ")
                    cout<< "\033[1;35m1.THE PLAYER NEEDS TO MOVE THE BOX TO PLACE IT ON THE PLACE HOLDER. \033[0m";
                else if(map[i][j] == "2.USE THE ARROW KEYS TO MOVE THE PLAYER TO PLACE IT ")
                    cout<< "\033[1;35m2.USE THE ARROW KEYS TO MOVE THE PLAYER TO PLACE IT \033[0m";
                else if(map[i][j] == "UP , DOWN , LEFT , RIIGHT.")
                    cout<< "\033[1;35mUP , DOWN , LEFT , RIIGHT.\033[0m";
                else if(map[i][j] =="3.ONCE ALL THE BOXES ARE PLACED THE PLAYER IS MOVED  NEW LEVEL. ")
                    cout<< "\033[1;35m3.ONCE ALL THE BOXES ARE PLACED THE PLAYER IS MOVED  NEW LEVEL. \033[0m";
                else if(map[i][j] =="4.IF THE PLAYER IS STUCK TEHY CAN RESPAWN THE LEVEL BY PRESSING KEY R.")
                    cout<< "\033[1;35m4.IF THE PLAYER IS STUCK TEHY CAN RESPAWN THE LEVEL BY PRESSING KEY R.\033[0m";
                else cout <<map[i][j];
            }
            cout << endl;

            // for (int j = 0; j < cols; j++)
            // {
            //     if (map[i][j] == "WW")
            //         cout << "\033[1;4;7;34m  \033[0m";
            //     else if (map[i][j] == "/\\")
            //         cout << "\033[1;31m/\\\033[0m";
            //     else if (map[i][j] == "\\/")
            //         cout << "\033[1;31m\\/\033[0m";
            //     else if (map[i][j] == "OO")
            //         cout << "\033[1;33mOO\033[0m";
            //     else if (map[i][j] == "XX")
            //         cout << "\033[1;36mXX\033[0m";
            //     else if (map[i][j] == "++")
            //         cout << "\033[1;32m++\033[0m";
            //     else
            //         cout << "\033[1;m  \033[0m";
            // }
            // cout << endl;

            // cout << "\033[2J\033[1;1H";
            // for (int i = 0; i < rows; i++)
            // {
            //     for (int j = 0; j < cols; j++)
            //         cout << map[i][j];
            //     cout << endl;
            // }
        }
    }
    
    //----------------------------------------------------------------
    // Injest the information of the player boxes and placesholders in the map matrix
    void injestPlayer(Player *player, int f_x, int f_y)
    {
        map[f_x][f_y] = player->top;
        map[f_x + 1][f_y] = player->bottom;
    }

    void injestBoxes()
    {
        for (int i = 0; i < boxCount; i++)
        {
            map[boxes[i]->x][boxes[i]->y] = boxes[i]->skin();
            map[boxes[i]->x + 1][boxes[i]->y] = boxes[i]->skin();
        }
    }

    void injestPlaceholders()

    {
        for (int i = 0; i < plhCount; i++)
        {
            map[placeholders[i]->x][placeholders[i]->y] = "++";
            map[placeholders[i]->x + 1][placeholders[i]->y] = "++";
        }
    }
    //----------------------------------------------------------------
};

class Game
{

public:
    int currentLevel;

    //----------------------------------------------------------------
    //Map elements
    string wal = "WW";
    string emp = "  ";
    string box = "OO";
    string plh = "++";
    //----------------------------------------------------------------
    //Map tilemap  
    string level0[mapSize][mapSize] = { 
    {wal,wal,wal,wal,wal,wal,wal,wal,wal,wal,wal,wal,wal,wal,wal,wal,wal,wal,wal,wal,wal,wal,wal,wal,wal,wal,wal,wal,wal,wal,wal,wal,wal,wal,wal,wal,wal,wal,wal,wal},
    {wal,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,wal},
    {wal,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,"SOKOBAN ",emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,wal},
    {wal,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,wal},
    {wal,emp,emp,"GET INTO THE OLD WORLD OF GAME WITH SOKOBAN ",emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,wal},
    {wal,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,wal},
    {wal,emp,emp,"HERE ARE THE RULES HOW TO PLAY:-- ",emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,wal},
    {wal,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,wal},
    {wal,emp,emp,"1.THE PLAYER NEEDS TO MOVE THE BOX TO PLACE IT ON THE PLACE HOLDER. ",emp,emp,wal},
    {wal,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,wal},
    {wal,emp,emp,"2.USE THE ARROW KEYS TO MOVE THE PLAYER TO PLACE IT ",emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,wal},
    {wal,emp,emp,emp,"UP , DOWN , LEFT , RIIGHT.",emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,wal},
    {wal,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,wal},
    {wal,emp,emp,"3.ONCE ALL THE BOXES ARE PLACED THE PLAYER IS MOVED  NEW LEVEL. ",emp,emp,emp,emp,wal},
    {wal,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,wal},
    {wal,emp,emp,"4.IF THE PLAYER IS STUCK TEHY CAN RESPAWN THE LEVEL BY PRESSING KEY R.",emp,wal},
    {wal,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,wal},
    {wal,emp,emp," PLAYER : ","/\\",emp,emp,emp,emp,emp,emp,emp," BOX  : ","OO ",emp,emp,emp,emp,emp," PLACEHOLDER :",emp," ++ ",emp,emp,emp,emp,wal},
    {wal,emp,emp,emp,emp,emp,emp,emp,"\\/",emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,"OO ",emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp," ++ ",emp,emp,emp,emp,wal},
    {wal,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,wal},
    {wal,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,wal},
    {wal,wal,wal,wal,wal,wal,wal,wal,wal,wal,wal,wal,wal,wal,wal,wal,wal,wal,wal,wal,wal,wal,wal,wal,wal,wal,wal,wal,wal,wal,wal,wal,wal,wal,wal,wal,wal,wal,wal,wal},
    {wal,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,wal},
    {wal,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,wal},
    {wal,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,box,emp,emp,emp,plh,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,wal},
    {wal,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,wal},
    {wal,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,wal},
    {wal,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,emp,wal},
    {wal,wal,wal,wal,wal,wal,wal,wal,wal,wal,wal,wal,wal,wal,wal,wal,wal,wal,wal,wal,wal,wal,wal,wal,wal,wal,wal,wal,wal,wal,wal,wal,wal,wal,wal,wal,wal,wal,wal,wal},
  };
    
    string level1[mapSize][mapSize] = {
        {emp, emp, emp, wal, wal, wal, wal, emp, emp, emp, emp, emp},
        {emp, emp, emp, wal, plh, emp, wal, emp, emp, emp, emp, emp},
        {emp, emp, emp, wal, emp, emp, wal, emp, emp, emp, emp, emp},
        {emp, emp, emp, wal, emp, emp, wal, emp, emp, emp, emp, emp},
        {wal, wal, wal, wal, emp, emp, wal, wal, wal, wal, wal, wal},
        {wal, emp, emp, emp, box, emp, emp, emp, emp, box, plh, wal},
        {wal, emp, emp, emp, emp, emp, emp, emp, emp, emp, emp, wal},
        {wal, plh, emp, emp, box, emp, box, emp, wal, wal, wal, wal},
        {wal, emp, emp, emp, emp, emp, emp, emp, wal, emp, emp, emp},
        {wal, wal, wal, wal, wal, wal, plh, emp, wal, emp, emp, emp},
        {emp, emp, emp, emp, emp, wal, emp, emp, wal, emp, emp, emp},
        {emp, emp, emp, emp, emp, wal, wal, wal, wal, emp, emp, emp}};

    string level2[mapSize][mapSize] = {
        {wal, wal, wal, wal, wal, wal, wal, emp, emp, emp, emp, emp, emp, emp},
        {wal, emp, emp, emp, emp, emp, wal, emp, emp, wal, wal, wal, wal, wal},
        {wal, emp, emp, emp, emp, emp, wal, emp, emp, wal, wal, wal, wal, wal},
        {wal, emp, emp, box, emp, emp, wal, emp, emp, wal, emp, plh, emp, wal},
        {wal, emp, emp, emp, emp, emp, wal, emp, emp, wal, emp, emp, emp, wal},
        {wal, emp, emp, box, emp, emp, wal, emp, emp, wal, emp, plh, emp, wal},
        {wal, emp, emp, emp, emp, emp, wal, emp, emp, wal, emp, emp, emp, wal},
        {wal, wal, wal, wal, emp, emp, wal, emp, emp, wal, emp, plh, emp, wal},
        {wal, wal, wal, wal, emp, emp, wal, wal, wal, wal, emp, emp, emp, wal},
        {wal, wal, emp, emp, box, emp, emp, emp, emp, emp, emp, emp, emp, wal},
        {wal, wal, emp, emp, emp, emp, emp, emp, emp, emp, emp, emp, emp, wal},
        {wal, wal, emp, emp, emp, emp, emp, emp, wal, emp, emp, emp, emp, wal},
        {wal, wal, emp, emp, emp, emp, emp, emp, wal, emp, emp, emp, emp, wal},
        {wal, wal, wal, wal, wal, wal, wal, wal, wal, wal, wal, wal, wal, wal}};

    string level3[mapSize][mapSize] = {
       {emp, emp, wal, wal, wal, wal, wal, wal, wal, wal, wal, emp},
        {wal, wal, wal, emp, emp, emp, emp, emp, emp, emp, wal, emp},
        {wal, wal, wal, emp, emp, emp, emp, emp, emp, emp, wal, emp},
        {wal, emp, emp, emp, emp, emp, emp, box, plh, emp, wal, emp},
        {wal, emp, emp, emp, emp, emp, emp, emp, emp, emp, wal, emp},
        {wal, emp, emp, emp, emp, emp, emp, emp, emp, emp, wal, emp},
        {wal, emp, emp, emp, emp, emp, emp, emp, emp, emp, wal, wal},
        {wal, wal, wal, wal, box, box, emp, emp, emp, emp, emp, wal},
        {wal, wal, wal, wal, emp, emp, emp, emp, emp, emp, emp, wal},
        {wal, plh, emp, emp, box, emp, emp, emp, emp, emp, emp, wal},
        {wal, emp, emp, emp, emp, emp, emp, emp, emp, emp, emp, wal},
        {wal, plh, plh, emp, emp, emp, box, emp, emp, plh, emp, wal},
        {wal, emp, emp, emp, emp, emp, emp, emp, emp, emp, emp, wal},
        {wal, wal, wal, wal, wal, wal, wal, wal, wal, wal, wal, wal}};

    string level4[mapSize][mapSize] = {
        {emp, wal, wal, wal, wal, wal, wal, wal, wal, wal, wal, emp},
        {emp, wal, emp, emp, emp, emp, emp, emp, emp, emp, wal, emp},
        {emp, wal, emp, emp, emp, emp, emp, emp, emp, emp, wal, emp},
        {emp, wal, emp, emp, emp, box, emp, emp, emp, emp, wal, emp},
        {emp, wal, emp, emp, emp, emp, emp, emp, emp, emp, wal, emp},
        {wal, wal, wal, emp, emp, emp, wal, emp, emp, emp, wal, emp},
        {wal, wal, wal, emp, emp, emp, wal, emp, emp, emp, wal, emp},
        {wal, plh, wal, emp, emp, emp, wal, emp, emp, emp, wal, emp},
        {wal, emp, wal, emp, emp, emp, wal, emp, emp, emp, wal, emp},
        {wal, plh, box, emp, emp, emp, emp, wal, emp, emp, wal, emp},
        {wal, emp, emp, emp, emp, emp, emp, wal, emp, emp, wal, emp},
        {wal, plh, emp, emp, emp, emp, emp, box, emp, emp, wal, emp},
        {wal, emp, emp, emp, emp, emp, emp, emp, emp, emp, wal, emp},
        {wal, wal, wal, wal, wal, wal, wal, wal, wal, wal, wal, emp}};

    string level5[mapSize][mapSize] = {
        {emp, emp, wal, wal, wal, wal, wal, wal, wal, wal, wal, emp},
        {emp, emp, wal, emp, emp, emp, emp, emp, emp, emp, wal, emp},
        {emp, emp, wal, emp, emp, emp, emp, emp, emp, emp, wal, emp},
        {emp, emp, wal, box, box, box, emp, emp, emp, emp, wal, emp},
        {wal, wal, wal, emp, emp, emp, emp, emp, emp, emp, wal, emp},
        {wal, emp, emp, box, plh, plh, emp, emp, emp, emp, wal, emp},
        {wal, emp, emp, emp, emp, emp, emp, emp, emp, emp, wal, emp},
        {wal, emp, box, plh, plh, plh, emp, emp, emp, emp, wal, emp},
        {wal, emp, emp, emp, emp, emp, emp, emp, emp, wal, wal, emp},
        {wal, wal, wal, wal, emp, emp, emp, emp, emp, wal, wal, emp},
        {emp, emp, emp, wal, emp, emp, emp, emp, emp, wal, emp, emp},
        {emp, emp, emp, wal, wal, wal, wal, wal, wal, wal, emp, emp}};

    string level6[mapSize][mapSize] = {
        {emp, emp, emp, emp, wal, wal, wal, wal, wal, wal, wal, wal, wal, emp},
        {emp, emp, emp, emp, wal, emp, emp, emp, emp, emp, emp, emp, wal, emp},
        {emp, emp, emp, emp, wal, emp, emp, emp, emp, emp, emp, emp, wal, emp},
        {emp, emp, emp, emp, wal, emp, emp, emp, emp, emp, emp, emp, wal, emp},
        {wal, wal, wal, wal, wal, emp, emp, emp, emp, emp, emp, emp, wal, wal},
        {wal, emp, emp, emp, emp, box, plh, emp, emp, emp, emp, emp, emp, wal},
        {wal, emp, emp, emp, emp, emp, emp, emp, emp, emp, emp, emp, emp, wal},
        {wal, emp, emp, emp, emp, plh, box, plh, emp, emp, emp, emp, emp, wal},
        {wal, emp, emp, emp, emp, emp, emp, emp, emp, emp, emp, emp, emp, wal},
        {wal, wal, wal, emp, emp, box, plh, box, emp, emp, emp, emp, emp, wal},
        {emp, emp, wal, emp, emp, emp, emp, emp, emp, emp, emp, emp, emp, wal},
        {emp, emp, wal, emp, emp, emp, emp, emp, emp, emp, emp, emp, wal, wal},
        {emp, emp, wal, emp, emp, emp, emp, emp, emp, emp, emp, emp, wal, emp},
        {emp, emp, wal, wal, wal, wal, wal, wal, wal, wal, wal, wal, wal, emp}};

    string level7[mapSize][mapSize] = {
       {emp, emp, emp, wal, wal, wal, wal, wal, wal, wal, wal, emp, emp, emp},
        {emp, emp, emp, wal, emp, emp, plh, plh, emp, emp, wal, emp, emp, emp},
        {emp, emp, emp, wal, emp, emp, emp, emp, emp, emp, wal, emp, emp, emp},
        {emp, emp, emp, wal, emp, emp, emp, plh, emp, emp, wal, emp, emp, emp},
        {emp, emp, emp, wal, emp, emp, emp, emp, emp, emp, wal, emp, emp, emp},
        {emp, emp, emp, wal, emp, emp, emp, box, emp, emp, wal, emp, emp, emp},
        {emp, emp, wal, wal, emp, emp, emp, emp, emp, emp, wal, wal, emp, emp},
        {emp, emp, wal, emp, emp, emp, box, emp, emp, emp, emp, wal, emp, emp},
        {wal, wal, wal, emp, emp, emp, emp, emp, emp, emp, emp, wal, wal, wal},
        {wal, emp, emp, emp, emp, emp, wal, box, box, emp, emp, emp, plh, wal},
        {wal, emp, emp, emp, emp, emp, wal, emp, emp, emp, emp, emp, emp, wal},
        {wal, emp, emp, emp, emp, emp, emp, emp, emp, emp, emp, emp, emp, wal},
        {wal, emp, emp, emp, emp, emp, emp, emp, emp, emp, emp, emp, emp, wal},
        {wal, wal, wal, wal, wal, wal, wal, wal, wal, wal, wal, wal, wal, wal}};

    string level8[mapSize][mapSize] = {
            {wal, wal, wal, wal, wal, wal, wal, wal, wal},
        {wal, emp, emp, wal, emp, emp, emp, emp, wal},
        {wal, emp, emp, wal, emp, emp, emp, emp, wal},
        {wal, emp, box, plh, plh, box, emp, emp, wal},
        {wal, emp, emp, emp, emp, emp, emp, emp, wal},
        {wal, emp, box, plh, box, plh, emp, wal, wal},
        {wal, emp, emp, emp, emp, emp, emp, wal, wal},
        {wal, emp, box, plh, plh, box, emp, emp, wal},
        {wal, emp, emp, emp, emp, emp, emp, emp, wal},
        {wal, emp, emp, wal, emp, emp, emp, emp, wal},
        {wal, emp, emp, wal, emp, emp, emp, emp, wal},
        {wal, wal, wal, wal, wal, wal, wal, wal, wal},
        {emp, emp, emp, emp, emp, emp, emp, emp, emp}};

    string level9[mapSize][mapSize] = {
        {wal, wal, wal, wal, wal, wal, wal, emp, emp, emp, emp, emp},
        {wal, emp, emp, emp, emp, emp, wal, emp, emp, emp, emp, emp},
        {wal, emp, emp, emp, emp, emp, wal, emp, emp, emp, emp, emp},
        {wal, emp, emp, box, box, box, wal, emp, emp, emp, emp, emp},
        {wal, emp, emp, emp, emp, emp, wal, wal, wal, emp, emp, emp},
        {wal, emp, emp, emp, wal, plh, emp, plh, wal, emp, emp, emp},
        {wal, emp, emp, emp, wal, emp, emp, emp, wal, wal, wal, wal},
        {wal, emp, emp, emp, emp, plh, emp, plh, box, emp, emp, wal},
        {wal, emp, emp, emp, emp, emp, emp, emp, emp, emp, emp, wal},
        {wal, wal, wal, emp, emp, emp, emp, emp, emp, emp, emp, wal},
        {emp, emp, wal, emp, emp, emp, emp, emp, emp, emp, emp, wal},
        //{emp, emp, wal, emp, emp, emp, emp, emp, emp, emp, emp, wal},
        {emp, emp, wal, wal, wal, wal, wal, wal, wal, wal, wal, wal}};

    string level10[mapSize][mapSize] = {
        {wal, wal, wal, wal, wal, wal, wal, emp, emp, emp, emp, emp},
        {wal, plh, plh, box, plh, plh, wal, emp, emp, emp, emp, emp},
        {wal, emp, emp, emp, emp, emp, wal, emp, emp, emp, emp, emp},
        {wal, plh, plh, wal, plh, plh, wal, emp, emp, emp, emp, emp},
        {wal, emp, emp, wal, emp, emp, wal, emp, emp, emp, emp, emp},
        {wal, emp, box, box, box, emp, wal, emp, emp, emp, emp, emp},
        {wal, emp, emp, emp, emp, emp, wal, emp, emp, emp, emp, emp},
        {wal, emp, emp, box, emp, emp, wal, emp, emp, emp, emp, emp},
        {wal, emp, emp, emp, emp, emp, wal, emp, emp, emp, emp, emp},
        {wal, emp, box, box, box, emp, wal, emp, emp, emp, emp, emp},
        {wal, emp, emp, emp, emp, emp, wal, emp, emp, emp, emp, emp},
        {wal, emp, emp, wal, emp, emp, wal, emp, emp, emp, emp, emp},
        {wal, emp, emp, wal, emp, emp, wal, emp, emp, emp, emp, emp},
        {wal, wal, wal, wal, wal, wal, wal, emp, emp, emp, emp, emp}};

    string level11[mapSize][mapSize] = {
        {emp, wal, wal, wal, wal, wal, wal, wal, emp, emp, emp, emp},
        {emp, wal, emp, emp, emp, emp, emp, wal, emp, emp, emp, emp},
        {emp, wal, emp, emp, emp, emp, emp, wal, wal, wal, emp, emp},
        {emp, wal, emp, emp, wal, box, emp, emp, emp, wal, emp, emp},
        {wal, wal, emp, emp, wal, emp, emp, emp, emp, wal, emp, emp},
        {wal, emp, box, plh, plh, emp, plh, emp, wal, wal, emp, emp},
        {wal, emp, emp, emp, emp, emp, emp, emp, wal, emp, emp, emp},
        {wal, emp, emp, emp, box, box, emp, emp, wal, emp, emp, emp},
        {wal, emp, emp, emp, emp, emp, emp, emp, wal, emp, emp, emp},
        {wal, wal, wal, emp, emp, wal, plh, emp, wal, emp, emp, emp},
        {emp, emp, wal, emp, emp, wal, emp, emp, wal, emp, emp, emp},
        {emp, emp, wal, emp, emp, emp, emp, emp, wal, emp, emp, emp},
        {emp, emp, wal, emp, emp, emp, emp, emp, wal, emp, emp, emp},
        {emp, emp, wal, wal, wal, wal, wal, wal, wal, emp, emp, emp}};

    string level12[mapSize][mapSize] = {
        {wal, wal, wal, wal, wal, wal, wal, wal, wal, wal, wal, wal, wal, wal, wal, wal},
        {wal, emp, emp, emp, emp, emp, emp, emp, emp, emp, emp, emp, emp, emp, emp, wal},
        {wal, emp, emp, emp, emp, emp, emp, emp, emp, emp, emp, emp, emp, emp, emp, wal},
        {wal, emp, emp, box, emp, emp, emp, emp, emp, emp, emp, emp, emp, emp, emp, wal},
        {wal, emp, emp, emp, emp, emp, emp, emp, emp, emp, emp, emp, emp, emp, emp, wal},
        {wal, wal, wal, box, emp, emp, emp, emp, emp, plh, emp, emp, emp, emp, emp, wal},
        {wal, wal, wal, emp, emp, emp, emp, emp, emp, emp, emp, emp, emp, emp, emp, wal},
        {wal, emp, emp, box, emp, emp, emp, emp, emp, plh, emp, emp, emp, emp, wal, wal},
        {wal, emp, emp, emp, emp, emp, emp, emp, emp, emp, emp, emp, emp, emp, wal, emp},
        {wal, emp, emp, box, emp, emp, emp, emp, emp, plh, emp, emp, emp, emp, wal, emp},
        {wal, emp, emp, emp, emp, emp, emp, emp, emp, emp, emp, emp, emp, emp, wal, emp},
        {wal, emp, emp, box, emp, emp, emp, emp, emp, plh, emp, emp, emp, emp, wal, emp},
        {wal, emp, emp, emp, emp, emp, emp, emp, emp, emp, emp, emp, emp, emp, wal, emp},
        {wal, emp, emp, plh, emp, emp, emp, emp, emp, emp, emp, emp, emp, emp, wal, emp},
        {wal, emp, emp, emp, emp, emp, emp, emp, emp, emp, emp, emp, emp, emp, wal, emp},
        {wal, wal, wal, wal, wal, wal, wal, wal, wal, wal, wal, wal, wal, wal, wal, emp},
    };

    string level13[mapSize][mapSize] = {
        {emp, emp, emp, wal, wal, wal, wal, wal, emp, emp, emp, emp},
        {emp, emp, emp, wal, emp, emp, emp, wal, emp, emp, emp, emp},
        {emp, emp, emp, wal, emp, emp, emp, wal, emp, emp, emp, emp},
        {emp, emp, emp, wal, box, emp, emp, wal, emp, emp, emp, emp},
        {wal, wal, wal, wal, emp, emp, emp, wal, wal, wal, wal, wal},
        {wal, emp, emp, emp, box, emp, plh, emp, emp, plh, emp, wal},
        {wal, emp, emp, emp, emp, emp, emp, emp, emp, emp, emp, wal},
        {wal, emp, emp, emp, box, emp, plh, emp, emp, emp, emp, wal},
        {wal, emp, emp, emp, emp, emp, emp, emp, emp, emp, emp, wal},
        {wal, emp, emp, emp, box, emp, emp, emp, emp, emp, emp, wal},
        {wal, emp, emp, emp, emp, emp, emp, emp, emp, emp, emp, wal},
        {wal, emp, emp, emp, box, emp, plh, emp, emp, emp, wal, wal},
        {wal, emp, emp, emp, emp, emp, emp, emp, emp, emp, wal, emp},
        {wal, wal, emp, emp, plh, emp, emp, emp, emp, emp, wal, emp},
        {emp, wal, emp, emp, emp, emp, emp, emp, emp, emp, wal, emp},
        {emp, wal, wal, wal, wal, wal, wal, wal, wal, wal, wal, emp},
    };
    
    string level14[mapSize][mapSize] = {
        {wal, wal, wal, wal, wal, wal, emp, emp, emp, emp, emp, emp, emp, emp, emp, emp},
        {wal, emp, emp, emp, emp, wal, emp, emp, emp, emp, emp, emp, emp, emp, emp, emp},
        {wal, emp, emp, emp, emp, wal, wal, wal, wal, wal, wal, wal, emp, emp, emp, emp},
        {wal, emp, emp, wal, emp, wal, emp, emp, emp, emp, emp, wal, emp, emp, emp, emp},
        {wal, emp, emp, wal, emp, wal, emp, emp, emp, emp, emp, wal, emp, emp, emp, emp},
        {wal, emp, emp, box, emp, emp, emp, box, emp, emp, emp, wal, emp, emp, emp, emp},
        {wal, emp, emp, emp, emp, emp, emp, emp, emp, emp, emp, wal, emp, emp, emp, emp},
        {wal, plh, emp, plh, wal, box, wal, box, emp, emp, emp, wal, emp, emp, emp, emp},
        {wal, emp, emp, emp, wal, emp, wal, emp, emp, emp, emp, wal, emp, emp, emp, emp},
        {wal, plh, emp, emp, box, emp, emp, emp, emp, emp, emp, wal, emp, emp, emp, emp},
        {wal, emp, emp, emp, emp, emp, emp, emp, emp, emp, emp, wal, emp, emp, emp, emp},
        {wal, plh, emp, plh, emp, emp, emp, emp, emp, emp, emp, wal, emp, emp, emp, emp},
        {wal, emp, emp, emp, emp, emp, emp, emp, emp, emp, emp, wal, emp, emp, emp, emp},
        {wal, wal, wal, wal, wal, wal, wal, wal, wal, wal, wal, wal, emp, emp, emp, emp},};

    string level15[mapSize][mapSize] = {
        {emp, emp, emp, wal, wal, wal, wal, wal, wal, wal, wal, emp, emp, emp, emp, emp},
        {emp, emp, emp, wal, emp, emp, emp, emp, emp, emp, wal, wal, wal, wal, emp, emp},
        {emp, emp, emp, wal, emp, emp, emp, emp, emp, emp, wal, wal, wal, wal, emp, emp},
        {wal, wal, wal, wal, plh, wal, wal, wal, box, emp, emp, emp, emp, wal, emp, emp},
        {wal, wal, wal, wal, emp, wal, wal, wal, emp, emp, emp, emp, emp, wal, emp, emp},
        {wal, emp, emp, emp, plh, plh, box, emp, emp, emp, emp, emp, emp, wal, emp, emp},
        {wal, emp, emp, emp, emp, emp, emp, emp, emp, emp, emp, emp, emp, wal, emp, emp},
        {wal, emp, emp, emp, emp, wal, box, emp, emp, emp, emp, emp, emp, wal, emp, emp},
        {wal, emp, emp, emp, emp, wal, emp, emp, emp, emp, emp, emp, emp, wal, emp, emp},
        {wal, emp, emp, emp, emp, emp, emp, emp, emp, emp, emp, wal, wal, wal, emp, emp},
        {wal, emp, emp, emp, emp, emp, emp, emp, emp, emp, emp, wal, wal, wal, emp, emp},
        {wal, wal, wal, wal, wal, wal, wal, wal, wal, wal, wal, wal, wal, wal, emp, emp},};
    //----------------------------------------------------------------

    //Level's Array 
    string (*currlevel[17])[40] = {
        level0,
        level1,
        level2,
        level3,
        level4,
        level5,
        level6,
        level7,
        level8, 
        level9,
        level10,
        level11,
        level12,
        level13,
        level14,
        level15
    };

    //Start positions of the player in every level
    vector<pair<int, int>> playerPostion()
    {
        vector<pair<int, int>> playerStartinPositin;
        playerStartinPositin.push_back(make_pair(22, 19));
        playerStartinPositin.push_back(make_pair(5, 7));
        playerStartinPositin.push_back(make_pair(1, 3));
        playerStartinPositin.push_back(make_pair(5, 2));
        playerStartinPositin.push_back(make_pair(1, 2));
        playerStartinPositin.push_back(make_pair(7, 1));
        playerStartinPositin.push_back(make_pair(1, 11));
        playerStartinPositin.push_back(make_pair(11, 6));
        playerStartinPositin.push_back(make_pair(1, 1)); // checked
        playerStartinPositin.push_back(make_pair(1, 1));
        playerStartinPositin.push_back(make_pair(5, 1));
        playerStartinPositin.push_back(make_pair(3, 3));
        playerStartinPositin.push_back(make_pair(1, 1));
        playerStartinPositin.push_back(make_pair(5, 1));
        playerStartinPositin.push_back(make_pair(11, 4));
        playerStartinPositin.push_back(make_pair(9, 5));
        return playerStartinPositin;
    }

    //Sizes of every level
    vector<pair<int, int>> mapSizes()
    {
        vector<pair<int, int>> xy;
        xy.push_back(make_pair(30, 40));
        xy.push_back(make_pair(12, 12));
        xy.push_back(make_pair(14, 16));
        xy.push_back(make_pair(14, 12));
        xy.push_back(make_pair(14, 12));
        xy.push_back(make_pair(12, 12));
        xy.push_back(make_pair(14, 14));
        xy.push_back(make_pair(14, 14));
        xy.push_back(make_pair(13, 12)); // checked
        xy.push_back(make_pair(12, 12));
        xy.push_back(make_pair(14, 12));
        xy.push_back(make_pair(14, 12));
        xy.push_back(make_pair(16, 16));
        xy.push_back(make_pair(17, 12));
        xy.push_back(make_pair(14, 16));
        xy.push_back(make_pair(12, 16));
        return xy;
    }

    //Variables that store all map sizes and player positions
    vector<pair<int, int>> mapXYSizes = mapSizes();
    vector<pair<int, int>> playerStartinPositin = playerPostion();

    //This function loops through every map one by one 
    Map *getCurrentlevelMap()
    {
        // Load the map, set the player position and run the map
        for (int i = 0; i < 16; i++)
        {
            Map *level = new Map(currlevel[i], mapXYSizes[i].first, mapXYSizes[i].second);
            i -= mainloop(level, playerStartinPositin[i].first, playerStartinPositin[i].second, mapXYSizes[i].first, mapXYSizes[i].second, i);
        }

        cout << endl
             << endl
             << "************GAME OVER*************" << endl
             << endl;
    }

    //This is the main function that gets input from user and performs required operations
    int mainloop(Map *level, int p_xcor, int p_ycor, int rows, int cols, int i)
    {
        //Player object is created and the initial starting position of the player is defined
        Player *player = new Player(p_xcor, p_ycor);

        //Position of placeholders, boxes and player is initialized in the Map object level
        level->injestPlaceholders();
        level->injestBoxes();
        level->injestPlayer(player, player->x, player->y);

        //Display everything of the map 
        level->render(rows, cols);
        while (1)
        {
            //Get the user input in ch
            int ch = (int)getch();
            //If input is 'R' then reload the map
            if (ch == 114)
                return 1;
            //If input is 'S' then skip the map
            if(ch==115)
                return 0;
            
            //Pass the ch in movement function to move the player accordingly
            level->movement(ch, player, rows, cols);
            
            //If all boxes are places correctly then break this loop so that to load next level 
            if (level->checkAllBoxPlaced())
                break;
        }
        return 0;
    }
};

int main()
{   
    // Create a game object
    Game game;
    // Get the current level and run the map
    game.getCurrentlevelMap();
    return 0;
}
