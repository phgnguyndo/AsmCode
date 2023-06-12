; AddTwo.asm - adds two 32-bit integers.
; Chapter 3 example
INCLUDE Irvine32.inc
.386
.model flat,stdcall
.stack 4096
ExitProcess proto,dwExitCode:dword
CheckSoNguyenTo PROTO, number:DWORD
Print PROTO,number:DWORD
.data
num dword ?
flag db ?
msg_01 db 'NHap so n: ',0
.code
main proc    
     call ReadNum
     MOV EDX,2
     .WHILE EDX < Num
          invoke CHeckSoNguyenTo,EDX
          invoke Print,EDX
          INC EDX
     .ENDW
	invoke ExitProcess,0
main endp
ReadNum PROC uses eax ebx ecx edx
     MOV EDX,OFFSET msg_01
     Call WriteString
     Call ReadInt
     MOV num,EAX
     ret
ReadNum ENDP
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
CHeckSoNguyenTo PROC uses eax ebx ecx edx, number:DWORD
     .IF number < 2
          MOV flag,0;return false
     .ENDIF
     MOV EDX,0
     MOV EAX,number
     MOV EBX,2
     DIV EBX     
     MOV EDI,EAX;num/2
     MOV ESI,2;i=2

     .WHILE ESI <= EDI
          MOV EDX,0
          MOV EAX,number
          DIV ESI

          .IF EDX ==0
               MOV flag,0;return false
               JMP endd
          .ENDIF
          INC ESI
     .ENDW
     MOV flag,01;return true
     endd:
          ret
CHeckSoNguyenTo ENDP
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
Print PROC uses eax ebx ecx edx,number:Dword              
     .IF flag==1;la so nguyen to
          MOV EAX,number
          call WriteDec
          call CRLF
     .ENDIF
     ret        
Print ENDP
end main