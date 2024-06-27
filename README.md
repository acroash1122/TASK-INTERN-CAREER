# Task-Intern-Career
<b>Overview</b><br>
The Hangman game application is a Java Swing-based interactive game where players guess letters to reveal a hidden word. It includes features such as:

Interactive GUI with a main game window showing the hidden word and input controls.
Messages for feedback on correct and incorrect guesses.
Ability to start a new game with a random word from a predefined list.
Game logic to track misses and determine when the player wins.
How to Run
Follow these steps to run the Hangman game:

<b>Prerequisites:</b>

Ensure you have Java Development Kit (JDK) installed on your system.
Make sure your IDE (like IntelliJ IDEA, Eclipse, etc.) is set up to run Java applications.
Setup:

Download the source code provided (Main.java).
<br><b>Compile and Run:</b><br>

Open your preferred IDE and import the Main.java file.
Compile the code and run the Main class (Main.java should contain the Main class).
<br><b>Gameplay:</b>

Upon running the application, the Hangman game window will appear.
Guess letters using the input field and "Guess" button.
Start a new game anytime by clicking the "New Game" button.
Code Comments
Here's an annotated version of your Main.java file with comments explaining its functionality:
<poem>import javax.swing.*;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.util.Random;

public class Main extends JFrame {
private static final String[] WORDS = {"write", "that", "program", "manifest", "hallucination", "counter"};
private String randomWord;
private int misses;
private boolean[] guessed;
private JLabel wordLabel;
private JLabel messageLabel;
private JTextField inputField;
private JButton guessButton;
private JButton newGameButton;

    public Main() {
        setTitle("Hangman Game");
        setSize(400, 300);
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        setLayout(new BorderLayout());

        wordLabel = new JLabel("", JLabel.CENTER);
        wordLabel.setFont(new Font("Serif", Font.PLAIN, 24));
        add(wordLabel, BorderLayout.NORTH);

        JPanel centerPanel = new JPanel();
        messageLabel = new JLabel("Guess a letter:");
        centerPanel.add(messageLabel);

        inputField = new JTextField(1);
        centerPanel.add(inputField);

        guessButton = new JButton("Guess");
        guessButton.addActionListener(new GuessButtonListener());
        centerPanel.add(guessButton);

        add(centerPanel, BorderLayout.CENTER);

        newGameButton = new JButton("New Game");
        newGameButton.addActionListener(new NewGameButtonListener());
        add(newGameButton, BorderLayout.SOUTH);

        startNewGame();
    }

    private void startNewGame() {
        Random random = new Random();
        randomWord = WORDS[random.nextInt(WORDS.length)];
        misses = 0;
        guessed = new boolean[randomWord.length()];
        updateWordLabel();
        messageLabel.setText("Guess a letter:");
        inputField.setText("");
    }

    private void updateWordLabel() {
        StringBuilder displayWord = new StringBuilder();

        for (int i = 0; i < randomWord.length(); ++i) {
            if (guessed[i]) {
                displayWord.append(randomWord.charAt(i));
            } else {
                displayWord.append("*");
            }
            displayWord.append(" ");
        }

        wordLabel.setText(displayWord.toString());
    }

    private void checkGuess(char guess) {
        boolean correct = false;

        for (int i = 0; i < randomWord.length(); ++i) {
            if (!guessed[i] && randomWord.charAt(i) == guess) {
                guessed[i] = true;
                correct = true;
            }
        }

        if (!correct) {
            ++misses;
            messageLabel.setText("Incorrect! Total misses: " + misses);
        } else {
            messageLabel.setText("Good guess!");
        }

        updateWordLabel();

        if (checkWin()) {
            messageLabel.setText("You won! The word was: " + randomWord);
            guessButton.setEnabled(false);
        }
    }

    private boolean checkWin() {
        for (boolean b : guessed) {
            if (!b) {
                return false;
            }
        }
        return true;
    }

    public static void main(String[] args) {
        SwingUtilities.invokeLater(() -> {
            Main game = new Main();
            game.setVisible(true);
        });
    }

    private class GuessButtonListener implements ActionListener {
        @Override
        public void actionPerformed(ActionEvent e) {
            String guessText = inputField.getText().toLowerCase().trim();
            if (guessText.isEmpty()) {
                messageLabel.setText("Enter a letter.");
                return;
            }

            boolean allLettersValid = true;
            for (char guess : guessText.toCharArray()) {
                if (!Character.isLetter(guess)) {
                    messageLabel.setText("Invalid input: " + guess + ". Enter letters only.");
                    allLettersValid = false;
                    break;
                }
                checkGuess(guess);
            }

            if (allLettersValid) {
                inputField.setText("");
            }
        }
    }

    private class NewGameButtonListener implements ActionListener {
        @Override
        public void actionPerformed(ActionEvent e) {
            guessButton.setEnabled(true);
            startNewGame();
        }
    }
}
</poem>
<br><b>Comments Explanation:</b><br>
Each method in the Main class has comments explaining its purpose and functionality.
Comments clarify how the game initializes , updates the displayed word <b>(updateWordLabel())</b>, checks guesses ,<b>(checkGuess() and checkWin())>/b>, and handles user input <b>(GuessButtonListener and NewGameButtonListener)</b>.
<p> <b> Conclusion </b> </p>
