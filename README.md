import pygame
import random

pygame.init()

WIDTH = 600
HEIGHT = 400
screen = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption("Catch the Block!")

WHITE = (255, 255, 255)
BLUE = (0, 0, 255)
RED = (255, 0, 0)

player_width = 80
player_height = 20
player_x = WIDTH // 2 - player_width // 2
player_y = HEIGHT - 40
player_speed = 7

# Falling block settings
block_size = 30
block_x = random.randint(0, WIDTH - block_size)
block_y = 0
block_speed = 5


score = 0
font = pygame.font.SysFont(None, 36)

clock = pygame.time.Clock()
running = True

while running:
    clock.tick(60)
    screen.fill(WHITE)

    
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False

    keys = pygame.key.get_pressed()
    if keys[pygame.K_LEFT] and player_x > 0:
        player_x -= player_speed
    if keys[pygame.K_RIGHT] and player_x < WIDTH - player_width:
        player_x += player_speed

    block_y += block_speed

    if (block_y + block_size >= player_y and
        player_x < block_x + block_size and
        player_x + player_width > block_x):

        score += 1
        block_y = 0
        block_x = random.randint(0, WIDTH - block_size)

    if block_y > HEIGHT:
        block_y = 0
        block_x = random.randint(0, WIDTH - block_size)

    pygame.draw.rect(screen, BLUE, (player_x, player_y, player_width, player_height))
    pygame.draw.rect(screen, RED, (block_x, block_y, block_size, block_size))

    score_text = font.render(f"Score: {score}", True, (0, 0, 0))
    screen.blit(score_text, (10, 10))

    pygame.display.update()

pygame.quit()

