.. _man-mathematical-operations:

*************************
 Operadores Matemáticos
*************************
Julia fornece uma coleção completa de aritmética básica e operadores binários em todos os seus tipos primitivos numéricos, bem como fornecendo portabilidade, eficientes implementações e uma ampla coleção de funções matemáticas padrão.

Aritimétia e Operadores Lógicos Binários
--------------------------------

Os seguintes operadores matemáticos <https://pt.wikipedia.org/wiki/Aritm%C3%A9tica>`_
são suportados para todos os tipos numéricos primitivos abaixo:

-  ``+x`` — "mais" unária é a operação de identidade.
-  ``-x`` — Mapas unários de valores negativos aos seus inversos aditivos.
-  ``x + y`` — binário "mais" realiza adição.
-  ``x - y`` — binário "menos" realiza subtração.
-  ``x * y`` — "vezes" executa multiplicação.
-  ``x / y`` — "dividir" executa divisão.

Os seguintes operadores lógicos binários <https://pt.wikipedia.org/wiki/L%C3%B3gica_bin%C3%A1ria>`_
são suportados para todos os tipos numéricos primitivos inteiros (https://pt.wikipedia.org/wiki/N%C3%BAmero_inteiro) abaixo:

-  ``~x``    — Binário lógico "não".
-  ``x & y`` — Binário "e".
-  ``x | y`` — binário "ou.
-  ``x $ y`` — binário "ou exclusivo" ou "xor".
-  ``x >>> y`` — Deslocamento lógico <http://en.wikipedia.org/wiki/Logical_shift>`_ right.
-  ``x >> y`` — `Deslocamento aritimético <https://pt.wikipedia.org/wiki/Deslocamento_aritm%C3%A9tico>`_ para a direita.
-  ``x << y`` — logico/aritmético deslocamento para a esquerda.

Aqui estão alguns exemplos simples usando operadores aritméticos:

    julia> 1 + 2 + 3
    6

    julia> 1 - 2
    -1

    julia> 3*2/12
    0.5

(Por convenção, temos a tendência de menos espaço para os operadores encadeados bem menos vinculados, mas não há restrições sintáticas.)

Julia possui um sistema de desenvolvimento que faz operações aritméticas com misturas de tipos de argumentos "apenas funcione" naturalmente e automaticamente. Ver :ref: `man-conversion-and-promotion` para obter mais detalhes do sistema de desenvolvimento

Aqui estão alguns exemplos simples usando operadores lógicos:
obs: os numeros estão em decimais mas Julia trabalha com esses números na forma binaria, ou seja, 123 = 1111011 em binário

    julia> ~123 
    -124

    julia> 123 & 234
    106

    julia> 123 | 234
    251

    julia> 123 $ 234
    145

    julia> ~uint32(123)
    0xffffff84

    julia> ~uint8(123)
    0x84


Toda aritmética binária e operadores lógicos também tem uma versão de atualização que atribui o resultado da operação de volta para seu operando esquerdo. Por exemplo, a forma de atualização de " + " é o operador "+ =" . Escrever  "x + = 3" é equivalente à escrita "x = x + 3" ::

      julia> x = 1
      1

      julia> x += 3
      4

      julia> x
      4

As versões de atualização de todos os operadores aritméticos e de lógica binária são estas::

    +=  -=  *=  /=  &=  |=  $=  >>>=  >>=  <<=


.. _man-numeric-comparisons:

Comparações Numéricas
-------------------

As Operações de comparação padrão são definidas para todos os tipos primitivos numéricos:

-  ``==`` — igualdade.
-  ``!=`` — desigualdade.
-  ``<`` — menor que.
-  ``<=`` — menor ou igual que.
-  ``>``  — maior que.
-  ``>=`` — maior ou igual que.

Aqui estão alguns exemplos simples:: 
obs: true é verdadeiro e false é falso

    julia> 1 == 1
    true

    julia> 1 == 2
    false

    julia> 1 != 2
    true

    julia> 1 == 1.0
    true

    julia> 1 < 2
    true

    julia> 1.0 > 3
    false

    julia> 1 >= 1.0
    true

    julia> -1 <= 1
    true

    julia> -1 <= -1
    true

    julia> -1 <= -2
    false

    julia> 3 < -0.5
    false

Números Inteiros (-25,-1,2,0,35..1250,..) são comparados no modo convencional - por comparação de bits. Números de ponto flutuante são comparados de acordo com o `padrão IEEE 754 <https://pt.wikipedia.org/wiki/IEEE_754> `_:
 
- Números finitos são ordenados da maneira usual

-  ``Inf`` (Infinito positivo) é igual a si mesmo e maior do que tudo o resto, exceto
   ``NaN``(não é número)
   
-  ``-Inf``(Infinito negativo) É igual a si próprio e menos então tudo o resto exceto
   ``NaN`` (não é número)
   
-  ``NaN`` não é igual, menor ou maior do que tudo, incluindo o próprio.

O último ponto é potencialmente surpreendente e, portanto, merece nota::

    julia> NaN == NaN
    false

    julia> NaN != NaN
    true

    julia> NaN < NaN
    false

    julia> NaN > NaN
    false

Para as situações em que se pretende comparar os valores de ponto flutuante para que "NaN" é igual a "NaN", como, por exemplo, as comparações de chave hash, a função "equivale" também é fornecido, o qual considera "NaN" s para ser igual a todos os outros::

    julia> isequal(NaN,NaN)
    true

Comparações do tipo mista entre inteiros definidos, inteiros sem sinal, e flutuantes(decimais) pode ser muito complicado. Foram tomadas grande cuidado para assegurar que Julia faça-os corretamente.


Diferentemente da maioria das outras linguagens, com a notável exceção do Python <http://en.wikipedia.org/wiki/Python_syntax_and_semantics#Comparison_operators> `_, comparações podem ser arbitrariamente encadeadas ::

    julia> 1 < 2 <= 2 < 3 == 3 > 2 >= 1 == 1 < 3 != 5
    true

O encadeamento de comparações muitas vezes é bastante conveniente em código numérico. As comparações numéricas em cadeia com o operador " &" , permite nos trabalhar com arrays. Por exemplo, "0 < A < 1" apresenta uma matriz booleana cujas entradas são verdadeiras onde os elementos correspondentes da "A" são entre 0 e 1.

Observe o comportamento de avaliação de comparações encadeadas ::

    v(x) = (println(x); x)

    julia> v(1) < v(2) <= v(3)
    2
    1
    3
    false

O meio termo é avaliada somente uma vez, em vez de duas vezes como seria se a expressão fosse escrita como "v(1) > v(2) & v(2) <= v(3) ". No entanto, o fim das avaliações em uma comparação de encadeamento é indefinido. É altamente recomendável não utilizar expressões com efeitos posteriores (como imprimir) encadeados em comparações. Se os efeitos posteriores são necessárias, o operador " &&"  deve ser utilizado explicitamente (veja :ref:`man-short-circuit-evaluation`).

Funções Matemáticas
----------------------

Julia oferece uma coleção abrangente de funções e operadores matemáticos.Estas operações matemáticas são definidos ao longo de uma ampla classe de valores numéricos como permitir definições bem estruturadas, incluindo inteiros, números de ponto flutuante, racionais, e complexos, onde quer que essas definições fazem sentido.

-  ``round(x)``  — Arredonda "x" para o número inteiro mais próximo.

-  ``iround(x)`` — Arredonda " x" para o número inteiro mais próximo, dando um resultado digitado inteiro.

-  ``floor(x)``  — Arredonda " x" em direção a "-Inf".

-  ``ifloor(x)``   — Arredonda " x" em direção "-Inf", dando um resultado digitado inteiro.

-  ``ceil(x)``     — Arredonda " x" em direção a "+ Inf".

-  ``iceil(x)``    — Arredonda " x" em direção " + Inf", dando um resultado digitado inteiro.

-  ``trunc(x)``    — Arredonda "x" para zero.

-  ``itrunc(x)``   — Arredonda " x" para zero, dando um resultado digitado inteiro.

-  ``div(x,y)``    — Divisão truncada; quociente arredondado para próximo de zero.

-  ``fld(x,y)``    — Divisão por baixo; quociente arredondada na direção de " -Inf".

-  ``rem(x,y)``    — Restante; satisfaz "x == div(x,y) * y + rem(x,y) ".

-  ``mod(x,y)``    — Módulo; satisfaz "x == f(x,y) * y + mod(x,y) ".

-  ``gcd(x,y...)`` — Maior divisor comum (MDC) de " x", " y" 

-  ``lcm(x,y...)`` — Mínimo múltiplo comum (MMC) de " x", " y"

-  ``abs(x)``      — um valor positivo com a magnitude do valor de "x".

-  ``abs2(x)``     — a magnitude quadrada do valor de "x".

-  ``sign(x)``     — Indica o sinal de "x", retornando -1, 0, ou 1.

-  ``signbit(x)``  — indica se o sinal binário é ligado (1) ou desligado (0).

-  ``copysign(x,y)`` — um valor com a magnitude de " x" e o sinal de " y".

-  ``flipsign(x,y)`` — a value with the magnitude of ``x`` and the sign   of ``x*y``.

-  ``sqrt(x)`` — the square root of ``x``.

-  ``cbrt(x)`` — the cube root of ``x``.

-  ``hypot(x,y)`` — accurate ``sqrt(x^2 + y^2)`` for all values of ``x``   and ``y``.

-  ``exp(x)`` — the natural exponential function at ``x``.

-  ``expm1(x)`` — accurate ``exp(x)-1`` for ``x`` near zero.

-  ``ldexp(x,n)`` — ``x*2^n`` computed efficiently for integer values of   ``n``.

-  ``log(x)`` — the natural logarithm of ``x``.

-  ``log(b,x)`` — the base ``b`` logarithm of ``x``.

-  ``log2(x)`` — the base 2 logarithm of ``x``.

-  ``log10(x)`` — the base 10 logarithm of ``x``.

-  ``log1p(x)`` — accurate ``log(1+x)`` for ``x`` near zero.

-  ``logb(x)`` — returns the binary exponent of ``x``.

-  ``erf(x)`` — the `error
   function <http://en.wikipedia.org/wiki/Error_function>`_ at ``x``.
   
-  ``erfc(x)`` — accurate ``1-erf(x)`` for large ``x``.

-  ``gamma(x)`` — the `gamma
   function <http://en.wikipedia.org/wiki/Gamma_function>`_ at ``x``.
   
-  ``lgamma(x)`` — accurate ``log(gamma(x))`` for large ``x``.

For an overview of why functions like ``hypot``, ``expm1``, ``log1p``,
and ``erfc`` are necessary and useful, see John D. Cook's excellent pair
of blog posts on the subject: `expm1, log1p,
erfc <http://www.johndcook.com/blog/2010/06/07/math-library-functions-that-seem-unnecessary/>`_,
and
`hypot <http://www.johndcook.com/blog/2010/06/02/whats-so-hard-about-finding-a-hypotenuse/>`_.

All the standard trigonometric functions are also defined::

    sin    cos    tan    cot    sec    csc
    sinh   cosh   tanh   coth   sech   csch
    asin   acos   atan   acot   asec   acsc
    acoth  asech  acsch  sinc   cosc   atan2

These are all single-argument functions, with the exception of
`atan2 <http://en.wikipedia.org/wiki/Atan2>`_, which gives the angle
in `radians <http://en.wikipedia.org/wiki/Radian>`_ between the *x*-axis
and the point specified by its arguments, interpreted as *x* and *y*
coordinates. In order to compute trigonometric functions with degrees
instead of radians, suffix the function with ``d``. For example, ``sind(x)``
computes the sine of ``x`` where ``x`` is specified in degrees.

For notational convenience, the ``rem`` functions has an operator form:

-  ``x % y`` is equivalent to ``rem(x,y)``.

The spelled-out ``rem`` operator is the "canonical" form, while the ``%`` operator
form is retained for compatibility with other systems. Like arithmetic and bitwise
operators, ``%`` and ``^`` also have updating forms. As with other updating forms,
``x %= y`` means ``x = x % y`` and ``x ^= y`` means ``x = x^y``::

    julia> x = 2; x ^= 5; x
    32

    julia> x = 7; x %= 4; x
    3

