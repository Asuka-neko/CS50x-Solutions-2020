# Caesar

```c
#include <stdio.h>
#include <cs50.h>
#include <ctype.h>
#include <string.h>
#include <stdlib.h>

bool is_int(string s);

int main(int argc, string argv[])
{
    if (argc != 2 || !is_int(argv[1]))
    {
        printf("Usage: ./caesar key\n");
        return 1;
    }
    else
    {
        int k = atoi(argv[1]);

        string plaintext = get_string("plaintext: ");

        printf("ciphertext: ");

        for(int i = 0; i < strlen(plaintext); i++)
        {
            if (plaintext[i] >= 'a' && plaintext[i] <= 'z')
            {
                printf("%c", (((plaintext[i] - 97) + k) % 26) + 97);
            }
            else if (plaintext[i] >= 'A' && plaintext[i] <= 'Z')
            {
                printf("%c", (((plaintext[i] - 65) + k) % 26) + 65);
            }
            else
            {
                printf("%c", plaintext[i]);
            }
        }
        printf("\n");
        return 0;
    }
}

bool is_int(string s)
{
    for(int i = 0; i < strlen(s); i++)
    {
        if (!isdigit(s[i]))
        {
            return false;
        }
    }
    return true;
}
```

