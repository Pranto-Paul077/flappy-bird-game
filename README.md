# flappy-bird-game
import pygame
import random

# Initialize pygame
pygame.init()

# Constants
WIDTH, HEIGHT = 400, 600
GRAVITY = 0.5
JUMP_STRENGTH = -10
PIPE_WIDTH = 60
PIPE_GAP = 180
PIPE_VELOCITY = -3
BIRD_X = 50
FONT = pygame.font.Font(None, 36)

# Colors
WHITE = (255, 255, 255)
GREEN = (0, 255, 0)
BLUE = (0, 0, 255)
BLACK = (0, 0, 0)

# Screen setup
screen = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption("Flappy Bird")

# Load images
bird_img = pygame.image.load("bird.png")
bird_img = pygame.transform.scale(bird_img, (40, 30))
background_img = pygame.image.load("background.png")
background_img = pygame.transform.scale(background_img, (WIDTH, HEIGHT))

# Bird class
class Bird:
    def __init__(self):
        self.y = HEIGHT // 2
        self.velocity = 0

    def update(self):
        self.velocity += GRAVITY
        self.y += self.velocity
        if self.y > HEIGHT:
            self.y = HEIGHT
            self.velocity = 0

    def jump(self):
        self.velocity = JUMP_STRENGTH

    def draw(self):
        screen.blit(bird_img, (BIRD_X, self.y))



# Pipe class
class Pipe:
    def __init__(self, x):
        self.x = x
        self.height = random.randint(100, 400)

    def update(self):
        self.x += PIPE_VELOCITY
        if self.x + PIPE_WIDTH < 0:
            self.x = WIDTH
            self.height = random.randint(100, 400)

    def draw(self):
        pygame.draw.rect(screen, GREEN, (self.x, 0, PIPE_WIDTH, self.height))
        pygame.draw.rect(screen, GREEN, (self.x, self.height + PIPE_GAP, PIPE_WIDTH, HEIGHT))

# Game loop
bird = Bird()
pipes = [Pipe(WIDTH + i * 200) for i in range(3)]
running = True
clock = pygame.time.Clock()
score = 0

while running:
    screen.blit(background_img, (0, 0))
    
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False
        if event.type == pygame.KEYDOWN and event.key == pygame.K_SPACE:
            bird.jump()
    
    bird.update()
    bird.draw()
    
    for pipe in pipes:
        pipe.update()
        pipe.draw()
        if pipe.x == BIRD_X:
            score += 1
    
    score_text = FONT.render(f"Score: {score}", True, BLACK)
    screen.blit(score_text, (10, 10))
    
    pygame.display.update()
    clock.tick(30)

pygame.quit()



