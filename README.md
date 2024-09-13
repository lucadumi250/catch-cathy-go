# catch-cathy-go

import pygame 
from turtle import *
from random import randint
from time import *

black = (0,0,0)
yellow = (255, 255, 153)
gay = (96,96,96)

pygame.init()
screen = pygame.display.set_mode((500, 500))
screen.fill(black)
frame_refresh = pygame.time.Clock()

text = pygame.font.Font(None, 50)
test = text.render('Cacth cacthy go', True, (255, 255, 153))
screen.blit(test, (120, 140))

class Button():
    def __init__(self, x=0, y=0, L=10, l=10, color=None):
        self.rect = pygame.Rect(x, y, L,l)
        self.fill_color = color
    def set_text(self, text, fsize=10, text_color=black):
        self.text = text
        self.image = pygame.font.Font(None, fsize).render(text, True, text_color)
    def draw(self, shift_x=0, shift_y=0):
        pygame.draw.rect(screen, self.fill_color, self.rect)
        screen.blit(self.image, (self.rect.x + shift_x, self.rect.y + shift_y))
    def click(self):
        if self.rect.collidepoint(pygame.mouse.get_pos()) and pygame.mouse.get_pressed()[0]:
            return True
        else:
            return False

rect =Button(105, 200, 305, 50, gay)
rect.set_text('Play',75)

quitbtn=Button(105, 300, 305, 50, gay)
quitbtn.set_text('Quit', 75)

def background():
    color('cyan')
    speed(1000)
    penup()
    goto(-500, -500)
    pendown()
    begin_fill()
    for i in range(4):
        forward(1000)
        left(90)
    end_fill()
    pensize(5)
    color('green', 'lime')
    penup()
    goto(-390, -390)
    pendown()
    begin_fill()
    for i in range(4):
        forward(780)
        left(90)
    end_fill()

while 1:
    rect.draw(100,0)
    click = rect.click()

    quitbtn.draw(100,0)
    qclick = quitbtn.click()

    if click == True:
        total = 0
        while total != 1:
            background()
            hideturtle()

            movement_x = randint(-390, 390)
            movement_y = randint(-390, 390)

            class Sprite(Turtle):
                def __init__(self, x, y, step=10, shape = 'circle', color= 'black'):
                    super().__init__()
                    self.penup()
                    self.goto(x, y)
                    self.step = step
                    self.shape(shape)
                    self.color(color)
                def is_collide(self, Sprite):
                    dist = self.distance(Sprite.xcor(), Sprite.ycor())
                    if dist < 30:
                        return True 
                    else:
                        return False
                def move_up(self):
                    self.goto(self.xcor(), self.ycor() + self.step)
                    if self.ycor() > 390:
                        self.goto(self.xcor(), self.ycor() - 10)
                def move_down(self):
                    self.goto(self.xcor(), self.ycor() - self.step)
                    if self.ycor() < -390:
                        self.goto(self.xcor(), self.ycor() + 10)
                def move_left(self):
                    self.goto(self.xcor() - self.step, self.ycor())
                    if self.xcor() < -390:
                        self.goto(self.xcor() + 10, self.ycor())
                def move_right(self):
                    self.goto(self.xcor() + self.step, self.ycor())
                    if self.xcor() > 390:
                        self.goto(self.xcor() - 10, self.ycor())
                def rand_move(self):
                    x= randint(-200, 200)
                    y= randint(-200, 200)
                    self.goto(x, y)

            t = Turtle()
            t.goto(0,0)
            time = 100
            n= 100
            for i in range(n):
                t.write(time, font=('Arial', 14, 'normal'))
                sleep(1)
                t.clear()
                time -= 1

            player1 = Sprite(380, 380, 10, 'turtle', 'green')

            player2 = Sprite(-380, -380, 10,'turtle', 'darkgreen')

            scr = player1.getscreen()
            scr.listen()

            scre = player2.getscreen()
            scre.listen()

            scr.onkey(player1.move_up, 'w')
            scr.onkey(player1.move_down, 's')
            scr.onkey(player1.move_left, 'a')
            scr.onkey(player1.move_right, 'd')

            scre.onkey(player2.move_up, 'i')
            scre.onkey(player2.move_down, 'k')
            scre.onkey(player2.move_left, 'j')
            scre.onkey(player2.move_right, 'l') 
            exitonclick()
            

            if player1.is_collide(player2):
                player1.hideturtle()

    if qclick == True:
        pygame.quit()

    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            pygame.quit()

    pygame.display.update()
    frame_refresh.tick(60)
