# Description of Ccoursework

## Description

The purpose of this game is the robot(green triangle) finding the marker(gray box) on the grid. The background of this game is the grid with obstacles and a marker.
The grid is 10*10 and obstacles and a marker is placed randomly as it compiles. 
I didn't use any specific algorithms(ex. dfs). I made a robot to turn right when it met the edge of the grid or obstacles. However, to prevent the infinite loop, while the forward function is used more than 40 times, the robot turn right randomly.


## Background

![image](https://user-images.githubusercontent.com/91853600/141026451-4dd2af42-315e-4ce5-9cb8-b3e53e1eaa20.png)


Draw the grid
```python
void drawgrid()
{
    background();
    for(int grid_offset = grid_start; grid_offset <= grid_end; grid_offset += grid_width)
    {
        drawLine(grid_start, grid_offset, grid_end, grid_offset);
        drawLine(grid_offset, grid_start,grid_offset, grid_end);
    }

}


```
Draw the robot
```python
void drawRobot(robot*aRobot)
{
    foreground();
    clear();
    aRobot->vertices = setDirection(aRobot->facing, aRobot->currentLocation);
    setColour(aRobot->objectColour);
    fillPolygon(3,aRobot->vertices.x, aRobot->vertices.y);
}
```

Create objects(Marker and obstacles)
```python
void createObjects(objectlist *objectlist, robot aRobot)
{
    for(int i =0; i<num_obstacles;i++)
    {
        objectlist->gridobject[i] = createObstacle(objectlist,aRobot);
    }
    objectlist->gridobject[num_obstacles] = createMarker(objectlist,aRobot);
}
```
Draw objects on the grid
```python
void drawObject(objectlist *objectlist)
{
    for(int i =0; i < *objectlist -> length; i++)
    {
        drawGridobject(objectlist->gridobject[i]);
    }
}
```
## foreground

### Robot and the direction
![image](https://user-images.githubusercontent.com/91853600/141026936-5b59d481-127c-4cab-8407-47aaf7809935.png)
![image](https://user-images.githubusercontent.com/91853600/141027043-5c912c25-098a-4c2a-85a1-61c93989c3ee.png)

Write down the vertices of the robot in each directions
```python
vertices setDirection(char facing[], coordinate currentLocation)
{
    vertices robotVertices;
    int xLocation = currentLocation.x;
    int yLocation = currentLocation.y;
    if(strcmp(facing,"north") ==0)
    {
        robotVertices.x[0] = xLocation;
        robotVertices.y[0] = yLocation + grid_width;
        robotVertices.x[1] = xLocation + grid_width/2;
        robotVertices.y[1] = yLocation;
        robotVertices.x[2] = xLocation + grid_width;
        robotVertices.y[2] = yLocation + grid_width;


    }
    else if(strcmp(facing,"south")==0)
    {
        robotVertices.x[0] = xLocation;
        robotVertices.y[0] = yLocation ;
        robotVertices.x[1] = xLocation + grid_width/2;
        robotVertices.y[1] = yLocation + grid_width;
        robotVertices.x[2] = xLocation + grid_width;
        robotVertices.y[2] = yLocation;
    }
    else if(strcmp(facing,"east") ==0)
    {
        robotVertices.x[0] = xLocation;
        robotVertices.y[0] = yLocation;
        robotVertices.x[1] = xLocation+grid_width;
        robotVertices.y[1] = yLocation+grid_width/2;
        robotVertices.x[2] = xLocation;
        robotVertices.y[2] = yLocation+grid_width;
    }
    else if(strcmp(facing,"west")==0)
    {
        robotVertices.x[0] = xLocation;
        robotVertices.y[0] = yLocation +grid_width/2;
        robotVertices.x[1] = xLocation +grid_width;
        robotVertices.y[1] = yLocation;
        robotVertices.x[2] = xLocation+grid_width;
        robotVertices.y[2] = yLocation+grid_width;
    }
    return robotVertices;

}
```

Draw robot
```python
void drawRobot(robot*aRobot)
{
    foreground();
    clear();
    aRobot->vertices = setDirection(aRobot->facing, aRobot->currentLocation);
    setColour(aRobot->objectColour);
    fillPolygon(3,aRobot->vertices.x, aRobot->vertices.y);
}
```

### function for robot direciton

Robot left
```python
void left(robot*aRobot)
{
    if(strcmp(aRobot->facing, "north") ==0)
      strcpy(aRobot->facing, "west");
    else if(strcmp(aRobot->facing, "east") ==0)
      strcpy(aRobot->facing, "north");
    else if(strcmp(aRobot->facing, "south") ==0)
      strcpy(aRobot->facing, "east");
    else if(strcmp(aRobot->facing, "west") ==0)
      strcpy(aRobot->facing, "south");
    drawRobot(aRobot);
}
```
Robot right
```python
void right(robot*aRobot)
{
    if(strcmp(aRobot->facing, "north") ==0)
      strcpy(aRobot->facing, "east");
    else if (strcmp(aRobot->facing, "east") ==0)
      strcpy(aRobot->facing, "south");
    else if (strcmp(aRobot->facing, "south") ==0)
      strcpy(aRobot->facing, "west");
    else if (strcmp(aRobot->facing, "west") ==0)
      strcpy(aRobot->facing, "north");
    drawRobot(aRobot);
}
```
Robot forward
```python
void forward(robot *aRobot, objectlist listofobject)
{
    if(canMoveForward(*aRobot,listofobject)==1)
    {
        aRobot -> currentLocation = getNewLocation(aRobot->currentLocation,aRobot->facing);
        drawRobot(aRobot);
    }
}
```
### How to understand the code written down in the terminal
![image](https://user-images.githubusercontent.com/91853600/141027043-5c912c25-098a-4c2a-85a1-61c93989c3ee.png)
![image](https://user-images.githubusercontent.com/91853600/141026936-5b59d481-127c-4cab-8407-47aaf7809935.png)
```python
int main(int argc, char **argv)
{
    srand(time(NULL));
    coordinate initialLocation;
    char *initialDirection;
    if(argc ==4)
    {
        initialLocation.x =(atoi(argv[1])-1)*grid_width+grid_start;
        initialLocation.y =(atoi(argv[2])-1)*grid_width+grid_start;
        initialDirection = argv[3];
    }
    else
    {
        initialLocation = generateCoordinate();
        getRandomDirection(initialDirection);
```
### The command needed to compile and run the program
./coursework x y direction | java -jar drawapp.jar

write down the x y which are the start position on the grid with a direction("south", "north", "west", "east")
## algorithm
I didn't use any specific algorithm. I made a robot to move right if it met any obstacles or the edge of the grid. However, to prevent the infinite loop, I added to turn right randomly after the forward function used 40times.
```python
void algorithm(robot *aRobot, objectlist listofobject)
{
    int nums_forward =0;
    
    while(!atMarker(*aRobot, listofobject))
    {
        
        forward(aRobot, listofobject);
        nums_forward +=1;
        while(!canMoveForward(*aRobot, listofobject))
        {
            
            
            right(aRobot);
            
        }
        for(int i=0; i<1000 ; i+=rand()%10)
        {
            if(nums_forward ==40 + i)
            right(aRobot);
            
        }
        
        sleep(500);
        
        
    }
    
}
```


