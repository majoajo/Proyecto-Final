# Proyecto-Final

Darth pyths - Sopa de letras

Integrantes: María José Barrios, Juan David Cordobá, Julian Marciales

## Índice de Contenidos
1. [Objetivo](#objetivo)
2. [Presentación del Problema](#presentación-del-problema)
3. [Codigo desarrollado](#code)
4. [Anexos](#anexos)

## Objetivo
Crear un juego de sopa de letras (en python) que permita a los usuarios interactuar con una matriz de letras generada aleatoriamente. 
- En esta entrega se expondra el desarrollo completo del codigo, su estructura y si se logro el objetivo.
- Desde el avance tenemos que agregar funciones para:
  - Elegir categoria de palabras
  - Elegir dificultad basado en parametros mas especificos
  - Ordenar palabras
  - Imprimir la matriz con las coordenadas de cada fila y columna
  - Subrayar las palabras encontradas
  - Asignar coordenadas a cada letra de la matriz
  - Colocar palbras en la matriz verificando que quepan y que esten en orden aleatorio
  - Validar la palabra que inserto la persona con las coordenadas

## Presentación del Problema

El problema consiste en crear una sopa de letras que cumpla con los requisitos impuestos por la tarea y por nosotros; encontrar la manera mas sencilla de resolver el problema con los recursos que no han enseñado en clase.

Lo primero que hay que hacer es crear un diagrama de flujo con las funciones principales relacionadas para asi poder estructurar el codigo

Ademas de esto se tuvo que investigar ciertos operadores o funciones que no conociamos al momento del desarrollo

Diagrama de flujo en anexos

### Funciones usadas:
1. ```mostrar_instrucciones()```
  - Muestra las reglas del juego y las opciones disponibles al usuario.
  - Herramientas:
    - print(): Para imprimir texto en consola
    - Strings con \n: Para saltos de línea y formato visual.
  - No usa lógica condicional ni variables

2.  ```crear_matriz(size)```
   - Crea una matriz cuadrada de tamaño size inicializada con '0's.
   - Herramientas:
     - Listas: matriz = [] y fila = ['0'] * size.
     - Bucles: for i in range(size) para crear filas.
   - Crea una lista de listas (matriz).
   - Cada fila se rellena con '0's usando multiplicación de listas.

3. ```obtener_dificultad()```
   - Obtiene la dificultad del usuario y traducirla al tamaño de la matriz
   - Herramientas:
     - input(), strip(), lower(): Para procesar entrada del usuario.
     - Diccionario: tamanos = {"baja": 10, ...}
     - Bucle while True: Para validación continua
   - Compara la entrada del usuario con las claves del diccionario
   - Retorna el tamaño correspondiente o repite hasta entrada válida

4. ```categoria_choose()```
   - Selecciona la categoría de palabras según la elección del usuario
   - Herramientas:
     - Diccionario categorias: Listas de palabras por número
     - try-except: Para manejar entradas no numéricas
     - Bucle while True: Para reintentar en caso de error
   - Convierte la entrada a entero (int(input(...))
   - Verifica si el número está en las claves del diccionario

5.  ```ordenar_palabras(palabras)```
   - Ordenar palabras de mayor a menor longitud
   - Herramientas:
     - Bubble Sort: Implementado con bucles for anidados.
     - len(): Para comparar longitudes
     - Intercambio de elementos: palabras[j], palabras[j + 1] = ...
   - Recorre la lista múltiples veces hasta ordenarla
   - Prioriza palabras largas para facilitar su colocación en la sopa

6. ```rellenar_matriz(matriz)```
   - Rellenar espacios vacíos ('0') con letras aleatorias.
   - Heramientas:
     - random.choice(string.ascii_uppercase): Letras mayúsculas aleatorias
     - Bucles anidados: for fila in range(...) y for columna in range(...)
   - Itera sobre cada celda de la matriz.
   - Reemplaza '0's con letras usando la librería string

7.  ```imprimir_matriz(matriz)```
   - Mostrar la matriz con coordenadas (letras para columnas, números para filas)
   - Herramientas:
     - chr(ord('A') + i): Genera letras (A, B, C...) para columnas
     - enumerate(): Para numerar filas (empezando en 1)
     - Formato de strings: f"{i+1:2} " para alinear números.
   - Imprime encabezados de columnas con join()
   - Muestra cada fila con su número y elementos separados por espacios

8.  ```imprimir_sopa_coloreada(sopa, palabras_colocadas, palabras_encontradas)```
   - Mostrar la sopa con palabras encontradas en color verde
   - Herramientas:
     - ANSI Escape Codes: \033[92m (verde) y \033[0m (reset)
     - Conjunto palabras_encontradas: Almacena coordenadas de letras correctas
   - Verifica si cada coordenada está en el conjunto de encontradas
   - Aplica color verde a las letras validadas

9.  ```convertir_coordenada(coordenada, size)```
   - Convertir coordenadas a indices numericos (fila, columna)
   - Herramientas:
     - ord() y upper(): Para convertir letras a índices (ej: 'A' → 0)
     - try-except: Maneja errores de formato (ValueError, IndexError)
   - Separa letra y número de la entrada (ej: coordenada[0] y coordenada[1:])
   - Valida que los índices estén dentro del rango de la matriz

10. ```espacio_disponible(matriz, palabra, fila, columna, dir_fila, dir_columna)```
    - Verificar si una palabra cabe en una dirección dada sin colisiones
    - Herramientas:
      - Aritmética de direcciones: nueva_fila = fila + dir_fila * i
      - Operadores lógicos: and/or para validar celdas
    - Calcula la posición de cada letra de la palabra
    - Retorna False si hay colisión o está fuera de la matriz

11. ```colocar_palabra(matriz, palabra, fila, columna, dir_fila, dir_columna)```
    - Insertar una palabra en la matriz en una dirección específica
    - Herramientas:
      - Asignación directa: matriz[nueva_fila][nueva_columna] = palabra[i]
      - Aritmética de direcciones: Similar a espacio_disponible
    - Itera sobre cada letra de la palabra y la coloca en la matriz

12. ```colocar_palabras(matriz, palabras)```
    - Colocar todas las palabras en la matriz de forma aleatoria
    - Herramientas:
      - random.shuffle(direcciones): Mezcla direcciones posibles
      - Bucle while con límite de intentos (500).
    - Para cada palabra, prueba direcciones y posiciones aleatorias
    - Usa espacio_disponible y colocar_palabra como helpers

13. ```validar_palabra_encontrada(matriz, palabras_colocadas, inicio, fin, palabras_encontradas)```
    - Validar si las coordenadas ingresadas forman una palabra correcta
    - Herramientas:
      - Cálculo de dirección: dir_fila = fila_fin - fila_inicio
      - Strings: Reconstruye la palabra desde las coordenadas
      - Conjuntos: palabras_encontradas guarda coordenadas validadas
    - Determina la dirección entre inicio y fin
    - Reconstruye la palabra y verifica si está en la lista
    - Actualiza la interfaz gráfica si es correcta
   
14. ```main()```
    - Orquestar el flujo completo del juego
    - Herramientas:
      - Funciones anteriores: Encadenamiento lógico
      - while palabras_colocadas: Bucle principal del juego
    - Configuración inicial (dificultad, categoría, matriz)
    - Colocación y relleno de palabras
    - Bucle de interacción con el usuario hasta encontrar todas las palabras.

## Code

```python
import random #este modulo genera numeros o escoge aleatoriamente
import string
# modulo para manipular los strings en este caso solo se usa para que la sopa se rellene con letras mayusculas

def mostrar_instrucciones():
    print("\nBienvenid@ a la sopa de letras de Darth Pyths")
    print("\nMENU")
    print("1. La sopa de letras tiene 3 dificultades:")
    print("  - Baja: 10x10")
    print("  - Media: 15x15")
    print("  - Alta: 20x20")
    print("2. Escoge una categoría de palabras:")
    print("   1. Animales")
    print("   2. Personajes de Star Wars")
    print("   3. Random")
    print("\nTen en cuenta que:") #salto de linea para organizar
    print("Las palabras pueden estar en dirección vertical, horizontal o diagonal.")
    print("El objetivo es encontrar todas las palabras insertando coordenadas iniciales y finales.\n")

def crear_matriz(size):
    """Crea una matriz cuadrada de tamaño 'size' inicializada con espacios."""
    matriz = [] #se inicia lista para almacenar filas
    for i in range(size): #itera las veces que pid el tamaño para ahcer las filas
        fila = ['0'] * size
        matriz.append(fila) #agrega la fila a la matriz
    return matriz

def obtener_dificultad():
    """Obtiene la dificultad y devuelve el tamaño de la matriz."""
    tamanos = {"baja": 10, "media": 15, "alta": 20}
    while True:
        dificultad = input("Elige dificultad (baja/media/alta): ").strip().lower()
        #estos metodos toman los caracteres sin espacios al inicio o al final y covierte todo a minuscula
        if dificultad in tamanos: #verifica si lo ingresado es valido
            return tamanos[dificultad] #int(tamaño elegido)
        print("Dificultad no válida. Intenta de nuevo.")

def categoria_choose():
   #devuelve el diccionario elegido por el usuario
    categorias = {
        2: ["ANAKIN", "CHEWBACCA", "LEIA", "OBIWANKENOBI", "LUKE", "KYLOREN", "JABBA", "PADME", "MANDALORIAN", "MACE", "QUIGON"],
        1: ["CONDOR", "CHIGUIRO", "PUMA", "RANA", "COLIBRI", "OSO", "CAIMAN", "ARMADILLO", "DELFIN", "JAGUAR", "TORTUGA", "HIPOPOTAMO", "MONOTITI", "SIRIRI"],
        3: ["TUPLA", "MATRIZ", "MAGENTA", "LULO", "LUDOPATIA", "POKER", "PLANCHA", "SANCOCHO", "MALENIA", "CUADRO", "INTEGRALES", "NUTELLA", "AURA", "FENTANILO", "MARTES", "PARCIAL", "ESCAMA", "PEDRO", "DEEPSEEK", "ADAWONG", "CORDYCEPS", "HORMIGA", "CLAUSTRO", "SORORIDAD", "FIRULAIS", "PILATES", "PESO", "PLUMA", "AYUDA", "ORTOGONAL", "NAVIDAD", "SHREK", "FIONA", "CIEGO", "SHAKIRA"]
    }

    while True: #bucle infinito hasta q se selccione categoria validad
        try:
            categoria = int(input("Elige una categoría (1, 2 o 3): ")) #se solicita al usuario
            if categoria in categorias: #comprueba si el numero corresponde a alguna categoria
                return categorias[categoria] #retorna la lista de palabras q encontro
            else:
                print("Categoría inválida. Intenta de nuevo.")
                #si la categoria no existe o la entrada no e svalida muestra error
        except ValueError:
            print("Entrada no válida. Ingresa un número válido.")

def ordenar_palabras(palabras):
  #para que luego se empiecen a ubicar primero en la sopa las mas largas y puedas caber bien
    #Ordena palabras de mayor a menor longitud
    n = len(palabras) #numero total de palabras en una lista
    for i in range(n): #recorre cada posicion de la lista
        for j in range(0, n - i - 1): # compara cada par de palabras
            if len(palabras[j]) < len(palabras[j + 1]):
              #si la palabra actual es mas corta que las iguiente a esta las intercambia
                palabras[j], palabras[j + 1] = palabras[j + 1], palabras[j]
    return palabras #retorna la lista ordenada

def rellenar_matriz(matriz):
    #rellena los espacio en 0s
    for fila in range(len(matriz)): #itera sobre cada fila
        for columna in range(len(matriz[fila])): #recorre cada celda
            if matriz[fila][columna] == '0':
              #si la celda es 0 la rellena aleatoriamente con letra mayuscula
                matriz[fila][columna] = random.choice(string.ascii_uppercase)

def imprimir_matriz(matriz): #esto es para que el usuario pueda ubicar de manera mas facil las coordenadas
    # Obtener el tamaño de la matriz
    size = len(matriz)

    # Imprimir la fila superior con las letras de las columnas
    print("   " + " ".join([chr(ord('A') + i) for i in range(size)]))

    # Imprimir cada fila de la matriz con su número correspondiente
    for i, fila in enumerate(matriz):
        print(f"{i+1:2} " + " ".join(fila))


def imprimir_sopa_coloreada(sopa, palabras_colocadas, palabras_encontradas):
    """
    Imprime la sopa de letras resaltando las palabras encontradas en color verde.
    """
    RESET = "\033[0m"  # Reset de color
    VERDE = "\033[92m"  # Color verde para las palabras encontradas

    filas, columnas = len(sopa), len(sopa[0])

    # Imprimir la fila superior con las letras de las columnas
    print("   " + " ".join([chr(ord('A') + i) for i in range(columnas)]))

    # Imprimir cada fila de la matriz con su número correspondiente
    for i in range(filas):
        print(f"{i+1:2} ", end="")
        for j in range(columnas):
            if (i, j) in palabras_encontradas:
                print(f"{VERDE}{sopa[i][j]}{RESET}", end=" ")
            else:
                print(f"{sopa[i][j]}", end=" ")
        print()  # Salto de línea al final de cada fila


def convertir_coordenada(coordenada, size):
    try:
        columna = ord(coordenada[0].upper()) - ord('A') # se usa upper para que sin importar que coloque el usuario funcione, y define a cada letra con su numero ascii
        fila = int(coordenada[1:]) - 1
        if 0 <= fila < size and 0 <= columna < size:  #aqui se delimita las coordenadas por si el jugador coloca alguna afuera de los limites
            return fila, columna
        else:
            print("Coordenadas fuera de rango.")
            return None
    except (ValueError, IndexError):
        print("Formato de coordenada no válido. Usa el formato 'A1', 'B2', etc.")
        return None


def espacio_disponible(matriz, palabra, fila, columna, dir_fila, dir_columna):
    #funcion para verificar si la palbra cabe en la direccion especificada
    tamano = len(matriz) #para saber el tamaño d el amatriz
    for i in range(len(palabra)): #recorre cada letra de la palabra
        nueva_fila = fila + dir_fila * i #calcula la fila para la letra actual segun la direccion escogida
        nueva_columna = columna + dir_columna * i #calcula la columna
        if not (0 <= nueva_fila < tamano and 0 <= nueva_columna < tamano) or (matriz[nueva_fila][nueva_columna] != '0' and matriz[nueva_fila][nueva_columna] != palabra[i]):
          #calcula que la nueva posicion este dentro d elos limites de la matriz
          #y que este disponible para usarse o sea la misma letra q se va a poner
            return False
    return True #si todas las letras s epueden colocar termina el bucle

#la siguiente funcion es para colocar las palabras sabiendo que si tienen espacio


def colocar_palabra(matriz, palabra, fila, columna, dir_fila, dir_columna):
    for i in range(len(palabra)): #recorre cada letra d ela palabra
        nueva_fila = fila + dir_fila * i
        nueva_columna = columna + dir_columna * i
        matriz[nueva_fila][nueva_columna] = palabra[i] #coloca la letra en la posicion calculada



def colocar_palabras(matriz, palabras):
    #coloca las palabras sin repetirlas
    size = len(matriz)
    #define las 8 posibles direcciones
    direcciones = [(-1, 0), (1, 0), (0, -1), (0, 1), (-1, -1), (-1, 1), (1, -1), (1, 1)]
    """
    (x(filas), y(columnas))
    (-1, 0) vertical hacia arriba
    (1, 0) vertical hacia abajo
    (0, -1) horizontal a la izq
    (0, 1) horizontal a la derecha
    (-1, -1) diagonal arriba-izquierda
    (-1, 1) diagonal arriba derecha
    (1, -1) diagonal abajo izquierda
    (1, 1) diagonal abajo derecha
    """
    palabras_colocadas = [] #alamacena las palabras que ya se han colocado mas adelante

    for palabra in palabras: #recorre cada palabra de la lista
        colocada = False #una bandera para saber si ya se puso
        intentos = 0 #contador de intentos

        while not colocada and intentos < 500: #intenta hasta los 100 intentos
            random.shuffle(direcciones) #mezcla las direcciones para q sean aleatorias
            for dir_fila, dir_col in direcciones: #recorre las direcciones
            #escoge una coordenada (f,c) aleatoria para empezar a poner la palabra
                fila = random.randint(0, size - 1)
                col = random.randint(0, size - 1)

                if espacio_disponible(matriz, palabra, fila, col, dir_fila, dir_col):
                    colocar_palabra(matriz, palabra, fila, col, dir_fila, dir_col)
                    #se coloco la palabra
                    palabras_colocadas.append(palabra)
                    #agrega la palabra a la lista
                    colocada = True #la marca como colocada
                    break #sale del bucle de direcciones para esta palabra
            intentos += 1 #actualizacion del contador de intentos

    return palabras_colocadas #retorna la lista de palabras

def validar_palabra_encontrada(matriz, palabras_colocadas, inicio, fin, palabras_encontradas):
    fila_inicio, col_inicio = inicio
    fila_fin, col_fin = fin

    # Determinar dirección de la palabra
    dir_fila = fila_fin - fila_inicio
    dir_col = col_fin - col_inicio

    # Normalizar dirección (puede ser -1, 0 o 1)
    if dir_fila != 0:
        dir_fila //= abs(dir_fila)
    if dir_col != 0:
        dir_col //= abs(dir_col)

    palabra = ""
    coordenadas = []  # Guardar coordenadas de la palabra encontrada
    fila, col = fila_inicio, col_inicio

    while 0 <= fila < len(matriz) and 0 <= col < len(matriz):
        palabra += matriz[fila][col]
        coordenadas.append((fila, col))  # Guardar la posición
        if (fila, col) == (fila_fin, col_fin):  # Corregido: detenerse exactamente en la coordenada final
            break
        fila += dir_fila
        col += dir_col

    if palabra in palabras_colocadas:
        print(f"¡Bien hecho! Encontraste la palabra: {palabra}")
        palabras_colocadas.remove(palabra)
        palabras_encontradas.update(coordenadas)  # Agregar coordenadas encontradas
        imprimir_sopa_coloreada(matriz, palabras_colocadas, palabras_encontradas)  # Imprimir resaltado
        return True
    else:
        print("Palabra incorrecta, intenta de nuevo.")
        return False


def main():
    mostrar_instrucciones()
    size = obtener_dificultad()  # Obtiene el tamaño de la matriz según la dificultad elegida
    matriz = crear_matriz(size)  # Crea una matriz con el tamaño
    diccionario = categoria_choose()  # Obtiene la lista según la categoría elegida
    palabras_filtradas = [p for p in diccionario if len(p) <= size]
    palabras_ordenadas = ordenar_palabras(palabras_filtradas.copy())
    palabras_colocadas = colocar_palabras(matriz, palabras_ordenadas)  # Coloca las palabras en la matriz
    rellenar_matriz(matriz)  # Rellena los espacios vacíos
    imprimir_matriz(matriz)  # Imprime en consola
    print("\nPalabras a encontrar:")  # Instrucciones que le indican al usuario qué palabras están en la sopa
    print(", ".join(palabras_colocadas))

    palabras_encontradas = set()  # Guardará las coordenadas de las letras encontradas

    while palabras_colocadas:
        coordenada_inicio = input("Ingresa la coordenada inicial (ej. A1): ")
        coordenada_fin = input("Ingresa la coordenada final (ej. B2): ")
        inicio = convertir_coordenada(coordenada_inicio, size)
        fin = convertir_coordenada(coordenada_fin, size)
        if inicio is None or fin is None:
            continue
        validar_palabra_encontrada(matriz, palabras_colocadas, inicio, fin, palabras_encontradas)


main()
```

## Anexos

# Diagrama de flujo 

![Diagrama programacion](https://github.com/user-attachments/assets/cca64906-fe13-48c5-9ee1-9b13eeb8cc83)

 # Repositorio de avance
 
https://github.com/majoajo/Proyecto-Darth-Pyths-/tree/main

# Pseudocodigo


    Mostrar instrucciones del juego  
    
    // Selección de dificultad y tamaño de la matriz
    size ← ObtenerDificultad()  
    
    // Creación de la matriz vacía
    matriz ← CrearMatriz(size)  
    
    // Selección de la categoría de palabras
    diccionario ← CategoriaChoose()  
    
    // Filtrar palabras que quepan en la matriz
    palabras_filtradas ← FiltrarPalabras(diccionario, size)  
    
    // Ordenar palabras de mayor a menor longitud
    palabras_ordenadas ← OrdenarPalabras(palabras_filtradas)  
    
    // Colocar palabras en la matriz sin repetirlas
    palabras_colocadas ← ColocarPalabras(matriz, palabras_ordenadas)  
    
    // Rellenar los espacios vacíos con letras aleatorias
    RellenarMatriz(matriz)  
    
    // Imprimir la sopa de letras en consola
    ImprimirMatriz(matriz)  
    
    // Mostrar las palabras que el jugador debe encontrar
    MostrarPalabrasAEncontrar(palabras_colocadas)  

    // Inicializar conjunto de coordenadas de palabras encontradas
    palabras_encontradas ← ConjuntoVacio()  

    // Bucle principal del juego
    Mientras palabras_colocadas no esté vacío Hacer  

        // Pedir coordenadas al usuario
        coordenada_inicio ← PedirCoordenada("Ingresa la coordenada inicial (ej. A1): ")  
        coordenada_fin ← PedirCoordenada("Ingresa la coordenada final (ej. B2): ")  

        // Convertir coordenadas a índices de la matriz
        inicio ← ConvertirCoordenada(coordenada_inicio, size)  
        fin ← ConvertirCoordenada(coordenada_fin, size)  

        // Si las coordenadas no son válidas, volver a pedirlas
        Si inicio es NULO O fin es NULO Entonces  
            Continuar  

        // Verificar si la palabra seleccionada es correcta
        resultado ← ValidarPalabraEncontrada(matriz, palabras_colocadas, inicio, fin, palabras_encontradas)  

    Fin 

FIN  


# link colab

https://colab.research.google.com/drive/19qCDLB4LWiOlUMVunskOYs6yTs1fIMEd?authuser=1#scrollTo=_MxfwQsRYA5o


