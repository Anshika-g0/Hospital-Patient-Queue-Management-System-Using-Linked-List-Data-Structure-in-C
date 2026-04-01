#include <stdio.h>
#include <stdlib.h>
#include <string.h>
/* ===================================================
HOSPITAL PATIENT QUEUE MANAGEMENT SYSTEM
Using Singly Linked List in C
Student: Anshika | UID: 25BCD10154BCA
Chandigarh University | Data Science
=================================================== */
/* Patient Node Structure */
struct Patient {
int patientID;
char name[50];
int age;
char disease[50];
int priority; /* 1=Emergency, 2=Urgent, 3=Normal */
struct Patient* next;
};
struct Patient* head = NULL; /* Front of queue */
/* Create a new patient node */
struct Patient* createPatient(int id, char name[], int age,
char disease[], int priority) {
struct Patient* p = (struct Patient*)malloc(sizeof(struct Patient));
if (p == NULL) { printf("Memory error!\n"); exit(1); }
p->patientID = id;
strcpy(p->name, name);
p->age = age;
strcpy(p->disease, disease);
p->priority = priority;
p->next = NULL;
return p;
}
/* Admit Emergency Patient- Insert at Front (O(1)) */
void admitEmergency(int id, char name[], int age, char disease[]) {
struct Patient* p = createPatient(id, name, age, disease, 1);
p->next = head;
head = p;
printf("[EMERGENCY] Patient %s admitted at front of queue.\n", name);
}
/* Admit Normal Patient- Insert at End */
void admitNormal(int id, char name[], int age, char disease[]) {
struct Patient* p = createPatient(id, name, age, disease, 3);
if (head == NULL) { head = p; }
else {
struct Patient* temp = head;
while (temp->next != NULL) temp = temp->next;
temp->next = p;
}
printf("Patient %s admitted at end of queue.\n", name);
}
/* Discharge Patient- Remove from Front (O(1)) */
void dischargePatient() {
if (head == NULL) {
printf("Queue is empty. No patients to discharge.\n");
return;
}
struct Patient* temp = head;
head = head->next;
printf("Patient %s (ID: %d) discharged successfully.\n",
temp->name, temp->patientID);
free(temp);
}
/* Display entire patient queue */
void displayQueue() {
if (head == NULL) { printf("Queue is empty.\n"); return; }
struct Patient* temp = head;
int pos = 1;
printf("\n--- HOSPITAL PATIENT QUEUE---\n");
printf("%-5s %-6s %-20s %-5s %-20s %-10s\n",
"Pos","ID","Name","Age","Disease","Priority");
printf("-------------------------------------------------------------\n");
while (temp != NULL) {
char pType[15];
if (temp->priority == 1) strcpy(pType, "EMERGENCY");
else if (temp->priority == 2) strcpy(pType, "URGENT");
else strcpy(pType, "NORMAL");
printf("%-5d %-6d %-20s %-5d %-20s %-10s\n",
pos++, temp->patientID, temp->name, temp->age,
temp->disease, pType);
temp = temp->next;
}
printf("-------------------------------------------------------------\n");
}
/* Search patient by ID */
void searchPatient(int id) {
struct Patient* temp = head;
int pos = 1;
while (temp != NULL) {
if (temp->patientID == id) {
printf("Found: ID=%d Name=%s Age=%d Disease=%s\n",
temp->patientID, temp->name, temp->age, temp->disease);
return;
}
temp = temp->next; pos++;
}
printf("Patient with ID %d not found.\n", id);
}
/* Count total patients in queue */
int countPatients() {
struct Patient* temp = head;
int count = 0;
while (temp != NULL) { count++; temp = temp->next; }
return count;
}
/* Delete patient by ID (mid-queue removal) */
void deletePatientByID(int id) {
if (head == NULL) { printf("Queue empty.\n"); return; }
struct Patient* temp = head;
struct Patient* prev = NULL;
while (temp != NULL && temp->patientID != id) {
prev = temp; temp = temp->next;
}
if (temp == NULL) { printf("Patient ID %d not found.\n", id); return; }
if (prev == NULL) head = temp->next;
else prev->next = temp->next;
printf("Patient %s (ID: %d) removed from queue.\n",
temp->name, temp->patientID);
free(temp);
}
/* Main function with menu */
int main() {
int choice, id, age;
char name[50], disease[50];
printf("=== HOSPITAL PATIENT QUEUE MANAGEMENT SYSTEM ===\n");
printf("
Chandigarh University | Anshika | 25BCD10154BCA\n");
printf("================================================\n\n");
do {
printf("\n--- MENU---\n");
printf("1. Admit Emergency Patient (Front)\n");
printf("2. Admit Normal Patient (End)\n");
printf("3. Discharge Next Patient (Front)\n");
printf("4. Display Patient Queue\n");
printf("5. Search Patient by ID\n");
printf("6. Remove Patient by ID\n");
printf("7. Count Patients in Queue\n");
printf("0. Exit\n");
printf("Enter choice: ");
scanf("%d", &choice);
switch (choice) {
case 1:
printf("Enter ID, Name, Age, Disease: ");
scanf("%d %s %d %s", &id, name, &age, disease);
admitEmergency(id, name, age, disease); break;
case 2:
printf("Enter ID, Name, Age, Disease: ");
scanf("%d %s %d %s", &id, name, &age, disease);
admitNormal(id, name, age, disease); break;
case 3: dischargePatient(); break;
case 4: displayQueue(); break;
case 5:
printf("Enter Patient ID: ");
scanf("%d", &id);
searchPatient(id); break;
case 6:
printf("Enter Patient ID to remove: ");
scanf("%d", &id);
deletePatientByID(id); break;
case 7:
printf("Total patients in queue: %d\n", countPatients());
break;
case 0: printf("Exiting system...\n"); break;
default: printf("Invalid choice!\n");
}
} while (choice != 0);
return 0;
}
