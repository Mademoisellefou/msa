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

data segment   
 salto db 10,13,"$"    
 espr db 10,13,"El numerode Pares es : ",10,13,"$"
 y dw 0
 num db 12 dup("$")    
 cad db 12 dup("$")    
 nrod db 0
ends

stack segment
    dw   128  dup(0)
ends

code segment
start:

    mov ax, data
    mov ds, ax
    mov es, ax
;=
    call leernum 
	mov ax,0
	mov ax,nrod
    call numcad   
    mostrarcad salto
    mostrarcad num
    ;mostrarcad espr
    ;mostrarcad cad

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
    call  eseven
    mov cl,al
    pop ax
    mul bx
    add ax,cx
	add nrod,1
    jmp c1  
    sc1:
    pop ax
    pop bx
    pop cx
    ret
    leernum endp
;=
    numcad proc
    push ax
    push bx
    push cx
    push dx
    mov cx,0
    mov bx,10
    mov si,0
    c6:   
    div bx
    push dx
    mov dx,0
    inc cx
    cmp ax,0
    je sc6
    jmp c6
    sc6:    
    pop dx
    add dl,30h
    mov num[si],dl
    inc si
    loop sc6
    pop dx
    pop cx
    pop bx
    pop ax
    ret
    numcad endp        
;=
    eseven proc 
    push cx
    push bx 
    push ax
    mov cx,ax
    mov ah,0
    mov bx,2
    mov dx,0
    div bx
    cmp dx,0
    jne noinsert
    mov si,y 
    mov dl,cl
    add dl,30h
    mov cad[si],dl
    add y,1
    mov ax,cx
    noinsert:
    pop ax 
    pop bx
    pop cx    
    ret 
    eseven endp

ends

end start
```
