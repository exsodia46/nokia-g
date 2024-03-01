# import the pygame library and initialise the game engine
import pygame
import random
pygame.init()

# opens a new window, caption it "pong"
screen = pygame.display.set_mode((700,600))
pygame.display.set_caption("pong")

# here's the variable that runs our game loop
doExit = False



#ball variables
bx = 350 #xposition
by = 250 #yposition
bVx = 5
bVy = 5



class brick:
    def __init__(self, xpos, ypos):
        self.xpos = xpos
        self.ypos = ypos
        self.color = (random.randrange(100, 250),random.randrange(100, 250),random.randrange(100, 250))
        self.isDead = False
    def draw(self):
        if not self.isDead:
            pygame.draw.rect(screen, self.color, (self.xpos, self.ypos, 100, 50))
    def collide(self, ball_x, ball_y):
        if not self.isDead:
            if (ball_x + 20 > self.xpos and
                ball_x < self.xpos + 100 and
                ball_y + 20 > self.ypos and
                ball_y < self.ypos + 50):
                self.isDead = True
                return True
        return False
    
p1x = 100
p1y = 500

b1 = brick(40, 50)
b2 = brick(160, 50)
b3 = brick(280, 50)
b4 = brick(400, 50)

clock = pygame.time.Clock()

while not doExit:
    #event queue
    clock.tick(100)
   
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            doExit = True
           
           
    #game logic/physics
    keys = pygame.key.get_pressed()
    if keys[pygame.K_LEFT]:
        p1x-=5
    if keys[pygame.K_RIGHT]:
        p1x+=5
        
    if bx < p1x + 100 and by > p1y and by < p1y + 20 and bx > p1x:
        bVy *= -1
    
    if b1.collide(bx, by):
        bVy *= -1
    
    if b2.collide(bx, by):
        bVy *= -1
        
    if b3.collide(bx, by):
        bVy *= -1
        
    if b4.collide(bx, by):
        bVy *= -1
        
    if bx < 0 or bx + 20 > 700:
        bVx *=-1
       
    if by < 0 or by + 20 > 600:
        bVy *=-1
     
    
    
    
   
       
    #ball movement
    bx += bVx
    by += bVy
               
     
    #render section---------------------------
    screen.fill((0,0,0))
    b1.draw()
    b2.draw()
    b3.draw()
    b4.draw()
    
    pygame.draw.rect(screen, (255, 255, 255), (p1x, p1y, 100, 20), 1)
    pygame.draw.circle(screen, (255, 255, 255), (bx, by,), 15)
    #update the screen
    pygame.display.flip()
   
   
   
   
   
#end of game loop
   
pygame.quit()

