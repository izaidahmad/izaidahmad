//Creating a Todo App program!!

#include <iostream>
#include <vector>
#include <fstream>
#include <string>
using namespace std;

class Task
{
public:
    string description;
    bool isCompleted;

    Task(string desc)
    {
        description = desc;
        isCompleted = false;
    }
};

class ToDoList
{
private:
    vector<Task> tasks;

public:
    // Add new task

    void addTask(string desc)
    {
        tasks.push_back(Task(desc));
        cout << "Task added successfully! " << endl;
    }

    // Mark task as completed

    void markCompleted(int index)
    {
        if (index >= 0 && index < tasks.size())
        {
            tasks[index].isCompleted = true;
            cout << "Task marked as completed! " << endl;
        }
        else
        {
            cout << "Invalid task index!" << endl;
        }
    }

    // Display tasks

    void displayTasks()
    {
        if (tasks.empty())
        {
            cout << "No tasks available." << endl;
            return;
        }
        cout << endl
             << "--- To-Do List " << endl;
        for (int i = 0; i < tasks.size(); i++)
        {
            cout << i + 1 << ". " << tasks[i].description
                 << (tasks[i].isCompleted ? " [is Completed]" : " [Pending]") << endl;
        }
    }

    // Save tasks to file

    void saveToFile(string filename)
    {
        ofstream file(filename);
        for (Task &t : tasks)
        {
            file << t.description << "," << t.isCompleted << endl;
        }
        file.close();
        cout << "Tasks saved successfully! " << endl;
    }

    // Load tasks from file

    void loadFromFile(string filename)
    {
        tasks.clear();
        ifstream file(filename);
        string desc;
        bool completed;
        while (file.good())
        {
            getline(file, desc, ',');
            file >> completed;
            file.ignore();
            if (!desc.empty())
                tasks.push_back(Task(desc)), tasks.back().isCompleted = completed;
        }
        file.close();
        cout << "Tasks loaded successfully! " << endl;
        ;
    }
};

int main()
{
    ToDoList todo;
    int choice;
    string desc;
    int index;

    todo.loadFromFile("tasks.txt"); // Load previous tasks

    do
    {
        cout << " TO-DO LIST MENU " << endl;
        cout << "1. Add Task" << endl;
        cout << "2. Mark Task Completed " << endl;
        cout << "3. View Tasks " << endl;
        cout << "4. Save & Exit" << endl;
        cout << "Enter choice: ";
        cin >> choice;
        cin.ignore();

        switch (choice)
        {
        case 1:
            cout << "Enter task description: ";
            getline(cin, desc);
            todo.addTask(desc);
            break;
        case 2:
            cout << "Enter task number to mark as completed: ";
            cin >> index;
            todo.markCompleted(index - 1);
            break;
        case 3:
            todo.displayTasks();
            break;
        case 4:
            todo.saveToFile("tasks.txt");
            cout << "Exiting..." << endl;
            break;
        default:
            cout << "Invalid choice! " << endl;
        }
    } while (choice != 4);

    return 0;
}
