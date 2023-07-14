# Ejercicio-de-persistencia

import java.io.File;
import java.io.FileWriter;
import java.io.IOException;
import java.util.Scanner;

public class MatrixMultiplier {
    public static void main(String[] args) {
        // Leer las matrices de los archivos de texto
        int[][] matrix1 = readMatrixFromFile("matrix1.txt");
        int[][] matrix2 = readMatrixFromFile("matrix2.txt");

        // Multiplicar las matrices
        int[][] resultMatrix = multiplyMatrices(matrix1, matrix2);

        // Guardar el resultado en un archivo de texto
        writeMatrixToFile(resultMatrix, "result.txt");

        System.out.println("La multiplicación de matrices se ha completado correctamente.");
    }

    public static int[][] readMatrixFromFile(String fileName) {
        try (Scanner scanner = new Scanner(new File(fileName))) {
            int rows = scanner.nextInt();
            int cols = scanner.nextInt();
            int[][] matrix = new int[rows][cols];

            for (int i = 0; i < rows; i++) {
                for (int j = 0; j < cols; j++) {
                    matrix[i][j] = scanner.nextInt();
                }
            }

            return matrix;
        } catch (IOException e) {
            System.err.println("Error al leer el archivo de matriz: " + e.getMessage());
            return null;
        }
    }

    public static void writeMatrixToFile(int[][] matrix, String fileName) {
        try (FileWriter writer = new FileWriter(fileName)) {
            int rows = matrix.length;
            int cols = matrix[0].length;

            writer.write(rows + " " + cols);
            writer.write(System.lineSeparator());

            for (int i = 0; i < rows; i++) {
                for (int j = 0; j < cols; j++) {
                    writer.write(matrix[i][j] + " ");
                }
                writer.write(System.lineSeparator());
            }
        } catch (IOException e) {
            System.err.println("Error al escribir en el archivo de resultado: " + e.getMessage());
        }
    }

    public static int[][] multiplyMatrices(int[][] matrix1, int[][] matrix2) {
        int rows1 = matrix1.length;
        int cols1 = matrix1[0].length;
        int rows2 = matrix2.length;
        int cols2 = matrix2[0].length;

        if (cols1 != rows2) {
            throw new IllegalArgumentException("Las matrices no se pueden multiplicar: las dimensiones no son válidas.");
        }

        int[][] resultMatrix = new int[rows1][cols2];

        for (int i = 0; i < rows1; i++) {
            for (int j = 0; j < cols2; j++) {
                int sum = 0;
                for (int k = 0; k < cols1; k++) {
                    sum += matrix1[i][k] * matrix2[k][j];
                }
                resultMatrix[i][j] = sum;
            }
        }

        return resultMatrix;
    }
}

import org.junit.jupiter.api.Assertions;
import org.junit.jupiter.api.Test;

public class MatrixMultiplierTest {
    @Test
    public void testMultiplyMatrices() {
        int[][] matrix1 = {{1, 2}, {3, 4}};
        int[][] matrix2 = {{5, 6}, {7, 8}};
        int[][] expected = {{19, 22}, {43, 50}};
        
        int[][] result = MatrixMultiplier.multiplyMatrices(matrix1, matrix2);

        Assertions.assertArrayEquals(expected, result);
    }
}
