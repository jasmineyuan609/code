# codeimport pygame
import random
import sys

# Initialize Pygame
pygame.init()

# Screen dimensions
SCREEN_WIDTH = 800
SCREEN_HEIGHT = 600

# Colors
WHITE = (255, 255, 255)
BLACK = (0, 0, 0)
RED = (255, 0, 0)
BLUE = (0, 0, 255)

# Setup screen
screen = pygame.display.set_mode((SCREEN_WIDTH, SCREEN_HEIGHT))
pygame.display.set_caption("Catch the Falling Object")

# Clock for controlling frame rate
clock = pygame.time.Clock()

# Player settings
player_width = 100
player_height = 20
player_x = (SCREEN_WIDTH - player_width) // 2
player_y = SCREEN_HEIGHT - player_height - 10
player_speed = 10

# Object settings
object_width = 30
object_height = 30
object_x = random.randint(0, SCREEN_WIDTH - object_width)
object_y = -object_height
object_speed = 5

# Score
score = 0
font = pygame.font.Font(None, 36)

# Game loop
running = True
while running:
    # Handle events
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False

    # Get pressed keys
    keys = pygame.key.get_pressed()
    if keys[pygame.K_LEFT] and player_x > 0:
        player_x -= player_speed
    if keys[pygame.K_RIGHT] and player_x < SCREEN_WIDTH - player_width:
        player_x += player_speed

    # Move the object
    object_y += object_speed
    if object_y > SCREEN_HEIGHT:
        object_y = -object_height
        object_x = random.randint(0, SCREEN_WIDTH - object_width)

    # Check for collision
    if (
        player_x < object_x < player_x + player_width or
        player_x < object_x + object_width < player_x + player_width
    ) and player_y < object_y + object_height < player_y + player_height:
        score += 1
        object_y = -object_height
        object_x = random.randint(0, SCREEN_WIDTH - object_width)

    # Draw everything
    screen.fill(WHITE)  # Clear screen
    pygame.draw.rect(screen, BLUE, (player_x, player_y, player_width, player_height))
    pygame.draw.rect(screen, RED, (object_x, object_y, object_width, object_height))

    # Display score
    score_text = font.render(f"Score: {score}", True, BLACK)
    screen.blit(score_text, (10, 10))

    # Update display
    pygame.display.flip()

    # Cap the frame rate
    clock.tick(30)

# Quit the game
pygame.quit()
sys.exit()
