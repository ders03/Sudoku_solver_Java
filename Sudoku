import java.util.*;

public class Sudoku {

  public static class Cell {
    ArrayList<Character> possible = new ArrayList<Character>();
  }

  static ArrayList<Character> vals = new ArrayList<Character>(Arrays.asList('.', '1', '2', '3', '4', '5', '6', '7', '8', '9'));
  public static void main(String[] args) {
    char[][] puzzle = SudokuP.puzzle();
    char[][] invalid = {
  {'.', '1', '6', '9', '.', '8', '4', '5', '7'},
  {'5', '2', '.', '.', '.', '1', '.', '9', '.'},
  {'.', '8', '7', '.', '.', '.', '.', '1', '.'},
  {'.', '.', '3', '.', '2', '.', '.', '.', '5'},
  {'9', '.', '.', '.', '1', '.', '6', '.', '2'},
  {'.', '5', '.', '8', '.', '3', '1', '.', '.'},
  {'1', '3', '.', '.', '9', '.', '.', '7', '4'},
  {'.', '.', '.', '.', '.', '2', '.', '.', '.'},
  {'.', '.', '5', '.', '.', '6', '3', '.', '.'},
  };
  System.out.println("Starting Board");
  for (int i = 0; i < puzzle.length; i++) {
    for (char c: puzzle[i]) {
      System.out.print(c + "  ");
    }
    System.out.println();
  }
  System.out.println("-------------------------");
  //System.out.println("Solution");
  //System.out.println(getSquares(invalid).get(1));
  //System.out.println(solve(puzzle));
  //System.out.println(checkSingleBoxTest(invalid, 1, 2, '3'));
  //System.out.println(checkCandidate(invalid, 0, 0, '2'));
  solve(puzzle);
  //System.out.println(checkSingleBoxTest(invalid, 1, 1, '1'));
  }

  /* valid if
     each row contains unique values from 1-9
     each col contains unique values from 1-9
     each of 9 subsquares contains a unique value from 1-9
  */
  public static boolean check(char[][] puzzle) {
    if (checkRow(puzzle) && checkCol(puzzle) && checkSquare(puzzle)) {
      return true;
    }
    return false;
  }

  // read in row, check for duplicate or invalid values
  public static boolean checkRow(char[][] puzzle) {
    int index = 0;
    char[] row = new char[puzzle.length];
    ArrayList<Character> check = new ArrayList<>();
    while (index < puzzle.length) {
      row = puzzle[index];
      for (char c: row) {
        if (c != '.' && !check.contains(c) && vals.contains(c)) {
          check.add(c);
        }
        else if (check.contains(c)) {
          return false;
        }
      }
      //System.out.println(row);
      check.clear();
      index += 1;
      if (index == puzzle.length) {
        break;
      }
    }
    return true;
  }

  // read in col, check for duplicate or invalid values
  public static boolean checkCol(char[][] puzzle) {
    int index = 0;
    char[] col = new char[puzzle.length];
    ArrayList<Character> check = new ArrayList<>();
    while (index < puzzle.length) {
      for (int i = 0; i < puzzle.length; i++) {
        col[i] = puzzle[i][index];
      }
      for (char c: col) {
        if (c != '.' && !check.contains(c) && vals.contains(c)) {
          check.add(c);
        }
        else if (check.contains(c) || !vals.contains(c)) {
          return false;
        }
      }
      check.clear();
      //System.out.println(col);
      index += 1;
      if (index == puzzle.length) {
        continue;
      }
    }
    return true;
  }

  // split puzzle to row of blocks
  // read blocks into String(want to use toCharArray())
  // format String to char[][] and put into checkRow()
  // if valid, check next row of blocks, else return false
  // NEED SINGLE CHECK SQUARE FUNCTION, only good for checking init and solved cases
  public static boolean checkSquare(char[][] puzzle) {
    String check = new String();
    String check2 = new String();
    String check3 = new String();
    char[][] formatedCheck = new char[1][9];
    char[][] formatedCheck2 = new char[1][9];
    char[][] formatedCheck3 = new char[1][9];
    // this traverses by block from left to right, then shifts down to next block row and repeats
    // every time a critical value is reached signalling block read in, block is checked for validity
    for (int i = 0; i < 9; i++) {
      for (int j = 0; j < 9; j++) {
        // row 1
        if (j < 3 && i < 3) {
          //block1[i][j] = puzzle[i][j];
          check += puzzle[i][j];
          //block2[i][j] = puzzle[i][j+3];
          check2 += puzzle[i][j+3];
          //block3[i][j] = puzzle[i][j+6];
          check3 += puzzle[i][j+6];
        }
        // row 2
        else if (j > 2 && j < 6 && i > 2 && i < 6) {
          //block1[i-3][j-3] = puzzle[i][j-3];
          check += puzzle[i][j-3];
          //block2[i-3][j-3] = puzzle[i][j];
          check2 += puzzle[i][j];
          //block3[i-3][j-3] = puzzle[i][j+3];
          check3 += puzzle[i][j+3];
        }
        // row 3
        else if (j > 5 && j < 9 && i > 5 && i < 9) {
          //block1[i-6][j-6] = puzzle[i][j-6];
          check += puzzle[i][j-6];
          //block2[i-6][j-6] = puzzle[i][j-3];
          check2 += puzzle[i][j-3];
          //block3[i-6][j-6] = puzzle[i][j];
          check3 += puzzle[i][j];
        }
      }
      // at these values, a complete set of blocks has been read in
      if (i == 2 || i == 5 || i == 8) {
        // format to put in checkRow()
        formatedCheck[0] = check.toCharArray();
        formatedCheck2[0] = check2.toCharArray();
        formatedCheck3[0] = check3.toCharArray();
        // reset for next set of blocks
        check = "";
        check2 = "";
        check3 = "";
        // check formated values
        if (!checkRow(formatedCheck) || !checkRow(formatedCheck2) || !checkRow(formatedCheck3)) {
          return false;
        }
      }
    }
    return true;
  }

  // populate possibilities to array, feed in to checker
  public static boolean solve(char[][] puzzle) {
    char[][] solution = new char[9][9];
    Cell[][] cells = new Cell[9][9];
    if (!check(puzzle)) {
      System.out.println("The given sudoku puzzle is invalid");
      return false;
    }
    // populate cells
    for (int i = 0; i < 9; i++) {
      for (int j = 0; j < 9; j++) {
        cells[i][j] = new Cell();
        if (puzzle[i][j] != '.') {
          cells[i][j].possible.add(puzzle[i][j]);
          solution[i][j] = puzzle[i][j];
        }
        else {
          solution[i][j] = '.';
        }
      }
    }
    // add possible values to empty cells and check for singles
    for (int i = 0; i < 9; i++) {
      for (int j = 0; j < 9; j++) {
        if (cells[i][j].possible.size() != 1) {
          for (char c: vals) {
            if (c != '.' && !getRow(solution, i).contains(c) && !getCol(solution, j).contains(c) && check(solution)) {
              cells[i][j].possible.add(c);
            }
          }
          if (cells[i][j].possible.size() == 1) {
            solution[i][j] = cells[i][j].possible.get(0);
          }
        }
      }
    }
    // if solution, print, else no solution
    if (Solution(solution, cells) && check(solution)) {
      for (int i = 0; i < 9; i++) {
        for (char c: solution[i]) {
          System.out.print(c + "  ");
        }
        System.out.println();
      }
      return true;
    }
    else {
      System.out.println("This puzzle is unsolvable");
    }
    return false;
  }

  // can use .contains() for ArrayList
  public static ArrayList getRow(char[][] puzzle, int index) {
    ArrayList<Character> result = new ArrayList<>();
    for (char c: puzzle[index]) {
      result.add(c);
    }
    return result;
  }

  // can use .contains() for ArrayList
  public static ArrayList getCol(char[][] puzzle, int index) {
    ArrayList<Character> result = new ArrayList<>();
    for (int i = 0; i < puzzle.length; i++) {
      result.add(puzzle[i][index]);
    }
    return result;
  }

  // iterate over single box determined by row and col input, check for given char present
  // if input checking first num after 0, 3 or 6, subtract one to get start index - (3x + 1)mod3 = 1
  // if input checking second num after 0, 3 or 6, subtract two to get start index - (3x + 2)mod3 = 2
  // if input checking 0, 3, 6 or 9, check 0, 3, 6, or 9 - (3x)mod3 = 0
  public static boolean checkSingleBox(char[][] puzzle, int row, int col, char candidate) {
    int rowStep = row - row%3; // set lower bounds
    int colStep = col - col%3; // this forces input vals to proper bounds - (x = 0||3, x = 3||6, x = 6||9)
    for (int i = rowStep; i < rowStep + 3; i++) {
      for (int j = colStep; j < colStep + 3; j++) {
        //System.out.println(i + " " + j);
        if (puzzle[i][j] == candidate) {
          return false;
        }
      }
    }
    return true;
  }

  // put candidate through iterative checker functions
  public static boolean checkCandidate(char[][] puzzle, int row, int col, char candidate) {
    return checkRow(puzzle) && checkCol(puzzle) && checkSingleBox(puzzle, row, col, candidate);
  }

  // check possible values, if a valid candidate works, return solution, else, restart check loop, after possible tried, return false
  public static boolean Solution(char[][] puzzle, Cell[][] cells) {
    for (int i = 0; i < 9; i++) {
      for (int j = 0; j < 9; j++) {
        // get empty cell
        if (puzzle[i][j] == '.') {
          // get possibilities
          for (char candidate: cells[i][j].possible) {
            // loop through possibilities
            if (checkCandidate(puzzle, i, j, candidate)) {
              puzzle[i][j] = candidate;
              if (Solution(puzzle, cells)) { // if Solution return true, it has iterated over every cell no false return, solved
                // at this point a valid value has been found, if present
                return true;
              }
              else {
                // reset loop for char check
                puzzle[i][j] = '.';
              }
            }
          }
          // this point reachable only after all possibilities have been tried
          return false;
        }
      }
    }
    return true;
  }
}
