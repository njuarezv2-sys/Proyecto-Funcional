# Proyecto-Funcional
Gestor de Notas Funcional
# GESTOR DE CURSOS Y NOTAS BY NERI JUAREZ.  :)
cursos = ["Contabilidad","Mate Discreta" , "PreCálculo", "Algoritmos", "Algebra lineal"]
notas = [83, 68, 61, 74, 80]
cola = []
historial = []

def menu():
    print("""
>>>>> MENU <<<<<
1. Agregar curso
2. Mostrar cursos
3. Promedio general
4. Aprobados / Reprobados
5. Buscar curso
6. Cambiar nota
7. Borrar curso
8. Ordenar por nota
9. Ordenar por nombre
10. Buscar binario
11. Revisiones
12. Ver historial
13. Salir
------------------
""")

#agregar un curso>>>>
def agregar():
    try:
        n = int(input("¿Cuántos cursos quiere agregar?: "))
    except:
        print("Solo números por favor")
        return
    for i in range(n):
        nombre = input(f"Curso {i+1}: ").strip()
        if not nombre or nombre in cursos:
            print("Ya existe o está vacío")
            continue
        try:
            nota = float(input("Nota: "))
            if nota < 0 or nota > 100:
                print("Nota fuera de rango (0-100)")
                continue
        except:
            print("Error, debe ser número")
            continue
        cursos.append(nombre)
        notas.append(nota)
    print("Cursos agregados :D")


#Mostrar los cursos>>>>
def mostrar():
    if not cursos:
        print("No hay cursos aún")
        return
    for i in range(len(cursos)):
        print(f"{i+1}. {cursos[i]} - {notas[i]} pts")


#Promedio de las notas en general>>>
def promedio():
    if notas:
        prom = sum(notas)/len(notas)
        print(f"Promedio general: {prom:.2f}")
    else:
        print("No hay notas todavía")


#Cursos aprobados>>>>
def aprobados():
    for i in range(len(cursos)):
        estado = "Aprobado" if notas[i] >= 61 else "Reprobado"
        print(f"{cursos[i]}: {notas[i]} ({estado})")


#Buscar por curso>>>>
def buscar():
    nom = input("Nombre del curso: ").strip()
    if nom in cursos:
        i = cursos.index(nom)
        print(f"{cursos[i]} - Nota: {notas[i]}")
    else:
        print("No se encontró ese curso")


#Cambiar la nota de cursos>>>
def cambiar_nota():
    nom = input("Curso a cambiar nota: ").strip()
    if nom not in cursos:
        print("No existe")
        return
    i = cursos.index(nom)
    try:
        nueva = float(input("Nueva nota: "))
        if 0 <= nueva <= 100:
            historial.append(f"{nom}: {notas[i]} → {nueva}")
            notas[i] = nueva
            print("Nota cambiada")
        else:
            print("Fuera de rango")
    except:
        print("Dato no válido")


#Borrar un curso >>>>>>
def borrar():
    nom = input("Curso a borrar: ").strip()
    if nom in cursos:
        i = cursos.index(nom)
        historial.append(f"Se borró {nom} ({notas[i]})")
        cursos.pop(i)
        notas.pop(i)
        print("Curso eliminado")
    else:
        print("No encontrado")


#Ordenar>>>
def ordenar_nota():
    for i in range(len(notas)-1):
        for j in range(len(notas)-1-i):
            if notas[j] > notas[j+1]:
                notas[j], notas[j+1] = notas[j+1], notas[j]
                cursos[j], cursos[j+1] = cursos[j+1], cursos[j]
    print("Ordenado por nota :)")

def ordenar_nombre():
    for i in range(1, len(cursos)):
        c_act, n_act = cursos[i], notas[i]
        j = i - 1
        while j >= 0 and cursos[j].lower() > c_act.lower():
            cursos[j+1], notas[j+1] = cursos[j], notas[j]
            j -= 1
        cursos[j+1], notas[j+1] = c_act, n_act
    print("Ordenado por nombre :)")

def buscar_binario():
    ordenar_nombre()
    nom = input("Buscar curso: ").lower()
    izq, der = 0, len(cursos)-1
    while izq <= der:
        mid = (izq + der)//2
        if cursos[mid].lower() == nom:
            print(f"{cursos[mid]} - Nota: {notas[mid]}")
            return
        elif nom < cursos[mid].lower():
            der = mid - 1
        else:
            izq = mid + 1
    print("No encontrado")

def revisiones():
    while True:
        print("\n1. Agregar  2. Atender  3. Ver cola  4. Salir")
        op = input("Opción: ")
        if op == "1":
            c = input("Curso: ").strip()
            if c in cursos: cola.append(c)
            else: print("No existe")
        elif op == "2":
            if not cola:
                print("Nada en cola")
                continue
            curso = cola.pop(0)
            i = cursos.index(curso)
            nueva = float(input(f"Nueva nota para {curso}: "))
            historial.append(f"Revisión {curso}: {notas[i]} → {nueva}")
            notas[i] = nueva
        elif op == "3":
            print("Cola actual:", cola or "vacía")
        elif op == "4":
            break

def ver_historial():
    if historial:
        print("\n--- Historial ---")
        for h in reversed(historial):
            print(h)
    else:
        print("Sin cambios aún")

# >>>> Programa principal <<<<
while True:
    menu()
    try:
        op = int(input("Opción: "))
    except:
        print("Error, ingrese número")
        continue

    if op == 1: agregar()
    elif op == 2: mostrar()
    elif op == 3: promedio()
    elif op == 4: aprobados()
    elif op == 5: buscar()
    elif op == 6: cambiar_nota()
    elif op == 7: borrar()
    elif op == 8: ordenar_nota()
    elif op == 9: ordenar_nombre()
    elif op == 10: buscar_binario()
    elif op == 11: revisiones()
    elif op == 12: ver_historial()
    elif op == 13:
        print("Saliendo..."); break
    else:
        print("Opción inválida")
