import java.util.HashSet;
import java.util.Set;

public class SudokuSolver {

    private static final int GRID_SIZE = 9;


    public static void main(String[] args){

        int[][] board = {
                {7,0,2,0,5,0,6,0,0},
                {0,0,0,0,0,3,0,0,0},
                {1,0,0,0,0,9,5,0,0},
                {8,0,0,0,0,0,0,9,0},
                {0,4,3,0,0,0,7,5,0},
                {0,9,0,0,0,0,0,0,8},
                {0,0,9,7,0,0,0,0,5},
                {0,0,0,2,0,0,0,0,0},
                {0,0,7,0,4,0,2,0,3}
        };

    if(solveBoard(board)){
        System.out.println("Solved Successfully");
    } else {
        System.out.println("Unsolvable board :(");
    }
        printBoard(board);

        System.out.println(isValidSudoku(board));

    }

    public static boolean isValidSudoku(int[][] board) {
        Set seen = new HashSet();
        for (int i=0; i<9; ++i) {
            for (int j=0; j<9; ++j) {
                int number = board[i][j];
                if (number != 0)
                    if (!seen.add(number + " in row " + i) ||
                            !seen.add(number + " in column " + j) ||
                            !seen.add(number + " in block " + i/3 + "-" + j/3))
                        return false;
            }
        }
        return true;
    }

    private static void printBoard(int[][] board) {
        for(int row = 0;row< GRID_SIZE;row++){
            if(row % 3 == 0 && row != 0){
                System.out.println("-----------");
            }
            for(int col = 0; col< GRID_SIZE;col++){
                if(col % 3 == 0 && col != 0){
                    System.out.print("|");
                }
                System.out.print(board[row][col]);
            }
            System.out.println();
        }
    }

    private static boolean isNumberInRow(int[][] board, int num, int row){
        for(int i=0;i<GRID_SIZE;i++){
            if(board[row][i] == num){
                return true;
            }
        }
        return false;
    }

    private static boolean isNumberInColumn(int[][] board, int num, int col){
        for(int i=0;i<GRID_SIZE;i++){
            if(board[i][col] == num){
                return true;
            }
        }
        return false;
    }


    private static boolean isNumberInBox(int[][] board, int num, int row, int col){
        int localBoxRow = row - row % 3;
        int localBoxCol = col - col % 3;

        for(int i=localBoxRow;i<localBoxRow+3;i++){
            for(int j = localBoxCol;j< localBoxCol + 3;j++){
               if(board[i][j] == num){
                   return true;
               }
            }
        }
        return false;
    }


    private static boolean isValidPlacement(int[][] board, int num, int row, int col){
        return !isNumberInRow(board,num,row) &&
                !isNumberInColumn(board,num,col) &&
                !isNumberInBox(board,num,row,col);
    }

    private static boolean solveBoard(int[][] board){
        for(int row=0;row<GRID_SIZE;row++){
            for(int col=0;col<GRID_SIZE;col++){
                if(board[row][col] == 0){
                    for(int numToTry = 1;numToTry <= GRID_SIZE; numToTry++){
                        if(isValidPlacement(board,numToTry,row,col)){
                            board[row][col] = numToTry;

                            if(solveBoard(board)){
                                return true;
                            } else {
                                board[row][col] = 0;
                            }
                        }

                    }
                    return false;
                }
            }
        }
        return true;
    }




}
