"""hacer un programa que gestiones datos para una escuela. 

El programa tiene que ser capaz de:

Llevar un registro de todos los datos de alumnos de la escuela (Nombre, Apellido, fecha de nacimiento, DNI, Nombre de Tutor, registro de todas las notas, cantidad de faltas, cantidad de amonestaciones recibidas. Recomendación: Para llevar un registro de estos dato se puede utilizar un diccionario estructurado de la siguiente manera: { “Alumnos” : [alumno1,alumno2,alumno3 ] } Donde cada alumno es otro diccionario estructurado de la siguiente forma: { “Nombre”: nombre de alumno, “Apellido” : apellido de alumno, “DNI” : DNI de alumno “Fecha de nacimiento”, fecha de nacimiento de alumno, “Tutor” : nombre y apellido de tutor, “Notas” : todas las notas del alumno, “Faltas” : cantidad de faltas, “amonestaciones” : cantidad de amonestaciones } En esta estructura: Datos = { “Alumnos” : [alumno1,alumno2,alumno3 ] } Para acceder por ejemplo al numero de DNI del tercer alumno podríamos hacer algo así: Datos[“Alumnos”][2][“DNI”] Este es un ejemplo de estructura, se puede cambiar completamente o hacer algunos cambios sobre el para mejorar el orden (si lo consideran necesario); Tienen que tener en cuenta que la información se puede perder cuando finalize el programa: para evitar esto, almacenenlo en algun archivo y haz los cambios en el mismo.

b) Mostrar los datos de cada alumno 

c) Modificar los datos de los alumnos 

d) Agregar alumnos 

e) Expulsar alumnos 

RECOMEDACIONN GENERAL:

 El programa es extenso, hacer por partes. 

Llevará mucho tiempo, la paciencia es importante. Internet es una gran ayuda. 

La prolijidad es fundamental Las funciones tendrán que recibir como parámetros los diccionarios que representan a los alumnos.
"""
import json
import os

FILE_PATH = 'datos_alumnos.json'

def cargar_datos():
    """Carga los datos desde el archivo JSON."""
    if os.path.exists(FILE_PATH):
        with open(FILE_PATH, 'r') as file:
            return json.load(file)
    else:
        return {"Alumnos": []}

def guardar_datos(datos):
    """Guarda los datos en el archivo JSON."""
    with open(FILE_PATH, 'w') as file:
        json.dump(datos, file, indent=4)

def mostrar_datos_alumno(alumno):
    """Muestra los datos de un alumno."""
    print(f"Nombre: {alumno['Nombre']}")
    print(f"Apellido: {alumno['Apellido']}")
    print(f"DNI: {alumno['DNI']}")
    print(f"Fecha de nacimiento: {alumno['Fecha de nacimiento']}")
    print(f"Tutor: {alumno['Tutor']}")
    print(f"Notas: {alumno['Notas']}")
    print(f"Faltas: {alumno['Faltas']}")
    print(f"Amonestaciones: {alumno['Amonestaciones']}")

def mostrar_datos_todos(datos):
    """Muestra los datos de todos los alumnos."""
    for alumno in datos["Alumnos"]:
        mostrar_datos_alumno(alumno)
        print()

def modificar_dato_alumno(alumno, campo, valor):
    """Modifica un dato específico de un alumno."""
    if campo in alumno:
        alumno[campo] = valor
    else:
        print(f"El campo {campo} no existe en los datos del alumno.")

def agregar_alumno(datos, nuevo_alumno):
    """Agrega un nuevo alumno a la lista de alumnos."""
    datos["Alumnos"].append(nuevo_alumno)

def expulsar_alumno(datos, dni):
    """Expulsa a un alumno según su DNI."""
    datos["Alumnos"] = [alumno for alumno in datos["Alumnos"] if alumno["DNI"] != dni]

def main():
    datos = cargar_datos()
    
    while True:
        print("\n1. Mostrar datos de todos los alumnos")
        print("2. Modificar datos de un alumno")
        print("3. Agregar nuevo alumno")
        print("4. Expulsar alumno")
        print("5. Salir")
        
        opcion = input("Selecciona una opción: ")
        
        if opcion == '1':
            mostrar_datos_todos(datos)
        elif opcion == '2':
            dni = input("Introduce el DNI del alumno a modificar: ")
            alumno = next((a for a in datos["Alumnos"] if a["DNI"] == dni), None)
            if alumno:
                campo = input("Introduce el campo a modificar (Nombre, Apellido, Fecha de nacimiento, Tutor, Notas, Faltas, Amonestaciones): ")
                valor = input("Introduce el nuevo valor: ")
                modificar_dato_alumno(alumno, campo, valor)
                guardar_datos(datos)
            else:
                print("Alumno no encontrado.")
        elif opcion == '3':
            nombre = input("Introduce el nombre del nuevo alumno: ")
            apellido = input("Introduce el apellido del nuevo alumno: ")
            dni = input("Introduce el DNI del nuevo alumno: ")
            fecha_nacimiento = input("Introduce la fecha de nacimiento del nuevo alumno: ")
            tutor = input("Introduce el nombre y apellido del tutor del nuevo alumno: ")
            notas = input("Introduce las notas del nuevo alumno (separadas por comas): ").split(',')
            faltas = int(input("Introduce la cantidad de faltas del nuevo alumno: "))
            amonestaciones = int(input("Introduce la cantidad de amonestaciones del nuevo alumno: "))
            
            nuevo_alumno = {
                "Nombre": nombre,
                "Apellido": apellido,
                "DNI": dni,
                "Fecha de nacimiento": fecha_nacimiento,
                "Tutor": tutor,
                "Notas": notas,
                "Faltas": faltas,
                "Amonestaciones": amonestaciones
            }
            agregar_alumno(datos, nuevo_alumno)
            guardar_datos(datos)
        elif opcion == '4':
            dni = input("Introduce el DNI del alumno a expulsar: ")
            expulsar_alumno(datos, dni)
            guardar_datos(datos)
        elif opcion == '5':
            break
        else:
            print("Opción no válida. Por favor, selecciona una opción del menú.")

if __name__ == "__main__":
    main()
