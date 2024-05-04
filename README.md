//# compiler-final-labb
#include <iostream>
#include <fstream>
#include <stdlib.h>
#include <string.h>
#include <ctype.h>

using namespace std;

bool isKeyword(char buffer[])
{
    char keywords[32][10] = {"auto", "break", "case", "char", "const", "continue", "default",
                             "do", "double", "else", "enum", "extern", "float", "for", "goto",
                             "if", "int", "long", "register", "return", "short", "signed",
                             "sizeof", "static", "struct", "switch", "typedef", "union",
                             "unsigned", "void", "volatile", "while"};
    int i;
    bool flag = false;

    for (i = 0; i < 32; ++i)
    {
        if (strcmp(keywords[i], buffer) == 0)
        {
            flag = true;
            break;
        }
    }
    return flag;
}

bool is_valid_identifier(char *ch)
{
    if (ch[0] == '_')
    {
        return true;
    }
    else if (ch[0] >= 'A' && ch[0] <= 'Z')
    {
        return true;
    }
    else if (ch[0] >= 'a' && ch[0] <= 'z')
    {
        return true;
    }
    return false;
}

bool is_numeric(char *buffer, int sz)
{
    bool has_decimal = false;
    for (int i = 0; i < sz; i++)
    {
        if (buffer[i] < '0')
        {
            return false;
        }
        else if (buffer[i] > '9')
        {
            if (buffer[i] == '.' && !has_decimal)
            {
                has_decimal = true;
            }
            else
            {
                return false;
            }
        }
    }
    return true;
}

bool is_delimiter(char ch)
{
    char delimiters[] = ",;\t";
    for (int i = 0; i < strlen(delimiters); i++)
    {
        if (ch == delimiters[i])
        {
            return true;
        }
    }
    return false;
}

int main()
{
    char ch, buffer[15], operators[] = "+-*/%=";
    ifstream fin("Input.txt");
    int i, j = 0;

    if (!fin.is_open())
    {
        cout << "Error opening the file\n";
        exit(0);
    }

    while (!fin.eof())
    {
        ch = fin.get();

        for (i = 0; i < 6; i++)
        {
            if (ch == operators[i])
            {
                cout << ch << " is operator\n";
            }
        }

        if (is_delimiter(ch))
        {
            cout << ch << " is delimiter\n";
        }
        else if (isalnum(ch))
        {
            buffer[j++] = ch;
        }
        else if ((ch == ' ' || ch == '\n') && (j != 0))
        {
            buffer[j] = '\0';
            int temp = j;
            j = 0;

            if (isKeyword(buffer))
            {
                cout << buffer << " is keyword\n";
            }
            else if (is_valid_identifier(buffer))
            {
                cout << buffer << " is identifier\n";
            }
            else
            {
                if (is_numeric(buffer, temp))
                {
                    cout << buffer << " is a numeric constant\n";
                }
                else
                {
                    cout << buffer << " is not a valid token\n";
                }
            }
        }
    }

    fin.close();
    return 0;
}
