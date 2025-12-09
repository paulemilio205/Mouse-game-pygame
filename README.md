# Mouse-game-pygame
#A mause pygame este es el juego de un raton creado en pygame
#espero que les guste
#en los apartados de cell cree las imagenes y nombrelas como el nobre especificado


#pgzero
import random



cell = Actor('pasto', size = (50,50))
cell1 = Actor('arbustos', size = (50,50))
cell2 = Actor('flores', size = (50,50))
cell3 = Actor('ladrillo', size = (50,50))
size_w = 9 
size_h = 10 
WIDTH = cell.width * size_w
HEIGHT = cell.height * size_h
enemies = []
mode = "game"
win = 0
for i in range(5):
    x = random.randint(1,7) * cell.width
    y = random.randint(1,7) * cell.height
    enemy = Actor("gato1",topleft = (x,y))
    enemy.health = random.randint(10,20)
    enemy.attack = random.randint(5,10)
    enemy.bonus = random.randint(0,3)
    enemies.append(enemy)

TITLE = "La ratita"
FPS = 30
my_map = [[3,3,3,3,3,3,3,3,3],
          [3,1,0,0,0,2,0,1,3],
          [3,0,0,0,2,0,0,2,3],
          [3,0,2,0,0,0,0,1,3],
          [3,0,0,0,0,1,0,2,3],
          [3,1,0,2,0,1,0,2,3],
          [3,2,0,1,0,2,0,0,3],
          [3,1,0,0,0,2,1,0,3],
          [3,3,3,3,3,3,3,3,3]]

#la ratita
Mouse = Actor('rata', size = (50,50))
Mouse.top = cell.height
Mouse.left = cell.width
Mouse.health = 50
Mouse.attack = 5
Mouse.Money = 0
vidas = []
espadas = []
monedas = []

def map_draw():
    for i in range(len(my_map)):
        for j in range(len(my_map[0])):
            if my_map[i][j] == 0:
                cell.left = cell.width*j
                cell.top = cell.height*i
                cell.draw()
            elif my_map[i][j] == 1:
                cell1.left = cell.width*j
                cell1.top = cell.height*i
                cell1.draw()
            elif my_map[i][j] == 2:
                cell2.left = cell.width*j
                cell2.top = cell.height*i
                cell2.draw()  
            elif my_map[i][j] == 3:
                cell3.left = cell.width*j
                cell3.top = cell.height*i
                cell3.draw() 
def victory():
    global mode, win
    if enemies == [] and Mouse.health > 0:
        mode = "end"
        win = 1
    elif Mouse.health <= 0:
        mode = "end"


def draw():
    if mode =="game":
        screen.fill("#2f3542")
        map_draw()
        Mouse.draw()
        screen.draw.text("HP:", center=(25, 475), color = 'white', fontsize = 20)
        screen.draw.text(Mouse.health, center=(75, 475), color = 'white', fontsize = 20)
        screen.draw.text("AP:", center=(375, 475), color = 'white', fontsize = 20)
        screen.draw.text(Mouse.attack, center=(425, 475), color = 'white', fontsize = 20)
        screen.draw.text("MY:", center=(175, 475), color = 'white', fontsize = 20)
        screen.draw.text(Mouse.Money, center=(220, 475), color = 'white', fontsize = 20)
        for i in range(len(enemies)):
            enemies[i].draw()
        for i in range(len(vidas)):
            vidas[i].draw()
        for i in range(len(espadas)):
            espadas[i].draw()
        for i in range(len(monedas)):
            monedas[i].draw()
    
    elif mode == "end":
        screen.fill("#2f3542")
        if win == 1:
            screen.draw.text("¡Ganaste!", center=(WIDTH/2 , HEIGHT/2), color = 'white', fontsize = 46)
        else:
            screen.draw.text("¡perdiste!", center=(WIDTH/2 , HEIGHT/2), color = 'white', fontsize = 46)
    
   
def on_key_down(key):
    old_x = Mouse.x
    old_y = Mouse.y
    if keyboard.right and Mouse.x + cell.width < WIDTH - cell.width:
        Mouse.x += cell.width
        
    elif keyboard.left and Mouse.x - cell.width > cell.width:
        Mouse.x -= cell.width
        
    elif keyboard.down and Mouse.y + cell.height < HEIGHT - cell.height*2:
        Mouse.y += cell.height
    elif keyboard.up and Mouse.y - cell.height > cell.height:
        Mouse.y -= cell.height
        
        
        
        
    enemy_index = Mouse.collidelist(enemies)
    if enemy_index != -1:
        Mouse.x = old_x
        Mouse.y = old_y
        enemy = enemies[enemy_index]
        enemy.health -= Mouse.attack
        Mouse.health -= enemy.attack

        if enemy.health <= 0:
            
            
            
                        
            if enemy.bonus == 1:
                heart = Actor("heart")
                heart.pos = enemy.pos
                vidas.append(heart)

                
            elif enemy.bonus == 2:
                sword = Actor("sword")
                sword.pos = enemy.pos
                espadas.append(sword)
                
                
            elif enemy.bonus == 3:
                Moneda = Actor("moneda")
                Moneda.pos = enemy.pos
                monedas.append(Moneda)
                
                
                
                

            enemies.pop(enemy_index)
            
            
            
            
            
            
            
def update(dt):
    global mode
    victory()
    for i in range(len(vidas)):
        if Mouse.colliderect(vidas[i]):
            Mouse.health += 10
            vidas.pop(i)
            break
    for i in range(len(espadas)):
        if Mouse.colliderect(espadas[i]):
            Mouse.attack += 5
            espadas.pop(i)
            break
    for i in range (len(monedas)):
        if Mouse.colliderect(monedas[i]):
            Mouse.Money += 5
            monedas.pop(i)
            break
