# Ex.No: 2 Implementation of Stack Plate game using Queue 
### DATE:                                                                            
### AIM: 
To write a python program to simulate the process of stacking plates.
### Algorithm:
1. Initialize the Stack
2. Create an empty list to represent the stack.
3. Push the plate on top of stack
4. Pop the plate from top.
5. Display the plate details.
6. Create an interactive menu and display it.
### Program:
```
Developed By: Sai Eswar Kandukuri
Reg.No: 212221240020
```
```
import pygame
import sys

# Initialize Pygame
pygame.init()

# Constants
SCREEN_WIDTH, SCREEN_HEIGHT = 800, 600
BACKGROUND_COLOR = (30, 30, 30)
PLATE_COLOR = (255, 100, 100)
STACK_COLOR = (100, 255, 100)
TEXT_COLOR = (255, 255, 255)
PLATE_WIDTH = 60
PLATE_HEIGHT = 20
STACK_SPACING = 150
PLATE_SPACING = 5
FONT_SIZE = 24
FPS = 60

# Setup the screen
screen = pygame.display.set_mode((SCREEN_WIDTH, SCREEN_HEIGHT))
pygame.display.set_caption("Stack of Plates")
clock = pygame.time.Clock()

# Font
font = pygame.font.Font(None, FONT_SIZE)

class Stack:
    def __init__(self, capacity, position):
        self.capacity = capacity
        self.stack = []
        self.position = position  # (x, y) position of the stack

    def is_full(self):
        return len(self.stack) >= self.capacity

    def is_empty(self):
        return len(self.stack) == 0

    def push(self, item):
        if not self.is_full():
            self.stack.append(item)
        else:
            raise Exception("Stack is full")

    def pop(self):
        if not self.is_empty():
            return self.stack.pop()
        else:
            raise Exception("Stack is empty")

    def draw(self):
        x, y = self.position
        for i, plate in enumerate(self.stack):
            pygame.draw.rect(screen, PLATE_COLOR, (x, y - i * (PLATE_HEIGHT + PLATE_SPACING), PLATE_WIDTH, PLATE_HEIGHT))
            plate_text = font.render(str(plate), True, TEXT_COLOR)
            screen.blit(plate_text, (x + PLATE_WIDTH // 2 - plate_text.get_width() // 2, 
                                     y - i * (PLATE_HEIGHT + PLATE_SPACING) + PLATE_HEIGHT // 2 - plate_text.get_height() // 2))


class SetOfStacks:
    def __init__(self, capacity, max_stacks):
        self.capacity = capacity
        self.max_stacks = max_stacks
        self.stacks = []

    def get_last_stack(self):
        if not self.stacks:
            return None
        return self.stacks[-1]

    def push(self, item):
        last_stack = self.get_last_stack()
        if last_stack and not last_stack.is_full():
            last_stack.push(item)
        else:
            if len(self.stacks) < self.max_stacks:
                new_stack_position = (len(self.stacks) * STACK_SPACING + 100, SCREEN_HEIGHT - 100)
                new_stack = Stack(self.capacity, new_stack_position)
                new_stack.push(item)
                self.stacks.append(new_stack)
            else:
                print("No more stacks can be added!")

    def pop(self):
        last_stack = self.get_last_stack()
        if not last_stack:
            raise Exception("No stacks available")
        item = last_stack.pop()
        if last_stack.is_empty():
            self.stacks.pop()
        return item

    def draw(self):
        for stack in self.stacks:
            stack.draw()


# Example usage in Pygame
set_of_stacks = SetOfStacks(capacity=5, max_stacks=5)

# Simulate pushing plates onto stacks
for i in range(15):
    set_of_stacks.push(i)

# Main Game Loop
running = True
while running:
    screen.fill(BACKGROUND_COLOR)

    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False
        elif event.type == pygame.KEYDOWN:
            if event.key == pygame.K_p:  # Press 'P' to push a new plate
                set_of_stacks.push(len(set_of_stacks.stacks) * 5)
            elif event.key == pygame.K_o:  # Press 'O' to pop the top plate
                if set_of_stacks.stacks:
                    set_of_stacks.pop()

    # Draw the stacks and plates
    set_of_stacks.draw()

    pygame.display.flip()
    clock.tick(FPS)

pygame.quit()
sys.exit()

```
### Output:
<img width="802" alt="Screenshot 2024-08-09 at 2 44 53 PM" src="https://github.com/user-attachments/assets/58feb8d1-ac61-4ca2-bc99-6b1408bd1862">

### Result:
Thus the simple Stack plate game was implemented using data structure Stack.
