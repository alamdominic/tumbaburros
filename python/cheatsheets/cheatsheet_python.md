# 🐍 Python Cheatsheet — Referencia Visual

> Referencia rápida de conceptos esenciales de Python 3.x+ · 14 módulos · 70+ conceptos

---

## 📦 Variables

Contenedores con nombre para guardar valores. No requieren declaración de tipo.

```python
nombre = "Ana"          # str
edad   = 25             # int
altura = 1.68           # float
activo = True           # bool
a, b, c = 1, 2, 3       # asignación múltiple
x = int("5")            # type cast
```

> **★ Convención:** Usa `snake_case` para nombres de variables: `mi_variable`

---

## 🔢 Tipos de Datos

| Tipo       | Ejemplo      | Descripción |
| ---------- | ------------ | ----------- |
| `int`      | `42`         | Entero      |
| `float`    | `3.14`       | Decimal     |
| `str`      | `"hola"`     | Texto       |
| `bool`     | `True/False` | Lógico      |
| `NoneType` | `None`       | Vacío       |
| `complex`  | `2+3j`       | Complejo    |

```python
type(42)             # <class 'int'>
isinstance("x", str) # True
int("10")  float("3.14")  str(99)
```

---

## ⚡ Operadores

| Operador     | Uso                 | Resultado |
| ------------ | ------------------- | --------- |
| `+ - * /`    | Aritmética          | número    |
| `//`         | División entera     | int       |
| `%`          | Módulo/resto        | int       |
| `**`         | Potencia            | número    |
| `== != < >`  | Comparación         | bool      |
| `and or not` | Lógico              | bool      |
| `is  in`     | Identidad/membresía | bool      |

```python
10 // 3    # 3
10 % 3     # 1
2 ** 8     # 256
x is None  # identidad
3 in [1,2,3]  # True
```

---

## 🔀 Condicionales

Bifurcan el flujo según condiciones. Python usa indentación (4 espacios).

```python
if edad >= 18:
    print("adulto")
elif edad >= 13:
    print("adolescente")
else:
    print("niño")

# Ternario (una línea)
msg = "par" if n % 2 == 0 else "impar"
```

> **Falsy:** `None`, `0`, `""`, `[]`, `{}`, `set()`

---

## 🔄 Loops / Bucles

### FOR — iterar secuencias

```python
for i in range(5):        # 0-4
    print(i)

for i, v in enumerate(lista):
    print(i, v)

for a, b in zip(l1, l2):
    print(a, b)
```

### WHILE — condición dinámica

```python
n = 10
while n > 0:
    print(n)
    n -= 1
else:           # si no hubo break
    print("fin")

break      # sale del loop
continue   # salta iteración
pass       # placeholder vacío
```

---

## ⚙️ Funciones

```python
# Estructura completa con type hints
def saludar(nombre: str, mayus: bool = False) -> str:
    """Docstring descriptivo."""
    msg = f"Hola, {nombre}!"
    return msg.upper() if mayus else msg

# *args → tupla de posicionales
def suma(*nums):
    return sum(nums)

# **kwargs → dict de keywords
def info(**datos):
    for k, v in datos.items():
        print(f"{k}: {v}")

# Lambda (función anónima)
doble = lambda x: x * 2
sorted(datos, key=lambda x: x[1])

# map / filter
list(map(lambda x: x**2, nums))
list(filter(lambda x: x > 0, nums))

# Retorno múltiple (tuple)
def minmax(lst):
    return min(lst), max(lst)

mn, mx = minmax([3, 1, 4])
```

> **★** Usa type hints (`: str → str`) para documentar mejor tus funciones

---

## 📋 Listas

Ordenada · mutable · permite duplicados

```python
# Crear
a = [1, 2, 3, "x", True]
b = list(range(5))        # [0,1,2,3,4]

# Indexing y Slicing
a[0]    # primer elemento
a[-1]   # último elemento
a[1:3]  # [2, 3]
a[::2]  # cada 2 elementos
a[::-1] # invertida

# Métodos clave
a.append(4)      # añade al final
a.insert(0, 99)  # inserta en posición
a.extend([5, 6]) # añade lista
a.remove(3)      # elimina valor
a.pop()          # quita último
a.sort()         # ordena in-place
a.reverse()      # invierte
len(a)  |  3 in a  |  sorted(a)
```

---

## 🗂️ Diccionarios

clave:valor · ordenado (3.7+) · mutable

```python
# Crear y acceder
d = {"nombre": "Ana", "edad": 25}
d["nombre"]            # "Ana"
d.get("x", "default")  # seguro sin KeyError
d["ciudad"] = "CDMX"   # añadir/modificar
del d["edad"]           # eliminar
"nombre" in d           # True

# Recorrer
for k in d.keys():    print(k)
for v in d.values():  print(v)
for k, v in d.items():
    print(f"{k}={v}")

# Fusionar (Python 3.9+)
merged = d1 | d2
d.update({"key": "val"})
d.pop("key", None)  # seguro
```

---

## 📌 Tuplas

Ordenada · **inmutable** · más rápida que lista

```python
t = (1, 2, 3)
t[0]           # 1
a, b, c = t    # unpack
uno, *resto = t
len(t)  |  t.count(1)  |  t.index(2)
solo = (42,)   # coma necesaria para 1 elemento
```

> **⚠️** No se puede modificar. Usa `list(t)` para convertir.

---

## 🔵 Sets

Sin orden · sin duplicados · búsquedas O(1)

```python
s = {1, 2, 3}
s.add(4)       |  s.remove(2)
s.discard(9)   # no lanza error si no existe

# Operaciones de conjuntos
a | b   # unión
a & b   # intersección
a - b   # diferencia
a ^ b   # diferencia simétrica
frozenset(s)  # inmutable
```

---

## 📦 Módulos e Imports

```python
# Formas de importar
import math
from math import sqrt, pi
import numpy as np
from os.path import join, exists

# Módulos stdlib útiles
import os        # sistema operativo
import sys       # intérprete
import json      # JSON
import datetime  # fechas
import random    # aleatoriedad

# Inspección
dir(math)         # lista atributos
help(math.sqrt)   # documentación

# __name__ guard
if __name__ == "__main__":
    main()
```

**Módulos populares:** `math` · `os` · `sys` · `json` · `datetime` · `random` · `re` · `pathlib` · `collections` · `itertools`

---

## 🛡️ Manejo de Errores

```python
# Estructura completa
try:
    resultado = 10 / divisor
except ZeroDivisionError:
    print("División por cero")
except (TypeError, ValueError) as e:
    print(f"Error: {e}")
else:
    print("Sin errores")   # si no hubo excepción
finally:
    print("Siempre ejecuta")  # pase lo que pase

# raise — lanzar excepciones
raise ValueError("Valor inválido")
raise  # relanzar excepción actual

# assert — verificar supuestos
assert x > 0, "x debe ser positivo"

# with — context manager (cierra automáticamente)
with open("datos.txt", "r") as f:
    contenido = f.read()

with open("out.txt", "w") as f:
    f.write("Hola")
```

> **Excepciones comunes:** `ValueError` · `TypeError` · `KeyError` · `IndexError` · `FileNotFoundError` · `AttributeError`
>
> **★ Siempre** captura excepciones específicas, nunca `except:` sin tipo

---

## ⚡ Comprensiones

```python
# List comprehension
cuadrados = [x**2 for x in range(10)]
pares     = [x for x in nums if x % 2 == 0]
flat      = [n for row in matrix for n in row]

# Dict comprehension
cuad_d = {x: x**2 for x in range(5)}
inv    = {v: k for k, v in d.items()}

# Set comprehension
uniq = {x % 5 for x in nums}

# Generator (lazy — sin [])  ★ ahorra memoria
gen = (x**2 for x in range(1_000_000))
next(gen)    # produce de uno en uno
sum(gen)     # consume sin guardar en memoria

# Función generadora
def fibonacci():
    a, b = 0, 1
    while True:
        yield a
        a, b = b, a + b
```

---

## ✨ Buenas Prácticas (PEP 8)

### Nomenclatura

```python
# ✓ snake_case para variables y funciones
mi_variable = 10
def mi_funcion(): ...

# ✓ PascalCase para clases
class MiClase: ...

# ✓ UPPER_CASE para constantes
MAX_SIZE = 100
```

### Estilo Pythonic

```python
# ✗ No Pythonic
if x == True: ...
result = ""
for i in range(len(lst)):
    result += str(lst[i])

# ✓ Pythonic
if x: ...
result = ", ".join(str(v) for v in lst)

# ✓ Swap elegante
a, b = b, a

# ✓ Valor por defecto en dict
d.setdefault("k", []).append(1)
```

### Reglas clave

| ✓ Hacer                  | ✗ Evitar                 |
| ------------------------ | ------------------------ |
| `x is None`              | `x == None`              |
| `with open(...)`         | Abrir sin cerrar         |
| F-strings: `f"{x}"`      | Concatenar con `+`       |
| `except ValueError:`     | `except:` a solas        |
| Type hints en funciones  | Funciones sin documentar |
| 2 líneas entre funciones | Código apiñado           |

---

## 🔍 Funciones Útiles Globales

```python
# Itertools útiles
any(iterable)        # True si alguno es True
all(iterable)        # True si todos son True
sorted(a)            # lista ordenada (nueva)
reversed(a)          # iterador invertido
enumerate(a)         # (índice, valor)
zip(a, b)            # pares de dos iterables
map(fn, iterable)    # aplica función
filter(fn, iterable) # filtra elementos

# Introspección
type(x)      # clase del objeto
dir(obj)     # atributos y métodos
help(obj)    # documentación
id(x)        # identidad del objeto
globals()    # variables globales
locals()     # variables locales
```

---

_🐍 Python 3.x · PEP 8 compliant · [python.org/doc](https://docs.python.org)_
