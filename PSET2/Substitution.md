# Substitution

```c
#include <stdio.h>
#include <cs50.h>
#include <ctype.h>
#include <string.h>
#include <stdlib.h>

bool is_alpha(string s);
bool is_valid(string s);

int main(int argc, string argv[])
{
    if (argc == 2 && is_alpha(argv[1]))
    {
        if (is_valid(argv[1]))
        {
            string plaintext = get_string("plaintext: ");

            string difference = argv[1];
            for (int i = 'A'; i <= 'Z'; i++)
            {
                difference[i - 'A'] = toupper(difference[i - 'A']) - i;
            }

            printf("ciphertext: ");

            for (int i = 0; i < strlen(plaintext); i++)
            {
                if (isalpha(plaintext[i]))
                {
                    plaintext[i] = plaintext[i] + difference[plaintext[i] - (isupper(plaintext[i]) ? 'A' : 'a')];
                }
                printf("%c", plaintext[i]);
            }
            printf("\n");
            return 0;
        }
        else
        {
            printf("Key must contain 26 characters.\n");
            return 1;
        }
    }
    else
    {
        printf("Usage: ./substitution key\n");
        return 1;
    }
}

bool is_alpha(string s)
{
    for (int i = 0; i < strlen(s); i++)
    {
        if (!isalpha(s[i]))
        {
            return false;
        }
    }
    return true;
}

bool is_valid(string s)
{
    if (strlen(s) != 26)
    {
        return false;
    }

    int freq[26] = { 0 };
    for (int i = 0; i < strlen(s); i++)
    {
        int index = toupper(s[i]) - 'A';
        if (freq[index] > 0)
        {
            return false;
        }
        freq[index]++;
    }
    return true;
}
```

