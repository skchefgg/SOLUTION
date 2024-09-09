# SOLUTION


MADE BY -SHUBHAM KUMARRRRR___________________________________________________________________________________________________________________________________________________________________

1-TETRIS MOVES WALA

#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

int pieceTypeL(const vector<int>& columns, int base, int occurrencesBase, int basePosition) {
    int nodesMinHeight = *min_element(columns.begin(), columns.end(), [base](int a, int b) { return a != base && a < b; });
    if (occurrencesBase == 1 && columns[basePosition - 1] == 2) {
        return 0; // "TODO" can be replaced with the correct logic here
    } else if (occurrencesBase == 1) {
        return 0; // "TODO" can be replaced with the correct logic here
    } else {
        return 0;
    }
}

int pieceTypeJ(const vector<int>& columns, int base, int occurrencesBase, int basePosition) {
    if (occurrencesBase == 2 &&
        (columns[basePosition + 1] == base || columns[basePosition - 1] == base)) {
        return 1;
    } else if (occurrencesBase == 1) {
        int borderMinHeight = min(columns[basePosition - 1], columns[basePosition + 1]);
        int nodesMinHeight = *min_element(columns.begin(), columns.end(), [base](int a, int b) { return a != base && a < b; });
        if (nodesMinHeight >= borderMinHeight) {
            return 1;
        } else {
            return 0;
        }
    } else {
        return 0;
    }
}

int pieceTypeI(const vector<int>& columns, int base, int occurrencesBase) {
    if (occurrencesBase == 1) {
        return *min_element(columns.begin(), columns.end(), [base](int a, int b) { return a != base && a < b; });
    } else {
        return 0;
    }
}

int processPiece(char piece, const vector<int>& columns, int base, int occurrencesBase, int basePosition) {
    switch (piece) {
        case 'I':
            return pieceTypeI(columns, base, occurrencesBase);
        case 'J':
            return pieceTypeJ(columns, base, occurrencesBase, basePosition);
        case 'L':
            return pieceTypeL(columns, base, occurrencesBase, basePosition);
        default:
            return 0;
    }
}

int TetrisMove(vector<string> strArr) {
    char piece = strArr[0][0];
    vector<int> columns(strArr.size() - 1);
    for (size_t i = 1; i < strArr.size(); ++i) {
        columns[i - 1] = stoi(strArr[i]);
    }

    int base = *min_element(columns.begin(), columns.end());
    int occurrencesBase = count(columns.begin(), columns.end(), base);
    int basePosition = find(columns.begin(), columns.end(), base) - columns.begin();

    if (occurrencesBase >= 1 && occurrencesBase < 3) {
        return processPiece(piece, columns, base, occurrencesBase, basePosition);
    } else {
        return 0;
    }
}

int main() {
    vector<string> inputJ = {"J", "5", "6", "7", "8", "6", "7", "0", "5", "5", "8", "7", "9"};
    cout << "Answer: " << TetrisMove(inputJ) << endl;

    return 0;
}

______________________________________________________________________________________________________________________________________________________________________________________

2-STRING WALA ZIGZAG
//  For this challenge you will be printing a string in a particular zig-zag format.
/*
have the function StringZigzag(strArr) read the array of strings stored in strArr, which will contain two elements, the first some sort of string and the second element will be a number ranging from 1 to 6. The number represents how many rows to print the string on so that it forms a zig-zag pattern. For example: if strArr is ["coderbyte", "3"] then this word will look like the following if you print it in a zig-zag pattern with 3 rows:

c	   r   e
 o   e   b   t
   d       y


Your program should return the word formed by combining the characters as you iterate through each row, so for this example your program should return the string creoebtdy.
*/

#include <iostream>
#include <string>
#include <vector>
#include <sstream>
using namespace std;


string StringZigzag(string strArr[], int size)
{
	int colSize = strArr[0].length();
	int rowSize;
	istringstream(strArr[1]) >> rowSize;

	if (rowSize <= 1)
	{
		return strArr[0];
	}

	vector < vector <char> > matrixMap(rowSize);

	// loop to create a matrix map with garbage values
	for (int row = 0; row < rowSize; row++)
	{
		for (int col = 0; col < colSize; col++)
		{
			matrixMap[row].push_back('#');
		}
	}

	int currentRow = 0;
	bool traverseDown = true;

	// loop to fill the matrix map with the characters from our input string
	for (int currentCol = 0; currentCol < strArr[0].length(); currentCol++)
	{
		// adding the current character to the correct location
		matrixMap[currentRow][currentCol] = strArr[0][currentCol];

		// update our row index
		if (traverseDown)
		{
			currentRow++;
		}
		else
		{
			currentRow--;
		}

		// Condition to determine if we traverse to the bottom or the top
		if (currentRow == rowSize)
		{
			currentRow -= 2;
			traverseDown = false;
		}
		else if (currentRow == -1 && !traverseDown)
		{
			currentRow += 2;
			traverseDown = true;
		}
		
	}
	
	string result = "";
	//loop to extract the characters in their new correct order
	for (int row = 0; row < rowSize; row++)
	{
		for (int col = 0; col < colSize; col++)
		{
			if (matrixMap[row][col] != '#')
			{
				result += matrixMap[row][col];
			}
		}
	}

	return result;
}

int main() 
{
	string A[] = { "coderbyte", "3" };
	string B[] = { "cat", "5" };
	string C[] = { "kaamvjjfl", "4" };
	string D[] = { "aeettym", "1" };

	cout << StringZigzag(A, sizeof(A) / sizeof(A[0])) << endl; // creoebtdy
	cout << StringZigzag(B, sizeof(B) / sizeof(B[0])) << endl; // cat
	cout << StringZigzag(C, sizeof(C) / sizeof(C[0])) << endl; // kjajfavlm
	cout << StringZigzag(D, sizeof(D) / sizeof(D[0])) << endl; // aeettym
	return 0;
}

_
______________________________________________________________________________________________________________________________________________________________________________________
_____________________________________________________________________________________

3-GAs station

ans 1 
#include <iostream>
#include <vector>
#include <string>
#include <sstream>
#include <unordered_map>

using namespace std;

int GasStation(vector<string>& strArr) {
    int L = stoi(strArr[0]);  // First element is the number of gas stations

    // Vector to hold gas and cost pairs
    vector<pair<int, int>> ht(L + 1);

    // Splitting and filling in the gas - cost values
    for (int i = 1; i <= L; ++i) {
        stringstream ss(strArr[i]);
        string gasStr, costStr;
        getline(ss, gasStr, ':');
        getline(ss, costStr);
        int gas = stoi(gasStr);
        int cost = stoi(costStr);
        
        if (i < L) {
            ht[i] = make_pair(gas - cost, i + 1);
        } else {
            ht[L] = make_pair(gas - cost, 1);  // Last station wraps to the first one
        }
    }

    // Try starting at each station
    for (int station = 1; station <= L; ++station) {
        int current = station;
        int gas = 0;

        // Check if we can complete the loop starting from the current station
        for (int j = 0; j < L; ++j) {
            gas += ht[current].first;
            if (gas < 0) {
                break;  // If gas is negative, move to the next starting station
            }
            current = ht[current].second;  // Move to the next station
        }

        // If we completed the loop without breaking, return the starting station
        if (gas >= 0) {
            return station;
        }
    }

    return -1;  // Return -1 for "impossible"
}

int main() {
    // Example input
    vector<string> input = {"4", "4:5", "6:5", "7:3", "4:5"};
    
    int result = GasStation(input);
    
    if (result == -1) {
        cout << "impossible" << endl;
    } else {
        cout << result << endl;
    }
    
    return 0;
}


______________________________________________________________________________________________________________________________________________________________________________________
3 GAS STATION (ANS2)

ans2

#include <iostream>
#include <vector>
#include <sstream>
#include <unordered_map>

using namespace std;

string GasStation(vector<string> strArr) {
    int L = stoi(strArr[0]);
    vector<pair<int, int>> stations(L + 1);
    
    for (int i = 1; i <= L; ++i) {
        stringstream ss(strArr[i]);
        string temp;
        getline(ss, temp, ':');
        int gas = stoi(temp);
        getline(ss, temp, ':');
        int cost = stoi(temp);
        stations[i] = {gas - cost, i + 1};
    }
    
    stations[L] = {stations[L].first, 1}; // wrap around
    
    for (int start = 1; start <= L; ++start) {
        int current = start;
        int gas = 0;
        for (int j = 0; j < L; ++j) {
            gas += stations[current].first;
            if (gas < 0) {
                break;
            }
            current = stations[current].second;
        }
        if (current == start) {
            return to_string(start);
        }
    }
    return "impossible";
}


______________________________________________________________________________________________________________________________________________________________________________________

4-EXPRESSION - BASIC CALCULATOR " 2-1 + 2 "

class Solution {
public:
    int calculate(string s) {
        int n = s.size();
        stack<int>st;
        int op=1;
        int sum=0;
       for(int i=0;i<n;i++)
       {
            if(isdigit(s[i]))
            {
                int num=s[i] -'0';
                while(i+1<n && isdigit(s[i+1]))
                {
                    num = num*10 + (s[i+1] -'0');
                    i++;
                }
                sum+=num*op;
            }else if(s[i]=='(')
            {
                st.push(sum);
                st.push(op);
                sum=0;
                op=1;
            }else if(s[i]=='+')
            {
                op=1;  
            }else if(s[i]=='-')
            {
                op=-1;
            }else if(s[i]==')') 
            {
                  int oper = st.top();
                  st.pop();
                  int res = st.top();
                  st.pop();
                 sum=res+(oper*sum);
            }

       }
       return sum;
    }
};

______________________________________________________________________________________________________________________________________________________________________________________


5 HIGHEST CARD KING QUEEN WALA QUESTION
ans 1
#include <iostream>
#include <vector>
#include <string>

using namespace std;

string BlackjackHighest(vector<string> strArr) {
    int highest = -1;
    string highcard = "";
    int total = 0;
    bool aceFound = false;

    vector<string> cards = {"ace", "two", "three", "four", "five", "six", "seven", "eight", "nine", "ten", "jack", "queen", "king"};
    vector<int> values = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 10, 10, 10};

    // Loop through all the cards in strArr
    for (const auto& card : strArr) {
        for (size_t j = 0; j < cards.size(); ++j) {
            if (card == cards[j]) {
                total += values[j];  // Add the card's value to the total
                if (j > highest) {
                    highest = j;       // Keep track of the highest card
                    highcard = cards[j];
                }
                if (j == 0) aceFound = true;  // Check if it's an ace
            }
        }
    }

    // If an ace is found and the total is <= 11, count ace as 11
    if (aceFound && total <= 11) {
        total += 10;
        highest = 11;
        highcard = "ace";
    }

    // Return the appropriate message based on the total
    if (total < 21) return "below " + highcard;
    if (total > 21) return "above " + highcard;
    if (total == 21) return "blackjack " + highcard;

    return "";  // Just to handle any unexpected cases (though should never be reached)
}

int main() {
    // Example input
    vector<string> input = {"ace", "king"};

    cout << BlackjackHighest(input) << endl;

    return 0;
}

______________________________________________________________________________________________________________________________________________________________________________________


5 HIGHEST CARD KING QUEEN WALA QUESTION (ANS 2)
ans 2

ans 2

#include <iostream>
#include <vector>
#include <unordered_map>
#include <string>

using namespace std;

string ArrayChallenge(vector<string>& arr) {
    unordered_map<string, int> val = {
        {"two", 2}, {"three", 3}, {"four", 4}, {"five", 5}, {"six", 6}, {"seven", 7},
        {"eight", 8}, {"nine", 9}, {"ten", 10}, {"jack", 10}, {"queen", 10}, {"king", 10}, {"ace", 11}
    };
    
    int tot = 0, ace = 0;
    string hi = "";
    vector<string> face = {"jack", "queen", "king"};

    // Loop through the array of cards
    for (const auto& c : arr) {
        if (c == "ace") {
            ace++;  // Count the number of aces
        } else {
            tot += val[c];  // Add the value of the card to the total
            if (find(face.begin(), face.end(), c) != face.end() || 
               (val[c] > val[hi] && find(face.begin(), face.end(), hi) == face.end())) {
                hi = c;  // Determine the highest card
            }
        }
    }

    // Adjust the total score considering the aces
    for (int i = 0; i < ace; i++) {
        tot += (tot + 11 <= 21) ? 11 : 1;  // Use 11 for ace if total + 11 <= 21, otherwise 1
    }

    // If there are aces and either total is 21 or there's no high card, set ace as the high card
    if (ace > 0 && (tot == 21 || hi == "")) {
        hi = "ace";
    }

    // Determine the final output based on the total
    if (tot == 21) {
        return "blackjack " + hi;
    } else if (tot > 21) {
        return "above " + hi;
    } else {
        return "below " + hi;
    }
}

int main() {
    // Example input
    vector<string> input = {"ace", "king", "two"};

    // Call the function and print the result
    cout << ArrayChallenge(input) << endl;

    return 0;
}


______________________________________________________________________________________________________________________________________________________________________________________

6-arrsy couple
// This challenge will require knowledge of arrays.
/*
have the function ArrayCouples(arr) take the arr parameter being passed which will be an array of an even number of positive integers, and determine if each pair of integers, [k, k+1], [k+2, k+3], etc. in the array has a corresponding reversed pair somewhere else in the array. For example: if arr is [4, 5, 1, 4, 5, 4, 4, 1] then your program should output the string yes because the first pair 4, 5 has the reversed pair 5, 4 in the array, and the next pair, 1, 4 has the reversed pair 4, 1 in the array as well. But if the array doesn't contain all pairs with their reversed pairs, then your program should output a string of the integer pairs that are incorrect, in the order that they appear in the array.
For example: if arr is [6, 2, 2, 6, 5, 14, 14, 1] then your program should output the string 5,14,14,1 with only a comma separating the integers.
*/

#include <iostream>
#include <string>
#include <map>
#include <iterator>
#include <sstream>
using namespace std;

/*
one approach is to traverse the array
at each iteration store the pairs into a hash map
when iterating check if the new pair is a reversed of a pair stored in our hash map

if true, than remove the stored pair
else add the new pair to our table
*/
string ArrayCouples(int arr[], int size) 
{
	map <int, int> table;

	// traverse our array
	for (int x = 0; x < size - 1; x += 2)
	{
		bool found = false;

		// section to analyze our hash table
		for (map<int, int>::iterator y = table.begin(); y != table.end(); y++)
		{
			// condition to check if pair has a reverse
			if ((y->first == arr[x] && y->second == arr[x + 1]) || (y->first == arr[x + 1] && y->second == arr[x]))
			{
				// remove the stored pair from our table
				table.erase(y->first);
				found = true;
				break;
			}
		}

		if (!found)
		{
			// adding to our table if the pair is new
			table[arr[x]] = arr[x + 1];
		}
	}

	// checking result based on the hash table
	if (table.empty())
	{
		return "yes";
	}

	// converting to string
	ostringstream convert;
	for (map<int, int>::iterator x = table.begin(); x != table.end(); x)
	{
		convert << x->first << "," << x->second;
		x++;

		if (x != table.end())
		{
			convert << ",";
		}
	}

	return convert.str();
}

int main() 
{
	int A[] = { 4, 5, 1, 4, 5, 4, 4, 1 };
	int B[] = { 6, 2, 2, 6, 5, 14, 14, 1 };
	int C[] = { 2, 1, 1, 2, 3, 3 };
	int D[] = { 5, 4, 6, 7, 7, 6, 4, 5 };

	cout << ArrayCouples(A, sizeof(A)/sizeof(A[0])) << endl; // yes
	cout << ArrayCouples(B, sizeof(B) / sizeof(B[0])) << endl; // 5,14,14,1
	cout << ArrayCouples(C, sizeof(C) / sizeof(C[0])) << endl; // 3,3
	cout << ArrayCouples(D, sizeof(D) / sizeof(D[0])) << endl; // yes
	return 0;

}



______________________________________________________________________________________________________________________________________________________________________________________

7-city traffic (BHOT JYADA BADA HAI..... GAAND FAT JAYEGII)

// For this challenge you will be finding the maximum traffic that will enter a node.
/*
have the function CityTraffic(strArr) read strArr which will be a representation of an undirected graph in a form similar to an adjacency list. Each element in the input will contain an integer which will represent the population for that city, and then that will be followed by a comma separated list of its neighboring cities and their populations (these will be separated by a colon). For example: strArr may be
["1:[5]", "4:[5]", "3:[5]", "5:[1,4,3,2]", "2:[5,15,7]", "7:[2,8]", "8:[7,38]", "15:[2]", "38:[8]"]. This graph then looks like the following picture:

Each node represents the population of that city and each edge represents a road to that city. Your goal is to determine the maximum traffic that would occur via a single road if everyone decided to go to that city. For example: if every single person in all the cities decided to go to city 7, then via the upper road the number of people coming in would be (8 + 38) = 46. If all the cities beneath city 7 decided to go to it via the lower road, the number of people coming in would be (2 + 15 + 1 + 3 + 4 + 5) = 30. So the maximum traffic coming into the city 7 would be 46 because the maximum value of (30, 46) = 46.

Your program should determine the maximum traffic for every single city and return the answers in a comma separated string in the format: city:max_traffic,city:max_traffic,... The cities should be outputted in sorted order by the city number. For the above example, the output would therefore be: 1:82,2:53,3:80,4:79,5:70,7:46,8:38,15:68,38:45. The cities will all be unique positive integers and there will not be any cycles in the graph. There will always be at least 2 cities in the graph.

*/

#include <iostream>
#include <string>
#include <vector>
#include <sstream>
#include <map>
#include <queue>
#include <stack>
#include <algorithm>
using namespace std;

/*
Our first steps will be to set up the graph

One approach we can use is to check every city
Once we arrive at a current city we now check its neighbors, the number of neighbors determine the possible numbers of incoming traffic. Example if a city has 2 neighbors, than is there will be 2 incoming traffics into that city. 

We can perform a BFS from each neighbor start point and get a total of the population.
We store the population of each incoming traffic and record only the maximum possible traffic for that city. 
*/

// hash map to quickly index the vertices
map <int, int> nodeIndex;

// a structure representing each vertex
struct node
{
	int cost;
	bool visited;
};

// method to  create a connection between 2 nodes
void makePairs(vector <vector <node*> >& graph, int source, int partner)
{
	// making the edge connection
	graph[nodeIndex[source]].push_back(graph[nodeIndex[partner]][0]);
}

// method to create our graph
void createGraph(vector <vector <node*> >& graph, string arr[], int size)
{
	// traverse the string array to first extract the cost of each node
	for (int x = 0; x < size; x++)
	{
		// variable to collect the string number
		string num;

		for (int y = 0; y < arr[x].length(); y++)
		{
			// condition to get the cost of the current node
			if (arr[x][y] >= '0' && arr[x][y] <= '9')
			{
				num.push_back(arr[x][y]);
			}
			else
			{
				break;
			}
		}

		// converting from string to int
		int costValue;
		istringstream value(num);
		value >> costValue;

		// adding the key value pair to our table
		nodeIndex.insert(make_pair(costValue, x));

		// creating the node for our graph
		graph[x].push_back(new node);

		// setting its default values
		graph[x][0]->cost = costValue;
		graph[x][0]->visited = false;
	}
}

// method to set up the connections
void addConnections(vector <vector <node*> >& graph, string arr[], int size)
{
	for (int x = 0; x < size; x++)
	{
		string num;

		// loop to check nodes that connect to the current node
		int start = arr[x].find("[");
		for (int y = start+1; y < arr[x].length(); y++)
		{
			if (arr[x][y] >= '0' && arr[x][y] <= '9')
			{
				num.push_back(arr[x][y]);
			}
			else
			{
				// if current character does not represent a number than convert the string value we collected
				int valueCost;
				istringstream convert(num);
				convert >> valueCost;

				// here we call our make pair function to create the connection
				// we are connecting the current node to the current cost extracted
				makePairs(graph, graph[x][0]->cost, valueCost);

				// reset the string number variable
				num.clear();
			}
		}
	}
}

// each time we call this method it will reset the visit signal of each node back to false
void graphReset(vector <vector <node*> >& graph)
{
	for (int x = 0; x < graph.size(); x++)
	{
		graph[x][0]->visited = false;
	}
}

// applying BFS from the current source, as we traverse we add up the population
int bfsSum(vector < vector <node*> > graph, vector <node*> source)
{
	queue < vector <node*> > list;

	list.push(source);

	int total = 0; // keep track of the total cost from traversing from this source node

	while (!list.empty())
	{
		vector <node* > currentNode = list.front();
		currentNode[0]->visited = true;
		total += currentNode[0]->cost;
		list.pop();

		// checking the neighbors
		for (int x = 1; x < currentNode.size(); x++)
		{
			if (!currentNode[x]->visited)
			{
				// adding valid neighbors to our queue
				list.push(graph[nodeIndex[currentNode[x]->cost]]);
			}
		}
	}

	return total;
}

// sorts our 2d vector which stores the city and max traffic
// using a quick bubble sort to sort our list
void sortResult(vector< vector<int> >& list)
{
	bool swap;

	do
	{
		swap = false;

		for (int row = 0; row < list.size()-1; row++)
		{
			if (list[row][0] > list[row + 1][0])
			{
				int temp1 = list[row][0];
				int temp2 = list[row][1];

				list[row][0] = list[row + 1][0];
				list[row][1] = list[row + 1][1];

				list[row + 1][0] = temp1;
				list[row + 1][1] = temp2;

				swap = true;
			}
		}

	} while (swap);
}

string CityTraffic(string strArr[], int size)
{
	nodeIndex.clear(); // clearing our hash table

	// creating our graph and making the connection
	vector <vector <node*> > graph(size);
	createGraph(graph, strArr, size);
	addConnections(graph, strArr, size);

	string result = ""; // output the final result
	vector < vector<int> > resultOrder(size); // will store the integer values of city:max traffic

	/*
	we analyze each city here and check for all possible routes of incoming traffic
	we collect the total population of people coming in from each route
	in each calculation we are only keeping track of the highest incoming population
	*/
	for (int x = 0; x < graph.size(); x++)
	{
		// reseting our graph each time
		// this step is mandatory so that BFS checks all the valid neighbors
		graphReset(graph); 

		int high = 0; // we store the highest possible incoming traffic

		// we set the current city as visited
		graph[x][0]->visited = true;

		// we take the neighbors
		for (int y = 1; y < graph[x].size(); y++)
		{
			// calling our BFS sum it will return the total cost for this route
			int incomingTraffic = bfsSum(graph, graph[nodeIndex[graph[x][y]->cost]]);
			
			// updating our highest incoming traffic
			if ( incomingTraffic > high)
			{
				high = incomingTraffic;
			}
		}

		// storing our values in a list for sorting later
		resultOrder[x].push_back(graph[x][0]->cost);
		resultOrder[x].push_back(high);
	}

	// sorting our list
	sortResult(resultOrder);

	// loop to extract the integer values and convert back to a final string result
	for (int x = 0; x < resultOrder.size(); x++)
	{
		stringstream convert;
		convert << resultOrder[x][0];
		result += convert.str();

		result += ':';

		convert.str("");
		convert << resultOrder[x][1];
		result += convert.str();
		result += ',';
	}

	result.pop_back(); // removing the last character 
	return result;
}

int main() 
{
	string A[] = { "1:[5]", "4:[5]", "3:[5]", "5:[1,4,3,2]", "2:[5,15,7]", "7:[2,8]", "8:[7,38]", "15:[2]", "38:[8]" };
	string B[] = { "1:[5]", "2:[5]", "3:[5]", "4:[5]", "5:[1,2,3,4]" };
	string C[] = { "1:[5]", "2:[5,18]", "3:[5,12]", "4:[5]", "5:[1,2,3,4]", "18:[2]", "12:[3]" };

	cout << CityTraffic(A, sizeof(A)/sizeof(A[0])) << endl; // 1:82,2:53,3:80,4:79,5:70,7:46,8:38,15:68,38:45
	cout << CityTraffic(B, sizeof(B) / sizeof(B[0])) << endl; // 1:14,2:13,3:12,4:11,5:4
	cout << CityTraffic(C, sizeof(C) / sizeof(C[0])) << endl; // 1:44,2:25,3:30,4:41,5:20,12:33,18:27

	return 0;
}


______________________________________________________________________________________________________________________________________________________________________________________

8-STRING CHALLENDE -ENCRYPTION
#include <iostream>
#include <string>
using namespace std;

void sequenceBreak(string&, char, char, bool&, bool&);

string AlphabetRunEncryption(string str)
{
 char start = NULL;
 char end = NULL;
 bool forward = false;
 bool backward = false;

 string result = "";
 string temp = "";

 for (int current = 0; current < str.length(); current++)
 {
  if (start == NULL)
  {
   start = str[current];
   end = str[current];
  }
  else if (str[current] == char(end + 1) && current < str.length() - 2 && str[current + 2] != 'S')
  {
   end = str[current];
   forward = true;
  }
  else if (str[current] == char(end - 1) && current < str.length() - 2 && str[current + 2] != 'S')
  {
   end = str[current];
   backward = true;
  }
  else if (str[current] == 'S')
  {
   if (!result.empty() && result[result.length() - 1] == start)
   {
    result.push_back(end);
   }
   else
   {
    result.push_back(start);
    result.push_back(end);
   }

   start = end = NULL;
  }
  else if (str[current] == 'N')
  {
   if (end != start)
   {
    sequenceBreak(result, start, str[current - 2], forward, backward);
   }

   result.push_back(end);
   start = end = NULL;
  }
  else if (str[current] == 'R')
  {
   if (!result.empty() && result[result.length() - 1] == char(start - 1))
   {
    result.push_back(char(start + 1));
   }
   else
   {
    result.push_back(char(start - 1));
    result.push_back(char(start + 1));
   }

   start = end = NULL;
  }
  else if (str[current] == 'L')
  {
   if (!result.empty() && result[result.length() - 1] == char(start + 1))
   {
    result.push_back(char(start - 1));
   }
   else
   {
    result.push_back(char(start + 1));
    result.push_back(char(start - 1));
   }

   start = end = NULL;
  }
  else
  {
   if ((start != char(end + 1) && start != char(end - 1) && start != end) &&
    !(current == str.length() - 2 && str[current + 1] >= 'a' && str[current + 1] <= 'z') &&
    current != str.length() - 1)
   {
    sequenceBreak(result, start, end, forward, backward);
    start = str[current];
    end = str[current];
   }
   else if (str[current] == char(end + 1))
   {
    end = str[current];
   }
   else if (str[current] == char(end - 1))
   {
    end = str[current];
   }
  }
 }

 if (end >= 'a' && end <= 'z')
 {
  sequenceBreak(result, start, end, forward, backward);
 }

 return result;
}

void sequenceBreak(string& result, char start, char end, bool& forward, bool& backward)
{
 if (forward)
 {
  if (!result.empty() && result[result.length() - 1] == char(start - 1))
  {
   result.push_back(char(end + 1));
  }
  else
  {
   result.push_back(char(start - 1));
   result.push_back(char(end + 1));
  }

  forward = false;
 }
 else if (backward)
 {
  if (!result.empty() && result[result.length() - 1] == char(start + 1))
  {
   result.push_back(char(end - 1));
  }
  else
  {
   result.push_back(char(start + 1));
   result.push_back(char(end - 1));
  }

  backward = false;
 }
}

int main() 
{
 cout << AlphabetRunEncryption("cdefghijklmnnmlkjihgfedcb") << endl; // boa
 cout << AlphabetRunEncryption("bcdefghijklmnopqrstN") << endl; // att
 cout << AlphabetRunEncryption("abSbaSaNbR") << endl; // abaac
 cout << AlphabetRunEncryption("defghijklmnnmlkjihgfedeS") << endl; // code
 return 0;
}


______________________________________________________________________________________________________________________________________________________________________________________


9-GAME CHALLENGE -  X O <> 
XOOOO

// For this challenge you will be determining the winning position of a Tic-tac-toe board.
/*
have the function NoughtsDeterminer(strArr) take the strArr parameter being passed which will be an array of size eleven. The array will take the shape of a Tic-tac-toe board with spaces strArr[3] and strArr[7] being the separators ("<>") between the rows, and the rest of the spaces will be either "X", "O", or "-" which signifies an empty space. So for example strArr may be ["X","O","-","<>","-","O","-","<>","O","X","-"]. This is a Tic-tac-toe board with each row separated double arrows ("<>"). Your program should output the space in the array by which any player could win by putting down either an "X" or "O". In the array above, the output should be 2 because if an "O" is placed in strArr[2] then one of the players wins. Each board will only have one solution for a win, not multiple wins. You output should never be 3 or 7 because those are the separator spaces.
*/

#include <iostream>
#include <string>
using namespace std;

bool checkHorizontal(string[],int,string);
bool checkVertical(string[],int,string);
bool checkDiagonal(string[],int, string);

/*
Solution involves traversing the input array
at each index equal to "-" we check for possible solutions
we call on helper functions to check for horizontal,vertical and diagonal solution
*/
int NoughtsDeterminer(string strArr[]) 
{
	// traverse the array
	for (int x = 0; x < 11; x++)
	{
		// condition to check if current spot has a winning condition for either X or O
		if (strArr[x] == "-")
		{
			if (checkHorizontal(strArr, x, "O") || checkVertical(strArr, x, "O") || checkDiagonal(strArr,x, "O"))
			{
				return x;
			}
			else if (checkHorizontal(strArr, x, "X") || checkVertical(strArr, x, "X") || checkDiagonal(strArr,x, "X"))
			{
				return x;
			}
		}
	}
}

// method to check for winning solution going horizontal
bool checkHorizontal(string arr[], int index, string symbol)
{
	// checking which row to analyze
	if (index < 3)
	{
		for (int x = 0; x < 3; x++)
		{
			if (x == index)
			{
				continue;
			}

			if (arr[x] != symbol)
			{
				return false;
			}
		}
		return true;
	}
	else if (index > 3 && index < 7)
	{
		for (int x = 4; x < 7; x++)
		{
			if (x == index)
			{
				continue;
			}

			if (arr[x] != symbol)
			{
				return false;
			}
		}
		return true;
	}
	else if (index > 7)
	{
		for (int x = 8; x < 11; x++)
		{
			if (x == index)
			{
				continue;
			}

			if (arr[x] != symbol)
			{
				return false;
			}
		}
		return true;
	}

	return false;
}

// method to check for a winning solution going vertical
bool checkVertical(string arr[], int index, string symbol)
{
	// locating index location
	if (index < 3)
	{
		if (arr[index+4] == symbol && arr[index+8] == symbol)
		{
			return true;
		}
	}
	else if (index > 3 && index < 7)
	{
		if (arr[index+4] == symbol && arr[index-4] == symbol)
		{
			return true;
		}
	}
	else if (index > 7)
	{
		if (arr[index-4] == symbol && arr[index-8] == symbol)
		{
			return true;
		}
	}

	return false;
}

// method to check for a winning solution going diagonal
bool checkDiagonal(string arr[], int index, string symbol)
{
	// checking which row to analyze
	if (index < 3)
	{
		if ((arr[5] == symbol && arr[8] == symbol && index == 2) || (arr[5] == symbol && arr[10] == symbol && index == 0))
		{
			return true;
		}
	}
	else if (index > 3 && index < 7)
	{
		if ((arr[2] == symbol && arr[8] == symbol && index == 5) || (arr[0] == symbol && arr[10] == symbol && index == 5))
		{
			return true;
		}
	}
	else if (index > 7)
	{
		if ((arr[5] == symbol && arr[0] == symbol && index == 10) || (arr[5] == symbol && arr[2] == symbol && index == 8))
		{
			return true;
		}
	}

	return false;
}

int main() 
{
	string A[] = { "X", "O", "-", "<>", "-", "O", "-", "<>", "O", "X", "-" };
	string B[] = { "X", "-", "O", "<>", "-", "-", "O", "<>", "-", "-", "X" };
	string C[] = { "X", "O", "X", "<>", "-", "O", "O", "<>", "X", "X", "O" };
	string D[] = { "O", "-", "O", "<>", "-", "X", "-", "<>", "-", "-", "X" };
	string E[] = { "X", "-", "X", "<>", "-", "O", "-", "<>", "-", "-", "O" };
	string F[] = { "X", "-", "X", "<>", "-", "-", "O", "<>", "O", "-", "-" };
	string G[] = { "X", "O", "X", "<>", "-", "O", "-", "<>", "-", "-", "-" };

	cout << NoughtsDeterminer(A) << endl; // 2
	cout << NoughtsDeterminer(B) << endl; // 5
	cout << NoughtsDeterminer(C) << endl; // 4 
	cout << NoughtsDeterminer(D) << endl; // 1
	cout << NoughtsDeterminer(E) << endl; // 1
	cout << NoughtsDeterminer(F) << endl; // 1
	cout << NoughtsDeterminer(G) << endl; // 9
	return 0;
}



______________________________________________________________________________________________________________________________________________________________________________________


10 - BOYS AND GIRLS WALA QUETION
[B G N]

int factorial(int n) {
    int res = 1;
    for (int i = 2; i <= n; i++) {
        res *= i;
    }
    return res;
}
int combination(int n, int r) {
    return factorial(n) / (factorial(r) * factorial(n - r));
}

int ArrayChallenge(int arr[]) {
    int B = arr[0], G = arr[1], N = arr[2];
    int halfN = N / 2;
    
    int boysComb = combination(B, halfN);
    int girlsComb = combination(G, halfN);
    int pairingWays = factorial(halfN);

    return boysComb * girlsComb * pairingWays;
}



______________________________________________________________________________________________________________________________________________________________________________________


11 -determine the largets element and then determine wherther or not you can reach the same element
#include <bits/stdc++.h>
using namespace std;
int left(int length, int index, int number) {
    int left = number % length;
    if (left > index) {
        left = length + index - left;
    } else {
        left = index - left;
    }
    return left;
}
int right(int length, int index, int number) {
    int right = number % length;
    if (right > length - index - 1) {
        right = right + index - length;
    } else {
        right = right + index;
    }
    return right;
}

int ArrayJumping(vector<int>& arr) {
    unordered_map<int, pair<int, int>> ht;
    int max_index = distance(arr.begin(), max_element(arr.begin(), arr.end()));
    int L = arr.size();
        for (int i = 0; i < L; i++) {
        ht[i] = {left(L, i, arr[i]), right(L, i, arr[i])};
    }
    
    if (ht[max_index].first == max_index || ht[max_index].second == max_index) {
        return 1;
    }
    unordered_set<int> travel_set = {ht[max_index].first, ht[max_index].second};
    for (int step = 2; step <= L; ++step) {
        unordered_set<int> new_travel_set = travel_set;
        for (int val : travel_set) {
            new_travel_set.insert(ht[val].first);
            new_travel_set.insert(ht[val].second);
        }
        travel_set = new_travel_set;
        
        if (travel_set.find(max_index) != travel_set.end()) {
            return step;
        }
    }
    return -1;
}

______________________________________________________________________________________________________________________

12 -- (x.,y) INTERSECTION NO INTERSECTION




#include <iostream>
#include <string>
#include <cmath>
#include <algorithm>
#include <utility>

using namespace std;

// Function to parse the coordinates from string format "(x,y)"
// pair<int, int> parseCoordinate(const string &coord)
// {
//     int x, y;
//     sscanf(coord.c_str(), "(%d,%d)", &x, &y); // Parsing the coordinates
//     return {x, y};
// }

pair<int, int> parseCoordinate(const string &coord)
{
    int x, y;
    char ignore;
    stringstream ss(coord);                     // Create a stringstream from the input string
    ss >> ignore >> x >> ignore >> y >> ignore; // Ignore the parentheses and extract x, y
    return {x, y};                              // Return the parsed coordinates as a pair
}

// Function to compute the greatest common divisor (GCD)
int gcd(int a, int b)
{
    return b == 0 ? a : gcd(b, a % b);
}

// Function to return the simplified fraction of the intersection point
string fraction(int num, int denom)
{
    if (denom == 0)
        return "no intersection"; // If the denominator is zero, there's no valid fraction
    if (num % denom == 0)
        return to_string(num / denom); // If divisible, return the integer result

    int g = gcd(abs(num), abs(denom)); // Simplifying the fraction using GCD
    num /= g;
    denom /= g;

    return (denom < 0 ? "-" : "") + to_string(num) + "/" + to_string(abs(denom)); // Ensure proper sign for fraction
}

// Function to compute the intersection point of two lines defined by four points
string intersectionPoint(pair<int, int> p1, pair<int, int> p2, pair<int, int> p3, pair<int, int> p4)
{
    // Line 1 represented by (p1, p2): A1 * x + B1 * y = C1
    int A1 = p2.second - p1.second;
    int B1 = p1.first - p2.first;
    int C1 = A1 * p1.first + B1 * p1.second;

    // Line 2 represented by (p3, p4): A2 * x + B2 * y = C2
    int A2 = p4.second - p3.second;
    int B2 = p3.first - p4.first;
    int C2 = A2 * p3.first + B2 * p3.second;

    // Determinant to check for parallel or intersecting lines
    int det = A1 * B2 - A2 * B1;

    if (det == 0)
    {
        return "no intersection"; // Parallel lines, no intersection
    }
    else
    {
        // Calculating the intersection point (x, y)
        int xNumerator = C1 * B2 - C2 * B1;
        int yNumerator = A1 * C2 - A2 * C1;
        int denom = det;

        string x = fraction(xNumerator, denom); // Simplifying the x coordinate
        string y = fraction(yNumerator, denom); // Simplifying the y coordinate

        return "(" + x + "," + y + ")"; // Returning the intersection point in string format
    }
}

// Main function to handle input and output for the Math Challenge
string MathChallenge(string strArr[], int arrLength)
{
    if (arrLength != 4)
        return "Invalid input"; // If the number of coordinates is incorrect

    auto p1 = parseCoordinate(strArr[0]);
    auto p2 = parseCoordinate(strArr[1]);
    auto p3 = parseCoordinate(strArr[2]);
    auto p4 = parseCoordinate(strArr[3]);

    return intersectionPoint(p1, p2, p3, p4); // Calculating the intersection point
}

int main()
{
    // Sample input
    string input[] = {"(1,2)", "(3,4)", "(5,6)", "(7,8)"};
    int arrLength = 4;

    // Calling the MathChallenge function and displaying the result
    cout << MathChallenge(input, arrLength) << endl;

    return 0;
}



_________________________________________________________________________________________________________________________

14- 8 cross 8 chess board wala questiom

#include <iostream>
#include <vector>
#include <queue>

using namespace std;

struct Position
{
    int x, y;
};

int GameChallenge(int x1, int y1, int x2, int y2)
{
    // Possible moves of a knight in chess (L-shaped moves)
    vector<pair<int, int>> moves = {
        {2, 1}, {2, -1}, {-2, 1}, {-2, -1}, {1, 2}, {1, -2}, {-1, 2}, {-1, -2}};

    // Queue for BFS and visited array
    queue<Position> q;
    vector<vector<bool>> visited(9, vector<bool>(9, false));

    q.push({x1, y1}); // Start from the initial position
    visited[x1][y1] = true;

    int movesCount = 0; // Number of moves to reach the target

    // Breadth-first search (BFS)
    while (!q.empty())
    {
        int size = q.size();
        for (int i = 0; i < size; ++i)
        {
            Position current = q.front();
            q.pop();

            // If reached the target position, return the number of moves
            if (current.x == x2 && current.y == y2)
            {
                return movesCount;
            }

            // Explore all possible knight moves
            for (auto move : moves)
            {
                int nx = current.x + move.first;
                int ny = current.y + move.second;

                // Check if the new position is valid and not visited
                if (nx > 0 && nx <= 8 && ny > 0 && ny <= 8 && !visited[nx][ny])
                {
                    visited[nx][ny] = true; // Mark as visited
                    q.push({nx, ny});       // Add new position to the queue
                }
            }
        }
        movesCount++; // Increment move count after processing one level of BFS
    }

    return -1; // Return -1 if no valid path is found
}

int main()
{
    int x1, y1, x2, y2;

    // Input using cin
    cout << "Enter starting position (x1 y1): ";
    cin >> x1 >> y1;
    cout << "Enter target position (x2 y2): ";
    cin >> x2 >> y2;

    cout << "Minimum moves: " << GameChallenge(x1, y1, x2, y2) << endl;

    return 0;
}

/// another solution---------

struct Position
{
    int x, y;
};

int GameChallenge(string str)
{

    vector<pair<int, int>> moves = {{2, 1}, {2, -1}, {-2, 1}, {-2, -1}, {1, 2}, {1, -2}, {-1, 2}, {-1, -2}};
    int x1, y1, x2, y2;
    sscanf(str.c_str(), "(%d %d)(%d %d)", &x1, &y1, &x2, &y2);
    queue<Position> q;
    vector<vector<bool>> visited(9, vector<bool>(9, false));
    q.push({x1, y1});
    visited[x1][y1] = true;

    int movesCount = 0;

    while (!q.empty())
    {
        int size = q.size();
        for (int i = 0; i < size; ++i)
        {
            Position current = q.front();
            q.pop();

            if (current.x == x2 && current.y == y2)
            {
                return movesCount;
            }

            for (auto move : moves)
            {
                int nx = current.x + move.first;
                int ny = current.y + move.second;

                if (nx > 0 && nx <= 8 && ny > 0 && ny <= 8 && !visited[nx][ny])
                {
                    visited[nx][ny] = true;
                    q.push({nx, ny});
                }
            }
        }
        movesCount++;
    }
    return -1;
}







