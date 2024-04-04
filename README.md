#include <iostream>
#include <fstream>
#include <cmath>
#include <vector>
#include <stdexcept>
// Функція для зчитування даних з файлу та побудови таблиці
std::vector<std::vector<double>> readTableFromFile(const std::string& filename) {
    std::ifstream file(filename);
    if (!file.is_open()) {
        throw std::runtime_error("Помилка відкриття файлу " + filename);
    }
    std::vector<std::vector<double>> table;
    double x, T, U;
    std::string text;
    while (file >> x >> U >> T >> text) {
        table.push_back({x, U, T});
    }
    return table;
}
// Функція для обчислення максимального значення серед параметрів
double computeMax(const std::vector<double>& params) {
    double maxVal = params[0];
    for (size_t i = 1; i < params.size(); ++i) {
        if (params[i] > maxVal) {
            maxVal = params[i];
        }
    }
    return maxVal;
}
// Алгоритм 1
double algorithm1(double k, double r) {
    return (k * r + 10270.) * (k * r + 89730.);
}
// Алгоритм 2
double algorithm2(double x, double y, double z) {
    return (x * y - x * z * y + x * y * y - x * y * y * 0.54 + x * y * y * 0.46);
}
// Алгоритм 3
double algorithm3(double x, double y, double z) {
    return (x * y - 1.5 * x * z * y + 251. * x * y * y - 751. * x * y * y);
}
int main() {
    double x, y, z;
    std::string text;
    std::cout << "Введіть значення x, y, z та текст: ";
    std::cin >> x >> y >> z >> text;
    try {
        std::vector<std::vector<double>> table4 = readTableFromFile("dat1.dat");
        std::vector<std::vector<double>> table5 = readTableFromFile("dat2.dat");
        std::vector<std::vector<double>> table6 = readTableFromFile("dat3.dat");
        double result;
        if (text.empty()) {
            result = computeMax({x, y, z});
        } else {
            result = std::stod(text);
        }

        if (x <= 5) {
            result = algorithm2(x, y, z);
        } else if (x > 5 && x <= 10) {
            result = algorithm3(x, y, z);
        }
        std::cout << "Результат обчислення Variant(r, k): " << result << std::endl;
    } catch (const std::runtime_error& e) {
        std::cerr << "Помилка: " << e.what() << std::endl;
    }
    return 0;
}
