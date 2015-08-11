.. _man-complex-and-rational-numbers:

******************************
 Complex and Rational Numbers  
******************************

Julia é estruturado com tipos predefinidos que representa ambos os números complexos e os números racionais, e suporta todas as operações matemáticas discutido no :ref:`man-mathematical-operations` sobre eles. Os desevolvimentos são definidos de modo que as operações em qualquer combinação de tipos numéricos predefinidos, primitivas ou composto, se comportam como esperado.


.. _man-complex-numbers:

Números Complexos
---------------

A constante global  ``im`` está vinculada ao complexo conjunto *i* , que representa uma das raízes quadradas de -1. Ela foi considerada prejudicial por a co-optar o nome "i" como constante global , uma vez que, nome de variável é um índice popular. Uma vez que Julia permite literais numéricos para ser :ref:`juxtaposed with identifiers as coefficients <man-numeric-literal-coefficients>`, esta ligação é suficiente para permitir uma fácil sintaxe de Notação matemática de números complexos, semelhante ao tradicional. :

    julia> 1 + 2im
    1 + 2im


Você pode executar todas as operações padrão aritméticas com números complexo na forma::

    julia> (1 + 2im)*(2 - 3im)
    8 + 1im

    julia> (1 + 2im)/(1 - 2im)
    -0.6 + 0.8im

    julia> (1 + 2im) + (1 - 2im)
    2 + 0im

    julia> (-3 + 2im) - (5 - 1im)
    -8 + 3im

    julia> (-1 + 2im)^2
    -3 - 4im

    julia> (-1 + 2im)^2.5
    2.729624464784009 - 6.9606644595719im

    julia> (-1 + 2im)^(1 + 1im)
    -0.27910381075826657 + 0.08708053414102428im

    julia> 3(2 - 5im)
    6 - 15im

    julia> 3(2 - 5im)^2
    -63 - 60im

    julia> 3(2 - 5im)^-1.0
    0.20689655172413793 + 0.5172413793103449im

A mecanismo de execução  garante que a combinação de operandos de tipos diferentes apenas funcione::

    julia> 2(1 - 1im)
    2 - 2im

    julia> (2 + 3im) - 1
    1 + 3im

    julia> (1 + 2im) + 0.5
    1.5 + 2.0im

    julia> (2 + 3im) - 0.5im
    2.0 + 2.5im

    julia> 0.75(1 + 2im)
    0.75 + 1.5im

    julia> (2 + 3im) / 2
    1.0 + 1.5im

    julia> (1 - 3im) / (2 + 2im)
    -0.5 - 1.0im

    julia> 2im^2
    -2 + 0im

    julia> 1 + 3/4im
    1.0 - 0.75im

Observe que ``3/4im == 3/(4*im) == -(3/4*im)``, Uma vez que o coeficiente literal une mais intimamente a divisão.

Funções padrão forncedias para manipular valores complexos::

    julia> real(1 + 2im)
    1

    julia> imag(1 + 2im)
    2

    julia> conj(1 + 2im)
    1 - 2im

    julia> abs(1 + 2im)
    2.23606797749979

    julia> abs2(1 + 2im)
    5

Como é comum, o valor absoluto de um número complexo é sua distância do zero. A função "abs2" retorna ao quadrado o valor absoluto, e é de uso particular de números complexos, onde se evita ter uma raiz quadrada. Toda uma gama de outras funções matemáticas são também definidas por números complexos:

    julia> sqrt(im)
    0.7071067811865476 + 0.7071067811865475im

    julia> sqrt(1 + 2im)
    1.272019649514069 + 0.7861513777574233im

    julia> cos(1 + 2im)
    2.0327230070196656 - 3.0518977991517997im

    julia> exp(1 + 2im)
    -1.1312043837568138 + 2.471726672004819im

    julia> sinh(1 + 2im)
    -0.48905625904129374 + 1.4031192506220407im

Note que funções matemáticas tipicas retornam valores reais quando aplicado aos números reais e valores complexos  quando aplicado aos números complexos.
Por exemplo, `` sqrt``, por exemplo, se comporta de modo diferente quando aplicado a `` -1``
contra `` -1 + 0im`` mesmo que `` -1 == -1 + 0im`` ::

    julia> sqrt(-1)
    ERROR: DomainError()
     in sqrt at math.jl:111

    julia> sqrt(-1 + 0im)
    0.0 + 1.0im

Se você precisa para construir um número complexo usando variáveis,a notação numérica do coeficiente literal não vai funcionar, embora escrito explicitamente, a operação de multiplicação será ::

    julia> a = 1; b = 2; a + b*im
    1 + 2im

Construir números complexos de valores de variáveis como este acima, no entanto, não é recomendado. Use a função ``complex``  para construir um complexo diretamente a partir do seu valor real e imaginário compondo as peças no lugar. Essa construção é a melhor escolha para o argumentos de uma variável  porque é mais eficiente do que a construir de multiplicação e adição , mas também porque alguns dos valores da ``b`` pode produzir resultados inesperados:

    julia> complex(a,b)
    1 + 2im

``Inf`` e ``NaN`` são utilizados, nas partes real e o imaginário de um número complexo conforme aritmética do IEEE-754 :

    julia> 1 + Inf*im
    complex(1.0,Inf)

    julia> 1 + NaN*im
    complex(1.0,NaN)


.. _man-rational-numbers:

Rational Numbers
----------------

Julia tem um tipo de número racional para representar proporções exatas de números inteiros. Racionais são construídas utilizando a // `` operador `` ::

    julia> 2//3
    2//3

Se o numerador eo denominador de uma racional têm fatores comuns, eles são reduzidos a termos mais baixos de tal forma que o denominador é não negativo ::

    julia> 6//9
    2//3

    julia> -4//8
    -1//2

    julia> 5//-15
    -1//3

    julia> -4//-12
    1//3

Esta forma normalizada para uma relação de números inteiros é único, por isso a igualdade racional de valores podem ser testados, verificando a igualdade do numerador e denominador. O numerador e denominador racional padronizado  de um valor pode ser extraído usando o "num lock" e "den" funções: :

    julia> num(2//3)
    2

    julia> den(2//3)
    3

Direct comparison of the numerator and denominator is generally not
necessary, since the standard arithmetic and comparison operations are
defined for rational values::

    julia> 2//3 == 6//9
    true

    julia> 2//3 == 9//27
    false

    julia> 3//7 < 1//2
    true

    julia> 3//4 > 2//3
    true

    julia> 2//4 + 1//6
    2//3

    julia> 5//12 - 1//4
    1//6

    julia> 5//8 * 3//12
    5//32

    julia> 6//5 / 10//7
    21//25

Rationals can be easily converted to floating-point numbers::

    julia> float(3//4)
    0.75

Conversion from rational to floating-point respects the following
identity for any integral values of ``a`` and ``b``, with the exception
of the case ``a == 0`` and ``b == 0``::

    julia> isequal(float(a//b), a/b)
    true

Constructing infinite rational values is acceptable::

    julia> 5//0
    Inf

    julia> -3//0
    -Inf

    julia> typeof(ans)
    Rational{Int64}

Trying to construct a NaN rational value, however, is not::

    julia> 0//0
    invalid rational: 0//0

As usual, the promotion system makes interactions with other numeric
types effortless::

    julia> 3//5 + 1
    8//5

    julia> 3//5 - 0.5
    0.1

    julia> 2//7 * (1 + 2im)
    2//7 + 4//7im

    julia> 2//7 * (1.5 + 2im)
    0.42857142857142855 + 0.5714285714285714im

    julia> 3//2 / (1 + 2im)
    3//10 - 3//5im

    julia> 1//2 + 2im
    1//2 + 2//1im

    julia> 1 + 2//3im
    1//1 + 2//3im

    julia> 0.5 == 1//2
    true

    julia> 0.33 == 1//3
    false

    julia> 0.33 < 1//3
    true

    julia> 1//3 - 0.33
    0.0033333333333332993

