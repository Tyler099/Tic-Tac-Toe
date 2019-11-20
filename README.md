//this is my tic tac toe game, written in java.
import java.util.Random;
import java.util.Scanner;
public class TicTacToe {
	public static void main(String[] args) {
		Scanner menu = new Scanner(System.in);
		Scanner pChoice = new Scanner(System.in);
		int menuChoice = 0;
		int playChoice = 0;
		final int ROUND_NUM = 0;
		final int ROUNDS_WON = 1;
		final int ROUNDS_LOST = 2;
		final int ROUNDS_TIED = 3;
		final int ROUNDS_PLAYED = 4;
		int stats[] = {1, 0, 0, 0, 0};
		System.out.println("Enter 0 if you are playing against the computer");
		System.out.println("\nEnter 1 if you are playing against another player");
		playChoice = Integer.parseInt(pChoice.nextLine());
		while (playChoice > 1 || playChoice < 0) {
			System.out.println("ERROR: Invalid input");
			playChoice = Integer.parseInt(pChoice.nextLine());
		}
		if (playChoice == 0) {
			playRound(stats, ROUND_NUM, ROUNDS_WON, ROUNDS_LOST, ROUNDS_TIED, ROUNDS_PLAYED);
			while (true) {
				System.out.println("\nEnter 1 if you would like to play another round");
				System.out.println("\nEnter 2 if you would like to display your stats");
				System.out.println("\nEnter 3 if you would like to quit the game");
				menuChoice = Integer.parseInt(menu.nextLine());
				while (menuChoice < 0 || menuChoice > 3) {
					System.out.println("ERROR: Invalid input");
					menuChoice = Integer.parseInt(menu.nextLine());
				}
				switch (menuChoice) {
				case 1:
					playRound(stats, ROUND_NUM, ROUNDS_WON, ROUNDS_LOST, ROUNDS_TIED, ROUNDS_PLAYED);
					break;
				case 2: 
					displayStats(stats, ROUNDS_WON, ROUNDS_LOST, ROUNDS_TIED, ROUNDS_PLAYED);
					break;
				case 3:
					return;
				}
		}
		}
		if (playChoice == 1) {
			playRound1(stats, ROUND_NUM, ROUNDS_WON, ROUNDS_LOST, ROUNDS_TIED, ROUNDS_PLAYED);
			while (true) {
				System.out.println("\nEnter 1 if you would like to play another round");
				System.out.println("\nEnter 2 if you would like to display your stats");
				System.out.println("\nEnter 3 if you would like to quit the game");
				menuChoice = Integer.parseInt(menu.nextLine());
				while (menuChoice < 0 || menuChoice > 3) {
					System.out.println("ERROR: Invalid input");
					menuChoice = Integer.parseInt(menu.nextLine());
				}
				switch (menuChoice) {
				case 1:
					playRound1(stats, ROUND_NUM, ROUNDS_WON, ROUNDS_LOST, ROUNDS_TIED, ROUNDS_PLAYED);
					break;
				case 2: 
					displayStats1(stats, ROUNDS_WON, ROUNDS_LOST, ROUNDS_TIED, ROUNDS_PLAYED);
					break;
				case 3:
					return;
				}
		}
			}
		}
	static void displayBoard(char[] c) {
		System.out.println("\n  -------------");
		System.out.println("  | " + c[0] + " | " + c[1] + " | " + c[2] + " |");
		System.out.println("  -------------");
		System.out.println("  | " + c[3] + " | " + c[4] + " | " + c[5] + " |");
		System.out.println("  -------------");
		System.out.println("  | " + c[6] + " | " + c[7] + " | " + c[8] + " |");
		System.out.println("  -------------");
	}
	static int userChoice() {
		Scanner select = new Scanner(System.in);
		System.out.println("\nEnter the number of the tile you would like to select");
			int choice = Integer.parseInt(select.nextLine());
			while (choice > 9 || choice < 1) {
				System.out.println("ERROR: Please select a number between 1 and 9!");
				choice = Integer.parseInt(select.nextLine());
			}
			return choice;
		}
	static void playRound(int[] stats, int roundNum, int roundsWon, int roundsLost, int roundsTied, int roundsPlayed) {
		Random random = new Random();
		char spots[] = {'1', '2', '3', '4', '5', '6', '7', '8', '9'};
		String gameStatus = "";
		System.out.println("TIC TAC TOE: ROUND " + stats[roundNum] + "\n");
		while (gameStatus.equals("")) {
			displayBoard(spots);
			int computerChoice = (random.nextInt(9) + 1);
			int choice = userChoice();
			while (spots[(choice - 1)] == 'X' || spots[(choice - 1)] == 'O') {
				System.out.println("ERROR: You must choose a spot that is not taken!");
				choice = userChoice();
			}
			spots[(choice - 1)] = 'X';
			gameStatus = checkWin(spots, stats, roundNum, roundsWon, roundsLost, roundsTied, roundsPlayed);
			if (gameStatus != "") {
				System.out.println("You chose: " + choice);
				break;
			}
			while (spots[(computerChoice - 1)] == 'X' || spots[(computerChoice - 1)] == 'O') {
				computerChoice = (random.nextInt(9) + 1);
			}
			spots[(computerChoice - 1)] = 'O';
			gameStatus = checkWin(spots, stats, roundNum, roundsWon, roundsLost, roundsTied, roundsPlayed);
			if (gameStatus != "") {
				System.out.println("You chose: " + choice);
				System.out.println("The computer chose: " + computerChoice);
				break;
			}
			System.out.println("You chose: " + choice);
			System.out.println("The computer chose: " + computerChoice);
		}
		displayBoard(spots);
		System.out.println(gameStatus);
	}
	static String checkWin(char[] spots, int[] stats, int roundNum, int roundsWon, int roundsLost, int roundsTied, int roundsPlayed) {
		String gameStatus = "";
		String line = "";
		int y = 0;
		for (int i = 0; i <= 2; i++) {
			for (int x = y; x <= (y + 2); x++) {
				line += spots[x];
				if (line.equals("XXX")) {
					gameStatus = wonRound(stats, roundNum, roundsWon, roundsPlayed);
					return gameStatus;
				}
				if (line.equals("OOO")) {
					gameStatus = lostRound(stats, roundNum, roundsLost, roundsPlayed);
					return gameStatus;
				}
			}
			line = "";
			y += 3;
		}
		y = 0;
		for (int i = 0; i <= 2; i++) {
			for (int x = y; x <= (y + 6); x += 3) {
				line += spots[x];
				if (line.equals("XXX")) {
					gameStatus = wonRound(stats, roundNum, roundsWon, roundsPlayed);
					return gameStatus;
				}
				if (line.equals("OOO")) {
					gameStatus = lostRound(stats, roundNum, roundsLost, roundsPlayed);
					return gameStatus;
				}
			}
			line = "";
			y += 1;
		}
		y = 0;
		for (int x = y; x <= (y + 8); x += 4) {
			line += spots[x];
			if (line.equals("XXX")) {
				gameStatus = wonRound(stats, roundNum, roundsWon, roundsPlayed);
				return gameStatus;
			}
			if (line.equals("OOO")) {
				gameStatus = lostRound(stats, roundNum, roundsLost, roundsPlayed);
				return gameStatus;
			}
		}
		line = "";
		y = 2;
		for (int x = y; x <= (y + 4); x += 2) {
			line += spots[x];
			if (line.equals("XXX")) {
				gameStatus = wonRound(stats, roundNum, roundsWon, roundsPlayed);
				return gameStatus;
			}
			if (line.equals("OOO")) {
				gameStatus = lostRound(stats, roundNum, roundsLost, roundsPlayed);
				return gameStatus;
			}
		}
		String hold = "";
		for (int i = 0; i <= 8; i++) {
			if (spots[i] == 'X' || spots[i] == 'O') {
				hold += "0";
			}
			else {
				break;
			}
		}
		if (hold.equals("000000000")) {
			gameStatus = tiedRound(stats, roundNum, roundsTied, roundsPlayed);
			return gameStatus;
		}
		return gameStatus;
	}
	static String wonRound(int[] stats, int roundNum, int roundsWon, int roundsPlayed) {
		stats[roundNum]++;
		stats[roundsWon]++;
		stats[roundsPlayed]++;
		return "You Win!";
	}
	static String lostRound(int[] stats, int roundNum, int roundsLost, int roundsPlayed) {
		stats[roundNum]++;
		stats[roundsLost]++;
		stats[roundsPlayed]++;
		return "You Lose!";
	}
	static String tiedRound(int[] stats, int roundNum, int roundsTied, int roundsPlayed) {
		stats[roundNum]++;
		stats[roundsTied]++;
		stats[roundsPlayed]++;
		return "This round is a tie!";
	}
	static void displayStats(int[] stats, int roundsWon, int roundsLost, int roundsTied, int roundsPlayed) {
		System.out.println("\nRounds Played: " + stats[roundsPlayed]);
		System.out.println("Rounds Won: " + stats[roundsWon]);
		System.out.println("Rounds Lost: " + stats[roundsLost]);
		System.out.println("Rounds Tied: " + stats[roundsTied]);
	}
	static void playRound1(int[] stats, int roundNum, int p1Wins, int p2Wins, int roundsTied, int roundsPlayed) {
		char spots[] = {'1', '2', '3', '4', '5', '6', '7', '8', '9'};
		String gameStatus = "";
		System.out.println("TIC TAC TOE: ROUND " + stats[roundNum] + "\n");
		while (gameStatus.equals("")) {
			displayBoard(spots);
			int p1Choice = userChoiceP1();
			while (spots[(p1Choice - 1)] == 'X' || spots[(p1Choice - 1)] == 'O') {
				System.out.println("ERROR: You must choose a spot that is not taken!");
				p1Choice = userChoiceP1();
			}
			spots[(p1Choice - 1)] = 'X';
			System.out.println("Player 1 chose: " + p1Choice);
			displayBoard(spots);
			gameStatus = checkWin1(spots, stats, roundNum, p1Wins, p2Wins, roundsTied, roundsPlayed);
			if (gameStatus != "") {
				System.out.println("Player 1 chose: " + p1Choice);
				break;
			}
			int p2Choice = 0;
			p2Choice = userChoiceP2();
			while (spots[(p2Choice - 1)] == 'X' || spots[(p2Choice - 1)] == 'O') {
				System.out.println("ERROR: You must choose a spot that is not taken!");
				p2Choice = userChoiceP2();
			}
			spots[(p2Choice - 1)] = 'O';
			System.out.println("Player 2 chose: " + p2Choice);
			gameStatus = checkWin1(spots, stats, roundNum, p1Wins, p2Wins, roundsTied, roundsPlayed);
			if (gameStatus != "") {
				System.out.println("Player 1 chose: " + p1Choice);
				System.out.println("Player 2 chose: " + p2Choice);
				break;
			}
		}
		displayBoard(spots);
		System.out.println(gameStatus);
		}
	static String checkWin1(char[] spots, int[] stats, int roundNum, int p1Wins, int p2Wins, int roundsTied, int roundsPlayed) {
		String gameStatus = "";
		String line = "";
		int y = 0;
		for (int i = 0; i <= 2; i++) {
			for (int x = y; x <= (y + 2); x++) {
				line += spots[x];
				if (line.equals("XXX")) {
					gameStatus = p1Win(stats, roundNum, p1Wins, roundsPlayed);
					return gameStatus;
				}
				if (line.equals("OOO")) {
					gameStatus = p2Win(stats, roundNum, p2Wins, roundsPlayed);
					return gameStatus;
				}
			}
			line = "";
			y += 3;
		}
		y = 0;
		for (int i = 0; i <= 2; i++) {
			for (int x = y; x <= (y + 6); x += 3) {
				line += spots[x];
				if (line.equals("XXX")) {
					gameStatus = p1Win(stats, roundNum, p1Wins, roundsPlayed);
					return gameStatus;
				}
				if (line.equals("OOO")) {
					gameStatus = p2Win(stats, roundNum, p2Wins, roundsPlayed);
					return gameStatus;
				}
			}
			line = "";
			y += 1;
		}
		y = 0;
		for (int x = y; x <= (y + 8); x += 4) {
			line += spots[x];
			if (line.equals("XXX")) {
				gameStatus = p1Win(stats, roundNum, p1Wins, roundsPlayed);
				return gameStatus;
			}
			if (line.equals("OOO")) {
				gameStatus = p2Win(stats, roundNum, p2Wins, roundsPlayed);
				return gameStatus;
			}
		}
		line = "";
		y = 2;
		for (int x = y; x <= (y + 4); x += 2) {
			line += spots[x];
			if (line.equals("XXX")) {
				gameStatus = p1Win(stats, roundNum, p1Wins, roundsPlayed);
				return gameStatus;
			}
			if (line.equals("OOO")) {
				gameStatus = p2Win(stats, roundNum, p2Wins, roundsPlayed);
				return gameStatus;
			}
		}
		String hold = "";
		for (int i = 0; i <= 8; i++) {
			if (spots[i] == 'X' || spots[i] == 'O') {
				hold += "0";
			}
			else {
				break;
			}
		}
		if (hold.equals("000000000")) {
			gameStatus = tiedRound(stats, roundNum, roundsTied, roundsPlayed);
			return gameStatus;
		}
		return gameStatus;
	}
	static String p1Win(int[] stats, int roundNum, int p1Wins, int roundsPlayed) {
		stats[roundNum]++;
		stats[p1Wins]++;
		stats[roundsPlayed]++;
		return "Player 1 Wins!";
	}
	static String p2Win(int[] stats, int roundNum, int p2Wins, int roundsPlayed) {
		stats[roundNum]++;
		stats[p2Wins]++;
		stats[roundsPlayed]++;
		return "Player 2 Wins!";
	}
	static void displayStats1(int[] stats, int p1Wins, int p2Wins, int roundsTied, int roundsPlayed) {
		System.out.println("\nRounds Played: " + stats[roundsPlayed]);
		System.out.println("Player 1 Wins: " + stats[p1Wins]);
		System.out.println("Player 2 Wins: " + stats[p2Wins]);
		System.out.println("Rounds Tied: " + stats[roundsTied]);
	}
	static int userChoiceP1() {
		Scanner select = new Scanner(System.in);
		System.out.println("\nPlayer 1, enter the number of the tile you would like to select");
		int choice = Integer.parseInt(select.nextLine());
		while (choice > 9 || choice < 1) {
			System.out.println("ERROR: Please select a number between 1 and 9!");
			choice = Integer.parseInt(select.nextLine());
		}
		return choice;
	}
	static int userChoiceP2() {
		Scanner select = new Scanner(System.in);
		System.out.println("\nPlayer 2, enter the number of the tile you would like to select");
		int choice = Integer.parseInt(select.nextLine());
		while (choice > 9 || choice < 1) {
			System.out.println("ERROR: Please select a number between 1 and 9!");
			choice = Integer.parseInt(select.nextLine());
		}
		return choice;
	}
}
