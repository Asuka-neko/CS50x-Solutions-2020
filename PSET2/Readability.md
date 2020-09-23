# Readability

```c
#include <stdio.h>
#include <cs50.h>
#include <string.h>
#include <ctype.h>
#include <math.h>

int main(void)
{
    string s = get_string("Text: ");

    int count_words = 1, count_sentences = 0, count_letters = 0;

    for (int i = 0; i < strlen(s); i++)
    {
        if(isalpha(s[i]))
        {
            count_letters++;
        }

        if(s[i] == '!' || s[i] == '.' || s[i] == '?')
        {
            count_sentences++;
        }

        if(isspace(s[i]))
        {
            count_words++;
        }

    }

    float L = (float) count_letters * 100 / (float) count_words;
    float S = (float) count_sentences * 100 / (float) count_words;
    float index = 0.0588 * L - 0.296 * S - 15.8;
    int grade = round(index);
    if(grade >= 16)
    {
        printf("Grade 16+\n");
    }
    else if(grade < 1)
    {
        printf("Before Grade 1\n");
    }
    else
    {
        printf("Grade %i\n", grade);
    }

}
```

