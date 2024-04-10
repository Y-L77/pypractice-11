import pygame #i try use more comments this project
import random

redteamgoals = 0
blueteamgoals = 0



WIDTH, HEIGHT = 1800, 1000
PLAYER_WIDTH, PLAYER_HEIGHT = 50, 200
BALL_SIZE = 50
TICKRATE = 444


pygame.mixer.init()
pygame.mixer.music.load("brutal calamity.mp3")

#color
BLACK = (0, 0, 0)
BLUE = (0, 0, 255)
RED = (255, 0, 0)
WHITE = (255, 255, 255)

#initialization
pygame.init()
WINDOW = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption("pong")
clock = pygame.time.Clock()
running = True

player = pygame.Rect((WIDTH - 100, HEIGHT // 2 - PLAYER_HEIGHT // 2, PLAYER_WIDTH, PLAYER_HEIGHT))
player2 = pygame.Rect((100, HEIGHT // 2 - PLAYER_HEIGHT // 2, PLAYER_WIDTH, PLAYER_HEIGHT))
pongball = pygame.Rect((WIDTH // 2 - BALL_SIZE // 2, HEIGHT // 2 - BALL_SIZE // 2, BALL_SIZE, BALL_SIZE))
move_choices = [(2, 2), (1, 3), (3, 1)]

if not pygame.mixer.music.get_busy():
    pygame.mixer.music.play()

timer = 0


while running:
    WINDOW.fill(BLACK)

    
    pygame.draw.rect(WINDOW, BLUE, player2)
    pygame.draw.rect(WINDOW, RED, player)
    pygame.draw.rect(WINDOW, WHITE, pongball)

    
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False

    #pong platformers
    key = pygame.key.get_pressed()
    if key[pygame.K_q]:
        player2.y -= 5
    elif key[pygame.K_a]:
        player2.y += 5
    if key[pygame.K_p]:
        player.y -= 5
    elif key[pygame.K_l]:
        player.y += 5

    
    pongball.move_ip(random.choice(move_choices)) #pongball move

    if pongball.top <= 0 or pongball.bottom >= HEIGHT:
        move_choices = [(move[0], -move[1]) for move in move_choices]
    if pongball.colliderect(player) or pongball.colliderect(player2): #collisions with the height
        move_choices = [(-move[0], move[1]) for move in move_choices]

    #scoring system
    if pongball.x <= 100:
        print(f"blue:{blueteamgoals} - red:{redteamgoals}")
        pongball.x = 900
        redteamgoals += 1

    elif pongball.x >= 1703:
        print(f"blue:{blueteamgoals} - red:{redteamgoals}")
        pongball.x = 900
        blueteamgoals += 1

    if redteamgoals == 10:
        print(f"RED TEAM WINS SCORE IS {redteamgoals} TO {blueteamgoals}")
        quit()

    elif blueteamgoals == 10:
        print(f"BLUE TEAM WINS SCORE IS {blueteamgoals} TO {redteamgoals}")
        quit()







    # Update display
    pygame.display.flip()
    clock.tick(TICKRATE)

pygame.quit()
