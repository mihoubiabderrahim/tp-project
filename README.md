#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define MAX_PROJECTS 100
#define MAX_HISTORY 10
#define MAX_QUEUE 100

typedef struct {
    int id;
    char title[100];
    char description[255];
    char history[MAX_HISTORY][255];
    int historyCount;
} Project;

typedef struct {
    Project projects[MAX_PROJECTS];
    int projectCount;
    int revisionQueue[MAX_QUEUE];
    int queueFront;
    int queueRear;
    int nextId;
} ProjectManager;

void initProjectManager(ProjectManager *manager) {
    manager->projectCount = 0;
    manager->queueFront = 0;
    manager->queueRear = 0;
    manager->nextId = 1;
}

void addProject(ProjectManager *manager, const char *title, const char *description) {
    if (manager->projectCount >= MAX_PROJECTS) {
        printf("Capacité maximale atteinte, impossible d'ajouter un nouveau projet.\n");
        return;
    }
    Project *newProject = &manager->projects[manager->projectCount++];
    newProject->id = manager->nextId++;
    strncpy(newProject->title, title, sizeof(newProject->title));
    strncpy(newProject->description, description, sizeof(newProject->description));
    newProject->historyCount = 0;
    strncpy(newProject->history[newProject->historyCount++], "Projet ajouté", 255);
    printf("Projet ajouté : %s (ID: %d)\n", newProject->title, newProject->id);
}

void deleteProject(ProjectManager *manager, int id) {
    int i, j;
    for (i = 0; i < manager->projectCount; i++) {
        if (manager->projects[i].id == id) {
            strncpy(manager->projects[i].history[manager->projects[i].historyCount++], "Projet supprimé", 255);
            printf("Projet supprimé : %s (ID: %d)\n", manager->projects[i].title, id);
            // Supprimer le projet en décalant les éléments
            for (j = i; j < manager->projectCount - 1; j++) {
                manager->projects[j] = manager->projects[j + 1];
            }
            manager->projectCount--;
            return;
        }
    }
    printf("Projet avec ID %d introuvable.\n", id);
}

// Rechercher un projet par ID
void searchProject(ProjectManager *manager, int id) {
    int i;
    for (i = 0; i < manager->projectCount; i++) {
        if (manager->projects[i].id == id) {
            printf("Projet trouvé : %s (ID: %d)\n", manager->projects[i].title, id);
            printf("Description : %s\n", manager->projects[i].description);
            return;
        }
    }
    printf("Projet avec ID %d introuvable.\n", id);
}

// Afficher l'historique des modifications d'un projet
void showHistory(ProjectManager *manager, int id) {
    int i, j;
    for (i = 0; i < manager->projectCount; i++) {
        if (manager->projects[i].id == id) {
            printf("Historique des modifications pour le projet ID %d :\n", id);
            for (j = 0; j < manager->projects[i].historyCount; j++) {
                printf("- %s\n", manager->projects[i].history[j]);
            }
            return;
        }
    }
    printf("Projet avec ID %d introuvable.\n", id);
}



// Gestion des files d'attente pour la revision des projet 

   // Partie1: Ajouter un projet à la file d'attente pour révision
  void addToRevisionQueue(ProjectManager *manager, int id) {
    if (manager->queueRear >= MAX_QUEUE) {
        printf("File d'attente pleine.\n");
        return;
    }
    int i;
    for (i = 0; i < manager->projectCount; i++) {
        if (manager->projects[i].id == id) {
            manager->revisionQueue[manager->queueRear++] = id;
            printf("Projet ID %d ajouté à la file d'attente pour révision.\n", id);
            return;
        }
    }
    printf("Projet avec ID %d introuvable.\n", id);
 }


 //Partie2: Traiter un projet de la file d'attente
void processRevision(ProjectManager *manager) {
    if (manager->queueFront == manager->queueRear) {
        printf("Aucun projet dans la file d'attente pour révision.\n");
        return;
    }
      int id = manager->revisionQueue[manager->queueFront++];
    int i;
    for (i = 0; i < manager->projectCount; i++) {
        if (manager->projects[i].id == id) {
            strncpy(manager->projects[i].history[manager->projects[i].historyCount++], "Projet révisé", 255);
            printf("Révision terminée pour le projet ID %d : %s\n", id, manager->projects[i].title);
            return;
        }
    }
    printf("Projet avec ID %d introuvable dans la base de données.\n", id);
} 
int main() {
    ProjectManager manager;
    initProjectManager(&manager);
    int choice, id;
    char title[100], description[255];
    // affichage de menu 
    do {
        printf("\n--- Menu de Gestion des Projets Étudiants ---\n");
        printf("1. Ajouter un projet\n");
        printf("2. Supprimer un projet\n");
        printf("3. Rechercher un projet\n");
        printf("4. Afficher l'historique d'un projet\n");
        printf("5. Ajouter un projet à la file d'attente pour révision\n");
        printf("6. Traiter un projet de la file d'attente\n");
        printf("0. Quitter\n");
        printf("Choix : ");
        scanf("%d", &choice);
        getchar();
     
