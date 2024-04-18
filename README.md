## Introduction
This practical work is delivery document for class of Video Game Development Techniques of the course of Engineering in Digital Game Development, proposed by Professor Daniel Nogueira. The work has several tasks and objectives given by teacher:
* A brief description of the game.
* Decisions made in development of game.
* The game instructions.
* Brief analysis and Organization of the files and code.

# The game "[Pacman](https://github.com/NoanFelipe/Pacman-Monogame-C-?tab=readme-ov-file)"
![192284-thelogicalindianfb1000x600-1](https://github.com/Mnbel555/Pacman-Report/assets/125232753/902aa612-c92a-462e-9150-e153204b1c3f)

## Introduction
Pac-Man is an action maze chase video game; the player controls the eponymous character through an enclosed maze. The objective of the game is to eat all of the dots placed in the maze while avoiding four colored ghosts—Blinky (red), Pinky (pink), Inky (cyan), and Clyde (orange)—who pursue Pac-Man.

## Mechanism 
The main objective of Pac-Man is to navigate through a maze, eating all the dots while avoiding being caught by ghosts. If Pac-Man touches a ghost, he loses a life.

### Objective
Eat all dots while avoiding ghosts.

### Components
- Pac-Man: Player-controlled character
- Ghosts: Enemy characters
- Dots and Power Pellets: Collectible items

### Gameplay
- Control Pac-Man with W,A,S,D.
- Eat dots to score points.
- Avoid ghosts; they end the game if touched.
- Eating power pellets allows Pac-Man to eat ghosts temporarily.
  
## Instructions 
"To play Pac-Man, use the W,A,S,D keys to control Pac-Man's movement through the maze. The objective is to eat all the dots scattered throughout the maze while avoiding contact with the ghosts. If Pac-Man touches a ghost, the game ends. However, eating power pellets will temporarily turn the ghosts blue and make them vulnerable, allowing Pac-Man to eat them for extra points. Keep an eye on your score as you gobble up dots and ghosts to see how well you're doing.

## Organization of Files and Folders
![Screenshot 2024-04-15 123044](https://github.com/Mnbel555/Pacman-Report/assets/125232753/70dc9aa5-e0aa-4ca9-b2bf-abfbd4dc8ccb)

These files doesnt seems properly organized, Anyways lets go through some of folder and files, `Content`  Content folder are  the  assets  that the  Monogame  framework recognizes  to locate and load images, audios and other files that are not directly part of the game code.

##  Analysis of Code
In this part, code blocks are described like addition of sprites, sounds, scores,controls of game etc.

 <details>
    <summary>Audio Initialization</summary>
 A static class MySounds that declares several SoundEffect fields
   
```csharp
//mySounds.cs
   public static class MySounds
    {
        public static SoundEffect game_start;
        public static SoundEffect munch;
        public static SoundEffectInstance munchInstance;
        public static SoundEffect credit;
        public static SoundEffect death_1;
        public static SoundEffect death_2;
        public static SoundEffect eat_fruit;
        public static SoundEffect eat_ghost;
        public static SoundEffect power_pellet;
        public static SoundEffectInstance power_pellet_instance;
        public static SoundEffect extend;
        public static SoundEffect intermission;
        public static SoundEffect retreating;
        public static SoundEffectInstance retreatingInstance;
        public static SoundEffect siren_1;
        public static SoundEffectInstance siren_1_instance;
        public static SoundEffect siren_2;
        public static SoundEffect siren_3;
        public static SoundEffect siren_4;
        public static SoundEffect siren_5;
    }

```
this code initializes the sound effects by loading them from the content pipeline using the Content.Load<SoundEffect> method

```csharp
//Game1.cs
 MySounds.credit = Content.Load<SoundEffect>("Sounds/credit");
            MySounds.death_1 = Content.Load<SoundEffect>("Sounds/death_1");
            MySounds.death_2 = Content.Load<SoundEffect>("Sounds/death_2");
            MySounds.eat_fruit = Content.Load<SoundEffect>("Sounds/eat_fruit");
            MySounds.eat_ghost = Content.Load<SoundEffect>("Sounds/eat_ghost");
            MySounds.extend = Content.Load<SoundEffect>("Sounds/extend");
            MySounds.game_start = Content.Load<SoundEffect>("Sounds/game_start");
            MySounds.intermission = Content.Load<SoundEffect>("Sounds/intermission");

            MySounds.munch = Content.Load<SoundEffect>("Sounds/munch");
            MySounds.munchInstance = MySounds.munch.CreateInstance();
            MySounds.munchInstance.Volume = 0.35f;
            MySounds.munchInstance.IsLooped = true;

            MySounds.power_pellet = Content.Load<SoundEffect>("Sounds/power_pellet");
            MySounds.power_pellet_instance = MySounds.power_pellet.CreateInstance();
            MySounds.power_pellet_instance.IsLooped = true;

            MySounds.retreating = Content.Load<SoundEffect>("Sounds/retreating");
            MySounds.retreatingInstance = MySounds.retreating.CreateInstance();
            MySounds.retreatingInstance.IsLooped = true;

            MySounds.siren_1 = Content.Load<SoundEffect>("Sounds/siren_1");
            MySounds.siren_1_instance = MySounds.siren_1.CreateInstance();
            MySounds.siren_1_instance.Volume = 0.8f;
            MySounds.siren_1_instance.IsLooped = true;

            MySounds.siren_2 = Content.Load<SoundEffect>("Sounds/siren_2");
            MySounds.siren_3 = Content.Load<SoundEffect>("Sounds/siren_3");
            MySounds.siren_4 = Content.Load<SoundEffect>("Sounds/siren_4");
            MySounds.siren_5 = Content.Load<SoundEffect>("Sounds/siren_5");

```

</details>

<details>
    <summary>Keyboard control</summary>
  
  
It handles the movement controls based on the keys pressed by the player (W, A, S, D keys for Up, Left, Down, Right respectively). Depending on the direction input, Pacman's nextDirection is updated

```csharp
public void Update(GameTime gameTime, Tile[,] tileArray)
{
    KeyboardState kState = Keyboard.GetState();
    float dt = (float)gameTime.ElapsedGameTime.TotalSeconds;
    if (!canMove)
    {
        canMoveTimer += dt;
        if (canMoveTimer >= TimerThreshold)
        {
            canMove = true;
            canMoveTimer = 0;
        }
    }
    playerAnim.Update(gameTime);

    if (kState.IsKeyDown(Keys.D))
    {
        nextDirection = Dir.Right;
    }
    if (kState.IsKeyDown(Keys.A))
    {
        nextDirection = Dir.Left;
    }
    if (kState.IsKeyDown(Keys.W))
    {
        nextDirection = Dir.Up;
    }
    if (kState.IsKeyDown(Keys.S))
    {
        nextDirection = Dir.Down;
    }
```
</details>

<details>
    <summary>Map Design</summary>
  
  
 
The mapDesign array represents the layout of the game board for Pacman. Each number in the array corresponds to a different type of tile or element on the game board.

```csharp
  private int[,] mapDesign = new int[,]{
            { 1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1},
            { 1,0,0,0,0,0,0,0,0,0,0,0,0,1,1,0,0,0,0,0,0,0,0,0,0,0,0,1},
            { 1,0,1,1,1,1,0,1,1,1,1,1,0,1,1,0,1,1,1,1,1,0,1,1,1,1,0,1},
            { 1,3,1,1,1,1,0,1,1,1,1,1,0,1,1,0,1,1,1,1,1,0,1,1,1,1,3,1},
            { 1,0,1,1,1,1,0,1,1,1,1,1,0,1,1,0,1,1,1,1,1,0,1,1,1,1,0,1},
            { 1,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,1},
            { 1,0,1,1,1,1,0,1,1,0,1,1,1,1,1,1,1,1,0,1,1,0,1,1,1,1,0,1},
            { 1,0,1,1,1,1,0,1,1,0,1,1,1,1,1,1,1,1,0,1,1,0,1,1,1,1,0,1},
            { 1,0,0,0,0,0,0,1,1,0,0,0,0,1,1,0,0,0,0,1,1,0,0,0,0,0,0,1},
            { 1,1,1,1,1,1,0,1,1,1,1,1,5,1,1,5,1,1,1,1,1,0,1,1,1,1,1,1},
            { 1,1,1,1,1,1,0,1,1,1,1,1,5,1,1,5,1,1,1,1,1,0,1,1,1,1,1,1},
            { 1,1,1,1,1,1,0,1,1,5,5,5,5,5,5,5,5,5,5,1,1,0,1,1,1,1,1,1},
            { 1,1,1,1,1,1,0,1,1,5,1,1,1,2,2,1,1,1,5,1,1,0,1,1,1,1,1,1},
            { 1,1,1,1,1,1,0,1,1,5,1,2,2,2,2,2,2,1,5,1,1,0,1,1,1,1,1,1},
            { 0,0,0,0,0,0,0,5,5,5,1,2,2,2,2,2,2,1,5,5,5,0,0,0,0,0,0,0},
            { 1,1,1,1,1,1,0,1,1,5,1,2,2,2,2,2,2,1,5,1,1,0,1,1,1,1,1,1},
            { 1,1,1,1,1,1,0,1,1,5,1,1,1,1,1,1,1,1,5,1,1,0,1,1,1,1,1,1},
            { 1,1,1,1,1,1,0,1,1,5,5,5,5,5,5,5,5,5,5,1,1,0,1,1,1,1,1,1},
            { 1,1,1,1,1,1,0,1,1,5,1,1,1,1,1,1,1,1,5,1,1,0,1,1,1,1,1,1},
            { 1,1,1,1,1,1,0,1,1,5,1,1,1,1,1,1,1,1,5,1,1,0,1,1,1,1,1,1},
            { 1,0,0,0,0,0,0,0,0,0,0,0,0,1,1,0,0,0,0,0,0,0,0,0,0,0,0,1},
            { 1,0,1,1,1,1,0,1,1,1,1,1,0,1,1,0,1,1,1,1,1,0,1,1,1,1,0,1},
            { 1,0,1,1,1,1,0,1,1,1,1,1,0,1,1,0,1,1,1,1,1,0,1,1,1,1,0,1},
            { 1,3,0,0,1,1,0,0,0,0,0,0,0,5,5,0,0,0,0,0,0,0,1,1,0,0,3,1},
            { 1,1,1,0,1,1,0,1,1,0,1,1,1,1,1,1,1,1,0,1,1,0,1,1,0,1,1,1},
            { 1,1,1,0,1,1,0,1,1,0,1,1,1,1,1,1,1,1,0,1,1,0,1,1,0,1,1,1},
            { 1,0,0,0,0,0,0,1,1,0,0,0,0,1,1,0,0,0,0,1,1,0,0,0,0,0,0,1},
            { 1,0,1,1,1,1,1,1,1,1,1,1,0,1,1,0,1,1,1,1,1,1,1,1,1,1,0,1},
            { 1,0,1,1,1,1,1,1,1,1,1,1,0,1,1,0,1,1,1,1,1,1,1,1,1,1,0,1},
            { 1,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,1},
            { 1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1}
        };
```
These values are used to define the layout and characteristics of the game board,
* 0: Represents an empty tile where Pacman can move freely.
* 1: Represents a wall or barrier that Pacman cannot pass through.
* 2: Represents a regular snack that Pacman can eat for points.
* 3: Represents the position where Pacman starts.
* 5: Represents a big snack (also known as a power pellet) that Pacman can eat to gain temporary power over the ghosts.

```csharp
public void createGrid() // creates grid that contains Tile objects, which represent 24x24 pixels squares in the game, all with types such as walls, snacks and etc.
        {
            for (int y = 0; y < numberOfTilesY; y++)
            {
                for (int x = 0; x < numberOfTilesX; x++)
                {
                    if (mapDesign[y, x] == 0) // small snack
                    {
                        tileArray[x, y] = new Tile(new Vector2(x * tileWidth, y * tileHeight + Game1.scoreOffSet), Tile.TileType.Snack);
                        tileArray[x, y].isEmpty = false;
                        snackList.Add(new Snack(Snack.SnackType.Small, new Vector2(x * tileWidth, y * tileHeight + Game1.scoreOffSet), new int[] { x, y}));
                    }
                    else if (mapDesign[y, x] == 1) // wall collider
                    {
                        tileArray[x, y] = new Tile(new Vector2(x * tileWidth, y * tileHeight + Game1.scoreOffSet), Tile.TileType.Wall);
                        tileArray[x, y].isEmpty = false;
                    }
                    else if (mapDesign[y, x] == 2) //  ghost house
                    {
                        tileArray[x, y] = new Tile(new Vector2(x * tileWidth, y * tileHeight + Game1.scoreOffSet), Tile.TileType.GhostHouse);
                        tileArray[x, y].isEmpty = false;
                    }
                    else if (mapDesign[y, x] == 3) // big snack
                    {
                        tileArray[x, y] = new Tile(new Vector2(x * tileWidth, y * tileHeight + Game1.scoreOffSet), Tile.TileType.Snack);
                        tileArray[x, y].isEmpty = false;
                        snackList.Add(new Snack(Snack.SnackType.Big, new Vector2(x * tileWidth, y * tileHeight + Game1.scoreOffSet), new int[] { x, y }));
                    }
                    else if (mapDesign[y, x] == 5) // empty
                    {
                        tileArray[x, y] = new Tile(new Vector2(x * tileWidth, y * tileHeight + Game1.scoreOffSet));
                    }
                }
            }
        }

```
</details>

<details>
    <summary>Scores</summary>
  
 In the Snack class constructor, the scoreGain property is set based on the SnackType provided, If the SnackType is Big, the scoreGain is set to 50, otherwise, it's set to 10. gridPosition and gridTile  serve similar purposes, but having both might provide flexibility in how the snack's position is represented or used in the code.

```csharp
public Snack(SnackType newSnackType, Vector2 newPosition, int[] newGridTile)
{
    snackType = newSnackType;
    if (newSnackType == SnackType.Big)
    {
        scoreGain = 50;
        radiusOffSet = 12;
    }
    else
    {
        scoreGain = 10;
        radiusOffSet = 3;
    }
        
    gridPosition = newPosition;
    gridTile = newGridTile;
}

```
                                                                                                                                                  
Here, when an enemy is eaten, the game increments the score based on the ghostScoreMultiplier. This multiplier seems to increase each time a ghost is eaten without Pacman consuming a power pellet. The score values are typical for Pacman: 200 points for the first ghost, 400 for the second, 800 for the third, and 1600 for the fourth.
                                                                                                                                                  
```csharp
public virtual void getEaten()
{
    switch (Game1.gameController.ghostScoreMultiplier)
    {
        case 1:
            Game1.score += 200;
            break;
        case 2:
            Game1.score += 400;
            break;
        case 3:
            Game1.score += 800;
            break;
        case 4:
            Game1.score += 1600;
            break;
    }
    Game1.gameController.ghostScoreMultiplier += 1;
    state = EnemyState.Eaten;
    speed = eatenSpeed;
    timerFrightened = 0;
    MySounds.eat_ghost.Play();
    MySounds.retreatingInstance.Play();
    MySounds.power_pellet_instance.Stop();
    return;
}

```

This line of code draws the score at the position (3, 3) with a font size of 24 and color white                                                                                                                                                                                                                                                                   
```csharp
text.draw(_spriteBatch, "score - " + score, new Vector2(3, 3), 24, Text.Color.White);

```    
</details>

<details>
    <summary>Enemies Movement and Collision</summary>
The decideDirection method is called to determine the direction the enemy should move in useing pathfinding algorithm to find the shortest path to the target position.The method checks the enemy's current state and the position of the player and other ghosts.Based on the state and positions, the method sets the pathToPacMan variable to the shortest path to the target position.
  
```csharp
  public void decideDirection(Vector2 playerTilePos, Dir playerDir, Controller gameController, Vector2 blinkyPos)
        {
            if (!foundpathTile.Equals(currentTile))
            {
                if (state == EnemyState.Scatter)
                {
                    pathToPacMan = Pathfinding.findPath(currentTile, getScatterTargetPosition(), gameController.tileArray, direction);
                } else if (state == EnemyState.Chase)
                {
                    pathToPacMan = Pathfinding.findPath(currentTile, getChaseTargetPosition(playerTilePos, playerDir, gameController.tileArray), gameController.tileArray, direction);
                    if (type == GhostType.Inky)
                    {
                        pathToPacMan = Pathfinding.findPath(currentTile, getChaseTargetPosition(playerTilePos, playerDir, gameController.tileArray, blinkyPos), gameController.tileArray, direction);
                    }
                } else if (state == EnemyState.Frightened)
                {
                    pathToPacMan = Pathfinding.findPath(currentTile, getFrightenedTargetPosition(), gameController.tileArray, direction);
                } else if (state == EnemyState.Eaten)
                {
                    pathToPacMan = Pathfinding.findPath(currentTile, getEatenTargetPosition(), gameController.tileArray, direction);
                }
                foundpathTile = currentTile;
            }


            if (currentTile.Equals(getEatenTargetPosition()) && state == EnemyState.Eaten)
            {
                state = EnemyState.Chase;
                speed = normalSpeed;
                MySounds.power_pellet_instance.Play();
            }
            if (playerTilePos.Equals(currentTile))
            {
                if (state == EnemyState.Frightened)
                {
                    getEaten();
                }
                if (state != EnemyState.Eaten)
                { 
                    colliding = true;
                    return;
                }
            }
            if (pathToPacMan.Count == 0) { return; }

            if (pathToPacMan[0].X > currentTile.X)
            {
                direction = Dir.Right;
                position.Y = gameController.tileArray[(int)currentTile.X, (int)currentTile.Y].Position.Y;
            }
            else if (pathToPacMan[0].X < currentTile.X)
            {
                direction = Dir.Left;
                position.Y = gameController.tileArray[(int)currentTile.X, (int)currentTile.Y].Position.Y;
            }
            else if (pathToPacMan[0].Y > currentTile.Y)
            {
                direction = Dir.Down;
                position.X = gameController.tileArray[(int)currentTile.X, (int)currentTile.Y].Position.X;
            }
            else if (pathToPacMan[0].Y < currentTile.Y)
            {
                direction = Dir.Up;
                position.X = gameController.tileArray[(int)currentTile.X, (int)currentTile.Y].Position.X;
            }
        }

```
The Move method is called to move the enemy to the next position in the path. it updates the enemy's position based on the direction and speed. the method also checks for collisions with the game board and other ghosts.

```csharp
  public void Move(GameTime gameTime, Tile[,] tileArray)
        {
            float dt = (float)gameTime.ElapsedGameTime.TotalSeconds;

            switch (direction)
            {
                case Dir.Right:
                    position.X += speed * dt;
                    if (state == EnemyState.Frightened)
                    {
                        if (timerFrightened > 5)
                        {
                            enemyAnim.setSourceRects(frightenedRectsEnd);
                            break;
                        }
                        enemyAnim.setSourceRects(frightenedRects);
                    }
                    else
                        enemyAnim.setSourceRects(rectsRight);
                    break;

                case Dir.Left:
                    position.X -= speed * dt;
                    if (state == EnemyState.Frightened)
                    {
                        if (timerFrightened > 5)
                        {
                            enemyAnim.setSourceRects(frightenedRectsEnd);
                            break;
                        }
                        enemyAnim.setSourceRects(frightenedRects);
                    }
                    else
                        enemyAnim.setSourceRects(rectsLeft);
                    break;

                case Dir.Down:
                    position.Y += speed * dt;
                    if (state == EnemyState.Frightened)
                    {
                        if (timerFrightened > 5)
                        {
                            enemyAnim.setSourceRects(frightenedRectsEnd);
                            break;
                        }
                        enemyAnim.setSourceRects(frightenedRects);
                    }
                    else
                        enemyAnim.setSourceRects(rectsDown);
                    break;

                case Dir.Up:
                    position.Y -= speed * dt;
                    if (state == EnemyState.Frightened)
                    {
                        if (timerFrightened > 5)
                        {
                            enemyAnim.setSourceRects(frightenedRectsEnd);
                            break;
                        }
                        enemyAnim.setSourceRects(frightenedRects);
                    }
                    else
                        enemyAnim.setSourceRects(rectsUp);
                    break;

                case Dir.None:
                    position = tileArray[(int)currentTile.X, (int)currentTile.Y].Position;
                    break;
            }
        }

```
</details>

<details>
    <summary>Sprites</summary>
This code is loading multiple textures (images) from the content pipeline . These textures include sprites for various game elements such as the player character, debugging indicators, and the Pac-Man logo , similar code is used in various parts of different files in the project.
```csharp
GeneralSprites1 = Content.Load<Texture2D>("SpriteSheets/GeneralSprites1");
            GeneralSprites2 = Content.Load<Texture2D>("SpriteSheets/GeneralSprites2");
            debuggingDot = Content.Load<Texture2D>("SpriteSheets/debuggingDot");
            debugLineX = Content.Load<Texture2D>("SpriteSheets/debugLineX");
            debugLineY = Content.Load<Texture2D>("SpriteSheets/debugLineY");
            playerDebugLineX = Content.Load<Texture2D>("SpriteSheets/playerDebugLineX");
            playerDebugLineY = Content.Load<Texture2D>("SpriteSheets/playerDebugLineY");
            pathfindingDebugLineX = Content.Load<Texture2D>("SpriteSheets/pathfindingDebugLineX");
            pathfindingDebugLineY = Content.Load<Texture2D>("SpriteSheets/pathfindingDebugLineY");
            Menu.setPacmanLogo = Content.Load<Texture2D>("SpriteSheets/pac-man-logo");

```
</details>

