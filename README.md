# project
This program helps a professor to analyze the grades of the students in the class

/*This program helps a professor to analyze the grades of the students in the class
COP1334  MW 9:30 AM-11:15AM
By: Franklin Calderon
*/

//#include <iostream>
//#include <fstream>
//#include <string>
//#include <iomanip>
//#include <vector>

using namespace std;

const int NUM_STUDENTS = 30;
const int NUM_QUIZZES = 10;
const int NUM_EXAMS = 5;

struct student {
	
	vector<string> firstName;
	vector<string> lastName;
	vector<int> quizzes[NUM_QUIZZES];
	vector<int> exams[NUM_EXAMS];
	vector<string>info[NUM_QUIZZES]; // Added into the array for easier proccessing
};

int menu(void) {
	int userChoice;

	cout << "Please choose one of these options(from 1 to 6)" << endl;
	cout << "1)Display a grade for a particular student" << endl;
	cout << "2)Display average for a particular quiz" << endl;
	cout << "3)Display average for a particular test" << endl;
	cout << "4)Display topic with the lowest average" << endl;
	cout << "5)Display summary of each student" << endl;
	cout << "6)Display a breakdown of letter grades for the class" << endl;
	cout << "Your choice is: ";
	cin >> userChoice;

	return userChoice;
}


void choice1(student &students, string name) {
	const float worthofitem = 0.5;	//Quzzes and test are 50% of the grade
	float quizgrade,testgrade;
	bool found=false; // used if student was found in roster
	
	for (int i = 0; i < NUM_STUDENTS; i++) {
		//Checks if student is in the class. If it is then it calculates the grade for the student
		if (name == students.firstName[i] || name==students.lastName[i]) {
			found = true;
			quizgrade = (students.quizzes[0][i] + students.quizzes[1][i] + students.quizzes[2][i] + students.quizzes[3][i]
				+ students.quizzes[4][i] + students.quizzes[5][i] + students.quizzes[6][i] + students.quizzes[7][i]
				+ students.quizzes[8][i] + students.quizzes[9][i]) / static_cast<float>(NUM_QUIZZES); // turns one into float so we get a perfect value
			quizgrade = quizgrade * worthofitem;
			//cout << "Quizzes grades total: " << quizgrade << endl; // Displays Quizzes Grades
			
			testgrade = (students.exams[0][i] + students.exams[1][i] + students.exams[2][i] + students.exams[3][i]
				+ students.exams[4][i]) / static_cast<float>(NUM_EXAMS); //Turns one into float so we get a perfect value
			testgrade = testgrade * worthofitem;
			//cout << "Test grades total: " << testgrade << endl; // Displays Test grades
			
			cout << students.firstName[i] << " " << students.lastName[i]<< " received a total of: "
			<< quizgrade + testgrade << " in the class." << endl;
			break;
		}
	}
	// If student is not in the roster we will print this
	if (found == false) {
		cout << "Student not found" << endl;
	}
}


//Calculates average for the choosen quiz
void choice2(student &students, int quiznumber) {
	float average=0.0;
	for (int i = 0; i < NUM_STUDENTS; i++) {
		average += students.quizzes[quiznumber-1][i];  // quiznumber -1 because user enters from 1-10,and our quizzes are from 0-9
	}
	average = average / NUM_STUDENTS;

	cout << "The average for Quiz #" << quiznumber << " is "  << average << endl;
}

//Calculates average for the choosen test
void choice3(student &students, int testnumber) {
	float average = 0.0;
	for (int i = 0; i < NUM_STUDENTS; i++) {
		average += students.exams[testnumber - 1][i];  // testnumber -1 because user enters from 1-5,and our quizzes are from 0-4
	}
	average = average / NUM_STUDENTS;

	cout << "The average for Test #" << testnumber << " is "  << average << endl;
}

//Display topic with lowest quiz average
void choice4(student &students) {
	float average = 0.0, lowest = 100.0;

	//Goes through each quiz and calculates the average of it
	//We need to see which is the lowest average
	for (int j = 0; j < NUM_QUIZZES; j++) {
		average = 0.0; // everytime loops finish average gets reset
		for (int i = 0; i < NUM_STUDENTS; i++) {
			average += students.quizzes[j][i];
		}
		average = average / NUM_STUDENTS;
		if (average < lowest) {
			lowest = average; // lowest now equals the lowest average
		}
	}
	//Goes through quizzes again but now sees which one equals to the lowest
	for (int j = 0; j < NUM_QUIZZES; j++) {
		average = 0.0; // everytime loops finish average gets reset
		for (int i = 0; i < NUM_STUDENTS; i++) {
			average += students.quizzes[j][i];
		}
		average = average / NUM_STUDENTS;
		for (int i = 0; i < NUM_STUDENTS; i++) {
			//Prints the topic with lowest average
			if (lowest==average) {
				cout << "Students got the lowest average on topic: \"" << students.info[j][j] << "\" \nWhich was quiz #" << j+1 <<
				endl;
				break;
			}
		}
	}
}

void choice5(student &students) {
	const float worthofitem = 0.5;	//Quzzes and test are 50% of the grade
	float quizgrade, testgrade,average=0.0;

	//Shows grade for every student
	for (int i = 0; i < NUM_STUDENTS; i++) {

			quizgrade = (students.quizzes[0][i] + students.quizzes[1][i] + students.quizzes[2][i] + students.quizzes[3][i]
				+ students.quizzes[4][i] + students.quizzes[5][i] + students.quizzes[6][i] + students.quizzes[7][i]
				+ students.quizzes[8][i] + students.quizzes[9][i]) / static_cast<float>(NUM_QUIZZES); // turns one into float so we get a perfect value
			quizgrade = quizgrade * worthofitem;
			//cout << "Quizzes grades total: " << quizgrade << endl; // Displays Quizzes Grades

			testgrade = (students.exams[0][i] + students.exams[1][i] + students.exams[2][i] + students.exams[3][i]
				+ students.exams[4][i]) / static_cast<float>(NUM_EXAMS); //Turns one into float so we get a perfect value
			testgrade = testgrade * worthofitem;
			//cout << "Test grades total: " << testgrade << endl; // Displays Test grades

			cout << students.firstName[i] << " " << students.lastName[i] << " received a total of: " << quizgrade + testgrade << " in the class." << endl;
		}

	//Shows average grade for every Quiz
	for (int j = 0; j < NUM_QUIZZES; j++) {
		for (int i = 0; i < NUM_STUDENTS; i++) {
			average += students.quizzes[j][i];
		}
		average = average / NUM_STUDENTS;
		cout << "The average for quiz #" << j + 1 << " is " << average << endl;
		average = 0.0;
	}

	//Shows average grade for every exam
	for (int j = 0; j < NUM_EXAMS; j++) {
		for (int i = 0; i < NUM_STUDENTS; i++) {
			average += students.exams[j][i];
		}
		average = average / NUM_STUDENTS;
		cout << "The average for exam #" << j + 1 << " is " << average << endl;
		average = 0.0;
	}
}
//Displays breakdown of letter grades for the class
void choice6(student &students) {
	const float worthofitem = 0.5;	//Quzzes and test are 50% of the grade
	float quizgrade, testgrade, average = 0.0,total=0;
	int countA=0,countB=0,countC=0,countD=0,countF=0;
	//Shows grade for every student
	for (int i = 0; i < NUM_STUDENTS; i++) {

		quizgrade = (students.quizzes[0][i] + students.quizzes[1][i] + students.quizzes[2][i] + students.quizzes[3][i]
			+ students.quizzes[4][i] + students.quizzes[5][i] + students.quizzes[6][i] + students.quizzes[7][i]
			+ students.quizzes[8][i] + students.quizzes[9][i]) / static_cast<float>(NUM_QUIZZES); // turns one into float so we get a perfect value
		quizgrade = quizgrade * worthofitem;
		//cout << "Quizzes grades total: " << quizgrade << endl; // Displays Quizzes Grades

		testgrade = (students.exams[0][i] + students.exams[1][i] + students.exams[2][i] + students.exams[3][i]
			+ students.exams[4][i]) / static_cast<float>(NUM_EXAMS); //Turns one into float so we get a perfect value
		testgrade = testgrade * worthofitem;
		//cout << "Test grades total: " << testgrade << endl; // Displays Test grades
		total = quizgrade + testgrade;
		//Checks what the grade each student got an adds +1 to the corresponding counter
		if (total >= 90) {
			countA++;
		}
		else if(total >=80) {
			countB++;
		}
		else if (total >= 70) {
			countC++;
		}
		else if (total >= 60) {
			countD++;
		}
		else {
			countF++;
		}
	}
	cout << countA << " students got an A in the class" << endl;
	cout << countB << " students got an B in the class" << endl;
	cout << countC << " students got an C in the class" << endl;
	cout << countD << " students got an D in the class" << endl;
	cout << countF << " students got an F in the class" << endl;
}

int main()
{	
	//assings students as structure name for main
	student students;

	//Variables for the file CGS1060 being read
	string first, last;
	int quiz0, quiz1, quiz2, quiz3, quiz4, quiz5, quiz6, quiz7, quiz8, quiz9,
		exam1, exam2, exam3, exam4, exam5;

	//Values for the file Topics being read
	string topic;

	ifstream inputFile; // for cgs1060 file
	ifstream topics;  //for topics file

	inputFile.open("C:/Users/Franklin/Desktop/cgs1060.txt"); // Location of file
	topics.open("C:/Users/Franklin/Desktop/Topics.txt");
	
	//If cannot open the file it will give this error
	if (!inputFile || !topics)
	{
		cout << "Could not open file." << endl;
		exit(2);
	}

	//Reads the file and assigns value to a variable by the order i gave it
	while (inputFile >> first >> last >> quiz0 >> quiz1 >> quiz2 >> quiz3 >> quiz4 >>
		 quiz5 >> quiz6 >> quiz7 >> quiz8 >> quiz9 >> exam1 >> exam2 >> exam3 >> exam4
		 >> exam5)
	{	
			//Stores the file information into the struct students
			students.firstName.push_back(first);
			students.lastName.push_back(last);
			students.quizzes[0].push_back(quiz0);
			students.quizzes[1].push_back(quiz1);
			students.quizzes[2].push_back(quiz2);
			students.quizzes[3].push_back(quiz3);
			students.quizzes[4].push_back(quiz4);
			students.quizzes[5].push_back(quiz5);
			students.quizzes[6].push_back(quiz6);
			students.quizzes[7].push_back(quiz7);
			students.quizzes[8].push_back(quiz8);
			students.quizzes[9].push_back(quiz9);
			students.exams[0].push_back(exam1);
			students.exams[1].push_back(exam2);
			students.exams[2].push_back(exam3);
			students.exams[3].push_back(exam4);
			students.exams[4].push_back(exam5);
		
	}
	/*
	//Prints file CGS1060(Just for testing if it works)
	for (int i = 0; i < NUM_STUDENTS; i++) {
		cout << left <<setw(10) << students.firstName[i] << left << setw(10) << students.lastName[i] << "\t " <<
			students.quizzes[0][i] << " " << students.quizzes[1][i] << " " << students.quizzes[2][i] << " " <<
			students.quizzes[3][i] << " " << students.quizzes[4][i] << " " << students.quizzes[5][i] << " " <<
			students.quizzes[6][i] << " " << students.quizzes[7][i] << " " << students.quizzes[8][i] << " " <<
			students.quizzes[9][i] << " " << students.exams[0][i] << " " << students.exams[1][i] << " " <<
			students.exams[2][i] << " " << students.exams[3][i] << " " << students.exams[4][i] << endl;
	}*/
	//Reads topic file line by line
	while (getline(topics,topic)) {
			students.info[0].push_back(topic);
			students.info[1].push_back(topic);
			students.info[2].push_back(topic);
			students.info[3].push_back(topic);
			students.info[4].push_back(topic);
			students.info[5].push_back(topic);
			students.info[6].push_back(topic);
			students.info[7].push_back(topic);
			students.info[8].push_back(topic);
			students.info[9].push_back(topic);
	}
	//Prints file TOPICS(Just for testing if it works)
	/*for (int i = 0; i < NUM_QUIZZES; i++) {
		cout << students.info[i][i] << endl;
	}*/
	
	int userChoice,quiznumber,testnumber; // userChoice will be used  for the menu
	string name;
	do {
		userChoice = menu();
		
		//Checks if the user choose an option between 1 and 6
		while (userChoice <1 || userChoice > 6) {
			cout << "Please enter a valid choice: ";
			cin >> userChoice;
		}

		if (userChoice == 1) {
			cout << "Enter the FIRST or LAST name of the student you want the course grade for " << endl;
			cin >> name;
			choice1(students, name); // requires the struct students and the input name
		}
		else if (userChoice == 2) {
			cout << "Please enter the quiz # you want average from(1-10) ";
			cin >> quiznumber;
			while (quiznumber < 1 || quiznumber >10) { // Won't go forward unless user inputs a correct quiznumber
				cout << "Please enter a valid quiz# ";
				cin >> quiznumber;
			}
			choice2(students, quiznumber); // requires the struct students and the quiz #
		}
		else if (userChoice == 3) {
			cout << "Please enter the test # you want average from(1-5) ";
			cin >> testnumber;
			while (testnumber < 1 || testnumber > 5) { // Won't go forward unless user inputs a correct testnumber
				cout << "Please enter a valid test#: ";
				cin >> testnumber;
			}
			choice3(students, testnumber); // requires the struct students and the test #
		}
		else if (userChoice == 4) {
			choice4(students); // Only requires structure
		}
		else if (userChoice == 5) {
			choice5(students); // Only required structure
		}
		else if (userChoice == 6) {
			choice6(students); // Only required structure
		}

		cout << "Want to continue? (-1 to exit)";
		cin >> userChoice; // Asks again if user wants to exit
		system("cls"); // Clears the screen :)

	} while (userChoice != -1);

	inputFile.close(); // closes file
	topics.close();

	system("pause");
	return 0;
}
