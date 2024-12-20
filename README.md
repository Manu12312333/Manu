import pygame
import random


pygame.init()

# Bildschirm-Setup
WIDTH, HEIGHT = 800, 600
screen = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption("Geschenk-Spiel")

# Farben
WHITE = (255, 255, 255)
BLACK = (0, 0, 0)
RED = (255, 0, 0)
GREEN = (0, 255, 0)
BLUE = (0, 0, 255)

# Schriftarten
font = pygame.font.SysFont("Arial", 24)

# Variablen
player_coins = 100
inventory = []
skins = ["Standard", "Gold", "Weihnachten", "Lila"]
active_skin = "Standard"
vip = False
ad_free = False

# Belohnungen
rewards = ["Item1", "Item2", "Item3", "Seltenes Item", "Episches Item"]

# Funktion zum Zeichnen von Text
def draw_text(text, x, y, color=BLACK):
    screen.blit(font.render(text, True, color), (x, y))

# Geschenk öffnen
def open_gift():
    reward = random.choice(rewards)
    inventory.append(reward)
    return reward

# Shop-Skins
def buy_skin(skin_name, cost):
    global player_coins, active_skin
    if player_coins >= cost:
        player_coins -= cost
        active_skin = skin_name
        return f"Skin {skin_name} aktiviert!"
    else:
        return "Nicht genug Münzen!"

# VIP-Code eingeben
def redeem_code(code):
    global vip, ad_free
    if code == "VIP2024":
        vip = True
        ad_free = True
        return "VIP aktiviert! Keine Werbung mehr!"
    else:
        return "Ungültiger Code!"

# Hauptspiel-Schleife
running = True
while running:
    screen.fill(WHITE)
    
    # Events
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False
        if event.type == pygame.KEYDOWN:
            if event.key == pygame.K_o:  # O für Geschenk öffnen
                reward = open_gift()
                print(f"Du hast {reward} erhalten!")
            if event.key == pygame.K_s:  # S für Skin kaufen
                print(buy_skin("Gold", 50))
            if event.key == pygame.K_c:  # C für Code eingeben
                print(redeem_code("VIP2024"))
    
    # Interface
    draw_text(f"Münzen: {player_coins}", 10, 10)
    draw_text(f"Inventar: {', '.join(inventory)}", 10, 40)
    draw_text(f"Aktiver Skin: {active_skin}", 10, 70)
    draw_text(f"VIP: {'Ja' if vip else 'Nein'}", 10, 100)

    # Aktualisierung
    pygame.display.flip()

# Pygame beenden
pygame.quit()
