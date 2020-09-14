# PSET1

## Mario

```c
#include <stdio.h>
#include <cs50.h>

int main(void)
{
    int height, row, column, space;
  
  	//set the range of height to 1-8
    do
    {
        height = get_int("Height: ");
    }
    while (height < 1 || height > 8); 

    // draw the pyramid
    for (row = 0; row < height; row++)
    {
      	//print the space before the #
        for (space = 0; space < height - row - 1; space++)
        {
            printf(" ");
        }
      
      	//print the #
        for (column = 0; column <= row ; column++)
        {
            printf("#");
        }
      
      	//print the two spaces inside the pyramids
        printf("  "); 
      	
      	//print another pyramid
        for (column = 0; column <= row ; column++)
        {
            printf("#");
        }
      
        printf("\n");
    }
}
```

## Cash

```c
#include <stdio.h>
#include <cs50.h>
#include <math.h>

int main(void)
{
    float dollars;

    // take input >= 0
    do
    {
        dollars = get_float("Changed owed: ");
    }
    while (dollars <= 0);

    // tracking the coins
    int cents = round(dollars * 100);
    int coins = 0;
    
    while (cents >= 25)
    {
        cents -= 25;
        coins++;
    }
    while (cents >= 10)
    {
        cents -= 10;
        coins++;
    }
    while (cents >= 5)
    {
        cents -= 5;
        coins++;
    }
    while (cents >= 1)
    {
        cents -= 1;
        coins++;
    }
    printf("%i\n", coins);
}
```

## Credit

```c
#include <stdio.h>
#include <cs50.h>
#include <string.h>

int main(void)
{
    long cardNumber;

    // get card number
    do
    {
        cardNumber = get_long("Number: ");
    }
    while (cardNumber <= 0);

    long CC = cardNumber; //keep the cardnumber unaffected
    int sum = 0;
    int count = 0;
    long divisor = 10;

    //1st step
    CC = cardNumber;
    while (CC > 0)
    {
        int lastDigit = CC % 10;
        sum = sum + lastDigit;
        CC = CC / 100;
    }

    //2nd step
    CC = cardNumber / 10;
    while (CC > 0)
    {
        int secondToLastDigit = CC % 10;
        int timesTwo = secondToLastDigit * 2;
        sum = sum + (timesTwo % 10) + (timesTwo / 10);
        CC = CC / 100;
    }

    // length of the card number
    CC = cardNumber;
    while (CC != 0)
    {
        CC = CC / 10;
        count++;
    }

    // divisor
    for (int i = 0; i < count - 2; i++)
    {
        divisor = divisor * 10;
    }

    int firstDigit = cardNumber / divisor;
    int firstTwoDigit = cardNumber / (divisor / 10);

    // check
    if (sum % 10 == 0)
    {
        if (firstDigit == 4 && (count == 13 || count == 16))
        {
            printf("VISA\n");
        }
        else if ((firstTwoDigit == 34 || firstTwoDigit == 37) && count == 15)
        {
            printf("AMEX\n");
        }
        else if ((50 < firstTwoDigit && firstTwoDigit < 56) && count == 16)
        {
            printf("MASTERCARD\n");
        }
        else
        {
            printf("INVALID\n");
        }
    }
    else
    {
        printf("INVALID\n");
    }
}
```

# PSET2

## Readability

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

## Caesar

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

## Substitution

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

