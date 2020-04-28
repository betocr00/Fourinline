# Fourinline
Four in line


package app;

import java.io.*; // Libreria basica de entrada y salida
import java.util.Scanner;

public class cuatroenlinea { // Inicio del programa

  // Variables globales de entrada y salida
  public static BufferedReader in = new BufferedReader(new InputStreamReader(System.in));
  public static PrintStream out = System.out;
  public static Scanner teclado = new Scanner(System.in);

  public static void main(String[] args) throws IOException { // Inicio del main

    jugar();

  }

  public static void jugar() {

    char J1 = 'X';
    char J2 = 'O';
    char vacio = '-';
    boolean turno = true;
    char tablero[][] = new char[6][7];
    int fila, columna;
    boolean posValida, correcto;

    llenarMatriz(tablero, vacio);

    while (!finPartida(tablero, vacio)) {

      do {
        mostratTurnoActual(turno);
        mostratMatriz(tablero);
        correcto = false;
        fila = pedirInt("Coloca la fila");
        columna = pedirInt("Coloca la columna");

        posValida = validarPosicion(tablero, fila, columna);

        if (posValida) {

          if (!hayValor(tablero, fila, columna, vacio)) {

            correcto = true;

          } else {
            out.println("Ya hay una marca en esa posicion");
          }

        } else {
          out.println("La posicion no es valida");
        }

      } while (!correcto);

      if (turno) {
        insetarEn(tablero, fila, columna, J1);
      } else {
        insetarEn(tablero, fila, columna, J2);
      }

      turno = !turno;
    }
    mostratMatriz(tablero);
    mostrarGanador(tablero, J1, J2, vacio);
  }

  public static void mostratTurnoActual(boolean turno) {
    if (turno) {
      out.println("Le toca al jugador 1");
    } else {
      out.println("Le toca al jugador 2");
    }

  }

  public static int pedirInt(String mensaje) {

    out.println(mensaje);
    int numero = teclado.nextInt();
    return numero;

  }

  public static void llenarMatriz(char matriz[][], char simbolo) {
    for (int i = 0; i < matriz.length; i++) {

      for (int a = 0; a < matriz.length; a++) {
        matriz[i][a] = simbolo;
      }
    }

  }

  public static void mostratMatriz(char matriz[][]) {

    for (int i = 0; i < matriz.length; i++) {

      for (int a = 0; a < matriz[0].length; a++) {
        out.print(matriz[i][a] + " ");
      }
      out.println("");
    }

  }

  public static boolean validarPosicion(char matriz[][], int fila, int columna) {

    if ((fila >= 0) && (fila < matriz.length) && (columna >= 0) && (columna < matriz.length)) {
      return true;
    }

    return false;

  }

  public static boolean hayValor(char matriz[][], int fila, int columna, char simbolo) {

    if (matriz[fila][columna] != simbolo) {
      return true;
    }
    return false;
  }


  public static void insetarEn(char matriz[][], int fila, int columna, char simbolo) {

    matriz[fila][columna] = simbolo;

  }

  public static boolean matrizLlena(char matriz[][], char simboloDef) {
    for (int i = 0; i < matriz.length; i++) {

      for (int j = 0; j < matriz[0].length; j++) {
        if (matriz[i][j] == simboloDef) {
          return false;
        }
      }
    }
    return true;
  }

  public static char coincidenciaLinea(char matriz[][], char simboloDef) {

    char simbolo;
    boolean coincidencia;

    for (int i = 0; i < matriz.length; i++) {
      coincidencia = true;
      simbolo = matriz[i][0];
      if (simbolo != simboloDef) {
        for (int j = 1; j < matriz[0].length; j++) {
          if (simbolo != matriz[i][j]) {
            coincidencia = false;
          }
        }
        if (coincidencia) {
          return simbolo;
        }
      }
    }
    return simboloDef;
  }

  public static char coincidenciaColumna(char matriz[][], char simboloDef) {

    char simbolo;
    boolean coincidencia;

    for (int j = 0; j < matriz.length; j++) {
      coincidencia = true;
      simbolo = matriz[0][j];
      if (simbolo != simboloDef) {
        for (int i = 1; i < matriz[0].length; i++) {
          if (simbolo != matriz[i][j]) {
            coincidencia = false;
          }
        }
        if (coincidencia) {
          return simbolo;
        }
      }
    }
    return simboloDef;
  }

  public static char coincidenciaDiagonal(char matriz[][], char simboloDef) {

    char simbolo;
    boolean coincidencia = true;

    // Diagonal principal
    simbolo = matriz[0][0];
    if (simbolo != simboloDef) {
      for (int i = 1; i < matriz.length; i++) {
        if (simbolo != matriz[i][i]) {
          coincidencia = false;
        }
      }
      if (coincidencia) {

        return simbolo;
      }
    }

    // Diegonal inversa
    simbolo = matriz[0][2];
    if (simbolo != simboloDef) {
      for (int i = 1, j = 1; i < matriz.length; i++, j--) {
        if (simbolo != matriz[i][j]) {
          coincidencia = false;
        }
      }
      if (coincidencia) {

        return simbolo;
      }
    }
    return simboloDef;
  }

  public static boolean finPartida(char matriz[][], char simboloDef) {

    if (matrizLlena(matriz, simboloDef) || coincidenciaLinea(matriz, simboloDef) != simboloDef
        || coincidenciaColumna(matriz, simboloDef) != simboloDef
        || coincidenciaDiagonal(matriz, simboloDef) != simboloDef) {
      return true;
    }
    return false;
  }

  public static void mostrarGanador(char matriz[][], char J1, char J2, char simDef) {

    char simbolo = coincidenciaLinea(matriz, simDef);

    if (simbolo != simDef) {

      if (simbolo == J1) {
        out.println("Ha ganado el jugador 1!!");
      } else {
        out.println("Ha ganado el jugador 2!!");
      }
      return;
    }

    simbolo = coincidenciaColumna(matriz, simDef);

    if (simbolo != simDef) {

      if (simbolo == J1) {
        out.println("Ha ganado el jugador 1!!");
      } else {
        out.println("Ha ganado el jugador 2!!");
      }
      return;
    }

    simbolo = coincidenciaDiagonal(matriz, simDef);

    if (simbolo != simDef) {

      if (simbolo == J1) {
        out.println("Ha ganado el jugador 1!!");
      } else {
        out.println("Ha ganado el jugador 2!!");
      }
      return;
    }

    out.println("Hay empate");
  }

}
