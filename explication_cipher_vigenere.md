    # Cifrado de Vigenère en Python

    # 1. Primera línea: creación de la función

    ```python
    def vigenere(texto, clave, cifrar=True):
    ```

    Esta línea crea una función llamada `vigenere`.

    La función recibe tres parámetros:

    ```text
    ┌─────────────────────────────────────┐
    │ vigenere(texto, clave, cifrar)      │
    ├──────────┬──────────┬───────────────┤
    │  texto   │  clave   │    cifrar     │
    ├──────────┼──────────┼───────────────┤
    │ mensaje  │ palabra  │ True / False  │
    │          │ secreta  │               │
    └──────────┴──────────┴───────────────┘
    ```

    ### `texto`

    Es el mensaje que queremos cifrar.

    ### `clave`

    Es la palabra que determina el desplazamiento de cada letra para su cifrado o descifrado.


    ### `cifrar`

    Indica qué operación realizar.

    ```text
    True  → Cifrar
    False → Descifrar
    ```
    ---

    # 2. Convertir la clave a mayúsculas

    ```python
    clave = clave.upper()
    ```

    `upper()` convierte todos los caracteres a mayúsculas.

    Esto permite trabajar de manera uniforme con las letras del alfabeto.

    ---

    # 3. Crear las variables de resultado y posición

    ```python
    resultado, j = "", 0
    ```
    Aquí se crean dos variables.

    ## `resultado`

    Inicialmente está vacío:

    ```text
    resultado = ""
    ```

    ## `j`

    Indica la posición actual dentro de la clave.

    Comienza en:

    ```text
    j = 0
    ```

    Visualmente:

    ```text
    CLAVE
    01234
    ↑
    j=0
    ```

    Después:

    ```text
    CLAVE
    01234
    ↑
    j=1
    ```

    Y así sucesivamente.

    ---

    # 4. Recorrer el texto

    ```python
    for c in texto.upper():
    ```

    Esta línea recorre el texto carácter por carácter.

    Si tenemos:

    ```text
    HOLA
    ```

    el ciclo realiza:

    ```text
    1 → H
    2 → O
    3 → L
    4 → A
    ```

    La variable `c` representa el carácter que se está procesando actualmente.

    Visualmente:

    ```text
    H O L A
    ↑
    c
    ```

    Después:

    ```text
    H O L A
    ↑
    c
    ```

    Después:

    ```text
    H O L A
        ↑
        c
    ```

    Y finalmente:

    ```text
    H O L A
        ↑
        c
    ```

    Además:

    ```python
    texto.upper()
    ```

    convierte el mensaje a mayúsculas.

    ---

    # 5. Comprobar si el carácter es una letra

    ```python
    if 'A' <= c <= 'Z':
    ```

    Esta condición pregunta:

    ```text
    ¿El carácter está entre A y Z?
    ```

    Por ejemplo:

    ```text
    H → SÍ
    O → SÍ
    L → SÍ
    A → SÍ
    ```

    Pero:

    ```text
    " " → NO
    "," → NO
    "!" → NO
    "1" → NO
    ```

    Esto permite que el programa cifre solamente las letras.

    ---

    # 6. Obtener el valor de la letra de la clave

    ```python
    k = ord(clave[j % len(clave)]) - 65
    ```

    Esta es una de las líneas más importantes. Su objetivo es obtener el valor numérico de la letra de la clave.

    Primero tenemos:

    ```text
    A = 0
    B = 1
    C = 2
    D = 3
    ...
    Z = 25
    ```

    ---

    ## 6.1 `len(clave)`

    Obtiene la cantidad de caracteres de la clave.

    ```python
    len("CLAVE")
    ```

    produce:

    ```text
    5
    ```

    Por lo tanto:

    ```text
    CLAVE
    01234
    ```

    ---

    ## 6.2 `j % len(clave)`

    El operador `%` obtiene el resto de una división.
    Sirve para repetir la clave.

    Por ejemplo:

    ```text
    j       j % 5
    ─────────────
    0   →     0
    1   →     1
    2   →     2
    3   →     3
    4   →     4
    5   →     0
    6   →     1
    7   →     2
    ```

    Esto produce:

    ```text
    CLAVECLAVECLAVE...
    012340123401234...
    ```

    Por eso la clave se repite automáticamente.

    ---

    # 6.3. `ord()` y la conversión A = 0

    La función:

    ```python
    ord()
    ```

    obtiene el código numérico de un carácter.

    Por ejemplo:

    ```python
    ord('A') = 65
    ord('B') = 66
    ord('C') = 67
    ```

    Como queremos:

    ```text
    A = 0
    B = 1
    C = 2
    ```

    restamos 65:

    ```text
    A → 65 - 65 = 0
    B → 66 - 65 = 1
    C → 67 - 65 = 2
    ```

    Por eso aparece:

    ```python
    ord(...) - 65
    ```

    El resultado es:

    ```text
    ┌───────┬───────┐
    │ Letra │ Valor │
    ├───────┼───────┤
    │ A     │   0   │
    │ B     │   1   │
    │ C     │   2   │
    │ D     │   3   │
    │ ...   │  ...  │
    │ Z     │  25   │
    └───────┴───────┘
    ```

    ---

    # 7. Cifrar o descifrar

    La parte:

    ```python
    (k if cifrar else -k)
    ```

    decide si debemos sumar o restar la clave.

    Es una expresión condicional.

    Equivale aproximadamente a:

    ```python
    if cifrar:
        usar +k
    else:
        usar -k
    ```

    Por lo tanto:

    ```text
                    ┌───────────────┐
                    │   cifrar ?    │
                    └───────┬───────┘
                            │
                    ┌─────────┴─────────┐
                    │                   │
                True                False
                    │                   │
                    ▼                   ▼
                +K                  -K
                    │                   │
                    ▼                   ▼
            C = (P+K)%26         P = (C-K)%26
    ```

    ---

    # 8. Convertir nuevamente el número a letra

    La parte:

    ```python
    chr(... + 65)
    ```

    convierte el número nuevamente en un carácter.

    Si obtenemos:

    ```text
    0
    ```

    sumamos 65:

    ```text
    0 + 65 = 65
    ```

    Y:

    ```python
    chr(65)
    ```

    produce:

    ```text
    A
    ```

    Por ejemplo:

    ```text
    0 + 65 → A
    1 + 65 → B
    2 + 65 → C
    ...
    25 + 65 → Z
    ```

    ---

    # 8.1. La línea principal del cifrado

    Toda esta línea:

    ```python
    resultado += chr((ord(c) - 65 + desplazamiento) % 26 + 65)
    ```

    Realiza todo el proceso matemático del algoritmo usando la formula de cifrado o descifrado.

    ---

    # 8.2. Agregar la letra al resultado

    ```python
    resultado += chr(...)
    ```

    El operador `+=` agrega la nueva letra al contenido anterior de `resultado`.

    Por ejemplo:

    ```text
    Inicio:

    resultado = ""
    ```

    Después de procesar la primera letra:

    ```text
    resultado = "J"
    ```

    Después:

    ```text
    resultado = "JY"
    ```

    Después:

    ```text
    resultado = "JYK"
    ```

    Y continúa hasta construir todo el mensaje.

    ---

    # 9. Avanzar a la siguiente posición de la clave

    ```python
    j += 1
    ```

    Aumenta `j` en uno.

    Por ejemplo:

    ```text
    Antes:
    j = 0

    Después:
    j = 1
    ```

    Esto permite avanzar por la clave:

    ```text
    CLAVE
    ↑
    j=0
    ```

    Luego:

    ```text
    CLAVE
    ↑
    j=1
    ```

    Luego:

    ```text
    CLAVE
    ↑
    j=2
    ```

    Cuando llega al final, el operador `%` hace que vuelva a comenzar.

    ---

    # 10. ¿Qué ocurre con los espacios?

    La condición:

    ```python
    if 'A' <= c <= 'Z':
    ```

    solamente acepta letras.

    Por lo tanto, si encuentra un espacio:

    ```text
    HOLA MUNDO
        ↑
    espacio
    ```

    entra al `else`:

    ```python
    else:
        resultado += c
    ```

    El espacio simplemente se copia.

    Por eso:

    ```text
    HOLA MUNDO
    ```

    mantiene su espacio después del cifrado.

    Lo mismo ocurre con:

    ```text
    ,
    .
    !
    ?
    123
    ```

    Estos caracteres no se cifran.

    ---