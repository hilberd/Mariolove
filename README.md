# Mariolove
mario game
import pygame
import sys

# Ініціалізація Pygame
pygame.init()

# Параметри вікна
WIDTH, HEIGHT = 800, 400
screen = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption("Mini Mario")

# Кольори
WHITE = (255, 255, 255)
BLACK = (0, 0, 0)
BLUE = (0, 0, 255)
RED = (255, 0, 0)

# FPS і таймер
FPS = 60
clock = pygame.time.Clock()

# Гравець
player_size = 40
player_x = 50
player_y = HEIGHT - player_size - 10
player_speed = 5
player_jump = -15
player_velocity_y = 0
on_ground = True

# Перешкоди
obstacle_width = 40
obstacle_height = 40
obstacle_x = WIDTH
obstacle_y = HEIGHT - obstacle_height - 10
obstacle_speed = 5

# Гравець рахунок
score = 0
font = pygame.font.SysFont(None, 35)

def display_score():
    score_text = font.render(f"Score: {score}", True, BLACK)
    screen.blit(score_text, (10, 10))

# Основний цикл гри
def game_loop():
    global player_x, player_y, player_velocity_y, on_ground, obstacle_x, score

    running = True
    while running:
        screen.fill(WHITE)

        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                pygame.quit()
                sys.exit()

        # Керування гравцем
        keys = pygame.key.get_pressed()
        if keys[pygame.K_LEFT]:
            player_x -= player_speed
        if keys[pygame.K_RIGHT]:
            player_x += player_speed
        if keys[pygame.K_SPACE] and on_ground:
            player_velocity_y = player_jump
            on_ground = False

        # Гравітація
        player_velocity_y += 1  # Прискорення вниз
        player_y += player_velocity_y

        # Обмеження руху гравця
        if player_y >= HEIGHT - player_size - 10:
            player_y = HEIGHT - player_size - 10
            on_ground = True

        if player_x < 0:
            player_x = 0
        if player_x > WIDTH - player_size:
            player_x = WIDTH - player_size

        # Рух перешкод
        obstacle_x -= obstacle_speed
        if obstacle_x < -obstacle_width:
            obstacle_x = WIDTH
            score += 1

        # Перевірка на зіткнення
        if (player_x < obstacle_x + obstacle_width and
            player_x + player_size > obstacle_x and
            player_y < obstacle_y + obstacle_height and
            player_y + player_size > obstacle_y):
            running = False

        # Малювання гравця та перешкод
        pygame.draw.rect(screen, BLUE, (player_x, player_y, player_size, player_size))
        pygame.draw.rect(screen, RED, (obstacle_x, obstacle_y, obstacle_width, obstacle_height))

        # Відображення рахунку
        display_score()

        # Оновлення екрану
        pygame.display.flip()
        clock.tick(FPS)

    # Кінець гри
    screen.fill(WHITE)
    game_over_text = font.render("Game Over! Press R to Restart or Q to Quit", True, BLACK)
    screen.blit(game_over_text, (WIDTH // 2 - 300, HEIGHT // 2))
    pygame.display.flip()

    # Чекати на дію гравця
    while True:
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                pygame.quit()
                sys.exit()
            if event.type == pygame.KEYDOWN:
                if event.key == pygame.K_r:
                    main()
                if event.key == pygame.K_q:
                    pygame.quit()
                    sys.exit()

def main():
    game_loop()

if __name__ == "__main__":
    main()
