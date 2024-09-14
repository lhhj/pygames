
### Introduktion
Dette projekt implementerer det klassiske slangespil ved hjælp af Python og Pygame-biblioteket. Spillet går ud på at styre en slange, der bevæger sig rundt på skærmen og vokser, når den spiser mad. Målet er at undgå at ramme slangen selv eller kanten af skærmen.

### Krav
For at køre dette spil skal du have følgende software installeret:

- Python 3.x
- Pygame-biblioteket

For at installere Pygame skal du bruge følgende kommando:
```bash
pip install pygame
```

### Funktioner
1. **Slangens bevægelse**: Spilleren kan styre slangen i fire retninger (op, ned, venstre og højre).
2. **Mad og vækst**: Slangen vokser, hver gang den spiser mad. Maden placeres tilfældigt på skærmen.
3. **Game over**: Spillet slutter, hvis slangen kolliderer med sig selv eller skærmens grænser.

### Sådan kører du spillet
1. Sørg for, at du har installeret Python og Pygame.
2. Kopier Python-koden fra slangespillet og gem det i en fil (f.eks. `snake_game.py`).
3. Åbn en terminal og kør spillet ved hjælp af følgende kommando:
```bash
python snake_game.py
```

### Spilkontroller
- **Op**: Flyt slangen opad.
- **Ned**: Flyt slangen nedad.
- **Venstre**: Flyt slangen til venstre.
- **Højre**: Flyt slangen til højre.

### Spillogik
- Spillet starter med en lille slange, som kan bevæge sig i en retning. Maden placeres tilfældigt på skærmen.
- Når slangen spiser mad, vokser dens længde, og ny mad placeres et andet sted på skærmen.
- Hvis slangen rammer kanten af skærmen eller rammer sin egen krop, slutter spillet.
- Spillerens score stiger med antallet af spiste fødevarer.

### Python Kode (forkortet)
```python
# importerer biblioteker
import pygame
import time
import random

slange_hastighed = 15

# Vinduesstørrelse
vindue_x = 720
vindue_y = 480

# definerer farver
sort = pygame.Color(0, 0, 0)
hvid = pygame.Color(255, 255, 255)
rød = pygame.Color(255, 0, 0)
grøn = pygame.Color(0, 255, 0)
blå = pygame.Color(0, 0, 255)

# Initialiserer pygame
pygame.init()

# Initialiserer spilvinduet
pygame.display.set_caption('GeeksforGeeks Slange')
spil_vindue = pygame.display.set_mode((vindue_x, vindue_y))

# FPS (frames per second) controller
fps = pygame.time.Clock()

# definerer slangens standardposition
slange_position = [100, 50]

# definerer de første 4 blokke af slangens krop
slange_krop = [[100, 50],
               [90, 50],
               [80, 50],
               [70, 50]
              ]

# frugtens position
frugt_position = [random.randrange(1, (vindue_x//10)) * 10, 
                  random.randrange(1, (vindue_y//10)) * 10]

frugt_spawn = True

# sætter slangens standardretning mod højre
retning = 'HØJRE'
ændring_til = retning

# startscore
score = 0

# funktion til at vise score
def vis_score(valg, farve, skrifttype, størrelse):
  
    # opretter skrifttype-objekt score_font
    score_font = pygame.font.SysFont(skrifttype, størrelse)
    
    # opretter overfladeobjektet score_surface
    score_surface = score_font.render('Score : ' + str(score), True, farve)
    
    # opretter et rektangel-objekt til tekstoverfladen
    score_rect = score_surface.get_rect()
    
    # viser teksten
    spil_vindue.blit(score_surface, score_rect)

# funktion til spilslutning
def spil_slut():
  
    # opretter skrifttype-objekt my_font
    my_font = pygame.font.SysFont('times new roman', 50)
    
    # opretter en tekstoverflade, hvor teksten vil blive tegnet
    spil_slut_surface = my_font.render(
        'Din Score er : ' + str(score), True, rød)
    
    # opretter et rektangel-objekt til tekstoverfladen
    spil_slut_rect = spil_slut_surface.get_rect()
    
    # indstiller positionen for teksten
    spil_slut_rect.midtop = (vindue_x/2, vindue_y/4)
    
    # blit vil tegne teksten på skærmen
    spil_vindue.blit(spil_slut_surface, spil_slut_rect)
    pygame.display.flip()
    
    # efter 2 sekunder lukker vi programmet
    time.sleep(2)
    
    # deaktiverer pygame biblioteket
    pygame.quit()
    
    # afslutter programmet
    quit()


# Hovedfunktion
while True:
    
    # håndtering af tastaturbegivenheder
    for event in pygame.event.get():
        if event.type == pygame.KEYDOWN:
            if event.key == pygame.K_UP:
                ændring_til = 'OP'
            if event.key == pygame.K_DOWN:
                ændring_til = 'NED'
            if event.key == pygame.K_LEFT:
                ændring_til = 'VENSTRE'
            if event.key == pygame.K_RIGHT:
                ændring_til = 'HØJRE'

    # Hvis to taster trykkes samtidigt, 
    # vil vi ikke have, at slangen bevæger sig i to retninger samtidig
    if ændring_til == 'OP' and retning != 'NED':
        retning = 'OP'
    if ændring_til == 'NED' and retning != 'OP':
        retning = 'NED'
    if ændring_til == 'VENSTRE' and retning != 'HØJRE':
        retning = 'VENSTRE'
    if ændring_til == 'HØJRE' and retning != 'VENSTRE':
        retning = 'HØJRE'

    # Bevægelse af slangen
    if retning == 'OP':
        slange_position[1] -= 10
    if retning == 'NED':
        slange_position[1] += 10
    if retning == 'VENSTRE':
        slange_position[0] -= 10
    if retning == 'HØJRE':
        slange_position[0] += 10

    # Slangens vækstmekanisme
    # Hvis frugten og slangen kolliderer, øges score med 10
    slange_krop.insert(0, list(slange_position))
    if slange_position[0] == frugt_position[0] and slange_position[1] == frugt_position[1]:
        score += 10
        frugt_spawn = False
    else:
        slange_krop.pop()
        
    if not frugt_spawn:
        frugt_position = [random.randrange(1, (vindue_x//10)) * 10, 
                          random.randrange(1, (vindue_y//10)) * 10]
        
    frugt_spawn = True
    spil_vindue.fill(sort)
    
    for pos in slange_krop:
        pygame.draw.rect(spil_vindue, grøn,
                         pygame.Rect(pos[0], pos[1], 10, 10))
    pygame.draw.rect(spil_vindue, hvid, pygame.Rect(
        frugt_position[0], frugt_position[1], 10, 10))

    # Game Over-betingelser
    if slange_position[0] < 0 or slange_position[0] > vindue_x-10:
        spil_slut()
    if slange_position[1] < 0 or slange_position[1] > vindue_y-10:
        spil_slut()

    # Hvis slangen rører sin egen krop
    for blok in slange_krop[1:]:
        if slange_position[0] == blok[0] and slange_position[1] == blok[1]:
            spil_slut()

    # Viser scoren kontinuerligt
    vis_score(1, hvid, 'times new roman', 20)

    # Opdaterer spilskærmen
    pygame.display.update()

    # Frame Per Second (FPS) opdateringshastighed
    fps.tick(slange_hastighed)

```

### Bidrag
Hvis du vil bidrage til dette projekt, er du velkommen til at lave en pull request eller oprette et issue.

---

Dette README.md kan bruges til at dokumentere dit Python-slangespilprojekt, oversat til dansk.
