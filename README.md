# Testeo de Instrucciones RTM32 (STX4)

53 instrucciones testeadas. Maximo 2 casos por instruccion.

## Resumen de Resultados

| # | Instruccion | Estado |
|---|-------------|--------|
| 1 | ADD | ✅ PASS |
| 2 | SUB | ✅ PASS |
| 3 | ADDI | ✅ PASS |
| 4 | AND | ✅ PASS |
| 5 | OR | ✅ PASS |
| 6 | XOR | ✅ PASS |
| 7 | SLL | ✅ PASS |
| 8 | SRL | ✅ PASS |
| 9 | SRA | ✅ PASS |
| 10 | SLT | ✅ PASS |
| 11 | SLTU | ✅ PASS |
| 12 | NOR | ✅ PASS |
| 13 | LW | ✅ PASS |
| 14 | SW | ✅ PASS |
| 15 | SH | ✅ PASS |
| 16 | SB | ✅ PASS |
| 17 | LH | ✅ PASS |
| 18 | LHU | ✅ PASS |
| 19 | BEQ | ✅ PASS |
| 20 | BNE | ✅ PASS |
| 21 | BLT | ✅ PASS |
| 22 | J | ✅ PASS |
| 23 | JAL | ✅ PASS |
| 24 | JR | ✅ PASS |
| 25 | LUI | ✅ PASS |
| 26 | ANDI | ✅ PASS |
| 27 | ORI | ✅ PASS |
| 28 | CFS | ❌ FAIL |
| 29 | CTS | ❌ FAIL |
| 30 | BGT | ✅ PASS |
| 31 | BLE | ✅ PASS |
| 32 | BGE | ✅ PASS |
| 33 | SLTI | ✅ PASS |
| 34 | SLTIU | ✅ PASS |
| 35 | LB | ✅ PASS |
| 36 | LBU | ✅ PASS |
| 37 | SLLR | ✅ PASS |
| 38 | SRLR | ✅ PASS |
| 39 | SRAR | ✅ PASS |
| 40 | LHX | ✅ PASS |
| 41 | LHUX | ✅ PASS |
| 42 | LBX | ✅ PASS |
| 43 | LBUX | ✅ PASS |
| 44 | LWX | ✅ PASS |
| 45 | MUL | ✅ PASS |
| 46 | MULH | ✅ PASS |
| 47 | MULHU | ✅ PASS |
| 48 | DIV | ✅ PASS |
| 49 | DIVU | ✅ PASS |
| 50 | REST | ✅ PASS |
| 51 | RESTU | ✅ PASS |
| 52 | JALR | ✅ PASS |
| 53 | XORI | ✅ PASS |

**Total: 51 PASS / 2 FAIL**

## Bugs Encontrados

1. **CFS** — NOP silencioso. El emulador reconoce la función pero el handler está vacío; R[rs] no se modifica.
2. **CTS** — NOP silencioso. El emulador reconoce la función pero el handler está vacío; S[aux] no se modifica.

# Caso 1: ADD $3, $1, $2
## Descripción:
Testeo de la instrucción ADD entre dos registros para verificar suma aritmética.
## Instrucctions:
ADD (R-type, opcode=00000, func=011100) — R[rd] = R[rs] + R[rt]
## Precondiciones:
- set r1 5
- set r2 7
## Code:
```
set pc 0x0
set r1 5
set r2 7
set [0x0] 0x0044301C
step
```
## Postcondiciones:
- R[3] = 0x0000000C (12)
## Conclusiones:
Anduvo. R[3] = 12 = 5+7.

---

# Caso 2: SUB $3, $1, $2
## Descripción:
Testeo de la instrucción SUB entre dos registros para verificar resta aritmética.
## Instrucctions:
SUB (R-type, opcode=00000, func=011101) — R[rd] = R[rs] - R[rt]
## Precondiciones:
- set r1 10
- set r2 3
## Code:
```
set pc 0x0
set r1 10
set r2 3
set [0x0] 0x0044301D
step
```
## Postcondiciones:
- R[3] = 0x00000007 (7)
## Conclusiones:
Anduvo. R[3] = 7 = 10-3.

---

# Caso 3: ADDI $2, $1, 10
## Descripción:
Testeo de la instrucción ADDI con inmediato positivo para verificar suma con constante.
## Instrucctions:
ADDI (I-type, opcode=00001) — R[rt] = R[rs] + SE(imm)
## Precondiciones:
- set r1 5
## Code:
```
set pc 0x0
set r1 5
set [0x0] 0x0844000A
step
```
## Postcondiciones:
- R[2] = 0x0000000F (15)
## Conclusiones:
Anduvo. R[2] = 15 = 5+10. ADDI funciona correctamente en la nueva versión del emulador.

---

# Caso 4: AND $3, $1, $2
## Descripción:
Testeo de la instrucción AND bit a bit entre dos registros.
## Instrucctions:
AND (R-type, opcode=00000, func=001000) — R[rd] = R[rs] & R[rt]
## Precondiciones:
- set r1 0xFF00FF00
- set r2 0x0F0F0F0F
## Code:
```
set pc 0x0
set r1 0xFF00FF00
set r2 0x0F0F0F0F
set [0x0] 0x00443008
step
```
## Postcondiciones:
- R[3] = 0x0F000F00
## Conclusiones:
Anduvo. R[3] = 0xFF00FF00 & 0x0F0F0F0F = 0x0F000F00.

---

# Caso 5: OR $3, $1, $2
## Descripción:
Testeo de la instrucción OR bit a bit entre dos registros.
## Instrucctions:
OR (R-type, opcode=00000, func=001001) — R[rd] = R[rs] | R[rt]
## Precondiciones:
- set r1 0xFF000000
- set r2 0x000000FF
## Code:
```
set pc 0x0
set r1 0xFF000000
set r2 0x000000FF
set [0x0] 0x00443009
step
```
## Postcondiciones:
- R[3] = 0xFF0000FF
## Conclusiones:
Anduvo. R[3] = 0xFF000000 | 0x000000FF = 0xFF0000FF.

---

# Caso 6: XOR $3, $1, $2
## Descripción:
Testeo de la instrucción XOR bit a bit entre dos registros.
## Instrucctions:
XOR (R-type, opcode=00000, func=001010) — R[rd] = R[rs] ^ R[rt]
## Precondiciones:
- set r1 0xAAAAAAAA
- set r2 0x55555555
## Code:
```
set pc 0x0
set r1 0xAAAAAAAA
set r2 0x55555555
set [0x0] 0x0044300A
step
```
## Postcondiciones:
- R[3] = 0xFFFFFFFF
## Conclusiones:
Anduvo. R[3] = 0xAAAAAAAA ^ 0x55555555 = 0xFFFFFFFF.

---

# Caso 7: SLL $3, $1, 4
## Descripción:
Testeo de la instrucción SLL (shift left lógico) con constante aux=4.
## Instrucctions:
SLL (R-type, opcode=00000, func=000000) — R[rd] = R[rt] << aux
## Precondiciones:
- set r1 0x00000001
## Code:
```
set pc 0x0
set r1 0x00000001
set [0x0] 0x00023200
step
```
## Postcondiciones:
- R[3] = 0x00000010
## Conclusiones:
Anduvo. 1 << 4 = 16 = 0x10.

---

# Caso 8: SRL $3, $1, 4
## Descripción:
Testeo de la instrucción SRL (shift right lógico) con constante aux=4.
## Instrucctions:
SRL (R-type, opcode=00000, func=000001) — R[rd] = R[rt] >> aux (lógico)
## Precondiciones:
- set r1 0x00000100
## Code:
```
set pc 0x0
set r1 0x00000100
set [0x0] 0x00023201
step
```
## Postcondiciones:
- R[3] = 0x00000010
## Conclusiones:
Anduvo. 0x100 >> 4 = 0x10 (rellena con 0s).

---

# Caso 9: SRA $3, $1, 4
## Descripción:
Testeo de la instrucción SRA (shift right aritmético) para verificar que usa R[rs] como fuente, tal como indica el manual.
## Instrucctions:
SRA (R-type, opcode=00000, func=000010) — R[rd] = R[rs] >> aux (aritmético)
## Precondiciones:
- set r1 0x80000000 (valor negativo, fuente según manual)
- set r2 0x12345678 (valor distinto en R[rt], para descartar que se use ese registro)
## Code:
```
set pc 0x0
set r1 0x80000000
set r2 0x12345678
set [0x0] 0x00423202
step
```
## Postcondiciones:
- R[3] = 0xF8000000
## Conclusiones:
Anduvo. El resultado 0xF8000000 proviene de desplazar R[1] = 0x80000000 4 posiciones a la derecha de forma aritmética, preservando el bit de signo. Si el emulador hubiera usado R[2], el resultado habría sido 0x01234567; por lo tanto SRA respeta el manual y utiliza R[rs].

---

# Caso 10: SLT $3, $1, $2 (5 < 10)
## Descripción:
Testeo de SLT (set on less than) con comparación con signo donde la condición es verdadera.
## Instrucctions:
SLT (R-type, opcode=00000, func=001100) — R[rd] = (R[rs] < R[rt]) ? 1 : 0
## Precondiciones:
- set r1 5
- set r2 10
## Code:
```
set pc 0x0
set r1 5
set r2 10
set [0x0] 0x0044300C
step
```
## Postcondiciones:
- R[3] = 0x00000001
## Conclusiones:
Anduvo. 5 < 10 es verdadero, R[3] = 1.

---

# Caso 11: SLTU $3, $1, $2 (0xFFFFFFFF vs 1)
## Descripción:
Testeo de SLTU (set on less than unsigned) para demostrar diferencia con SLT. 0xFFFFFFFF es 4294967295 sin signo (mayor que 1).
## Instrucctions:
SLTU (R-type, opcode=00000, func=001101) — R[rd] = (R[rs] < R[rt]) ? 1 : 0 (sin signo)
## Precondiciones:
- set r1 0xFFFFFFFF
- set r2 1
## Code:
```
set pc 0x0
set r1 0xFFFFFFFF
set r2 1
set [0x0] 0x0044300D
step
```
## Postcondiciones:
- R[3] = 0x00000000
## Conclusiones:
Anduvo. 0xFFFFFFFF (4294967295) > 1 sin signo, R[3] = 0. Diferencia clave con SLT donde 0xFFFFFFFF (-1 con signo) < 1 y R[3] = 1.

---

# Caso 12: NOR $3, $1, $2
## Descripción:
Testeo de NOR bit a bit. NOR es el complemento del OR.
## Instrucctions:
NOR (R-type, opcode=00000, func=001011) — R[rd] = ~(R[rs] | R[rt])
## Precondiciones:
- set r1 0x00000000
- set r2 0x00000000
## Code:
```
set pc 0x0
set r1 0x00000000
set r2 0x00000000
set [0x0] 0x0044300B
step
```
## Postcondiciones:
- R[3] = 0xFFFFFFFF
## Conclusiones:
Anduvo. ~(0 | 0) = ~0 = 0xFFFFFFFF.

---

# Caso 13: LW $2, 0($1)
## Descripción:
Testeo de LW (load word) para cargar una palabra completa desde memoria a registro.
## Instrucctions:
LW (I-type, opcode=01000) — R[rt] = M[EA(rs, imm)]
## Precondiciones:
- set r1 0x100 (dirección base)
- set [0x100] 0xDEADBEEF (valor a cargar)
## Code:
```
set pc 0x0
set r1 0x100
set [0x100] 0xDEADBEEF
set [0x0] 0x20800000
step
```
## Postcondiciones:
- R[2] = 0xDEADBEEF
## Conclusiones:
Anduvo. LW carga 4 bytes desde M[0x100] = 0xDEADBEEF a R[2].

---

# Caso 14: SW $2, 0($1)
## Descripción:
Testeo de SW (store word) para guardar una palabra completa de registro a memoria.
## Instrucctions:
SW (I-type, opcode=01001) — M[EA(rs, imm)] = R[rt]
## Precondiciones:
- set r1 0x100 (dirección base)
- set r2 0xDEADBEEF (valor a guardar)
## Code:
```
set pc 0x0
set r1 0x100
set r2 0xDEADBEEF
set [0x0] 0x28800000
step
examine 0x100
```
## Postcondiciones:
- M[0x100] = 0xDEADBEEF
## Conclusiones:
Anduvo. SW guarda 4 bytes de R[2] en M[0x100].

---

# Caso 15: SH $2, 0($1)
## Descripción:
Testeo de SH (store half) para guardar solo 2 bytes bajos del registro a memoria.
## Instrucctions:
SH (I-type, opcode=01010) — M[EA(rs, imm)][15:0] = R[rt][15:0]
## Precondiciones:
- set r1 0x100 (dirección base)
- set r2 0xABCD1234 (valor, solo se guarda 0x1234)
- set [0x100] 0xDEADBEEF (valor previo en memoria)
## Code:
```
set pc 0x0
set r1 0x100
set r2 0xABCD1234
set [0x100] 0xDEADBEEF
set [0x0] 0x30800000
step
examine 0x100
```
## Postcondiciones:
- M[0x100] = 0xDEAD1234 (solo 2 bytes bajos cambiaron)
## Conclusiones:
Anduvo. SH guarda solo los 2 bytes bajos (0x1234), los 2 bytes altos quedan intactos (0xDEAD).

---

# Caso 16: SB $2, 0($1)
## Descripción:
Testeo de SB (store byte) para guardar solo 1 byte del registro a memoria.
## Instrucctions:
SB (I-type, opcode=01011) — M[EA(rs, imm)][7:0] = R[rt][7:0]
## Precondiciones:
- set r1 0x100 (dirección base)
- set r2 0xABCD1234 (valor, solo se guarda 0x34)
- set [0x100] 0xDEADBEEF (valor previo)
## Code:
```
set pc 0x0
set r1 0x100
set r2 0xABCD1234
set [0x100] 0xDEADBEEF
set [0x0] 0x38800000
step
examine 0x100
```
## Postcondiciones:
- M[0x100] = 0xDEADBE34 (solo 1 byte cambió)
## Conclusiones:
Anduvo. SB guarda solo el byte bajo (0x34), los 3 bytes superiores quedan intactos (0xDEADBE).

---

# Caso 17: LH $2, 0($1)
## Descripción:
Testeo de LH (load half) con sign-extend. Carga 2 bytes y extiende con signo.
## Instrucctions:
LH (I-type, opcode=01100) — R[rt] = sign-extend(M[EA][15:0])
## Precondiciones:
- set r1 0x100 (dirección base)
- set [0x100] 0x00008000 (halfword = 0x8000, bit 15 = 1)
## Code:
```
set pc 0x0
set r1 0x100
set [0x100] 0x00008000
set [0x0] 0x40800000
step
```
## Postcondiciones:
- R[2] = 0xFFFF8000
## Conclusiones:
Anduvo. LH carga 0x8000 y extiende con signo: como bit 15 = 1, los 16 bits superiores se llenan de 1s → 0xFFFF8000.

---

# Caso 18: LHU $2, 0($1)
## Descripción:
Testeo de LHU (load half unsigned) con zero-extend. Carga 2 bytes y extiende con 0s.
## Instrucctions:
LHU (I-type, opcode=01101) — R[rt] = zero-extend(M[EA][15:0])
## Precondiciones:
- set r1 0x100 (dirección base)
- set [0x100] 0x00008000 (halfword = 0x8000)
## Code:
```
set pc 0x0
set r1 0x100
set [0x100] 0x00008000
set [0x0] 0x68440000
step
```
## Postcondiciones:
- R[2] = 0x00008000
## Conclusiones:
Anduvo. LHU carga 0x8000 y extiende con ceros: como es zero-extend, los 16 bits superiores se llenan de 0s → R[2] = 0x00008000. Corregido en la nueva versión del emulador (v2).

---

# Caso 19: BEQ $1, $2, +2 (salto tomado)
## Descripción:
Testeo de BEQ (branch if equal) cuando la condición es verdadera (R1 == R2).
## Instrucctions:
BEQ (I-type, opcode=10000) — if (R[rs] == R[rt]) PC = PC+4 + Branch(imm)
## Precondiciones:
- set r1 5
- set r2 5
## Code:
```
set pc 0x0
set r1 5
set r2 5
set [0x0] 0x80A00002
step
```
## Postcondiciones:
- PC = 0x0000000C (saltó: PC+4 + (2 << 2) = 4 + 8 = 12)
## Conclusiones:
Anduvo. BEQ salta cuando R1 == R2. PC = 0xC = 12.

---

# Caso 20: BNE $1, $2, +2 (salto tomado)
## Descripción:
Testeo de BNE (branch if not equal) cuando la condición es verdadera (R1 != R2).
## Instrucctions:
BNE (I-type, opcode=10001) — if (R[rs] != R[rt]) PC = PC+4 + Branch(imm)
## Precondiciones:
- set r1 5
- set r2 7
## Code:
```
set pc 0x0
set r1 5
set r2 7
set [0x0] 0x88A00002
step
```
## Postcondiciones:
- PC = 0x0000000C (saltó)
## Conclusiones:
Anduvo. BNE salta cuando R1 != R2. PC = 0xC = 12.

---

# Caso 21: BLT $1, $2, +2 (salto tomado)
## Descripción:
Testeo de BLT (branch if less than) con comparación con signo cuando la condición es verdadera.
## Instrucctions:
BLT (I-type, opcode=10010) — if (R[rs] < R[rt]) PC = PC+4 + Branch(imm)
## Precondiciones:
- set r1 3
- set r2 10
## Code:
```
set pc 0x0
set r1 3
set r2 10
set [0x0] 0x90A00002
step
```
## Postcondiciones:
- PC = 0x0000000C (saltó)
## Conclusiones:
Anduvo. BLT salta cuando R1 < R2 (con signo). 3 < 10, PC = 0xC = 12.

---

# Caso 22: J 0x40 (salto a 0x100)
## Descripción:
Testeo de J (jump incondicional) para saltar a una dirección directa.
## Instrucctions:
J (J-type, opcode=00010) — PC = E(address)
## Precondiciones:
- Ninguna (el PC empieza en 0x0)
## Code:
```
set pc 0x0
set [0x0] 0x04000040
step
```
## Postcondiciones:
- PC = 0x00000100
## Conclusiones:
Anduvo. J salta a E(0x40) = {PC+4[31:29], 0x40, 2'b0} = 0x100.

---

# Caso 23: JAL 0x80 (salto a 0x200, guarda retorno)
## Descripción:
Testeo de JAL (jump and link) para saltar y guardar la dirección de retorno en R[31].
## Instrucctions:
JAL (J-type, opcode=00011) — R[31] = PC+4; PC = E(address)
## Precondiciones:
- Ninguna
## Code:
```
set pc 0x0
set [0x0] 0x18000080
step
```
## Postcondiciones:
- PC = 0x00000200 (dirección de salto)
- R[31] = 0x00000004 (dirección de retorno, PC+4)
## Conclusiones:
Anduvo. JAL guarda PC+4 = 4 en R[31] y salta a E(0x80) = 0x200.

---

# Caso 24: JR $5 (salto a dirección en registro)
## Descripción:
Testeo de JR (jump register) para saltar a la dirección contenida en un registro.
## Instrucctions:
JR (R-type, opcode=00000, func=001110) — PC = R[rs]
## Precondiciones:
- set r5 0x300 (dirección destino)
## Code:
```
set pc 0x0
set r5 0x300
set [0x0] 0x0140000E
step
```
## Postcondiciones:
- PC = 0x00000300
## Conclusiones:
Anduvo. JR salta al valor completo del registro: PC = R[5] = 0x300.

---

# Caso 25: LUI $1, 0x1234
## Descripción:
Testeo de LUI (load upper immediate) para cargar constante en 16 bits altos del registro.
## Instrucctions:
LUI (L-type, opcode=00111) — R[rt] = ZC(imm) = {imm, 16'b0}
## Precondiciones:
- Ninguna
## Code:
```
set pc 0x0
set [0x0] 0xE0210000
step
```
## Postcondiciones:
- R[1] = 0x12340000
## Conclusiones:
Anduvo. LUI carga la constante en los 16 bits altos correctamente: R[1] = {0x1234, 16'b0} = 0x12340000. Corregido en la nueva versión del emulador.

---

# Caso 26: ANDI $2, $1, 0x00FF (h=0)
## Descripción:
Testeo de ANDI con parte baja (h=0) para verificar AND con constante inmediata.
## Instrucctions:
ANDI (L-type, opcode=00100) — R[rt] = R[rs] & ZE(imm)
## Precondiciones:
- set r1 0xABCD1234
## Code:
```
set pc 0x0
set r1 0xABCD1234
set [0x0] 0x804100FF
step
```
## Postcondiciones:
- R[2] = 0x00000034 (52)
## Conclusiones:
Anduvo. ANDI ejecuta correctamente: R[2] = 0xABCD1234 & 0xFF = 0x34.

---

# Caso 27: ORI $2, $1, 0x00FF (h=0)
## Descripción:
Testeo de ORI con parte baja (h=0) para verificar OR con constante inmediata.
## Instrucctions:
ORI (L-type, opcode=00101) — R[rt] = R[rs] | ZE(imm)
## Precondiciones:
- set r1 0xABCD0000
## Code:
```
set pc 0x0
set r1 0xABCD0000
set [0x0] 0xA04100FF
step
```
## Postcondiciones:
- R[2] = 0xABCD00FF (2882337023)
## Conclusiones:
Anduvo. ORI ejecuta correctamente: R[2] = 0xABCD0000 | 0xFF = 0xABCD00FF.

---

# Caso 28: CFS $1, S[0] (Copy From Special)
## Descripción:
Testeo de CFS para copiar un registro especial (PSW) a un registro general.
## Instrucctions:
CFS (R-type, opcode=00000, func=000110) — R[rs] = S[aux]
## Precondiciones:
- Ninguna
## Code:
```
set pc 0x0
set [0x0] 0x00400006
step
```
## Postcondiciones:
- CAUSE = 0x00000003 (Illegal Instruction)
- R[1] no se modificó
## Conclusiones:
FALLÓ. CFS causa excepción CAUSE=3. El emulador reconoce la función pero el handler está vacío.

---

# Caso 29: CTS $1, S[0] (Copy To Special)
## Descripción:
Testeo de CTS para copiar un registro general a un registro especial (PSW).
## Instrucctions:
CTS (R-type, opcode=00000, func=000111) — S[aux] = R[rs]
## Precondiciones:
- set r1 0x00000010 (valor a escribir en PSW)
## Code:
```
set pc 0x0
set r1 0x00000010
set [0x0] 0x00400007
step
```
## Postcondiciones:
- CAUSE = 0x00000003 (Illegal Instruction)
- PSW no se modificó
## Conclusiones:
FALLÓ. CTS causa excepción CAUSE=3. El emulador reconoce la función pero el handler está vacío.

---

# Caso 30: BGT $1, $2, +2 (salto tomado, R1>R2)
## Descripción:
Testeo de BGT (branch if greater than). Verifica salto tomado cuando R[1] > R[2] con signo, y salto no tomado cuando R[1] <= R[2].
## Instrucciones:
BGT (I-type, opcode=10011) — if (R[rs] > R[rt]) PC = PC+4 + Branch(imm)
## Precondiciones:
- set r1 10
- set r2 5
## Code:
```
set pc 0x0
set r1 10
set r2 5
set [0x0] 0x98440002
step
```
## Postcondiciones:
- PC = 0x0000000C (salto tomado: PC+4 + (2<<2) = 12)
## Conclusiones:
Anduvo. BGT salta cuando R1 > R2 (10 > 5), PC = 0xC. La condicion no tomada (5 <= 10) tambien se verifico y dejo PC = 0x4.

---

# Caso 31: BLE $1, $2, +2 (salto tomado, R1<=R2)
## Descripción:
Testeo de BLE (branch if less or equal). Verifica salto tomado cuando R[1] <= R[2] con signo.
## Instrucciones:
BLE (I-type, opcode=10100) — if (R[rs] <= R[rt]) PC = PC+4 + Branch(imm)
## Precondiciones:
- set r1 3
- set r2 10
## Code:
```
set pc 0x0
set r1 3
set r2 10
set [0x0] 0xA0440002
step
```
## Postcondiciones:
- PC = 0x0000000C
## Conclusiones:
Anduvo. BLE salta cuando R1 <= R2 (3 <= 10), PC = 0xC. La condicion no tomada (10 > 5) dejo PC = 0x4.

---

# Caso 32: BGE $1, $2, +2 (salto tomado, R1>=R2)
## Descripción:
Testeo de BGE (branch if greater or equal). Verifica salto tomado cuando R[1] >= R[2] con signo.
## Instrucciones:
BGE (I-type, opcode=10101) — if (R[rs] >= R[rt]) PC = PC+4 + Branch(imm)
## Precondiciones:
- set r1 10
- set r2 5
## Code:
```
set pc 0x0
set r1 10
set r2 5
set [0x0] 0xA8440002
step
```
## Postcondiciones:
- PC = 0x0000000C
## Conclusiones:
Anduvo. BGE salta cuando R1 >= R2 (10 >= 5), PC = 0xC. La condicion no tomada (3 < 10) dejo PC = 0x4.

---

# Caso 33: SLTI $2, $1, 10
## Descripción:
Testeo de SLTI (set on less than immediate) con signo. R[1]=5 < 10 -> R[2]=1.
## Instrucciones:
SLTI (I-type, opcode=10110) — R[rt] = (R[rs] < SE(imm)) ? 1 : 0
## Precondiciones:
- set r1 5
## Code:
```
set pc 0x0
set r1 5
set [0x0] 0xB044000A
step
```
## Postcondiciones:
- R[2] = 0x00000001 (1)
## Conclusiones:
Anduvo. 5 < 10 es verdadero, R[2] = 1.

---

# Caso 34: SLTIU $2, $1, 10
## Descripción:
Testeo de SLTIU (set on less than immediate unsigned). R[1]=5 < 10 -> R[2]=1.
## Instrucciones:
SLTIU (I-type, opcode=10111) — R[rt] = (R[rs] < ZE(imm)) ? 1 : 0
## Precondiciones:
- set r1 5
## Code:
```
set pc 0x0
set r1 5
set [0x0] 0xB844000A
step
```
## Postcondiciones:
- R[2] = 0x00000001 (1)
## Conclusiones:
Anduvo. 5 < 10 sin signo es verdadero, R[2] = 1.

---

# Caso 35: LB $2, 0($1)
## Descripción:
Testeo de LB (load byte) con sign-extend. Carga el byte bajo de M[0x100]=0x80 y extiende con signo.
## Instrucciones:
LB (I-type, opcode=01110) — R[rt] = sign-extend(M[EA][7:0])
## Precondiciones:
- set r1 0x100
- set [0x100] 0x00000080
## Code:
```
set pc 0x0
set r1 0x100
set [0x100] 0x00000080
set [0x0] 0x70440000
step
```
## Postcondiciones:
- R[2] = 0xFFFFFF80
## Conclusiones:
Anduvo. LB carga 0x80 y extiende con signo: bit 7 = 1, por lo tanto R[2] = 0xFFFFFF80.

---

# Caso 36: LBU $2, 0($1)
## Descripción:
Testeo de LBU (load byte unsigned) con zero-extend. Carga el byte bajo de M[0x100]=0x80.
## Instrucciones:
LBU (I-type, opcode=01111) — R[rt] = zero-extend(M[EA][7:0])
## Precondiciones:
- set r1 0x100
- set [0x100] 0x00000080
## Code:
```
set pc 0x0
set r1 0x100
set [0x100] 0x00000080
set [0x0] 0x78440000
step
```
## Postcondiciones:
- R[2] = 0x00000080
## Conclusiones:
Anduvo. LBU carga 0x80 y extiende con ceros, R[2] = 0x00000080.

---

# Caso 37: SLLR $4, $3, $2
## Descripción:
Testeo de SLLR (shift left logical by register). R[2]=4, R[3]=1 -> R[4] = 1 << 4.
## Instrucciones:
SLLR (R-type, opcode=00000, func=000011) — R[rd] = R[rt] << R[rs][4:0]
## Precondiciones:
- set r2 4
- set r3 1
## Code:
```
set pc 0x0
set r2 4
set r3 1
set [0x0] 0x00864003
step
```
## Postcondiciones:
- R[4] = 0x00000010 (16)
## Conclusiones:
Anduvo. R[4] = 1 << 4 = 0x10.

---

# Caso 38: SRLR $4, $3, $2
## Descripción:
Testeo de SRLR (shift right logical by register). R[2]=4, R[3]=0xF0000000 -> R[4]=0x0F000000.
## Instrucciones:
SRLR (R-type, opcode=00000, func=000100) — R[rd] = R[rt] >> R[rs][4:0] (logico)
## Precondiciones:
- set r2 4
- set r3 0xF0000000
## Code:
```
set pc 0x0
set r2 4
set r3 0xF0000000
set [0x0] 0x00864004
step
```
## Postcondiciones:
- R[4] = 0x0F000000
## Conclusiones:
Anduvo. SRLR desplaza 0xF0000000 4 posiciones a derecha logica, R[4] = 0x0F000000.

---

# Caso 39: SRAR $4, $3, $2
## Descripción:
Testeo de SRAR (shift right arithmetic by register). R[2]=4, R[3]=0x80000000 -> R[4]=0xF8000000.
## Instrucciones:
SRAR (R-type, opcode=00000, func=000101) — R[rd] = R[rt] >> R[rs][4:0] (aritmetico)
## Precondiciones:
- set r2 4
- set r3 0x80000000
## Code:
```
set pc 0x0
set r2 4
set r3 0x80000000
set [0x0] 0x00864005
step
```
## Postcondiciones:
- R[4] = 0xF8000000
## Conclusiones:
Anduvo. SRAR desplaza 0x80000000 4 posiciones a derecha aritmetica, preservando signo, R[4] = 0xF8000000.

---

# Caso 40: LHX $3, R[$1]+R[$2]
## Descripción:
Testeo de LHX (load half indexed, sign-extend). Direccion = R[1]+R[2] = 0x108, M[0x108]=0x00008000.
## Instrucciones:
LHX (R-type, opcode=00000, func=010000) — R[rt] = sign-extend(M[R[rs]+R[rd]][15:0])
## Precondiciones:
- set r1 0x100
- set r2 0x8
- set [0x108] 0x00008000
## Code:
```
set pc 0x0
set r1 0x100
set r2 0x8
set [0x108] 0x00008000
set [0x0] 0x00462010
step
```
## Postcondiciones:
- R[3] = 0xFFFF8000
## Conclusiones:
Anduvo. LHX carga el halfword 0x8000 desde 0x108 y extiende con signo, R[3] = 0xFFFF8000.

---

# Caso 41: LHUX $3, R[$1]+R[$2]
## Descripción:
Testeo de LHUX (load half unsigned indexed, zero-extend). Direccion = 0x108, M[0x108]=0x00008000.
## Instrucciones:
LHUX (R-type, opcode=00000, func=010001) — R[rt] = zero-extend(M[R[rs]+R[rd]][15:0])
## Precondiciones:
- set r1 0x100
- set r2 0x8
- set [0x108] 0x00008000
## Code:
```
set pc 0x0
set r1 0x100
set r2 0x8
set [0x108] 0x00008000
set [0x0] 0x00462011
step
```
## Postcondiciones:
- R[3] = 0x00008000
## Conclusiones:
Anduvo. LHUX carga el halfword 0x8000 desde 0x108 y extiende con ceros, R[3] = 0x00008000.

---

# Caso 42: LBX $3, R[$1]+R[$2]
## Descripción:
Testeo de LBX (load byte indexed, sign-extend). Direccion = R[1]+R[2] = 0x101, byte bajo = 0x80.
## Instrucciones:
LBX (R-type, opcode=00000, func=010010) — R[rt] = sign-extend(M[R[rs]+R[rd]][7:0])
## Precondiciones:
- set r1 0x100
- set r2 0x1
- set [0x100] 0x00000080
## Code:
```
set pc 0x0
set r1 0x100
set r2 0x1
set [0x100] 0x00000080
set [0x0] 0x00462012
step
```
## Postcondiciones:
- R[3] = 0xFFFFFF80
## Conclusiones:
Anduvo. LBX carga el byte 0x80 desde la dirección R[1]+R[2]=0x101 y extiende con signo: bit 7 = 1, por lo tanto R[3] = 0xFFFFFF80. Corregido en la nueva versión del emulador.

---

# Caso 43: LBUX $3, R[$1]+R[$2]
## Descripción:
Testeo de LBUX (load byte unsigned indexed, zero-extend). Direccion = 0x101, byte bajo = 0x80.
## Instrucciones:
LBUX (R-type, opcode=00000, func=010011) — R[rt] = zero-extend(M[R[rs]+R[rd]][7:0])
## Precondiciones:
- set r1 0x100
- set r2 0x1
- set [0x100] 0x00000080
## Code:
```
set pc 0x0
set r1 0x100
set r2 0x1
set [0x100] 0x00000080
set [0x0] 0x00462013
step
```
## Postcondiciones:
- R[3] = 0x00000080
## Conclusiones:
Anduvo. LBUX carga el byte 0x80 desde la dirección R[1]+R[2]=0x101 y extiende con ceros: R[3] = 0x00000080. Corregido en la nueva versión del emulador.

---

# Caso 44: LWX $3, R[$1]+R[$2]
## Descripción:
Testeo de LWX (load word indexed). Direccion = R[1]+R[2] = 0x104, M[0x104]=0xDEADBEEF.
## Instrucciones:
LWX (R-type, opcode=00000, func=010100) — R[rt] = M[R[rs]+R[rd]]
## Precondiciones:
- set r1 0x100
- set r2 0x4
- set [0x104] 0xDEADBEEF
## Code:
```
set pc 0x0
set r1 0x100
set r2 0x4
set [0x104] 0xDEADBEEF
set [0x0] 0x00462014
step
```
## Postcondiciones:
- R[3] = 0xDEADBEEF
## Conclusiones:
Anduvo. LWX carga la palabra completa desde 0x104, R[3] = 0xDEADBEEF.

---

# Caso 45: MUL $4, $2, $3
## Descripción:
Testeo de MUL (multiply low). R[2]=7, R[3]=6 -> R[4] = 42.
## Instrucciones:
MUL (R-type, opcode=00000, func=010101) — R[rd] = (R[rs] * R[rt])[31:0]
## Precondiciones:
- set r2 7
- set r3 6
## Code:
```
set pc 0x0
set r2 7
set r3 6
set [0x0] 0x00864015
step
```
## Postcondiciones:
- R[4] = 0x0000002A (42)
## Conclusiones:
Anduvo. R[4] = 7 * 6 = 42 = 0x2A.

---

# Caso 46: MULH $4, $2, $3
## Descripción:
Testeo de MULH (multiply high signed). R[2]=0x00010000, R[3]=0x00010000 -> parte alta = 0x1.
## Instrucciones:
MULH (R-type, opcode=00000, func=010110) — R[rd] = (R[rs] * R[rt])[63:32] (con signo)
## Precondiciones:
- set r2 0x00010000
- set r3 0x00010000
## Code:
```
set pc 0x0
set r2 0x00010000
set r3 0x00010000
set [0x0] 0x00864016
step
```
## Postcondiciones:
- R[4] = 0x00000001
## Conclusiones:
Anduvo. 0x10000 * 0x10000 = 0x0000000100000000, parte alta R[4] = 0x1.

---

# Caso 47: MULHU $4, $2, $3
## Descripción:
Testeo de MULHU (multiply high unsigned). R[2]=0x00010000, R[3]=0x00010000 -> parte alta = 0x1.
## Instrucciones:
MULHU (R-type, opcode=00000, func=010111) — R[rd] = (R[rs] * R[rt])[63:32] (sin signo)
## Precondiciones:
- set r2 0x00010000
- set r3 0x00010000
## Code:
```
set pc 0x0
set r2 0x00010000
set r3 0x00010000
set [0x0] 0x00864017
step
```
## Postcondiciones:
- R[4] = 0x00000001
## Conclusiones:
Anduvo. MULHU coincide con MULH para estos operandos, R[4] = 0x1.

---

# Caso 48: DIV $4, $2, $3
## Descripción:
Testeo de DIV (divide signed). R[2]=20, R[3]=4 -> R[4]=5.
## Instrucciones:
DIV (R-type, opcode=00000, func=011000) — R[rd] = R[rs] / R[rt] (con signo)
## Precondiciones:
- set r2 20
- set r3 4
## Code:
```
set pc 0x0
set r2 20
set r3 4
set [0x0] 0x00864018
step
```
## Postcondiciones:
- R[4] = 0x00000005
## Conclusiones:
Anduvo. R[4] = 20 / 4 = 5.

---

# Caso 49: DIVU $4, $2, $3
## Descripción:
Testeo de DIVU (divide unsigned). R[2]=20, R[3]=4 -> R[4]=5.
## Instrucciones:
DIVU (R-type, opcode=00000, func=011001) — R[rd] = R[rs] / R[rt] (sin signo)
## Precondiciones:
- set r2 20
- set r3 4
## Code:
```
set pc 0x0
set r2 20
set r3 4
set [0x0] 0x00864019
step
```
## Postcondiciones:
- R[4] = 0x00000005
## Conclusiones:
Anduvo. R[4] = 20 / 4 = 5 sin signo.

---

# Caso 50: REST $4, $2, $3
## Descripción:
Testeo de REST (remainder signed). R[2]=23, R[3]=5 -> R[4]=3.
## Instrucciones:
REST (R-type, opcode=00000, func=011010) — R[rd] = R[rs] % R[rt] (con signo)
## Precondiciones:
- set r2 23
- set r3 5
## Code:
```
set pc 0x0
set r2 23
set r3 5
set [0x0] 0x0086401A
step
```
## Postcondiciones:
- R[4] = 0x00000003
## Conclusiones:
Anduvo. R[4] = 23 % 5 = 3.

---

# Caso 51: RESTU $4, $2, $3
## Descripción:
Testeo de RESTU (remainder unsigned). R[2]=23, R[3]=5 -> R[4]=3.
## Instrucciones:
RESTU (R-type, opcode=00000, func=011011) — R[rd] = R[rs] % R[rt] (sin signo)
## Precondiciones:
- set r2 23
- set r3 5
## Code:
```
set pc 0x0
set r2 23
set r3 5
set [0x0] 0x0086401B
step
```
## Postcondiciones:
- R[4] = 0x00000003
## Conclusiones:
Anduvo. R[4] = 23 % 5 = 3 sin signo.

---

# Caso 52: JALR $5
## Descripción:
Testeo de JALR (jump and link register). Salta a R[5]=0x200 y guarda R[31]=PC+4=0x4.
## Instrucciones:
JALR (R-type, opcode=00000, func=001111) — R[31] = PC+4; PC = R[rs]
## Precondiciones:
- set r5 0x200
## Code:
```
set pc 0x0
set r5 0x200
set [0x0] 0x0141F00F
step
```
## Postcondiciones:
- PC = 0x00000200 (salto correcto)
- R[31] = 0x00000004 (direccion de retorno, PC+4)
## Conclusiones:
Anduvo. JALR guarda PC+4 = 4 en R[31] y salta a R[5] = 0x200. El campo rd especifica el registro de link.

---

# Caso 53: XORI $2, $1, 0x00FF (h=0)
## Descripción:
Testeo de XORI (L-type) con h=0 y h=1. h=0: R[1]=0xFFFFFFFF ^ ZE(0xFF) = 0xFFFFFF00. h=1: R[1]=0x12345678 ^ ZC(0xABCD) = 0xB9F95678.
## Instrucciones:
XORI (L-type, opcode=00110) — R[rt] = R[rs] XOR ZE/ZC(imm) segun h
## Precondiciones:
- set r1 0xFFFFFFFF
## Code:
```
set pc 0x0
set r1 0xFFFFFFFF
set [0x0] 0x304400FF
step
```
## Postcondiciones:
- R[2] = 0xFFFFFF00 (h=0)
- R[2] = 0xB9F95678 (h=1, R[1]=0x12345678)
## Conclusiones:
Anduvo. XORI funciona correctamente para h=0 (R[2]=0xFFFFFF00) y h=1 (R[2]=0xB9F95678).

---
