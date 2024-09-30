#include <stdio.h>
#include <string.h>
#include <stdint.h>
#include <stdbool.h>
#include <ctype.h>

const char *const error_msg_decimal = "Error! That set of number is not a decimal number.\n";
const char *const error_msg_trinary = "Error! That set of number is not a trinary number.\n";
const char *const error_msg_duodecimal = "Error! That set of number is not a duodecimal number.\n";
const char *const error_msg_unsupported_system = "Error! The number system is not supported.\n";

const char *const msg_prompt_number = "Please enter a set of number:\n";
const char *const msg_prompt_current_number_system = "Please enter the current number system:\n";
const char *const msg_prompt_number_system_to_convert = "Please enter the number system you want the set of number be converted to:\n";
const char *const msg_output = "Output=";

long long int toDecimal(const char *number, int base)
{
    long long int result = 0;
    int length = strlen(number);
    for (int i = 0; i < length; i++)
    {
        int value = number[length - 1 - i] - '0';
        if (number[length - 1 - i] >= 'A')
        {
            value = number[length - 1 - i] - 'A' + 10;
        }
        result += value * pow(base, i);
    }
    return result;
}

void toBase(long long int number, int base, char *result)
{
    int index = 0;
    if (number == 0)
    {
        result[index++] = '0';
    }
    else
    {
        while (number > 0)
        {
            result[index++] = (number % base) + '0';
            number /= base;
        }
    }
    result[index] = '\0';
    for (int i = 0; i < index / 2; i++)
    {
        char temp = result[i];
        result[i] = result[index - 1 - i];
        result[index - 1 - i] = temp;
    }
}

int main()
{
    long long int decimalValue;
    char number[65];
    char convertedNumber[65];
    int frombase, tobase;
    printf("%s", msg_prompt_number);
    scanf("%s", number);
    long long int result = 0;
    int length = strlen(number);
    printf("%s", msg_prompt_current_number_system);
    scanf("%d", &frombase);
    if (frombase != 12 && frombase != 3 && frombase != 10)
    {
        printf("%s", error_msg_unsupported_system);
        return 0;
    }
    else if (frombase == 3)
    {
        for (int i = 0; i < length; i++)
        {
            if (number[i] - '0' > 3)
            {
                printf("%s", error_msg_trinary);
                return 0;
            }
        }
    }
    else if (frombase == 10)
    {
        for (int i = 0; i < length; i++)
        {
            if (number[i] > 57)
            {
                printf("%s", error_msg_decimal);
                return 0;
            }
        }
    }
    else if (frombase == 12)
    {
        for (int i = 0; i < length; i++)
        {
            if (number[i] > 'B')
            {
                printf("%s", error_msg_duodecimal);
                return 0;
            }
        }
    }
    printf("%s", msg_prompt_number_system_to_convert);
    scanf("%d", &tobase);
    decimalValue = toDecimal(number, frombase);
    toBase(decimalValue, tobase, convertedNumber);
    printf("after convert: %s\n", convertedNumber);
    return 0;
}
// C programmers never die. They are just cast into void.
