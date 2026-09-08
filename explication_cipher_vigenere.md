```markdown
# Cifrado de Vigenère en Python

## 1. Definición de la función

```python
def vigenere(texto, clave, cifrar=True):
```

Se crea una función llamada `vigenere` que recibe tres parámetros:

| Parámetro | Descripción |
|-----------|-------------|
| `texto`   | Mensaje a cifrar o descifrar |
| `clave`   | Palabra que determina el desplazamiento |
| `cifrar`  | `True` → Cifrar / `False` → Descifrar |

---

## 2. Normalizar la clave

```python
clave = clave.upper()
```

Convierte la clave a mayúsculas para trabajar de forma uniforme con el alfabeto.

---

## 3. Inicializar variables

```python
resultado, j = "", 0
```

- `resultado`: cadena vacía donde se acumulará el mensaje final.
- `j`: índice que indica la posición actual dentro de la clave (comienza en 0).

```
CLAVE
01234
↑
j=0
```

---

## 4. Recorrer el texto

```python
for c in texto.upper():
```

Se itera sobre cada carácter del texto (convertido a mayúsculas).

Ejemplo con `"HOLA"`:

```
H O L A
↑
c
```

Luego avanza carácter por carácter.

---

## 5. Verificar si es una letra

```python
if 'A' <= c <= 'Z':
```

Solo se procesan las letras de la `A` a la `Z`.  
Los espacios, signos de puntuación, números y otros caracteres se copian sin modificar.

---

## 6. Obtener el valor numérico de la letra de la clave

```python
k = ord(clave[j % len(clave)]) - 65
```

### `len(clave)`
Obtiene la longitud de la clave.  
Ejemplo: `len("CLAVE")` → `5`

### `j % len(clave)`
El operador módulo (`%`) permite que la clave se repita cíclicamente:

| j | j % 5 |
|---|-------|
| 0 | 0 |
| 1 | 1 |
| 2 | 2 |
| 3 | 3 |
| 4 | 4 |
| 5 | 0 |
| 6 | 1 |

Esto produce la repetición: `CLAVECLAVECLAVE...`

### `ord()` y ajuste a 0
`ord('A') = 65`, `ord('B') = 66`, etc.  
Restamos 65 para que `A = 0`, `B = 1`, ..., `Z = 25`.

| Letra | Valor |
|-------|-------|
| A     | 0     |
| B     | 1     |
| C     | 2     |
| ...   | ...   |
| Z     | 25    |

---

## 7. Determinar desplazamiento (cifrar o descifrar)

```python
(k if cifrar else -k)
```

- Si `cifrar = True` → se suma `k` (desplazamiento positivo).
- Si `cifrar = False` → se resta `k` (desplazamiento negativo).

```
            cifrar ?
           /        \
        True        False
         |            |
        +k           -k
         |            |
    C = (P+k)%26  P = (C-k)%26
```

---

## 8. Convertir el número a letra y agregar al resultado

```python
resultado += chr((ord(c) - 65 + desplazamiento) % 26 + 65)
```

- `ord(c) - 65` → valor numérico de la letra actual (0-25).
- Se suma o resta el desplazamiento según la operación.
- `% 26` asegura que el resultado esté en el rango 0-25.
- Se suma 65 para obtener el código ASCII.
- `chr()` convierte el número en el carácter correspondiente.

### Ejemplo de construcción de `resultado`:

```
Inicio:  resultado = ""
1ª letra: resultado = "J"
2ª letra: resultado = "JY"
3ª letra: resultado = "JYK"
...
```

---

## 9. Avanzar en la clave

```python
j += 1
```

Incrementa `j` para usar la siguiente letra de la clave en el próximo carácter.

```
CLAVE
↑
j=0

CLAVE
 ↑
 j=1

CLAVE
  ↑
  j=2
```

Cuando `j` supera la longitud de la clave, el operador `%` la reinicia automáticamente.

---

## 10. Manejo de caracteres no alfabéticos

```python
else:
    resultado += c
```

Si el carácter no es una letra (espacios, comas, números, etc.), se copia tal cual sin cifrar.

Ejemplo con `"HOLA MUNDO"`:

```
HOLA MUNDO
    ↑
   espacio → se conserva
```

El espacio y otros signos permanecen en el mensaje final.
```