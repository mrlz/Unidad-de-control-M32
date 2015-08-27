El circuito l�gico corresponde a la unidad de control de la
arquitectura M32 estudiada en el curso. Fue dise�ado en Logisim y
se utiliz� el material que describe la arquitectura M32 y sus
operaciones como gu�a. Dichos archivos se incluyen para dar
claridad de lo realizado.
La idea detr�s del texto que se incluye a continuaci�n es m�s bien
ser un material de apoyo para quien se encuentre inspeccionando
la estructura y las partes del circuito en el programa Logisim, por lo
que no se incluyen im�genes ni una descripci�n excesivamente
t�cnica, el �nfasis est� en dar claridad del esquema del dise�o del
circuito y la finalidad de sus partes.

Dise�o:
-------
Para el dise�o del circuito de control se utiliz� el esquema indicado
en el apunte del curso, en el cual se describen 5 estados (a saber:
Fetch1, Fetch2, Decode, Exec1 y Exec2).
Los estados se codifican:
Fetch1 : 000
Fetch2 : 001
Decode : 010
Execute1:011
Execute2:100
Se guarda el estado en la memoria del circuito secuencial.
(Ver imagen Circuito Secuencial)

Para cada estado se confeccion� un circuito que calculase las
se�ales de control a enviar dada la instrucci�n actual, dichos
circuitos corresponden a Fetch1, Fetch2, Decode, exec1 y exec2.
Fetch1, Fetch2 y Decode contienen se�ales que permanecen
invariantes frente al c�digo de instrucci�n, por lo que los valores
que entregan son constantes, pese a lo anterior se tiene que Fetch2
es sensible a la instrucci�n WAIT, al igual que exec2.
exec1 y exec2 s� utilizan los datos de la instrucci�n recibida, la cual
corresponde a 5 bits a trav�s de los cuales se identifican las 30
operaciones distintas que existen en la arquitectura M32. exec1
tambi�n utiliza la se�al BR?, con el fin de determinar si se debe
realizar un salto condicional o no.
Los c�digos de las operaciones son los siguientes:

0000| 0
0001| 0
0010| 0
0011| 0
0100| 0 Bajo el criterio termina con 0 y menor o igual a 18 se agrupan las
0101| 0 operaciones de la ALU, la idea es simplemente enviar a la ALU
0110| 0 los 4 bits m�s significativos sin tener que realizar c�lculos
0111| 0 adicionales. �ste grupo ser� codificado con 01.
1000| 0
1001| 0
________________________________________________________________________
000| 11
001| 11
010| 11
011| 11 Bajo el criterio termina con 11 se agrupan las operaciones de DBI
100| 11 Al igual que en el caso anterior la idea es poder traspasarle los
101| 11 3 bits m�s significativos de forma directa. �ste grupo ser� codificado
110| 11 con 10.
111| 11
________________________________________________________________________

00001 = JMPL
00101
01001
01101 De los n�meros restantes se eligi� de forma arbitraria los que terminan con
10001 01 y 4 de los sobrantes terminados en 0 para describir las operaciones
      Condicionales. �ste grupo ser� codificado con 00.
10101
11001
11101
--------
10100
10110
11000
11010
________________________________________________________________________

Lo anterior permite, con un mux, activar uno de 3 m�dulos en los
cuales se agrupa cada familia de instrucciones. Para determinar la
familia a la que pertenece cada operaci�n se utiliza el circuito
discriminate, cuyo output corresponde, como se mencion� arriba, a
la codificaci�n
Familia Aritm�tico/L�gica = 01
Familia Lectura/Escritura = 11
Familia Condicional = 00
Gracias a la divisi�n anterior se tiene que, en la estructura de exec1
y exec2, se encuentran encapsulados los comportamientos que
debe tener cada familia, dado el estado actual, en 3 circuitos: ALU
Instruction, DBI Instruction y Conditional Instruction,
respectivamente.
Para exec1 se tiene ALU Instruction, DBI Instruction y Conditional
Instruction.
Para exec2 se tiene DBI Instruction 2 y Conditional Instruction 2.

Lo anterior dado que no existen, en la implementaci�n actual,
operaciones aritm�tico/l�gicas que requieran pasar al estado
exec2, de forma que al terminar sus operaciones en el estado exec1
pasan directamente de vuelta a Fetch1.
