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
