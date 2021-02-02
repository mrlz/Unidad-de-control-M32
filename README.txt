Video explicativo: https://www.youtube.com/watch?v=uUaO0f04fEs

El circuito lógico corresponde a la unidad de control de la
arquitectura M32 estudiada en el curso. Fue diseñado en Logisim y
se utilizó el material que describe la arquitectura M32 y sus
operaciones como guía. Dichos archivos se incluyen para dar
claridad de lo realizado.
La idea detrás del texto que se incluye a continuación es más bien
ser un material de apoyo para quien se encuentre inspeccionando
la estructura y las partes del circuito en el programa Logisim, por lo
que no se incluyen imágenes ni una descripción excesivamente
técnica, el énfasis está en dar claridad del esquema del diseño del
circuito y la finalidad de sus partes.

Diseño:
-------
Para el diseño del circuito de control se utilizó el esquema indicado
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

Para cada estado se confeccionó un circuito que calculase las
señales de control a enviar dada la instrucción actual, dichos
circuitos corresponden a Fetch1, Fetch2, Decode, exec1 y exec2.
Fetch1, Fetch2 y Decode contienen señales que permanecen
invariantes frente al código de instrucción, por lo que los valores
que entregan son constantes, pese a lo anterior se tiene que Fetch2
es sensible a la instrucción WAIT, al igual que exec2.
exec1 y exec2 sí utilizan los datos de la instrucción recibida, la cual
corresponde a 5 bits a través de los cuales se identifican las 30
operaciones distintas que existen en la arquitectura M32. exec1
también utiliza la señal BR?, con el fin de determinar si se debe
realizar un salto condicional o no.
Los códigos de las operaciones son los siguientes:

0000| 0
0001| 0
0010| 0
0011| 0
0100| 0 Bajo el criterio termina con 0 y menor o igual a 18 se agrupan las
0101| 0 operaciones de la ALU, la idea es simplemente enviar a la ALU
0110| 0 los 4 bits más significativos sin tener que realizar cálculos
0111| 0 adicionales. Éste grupo será codificado con 01.
1000| 0
1001| 0
________________________________________________________________________
000| 11
001| 11
010| 11
011| 11 Bajo el criterio termina con 11 se agrupan las operaciones de DBI
100| 11 Al igual que en el caso anterior la idea es poder traspasarle los
101| 11 3 bits más significativos de forma directa. Éste grupo será codificado
110| 11 con 10.
111| 11
________________________________________________________________________

00001 = JMPL
00101
01001
01101 De los números restantes se eligió de forma arbitraria los que terminan con
10001 01 y 4 de los sobrantes terminados en 0 para describir las operaciones
      Condicionales. Éste grupo será codificado con 00.
10101
11001
11101
--------
10100
10110
11000
11010
________________________________________________________________________

Lo anterior permite, con un mux, activar uno de 3 módulos en los
cuales se agrupa cada familia de instrucciones. Para determinar la
familia a la que pertenece cada operación se utiliza el circuito
discriminate, cuyo output corresponde, como se mencionó arriba, a
la codificación
Familia Aritmético/Lógica = 01
Familia Lectura/Escritura = 11
Familia Condicional = 00
Gracias a la división anterior se tiene que, en la estructura de exec1
y exec2, se encuentran encapsulados los comportamientos que
debe tener cada familia, dado el estado actual, en 3 circuitos: ALU
Instruction, DBI Instruction y Conditional Instruction,
respectivamente.
Para exec1 se tiene ALU Instruction, DBI Instruction y Conditional
Instruction.
Para exec2 se tiene DBI Instruction 2 y Conditional Instruction 2.

Lo anterior dado que no existen, en la implementación actual,
operaciones aritmético/lógicas que requieran pasar al estado
exec2, de forma que al terminar sus operaciones en el estado exec1
pasan directamente de vuelta a Fetch1.
