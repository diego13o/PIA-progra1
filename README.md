# PIA-progra1
pia programación equipo 5

import datetime

print("Bienvenido al Instituto de Migración de Australia")

# Función para preguntar por el grado académico
def preguntar_grado_academico():
    while True:
        grado_academico = input("Ingrese su grado académico (P: Postgraduado; U: Universitario; T: Técnico; N: Ninguno): ").upper()
        if grado_academico in ['P', 'U', 'T', 'N']:
            return grado_academico
        else:
            print("Error: Ingrese una opción válida.")

# Lista de oficios
oficios_deseados = ["carpintero", "albañil", "electricista", "ingeniero civil", "ingeniero mecanico",
                    "ingeniero software", "medico general", "enfermero", "ingeniero electrico", "analista de datos"]

# Función para mostrar la lista de oficios
def mostrar_lista_oficios():
    print("\nLista de oficios:")
    for i, oficio in enumerate(oficios_deseados, 1):
        print(f"{i}. {oficio}")

# Validar respuesta para oficios
def validar_respuesta_oficios(respuesta):
    return respuesta.upper() in ['SI', 'NO']

# Calcular puntos por edad fértil
def calcular_puntos_edad_fertil(fecha_nacimiento, hoy, sexo):
    edad = hoy.year - fecha_nacimiento.year - ((hoy.month, hoy.day) < (fecha_nacimiento.month, fecha_nacimiento.day))

    # Si es femenino y menor de 20 años, otorgar 15 puntos
    if sexo == 'Femenino' and edad < 20:
        return 15
    # Si es femenino y está en el rango de edad fértil (20 a 35 años), otorgar puntos según la edad
    elif sexo == 'Femenino' and 20 <= edad <= 35:
        return max(15 - (edad - 20), 0)
    else:
        return 0

# Calcular puntos adicionales por oficio y grado académico
def calcular_puntos_adicionales(tiene_oficio, grado_academico):
    puntos_adicionales = 0
    if tiene_oficio:
        puntos_adicionales += 8
    if grado_academico == 'P':
        puntos_adicionales += 5
    elif grado_academico == 'U':
        puntos_adicionales += 3
    return puntos_adicionales

# Función para preguntar datos de progenitores
def preguntar_datos_progenitor(num_progenitor, hoy):
    print(f"\nDatos del progenitor {num_progenitor}:")

    while True:
        sexo = input("Sexo (Masculino/Femenino/No binario): ").capitalize()
        if sexo in ['Masculino', 'Femenino', 'No binario']:
            break
        else:
            print("Error: Ingrese una opción válida.")

    while True:
        fecha_nacimiento = input("Fecha de nacimiento (yyyy-mm-dd): ")

        # Asegúrate de que la fecha tenga el formato correcto
        try:
            fecha_nacimiento = datetime.datetime.strptime(fecha_nacimiento, "%Y-%m-%d").date()
            break
        except ValueError:
            print("Error: La fecha ingresada no tiene el formato correcto (YYYY-MM-DD). Inténtelo de nuevo.")

    grado_academico = preguntar_grado_academico()

    # Preguntar por oficio deseado
    while True:
        mostrar_lista_oficios()
        tiene_oficio = input("¿Tiene alguno de los 10 oficios requeridos? (Si/No): ")
        if validar_respuesta_oficios(tiene_oficio):
            break
        else:
            print("Error: Ingrese una opción válida (Si/No).")

    # Calcular puntos por edad fértil
    puntos_edad_fertil = calcular_puntos_edad_fertil(fecha_nacimiento, hoy, sexo)

    # Calcular puntos adicionales por oficio y grado académico
    puntos_adicionales = calcular_puntos_adicionales(tiene_oficio.upper() == 'SI', grado_academico)

    # Calcular puntos totales para el progenitor
    puntos_totales = 6 + puntos_edad_fertil + puntos_adicionales

    return puntos_totales

# Preguntar tipo de familia
while True:
    tipo_familia = input("¿La familia es biparental (B) o monoparental (M)? ").upper()
    if tipo_familia in ['B', 'M']:
        break
    else:
        print("Error: Ingrese una opción válida.")

# Procesar datos según el tipo de familia
hoy = datetime.date.today()

if tipo_familia == 'B':
    puntos_totales_progenitor_1 = preguntar_datos_progenitor(1, hoy)
    puntos_totales_progenitor_2 = preguntar_datos_progenitor(2, hoy)

    # Calcular puntos totales para la familia
    puntos_totales = puntos_totales_progenitor_1 + puntos_totales_progenitor_2

elif tipo_familia == 'M':
    puntos_totales_progenitor_unico = preguntar_datos_progenitor(1, hoy)
    if puntos_totales_progenitor_unico:
        # Calcular puntos totales para la familia
        puntos_totales = puntos_totales_progenitor_unico

# Preguntar datos de hijos
while True:
    try:
        numero_hijos = int(input("\n¿Cuántos hijos tiene la familia?: "))
        if numero_hijos >= 0:
            break
        else:
            print("Error: Ingrese un número válido.")
    except ValueError:
        print("Error: Ingrese un número válido.")

puntos_hijos = 0

for i in range(numero_hijos):
    print(f"\nDatos del hijo {i + 1}:")

    while True:
        sexo = input("Sexo (Masculino/Femenino/No binario): ").capitalize()
        if sexo in ['Masculino', 'Femenino', 'No binario']:
            break
        else:
            print("Error: Ingrese una opción válida.")

    while True:
        fecha_nacimiento = input("Fecha de nacimiento (yyyy-mm-dd): ")

        # Asegúrate de que la fecha tenga el formato correcto
        try:
            fecha_nacimiento = datetime.datetime.strptime(fecha_nacimiento, "%Y-%m-%d").date()
            break
        except ValueError:
            print("Error: La fecha ingresada no tiene el formato correcto (YYYY-MM-DD). Inténtelo de nuevo.")

    # Calcular puntos por edad fértil de las mujeres
    if sexo == 'Femenino':
        puntos_hijos += calcular_puntos_edad_fertil(fecha_nacimiento, hoy, sexo)

# Calcular puntos totales por hijos
puntos_totales += numero_hijos * 8 + puntos_hijos

# Mostrar el puntaje total
print(f"\nEl puntaje total de la familia es: {puntos_totales} puntos")
