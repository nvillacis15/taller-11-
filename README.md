
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



#include <stdio.h>
#include <string.h>
#include <stdlib.h>

#define MAX_LIBROS 10
#define MAX_TITULO 100
#define MAX_AUTOR 50

typedef struct {
    int id;
    char titulo[MAX_TITULO];
    char autor[MAX_AUTOR];
    int anio;
    char estado[15]; // "Disponible" o "Prestado"
} Libro;

Libro biblioteca[MAX_LIBROS];
int totalLibros = 0;


void menuPrincipal();
void registrarLibro();
void mostrarLibros();
void buscarLibro();
void actualizarEstado();
void eliminarLibro();
int validarIDUnico(int id);
void limpiarBuffer();

int main() {
    int opcion;
    
    do {
        menuPrincipal();
        printf("Seleccione una opcion: ");
        scanf("%d", &opcion);
        limpiarBuffer();
        
        switch(opcion) {
            case 1:
                registrarLibro();
                break;
            case 2:
                mostrarLibros();
                break;
            case 3:
                buscarLibro();
                break;
            case 4:
                actualizarEstado();
                break;
            case 5:
                eliminarLibro();
                break;
            case 6:
                printf("\n¡Gracias por usar el sistema!\n");
                break;
            default:
                printf("\nOpcion invalida. Intente nuevamente.\n");
        }
        
        if(opcion != 6) {
            printf("\nPresione Enter para continuar...");
            getchar();
        }
        
    } while(opcion != 6);
    
    return 0;
}

void menuPrincipal() {
    system("clear || cls"); // Limpia pantalla en Linux/Windows
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
    printf("Total de libros registrados: %d/%d\n", totalLibros, MAX_LIBROS);
    printf("========================================\n");
}

void registrarLibro() {
    if(totalLibros >= MAX_LIBROS) {
        printf("\nError: La biblioteca esta llena. No se pueden agregar mas libros.\n");
        return;
    }
    
    Libro nuevoLibro;
    
    printf("\n--- REGISTRAR NUEVO LIBRO ---\n");
    
    // Validar ID único
    do {
        printf("ID del libro: ");
        scanf("%d", &nuevoLibro.id);
        limpiarBuffer();
        
        if(!validarIDUnico(nuevoLibro.id)) {
            printf("Error: El ID ya existe. Ingrese uno diferente.\n");
        }
    } while(!validarIDUnico(nuevoLibro.id));
    
  
    printf("Titulo: ");
    fgets(nuevoLibro.titulo, MAX_TITULO, stdin);
    nuevoLibro.titulo[strcspn(nuevoLibro.titulo, "\n")] = 0; 
    
   
    printf("Autor: ");
    fgets(nuevoLibro.autor, MAX_AUTOR, stdin);
    nuevoLibro.autor[strcspn(nuevoLibro.autor, "\n")] = 0;
    
   
    do {
        printf("Año de publicacion (1000-2024): ");
        scanf("%d", &nuevoLibro.anio);
        limpiarBuffer();
        
        if(nuevoLibro.anio < 1000 || nuevoLibro.anio > 2024) {
            printf("Error: Ingrese un año valido.\n");
        }
    } while(nuevoLibro.anio < 1000 || nuevoLibro.anio > 2024);
    
   
    strcpy(nuevoLibro.estado, "Disponible");
    
    
    biblioteca[totalLibros] = nuevoLibro;
    totalLibros++;
    
    printf("\n¡Libro registrado exitosamente!\n");
}

void mostrarLibros() {
    if(totalLibros == 0) {
        printf("\nNo hay libros registrados en la biblioteca.\n");
        return;
    }
    
    printf("\n========================================================================================================\n");
    printf("%-5s %-30s %-25s %-10s %-15s\n", "ID", "TITULO", "AUTOR", "AÑO", "ESTADO");
    printf("========================================================================================================\n");
    
    for(int i = 0; i < totalLibros; i++) {
        printf("%-5d %-30s %-25s %-10d %-15s\n",
               biblioteca[i].id,
               biblioteca[i].titulo,
               biblioteca[i].autor,
               biblioteca[i].anio,
               biblioteca[i].estado);
    }
    
    printf("========================================================================================================\n");
}

void buscarLibro() {
    int opcion;
    
    printf("\n--- BUSCAR LIBRO ---\n");
    printf("1. Buscar por ID\n");
    printf("2. Buscar por titulo\n");
    printf("Seleccione opcion: ");
    scanf("%d", &opcion);
    limpiarBuffer();
    
    if(opcion == 1) {
        int id;
        printf("Ingrese ID del libro: ");
        scanf("%d", &id);
        limpiarBuffer();
        
        int encontrado = 0;
        for(int i = 0; i < totalLibros; i++) {
            if(biblioteca[i].id == id) {
                printf("\n--- INFORMACION DEL LIBRO ---\n");
                printf("ID: %d\n", biblioteca[i].id);
                printf("Titulo: %s\n", biblioteca[i].titulo);
                printf("Autor: %s\n", biblioteca[i].autor);
                printf("Año: %d\n", biblioteca[i].anio);
                printf("Estado: %s\n", biblioteca[i].estado);
                encontrado = 1;
                break;
            }
        }
        
        if(!encontrado) {
            printf("\nLibro no encontrado.\n");
        }
    }
    else if(opcion == 2) {
        char titulo[MAX_TITULO];
        printf("Ingrese titulo del libro: ");
        fgets(titulo, MAX_TITULO, stdin);
        titulo[strcspn(titulo, "\n")] = 0;
        
        int encontrado = 0;
        for(int i = 0; i < totalLibros; i++) {
            if(strcasecmp(biblioteca[i].titulo, titulo) == 0) {
                printf("\n--- INFORMACION DEL LIBRO ---\n");
                printf("ID: %d\n", biblioteca[i].id);
                printf("Titulo: %s\n", biblioteca[i].titulo);
                printf("Autor: %s\n", biblioteca[i].autor);
                printf("Año: %d\n", biblioteca[i].anio);
                printf("Estado: %s\n", biblioteca[i].estado);
                encontrado = 1;
                break;
            }
        }
        
        if(!encontrado) {
            printf("\nLibro no encontrado.\n");
        }
    }
    else {
        printf("\nOpcion invalida.\n");
    }
}

void actualizarEstado() {
    int id;
    
    printf("\n--- ACTUALIZAR ESTADO ---\n");
    printf("Ingrese ID del libro: ");
    scanf("%d", &id);
    limpiarBuffer();
    
    int encontrado = 0;
    for(int i = 0; i < totalLibros; i++) {
        if(biblioteca[i].id == id) {
            printf("\nEstado actual: %s\n", biblioteca[i].estado);
            
            if(strcmp(biblioteca[i].estado, "Disponible") == 0) {
                strcpy(biblioteca[i].estado, "Prestado");
            } else {
                strcpy(biblioteca[i].estado, "Disponible");
            }
            
            printf("Nuevo estado: %s\n", biblioteca[i].estado);
            printf("\n¡Estado actualizado exitosamente!\n");
            encontrado = 1;
            break;
        }
    }
    
    if(!encontrado) {
        printf("\nLibro no encontrado.\n");
    }
}

void eliminarLibro() {
    int id;
    
    printf("\n--- ELIMINAR LIBRO ---\n");
    printf("Ingrese ID del libro a eliminar: ");
    scanf("%d", &id);
    limpiarBuffer();
    
    int encontrado = 0;
    for(int i = 0; i < totalLibros; i++) {
        if(biblioteca[i].id == id) {
            printf("\nLibro encontrado: %s\n", biblioteca[i].titulo);
            printf("¿Esta seguro de eliminar este libro? (s/n): ");
            
            char confirmacion;
            scanf("%c", &confirmacion);
            limpiarBuffer();
            
            if(confirmacion == 's' || confirmacion == 'S') {
                for(int j = i; j < totalLibros - 1; j++) {
                    biblioteca[j] = biblioteca[j + 1];
                }
                totalLibros--;
                printf("\n¡Libro eliminado exitosamente!\n");
            } else {
                printf("\nEliminacion cancelada.\n");
            }
            
            encontrado = 1;
            break;
        }
    }
    
    if(!encontrado) {
        printf("\nLibro no encontrado.\n");
    }
}

int validarIDUnico(int id) {
    for(int i = 0; i < totalLibros; i++) {
        if(biblioteca[i].id == id) {
            return 0; 
        }
    }
    return 1; 
}

void limpiarBuffer() {
    int c;
    while((c = getchar()) != '\n' && c != EOF);
}
