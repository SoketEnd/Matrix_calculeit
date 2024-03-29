//# Matrix_calculeit
#include <iostream>
#include <vector>
#include <iomanip>
#include <locale>

class Matrix_Calkuleit {
public:
    std::vector<int> findMatrix_vector(const std::vector<std::vector<int>>& matrix, const std::vector<int>& vectorB)
    {
        int row = matrix.size(); 
        int col = matrix[0].size(); 
        std::vector<int> res(row);

        if (col != vectorB.size()) 
        {
            throw std::invalid_argument("Матрица и вектор несовместимы для умножения");
        }
        else
        {
            for (int i = 0; i < row; ++i)
            {
                int comp = 0;
                for (int j = 0; j < col; ++j)
                {
                    comp += matrix[i][j] * vectorB[j];
                }
                res[i] = comp;
            }
            return res;
        }
    }

    std::vector<std::vector<int>> Multi_on_num(const std::vector<std::vector<int>>& matrix, int num)
    {
        int row = matrix.size();
        int col = matrix[0].size();
        std::vector<std::vector<int>> res(row, std::vector<int>(col));

        for (int i = 0; i < row; ++i)
        {
            for (int j = 0; j < col; ++j)
            {
                res[i][j] = matrix[i][j] * num;
            }
        }
        return res;
    }
 
    std::vector<std::vector<int>> findsum(const std::vector<std::vector<int>>& matrixA, const std::vector<std::vector<int>>& matrixB)
    {
        int rowA = matrixA.size();
        int colA = matrixA[0].size();
        int rowB = matrixB.size();
        int colB = matrixB[0].size();

        if (rowA != rowB || colA != colB)
        {
            throw std::invalid_argument("Матрицы разного размера");
        }

        std::vector<std::vector<int>> res(rowA, std::vector<int>(colA));

        for (int i = 0; i < rowA; ++i)
        {
            for (int j = 0; j < colA; ++j)
            {
                res[i][j] = matrixA[i][j] + matrixB[i][j];
            }
        }
        return res;
    }

    std::vector<std::vector<int>> findDifference(const std::vector<std::vector<int>>& matrixA, const std::vector<std::vector<int>>& matrixB)
    {
        int rowA = matrixA.size();
        int colA = matrixA[0].size();
        int rowB = matrixB.size();
        int colB = matrixB[0].size();

        if (rowA != rowB || colA != colB)
        {
            throw std::invalid_argument("Матрицы разного размера");
        }

        std::vector<std::vector<int>> res(rowA, std::vector<int>(colA));

        for (int i = 0; i < rowA; ++i)
        {
            for (int j = 0; j < colA; ++j)
            {
                res[i][j] = matrixA[i][j] - matrixB[i][j];
            }
        }
        return res;
    }

    std::vector<std::vector<int>> findmultiplication(const std::vector<std::vector<int>>& matrixA, const std::vector<std::vector<int>>& matrixB)
    {
        int rowA = matrixA.size();
        int colA = matrixA[0].size();
        int rowB = matrixB.size();
        int colB = matrixB.size();
        if (colA != rowB)
        {
            throw std::invalid_argument("Неправильные размеры матриц для умножения");
        }

        std::vector<std::vector<int>> res(rowA, std::vector<int>(colB, 0));

        for (int i = 0; i < rowA; ++i)
        {
            for (int j = 0; j < colB; ++j)
            {
                for (int k = 0; k < colB; ++k)
                {
                    res[i][j] += matrixA[i][k] * matrixB[k][j];
                }
            }
        }
        return res;
    }

    int determinant(const std::vector<std::vector<int>>& matrix) {
        int n = matrix.size();
        if (n != matrix[0].size()) {
            throw std::invalid_argument("Матрица не является квадратной");
        }

        if (n == 1) {
            return matrix[0][0];
        }

        int det = 0;
        std::vector<std::vector<int>> submatrix(n - 1, std::vector<int>(n - 1));
        for (int k = 0; k < n; ++k) {
            int i_sub = 0;
            for (int i = 1; i < n; ++i) {
                int j_sub = 0;
                for (int j = 0; j < n; ++j) {
                    if (j == k) continue;
                    submatrix[i_sub][j_sub] = matrix[i][j];
                    ++j_sub;
                }
                ++i_sub;
            }
            det += pow(-1, k) * matrix[0][k] * determinant(submatrix);
        }
        return det;
    }

};

int main()
{
    setlocale(LC_ALL, "RUSSIAN");
    Matrix_Calkuleit in;
    int row;
    int col;
    int choice;
    std::cout << "Выберите операцию: " << std::endl;
    std::cout << "1. Умножение матрицы на вектор и получение Максимума " << std::endl;
    std::cout << "2. Сложение двух матриц" << std::endl;
    std::cout << "3. Вычитание одной матрицы из другой" << std::endl;
    std::cout << "4. Перемножение матриц" << std::endl;
    std::cout << "5. Нахождение определителя матрицы" << std::endl;
    std::cout << "6. Произведение числа на матрицу" << std::endl;
    std::cin >> choice;

    std::cout << "Введите количество строк: ";
    std::cin >> row;
    std::cout << "Введите количество столбцов: ";
    std::cin >> col;

    std::vector<std::vector<int>> matrix(row, std::vector<int>(col));
    std::vector<std::vector<int>> matrixB(row, std::vector<int>(col));
    std::vector<int> vectorb(col);

    switch (choice)
    {
    case 1:
        try {
            std::cout << "Создайте матрицу:" << std::endl;
            for (int i = 0; i < row; ++i)
            {
                for (int j = 0; j < col; ++j)
                {
                    std::cout << "Введите элемент матрицы [" << i << "][" << j << "]: ";
                    std::cin >> matrix[i][j];
                }
            }

            std::cout << "Введите вектор размера " << col << ":" << std::endl;
            for (int i = 0; i < col; ++i)
            {
                std::cout << "Введите элемент вектора [" << i << "]: ";
                std::cin >> vectorb[i];
            }
            std::vector<int> MatrixVector = in.findMatrix_vector(matrix, vectorb);
            std::cout << "Результат умножения матрицы на вектор:" << std::endl;
            for (int i = 0; i < MatrixVector.size(); ++i)
            {
                std::cout << std::setw(4) << MatrixVector[i] << " ";
            }
            std::cout << std::endl;
        }
        catch (const std::invalid_argument& e) {
            std::cerr << e.what() << std::endl;
        }
        break;

    case 2:
        try {
            std::cout << "Нахождение суммы матриц:" << std::endl;

            std::cout << "Создайте первую матрицу:" << std::endl;
            for (int i = 0; i < row; ++i)
            {
                for (int j = 0; j < col; ++j)
                {
                    std::cout << "Введите элемент матрицы A[" << i << "][" << j << "]: ";
                    std::cin >> matrix[i][j];
                }
            }

            std::cout << "Создайте вторую матрицу:" << std::endl;
            for (int i = 0; i < row; ++i)
            {
                for (int j = 0; j < col; ++j)
                {
                    std::cout << "Введите элемент матрицы B[" << i << "][" << j << "]: ";
                    std::cin >> matrixB[i][j];
                }
            }
            std::vector<std::vector<int>> SumMatrix = in.findsum(matrix, matrixB);
            std::cout << "Результат сложения матриц " << std::endl;
            for (int i = 0; i < row; ++i)
            {
                for (int j = 0; j < col; ++j)
                {
                    std::cout << std::setw(4) << SumMatrix[i][j] << " ";
                }
                std::cout << std::endl;
            }
        }
        catch (const std::invalid_argument& e) {
            std::cerr << e.what() << std::endl;
        }
        break;
    case 3:
        try {
            std::cout << "Нахождение разности матриц:" << std::endl;

            std::cout << "Создайте первую матрицу:" << std::endl;
            for (int i = 0; i < row; ++i)
            {
                for (int j = 0; j < col; ++j)
                {
                    std::cout << "Введите элемент матрицы A[" << i << "][" << j << "]: ";
                    std::cin >> matrix[i][j];
                }
            }
            std::cout << "Создайте первую матрицу:" << std::endl;
            for (int i = 0; i < row; ++i)
            {
                for (int j = 0; j < col; ++j)
                {
                    std::cout << "Введите элемент матрицы B[" << i << "][" << j << "]: ";
                    std::cin >> matrixB[i][j];
                }
            }

            std::vector<std::vector<int>> DifMatrix = in.findDifference(matrix, matrixB);
            std::cout << "Результат сложения матриц " << std::endl;
            for (int i = 0; i < row; ++i)
            {
                for (int j = 0; j < col; ++j)
                {
                    std::cout << std::setw(4) << DifMatrix[i][j] << " ";
                }
                std::cout << std::endl;
            }

        }
        catch (const std::invalid_argument& e) {
            std::cerr << e.what() << std::endl;
        }
        break;
    case 4:
        try {
            std::cout << "Нахождение произведения матриц:" << std::endl;

            std::cout << "Создайте первую матрицу:" << std::endl;
            for (int i = 0; i < row; ++i)
            {
                for (int j = 0; j < col; ++j)
                {
                    std::cout << "Введите элемент матрицы A[" << i << "][" << j << "]: ";
                    std::cin >> matrix[i][j];
                }
            }
            std::cout << "Создайте первую матрицу:" << std::endl;
            for (int i = 0; i < row; ++i)
            {
                for (int j = 0; j < col; ++j)
                {
                    std::cout << "Введите элемент матрицы B[" << i << "][" << j << "]: ";
                    std::cin >> matrixB[i][j];
                }
            }

            std::vector<std::vector<int>> MultyMatrix = in.findmultiplication(matrix, matrixB);
            std::cout << "Результат произведения матриц " << std::endl;
            for (int i = 0; i < row; ++i)
            {
                for (int j = 0; j < col; ++j)
                {
                    std::cout << std::setw(4) << MultyMatrix[i][j] << " ";
                }
                std::cout << std::endl;
            }
        }
        catch (const std::invalid_argument& e) {
            std::cerr << e.what() << std::endl;
        }
        break;
    case 5:
        try
        {
            std::cout << "Нахождение определителя матрицы " << std::endl;
            std::cout << "Создайте матрицу:" << std::endl;
            for (int i = 0; i < row; ++i)
            {
                for (int j = 0; j < col; ++j)
                {
                    std::cout << "Введите элемент матрицы [" << i + 1 << "][" << j + 1 << "]: ";
                    std::cin >> matrix[i][j];
                }
            }
            
            int deter = in.determinant(matrix);

            std::cout << "Определитель матрицы = " << deter;
        }
        catch (const std::invalid_argument& e) {
            std::cerr << e.what() << std::endl;
        }
    case 6:
        try
        {
            std::cout << "Нахождение произведения матрицы на число " << std::endl;

            std::cout << "Создайте матрицу:" << std::endl;
            for (int i = 0; i < row; ++i)
            {
                for (int j = 0; j < col; ++j)
                {
                    std::cout << "Введите элемент матрицы [" << i + 1 << "][" << j + 1 << "]: ";
                    std::cin >> matrix[i][j];
                }
            }
            int num;
            std::cout << "Введите чило на котрое хотите умножить: " << std::endl;;
            std::cin >> num;

            std::vector<std::vector<int>> numMatrix = in.Multi_on_num(matrix, num);

            std::cout << "Результат умнажения числа на матрицу " << std::endl;
            for (int i = 0; i < row; ++i)
            {
                for (int j = 0; j < col; ++j)
                {
                    std::cout << std::setw(4) << numMatrix[i][j] << " ";
                }
                std::cout << std::endl;
            }
        }
        catch(const std::invalid_argument& e) {
            std::cerr << e.what() << std::endl;
        }
    }
    return 0;
}
