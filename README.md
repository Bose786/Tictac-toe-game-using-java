 int currentRow, currentCol;

    public static Scanner in = new Scanner(System.in);

    public static void main (String args[]){
        initGame();

        do{
            playerMove(currentPlayer);
            updateGame(currentPlayer,currentRow,currentCol);
            printBoard();

            if(currentState == CROSS_WON){
                System.out.println(" X won! Bye!");
            }else if(currentState == NOUGHT_WON){
                System.out.println(" Y won! Bye!");
            }else if(currentState == DRAW){
                System.out.println("It is a Draw Bye!");
            }

            currentPlayer = (currentPlayer == CROSS) ? NOUGHT : CROSS;
        }while (currentState == PLAYING);
    }

    public static void initGame(){
        for(int row = 0; row < ROWS; row++){
            for (int col = 0; col <  COLS; col++){
                board[row][col] = EMPTY;
            }
        }

        currentState = PLAYING;
        currentPlayer = CROSS;
    }

    public static void playerMove(int theseed){
        boolean validInput = false;

        do{
            if(theseed == CROSS){
                System.out.println("PLAYER X ENTER YOUR MOVE (row[1-3] column[1-3]): ");
            }else{
                System.out.println("PLAYER O ENTER YOUR MOVE (row[1-3] column[1-3]): ");
            }

            int row = in.nextInt() - 1;
            int col = in.nextInt() - 1;

            if(row >= 0 && row < ROWS && col >= 0 && col < COLS && board[row][col] == EMPTY){
                currentRow = row;
                currentCol = col;
                board[currentRow][currentCol] = theseed;
                validInput =true;
            }else{
                System.out.println("The move at (" + (row + 1) + " , " + (col + 1) + ") is not valid . Try again....");
            }
        }while (!validInput);
    }

    public static void updateGame(int theSeed, int currentRow, int currentCol){
        if(hasWon(theSeed, currentRow, currentCol)){
            currentState = (theSeed == CROSS) ? CROSS_WON : NOUGHT_WON;
        }else if(isDraw()){
            currentState = DRAW;
        }
    }

    public static boolean isDraw(){
        for (int row = 0; row < ROWS; ++row){
            for (int col = 0; col < COLS; ++col){
                if(board[row][col] == EMPTY){
                    return false;
                }
            }
        }

        return true;
    }

    public static boolean hasWon(int theseed, int currentRow, int currentCol){
        return (board[currentCol][0] == theseed && board[currentCol][1] == theseed && board[currentCol][2] == theseed
                || board[0][currentCol] == theseed && board[1][currentCol] == theseed && board[2][currentCol] == theseed
                || currentRow == currentCol && board[0][0] == theseed && board[1][1] == theseed && board[2][2] == theseed || currentRow + currentCol == 2 && board[0][2] ==  theseed && board[1][1] == theseed && board[2][0] == theseed
                );
    }

    public static void printBoard(){
        for (int row = 0; row < ROWS; ++row){
            for (int col = 0; col < COLS; ++col){
                printCell(board[row][col]);
                if(col != COLS - 1){
                    System.out.print(" | ");
                }
            }

            System.out.println();
            if (row != ROWS - 1){
                System.out.println("-----------");
            }
        }

        System.out.println();
    }

    public static void printCell(int content){
        switch (content){
            case EMPTY :
                System.out.print(" ");break;
            case NOUGHT:
                System.out.print(" O ");break;
            case CROSS:
                System.out.print(" X ");break;
        }
    }
}
