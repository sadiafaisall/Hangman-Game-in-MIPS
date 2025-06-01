.data
# Word bank
words:
    .word word0, word1, word2, word3, word4, word5, word6, word7, word8, word9,
          word10, word11, word12, word13, word14, word15, word16, word17, word18, word19

word0:  .asciiz "apple"   
word1:  .asciiz "mouse"
word2:  .asciiz "debug"   
word3:  .asciiz "sugar"  
word4:  .asciiz "clock"   
word5:  .asciiz "print"   
word6:  .asciiz "table"   
word7:  .asciiz "zebra"   
word8:  .asciiz "route" 
word9:  .asciiz "light"   
word10: .asciiz "cache"   
word11: .asciiz "grape"   
word12: .asciiz "login"   
word13: .asciiz "panel"   
word14: .asciiz "brave"   
word15: .asciiz "stone"   
word16: .asciiz "flame"   
word17: .asciiz "array"   
word18: .asciiz "candy"   
word19: .asciiz "click"  
display:       .space 10

# Messages
guess_msg:     .asciiz "\nGuess a letter: "
correct_msg:   .asciiz "\nCorrect!\n"
wrong_msg:     .asciiz "\nWrong guess.\n"
win_msg:       .asciiz "\nYou win! The word was: "
lose_msg:      .asciiz "\nYou lose! The word was: "
newline:       .asciiz "\n"

# Hangman parts
hang0:   .asciiz " _______          \n"     # top bar (always shown)
hang1:   .asciiz "|        |         \n"     # rope
hang2:   .asciiz "|       (_)       \n"     # head
hang3:   .asciiz "|        |        \n"     # body
hang4:   .asciiz "|       \\|        \n"     # one arm
hang5:   .asciiz "|       \\|/       \n"     # both arms
hang6:   .asciiz "|        |        \n"     # torso
hang10: .asciiz " |       |\n"

hang7:   .asciiz "|       /|          \n"     # one leg
hang8:   .asciiz "|      / |\\        \n"     # both legs
hang9:   .asciiz "|_______           \n"     # bottom (always shown)

# Steps excluding top/bottom
hangman_steps:
    .word hang1, hang2, hang3, hang4, hang5, hang6, hang7, hang8,hang9,hang10

.text
.globl main

main:
    # Random index 0-5
    li $v0, 30
    syscall
    li $t0, 20
    remu $t1, $a0, $t0

    la $t2, words
    sll $t3, $t1, 2
    add $t2, $t2, $t3
    lw $s2, 0($t2)

    # Init display to underscores
    la $t0, display
    li $t1, 5
init_loop:
    li $t2, '_'
    sb $t2, 0($t0)
    addi $t0, $t0, 1
    addi $t1, $t1, -1
    bgtz $t1, init_loop

    li $s0, 0      # wrong guesses
    li $s1, 0      # correct guesses

game_loop:
    li $v0, 4
    la $a0, newline
    syscall

    # Always draw top
    li $v0, 4
    la $a0, hang0
    syscall

    # Draw body based on wrong guesses
    li $t3, 0
draw_loop:
    bge $t3, $s0, draw_end
    sll $t4, $t3, 2
    la $t5, hangman_steps
    add $t5, $t5, $t4
    lw $a0, 0($t5)
    li $v0, 4
    syscall
    addi $t3, $t3, 1
    j draw_loop

draw_end:
    # Always draw bottom
    li $v0, 4
    la $a0, hang9
    syscall

    # Show wrong guess count: X / 8
    li $v0, 1
    move $a0, $s0
    syscall

    li $v0, 11
    li $a0, 32     # space
    syscall

    li $v0, 11
    li $a0, 47     # '/'
    syscall

    li $v0, 11
    li $a0, 32     # space
    syscall

    li $v0, 1
    li $a0, 8      # total guesses
    syscall

    li $v0, 4
    la $a0, newline
    syscall

    # Print word display
    li $v0, 4
    la $a0, newline
    syscall

    li $t1, 0
    la $t0, display
print_display:
    lb $a0, 0($t0)
    li $v0, 11
    syscall
    li $a0, 32
    li $v0, 11
    syscall
    addi $t0, $t0, 1
    addi $t1, $t1, 1
    li $t2, 5
    blt $t1, $t2, print_display

    # Ask for input
    li $v0, 4
    la $a0, guess_msg
    syscall
    li $v0, 12
    syscall
    move $t5, $v0

    # Check guess
    li $t2, 0
    li $t3, 0
    move $t0, $s2
    la $t1, display

check_loop:
    lb $t4, 0($t0)
    beqz $t4, check_end
    beq $t4, $t5, match_found
    addi $t0, $t0, 1
    addi $t1, $t1, 1
    addi $t2, $t2, 1
    j check_loop

match_found:
    lb $t6, 0($t1)
    beq $t6, $t5, skip_dup
    sb $t5, 0($t1)
    li $t3, 1
    addi $s1, $s1, 1
skip_dup:
    addi $t0, $t0, 1
    addi $t1, $t1, 1
    j check_loop

check_end:
    beqz $t3, wrong
    li $v0, 4
    la $a0, correct_msg
    syscall
    j check_win

wrong:
    li $v0, 4
    la $a0, wrong_msg
    syscall
    addi $s0, $s0, 1

check_win:
    li $t0, 5
    beq $s1, $t0, won
    li $t1, 8
    beq $s0, $t1, lost
    j game_loop

won:
    li $v0, 4
    la $a0, win_msg
    syscall
    move $a0, $s2
    li $v0, 4
    syscall
    j exit

lost:
    # Draw full hangman on loss
    li $v0, 4
    la $a0, hang0
    syscall

    li $t3, 0
draw_full:
    li $t2, 8          # number of body parts
    bge $t3, $t2, draw_bottom

    sll $t4, $t3, 2
    la $t5, hangman_steps
    add $t5, $t5, $t4
    lw $a0, 0($t5)
    li $v0, 4
    syscall
    addi $t3, $t3, 1
    j draw_full

draw_bottom:
    li $v0, 4
    la $a0, hang9
    syscall

    # Show lose message
    li $v0, 4
    la $a0, lose_msg
    syscall
    move $a0, $s2
    li $v0, 4
    syscall
    j exit


exit:
    li $v0, 10
    syscall

