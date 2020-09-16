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

# PSET3

## Plurality

```c
#include <cs50.h>
#include <stdio.h>
#include <string.h>

// Max number of candidates
#define MAX 9

// Candidates have name and vote count
typedef struct
{
    string name;
    int votes;
}
candidate;

// Array of candidates
candidate candidates[MAX];

// Number of candidates
int candidate_count;
int voter_count;

// Function prototypes
bool vote(string name);
void print_winner(void);

int main(int argc, string argv[])
{
    // Check for invalid usage
    if (argc < 2)
    {
        printf("Usage: plurality [candidate ...]\n");
        return 1;
    }

    // Populate array of candidates
    candidate_count = argc - 1;
    if (candidate_count > MAX)
    {
        printf("Maximum number of candidates is %i\n", MAX);
        return 2;
    }
    for (int i = 0; i < candidate_count; i++)
    {
        candidates[i].name = argv[i + 1];
        candidates[i].votes = 0;
    }

    voter_count = get_int("Number of voters: ");

    // Loop over all voters
    for (int i = 0; i < voter_count; i++)
    {
        string name = get_string("Vote: ");

        // Check for invalid vote
        if (!vote(name))
        {
            printf("Invalid vote.\n");
        }
    }

    // Display winner of election
    print_winner();
    return 0;
}

// Update vote totals given a new vote
bool vote(string name)
{
    for (int i = 0; i < candidate_count; i++)
    {
        if (strcmp(candidates[i].name, name) == 0)
        {
            candidates[i].votes++;
            return true;
        }
    }
    return false;
}

// Print the winner (or winners) of the election
void print_winner(void)
{
    int max = candidates[0].votes;
    for (int i = 0; i < candidate_count; i++)
    {
        if (candidates[i].votes > max)
        {
            max = candidates[i].votes;
        }
    }

    for (int i = 0; i < candidate_count; i++)
    {
        if (candidates[i].votes == max)
        {
            printf("%s\n", candidates[i].name);
        }
    }
}
```

## Runoff

```c
#include <cs50.h>
#include <stdio.h>
#include <string.h>
#include <math.h>

// Max voters and candidates
#define MAX_VOTERS 100
#define MAX_CANDIDATES 9

// preferences[i][j] is jth preference for voter i
int preferences[MAX_VOTERS][MAX_CANDIDATES];

// Candidates have name, vote count, eliminated status
typedef struct
{
    string name;
    int votes;
    bool eliminated;
}
candidate;

// Array of candidates
candidate candidates[MAX_CANDIDATES];

// Numbers of voters and candidates
int voter_count;
int candidate_count;

// Function prototypes
bool vote(int voter, int rank, string name);
void tabulate(void);
bool print_winner(void);
int find_min(void);
bool is_tie(int min);
void eliminate(int min);

int main(int argc, string argv[])
{
    // Check for invalid usage
    if (argc < 2)
    {
        printf("Usage: runoff [candidate ...]\n");
        return 1;
    }

    // Populate array of candidates
    candidate_count = argc - 1;
    if (candidate_count > MAX_CANDIDATES)
    {
        printf("Maximum number of candidates is %i\n", MAX_CANDIDATES);
        return 2;
    }
    for (int i = 0; i < candidate_count; i++)
    {
        candidates[i].name = argv[i + 1];
        candidates[i].votes = 0;
        candidates[i].eliminated = false;
    }

    voter_count = get_int("Number of voters: ");
    if (voter_count > MAX_VOTERS)
    {
        printf("Maximum number of voters is %i\n", MAX_VOTERS);
        return 3;
    }

    // Keep querying for votes
    for (int i = 0; i < voter_count; i++)
    {

        // Query for each rank
        for (int j = 0; j < candidate_count; j++)
        {
            string name = get_string("Rank %i: ", j + 1);

            // Record vote, unless it's invalid
            if (!vote(i, j, name))
            {
                printf("Invalid vote.\n");
                return 4;
            }
        }

        printf("\n");
    }

    // Keep holding runoffs until winner exists
    while (true)
    {
        // Calculate votes given remaining candidates
        tabulate();

        // Check if election has been won
        bool won = print_winner();
        if (won)
        {
            break;
        }

        // Eliminate last-place candidates
        int min = find_min();
        bool tie = is_tie(min);

        // If tie, everyone wins
        if (tie)
        {
            for (int i = 0; i < candidate_count; i++)
            {
                if (!candidates[i].eliminated)
                {
                    printf("%s\n", candidates[i].name);
                }
            }
            break;
        }

        // Eliminate anyone with minimum number of votes
        eliminate(min);

        // Reset vote counts back to zero
        for (int i = 0; i < candidate_count; i++)
        {
            candidates[i].votes = 0;
        }
    }
    return 0;
}

// Record preference if vote is valid
bool vote(int voter, int rank, string name)
{
    for (int i = 0; i < candidate_count; i++)
    {
        if (strcmp(candidates[i].name, name) == 0)
        {
            preferences[voter][rank] = i;
            return true;
        }
    }
    return false;
}

// Tabulate votes for non-eliminated candidates
void tabulate(void)
{
    for (int i = 0; i < voter_count; i++)
    {
        int j = 0;
        while (candidates[preferences[i][j]].eliminated)
        {
            j++;
        }
        candidates[preferences[i][j]].votes++;
    }
}

// Print the winner of the election, if there is one
bool print_winner(void)
{
    int major = round(voter_count / 2);
    bool winner = false;
    for (int i = 0; i < candidate_count; i++)
    {
        if (candidates[i].votes > major)
        {
            printf("%s\n", candidates[i].name);
            winner = true;
        }
    }
    return winner;
}

// Return the minimum number of votes any remaining candidate has
int find_min(void)
{
    int lowest = voter_count;
    for (int i = 0; i < candidate_count; i++)
    {
        if (!candidates[i].eliminated && candidates[i].votes < lowest)
        {
            lowest = candidates[i].votes;
        }
    }
    return lowest;
}


// Return true if the election is tied between all candidates, false otherwise
bool is_tie(int min)
{
    for (int i = 0; i < candidate_count; i++)
    {
        if (candidates[i].eliminated == false && candidates[i].votes > min)
            return false;
    }
    return true;
}

// Eliminate the candidate (or candidates) in last place
void eliminate(int min)
{
    for (int i = 0; i < candidate_count; i++)
    {
        if (!candidates[i].eliminated && candidates[i].votes == min)
        {
            candidates[i].eliminated = true;
        }
    }
}
```

