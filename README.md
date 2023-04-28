import pygame
import random

pygame.init()

width = 500
height = 500

# Colors
white = (255, 255, 255)
black = (0, 0, 0)
red = (255, 0, 0)

# Create the screen
screen = pygame.display.set_mode((width, height))
pygame.display.set_caption("Snake Game")

# Set the clock
clock = pygame.time.Clock()

# Set the font
font_style = pygame.font.SysFont(None, 30)


def message(msg, color):
    """Function to display messages on the screen"""
    text = font_style.render(msg, True, color)
    screen.blit(text, [width / 6, height / 3])


def snake(block_size, snake_list):
    """Function to draw the snake"""
    for x in snake_list:
        pygame.draw.rect(screen, black, [x[0], x[1], block_size, block_size])


def gameLoop():
    """Main game loop"""
    game_over = False
    game_close = False

    # Starting position of the snake
    x1 = width / 2
    y1 = height / 2

    # Movement of the snake
    x1_change = 0
    y1_change = 0

    # Length of the snake
    snake_List = []
    Length_of_snake = 1

    # Starting position of the food
    foodx = round(random.randrange(0, width - block_size) / 10.0) * 10.0
    foody = round(random.randrange(0, height - block_size) / 10.0) * 10.0

    # Game loop
    while not game_over:

        # Game over screen
        while game_close == True:
            screen.fill(white)
            message("You Lost! Press Q-Quit or C-Play Again", red)
            pygame.display.update()

            for event in pygame.event.get():
                if event.type == pygame.KEYDOWN:
                    if event.key == pygame.K_q:
                        game_over = True
                        game_close = False
                    if event.key == pygame.K_c:
                        gameLoop()

        # Event loop
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                game_over = True
            if event.type == pygame.KEYDOWN:
                if event.key == pygame.K_LEFT:
                    x1_change = -block_size
                    y1_change = 0
                elif event.key == pygame.K_RIGHT:
                    x1_change = block_size
                    y1_change = 0
                elif event.key == pygame.K_UP:
                    y1_change = -block_size
                    x1_change = 0
                elif event.key == pygame.K_DOWN:
                    y1_change = block_size
                    x1_change = 0

        # Check if the snake goes out of bounds
        if x1 >= width or x1 < 0 or y1 >= height or y1 < 0:
            game_close = True

        # Update the position of the snake
        x1 += x1_change
        y1 += y1_change

        # Draw the snake
        screen.fill(white)
        pygame.draw.rect(screen, red, [foodx, foody, block_size, block_size])
        snake_Head = []
        snake_Head.append(x1)
        snake_Head.append(y1)
        snake_List.append(snake_Head)
        if len(snake_List) > Length_of_snake:
            del snake
