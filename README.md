
#ifndef BIBLIOTECA_H
#define BIBLIOTECA_H

#define MAX_LIBROS 10
#define MAX_TITULO 100
#define MAX_AUTOR 50

typedef struct {
    int id;
    char titulo[MAX_TITULO];
    char autor[MAX_AUTOR];
    int anio;
    char estado[15];   // "Disponible" o "Prestado"
} Libro;

extern Libro biblioteca[MAX_LIBROS];
extern int totalLibros;

void menuPrincipal();
void registrarLibro();
void mostrarLibros();
void buscarLibro();
void actualizarEstado();
void eliminarLibro();
int validarIDUnico(int id);
void limpiarBuffer();

#endif

#include <stdio.h>
#include <string.h>
#include <stdlib.h>

Libro biblioteca[MAX_LIBROS];
int totalLibros = 0;


void menuPrincipal() {
    system("clear || cls");
    printf("========================================\n");
    printf("   SISTEMA DE GESTION DE BIBLIOTECA\n");
    printf("========================================\n");
    printf("1. Registrar libro\n");
    printf("2. Mostrar todos los libros\n");
    printf("3. Buscar libro\n");
    printf("4. Actualizar estado de libro\n");
    printf("5. Eliminar libro\n");
    printf("6. Salir\n");
    printf("========================================\n");
    printf("Total de libros: %d/%d\n", totalLibros, MAX_LIBROS);
    printf("========================================\n");
}


void registrarLibro() {
    if (totalLibros >= MAX_LIBROS) {
        printf("La biblioteca está llena.\n");
        return;
    }

    Libro nuevo;

    do {
        printf("ID del libro: ");
        scanf("%d", &nuevo.id);
        limpiarBuffer();
    } while (!validarIDUnico(nuevo.id));

    printf("Titulo: ");
    fgets(nuevo.titulo, MAX_TITULO, stdin);
    nuevo.titulo[strcspn(nuevo.titulo, "\n")] = 0;

    printf("Autor: ");
    fgets(nuevo.autor, MAX_AUTOR, stdin);
    nuevo.autor[strcspn(nuevo.autor, "\n")] = 0;

    do {
        printf("Año de publicacion (1000-2024): ");
        scanf("%d", &nuevo.anio);
        limpiarBuffer();
    } while (nuevo.anio < 1000 || nuevo.anio > 2024);

    strcpy(nuevo.estado, "Disponible");

    biblioteca[totalLibros++] = nuevo;
    printf("Libro registrado exitosamente.\n");
}


void mostrarLibros() {
    if (totalLibros == 0) {
        printf("No hay libros registrados.\n");
        return;
    }

    printf("\nID | TITULO | AUTOR | AÑO | ESTADO\n");
    for (int i = 0; i < totalLibros; i++) {
        printf("%d | %s | %s | %d | %s\n",
               biblioteca[i].id,
               biblioteca[i].titulo,
               biblioteca[i].autor,
               biblioteca[i].anio,
               biblioteca[i].estado);
    }
}

void buscarLibro() {
    int id;
    printf("Ingrese ID del libro: ");
    scanf("%d", &id);
    limpiarBuffer();

    for (int i = 0; i < totalLibros; i++) {
        if (biblioteca[i].id == id) {
            printf("\nLibro encontrado:\n");
            printf("Titulo: %s\n", biblioteca[i].titulo);
            printf("Autor: %s\n", biblioteca[i].autor);
            printf("Año: %d\n", biblioteca[i].anio);
            printf("Estado: %s\n", biblioteca[i].estado);
            return;
        }
    }
    printf("Libro no encontrado.\n");
}


void actualizarEstado() {
    int id;
    printf("Ingrese ID del libro: ");
    scanf("%d", &id);
    limpiarBuffer();

    for (int i = 0; i < totalLibros; i++) {
        if (biblioteca[i].id == id) {
            if (strcmp(biblioteca[i].estado, "Disponible") == 0)
                strcpy(biblioteca[i].estado, "Prestado");
            else
                strcpy(biblioteca[i].estado, "Disponible");

            printf("Estado actualizado.\n");
            return;
        }
    }
    printf("Libro no encontrado.\n");
}


void eliminarLibro() {
    int id;
    printf("Ingrese ID del libro a eliminar: ");
    scanf("%d", &id);
    limpiarBuffer();

    for (int i = 0; i < totalLibros; i++) {
        if (biblioteca[i].id == id) {
            for (int j = i; j < totalLibros - 1; j++)
                biblioteca[j] = biblioteca[j + 1];

            totalLibros--;
            printf("Libro eliminado correctamente.\n");
            return;
        }
    }
    printf("Libro no encontrado.\n");
}


int validarIDUnico(int id) {
    for (int i = 0; i < totalLibros; i++) {
        if (biblioteca[i].id == id)
            return 0;
    }
    return 1;
}


void limpiarBuffer() {
    int c;
    while ((c = getchar()) != '\n' && c != EOF);
}


int main() {
    int opcion;

    do {
        menuPrincipal();

        printf("Seleccione una opcion: ");
        scanf("%d", &opcion);
        limpiarBuffer();

        switch (opcion) {
            case 1: registrarLibro(); break;
            case 2: mostrarLibros(); break;
            case 3: buscarLibro(); break;
            case 4: actualizarEstado(); break;
            case 5: eliminarLibro(); break;
            case 6: printf("Saliendo del sistema...\n"); break;
            default: printf("Opcion invalida.\n");
        }

        if (opcion != 6) {
            printf("\nPresione ENTER para continuar...");
            getchar();
        }

    } while (opcion != 6);

    return 0;
}




        
       
        
