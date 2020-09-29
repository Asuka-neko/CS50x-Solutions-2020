```c
#include <cs50.h>
#include <stdio.h>
#include <string.h>

// Max number of candidates
#define MAX 9

// preferences[i][j] is number of voters who prefer i over j
int preferences[MAX][MAX];

// locked[i][j] means i is locked in over j
bool locked[MAX][MAX];

// Each pair has a winner, loser
typedef struct
{
    int winner;
    int loser;
}
pair;

// Array of candidates
string candidates[MAX];
pair pairs[MAX * (MAX - 1) / 2];

int pair_count;
int candidate_count;

// Function prototypes
bool vote(int rank, string name, int ranks[]);
void record_preferences(int ranks[]);
void add_pairs(void);
void sort_pairs(void);
void lock_pairs(void);
void print_winner(void);

bool hasCycle(int winner, int loser)
{
    // When the winner exists and the candidates in the pair doesn't have the same votes 
  	while (winner != -1 && winner != loser)
    {
        // record the winner of the locked pair
        bool found = false;
        for (int i = 0; i < candidate_count; i++)
        {
            if (locked[i][winner])
            {
                found = true;
                winner = i;
            }
        }
      
        // exit the loop
        if (!found)
        {
            winner = -1;
        }
    }
    // check if it's a circle
    if (winner == loser)
    {
        return true;
    }
    return false;
}

int main(int argc, string argv[])
{
    // Check for invalid usage
    if (argc < 2)
    {
        printf("Usage: tideman [candidate ...]\n");
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
        candidates[i] = argv[i + 1];
    }

    // Clear graph of locked in pairs
    for (int i = 0; i < candidate_count; i++)
    {
        for (int j = 0; j < candidate_count; j++)
        {
            locked[i][j] = false;
        }
    }

    pair_count = 0;
    int voter_count = get_int("Number of voters: ");

    // Query for votes
    for (int i = 0; i < voter_count; i++)
    {
        // ranks[i] is voter's ith preference
        int ranks[candidate_count];

        // Query for each rank
        for (int j = 0; j < candidate_count; j++)
        {
            string name = get_string("Rank %i: ", j + 1);

            if (!vote(j, name, ranks))
            {
                printf("Invalid vote.\n");
                return 3;
            }
        }

        record_preferences(ranks);

        printf("\n");
    }

    add_pairs();
    sort_pairs();
    lock_pairs();
    print_winner();
    return 0;
}

// Update ranks given a new vote
bool vote(int rank, string name, int ranks[])
{
    //look for candidates.name
    //if found, updates, else return false
    //return true.ranks[i](the voter's ith preference)
    for (int i = 0; i < candidate_count; i++)
    {
        if (strcmp(candidates[i], name) == 0)
        {
            ranks[rank] = i;
            return true;
        }
    }
    return false;
}

// Update preferences given one voter's ranks
void record_preferences(int ranks[])
{
    //take ranks
    //update the preferences array
    for (int i = 0; i < candidate_count; i++)
    {
        for (int j = i + 1; j < candidate_count; j++)
        {
            preferences[ranks[i]][ranks[j]]++;
        }
    }
}

// Record pairs of candidates where one is preferred over the other
void add_pairs(void)
{
    //if !tied, add pairs to the pairs array
    //pair_count++
    for (int row = 0; row < MAX; row++)
    {
        for (int column = row + 1; column < MAX; column++)
        {
            int ci = preferences[row][column];
            int cj = preferences[column][row];
            if (ci != cj)
            {
                pair p;
                if (ci > cj)
                {
                    p.winner = row;
                    p.loser = column;
                }
                else
                {
                    p.winner = column;
                    p.loser = row;
                }
                pairs[pair_count++] = p;
            }
        }
    }
    return;
}

// Sort pairs in decreasing order by strength of victory
void sort_pairs(void)
{
    for (int i = 0; i < pair_count; i++)
    {
        int max_index = i;
        int current_strength = preferences[pairs[i].winner][pairs[i].loser] - preferences[pairs[i].loser][pairs[i].winner];

        for (int j = i + 1; j < pair_count; j++)
        {
            int temp_strength = preferences[pairs[j].winner][pairs[j].loser] - preferences[pairs[j].loser][pairs[j].winner];
            if (temp_strength > current_strength)
            {
                max_index = j;
                current_strength = preferences[pairs[j].winner][pairs[j].loser] - preferences[pairs[j].loser][pairs[j].winner];
            }
        }

        pair temp = pairs[max_index];
        pairs[max_index] = pairs[i];
        pairs[i] = temp;
    }
}

// Lock pairs into the candidate graph in order, without creating cycles
void lock_pairs(void)
{
    for (int i = 0; i < pair_count; i++)
    {
        if (!hasCycle(pairs[i].winner, pairs[i].loser))
        {
            locked[pairs[i].winner][pairs[i].loser] = true;
        }
    }
}

// Print the winner of the election
void print_winner(void)
{
    for (int column = 0; column < MAX; column++)
    {
        bool found_source = true;
        for (int row = 0; row < MAX; row++)
        {
            if (locked[row][column] == true)
            {
                found_source = false;
                break;
            }
        }
        if (found_source)
        {
            printf("%s\n", candidates[column]);
            return;
        }
    }
    return;
}


```

