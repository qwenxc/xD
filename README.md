from pygame import *
from random import randint


font.init()
font = font.SysFont('Arial', 35)
lose1 = font.render('PLAYER 1 LOSE!', True, (180, 0, 0))
lose2 = font.render('PLAYER 2 LOSE!', True, (180, 0, 0))

img_racket='racket.png'
img_ball = 'tenis_ball.png'

class GSprite(sprite.Sprite):
    def __init__(self,pl_image,pl_x,pl_y,size_x,size_y,pl_speed):
        super().__init__()
        self.image = transform.scale(image.load(pl_image),(size_x,size_y))
        self.rect = self.image.get_rect()
        self.speed = pl_speed
        self.rect.x = pl_x
        self.rect.y = pl_y
    def reset(self):
        window.blit(self.image,self.rect)

class Player(GSprite):
    def update(self):
        keys = key.get_pressed()
        if keys[K_w] and self.rect.y >= 20:
            self.rect.y -= self.speed
        if keys[K_s] and self.rect.y <= 400:
            self.rect.y += self.speed

class Player1(GSprite):
    def update(self):
        keys = key.get_pressed()
        if keys[K_UP] and self.rect.y >= 20:
            self.rect.y -= self.speed
        if keys[K_DOWN] and self.rect.y <= 400:
            self.rect.y += self.speed
speed_x = 3
speed_y = 3

class Ball(GSprite):
    def update(self):
        global speed_x,speed_y
        
        self.rect.x += speed_x
        self.rect.y -= speed_y
        if self.rect.y>470:
            speed_y *= -1
        if self.rect.y<30:
            speed_y *= -1
        if sprite.collide_rect(player,ball):
            speed_x *= -1
        if sprite.collide_rect(player1,ball):
            speed_x *= -1
            
        



player = Player(img_racket,650,250,30,90,4)
player1 = Player1(img_racket,50,250,30,90,4)
ball= Ball(img_ball,350,250,30,30,3)



win_height = 500
win_width = 700
display.set_caption('anetie')
window = display.set_mode((win_width,win_height))
back = transform.scale(image.load('r.jpg'),(win_width,win_height))

yea = True
finish = False
while yea:
    for e in event.get():
        if e.type == QUIT:
            yea = False

    if ball.rect.x> 700:
        window.blit(lose1,(240,270))
        finish = True

    if ball.rect.x<0:
        window.blit(lose2,(240,270))
        finish = True


    if not finish:
        window.blit(back,(0,0))
        player.update()
        player.reset()
        player1.update()
        player1.reset()
        ball.update()
        ball.reset()







    display.update()
    time.delay(10)
display.update()
