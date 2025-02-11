# Maths 2 CTF Challenge Solution

## Initial Thoughts

After solving the coding-based questions in the CTF, I realized that this particular challenge required automation using a script due to the time constraints. The challenge involved connecting to a server that would ask 100 questions, each following one of three patterns:

1. **Greatest Common Divisor (GCD)**
2. **Least Common Multiple (LCM)**
3. **Greatest Prime Factor (GPF)**

The numbers to compute were present in the question itself. For example:

```
Give me the least common multiple of 138 and 28:
```

Here, the 4th last and 2nd last words are the numbers (138 and 28). The answer was required to be printed on the same line as the question without hitting enter.

## Approach

1. **Attempt with Python**: Initially, I wrote a Python script to solve the problem, but after multiple modifications, the execution time was too high, leading to a time limit exceeded (TLE) error.
2. **Switch to C++**: To optimize performance, I decided to write the script in C++, which is significantly faster for this type of computation.
3. **Use of Regex for Parsing**: The script used regex to extract numbers from the question.
4. **Socket Programming**: The script connected to the given server, read the questions, computed the answers, and responded accordingly.

## C++ Implementation

```cpp
#include <bits/stdc++.h>
#include <arpa/inet.h>
#include <unistd.h>
#include <netdb.h>
using namespace std;

int gcd(int a, int b) {
    return b == 0 ? a : gcd(b, a % b);
}

int lcm(int a, int b) {
    return (a / gcd(a, b)) * b;
}

int greatest_prime_factor(int n) {
    int maxPrime = -1;
    while (n % 2 == 0) {
        maxPrime = 2;
        n /= 2;
    }
    for (int i = 3; i * i <= n; i += 2) {
        while (n % i == 0) {
            maxPrime = i;
            n /= i;
        }
    }
    if (n > 2) maxPrime = n;
    return maxPrime;
}

void solve(int sock) {
    char buffer[1024];
    while (true) {
        memset(buffer, 0, sizeof(buffer));
        int bytesRead = read(sock, buffer, sizeof(buffer) - 1);
        if (bytesRead <= 0) {
            cerr << "Error reading from socket or connection closed." << endl;
            break;
        }

        string question(buffer);
        cout << "Received question: " << question << endl;

        smatch match;
        int answer = 0;

        if (question.find("greatest prime factor") != string::npos) {
            regex pattern(".*?(\\d+):"); // Extract single number
            if (regex_search(question, match, pattern) && match.size() >= 2) {
                int num = stoi(match[1].str());
                cout << "Extracted number: " << num << endl;
                answer = greatest_prime_factor(num);
            } else {
                cerr << "Error: Unable to extract number from the question." << endl;
                continue;
            }
        } else {
            regex pattern(".*?(\\d+) and (\\d+):"); // Extract two numbers
            if (regex_search(question, match, pattern) && match.size() >= 3) {
                int num1 = stoi(match[1].str());
                int num2 = stoi(match[2].str());
                cout << "Extracted numbers: " << num1 << " and " << num2 << endl;

                if (question.find("greatest common divisor") != string::npos) {
                    answer = gcd(num1, num2);
                } else if (question.find("least common multiple") != string::npos) {
                    answer = lcm(num1, num2);
                } else {
                    cerr << "Error: Unknown question format." << endl;
                    continue;
                }
            } else {
                cerr << "Error: Unable to extract numbers from the question." << endl;
                continue;
            }
        }

        cout << "Computed answer: " << answer << endl;

        string response = to_string(answer) + "\n";
        int bytesWritten = write(sock, response.c_str(), response.length());
        if (bytesWritten < 0) {
            cerr << "Error writing to socket." << endl;
            break;
        }
    }
}

int main() {
    int sock = socket(AF_INET, SOCK_STREAM, 0);
    if (sock < 0) {
        cerr << "Socket creation failed" << endl;
        return 1;
    }
    cout << "Socket created successfully." << endl;

    sockaddr_in server;
    server.sin_family = AF_INET;
    server.sin_port = htons(9019);

    struct hostent *he = gethostbyname("addition.ctf.dscjssstuniv.in");
    if (he == nullptr) {
        cerr << "Hostname resolution failed" << endl;
        return 1;
    }
    memcpy(&server.sin_addr, he->h_addr_list[0], he->h_length);

    if (connect(sock, (sockaddr*)&server, sizeof(server)) < 0) {
        cerr << "Connection failed" << endl;
        return 1;
    }
    cout << "Connected to server successfully." << endl;

    solve(sock);
    close(sock);
    cout << "Connection closed." << endl;
    return 0;
}
```

## Execution & Result

1. **Compiled and ran the C++ script in the Ubuntu terminal.**
2. **Successfully connected to the server and processed 100 questions efficiently.**
3. **Retrieved the flag and submitted it to solve the challenge.**

This optimized approach helped in avoiding the time limit exceeded issue faced in Python and ensured smooth execution.
