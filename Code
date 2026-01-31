import javax.swing.*;
import java.awt.*;
import java.awt.event.*;
import java.util.Random;

public class SnakeGame extends JPanel implements ActionListener, KeyListener {
    private final int TILE_SIZE = 25;
    private final int WIDTH = 20;   // 20 tiles wide
    private final int HEIGHT = 20;  // 20 tiles tall
    private final int DELAY = 150;

    private int[] x = new int[WIDTH * HEIGHT];
    private int[] y = new int[WIDTH * HEIGHT];
    private int snakeLength;
    private int foodX, foodY;
    private char direction = 'R'; 
    private boolean running = true;
    private boolean gameOver = false;
    private Timer timer;
    private Random random;
    private int score = 0;

    public SnakeGame() {
        setPreferredSize(new Dimension(WIDTH * TILE_SIZE, HEIGHT * TILE_SIZE));
        setBackground(Color.black);
        setFocusable(true);
        addKeyListener(this);

        random = new Random();
        startGame();
    }

    private void startGame() {
        snakeLength = 3;
        score = 0;
        direction = 'R';
        gameOver = false;
        running = true;

        for (int i = 0; i < snakeLength; i++) {
            x[i] = 100 - i * TILE_SIZE;
            y[i] = 100;
        }

        spawnFood();
        timer = new Timer(DELAY, this);
        timer.start();
    }

    private void spawnFood() {
        foodX = random.nextInt(WIDTH) * TILE_SIZE;
        foodY = random.nextInt(HEIGHT) * TILE_SIZE;
    }

    @Override
    public void paintComponent(Graphics g) {
        super.paintComponent(g);
        if (running) {
            // Score
            g.setColor(Color.white);
            g.setFont(new Font("Arial", Font.PLAIN, 20));
            g.drawString("Score: " + score, 10, 20);

            // Food
            g.setColor(Color.red);
            g.fillOval(foodX, foodY, TILE_SIZE, TILE_SIZE);

            // Snake
            for (int i = 0; i < snakeLength; i++) {
                if (i == 0) g.setColor(Color.green);
                else g.setColor(Color.lightGray);
                g.fillRect(x[i], y[i], TILE_SIZE, TILE_SIZE);
            }
        } else {
            showGameOver(g);
        }
    }

    private void showGameOver(Graphics g) {
        g.setColor(Color.red);
        g.setFont(new Font("Arial", Font.BOLD, 40));
        g.drawString("Game Over", WIDTH * TILE_SIZE / 4, HEIGHT * TILE_SIZE / 2);

        g.setColor(Color.white);
        g.setFont(new Font("Arial", Font.PLAIN, 25));
        g.drawString("Final Score: " + score, WIDTH * TILE_SIZE / 3, HEIGHT * TILE_SIZE / 2 + 40);

        g.setFont(new Font("Arial", Font.PLAIN, 20));
        g.drawString("Press SPACE to Restart", WIDTH * TILE_SIZE / 4, HEIGHT * TILE_SIZE / 2 + 80);
    }

    private void move() {
        for (int i = snakeLength - 1; i > 0; i--) {
            x[i] = x[i - 1];
            y[i] = y[i - 1];
        }

        switch (direction) {
            case 'U': y[0] -= TILE_SIZE; break;
            case 'D': y[0] += TILE_SIZE; break;
            case 'L': x[0] -= TILE_SIZE; break;
            case 'R': x[0] += TILE_SIZE; break;
        }

        // Wrap around logic
        if (x[0] < 0) x[0] = (WIDTH - 1) * TILE_SIZE;
        else if (x[0] >= WIDTH * TILE_SIZE) x[0] = 0;
        if (y[0] < 0) y[0] = (HEIGHT - 1) * TILE_SIZE;
        else if (y[0] >= HEIGHT * TILE_SIZE) y[0] = 0;
    }

    private void checkFood() {
        if (x[0] == foodX && y[0] == foodY) {
            snakeLength++;
            score += 10;
            spawnFood();
        }
    }

    private void checkCollision() {
        for (int i = 1; i < snakeLength; i++) {
            if (x[0] == x[i] && y[0] == y[i]) {
                running = false;
                gameOver = true;
                timer.stop();
            }
        }
    }

    @Override
    public void actionPerformed(ActionEvent e) {
        if (running) {
            move();
            checkFood();
            checkCollision();
        }
        repaint();
    }

    @Override
    public void keyPressed(KeyEvent e) {
        if (!running && gameOver && e.getKeyCode() == KeyEvent.VK_SPACE) {
            startGame();
        }

        switch (e.getKeyCode()) {
            case KeyEvent.VK_LEFT: if (direction != 'R') direction = 'L'; break;
            case KeyEvent.VK_RIGHT: if (direction != 'L') direction = 'R'; break;
            case KeyEvent.VK_UP: if (direction != 'D') direction = 'U'; break;
            case KeyEvent.VK_DOWN: if (direction != 'U') direction = 'D'; break;
        }
    }

    @Override public void keyReleased(KeyEvent e) {}
    @Override public void keyTyped(KeyEvent e) {}

    public static void main(String[] args) {
        JFrame frame = new JFrame("Snake Game");
        SnakeGame game = new SnakeGame();
        frame.add(game);
        frame.pack();
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        frame.setVisible(true);
    }
}
