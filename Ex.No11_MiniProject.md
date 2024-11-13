# Ex.No: 11  Mini Project 
### DATE:                                                                            
### REGISTER NUMBER : 212221240020
### AIM: 
To write a Python program to simulate the game using Pygame and AI-based obstacle avoidance.
### Algorithm:
#### Step 1: 
Set up Pygame to create the game window and handle game events.
#### Step 2: 
Define screen dimensions, player size, and colors, avoiding blue colors for obstacles to contrast with a sky-blue background.
#### Step 3: 
Load and scale a background image for a visually engaging environment.
#### Step 4: 
Set initial positions and speeds for the player and falling obstacles.
#### Step 5: 
Define a function to generate obstacles at random horizontal positions with colors that contrast against the background.
#### Step 6: 
Capture keyboard inputs to allow the player to move left and right on the screen.
#### Step 7: 
Check if any obstacles collide with the player. If a collision occurs, end the game.
#### Step 8: 
Increase the score for each obstacle successfully avoided. Gradually increase obstacle speed as the score rises.
#### Step 9: 
Render the current score on the screen.
#### Step 10: 
When the game ends, display the final score for a brief period before closing.

### Program:
```
import pygame
import random

# Initialize Pygame
pygame.init()

# Increased Screen settings
WIDTH, HEIGHT = 800, 600  # Larger dimensions for better reaction time
screen = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption("Dodge the Falling Obstacles")

# Colors
WHITE = (255, 255, 255)
PLAYER_COLOR = (0, 0, 150)  # Darker blue for player to stand out against the sky-blue background

# Background image
background = pygame.image.load("background.jpg")  # Ensure background.jpg is sky blue or your chosen image.
background = pygame.transform.scale(background, (WIDTH, HEIGHT))

# Game variables
player_size = 50
player_x = WIDTH // 2
player_y = HEIGHT - player_size - 10
player_speed = 8  # Player speed for faster movement

obstacle_width, obstacle_height = 50, 50
obstacle_speed = 4  # Slightly slower initial obstacle speed for the larger screen
obstacles = []
score = 0
clock = pygame.time.Clock()

# Font for score
font = pygame.font.Font(None, 36)

# Function to generate obstacles
def create_obstacle():
    x_pos = random.randint(0, WIDTH - obstacle_width)
    y_pos = 0
    # Ensure obstacles avoid blue-like colors by using a color range excluding blues
    color = (random.randint(150, 255), random.randint(0, 150), random.randint(0, 150))  # Avoid blues
    obstacles.append({"rect": pygame.Rect(x_pos, y_pos, obstacle_width, obstacle_height), "color": color})

# Main game loop
running = True
while running:
    screen.blit(background, (0, 0))  # Draw background image
    pygame.time.delay(30)

    # Event handling
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False

    # Player movement
    keys = pygame.key.get_pressed()
    if keys[pygame.K_LEFT] and player_x - player_speed > 0:
        player_x -= player_speed
    if keys[pygame.K_RIGHT] and player_x + player_speed + player_size < WIDTH:
        player_x += player_speed

    player_rect = pygame.Rect(player_x, player_y, player_size, player_size)

    # Obstacle generation
    if random.randint(1, 20) == 1:
        create_obstacle()

    # Move and draw obstacles
    for obstacle in obstacles[:]:
        obstacle["rect"].y += obstacle_speed
        if obstacle["rect"].colliderect(player_rect):
            running = False  # End game on collision
        if obstacle["rect"].y > HEIGHT:
            obstacles.remove(obstacle)
            score += 1  # Increase score when an obstacle is dodged

    # Draw player and obstacles
    pygame.draw.rect(screen, PLAYER_COLOR, player_rect)
    for obstacle in obstacles:
        pygame.draw.rect(screen, obstacle["color"], obstacle["rect"])

    # Display score
    score_text = font.render(f"Score: {score}", True, (0, 0, 0))
    screen.blit(score_text, (10, 10))

    # Increase difficulty over time
    if score % 10 == 0 and score != 0:
        obstacle_speed += 0.1

    pygame.display.flip()
    clock.tick(30)

# Game Over display
screen.fill(WHITE)
game_over_text = font.render(f"Game Over! Final Score: {score}", True, (0, 0, 0))
screen.blit(game_over_text, (WIDTH // 2 - 100, HEIGHT // 2))
pygame.display.flip()
pygame.time.delay(3000)

pygame.quit()
```
### Output:
![Screenshot 2024-11-07 205348](https://github.com/user-attachments/assets/4bee3333-64f7-44e0-afe5-9e7c0028ced5)
![Screenshot 2024-11-07 205436](https://github.com/user-attachments/assets/11666625-f5a5-4509-8369-4d805aa59f65)
![Screenshot 2024-11-07 205526](https://github.com/user-attachments/assets/f639a33b-66b6-438d-b74c-983c5acfd86d)

### Result:
Thus, the simple game was implemented using Pygame and basic AI techniques for obstacle avoidance.
