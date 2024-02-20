import pygame
import sys

# Initialize Pygame
pygame.init()

# Set up the screen
WIDTH, HEIGHT = 800, 600
screen = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption("Admin Panel")

# Define colors
WHITE = (255, 255, 255)
BLACK = (0, 0, 0)
GRAY = (150, 150, 150)

# Define fonts
font = pygame.font.SysFont(None, 36)

# Define admin panel buttons
god_mode_button = pygame.Rect(50, 50, 200, 50)
creator_perks_button = pygame.Rect(50, 150, 200, 50)

# Define your username
YOUR_USERNAME = "Your_User"

# Define game state variables
god_mode_enabled = False
creator_perks_enabled = False

# Authentication flag
authenticated = False

# Main loop
while True:
    screen.fill(WHITE)

    # Draw buttons
    pygame.draw.rect(screen, GRAY, god_mode_button)
    pygame.draw.rect(screen, GRAY, creator_perks_button)

    # Draw button text
    god_mode_text = font.render("God Mode: " + str(god_mode_enabled), True, BLACK)
    screen.blit(god_mode_text, (50, 50))

    creator_perks_text = font.render("Creator Perks: " + str(creator_perks_enabled), True, BLACK)
    screen.blit(creator_perks_text, (50, 150))

    # Handle events
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            pygame.quit()
            sys.exit()
        elif event.type == pygame.MOUSEBUTTONDOWN:
            if event.button == 1:
                # Check if the username is authenticated
                if authenticated:
                    # Check if buttons are clicked
                    if god_mode_button.collidepoint(event.pos):
                        god_mode_enabled = not god_mode_enabled
                    elif creator_perks_button.collidepoint(event.pos):
                        creator_perks_enabled = not creator_perks_enabled
                else:
                    print("Access denied!")

        elif event.type == pygame.KEYDOWN:
            if not authenticated and event.key == pygame.K_u and pygame.key.get_mods() & pygame.KMOD_LCTRL:
                # Authentication process: LCtrl + U
                username_input = input("Enter your username: ")
                if username_input == YOUR_USERNAME:
                    authenticated = True
                    print("Access granted!")
                else:
                    print("Access denied!")

    # Update the display
    pygame.display.flip()
