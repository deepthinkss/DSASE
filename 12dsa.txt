#include <iostream>
#include <fstream>

using namespace std;

struct Record {
    int id;
    string name;
    string email;
};

void insertRecord(const Record& record) {
    ofstream file("records.dat", ios::binary | ios::app);
    if (!file) {
        cerr << "Error opening file." << endl;
        return;
    }

    file.write(reinterpret_cast<const char*>(&record), sizeof(record));
    file.close();

    cout << "Record inserted successfully." << endl;
}

void deleteRecord(int id) {
    ifstream file("records.dat", ios::binary);
    if (!file) {
        cerr << "Error opening file." << endl;
        return;
    }

    ofstream tempFile("temp.dat", ios::binary);
    if (!tempFile) {
        cerr << "Error creating temporary file." << endl;
        file.close();
        return;
    }

    Record record;
    bool found = false;
    while (file.read(reinterpret_cast<char*>(&record), sizeof(record))) {
        if (record.id != id) {
            tempFile.write(reinterpret_cast<const char*>(&record), sizeof(record));
        } else {
            found = true;
        }
    }

    file.close();
    tempFile.close();

    if (found) {
        remove("records.dat");
        rename("temp.dat", "records.dat");
        cout << "Record deleted successfully." << endl;
    } else {
        remove("temp.dat");
        cout << "Record not found." << endl;
    }
}

int main() {
    // Inserting a record
    Record newRecord;
    newRecord.id = 1;
    newRecord.name = "John Doe";
    newRecord.email = "john.doe@example.com";
    insertRecord(newRecord);

    // Deleting a record
    int recordIdToDelete = 1;
    deleteRecord(recordIdToDelete);

    return 0;
}