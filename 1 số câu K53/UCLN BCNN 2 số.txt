; AddTwo.asm - adds two 32-bit integers.
; Chapter 3 example
INCLUDE Irvine32.inc
.386
.model flat,stdcall
.stack 4096
ExitProcess proto,dwExitCode:dword

.data
msg_01 db 'Nhap 1 so: ',0
msg_02 db 'UCLN la: ',0
msg_03 db 'BCNN la: ',0
a db ?
b db ?
uclnNum db ?
bcnnNum db ?
.code
main proc    
    call ReadNUm
    call UCLN
    call BCNN
    call Print
	invoke ExitProcess,0
main endp
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
ReadNum PROC
     MOV EDX,OFFSET msg_01
     CALL WriteString
     CALL ReadDec
     MOV a,AL    
     CALL ReadDec
     MOV b,AL 
     RET
ReadNum ENDP
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
UCLN PROC
     MOV AL,a
     MOV BL,b
     .WHILE AL != BL
          .IF AL < BL
               SUB BL,AL
          .ELSEIF AL > BL
               SUB AL,BL
          .ENDIF
     .ENDW
     MOV uclnNum,AL;ucln 2 so
     RET
UCLN ENDP
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
BCNN PROC
     MOV AL,a
     MOV BL,b
     MUL BL;AX=a*b
     MOV DL,uclnNum
     DIV DL;AX=a*b/ucln
     MOV bcnnNum,AL;thanh ghi AL chua phan thuong cua phep chia
     RET
BCNN ENDP
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
print PROC
       MOV EDX,offset msg_02
       call writestring
       MOV EAX,0
       MOV AL,uclnNum
       call writeDec
       call CRLF

       MOV EDX,offset msg_03
       call writestring
       MOV EAX,0
       MOV AL,bcnnNum
       call writeDec
       call CRLF
       RET
print ENDP
end main