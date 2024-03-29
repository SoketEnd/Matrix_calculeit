//# Matrix_calculeit
#include <iostream>
#include <vector>
#include <iomanip>
#include <locale>
#include <cmath>

// Class for performing matrix calculations
class Matrix_Calculator {
public:
    // Method to find the product of a matrix and a vector
    std::vector<int> findMatrix_vector(const std::vector<std::vector<int>>& matrix, const std::vector<int>& vectorB)
    {
        int row = matrix.size(); 
        int col = matrix[0].size(); 
        std::vector<int> res(row);

        if (col != vectorB.size()) 
        {
            throw std::invalid_argument("Matrix and vector are incompatible for multiplication");
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

    // Method to multiply a matrix by a scalar
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
 
    // Method to find the sum of two matrices
    std::vector<std::vector<int>> findsum(const std::vector<std::vector<int>>& matrixA, const std::vector<std::vector<int>>& matrixB)
    {
        int rowA = matrixA.size();
        int colA = matrixA[0].size();
        int rowB = matrixB.size();
        int colB = matrixB[0].size();

        if (rowA != rowB || colA != colB)
        {
            throw std::invalid_argument("Matrices have different sizes");
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

    // Method to find the difference of two matrices
    std::vector<std::vector<int>> findDifference(const std::vector<std::vector<int>>& matrixA, const std::vector<std::vector<int>>& matrixB)
    {
        int rowA = matrixA.size();
        int colA = matrixA[0].size();
        int rowB = matrixB.size();
        int colB = matrixB[0].size();

        if (rowA != rowB || colA != colB)
        {
            throw std::invalid_argument("Matrices have different sizes");
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

    // Method to find the product of two matrices
    std::vector<std::vector<int>> findmultiplication(const std::vector<std::vector<int>>& matrixA, const std::vector<std::vector<int>>& matrixB)
    {
        int rowA = matrixA.size();
        int colA = matrixA[0].size();
        int rowB = matrixB.size();
        int colB = matrixB.size();
        if (colA != rowB)
        {
            throw std::invalid_argument("Incorrect matrix sizes for multiplication");
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

    // Method to transpose a matrix
    std::vector<std::vector<int>> Tranponeyt(const std::vector<std::vector<int>>& matrixA)
    {
        int row = matrixA.size();
        int col = matrixA[0].size();
        std::vector<std::vector<int>> res(row,std::vector<int>(col));
        for (int i = 0; i < row; ++i)
        {
            for (int j = 0; j < col; ++j)
            {
                res[j][i] = matrixA[i][j];
            }
        }
        return res;
    }

    // Method to find the inverse of a matrix
    std::vector<std::vector<double>> inverseMatrix(const std::vector<std::vector<double>>& matrix) {
        int n = matrix.size();

        // Create an augmented matrix containing the original matrix and the identity matrix
        std::vector<std::vector<double>> augmentedMatrix(n, std::vector<double>(2 * n, 0.0));
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < n; ++j) {
                augmentedMatrix[i][j] = matrix[i][j];
            }
            augmentedMatrix[i][i + n] = 1.0;
        }

        // Gaussian elimination to reduce the left part to diagonal form
        for (int i = 0; i < n; ++i) {
            // Divide the current row by the diagonal element
            double divisor = augmentedMatrix[i][i];
            for (int j = 0; j < 2 * n; ++j) {
                augmentedMatrix[i][j] /= divisor;
            }

            // Subtract the current row from the other rows to make elements below the diagonal zero
            for (int k = 0; k < n; ++k) {
                if (k != i) {
                    double multiplier = augmentedMatrix[k][i];
                    for (int j = 0; j < 2 * n; ++j) {
                        augmentedMatrix[k][j] -= multiplier * augmentedMatrix[i][j];
                    }
                }
            }
        }

        // Extract the inverse matrix from the reduced augmented matrix
        std::vector<std::vector<double>> inverse(n, std::vector<double>(n, 0.0));
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < n; ++j) {
                inverse[i][j] = augmentedMatrix[i][j + n];
            }
        }

        return inverse;
    }

    // Method to find the determinant of a matrix
    int determinant(const std::vector<std::vector<int>>& matrix) {
        int n = matrix.size();
        if (n != matrix[0].size()) {
            throw std::invalid_argument("Matrix is not square");
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

    // Method to perform LU factorization of a matrix
    std::pair<std::vector<std::vector<double>>, std::vector<std::vector<double>>> factorize(const std::vector<std::vector<int>>& matrix) {
        int n = matrix.size();

        std::vector<std::vector<double>> L(n, std::vector<double>(n, 0.0));
        std::vector<std::vector<double>> U(n, std::vector<double>(n, 0.0));

        for (int i = 0; i < n; ++i) {
            for (int k = i; k < n; ++k) {
                double sum = 0.0;
                for (int j = 0; j < i; ++j)
                    sum += (L[i][j] * U[j][k]);
                U[i][k] = matrix[i][k] - sum;
            }

            for (int k = i; k < n; ++k) {
                if (i == k)
                    L[i][i] = 1;
                else {
                    double sum = 0.0;
                    for (int j = 0; j < i; ++j)
                        sum += (L[k][j] * U[j][i]);
                    L[k][i] = (matrix[k][i] - sum) / U[i][i];
                }
            }
        }

        return std::make_pair(L, U);
    }
};

// Class for handling matrix input and output
class InputMatrix
{
public:
    // Method to print a matrix
    void printMatrix(const std::vector<std::vector<int>>& matrix) {
        int rows = matrix.size();
        int cols = matrix[0].size();

        for (int i = 0; i < rows; ++i) {
            for (int j = 0; j < cols; ++j) {
                std::cout << std::setw(8) << std::fixed << std::setprecision(2) << matrix[i][j] << " ";
            }
            std::cout << std::endl;
        }
    }
};

int main()
{
    setlocale(LC_ALL, "RUSSIAN");
    Matrix_Calculator in;
    InputMatrix ni;
    int row;
    int col;
    int choice;
    std::cout << "Choose an operation: " << std::endl;
    std::cout << "1. Matrix-vector multiplication and Maximum" << std::endl;
    std::cout << "2. Summation of two matrices" << std::endl;
    std::cout << "3. Subtraction of one matrix from another" << std::endl;
    std::cout << "4. Matrix multiplication" << std::endl;
    std::cout << "5. Determinant of a matrix" << std::endl;
    std::cout << "6. Product of a number by a matrix" << std::endl;
    std::cout << "7. Finding the inverse of a matrix" << std::endl;
    std::cout << "8. LU-factorization method" << std::endl;
    std::cout << "9. Transposition" << std::endl;
    std::cin >> choice;

    std::cout << "Enter the number of rows: ";
    std::cin >> row;
    std::cout << "Enter the number of columns: ";
    std::cin >> col;

    std::vector<std::vector<int>> matrix(row, std::vector<int>(col));
    std::vector<std::vector<double>> matrixA(row, std::vector<double>(col));
    std::vector<std::vector<int>> matrixB(row, std::vector<int>(col));
    std::vector<int> vectorb(col);

    switch (choice)
    {
    case 1:
        try {
            std::cout << "Create a matrix:" << std::endl;
            for (int i = 0; i < row; ++i)
            {
                for (int j = 0; j < col; ++j)
                {
                    std::cout << "Enter the element of the matrix [" << i << "][" << j << "]: ";
                    std::cin >> matrix[i][j];
                }
            }

            std::cout << "Enter a vector of size " << col << ":" << std::endl;
            for (int i = 0; i < col; ++i)
            {
                std::cout << "Enter the element of the vector [" << i << "]: ";
                std::cin >> vectorb[i];
            }
            std::vector<int> MatrixVector = in.findMatrix_vector(matrix, vectorb);
            std::cout << "Result of matrix-vector multiplication:" << std::endl;
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
            std::cout << "Finding the sum of matrices:" << std::endl;

            std::cout << "Create the first matrix:" << std::endl;
            for (int i = 0; i < row; ++i)
            {
                for (int j = 0; j < col; ++j)
                {
                    std::cout << "Enter the element of matrix A[" << i << "][" << j << "]: ";
                    std::cin >> matrix[i][j];
                }
            }

            std::cout << "Create the second matrix:" << std::endl;
            for (int i = 0; i < row; ++i)
            {
                for (int j = 0; j < col; ++j)
                {
                    std::cout << "Enter the element of matrix B[" << i << "][" << j << "]: ";
                    std::cin >> matrixB[i][j];
                }
            }
            
            std::vector<std::vector<int>> SumMatrix = in.findsum(matrix, matrixB);
            
            std::cout << "Result of matrix summation:" << std::endl;
            ni.printMatrix(SumMatrix);
        }
        catch (const std::invalid_argument& e) {
            std::cerr << e.what() << std::endl;
        }
        break;
    case 3:
        try {
            std::cout << "Finding the difference of matrices:" << std::endl;

            std::cout << "Create the first matrix:" << std::endl;
            for (int i = 0; i < row; ++i)
            {
                for (int j = 0; j < col; ++j)
                {
                    std::cout << "Enter the element of matrix A[" << i << "][" << j << "]: ";
                    std::cin >> matrix[i][j];
                }
            }
            std::cout << "Create the second matrix:" << std::endl;
            for (int i = 0; i < row; ++i)
            {
                for (int j = 0; j < col; ++j)
                {
                    std::cout << "Enter the element of matrix B[" << i << "][" << j << "]: ";
                    std::cin >> matrixB[i][j];
                }
            }

            std::vector<std::vector<int>> DifMatrix = in.findDifference(matrix, matrixB);
            std::cout << "Result of matrix subtraction:" << std::endl;
            ni.printMatrix(DifMatrix);
        }
        catch (const std::invalid_argument& e) {
            std::cerr << e.what() << std::endl;
        }
        break;
    case 4:
        try {
            std::cout << "Finding the product of matrices:" << std::endl;

            std::cout << "Create the first matrix:" << std::endl;
            for (int i = 0; i < row; ++i)
            {
                for (int j = 0; j < col; ++j)
                {
                    std::cout << "Enter the element of matrix A[" << i << "][" << j << "]: ";
                    std::cin >> matrix[i][j];
                }
            }
            std::cout << "Create the second matrix:" << std::endl;
            for (int i = 0; i < row; ++i)
            {
                for (int j = 0; j < col; ++j)
                {
                    std::cout << "Enter the element of matrix B[" << i << "][" << j << "]: ";
                    std::cin >> matrixB[i][j];
                }
            }

            std::vector<std::vector<int>> MultyMatrix = in.findmultiplication(matrix, matrixB);
            std::cout << "Result of matrix multiplication:" << std::endl;
            ni.printMatrix(MultyMatrix);
        }
        catch (const std::invalid_argument& e) {
            std::cerr << e.what() << std::endl;
        }
        break;
    case 5:
        try
        {
            std::cout << "Finding the determinant of a matrix" << std::endl;
            std::cout << "Create a matrix:" << std::endl;
            for (int i = 0; i < row; ++i)
            {
                for (int j = 0; j < col; ++j)
                {
                    std::cout << "Enter the element of the matrix [" << i + 1 << "][" << j + 1 << "]: ";
                    std::cin >> matrix[i][j];
                }
            }
            
            int deter = in.determinant(matrix);

            std::cout << "Determinant of the matrix = " << deter;
        }
        catch (const std::invalid_argument& e) {
            std::cerr << e.what() << std::endl;
        }
        break;
    case 6:
        try
        {
            std::cout << "Finding the product of a matrix by a number" << std::endl;

            std::cout << "Create a matrix:" << std::endl;
            for (int i = 0; i < row; ++i)
            {
                for (int j = 0; j < col; ++j)
                {
                    std::cout << "Enter the element of the matrix [" << i + 1 << "][" << j + 1 << "]: ";
                    std::cin >> matrix[i][j];
                }
            }
            int num;
            std::cout << "Enter the number you want to multiply by: " << std::endl;;
            std::cin >> num;

            std::vector<std::vector<int>> numMatrix = in.Multi_on_num(matrix, num);

            std::cout << "Result of multiplying a number by a matrix:" << std::endl;
            ni.printMatrix(numMatrix);
        }
        catch(const std::invalid_argument& e) {
            std::cerr << e.what() << std::endl;
        }
        break;
    case 7:
        try
        {
            std::cout << "Finding the inverse of a matrix" << std::endl;

            std::cout << "Create a matrix" << std::endl;

            for (int i = 0; i < row; ++i)
            {
                for (int j = 0; j < col; ++j)
                {
                    std::cout << "Enter the element of the matrix [" << i + 1 << "][" << j + 1 << "]: ";
                    std::cin >> matrixA[i][j];
                }
            }

            std::vector<std::vector<double>> InversMatrix = in.inverseMatrix(matrixA);

            std::cout << "Result of finding the inverse matrix" << std::endl;

            for (int i = 0; i < row; ++i)
            {
                for (int j = 0; j < col; ++j)
                {
                    std::cout << std::setw(8) << std::fixed << std::setprecision(2) << InversMatrix[i][j] << " ";
                }
                std::cout << std::endl;
            }

        }
        catch (const std::invalid_argument& e) {
            std::cerr << e.what() << std::endl;
        }
        break;
    case 8:
        try
        {
            std::cout << "LU factorization of the matrix" << std::endl;

            std::cout << "Create a matrix" << std::endl;

            for (int i = 0; i < row; ++i)
            {
                for (int j = 0; j < col; ++j)
                {
                    std::cout << "Enter the element of the matrix [" << i + 1 << "][" << j + 1 << "]: ";
                    std::cin >> matrixA[i][j];
                }
            }

            std::pair<std::vector<std::vector<double>>, std::vector<std::vector<double>>> LU = in.factorize(matrixA);

            std::cout << "Result of LU factorization:" << std::endl;

            std::cout << "Lower triangular matrix (L):" << std::endl;
            ni.printMatrix(LU.first);

            std::cout << "Upper triangular matrix (U):" << std::endl;
            ni.printMatrix(LU.second);

        }
        catch (const std::invalid_argument& e) {
            std::cerr << e.what() << std::endl;
        }
        break;
    case 9:
        try
        {
            std::cout << "Matrix transposition" << std::endl;

            std::cout << "Create a matrix" << std::endl;

            for (int i = 0; i < row; ++i)
            {
                for (int j = 0; j < col; ++j)
                {
                    std::cout << "Enter the element of the matrix [" << i + 1 << "][" << j + 1 << "]: ";
                    std::cin >> matrixA[i][j];
                }
            }

            std::vector<std::vector<int>> TrasMatrix = in.Tranponeyt(matrixA);

            std::cout << "Result of matrix transposition:" << std::endl;

            ni.printMatrix(TrasMatrix);

        }
        catch (const std::invalid_argument& e) {
            std::cerr << e.what() << std::endl;
        }
        break;

    default:
        std::cout << "Invalid choice" << std::endl;
    }

    return 0;
}

