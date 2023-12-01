package checkers;

public class EvaluatePosition {
    static private final int WIN = Integer.MAX_VALUE / 2;
    static private final int LOSE = Integer.MIN_VALUE / 2;
    static private boolean _color;

    static public void changeColor(boolean color) {
        _color = color;
    }

    static public boolean getColor() {
        return _color;
    }

    static private boolean isInBoard(int i, int j) {
        return i - 2 >= 0 && i + 2 < 8 && j - 2 >= 0 && j + 2 < 8;
    }

    static private int kingCanCapture(AIBoard board, int i, int j, boolean white, int multiKill) {
        if (multiKill > 4) return multiKill;

        int iterResult = multiKill;

        int[][] directions = {{2, 2}, {2, -2}, {-2, 2}, {-2, -2}};

        for (int[] dir : directions) {
            int x = i + dir[0];
            int y = j + dir[1];

            if (isInBoard(x, y) && !board._board[i + dir[0] / 2][j + dir[1] / 2].empty &&
                    board._board[i + dir[0] / 2][j + dir[1] / 2].white != white && board._board[x][y].empty) {
                iterResult += kingCanCapture(board, x, y, board._board[i + dir[0] / 2][j + dir[1] / 2].white, 1 + multiKill);
            }
        }

        return iterResult;
    }

    static private int peaceCanCapture(AIBoard board, int i, int j, boolean white, int multiKill) {
        if (multiKill > 4) return multiKill;

        int iterResult = multiKill;

        int reverse = white ? 1 : -1;

        int[][] directions = {{2, 2 * reverse}, {-2, 2 * reverse}};

        for (int[] dir : directions) {
            int x = i + dir[0];
            int y = j + dir[1];

            if (isInBoard(x, y) && !board._board[i + dir[0] / 2][j + dir[1] / 2].empty &&
                    board._board[i + dir[0] / 2][j + dir[1] / 2].white != white && board._board[x][y].empty) {
                iterResult += kingCanCapture(board, x, y, board._board[i + dir[0] / 2][j + dir[1] / 2].white, 1 + multiKill);
            }
        }

        return iterResult;
    }

    static private int isInDanger(AIBoard board, int i, int j, boolean white) {
        int reverse = white ? 1 : -1;

        if (isInBoard(i + 1, j + reverse) && isInBoard(i - 1, j - reverse) &&
                !board._board[i + 1][j + reverse].empty && board._board[i + 1][j + reverse].white != white &&
                !board._board[i - 1][j - reverse].empty) {
            return 1;
        }

        if (isInBoard(i + 1, j + reverse) && isInBoard(i - 1, j - reverse) &&
                !board._board[i - 1][j - reverse].empty && board._board[i - 1][j - reverse].white != white &&
                !board._board[i + 1][j - reverse].empty) {
            return 1;
        }

        return 0;
    }

    static private int isCovered(AIBoard board, int i, int j, boolean white) {
        int reverse = white ? 1 : -1;

        if (i - 1 < 0 || i + 1 > 7 || j - 1 < 0 || j + 1 > 7) return 1;

        if (!board._board[i + 1][j + reverse].empty && board._board[i + 1][j + reverse].white == white) return 1;
        if (!board._board[i - 1][j + reverse].empty && board._board[i - 1][j + reverse].white == white) return 1;

        return -1;
    }

    static public int evaluatePosition(AIBoard board) {
        int myRating = 0;
        int opponentsRating = 0;
        int size = board.getSize();

        for (int i = 0; i < size; i++) {
            for (int j = (i + 1) % 2; j < size; j += 2) {
                if (!board._board[i][j].empty) {
                    if (board._board[i][j].white == getColor()) {
                        // To jest mój pionek
                        if (board._board[i][j].king) {
                            myRating += 50;
                            myRating += 7 * kingCanCapture(board, i, j, board._board[i][j].white, 0);
                            myRating += 3 * isInDanger(board, i, j, board._board[i][j].white);
                            myRating += 2 * isCovered(board, i, j, board._board[i][j].white);
                        } else {
                            myRating += 30;
                            myRating += 6 * peaceCanCapture(board, i, j, board._board[i][j].white, 0);
                            myRating += 3 * isInDanger(board, i, j, board._board[i][j].white);
                            myRating += 2 * isCovered(board, i, j, board._board[i][j].white);
                        }
                    } else {
                        // To jest pionek przeciwnika
                        if (board._board[i][j].king) {
                            opponentsRating += 50;
                            opponentsRating += 7 * kingCanCapture(board, i, j, board._board[i][j].white, 0);
                            opponentsRating += 3 * isInDanger(board, i, j, board._board[i][j].white);
                            opponentsRating += 2 * isCovered(board, i, j, board._board[i][j].white);
                        } else {
                            opponentsRating += 30;
                            opponentsRating += 6 * peaceCanCapture(board, i, j, board._board[i][j].white, 0);
                            opponentsRating += 3 * isInDanger(board, i, j, board._board[i][j].white);
                            opponentsRating += 2 * isCovered(board, i, j, board._board[i][j].white);
                        }
                    }
                }
            }
        }

        if (myRating == 0) return LOSE;
        else if (opponentsRating == 0) return WIN;
        else return myRating - opponentsRating;
    }
}

