import pygame
import sys
import time
from random import choice
import random

pygame.init()

WIDTH = 500
HEIGHT = 500
FPS = 60
screen = pygame.display.set_mode((WIDTH, HEIGHT))
clock = pygame.time.Clock()

class Sprite:
    def __init__(self, x, y, w, h, img_path):
        self.rect = pygame.Rect(x, y, w, h)
        self.img_orig = pygame.image.load(img_path)
        self.img = pygame.transform.scale(self.img_orig, (self.rect.width, self.rect.height))
        self.direction = "None"

    def move(self):
        if self.direction == "right":
            self.rect.x += 5
        if self.direction == "left":
            self.rect.x -= 5
        if self.direction == "up":
            self.rect.y -= 5
        if self.direction == "down":
            self.rect.y += 5

    def draw(self):
        screen.blit(self.img, self.rect)

player = Sprite(0, 0, 55, 75, "steve.png")

enemy = Sprite(0, 0, 100, 100, "present.jpg")

enemy2 = Sprite(0, 0, 100, 100, "present.jpg")

font = pygame.font.SysFont('Arial', 30)

start_text_1 = font.render('Охота На Призраков', True,(0, 0, 255))
start_text_2 = font.render('Нажмите пробел, чтобы начать', True,(0, 0, 255))
end_text_1 = font.render('Конец', True,(0, 0, 255))
end_text_2 = font.render('Нажмите пробел, чтобы перезапустить игру', True,(0, 0, 255))

exit_img = pygame.image.load("exit-door.png")
exit_img = pygame.transform.scale(exit_img, (30, 30))

skin1 = Sprite(100, 225, 50, 50, "steve.png")
skin2 = Sprite(225, 225, 50, 50, "revolver.png")
skin3 = Sprite(350, 225, 50, 50, "rogue.png")

game_state = 0

wall = Sprite(0, 0, 500, 500, "kod.jpg")

while True:
    # screen.fill((255, 255, 255))
    wall.draw()
    if game_state == 3:
        for event in pygame.event.get():
            if event.type == pygame.MOUSEBUTTONDOWN:
                x, y = event.pos
                if skin2.rect.x < x < skin2.rect.right and skin2.rect.y < y < skin2.rect.bottom:
                    player.img_orig = pygame.image.load("revolver.png")
                    player.img = pygame.transform.scale(player.img_orig, (player.rect.width, player.rect.height))
                    game_state = 1
                if skin3.rect.x < x < skin3.rect.right and skin3.rect.y < y < skin3.rect.bottom:
                    player.img_orig = pygame.image.load("rogue.png")
                    player.img = pygame.transform.scale(player.img_orig, (player.rect.width, player.rect.height))
                    game_state = 1
                if skin1.rect.x < x <  skin1.rect.right and skin1.rect.y < y < skin1.rect.bottom:
                    game_state = 1

        skin1.draw()
        skin2.draw()
        skin3.draw()

    if game_state == 0:
        for event in pygame.event.get():
            if event.type == pygame.MOUSEBUTTONDOWN:
                x, y = event.pos
                if 10<x<40 and 10<y<40:
                    sys.exit()
            if event.type == pygame.QUIT:
                sys.exit()
            if event.type == pygame.KEYDOWN:
                if event.key == pygame.K_SPACE:
                    game_state = 3
        screen.blit(start_text_1, (140, 200))
        screen.blit(start_text_2, (50, 300))

    if game_state == 2:
        for event in pygame.event.get():
            if event.type == pygame.MOUSEBUTTONDOWN:
                x, y = event.pos
                if 10<x<40 and 10<y<40:
                    sys.exit()
            if event.type == pygame.QUIT:
                sys.exit()
            if event.type == pygame.KEYDOWN:
                if event.key == pygame.K_SPACE:
                    player.rect.x, player.rect.y, player.rect.width, player.rect.height = 400, 400, 55, 75
                    player.img = pygame.transform.scale(player.img_orig, (player.rect.width, player.rect.height))
                    enemy.rect.x, enemy.rect.y = 50, 50
                    game_state = 1
        screen.blit(end_text_1, (140, 200))
        screen.blit(end_text_2, (50, 300))

    if game_state == 1:
        for event in pygame.event.get():
            if event.type == pygame.MOUSEBUTTONDOWN:
                x, y = event.pos
                if 10<x<40 and 10<y<40:
                    sys.exit()
            if event.type == pygame.QUIT:
                sys.exit()
            if event.type == pygame.KEYDOWN:
                if event.key == pygame.K_d:
                    player.direction = "right"
                if event.key == pygame.K_a:
                    player.direction = "left"
                if event.key == pygame.K_w:
                    player.direction = "up"
                if event.key == pygame.K_s:
                    player.direction = "down"
            if event.type == pygame.KEYUP:
                player.direction = "none"

        player.move()

        if player.rect.colliderect(enemy.rect):
            player.rect.width += 3
            player.rect.height += 5
            player.img = pygame.transform.scale(player.img_orig, (player.rect.width, player.rect.height))
            enemy.rect.x = random.randint(0,450)
            enemy.rect.y = random.randint(0,450)

        elif player.rect.colliderect(enemy2.rect):
            player.rect.width += 3
            player.rect.height += 5
            player.img = pygame.transform.scale(player.img_orig, (player.rect.width, player.rect.height))
            enemy2.rect.x = random.randint(0,450)
            enemy2.rect.y = random.randint(0,450)

        if player.rect.width >= 200:
            game_state = 2

        player.draw()
        enemy.draw()
        enemy2.draw()

    screen.blit(exit_img, (10, 10))
    pygame.display.update()
    clock.tick(FPS)
