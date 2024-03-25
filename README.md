#include <iostream>
#include <vector>
#include <algorithm>
// Функція для обчислення G
double calculateG(const std::vector<int>& g0, const std::vector<int>& g1, const std::vector<int>& g2, const std::vector<int>& g3, const std::vector<int>& g4, const std::vector<int>& g5, const std::vector<int>& g6) {
    double G = 0.0;
    int size = g0.size(); // Розмір вектора g0 (однаковий для всіх g векторів)
    for (int j = 1; j < size; ++j) {
        double maxGj = std::max({static_cast<double>(g0[j]), static_cast<double>(g1[j]), static_cast<double>(g2[j]), static_cast<double>(g3[j]), static_cast<double>(g4[j]), static_cast<double>(g5[j]), static_cast<double>(g6[j])});
        double minGj_1 = std::min({static_cast<double>(g0[j - 1]), static_cast<double>(g1[j - 1]), static_cast<double>(g2[j - 1]), static_cast<double>(g3[j - 1]), static_cast<double>(g4[j - 1]), static_cast<double>(g5[j - 1]), static_cast<double>(g6[j - 1])});
        G += maxGj - minGj_1;
    }
    return G;
}
int main() {
    int k;
    std::cout << "Введіть ціле число k (k > 5): ";
    std::cin >> k;
    int N = 15 * k;
    // Оголошення та ініціалізація вектора v
    std::vector<int> v(N);
    // Отримання вхідних даних (елементів вектора v)
    std::cout << "Введіть " << N << " чисел для вектора v:\n";
    for (int i = 0; i < N; ++i) {
        std::cin >> v[i];
    }
    // Побудова векторів g0, g1, g2, g3
    std::vector<int> g0(v.begin() + 2, v.begin() + 9 * k + 2);
    std::vector<int> g1, g2, g3;
    for (int j = 0; j < 10; j += 3) {
        for (int i = j; i <= j + 3 * k; i += 3) {
            g1.push_back(v[i]);
            g1.push_back(v[i + 3]);
            g2.push_back(v[10 + j]);
            g2.push_back(v[10 + j + 3]);
            g3.push_back(v[20 + j]);
            g3.push_back(v[20 + j + 3]);
        }
    }
    // Побудова векторів g4, g5, g6
    std::vector<int> g4(g0.size());
    std::vector<int> g5(g0.size());
    std::vector<int> g6(g0.size());
    std::transform(g0.begin(), g0.end(), g1.begin(), g4.begin(), std::plus<int>());
    std::transform(g3.begin(), g3.end(), g2.begin(), g5.begin(), std::minus<int>());
    std::transform(g0.begin(), g0.end(), g6.begin(), [k](int value) { return value / k; });
    // Обчислення значення G
    double G = calculateG(g0, g1, g2, g3, g4, g5, g6);
    // Виведення результату
    std::cout << "Результат обчислення G: " << G << std::endl;
    return 0;
}
