import pygame
import random
import time

# Inicialização do pygame
pygame.init()

# Cores
preto = (0, 0, 0)
branco = (255, 255, 255)
vermelho = (255, 0, 0)
verde = (0, 255, 0)
azul = (50, 153, 213)

# Configurações do jogo
largura = 800
altura = 600
tamanho_bloco = 20
velocidade = 15

# Criando a tela do jogo
tela = pygame.display.set_mode((largura, altura))
pygame.display.set_caption('Jogo da Cobrinha')

# Fonte
fonte = pygame.font.SysFont(None, 35)

# Função para exibir mensagens na tela
def mensagem(msg, cor):
    texto = fonte.render(msg, True, cor)
    tela.blit(texto, [largura/6, altura/3])

# Função para exibir a cobra
def desenhar_cobra(tamanho_bloco, lista_cobra):
    for x in lista_cobra:
        pygame.draw.rect(tela, verde, [x[0], x[1], tamanho_bloco, tamanho_bloco])

# Função principal do jogo
def jogo():
    game_over = False
    game_close = False

    # Posições iniciais da cobra
    x1 = largura / 2
    y1 = altura / 2

    # Definindo a direção inicial da cobra
    x1_change = 0
    y1_change = 0

    # Lista para armazenar os segmentos da cobra
    lista_cobra = []
    tamanho_cobra = 1

    # Definindo a posição inicial da maçã
    comida_x = round(random.randrange(0, largura - tamanho_bloco) / tamanho_bloco) * tamanho_bloco
    comida_y = round(random.randrange(0, altura - tamanho_bloco) / tamanho_bloco) * tamanho_bloco

    # Clock para controlar a velocidade do jogo
    clock = pygame.time.Clock()

    # Laço principal do jogo
    while not game_over:
        while game_close:
            tela.fill(preto)
            mensagem('Você perdeu! Pressione C para jogar novamente ou Q para sair.', vermelho)
            pygame.display.update()

            # Captura eventos de teclado para sair ou reiniciar o jogo
            for event in pygame.event.get():
                if event.type == pygame.KEYDOWN:
                    if event.key == pygame.K_q:
                        game_over = True
                        game_close = False
                    if event.key == pygame.K_c:
                        jogo()

        # Captura eventos de teclado para controlar a direção da cobra
        for event in pygame.event.get():
            if event.type == pygame.KEYDOWN:
                if event.key == pygame.K_LEFT:
                    x1_change = -tamanho_bloco
                    y1_change = 0
                elif event.key == pygame.K_RIGHT:
                    x1_change = tamanho_bloco
                    y1_change = 0
                elif event.key == pygame.K_UP:
                    x1_change = 0
                    y1_change = -tamanho_bloco
                elif event.key == pygame.K_DOWN:
                    x1_change = 0
                    y1_change = tamanho_bloco

        # Verifica se a cobra saiu da tela
        if x1 >= largura or x1 < 0 or y1 >= altura or y1 < 0:
            game_close = True
        
        # Atualiza a posição da cobra
        x1 += x1_change
        y1 += y1_change
        
        # Limpa a tela
        tela.fill(azul)
        
        # Desenha a maçã
        pygame.draw.rect(tela, vermelho, [comida_x, comida_y, tamanho_bloco, tamanho_bloco])
        
        # Atualiza a lista de segmentos da cobra
        cabeça_cobra = []
        cabeça_cobra.append(x1)
        cabeça_cobra.append(y1)
        lista_cobra.append(cabeça_cobra)
        
        # Limita o tamanho da cobra
        if len(lista_cobra) > tamanho_cobra:
            del lista_cobra[0]
        
        # Verifica se a cobra colidiu consigo mesma
        for segmento in lista_cobra[:-1]:
            if segmento == cabeça_cobra:
                game_close = True
        
        # Desenha a cobra
        desenhar_cobra(tamanho_bloco, lista_cobra)
        
        # Atualiza a tela
        pygame.display.update()
        
        # Verifica se a cobra comeu a maçã
        if x1 == comida_x and y1 == comida_y:
            comida_x = round(random.randrange(0, largura - tamanho_bloco) / tamanho_bloco) * tamanho_bloco
            comida_y = round(random.randrange(0, altura - tamanho_bloco) / tamanho_bloco) * tamanho_bloco
            tamanho_cobra += 1
        
        # Controla a velocidade do jogo
        clock.tick(velocidade)

    # Fecha o pygame ao sair do jogo
    pygame.quit()
    quit()

# Inicia o jogo
jogo()
