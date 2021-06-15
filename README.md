# project6
snake game

## main.py

from turtle import Screen

from questionmodel import Snake

from food import Food

import time

from scoreboard import Scoreboard

screen = Screen()

screen.setup(width=600, height=600)

screen.bgcolor("black")

screen.title("my snake game")

screen.tracer(0)

snake = Snake()

food = Food()

scoreboard = Scoreboard()

screen.listen()

screen.onkey(snake.up, "U")
screen.onkey(snake.down, "D")
screen.onkey(snake.left, "L")
screen.onkey(snake.right, "R")
game_is_on = True
while game_is_on:
    screen.update()
    time.sleep(0.1)
    snake.move()

    if snake.head.distance(food) < 15:
        food.refresh()
        snake.extend()
        scoreboard.increase_score()

    for seg in snake.segment[1:]:
        if snake.head.distance(seg) < 10 :
            game_is_on=False
            scoreboard.game_over()




    if snake.head.xcor() > 280 or snake.head.xcor()  < -280 or snake.head.ycor() > 280 or snake.head.ycor() < -280:
        game_is_on=False
        scoreboard.game_over()


screen.exitonclick()


# food.py



from turtle import Turtle
import random


class Food(Turtle):

    def __init__(self):
        super().__init__()
        self.shape("circle")
        self.penup()
        self.shapesize(stretch_len=0.5, stretch_wid=0.5)
        self.color("blue")
        self.speed("fastest")
        self.refresh()

    def refresh(self):
        random_x = random.randint(-280, 280)
        random_y = random.randint(-280, 280)
        self.goto(random_x, random_y)
        
        
        
 ## scoreboard
 
 from turtle import Turtle
ALIGNMENT="center"
FONT=("Arial", 20, "normal")


class Scoreboard(Turtle):


    def __init__(self):
        super().__init__()
        self.score=0
        self.color("white")
        self.penup()
        self.goto(0,270)
        self.hideturtle()
        self.score_update()

    def score_update(self):
        self.write(f"score is : {self.score}",align=ALIGNMENT, font=FONT)


    def game_over(self):
        self.goto(0,0)
        self.write(" GAME IS OVER ", align=ALIGNMENT, font=FONT)




    def increase_score(self):
        self.score += 10
        self.clear()
        self.score_update()
        
        
    ## questionmodel ##create snake
    
    from turtle import Turtle

starting_pos = [(0, 0), (-20, 0), (-40, 0)]
move_forward = 20
U = 90
D = 270
L = 180
R = 0


class Snake:
    def __init__(self):
        self.segment = []
        self.create_snake()
        self.head = self.segment[0]

    def create_snake(self):
        for position in starting_pos:
            self.add_segment(position)

    def add_segment(self,position):
        tim = Turtle("square")
        tim.color("white")
        tim.penup()
        tim.goto(position)
        self.segment.append(tim)
    def extend(self):
        self.add_segment(self.segment[-1].position())


    def move(self):
        for seg_num in range(len(self.segment) - 1, 0, -1):
            new_x = self.segment[seg_num - 1].xcor()
            new_y = self.segment[seg_num - 1].ycor()
            self.segment[seg_num].goto(new_x, new_y)

        self.head.forward(move_forward)

    def up(self):
        if self.head.heading() != D:
            self.head.setheading(U)

    def down(self):
        if self.head.heading() != U:
            self.head.setheading(D)

    def left(self):
        if self.head.heading() != L:
            self.head.setheading(R)

    def right(self):
        if self.head.heading() != R:
            self.head.setheading(L)
