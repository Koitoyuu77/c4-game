using System;

public class ConnectFour
{
    private const int Rows = 6;
    private const int Columns = 7;
    private char[,] board = new char[Rows, Columns];
    private char currentPlayer = 'X';

    public ConnectFour()
    {
        // Initialize the board with empty spaces
        for (int row = 0; row < Rows; row++)
        {
            for (int col = 0; col < Columns; col++)
            {
                board[row, col] = '|';
            }
        }
    }

    public void DisplayBoard()
    {
        Console.Clear();
        for (int row = 0; row < Rows; row++)
        {
            for (int col = 0; col < Columns; col++)
            {
                Console.Write(board[row, col] + " ");
            }
            Console.WriteLine();
        }
        Console.WriteLine();
    }

    public bool DropPiece(int column)
    {
        if (column < 0 || column >= Columns || board[0, column] != '|')
        {
            return false; // Invalid move
        }

        // Find the first available row in the column
        for (int row = Rows - 1; row >= 0; row--)
        {
            if (board[row, column] == '|')
            {
                board[row, column] = currentPlayer;
                return true;
            }
        }

        return false; // If no space is available
    }

    public bool CheckWin()
    {
        // Check horizontal, vertical, and diagonal wins
        for (int row = 0; row < Rows; row++)
        {
            for (int col = 0; col < Columns; col++)
            {
                if (board[row, col] != '|')
                {
                    if (CheckDirection(row, col, 1, 0) || // Horizontal
                        CheckDirection(row, col, 0, 1) || // Vertical
                        CheckDirection(row, col, 1, 1) || // Diagonal /
                        CheckDirection(row, col, 1, -1))  // Diagonal \
                    {
                        return true;
                    }
                }
            }
        }
        return false;
    }

    private bool CheckDirection(int row, int col, int dRow, int dCol)
    {
        char piece = board[row, col];
        int count = 0;

        // Check in the given direction (dRow, dCol)
        for (int i = 0; i < 4; i++)
        {
            int r = row + dRow * i;
            int c = col + dCol * i;

            // Check if we're within bounds and the piece matches
            if (r >= 0 && r < Rows && c >= 0 && c < Columns && board[r, c] == piece)
            {
                count++;
            }
            else
            {
                break;
            }
        }

        return count == 4;
    }

    public void SwitchPlayer()
    {
        currentPlayer = (currentPlayer == 'X') ? 'O' : 'X';
    }

    public static void Main(string[] args)
    {
        ConnectFour game = new ConnectFour();

        while (true)
        {
            game.DisplayBoard();

            Console.WriteLine($"Player {game.currentPlayer}'s turn!");
            Console.Write("Enter column (0-6): ");
            int column;
            if (!int.TryParse(Console.ReadLine(), out column) || column < 0 || column >= Columns)
            {
                Console.WriteLine("Invalid column. Try again.");
                continue;
            }

            if (!game.DropPiece(column))
            {
                Console.WriteLine("Column is full or invalid. Try again.");
                continue;
            }

            if (game.CheckWin())
            {
                game.DisplayBoard();
                Console.WriteLine($"Player {game.currentPlayer} wins!");
                break;
            }

            game.SwitchPlayer();
        }
    }
}
