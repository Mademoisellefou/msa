# msa
primo
```


mostrarcad macro text
push ax
push dx
lea dx,text
mov ah,09h 
int 21h
pop dx
pop ax
endm

.model small
.stack 64
.data
 espr db 10,13,"==========ES PRIMO=============",10,13,"$"
 noespr  db 10,13,"==========NO ES PRIMO=========",10,13,"$"
 x dw 0
.code
inicio:

    mov ax, @data
    mov ds, ax
;=
    call leernum
    mov  x,ax
    call esprimo
    ;---
    mov ax, 4c00h 
    int 21h    
;=
    leernum proc 
    push cx
    push bx
    mov ax,0
    mov bx,10
    mov cx,0
    c1:
    push ax
    mov ah,01h
    int 21h
    cmp al,13
    je sc1   
    sub al,30h
    mov cl,al
    pop ax
    mul bx
    add ax,cx
    jmp c1  
    sc1:
    pop ax
    pop bx
    pop cx
    ret
    leernum endp
;=
    esprimo proc 
    push ax
    push bx
    push cx
    push dx    
    mov bx,2
    mov ax,x
    div bx 
    cmp dx,0
    je noesprimo
    mov dx,0
    ;=
    mov cx,1
    c3:   
    inc cx
    mov bx,2
    cmp cx,x
    ja noesprimo
    cmp cx,2
    je insert
    cmp cx,3
    je insert
    cmp cx,4
    jae verificar
    verificar:
    mov ax,cx
    div bx   
    cmp dx,0
    je c3       
    mov dx,0
    add bx,1
    cmp bx,ax
    jge insert
    jmp verificar
    insert:
    cmp cx,x
    je xprimo
    jmp c3   
    xprimo:   
    mostrarcad espr
    jmp sc4
    noesprimo:
    mostrarcad noespr
    jmp sc4          
    sc4:
    ;=
    pop dx
    pop cx
    pop bx
    pop ax
    ret 
    esprimo endp
end inicio
```
