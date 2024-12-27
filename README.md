#include <stdio.h>

// Function to calculate the number of days in a given month
int get_days_in_month(int month, int year) {
    switch (month) {
        case 1: case 3: case 5: case 7: case 8: case 10: case 12:
            return 31;
        case 4: case 6: case 9: case 11:
            return 30;
        case 2:
            if ((year % 4 == 0 && year % 100 != 0) || (year % 400 == 0)) {
                return 29; // Leap year
            }
            return 28;
        default:
            return 0; // Invalid month
    }
}

// Function to get the day of the week for the 1st day of a given month
// Using Zeller's Congruence Algorithm
int get_first_day_of_month(int month, int year) {
    if (month < 3) {
        month += 12;
        year -= 1;
    }

    int k = year % 100;
    int j = year / 100;
    int day = (1 + (13 * (month + 1)) / 5 + k + k / 4 + j / 4 + 5 * j) % 7;

    return day; // Day of the week (0=Saturday, 1=Sunday, ..., 6=Friday)
}

// Function to print the calendar for a given month and year
void print_calendar(int month, int year) {
    printf("\nCalendar for %d-%d\n", month, year);
    printf("Sun Mon Tue Wed Thu Fri Sat\n");

    // Get the first day of the month
    int first_day = get_first_day_of_month(month, year);

    // Get the number of days in the month
    int days_in_month = get_days_in_month(month, year);

    // Print leading spaces for the first day
    for (int i = 0; i < first_day; i++) {
        printf("    ");
    }

    // Print the days of the month
    for (int day = 1; day <= days_in_month; day++) {
        printf("%3d ", day);

        // Move to the next line after Saturday
        if ((first_day + day) % 7 == 0) {
            printf("\n");
        }
    }

    printf("\n");
}

int main() {
    int month, year;

    // Input month and year from the user
    printf("Enter month (1-12): ");
    scanf("%d", &month);
    printf("Enter year: ");
    scanf("%d", &year);

    // Validate the input
    if (month < 1 || month > 12) {
        printf("Invalid month. Please enter a value between 1 and 12.\n");
        return 1;
    }

    // Print the calendar for the given month and year
    print_calendar(month, year);

    return 0;
}
