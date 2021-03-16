# Snake Tutorial Python

import random
import pygame
import tkinter as tk
from tkinter import messagebox

# Draw the cube as well as move the cube.
class cube(object):
    rows = 20
    w = 500

    def __init__(self, start, dirnx = 1, dirny = 0, color=(255, 0, 0)):
        self.pos = start
        self.dirnx = 1  # This is to allow the snake to start moving at the start otherwise you will need to press a key stroke in order for it to start moving.
        self.dirny = 0 # If you also put this as 1, then the snake will start out moving diagonally
        self.color = color

    # Changes the position of the snake.
    def move(self, dirnx, dirny):
        self.dirnx = dirnx    # Direction which it will move towards
        self.dirny = dirny  # Direction which it will move towards
        self.pos = (self.pos[0] + self.dirnx, self.pos[1] + self.dirny) # This will do the actual change in position of the snake which will in turn cause the snake to move.

    # Draw each cube of snake.
    def draw(self, surface, eyes = False):  # This do the actual drawing of the cubes with a default argument set in place.
        dis = self.w // self.rows   # This gives the distance between each grid lines.
        i = self.pos[0] # This obtains the column it is at. Remember that we are not obtaining the position via coordinates! But by rows and columns in the grid.
        j = self.pos[1] # This obtains the row which it is at.

        pygame.draw.rect(surface, self.color, (i * dis + 1, j * dis + 1, dis - 2, dis - 2)) # This calls the method in pygame to draw a rectangle with a color and draws it at starting at the coordinate pixel stated and width and height stated.

        # Draw the eyes if it is really the head.
        if eyes:    # Check if eyes have boolean True.
            centre = dis // 2
            radius = 3
            circleMiddle = (i * dis + centre - radius, j * dis + 8)
            circleMiddle2 = (i * dis + dis - radius * 2, j * dis + 8)
            pygame.draw.circle(surface, (0, 0, 0), circleMiddle, radius)
            pygame.draw.circle(surface, (0, 0, 0), circleMiddle2, radius)


class snake(object):
    body = []   # This will store the position which the snake had been through in a list. But this list will not increase in its number of items will match the number of cube that makes up the snake.
    turns = {}  # This dictionary contains the position which the snake have turned.

    def __init__(self, color, pos):
        self.color = color
        self.head = cube(pos)   # This self.head now becomes an object of cube as it tries to access the cube class. And this will pass only a single argument to class cube and that is ok since the cube class already has the other variables assigned their default values if they do not have arguements passed to them.
        self.body.append(self.head) # This will put the object of cube self.head into the list body inside this class of snake.
        self.dirnx = 1
        self.dirny = 0

    # Movement of snake
    def move(self):
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                pygame.quit()

            # This part check for the key stroke.
            keys = pygame.key.get_pressed() # This just checks for which key have been depressed! This is returned in the form of a list... Hence we can use the for loop below to iterate through.

            for key in keys:    # Now here we are iterating through the keys.
                if keys[pygame.K_LEFT]: # This has the highest priority. What this does is that it access the current position when key stroke is pressed and then we put it into the dictionary so called as turn.
                    self.dirnx = -1
                    self.dirny = 0
                    self.turns[self.head.pos[:]] = [self.dirnx, self.dirny] # Notice the syntax of self.head, it is an object and we are trying to access its method which is so call .pos and the [:] will retrieve all of the present values... And this .pos will be stored as a dictionary key and assigned the values as dictated by the right hand side of the operator.

                elif keys[pygame.K_RIGHT]:  # This has the second highest priority.
                    self.dirnx = 1
                    self.dirny = 0
                    self.turns[self.head.pos[:]] = [self.dirnx, self.dirny]

                elif keys[pygame.K_UP]: # This has the third highest priority.
                    self.dirnx = 0
                    self.dirny = -1
                    self.turns[self.head.pos[:]] = [self.dirnx, self.dirny]

                elif keys[pygame.K_DOWN]:   # This has the fourth highest priority.
                    self.dirnx = 0
                    self.dirny = 1
                    self.turns[self.head.pos[:]] = [self.dirnx, self.dirny]

        # Iterates through the body list of the position which the snake has moved towards.
        for i, c in enumerate(self.body):   # This iterates through the list of position which the snake has been through.
            p = c.pos[:]    # This syntax is like so because remember that we have inserted the object self.head into the list and thus every item of the list will be the object self.head and this belongs to the cube class. And this item object self.head has no meaning unless you obtained the stored value via the cube class's method so called as .pos... Thus now c is the iterable vairable which will contain that object.

            # For Testing purpose
            # print(p)

            # This part check for the turning of snake.
            if p in self.turns: # Checks for the presence of the position which the snake have been through (body list) in the position which the snake make the turn which is inside the dictionary so called (turn).
                turn = self.turns[p]    # If there is presence of turn inside the body, then we will assign that position from the dictionary to this variable called turn.
                c.move(turn[0], turn[1])    # Then we will have the c object (Yes remember this is currently the object of cube) access the move method inside cube. And assign the first and second item of the list as varible to the method.
                if i == len(self.body) - 1: # Now we shall spot the tail of the snake with this code. This is because we MUST remove the turn position once the whole body of the snake have passed through this turning point otherwise when the next time the snake pass through this point, it will turn again even without your key stroke input.
                    self.turns.pop(p)   # Remove the dictionary with the key of p.

            # If the snake is not turing then check if it is hitting any boundary...
            else:
                if c.dirnx == -1 and c.pos[0] <= 0: # Moving left hitting left boundary
                    c.pos = (c.rows - 1, c.pos[1])
                elif c.dirnx == 1 and c.pos[0] >= c.rows - 1:   # Moving right and hitting right boundary
                    c.pos = (0, c.pos[1])
                elif c.dirny == 1 and c.pos[1] >= c.rows - 1:   # Moving down and hitting bottom boundary
                    c.pos = (c.pos[0], 0)
                elif c.dirny == -1 and c.pos[1] <= 0:   # Moving up and hitting top boundary
                    c.pos = (c.pos[0], c.rows - 1)
                else:   # If the snake is not hitting boundary or turning at all...
                    c.move(c.dirnx, c.dirny)    # We call the method of cube class. To have it move as it should be moving... Continue in its direction of motion.

    # This basically reset the state of the snake, to the original position and its original length of a single cube.
    def reset(self, pos):
        self.head = cube(pos)
        self.body = []
        self.body.append(self.head)
        self.turns = {}
        self.dirnx = 0
        self.dirny = 1

    def addCube(self):
        tail = self.body[-1]    # We take the last position of the snake's body which is the tail.
        dx, dy = tail.dirnx, tail.dirny # And don't forget that each item of the body's list are object of cube and that we can access the data attribute via dirnx and dirny.

        # To check which direction is the tail moving towards.
        if dx == 1 and dy == 0: # Tail moving to the right.
            self.body.append(cube((tail.pos[0] - 1, tail.pos[1])))  # This will create a cube object with the coordintates being 1 position to the left of the tail's position such that it will be trailing the cube tail.
        elif dx == -1 and dy == 0:  # Tail moving to the left.
            self.body.append(cube((tail.pos[0] + 1, tail.pos[1])))  # Create cube object 1 position to the right of tail making sure its trailing.
        elif dx == 0 and dy == 1:   # Tail moving downwards.
            self.body.append(cube((tail.pos[0], tail.pos[1] - 1)))  # Create cube object 1 position on top of tail making sure its trailing.
        elif dx == 0 and dy == -1:  # Tail moving upwards.
            self.body.append(cube((tail.pos[0], tail.pos[1] + 1)))  # Create cube object 1 position below the tail making sure its trailing.

        # Set the direction of the new tail.
        self.body[-1].dirnx = dx    # Now the new cube will be the new tail. Thus we have to give it a direction which is equal to the direction of the previous tail. Otherwise it won't move.
        self.body[-1].dirny = dy    # Now the new cube will be the new tail. Thus we have to give it a direction which is equal to the direction of the previous tail. Otherwise it won't move.

    # Draw the snake's head and body.
    def draw(self, surface):
        for i, c in enumerate(self.body):
            if i == 0:  # When i == 0, it means it is the head.
                c.draw(surface, True)   # We have to draw the head in this case so we pass True as second argument to the draw method of the cube class.
            else:   # Else it is just the body of snake.
                c.draw(surface) # Just draw a generic cube.

# This just draws the grid. Simple as it is...
def drawGrid(w, rows, surface):
    sizeBtwn = w // rows

    x = 0
    y = 0
    for l in range(rows):
        x = x + sizeBtwn
        y = y + sizeBtwn

        pygame.draw.line(surface, (255, 255, 255), (x, 0), (x, w))
        pygame.draw.line(surface, (255, 255, 255), (0, y), (w, y))

# Do the drawing of the background, grid and the snake...
def redrawWindow(surface):
    global rows, width, s, snack
    surface.fill((0, 0, 0)) # Fill the background with black.
    s.draw(surface)
    snack.draw(surface)
    drawGrid(width, rows, surface)
    pygame.display.update()

# Generates the snack at random location.
def randomSnack(rows, item):
    positions = item.body

    while True:
        x = random.randrange(rows)
        y = random.randrange(rows)
        if len(list(filter(lambda z: z.pos == (x, y), positions))) > 0:
            continue
        else:
            break

    return (x, y)

# Displays the game over message.
def message_box(subject, content):
    root = tk.Tk()
    root.attributes("-topmost", True)
    root.withdraw()
    messagebox.showinfo(subject, content)
    try:
        root.destroy()
    except:
        pass

# This is the main function.
def main():
    global width, rows, s, snack    # These are the variables with global perspective...
    width = 500 # We set the width and the height of the window to 500px for both.
    rows = 20   # We want 20 rows and columns for the gridlines to be dividing into...
    win = pygame.display.set_mode((width, width))   # Storing this as a variable... Moreover this cannot be placed here since it needs to be in the main while loop
    s = snake((255, 0, 0), (10, 10))    # Now snake is a class. And s will be an object of it with the color and position as stated.
    snack = cube(randomSnack(rows, s), color=(0, 255, 0))   # Cube is a class.
    flag = True # Just a variable storing the Boolean True.

    clock = pygame.time.Clock() # This is assigned to the variable clock.

    list_alpha = [] # This is just an empty list for testing

    # Main loop
    while flag:
        pygame.time.delay(50)   # Just give a delay of a certain time before proceeding to the next code.
        clock.tick(10)  # We are accessing the method of pygame...
        s.move()    # We are accessing the method of the class snake. And This changes the position with each iteration.

        # Check for collision of the snack
        if s.body[0].pos == snack.pos:  # Now if the snack position and the head (.body[0]) coincide then the snake have eaten the snack.
            s.addCube() # We execute the addCube() method of snake class.
            snack = cube(randomSnack(rows, s), color=(0, 255, 0))

        # Check for collision of body with the head.
        if s.body[0].pos in list(map(lambda z: z.pos, s.body[1:])):
            print('Score: ', len(s.body))
            message_box('You Lost!', 'Play again...')
            s.reset((10, 10))

        redrawWindow(win)   # Here win is used as the surface.

        # This part is for testing purpose. Uncomment out to test it out yourself.
        # a = s.body  # This basically obtains the body list which we can access via the object of the class snake. Because the items stored within this list is all objects we are gonna have to convert them.
        # for l in a: # We loop through the list.
            # b = l.pos[:]    # We use the pos[:] method to obtain the position attribute of the object at that time.
            # list_alpha.append(b)    # We use this to build up the empty list we created outside of the main while loop.
        # print(list_alpha)   # We will print out the entire list after every iteration of the main while loop.

    # pass # This is unnecessary...

main()  # We call the main function right here.
