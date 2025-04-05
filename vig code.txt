#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <time.h>

void parse_pdf(const char *data, size_t size) {
    if (size >= 5 && strncmp(data, "CRASH", 5) == 0) {
        printf("Crash! Vulnerability triggered with input: %.*s\n", (int)size, data);
        exit(1);
    }
    printf("Processed input: %.*s\n", (int)size, data);
}
void generate_input(char *buffer, size_t size) {
    const char charset[] = "ABCDEFGHIJKLMNOPQRSTUVWXYZ";
    for (size_t i = 0; i < size - 1; i++) {
        buffer[i] = charset[rand() % (sizeof(charset) - 1)];
    }
    buffer[size - 1] = '\0';
}

int main() {
    srand((unsigned int)time(NULL));
    const int max_tests = 100;
    const size_t input_size = 16;

    char input[input_size];

    printf("Starting fuzzing harness for mock PDF parser...\n");

    for (int i = 0; i < max_tests; i++) {
        generate_input(input, input_size);
        if (rand() % 20 == 0) {
            strncpy(input, "CRASH", 5);
        }

        printf("Test #%d: ", i + 1);
        parse_pdf(input, strlen(input));
    }

    printf("Fuzzing completed.\n");
    return 0;
}