# snake
import pygame
import random

pygame.init()
clock = pygame.time.Clock()
orange_color = (255, 123, 7)
black_color = (0, 0, 0)
red_color = (213, 50, 80)
green_color = (0, 255, 0)
blue_color = (50, 153, 213)


display_width = 600
display_height = 400
dis = pygame.display.set_mode((display_width, display_height))
pygame.display.set_caption("Snake Game") 
snake_block = 10
snake_speed = 15
snake_list = []

def snake(snake_block,snake_list):
    for x in snake_list:
        pygame.draw.rect(dis, orange_color, [x[0], x[1], snake_block, snake_block])

def snakegame():
    game_over = False
    game_end = False

    d1 = display_width / 2
    s1 = display_height / 2

    d1_change = 0
    s1_change = 0

  
    snake_list = []
    Length_of_snake = 1

    # the co-ordinates of food element
    food_d = round(random.randrange(0, display_width - snake_block) / 10.0) * 10.0
    food_s = round(random.randrange(0, display_height - snake_block) / 10.0) * 10.0

    while not game_over:
        while game_end == True:
            dis.fill(blue_color)
            font_style = pygame.font.SysFont("comicsansms", 25)
            mesg = font_style.render("You Lost!Wanna play again? Press P", True, redcolor)
            dis.blit(mesg, [display_width / 6, display_height / 3])

            # For displaying the score
            score = Length_of_snake - 1
            score_font = pygame.font.SysFont("comicsansms", 35)
            value = score_font.render("Your Score: " + str(score), True, green_color)
            dis.blit(value, [display_width / 3, display_height / 5])
            pygame.display.update()
            for event in pygame.event.get():
               if event.type == pygame.KEYDOWN:
                   if event.key == pygame.K_p:
                       snakegame()

               if event.type == pygame.QUIT:
                    game_over = True
                    game_end = False


        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                game_over = True
            if event.type == pygame.KEYDOWN:
                if event.key == pygame.K_LEFT:
                    d1_change = -snake_block
                    s1_change = 0
                elif event.key == pygame.K_RIGHT:
                    d1_change = snake_block
                    s1_change = 0
                elif event.key == pygame.K_UP:
                    s1_change = snake_block
                    d1_change = 0
                elif event.key == pygame.K_DOWN:
                    s1_change = snake_block
                    d1_change = 0
        if d1 >= display_width or d1 < 0 or s1 >= display_height or s1 < 0:
            game_end = True

        d1 += d1_change
        s1 += s1_change
        dis.fill(black_color)
        pygame.draw.rect(dis, green_color, [food_d, food_s, snake_block, snake_block])
        snake_Head = []
        snake_Head.append(d1)
        snake_Head.append(s1)
        snake_list.append(snake_Head)

        if len(snake_list) > Length_of_snake:
            del snake_list[0]

        for x in snake_list[:-1]:
            if x == snake_Head:
                game_end = True
        snake(snake_block, snake_list)
        pygame.display.update()

        if d1 == food_d and s1 == food_s:
            food_d = round(random.randrange(0, display_width - snake_block) / 10.0) * 10.0
            food_s = round(random.randrange(0, display_height - snake_block) / 10.0) * 10.0
            Length_of_snake += 1
        clock.tick(snake_speed)
    pygame.quit()
    quit()
snakegame()
