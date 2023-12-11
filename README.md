# sorting-an-array
 # mips
 
.data
A: .space 200
N: .word 0
I: .word 0
J: .word 0
AIDE: .word 0
PMAX: .word 0
msg1: .asciiz "Dimension du tableau (max.50) :"
msg2: .asciiz "Element : "
msg3: .asciiz "Tableau donné :"
msg4: .asciiz "Tableau trie :"
msg5: .asciiz " "

.text 
main:

    la $a0, msg1
    li $v0, 4
    syscall       # afficher msg

    li $v0, 5
    syscall       # lire N
    sw $v0, N

    la $s0, A       # @ de base de A
    li $s1, 0       # J=0
    li $s2, 0       # I=0
    move $s3, $v0  # N
    lw $s4 , PMAX($0)
    lw $s5 , AIDE($0)
    li $t0, 4       # 4 pour le decalage dans A


for1:  # for (J=0; J<N; J++)  remplir A
    bge $s1, $s3, endfor1

    la $a0, msg2
    li $v0, 4
    syscall       # afficher msg

    li $v0, 5
    syscall       # lire la valeur d'element

    mul $t1, $s1 , $t0  # calculer le decalage j*4
    add $t1, $t1, $s0  # calculer @ d'element @A + j*4
    sw $v0, 0($t1)     # A(j) = valeur

    addi $s1, $s1, 1    # j++
    j for1

endfor1:  # afficher A

    li $s1, 0   # j=0

    la $a0, msg3
    li $v0, 4
    syscall       # afficher msg

for2:  # for (J=0; J<N; J++)
    bge $s1, $s3, endfor2

    mul $t1 , $s1 ,$t0 #calculer le decalage
    add $t1, $t1, $s0  #calculer @A + le decalage
    lw $a0, 0($t1)

    li $v0, 1
    syscall

    la $a0, msg5 
    li $v0, 4
    syscall  # afficher l'espace "  "

    addi $s1, $s1, 1  #j++
    j for2

endfor2:

li $s1 , 1  # j=1
li $s2 , 0  #i=0
addi $t2 , $s3 , -1  # $t2 = N-1


for3:  # for (I=0; I<N-1; I++)
    bge $s2, $t2, endfor3  
    #  Recherche du maximum à droite de A[I] 
    move $s4 , $s2  # PMAX=I
    
for4:  #for (J=I+1; J<N; J++)
    bge $s1, $s3, endfor4  # for (J=I+1; J<N; J++)

    mul $s4, $s4, $t0  # pmax*4
    add $s4, $s4, $s0  # pmax*4 + @A
    lw $s4, 0($s4)  # $s4 = A[pmax*4 + @A]

    mul $t5, $s1, $t0  # j*4
    add $t5, $t5, $s0  # j*4 + @A
    lw $t5, 0($t5)  # $t4 = A[j*4 + @A]

    blt $t5, $s4, endif  #if (A[J]>A[PMAX])
    move $s4, $s1   # Pmax = j
    addi $s1, $s1, 1
    j for4

endif:
    addi $s1, $s1, 1
    j for4

endfor4: # Echange de A[I] avec le maximum 

    mul $t5, $s2, $t0  # i*4
    add $t5, $t5, $s0  # i*4 + @A 

    lw $s5, 0($t5)  # AIDE = A[I]

    mul $s4 , $s4 , $t0  # PMAX = PMAX * 4
    add $s4 , $s4 , $s0  # @PMAX = PMAX*4 + @A

    lw $t6 , 0($s4) #$t6 = A[PMAX]
    sw $t6 , 0($t5)  # A[I] = A[PMAX]

    sw $s5, 0($s4)  # A[PMAX] = AIDE

    addi $s2, $s2, 1
    j for3

endfor3:  # Edition du resultat

la $a0, msg4
li $v0, 4
syscall       # afficher msg


    li $s1, 0   # j=0

for5:  # for (J=0; J<N; J++)
    bge $s1, $s3, exit

    mul $t1, $s1, $t0 #j*4
    add $t1, $t1, $s0 #@A + j*4

    lw $a0, 0($t1)  # $a0 = A[J] 
  
    li $v0, 1
    syscall  #afficher A[J]

    la $a0, msg5
    li $v0, 4
    syscall  

    addi $s1, $s1, 1 #j++
    j for5

exit:
    li $v0, 10  # fin programme
    syscall


 
#c

#include <stdio.h>
main()
{
/* Déclarations */
int A[50];/*tableau donné */
int N; /*dimension */
int I; /* indice élément tableau */
int J; /* indice élément tableau */
int AIDE; /* pour la permutation */
int PMAX; /* indique la position de l'élément */
/* maximal à droite de A[I] */
/* Saisie des données */
printf("Dimension du tableau (max.50) : ");
scanf("%d", &N );
for (J=0; J<N; J++)
{
printf("Elément %d : ", J);
scanf("%d", &A[J]);
}
/* Affichage du tableau */
printf("Tableau donné :\n");
for (J=0; J<N; J++)
printf("%d ", A[J]);
printf("\n");
/* Tri du tableau */
for (I=0; I<N-1; I++)
{
/* Recherche du maximum à droite de A[I] */
PMAX=I;
for (J=I+1; J<N; J++)
if (A[J]>A[PMAX]) PMAX=J;
/* Echange de A[I] avec le maximum */
AIDE=A[I];
A[I]=A[PMAX];
A[PMAX]=AIDE;
}
/* Edition du résultat */
printf("Tableau trié :\n");
for (J=0; J<N; J++)
printf("%d ", A[J]);
printf("\n");
return 0;
}

