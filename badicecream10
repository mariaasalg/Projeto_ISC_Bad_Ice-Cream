.include "MACROSv21.s"
.data
CHAR_POS:	.half 32,16
OLD_CHAR_POS:	.half 32,16

INIMIGO_POS: 	.half 144,176
OLD_INIMIGO_POS: .half 144,176

BLOCO_POS:	.half 0,0	
SCORE1: 	.half 0
STRING1:	.string "             PONTOS:           FASE 1 "
STRING2:        .string "             PONTOS:           FASE 2 "
STRING3:        .string "             PONTOS:           FASE 3 "
STRING_VAZIA:   .string "                                   "
CRIAR_OU_QUEBRAR: .half 0

TEMPO_INICIAL: .word 0
TEMPO_ATUAL: .word 0
DIR_INIMIGO: .half 1 # anti horário = 1 horário = 0

NUM: .word 40
NOTAS: 67,250,67,250,64,250,64,250,65,250,64,250,62,250,62,250,67,250,67,250,64,250,64,250,65,250,69,250,67,250,67,250,69,250,72,250,71,250,69,250,67,250,72,250,67,250,65,250,64,250,65,250,64,250,62,250,67,210,77,39,79,250,60,250,65,250,64,250,64,250,67,250,67,250,72,250,72,250,60,250,60,250
CONTADOR_NOTAS: .word 0
.text 

# a0 = endereço da imagem
# a1 = x
# a2 = y
# a3 = frame (0 ou 1)

MENU:
	la a0,menu
	li a1,0
	li a2,0
	li a3,0
	call PRINT
	li a3,0
	call PRINT
	li a3,1
			
ESPACO:	   li t1,0xFF200000	
	   lw t0,0(t1)		
	   andi t0,t0,0x0001	
	   beq t0,zero,ESPACO	
	   lw t2,4(t1)		
	   
	   li t0,32
	   beq t0,t2,SETUP
	  	
SETUP:	la s7,mapacolisaof14
	
	li s11, 1
	la a0,mapfase12		# carrega o endereço do map em a0
	li a1,0			# x = 0
	li a2,0			# y = 0
	li a3,0			# frame = 0
	call PRINT		# imprime o mapa
	li a3,1
	call PRINT
	
	la a0,STRING1
	li a1,0
	li a2,5
	li a3,0xFF00
	li a4,1
	li a7,104
	ecall 
	
	la a0,STRING1
	li a1,0
	li a2,5
	li a3,0xFF00
	li a4,0
	li a7,104
	ecall 
	
	la a0,blueberry		# carrega o endereço do map em a0
	li a1,20		# x = 0
	li a2,0		# y = 0
	li a3,0			# frame = 0
	call PRINT		# imprime o mapa
	li a3,1
	call PRINT

SET_MUSICA:
	la s5,NUM		# define o endereço do número de notas
	lw s6,0(s5)		# le o numero de notas
	la s5,NOTAS		# define o endereço das notas
		
SET_TEMPO: li a7,30
	   ecall 
	   la s1,TEMPO_INICIAL
	   sw a0,0(s1)

	la a0,char16dir
GAME_LOOP: 
	   call TEMPO	   
GAME_LOOP1:
           call KEY2
	   
GAME_LOOP2: 
	   xori s0,s0,1
	   
	   li t0,0xFF200604	# carrega em t0 a troca de frame
	   sw s0,0(t0)		# mostra o sprite
	   
	   la t0,OLD_CHAR_POS	#carrega em t0 o endereço de oldcharpos
	   lw s10,0(t0)
	   
	   # LIMPA O RASTRO # 
	   
	   la a0,tile_neve	        # carrega o endereço do tile em a0
	   lh a1,0(t0)		# posição 
	   lh a2,2(t0)
	   
	   mv a3,s0		# carrega o frame atual
	   xori a3,a3,1		# inverte a3 
	   call PRINT		# imprime
		  
	   j GAME_LOOP		# continua o loop
	   
KEY2:	   li t1,0xFF200000	#KDMMIO
	   lw t0,0(t1)		# le bit de controle do teclado
	   andi t0,t0,0x0001	# mascara o bit menos significativo
	   beq t0,zero,FIM	# se não tem tecla pressionada, vai pra FIM
	   lw t2,4(t1)		# le o valor da tecla
	   
	   li t0,'w'
	   beq t2,t0,CHAR_CIMA
	   
	   li t0,'a'  
	   beq t2,t0,CHAR_ESQ
	   
	   li t0,'s'
	   beq t2,t0,CHAR_BAIXO
	   
	   li t0,'d'
	   beq t2,t0,CHAR_DIR
	   
	   li t0,32
	   beq t2,t0,BLOCO_GELO
	   
TEMPO: 	  li a7,30			# guarda o tempo atual
	  ecall 
	  
	  la s2,TEMPO_ATUAL		# pega o endereço do tempo atual
	  sw a0,0(s2)			# guarda o endereço em tempo atual
	  
	  la s1,TEMPO_INICIAL		# chama o endereço do tempo inicial
	  
	  lw t3,0(s1)		
	  lw t4,0(s2)
	  
	  li t5,250			
	  sub s3,t4,t3 			
	  
	  blt s3,t5,GAME_LOOP1		# se a diferença for menor que 750, volta pro loop **********
            
	  sw t4,0(s1)			# coloca o tempo atual em tempo inicial
	  
         j MUSICA 	
  	  
INIMIGO:      
	 la t3,INIMIGO_POS
         lh t3,0(t3)
         li t4,256
         beq t3,t4,CIMA_OU_ESQUERDA     
         
         la t3,INIMIGO_POS
         lh t3,2(t3)
         li t4,176
         beq t3,t4,DIREITA_OU_ESQUERDA
            
         la t3,INIMIGO_POS
         lh t3,2(t3)
         li t4,48
         beq t3,t4,ESQUERDA_OU_BAIXO
         
         la t3,INIMIGO_POS
         lh t3,0(t3)
         li t4,48
         beq t3,t4,BAIXO_OU_DIREITA
         
CIMA_OU_ESQUERDA:
	 la t3,INIMIGO_POS
	 lh t3,2(t3)
	 li t4,48
	 
	 la t1,DIR_INIMIGO
	 lw t2,0(t1)
	 
	 li t5,0
	 beq t2,t5,BAIXO_OU_ESQUERDA
	 
	 beq t3,t4,ESQUERDA
	
	 j CIMA
	 
ESQUERDA_OU_BAIXO:
	 la t3,INIMIGO_POS
	 lh t3,0(t3)
	 li t4,48
	 
	 la t1, DIR_INIMIGO
	 lw t2,0(t1)
	 
	li t5,0
	beq t2,t5,DIREITA
	
         beq t3,t4,BAIXO
	 j ESQUERDA

BAIXO_OU_DIREITA:
	 la t3,INIMIGO_POS
	 lh t3,2(t3)
	 
	 la t1, DIR_INIMIGO
	 lw t2,0(t1)
	 
	li t5,0
	beq t2,t5,CIMA
	
	li t4,176
	beq t3,t4,DIREITA
	j BAIXO
         
BAIXO_OU_ESQUERDA:
	 la t3,INIMIGO_POS
	 lh t3,2(t3)
	 li t4,176
	 beq t3,t4,ESQUERDA
	 j BAIXO
	 
DIREITA_OU_ESQUERDA:
	la t3,INIMIGO_POS
	lh t3,0(t3)
	li t4,48
	
	beq t3,t4,BAIXO_OU_DIREITA
	
	la t1, DIR_INIMIGO
	lw t2,0(t1)
	 
	li t5,0
	beq t2,t5,ESQUERDA
	
	j DIREITA
	
DIREITA:
	
        la t2,INIMIGO_POS		# carrega a posição do inimigo
        
        la t1,CHAR_POS
	lw t4,0(t1)
	lw t5,0(t2)
	beq t4,t5,GAME_OVER	
	
	mv a2,s7
	
	   lh t3,0(t2)			# carrega a posição x do personagem
	   lh t4,2(t2)			# carrega a posição y do personagem
	    
	  li t5,16
	  div t3,t3,t5
	  div t4,t4,t5
	    
	  addi t3,t3,1		# aumenta o y em 1
	    
	  li t5,20
	  mul a1,t4,t5		# a1 = y * 20
	  add a2,a2,a1		# adiciona y ao endereço base
	  add a2,t3,a2		# adiciona x ao endereço base		
	    
	  lb s8,0(a2)			# carrega o pixel 
	  
	  li a3,-64
	  beq s8,a3,VAI_PRA_ESQUERDA
	  
	  li a3,0x00000000
	  beq s8,a3,VAI_PRA_ESQUERDA
		
	 lh t4,0(t2)			# carrega o x	
	lh t4,0(t2)			# carrega o x	
	 
	addi t4,t4,16  			# aumenta o x em 16 
	 
	la t6,OLD_INIMIGO_POS         	# carrega a posição antiga
	lw t3,0(t2)			# pega a posição atual do inimigo e coloca em t3
	sw t3,0(t6) 			# guarda a posição atual na posição antiga
	 
	sh t4,0(t2)			# atualizando o x
	
	la a0,inimigo_dir			# carrega o sprite
	
	li t3,2
	beq t3,s11,SPRITE_2_DIR
	
	j PRINT_INIMIGO	
	
SPRITE_2_DIR:
	
	la a0,inimigo2dir
	j PRINT_INIMIGO
	

CIMA:    la t2,INIMIGO_POS		# carrega a posição do inimigo
	 lh t4,2(t2)			# carrega o x	
	 
        la t1,CHAR_POS
	lw t4,0(t1)
	lw t5,0(t2)
	beq t4,t5,GAME_OVER

	mv a2,s7

	 lh t3,0(t2)			# carrega a posição x do personagem
	 lh t4,2(t2)			# carrega a posição y do personagem
	    
	  li t5,16
	  div t3,t3,t5
	  div t4,t4,t5
	    
	  addi t4,t4,-1		# aumenta o y em 1
	    
	  li t5,20
	  mul a1,t4,t5		# a1 = y * 20
	  add a2,a2,a1		# adiciona y ao endereço base
	  add a2,t3,a2		# adiciona x ao endereço base		
	    
	  lb s8,0(a2)			# carrega o pixel 
	  
	  li a3,-64
	  beq s8,a3,VAI_PRA_BAIXO
	  
	  li a3,0x00000000
	  beq s8,a3,VAI_PRA_BAIXO
	  
	  lh t4,2(t2)
	 
	addi t4,t4,-16  		# diminui o y em 16
	 
	la t6,OLD_INIMIGO_POS         	# carrega a posição antiga
	lw t3,0(t2)			# pega a posição atual do inimigo e coloca em t3
	sw t3,0(t6) 			# guarda a posição atual na posição antiga
	 
	sh t4,2(t2)			# arualizando o y
	
	la a0,inimigo_cima			# carrega o sprite
	j PRINT_INIMIGO
	
	
ESQUERDA: la t2,INIMIGO_POS		# carrega a posição do inimigo

	  la t1,CHAR_POS
	  lw t4,0(t1)
	  lw t5,0(t2)
	  beq t4,t5,GAME_OVER
	
	  mv a2,s7

	   lh t3,0(t2)			# carrega a posição x do personagem
	   lh t4,2(t2)			# carrega a posição y do personagem
	    
	  li t5,16
	  div t3,t3,t5
	  div t4,t4,t5
	    
	  addi t3,t3,-1		# aumenta o y em 1
	    
	  li t5,20
	  mul a1,t4,t5		# a1 = y * 20
	  add a2,a2,a1		# adiciona y ao endereço base
	  add a2,t3,a2		# adiciona x ao endereço base		
	    
	  lb s8,0(a2)			# carrega o pixel 
	  
	  li a3,-64
	  beq s8,a3,VAI_PRA_DIREITA
	  
	  li a3,0x00000000
	  beq s8,a3,VAI_PRA_DIREITA
		
	 lh t4,0(t2)			# carrega o x	
	 
	addi t4,t4,-16  		# diminui o x em 16 
	 
	la t6,OLD_INIMIGO_POS         	# carrega a posição antiga
	lw t3,0(t2)			# pega a posição atual do inimigo e coloca em t3
	sw t3,0(t6) 			# guarda a posição atual na posição antiga
	 
	sh t4,0(t2)			# arualizando o x
	
	la a0,inimigo_esq			# carrega o sprite
	
	li t3,2
	beq t3,s11,SPRITE_2_ESQ
	
	j PRINT_INIMIGO
	
SPRITE_2_ESQ:

	la a0,inimigo2esq
	j PRINT_INIMIGO
	
BAIXO:	la t2,INIMIGO_POS		# carrega a posição do inimigo
        la t1,CHAR_POS
	lw t4,0(t1)
	lw t5,0(t2)
	beq t4,t5,GAME_OVER

	mv a2,s7
	
	   lh t3,0(t2)			# carrega a posição x do personagem
	   lh t4,2(t2)			# carrega a posição y do personagem
	    
	  li t5,16
	  div t3,t3,t5
	  div t4,t4,t5
	    
	  addi t4,t4,1		# aumenta o y em 1
	    
	  li t5,20
	  mul a1,t4,t5		# a1 = y * 20
	  add a2,a2,a1		# adiciona y ao endereço base
	  add a2,t3,a2		# adiciona x ao endereço base		
	    
	  lb s8,0(a2)			# carrega o pixel 
	  
	  li a3,-64
	  beq s8,a3,VAI_PRA_CIMA
	  
	  li a3,0x00000000
	  beq s8,a3,VAI_PRA_CIMA
	  	
	lh t4,2(t2)			# carrega o y
	 
	addi t4,t4,16  		# aumenta o y em 16
	 
	la t6,OLD_INIMIGO_POS         	# carrega a posição antiga
	lw t3,0(t2)			# pega a posição atual do inimigo e coloca em t3
	sw t3,0(t6) 			# guarda a posição atual na posição antiga
	 
	sh t4,2(t2)			# arualizando o y
	
	la a0,inimigo_baixo			# carrega o sprite
	j PRINT_INIMIGO
	
	
VAI_PRA_CIMA:
	la t1,DIR_INIMIGO
	lw t2,0(t1)
	
	xori t2,t2,1 
	
	sw t2,0(t1)
	j CIMA
	
VAI_PRA_BAIXO: 
	la t1,DIR_INIMIGO
	lw t2,0(t1)
	
	xori t2,t2,1
	
	sw t2,0(t1)
	j BAIXO
	
VAI_PRA_DIREITA:
	la t1,DIR_INIMIGO
	lw t2,0(t1)
	
	xori t2,t2,1
	
	sw t2,0(t1)
	j DIREITA
	
VAI_PRA_ESQUERDA:
	la t1,DIR_INIMIGO
	lw t2,0(t1)
	
	xori t2,t2,1
	
	sw t2,0(t1)
	j ESQUERDA
	
PRINT_INIMIGO:  
	
	   xori s0,s0,1
	   la t2,INIMIGO_POS	#carrega em t0 o endereço de charpos
	   lh a1,0(t2)		# carrega x do personagem em a1
	   lh a2,2(t2)		# carrega y do personagem em a2
	   mv a3,s0		# carrega o valor do frame em a3
	   call PRINT 		# imprime o sprite 	  
	   
	   
	   xori s0,s0,1		# troca de frame
	   la t2,INIMIGO_POS	#carrega em t0 o endereço de charpos
	   lh a1,0(t2)		# carrega x do personagem em a1
	   lh a2,2(t2)		# carrega y do personagem em a2
	   mv a3,s0		# carrega o valor do frame em a3
	   call PRINT 		# imprime o sprite
	   
	   li t0,0xFF200604	# carrega em t0 a troca de frame
	   sw s0,0(t0)		# mostra o sprite
	   
	   la t2,OLD_INIMIGO_POS	#carrega em t0 o endereço de oldcharpos
	   	   
	   # LIMPA O RASTRO # 
           xori s0,s0,1	   
	   la a0,tile_neve	# carrega o endereço do tile em a0
	   lh a1,0(t2)	        
	   lh a2,2(t2)
	   
	   mv a3,s0		# carrega o frame atual
	   call PRINT		# imprime
	   
	   xori s0,s0,1
	   la t2,OLD_INIMIGO_POS
	   la a0,tile_neve
	   lh a1,0(t2)
	   lh a2,2(t2)
	   mv a3,s0
	   call PRINT
	
	   j GAME_LOOP1		# continua o loop
	   
MUSICA:

	la t5,CONTADOR_NOTAS
	lw t6,0(t5)			
	li a2,32 	      # define o instrumento
	li a3,80		# define o volume	
	 
	beq t6,s6,FIM_MUSICA_   # contador chegou no final? então  vá para FIM_MUSICA
	lw a0,0(s5)		# le o valor da nota
	lw a1,4(s5)		# le a duracao da nota
	li a7,31		# define a chamada de syscall
	ecall 			# toca a nota

	addi s5,s5,8		# incrementa para o endereço da próxima nota
	addi t6,t6,1		# incrementa o contador de notas
	sw t6,0(t5)		# guarda o novo valor no contador_notas
	
	li t5,2
	beq t5,s11,INIMIGO_2
	
	j INIMIGO			# volta ao loop

FIM_MUSICA_:	
	la t1,CONTADOR_NOTAS
	li t0,0
	sw t0,0(t1)
	
	la s5,NUM		# define o endereço do número de notas
	lw s6,0(s5)		# le o numero de notas
	la s5,NOTAS		# define o endereço das notas

	li t5,2
	beq t5,s11,INIMIGO_2
		
	j INIMIGO
	  
PRINT_CHAR:
	   xori s0,s0,1
	   la t0,CHAR_POS 	#carrega em t0 o endereço de charpos
	   lh a1,0(t0)		# carrega x do personagem em a1
	   lh a2,2(t0)		# carrega y do personagem em a2
	   mv a3,s0		# carrega o valor do frame em a3
	   call PRINT 		# imprime o sprite 	  
	   
	   xori s0,s0,1		# troca de frame
	   la t0,CHAR_POS 	#carrega em t0 o endereço de charpos
	   lh a1,0(t0)		# carrega x do personagem em a1
	   lh a2,2(t0)		# carrega y do personagem em a2
	   mv a3,s0		# carrega o valor do frame em a3
	   call PRINT 		# imprime o sprite
	   
FIM: 	  j GAME_LOOP2

CHAR_ESQ: la t0,CHAR_POS	# carrega em t0 o endereço do personagem
	
	  la t1,INIMIGO_POS
	  lw t3,0(t1)
	  lw t2,0(t0)
	  beq t3,t2,GAME_OVER
	  
	  la t3,SCORE1
	  lh t2,0(t3)
	  li t4,800
	  beq t2,t4,FIM_FASE
	  
	# CHECA COLISÃO À ESQUERDA
	
	  mv a6,s7
	  
	  lh t3,0(t0)			# carrega a posição x do personagem
	  lh t4,2(t0)			# carrega a posição y do personagem
	    
	  li t5,16
	  div t3,t3,t5
	  div t4,t4,t5      
	    
	  addi t3,t3,-1		# descresce o x em 1
	    
	  li t5,20
	  mul a1,t4,t5		# a1 = y * 20
	  add a6,a6,a1		# adiciona y ao endereço base
	  add a6,t3,a6	         # adiciona x ao endereço base		
	    
	  lb s8,0(a6)			# carrega o pixel 
	   
	  li a3,0x00000000		# tile preto
	  beq s8,a3,KEY2		# se for igual, recomeça
	  
	  li a3,-64
	  beq s8,a3,KEY2

	  li a5,1
	  
	  li a3,-57
	  bne s8,a3,CONT_ESQ		# se não tiver a fruta, continua o movimento
	  jal PONTUACAO			# muda a pontuação
	  
	  li t3,0xFF			# lê um tile branco
	  sb t3,0(a6)    		# o tile que antes tinha fruta, agora é um tile normal
	  
CONT_ESQ: 
	  la t1,OLD_CHAR_POS	# carrega em t1 o endereço de oldcharpos
	  lw t2,0(t0)		
	  sw t2,0(t1)		# salva a posição atual do personagem em oldcharpos
	  

	  la t0,CHAR_POS	# carrega o x atual do personagem
	  lh t1,0(t0)		
	  addi t1,t1,-16	# diminui 16 pixels
	  sh t1,0(t0)		# salva
	  
	  la a0,charesquerda		
	  j PRINT_CHAR		# manda printar o personagem
	  
CHAR_DIR: la t0,CHAR_POS	# carrega em t0 o endereço do personagem

	  la t1,INIMIGO_POS
	  lw t3,0(t1)
	  lw t2,0(t0)
	  beq t2,t3,GAME_OVER
	  
	  la t3,SCORE1
	  lh t2,0(t3)
	  li t4,800
	  beq t2,t4,FIM_FASE
	  
	  #CHECA COLISÃO À DIREITA	
	  
	  mv a6,s7
	  
	  lh t3,0(t0)			# carrega a posição x do personagem
	  lh t4,2(t0)			# carrega a posição y do personagem
	    
	  li t5,16
	  div t3,t3,t5
	  div t4,t4,t5
	    
	  addi t3,t3,1		# aumenta o x em 1
	    
	  li t5,20
	  mul a1,t4,t5		# a1 = y * 20
	  add a6,a6,a1		# adiciona y ao endereço base
	  add a6,t3,a6		# adiciona x ao endereço base		
	    
	  lb s8,0(a6)			# carrega o pixel 
	  
	  
	  li a3,0x00000000		# tile preto
	  beq s8,a3,KEY2		# se for igual, recomeça
	  
	  li a3,-64
	  beq s8,a3,KEY2
	  
	  li a5,2
	  
	  li a3,-57			# tile fruta
	  bne s8,a3,CONT_DIR		# se não tiver fruta, continua normal
	  jal PONTUACAO			# se tem fruta, muda a pontuação
	  
	  li t3,0xFF			# tile branco
	  sb t3,0(a6)			# troca o tile colorido (fruta) por branco
	  	  
CONT_DIR: la t1,OLD_CHAR_POS	# carrega em t1 oldcharpos
	  lw t2,0(t0)		
	  sw t2,0(t1)		# salva a posição atual em oldcharpos


	  la t0,CHAR_POS	# carrega o endereço do personagem
	  lh t1,0(t0)		
	  addi t1,t1,16		# aumenta 16 pixels
	  sh t1,0(t0)		# salva 
	  
	  la a0,char16dir
	  j PRINT_CHAR		# manda printar o personagem
	  
	  
	  
CHAR_CIMA: la t0,CHAR_POS	# carrega a posição do personagem

	  la t1,INIMIGO_POS
	  lw t3,0(t1)
	  lw t2,0(t0)
	  beq t3,t2,GAME_OVER

	  la t3,SCORE1
	  lh t2,0(t3)
	  li t4,800
	  beq t2,t4,FIM_FASE
	  
	  # CHECA COLISÃO ACIMA
	  mv a6,s7 
	  
	   lh t3,0(t0)			# carrega a posição x do personagem
	   lh t4,2(t0)			# carrega a posição y do personagem
	    
	  li t5,16
	  div t3,t3,t5
	  div t4,t4,t5
	    
	  addi t4,t4,-1		# decresce o y em 1
	    
	  li t5,20
	  mul a1,t4,t5		# a1 = y * 20
	  add a6,a6,a1		# adiciona y ao endereço base
	  add a6,t3,a6		# adiciona x ao endereço base		
	    
	  lb s8,0(a6)			# carrega o pixel 
	   
	  li a3,0x00000000		# tile preto
	  beq s8,a3,KEY2		# se for igual, recomeça
	  
	  li a3,-64
	  beq s8,a3,KEY2
	  
	  li a5,3
	  
	  li a3,-57			# tile fruta
	  bne s8,a3,CONT_CIMA		# se não tem fruta, anda normal
	  jal PONTUACAO			# se tem fruta, muda pontuação
	  
	 li t3,0xFF			# tile branco
	 sb t3,0(a6)			# troca o tile de fruta por branco
	  
	  
CONT_CIMA: la t1,OLD_CHAR_POS	# carrega a posição do oldcharpos
	   lw t2,0(t0)		
	   sw t2,0(t1)		# salva a posição do personagem em oldcharpos
	   
	   la t0,CHAR_POS	
	   lh t1,2(t0)		# carrega a posição y do personagem
	   addi t1,t1,-16	# diminiu 16 pixels
	   sh t1,2(t0)		# salva em charpos
	   
	   la a0,charcima
	   j PRINT_CHAR 	# manda printar o personagem
	   
CHAR_BAIXO: la t0,CHAR_POS	# carrega a posição do personagem

	  la t1,INIMIGO_POS
	  lw t3,0(t1)
	  lw t2,0(t0)
	  beq t2,t3,GAME_OVER
	    
	  la t3,SCORE1
	  lh t2,0(t3)
	  li t4,800
	  beq t2,t4,FIM_FASE
	    
	  # CHECA COLISÃO EMBAIXO
	   mv a6,s7
	   
	   lh t3,0(t0)			# carrega a posição x do personagem
	   lh t4,2(t0)			# carrega a posição y do personagem
	   
	  li t5,16
	  div t3,t3,t5
	  div t4,t4,t5
	    
	  addi t4,t4,1		# aumenta o y em 1
	    
	  li t5,20
	  mul a1,t4,t5		# a1 = y * 20
	  add a6,a6,a1		# adiciona y ao endereço base
	  add a6,t3,a6		# adiciona x ao endereço base		
	    
	  lb s8,0(a6)			# carrega o pixel 
	   
	  li a3,0x00000000		# tile preto
	  beq s8,a3,KEY2		# se for igual, recomeça
	  
	  li a3,-64
	  beq s8,a3,KEY2
	  
	  li a5,4
	  
	  li a3,-57			# tile fruta
	  bne s8,a3,CONT_BAIXO		# se o tile nao for fruta, anda normal
	  jal PONTUACAO			# se o tile for fruta, muda a pontuação
	  	  
	  li t3,0xFF			# tile branco
	  sb t3,0(a6)			# troca o tile de fruta pelo tile branco
	  	   
CONT_BAIXO: la t1,OLD_CHAR_POS	# carrega oldcharpos
	    lw t2,0(t0)		
	    sw t2,0(t1)		# salva a posição atual em oldcharpos
	    
	    la t0,CHAR_POS	# carrega charpos
	    lh t1,2(t0)		# carrega a posição y do personagem
	    addi t1,t1,16	# aumenta 16 pixels
	    sh t1,2(t0)		# salva en charpos
	    
	    la a0,charbaixo	
	    j PRINT_CHAR	# manda printar o personagem
	    
BLOCO_GELO: 
	la t0,CHAR_POS
	
	lh t3,0(t0)			# carrega a posição x do personagem
	lh t4,2(t0)			# carrega a posição y do personagem
	    
	li t5,16			# coloca nas dimensões do mapa de colisão
	div t3,t3,t5
	div t4,t4,t5
	
	li s4,1
	beq a5,s4,BLOCO_ESQ
	
	li s4,2
	beq a5,s4,BLOCO_DIR

	li s4,3
	beq a5,s4,BLOCO_CIMA
     
	li s4,4
	beq a5,s4,BLOCO_BAIXO
	
BLOCO_ESQ: 
	  mv a6,s7
	  addi t3,t3,-1		# descresce o x em 1
	    
	  li t5,20
	  mul a1,t4,t5		# a1 = y * 20
	  add a6,a6,a1		# adiciona y ao endereço base
	  add a6,t3,a6	        # adiciona x ao endereço base		
	    
	  lb s8,0(a6)			# carrega o pixel 
	  
	  li a3,0x00000000
	  beq a3,s8,KEY2
	  
	  li a3,-1
	  beq s8,a3,CRIAR_BLOCO_ESQ
	  
	  li a3,-64
	  beq s8,a3,QUEBRA_BLOCO_ESQ

BLOCO_DIR:
	mv a6,s7
	 addi t3,t3,1		# aumenta o x em 1
	    
	  li t5,20
	  mul a1,t4,t5		# a1 = y * 20
	  add a6,a6,a1		# adiciona y ao endereço base
	  add a6,t3,a6		# adiciona x ao endereço base		
	    
	  lb s8,0(a6)			# carrega o pixel 
	  
	  li a3,0x00000000
	  beq a3,s8,KEY2
	  
	  li a3,-1
	  beq s8,a3,CRIAR_BLOCO_DIR
	  
	  li a3,-64
	  beq s8,a3,QUEBRA_BLOCO_DIR

BLOCO_CIMA:
	 mv a6,s7
	 addi t4,t4,-1		# diminui o y em 1
	    
	  li t5,20
	  mul a1,t4,t5		# a1 = y * 20
	  add a6,a6,a1		# adiciona y ao endereço base
	  add a6,t3,a6		# adiciona x ao endereço base		
	    
	  lb s8,0(a6)			# carrega o pixel 
	  
	  li a3,0x00000000
	  beq a3,s8,KEY2
	  
	  li a3,-1
	  beq s8,a3,CRIAR_BLOCO_CIMA
	  
	  li a3,-64
	  beq s8,a3,QUEBRA_BLOCO_CIMA

BLOCO_BAIXO:
	mv a6,s7
	 addi t4,t4,1		# aumenta o y em 1
	    
	  li t5,20
	  mul a1,t4,t5		# a1 = y * 20
	  add a6,a6,a1		# adiciona y ao endereço base
	  add a6,t3,a6   	# adiciona x ao endereço base		
	    
	  lb s8,0(a6)			# carrega o pixel 
	  
	  li a3,0x00000000
	  beq a3,s8,KEY2
	  
	  li a3,-1
	  beq s8,a3,CRIAR_BLOCO_BAIXO
	  
	  li a3,-64
	  beq s8,a3,QUEBRA_BLOCO_BAIXO
	  
CRIAR_BLOCO_ESQ: 
	   li a0,60
	   li a1,250
	   li a2,76
	   li a3,80
	   li a7,31
	   ecall 
	   
	   la a0,iceblock
	   
	   la t3,CRIAR_OU_QUEBRAR
	   li t5,1
	   sh t5,0(t3)
	   
	   li t3,-64		# troca a cor do tile do mapa de colisão por azul
	   sb t3,0(a6)
	   
	   la t0,CHAR_POS
	   la t3,BLOCO_POS        
	   lw s8,0(t0)
	   sw s8,0(t3)

           lh t5,0(t3)
	   addi t5,t5,-16
	   sh t5,0(t3)
	   j PRINT_BLOCO
	   
CRIAR_BLOCO_DIR:
	   li a0,60
	   li a1,250
	   li a2,76
	   li a3,80
	   li a7,31
	   ecall 
	   	   
	   la a0,iceblock
	   
	   la t3,CRIAR_OU_QUEBRAR
	   li t5,1
	   sh t5,0(t3)
	   
	   li t3,-64		# troca a cor do tile do mapa de colisão por azul
	   sb t3,0(a6)
	   
	   la t0,CHAR_POS
	   la t3,BLOCO_POS         
	   lw s8,0(t0)
	   sw s8,0(t3)

           lh t5,0(t3)
	   addi t5,t5,16
	   sh t5,0(t3)
	   j PRINT_BLOCO 

CRIAR_BLOCO_CIMA: 
	   li a0,60
	   li a1,250
	   li a2,76
	   li a3,80
	   li a7,31
	   ecall 
	   
	   la a0,iceblock
	   
	   li t3,-64		# troca a cor do tile do mapa de colisão por azul
	   sb t3,0(a6)
	   
	   la t3,CRIAR_OU_QUEBRAR
	   li t5,1
	   sh t5,0(t3)
	   
	   la t0,CHAR_POS
	   la t3,BLOCO_POS         
	   lw s8,0(t0)
	   sw s8,0(t3)

           lh t5,2(t3)
	   addi t5,t5,-16
	   sh t5,2(t3)
	   j PRINT_BLOCO
	   
CRIAR_BLOCO_BAIXO: 
	   li a0,60
	   li a1,250
	   li a2,76
	   li a3,80
	   li a7,31
	   ecall 
	   
	   la a0,iceblock
	   
	   la t3,CRIAR_OU_QUEBRAR
	   li t5,1
	   sh t5,0(t3)
	   
	   li t3,-64		# troca a cor do tile do mapa de colisão por azul
	   sb t3,0(a6)
	   
	   la t0,CHAR_POS
	   la t3,BLOCO_POS         
	   lw s8,0(t0)
	   sw s8,0(t3)

           lh t5,2(t3)
	   addi t5,t5,16
	   sh t5,2(t3)
	   j PRINT_BLOCO
	   
QUEBRA_BLOCO_ESQ:
           li a0,60
	   li a1,250
	   li a2,127
	   li a3,80
	   li a7,31
	   ecall 

	   la a0,tile_neve
	
	   li t3,-1		# troca a cor do tile do mapa de colisão por branco
	   sb t3,0(a6)
	 
	   la t3,CRIAR_OU_QUEBRAR
	   li t5,2
	   sh t5,0(t3)
	   
	   la t0,CHAR_POS
	   la t3,BLOCO_POS         
	   lw s8,0(t0)
      	   sw s8,0(t3)

           lh t5,0(t3)
	   addi t5,t5,-16
	   sh t5,0(t3)
	   j PRINT_BLOCO
	 
QUEBRA_BLOCO_DIR:
	   li a0,60
	   li a1,250
	   li a2,127
	   li a3,80
	   li a7,31
	   ecall 
	   
	la a0,tile_neve
	
	 li t3,-1		# troca a cor do tile do mapa de colisão por branco
	 sb t3,0(a6)
	 
	  la t3,CRIAR_OU_QUEBRAR
	  li t5,2
	  sh t5,0(t3)
	   
	 la t0,CHAR_POS
	 la t3,BLOCO_POS         
	 lw s8,0(t0)
	 sw s8,0(t3)

         lh t5,0(t3)
	 addi t5,t5,16
	 sh t5,0(t3)
	 j PRINT_BLOCO
	 
QUEBRA_BLOCO_CIMA:
	   li a0,60
	   li a1,250
	   li a2,127
	   li a3,80
	   li a7,31
	   ecall 	
	   
	   la a0,tile_neve
	   
	   li t3,-1		# troca a cor do tile do mapa de colisão por branco
	   sb t3,0(a6)
	   
	   la t3,CRIAR_OU_QUEBRAR
	   li t5,2
	   sh t5,0(t3)
	   
	   la t0,CHAR_POS
	   la t3,BLOCO_POS         
	   lw s8,0(t0)
	   sw s8,0(t3)

           lh t5,2(t3)
	   addi t5,t5,-16
	   sh t5,2(t3)
	   j PRINT_BLOCO
	   
QUEBRA_BLOCO_BAIXO:
	   li a0,60
	   li a1,250
	   li a2,127
	   li a3,80
	   li a7,31
	   ecall 
	   
	   la a0,tile_neve
	   
	   li t3,-1		# troca a cor do tile do mapa de colisão por branco
	   sb t3,0(a6)
	   
	   la t3,CRIAR_OU_QUEBRAR
	   li t5,2
	   sh t5,0(t3)
	   
	   la t0,CHAR_POS
	   la t3,BLOCO_POS         
	   lw s8,0(t0)
	   sw s8,0(t3)

           lh t5,2(t3)
	   addi t5,t5,16
	   sh t5,2(t3)
	   j PRINT_BLOCO
		   
LOOP_BLOCO:  
	la t3,BLOCO_POS
	
	lh t6,0(t3)			# carrega a posição x do personagem
	lh t4,2(t3)			# carrega a posição y do personagem
	    
	li t5,16			# coloca nas dimensões do mapa de colisão
	div t6,t6,t5
	div t4,t4,t5
	
	li s4,1
	beq a5,s4,CONT_LOOP_ESQ
	
	li s4,2
	beq a5,s4,CONT_LOOP_DIR
	
	li s4,3
	beq a5,s4,CONT_LOOP_CIMA
	
	li s4,4
	beq a5,s4,CONT_LOOP_BAIXO
	
CONT_LOOP_ESQ: addi t6,t6,-1			# decresce o x em 1
	       j CONT_LOOP
	       
CONT_LOOP_DIR: addi t6,t6,1			# aumenta o x em 1
	       j CONT_LOOP
	       
CONT_LOOP_CIMA: addi t4,t4,-1			# diminui o y em 1
		j CONT_LOOP

CONT_LOOP_BAIXO: addi t4,t4,1			# aumenta o y em 1
		j CONT_LOOP
		
CONT_LOOP: li t5,20

	mv a6,s7 
	 mul a1,t4,t5		# a1 = y * 20
	 add a6,a6,a1		# adiciona y ao endereço base
	 add a6,t6,a6		# adiciona x ao endereço base		
	    
	 lb s8,0(a6)			# carrega o pixel 
	
	li a3,0x00000000
	beq a3,s8,KEY2
	
	li a3,-57
	beq a3,s8,KEY2
	
	la t3,CRIAR_OU_QUEBRAR
	lh t6,0(t3)
	
	li t2,1
	beq t2,t6,COND_CRIAR
	
	li t2,2
	beq t2,t6,COND_QUEBRAR
	
COND_CRIAR:
	li a3,-64
	beq a3,s8,KEY2
	
	li t5,-64		# troca a cor do tile do mapa de colisão por azul
	sb t5,0(a6)
	j CONTINUACAO
	
COND_QUEBRAR:
	li a3,-1
	beq a3,s8,KEY2
	
	li t5,-1
	sb t5,0(a6)
	j CONTINUACAO
	
CONTINUACAO:	
	li s4,1
	beq a5,s4,LOOP_2_ESQ
	
	li s4,2
	beq a5,s4,LOOP_2_DIR
	
	li s4,3
	beq a5,s4,LOOP_2_CIMA
	
	li s4,4
	beq a5,s4,LOOP_2_BAIXO
	
LOOP_2_ESQ: la t3,BLOCO_POS
	    lh t5,0(t3)
	    addi t5,t5,-16
	    sh t5,0(t3)
	    
	    la t2,INIMIGO_POS
	    lw t4,0(t2)
	    lw t5,0(t3)
	    
	    beq t4,t5,KEY2
	    
	    j PRINT_BLOCO
	    
LOOP_2_DIR: la t3,BLOCO_POS
	    lh t5,0(t3)
	    addi t5,t5,16
	    sh t5,0(t3)
	    
	    la t2,INIMIGO_POS
	    lw t4,0(t2)
	    lw t5,0(t3)
	    
	    beq t4,t5,KEY2
	    j PRINT_BLOCO
	    
LOOP_2_CIMA: la t3,BLOCO_POS
             lh t5,2(t3)
	     addi t5,t5,-16
	     sh t5,2(t3)
	     
	    la t2,INIMIGO_POS
	    lw t4,0(t2)
	    lw t5,0(t3)
	    
	    beq t4,t5,KEY2
	     j PRINT_BLOCO
	     
LOOP_2_BAIXO: la t3,BLOCO_POS
	      lh t5,2(t3)
	      addi t5,t5,16
	      sh t5,2(t3)
	      
	    la t2,INIMIGO_POS
	    lw t4,0(t2)
	    lw t5,0(t3)
	    
	    beq t4,t5,KEY2
	      j PRINT_BLOCO
	  	   
PRINT_BLOCO:
	   xori s0,s0,1
	   la t3,BLOCO_POS
	   lh a1,0(t3)		# carrega x do bloco em a1
	   lh a2,2(t3)		# carrega y do bloco em a2
	   mv a3,s0		# carrega o valor do frame em a3
	   call PRINT 		# imprime o sprite 	  
	   
	   xori s0,s0,1		# troca de frame
	   la t3,BLOCO_POS
	   lh a1,0(t3)		# carrega x do bloco em a1
	   lh a2,2(t3)		# carrega y do bloco em a2
	   mv a3,s0		# carrega o valor do frame em a3
	   call PRINT 		# imprime o sprite
	     
	  j LOOP_BLOCO
	   
	     	   
PONTUACAO:
	li a0,72
	li a1,250
	li a2,32
	li a3,80
	li a7,31
	ecall 
		 	   		   
	la s9,SCORE1
	lh t2,0(s9)
	addi t2,t2,100
	sh t2,0(s9)
	
	mv a0,t2
	li a1,160
	li a2,5
	li a3,0xFF00
	li a4,0
	
        li a7,101
	ecall 
	
	mv a0,t2
	li a1,160
	li a2,5
	li a3,0xFF00
	li a4,1
	
	li a7,101
	ecall 
	
#	li t3,800
#	beq t2,t3,FIM_FASE
	ret

GAME_OVER:
	la a0,gameover
	li a1,0
	li a2,0
	li a3,0
	call PRINT
	li a3,1
	call PRINT
	
	li a7,10
	ecall
		   
FIM_FASE:
	la a0,STRING1
	li a7,4
	ecall
	
	la t0,CHAR_POS
	li t2,32
	sh t2,0(t0)
	li t2,16
	sh t2,2(t0)
	
	la t1,OLD_CHAR_POS
	li t2,32
	sh t2,0(t1)
	li t2,16
	sh t2,2(t1)
	
	la t5,DIR_INIMIGO
	li t6,1
	sw t6,0(t5)
	
	la t5,SCORE1
	li t6,0
	sw t6,0(t5)
	
	la t5,BLOCO_POS
	li t6,0
	sh t6,0(t5)
	li t6,0
	sh t6,2(t5)
	
	li t1,3
	beq t1,s11,FIM_JOGO
	
	la a0,level2
	li a1,0
	li a2,0
	li a3,0
	call PRINT
	li a3,1
	call PRINT
	
	li t1,2
	beq t1,s11,SET_FASE3
	
	
SET_FASE2:	
	la t3,INIMIGO_POS	# coloca o inimigo em uma nova posição
	li t2,32
	sh t2,0(t3)
	li t2,112
	sh t2,2(t3)
	
	la t4,OLD_INIMIGO_POS
	li t2,32
	sh t2,0(t4)
	li t2,112
	sh t2,2(t4)
	
	li s7,0
	li a2,0
	li a6,0
	la s7,mapacolisaof22	# define s7 agora com o novo mapa de colisão
	li s11,2		# fase 2
	
	la a0,mapafase21	# carrega o endereço do map em a0
	li a1,0			# x = 0
	li a2,0			# y = 0
	li a3,0			# frame = 0
	call PRINT		# imprime o mapa
	li a3,1			# imprime o mapa no outro frame
	call PRINT
	
	la a0,STRING2		# string do menu
	li a1,0
	li a2,5
	li a3,0xFF00
	li a4,1
	li a7,104
	ecall 
	
	la a0,STRING2
	li a1,0
	li a2,5
	li a3,0xFF00
	li a4,0
	li a7,104
	ecall 
	
	la a0,melancia		# carrega o endereço do map em a0
	li a1,20		# x = 0
	li a2,0			# y = 0
	li a3,0			# frame = 0
	call PRINT		# imprime a fruta (objetivo)
	li a3,1
	call PRINT
	
	j SET_MUSICA
	
INIMIGO_2:
	la t0,DIR_INIMIGO
	lw t2,0(t0)
	
	li t1,1
	beq t1,t2,DIREITA
	
	li t1,0
	beq t1,t2,ESQUERDA
	
SET_FASE3:
	la t3,INIMIGO_POS	# coloca o inimigo em nova posição
	li t2,144
	sh t2,0(t3)
	li t2,176
	sh t2,2(t3)
	
	la t4,OLD_INIMIGO_POS
	li t2,144
	sh t2,0(t4)
	li t2,176
	sh t2,2(t4)
	
	li s7,0
	li a6,0
	li a2,0
	la s7,mapacolisaof3	# s7 agora é um novo mapa de colisão
	li s11,3		# fase 3
	
	la a0,mapafase3		# carrega o endereço do map em a0
	li a1,0			# x = 0
	li a2,0			# y = 0
	li a3,0			# frame = 0
	call PRINT		# imprime o mapa
	li a3,1			
	call PRINT
	
	la a0,STRING3		# string do menu fase 3
	li a1,0
	li a2,5
	li a3,0xFF00
	li a4,1
	li a7,104
	ecall 
	
	la a0,STRING3
	li a1,0
	li a2,5
	li a3,0xFF00
	li a4,0
	li a7,104
	ecall 
	
	la a0,laranja2		# carrega o endereço do map em a0
	li a1,20		# x = 0
	li a2,0		        # y = 0
	li a3,0			# frame = 0
	call PRINT		# imprime a fruta (objetivo)
	li a3,1
	call PRINT
	
	j SET_MUSICA
	
FIM_JOGO:
	la a0,youwin2
	li a1,0
	li a2,0
	li a3,0
	call PRINT
	li a3,1
	call PRINT

	
	li a7,10
	ecall

PRINT:	    li t0,0xFF0			# carrega 0xFF0 em t0
	    add t0,t0,a3		# adiciona o frame ao FF0 (se for 1 vira FF1, se for 0 vira FF0)
	    slli t0,t0,20		# shift de 20 bits pra esquerda
	    
	    add t0,t0,a1		# adiciona x ao t0
	    
	    li t1,320			# t1 = 320
	    mul t1,t1,a2 		# t1 = 320¨* y
	    add t0,t0,t1		# adiciona t1 a0 t0
	    
	    addi t1,a0,8		
	    
	    mv t2,zero			# zera t2
	    mv t3,zero			# zera t3
	    
	    lw t4,0(a0)			# carrega a largura em t4
	    lw t5,4(a0)			# carrega a altura em t5
	    
PRINT_LINHA: 
	    lw t6,0(t1)			# carrega em t6 4 pixels imagem
	    sw t6,0(t0)			# imprime no bitmap a word da imagem
	    
	    addi t0,t0,4		# incrementa endereco do bitmap
	    addi t1,t1,4		# incrementa endereco da imagem 
	    
	    addi t3,t3,4		# incrementa contador de coluna
	    blt t3,t4,PRINT_LINHA	# se contador da coluna < largura, continue imprimindo
	    
	    addi t0,t0,320		
	    sub t0,t0,t4		# t0 - largura da imagem
	    
	    mv t3,zero			# zera t3(contador de coluna)
	    addi t2,t2,1		# incrementa contador de linha
	    bgt t5,t2,PRINT_LINHA	# se altura > contador de linha continue imprimindo
	    
	    ret
	    
.data

# sprites
.include "Exemplos/tile_neve.data"
.include "Exemplos/mapfase12.data"
.include "Exemplos/char16dir.data"
.include "Exemplos/charesquerda.data"
.include "Exemplos/charcima.data"
.include "Exemplos/charbaixo.data"
.include "Exemplos/iceblock.data"
.include "Exemplos/outrotile.data"
.include "Exemplos/enemy2.data"
.include "Exemplos/inimigo_baixo.data"
.include "Exemplos/inimigo_esq.data"
.include "Exemplos/inimigo_dir.data"
.include "Exemplos/inimigo_cima.data"
.include "Exemplos/blueberry.data"
.include "Exemplos/mapafase21.data"
.include "Exemplos/melancia.data"
.include "Exemplos/inimigo2esq.data"
.include "Exemplos/inimigo2dir.data"
.include "Exemplos/menu.data"
.include "Exemplos/mapafase3.data"
.include "Exemplos/laranja2.data"
.include "Exemplos/youwin2.data"
.include "Exemplos/level2.data"
.include "Exemplos/gameover.data"

# mapa de colisão
.include "Exemplos/mapacolisaof14.data"
.include "Exemplos/mapacolisaof22.data"
.include "Exemplos/mapacolisaof3.data"

.include "SYSTEMv21.s"
