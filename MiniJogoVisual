import javax.swing.*;
import java.awt.*;
import java.awt.event.*;
import java.util.HashMap;

public class MiniJogoVisual extends JPanel implements KeyListener, Runnable {

    // Variáveis do jogador - sincronizadas
    private volatile int x = 50, y = 50;
    private final int VELOCIDADE = 5;
    private final int LARGURA_JOGADOR = 30, ALTURA_JOGADOR = 30;

    // Estado do jogo - sincronizado
    private volatile int pontuacao = 0;
    private volatile int nivel = 1;
    private volatile boolean jogoFinalizado = false;
    private volatile boolean efeitoMoeda = false;
    private volatile int contadorEfeito = 0;

    // Controles
    private final HashMap<Integer, Boolean> teclas = new HashMap<>();

    // Elementos do jogo - sincronizados
    private volatile Rectangle[] obstaculos = new Rectangle[0];
    private volatile Rectangle[] moedas = new Rectangle[0];
    private volatile boolean[] coletadas = new boolean[0];
    private volatile Rectangle[] inimigos = new Rectangle[0];

    // Dimensões da tela
    private static final int LARGURA_TELA = 600;
    private static final int ALTURA_TELA = 600;

    public MiniJogoVisual() {
        // Configuração inicial das teclas
        teclas.put(KeyEvent.VK_W, false);
        teclas.put(KeyEvent.VK_A, false);
        teclas.put(KeyEvent.VK_S, false);
        teclas.put(KeyEvent.VK_D, false);

        // Configurações do painel
        setFocusable(true);
        setPreferredSize(new Dimension(LARGURA_TELA, ALTURA_TELA));
        addKeyListener(this); // KeyListener no painel, não no frame

        // Carregar nível inicial
        carregarNivel(nivel);
    }

    private synchronized void carregarNivel(int nivelAtual) {
        switch (nivelAtual) {
            case 1 -> { // Nível 1: Tutorial - Básico
                obstaculos = new Rectangle[]{
                        new Rectangle(150, 100, 100, 20),
                        new Rectangle(300, 200, 20, 150),
                        new Rectangle(100, 300, 200, 20)
                };
                moedas = new Rectangle[]{
                        new Rectangle(500, 50, 20, 20),
                        new Rectangle(50, 500, 20, 20)
                };
                coletadas = new boolean[moedas.length];
                inimigos = new Rectangle[]{new Rectangle(400, 400, 30, 30)};
                // Posição inicial padrão
                x = 50;
                y = 50;
            }
            case 2 -> { // Nível 2: Labirinto Simples
                obstaculos = new Rectangle[]{
                        new Rectangle(100, 100, 400, 20),
                        new Rectangle(100, 200, 20, 300),
                        new Rectangle(200, 400, 300, 20)
                };
                moedas = new Rectangle[]{
                        new Rectangle(550, 50, 20, 20),
                        new Rectangle(50, 550, 20, 20),
                        new Rectangle(300, 300, 20, 20)
                };
                coletadas = new boolean[moedas.length];
                inimigos = new Rectangle[]{
                        new Rectangle(500, 500, 30, 30),
                        new Rectangle(250, 250, 30, 30)
                };
                // Posição inicial padrão
                x = 50;
                y = 50;
            }
            case 3 -> { // Nível 3: Cruz Central
                obstaculos = new Rectangle[]{
                        new Rectangle(0, 250, 200, 100),
                        new Rectangle(250, 0, 100, 200),
                        new Rectangle(250, 350, 100, 250),
                        new Rectangle(400, 250, 200, 100)
                };
                moedas = new Rectangle[]{
                        new Rectangle(50, 50, 20, 20),
                        new Rectangle(530, 50, 20, 20),
                        new Rectangle(50, 530, 20, 20),
                        new Rectangle(530, 530, 20, 20)
                };
                coletadas = new boolean[moedas.length];
                inimigos = new Rectangle[]{
                        new Rectangle(300, 300, 30, 30),
                        new Rectangle(100, 100, 30, 30),
                        new Rectangle(450, 450, 30, 30)
                };
                // Posição inicial padrão
                x = 50;
                y = 50;
            }
            case 4 -> { // Nível 4: Corredor em Zigue-Zague
                obstaculos = new Rectangle[]{
                        // Paredes superior e inferior
                        new Rectangle(0, 0, 600, 80),
                        new Rectangle(0, 520, 600, 80),

                        // Zigue-zague - primeira seção
                        new Rectangle(0, 80, 50, 200),
                        new Rectangle(150, 80, 300, 50),

                        // Zigue-zague - segunda seção
                        new Rectangle(550, 130, 50, 200),
                        new Rectangle(150, 230, 300, 50),

                        // Zigue-zague - terceira seção
                        new Rectangle(0, 280, 50, 200),
                        new Rectangle(150, 330, 300, 50),

                        // Zigue-zague - quarta seção
                        new Rectangle(550, 380, 50, 140)
                };
                moedas = new Rectangle[]{
                        new Rectangle(75, 100, 20, 20),
                        new Rectangle(525, 150, 20, 20),
                        new Rectangle(75, 250, 20, 20),
                        new Rectangle(525, 300, 20, 20),
                        new Rectangle(75, 400, 20, 20),
                        new Rectangle(300, 480, 20, 20)
                };
                coletadas = new boolean[moedas.length];
                inimigos = new Rectangle[]{
                        new Rectangle(100, 180, 25, 25),
                        new Rectangle(475, 200, 25, 25),
                        new Rectangle(100, 350, 25, 25),
                        new Rectangle(475, 450, 25, 25)
                };
                // Posição inicial específica para o nível 4 (área livre no início)
                x = 75;
                y = 90;
            }
            case 5 -> { // Nível 5: Labirinto Complexo
                obstaculos = new Rectangle[]{
                        new Rectangle(0, 0, 200, 50),
                        new Rectangle(250, 0, 350, 50),
                        new Rectangle(0, 100, 50, 150),
                        new Rectangle(100, 100, 100, 50),
                        new Rectangle(250, 100, 50, 100),
                        new Rectangle(350, 100, 250, 50),
                        new Rectangle(150, 200, 100, 50),
                        new Rectangle(350, 200, 50, 150),
                        new Rectangle(450, 200, 150, 50),
                        new Rectangle(0, 300, 150, 50),
                        new Rectangle(200, 300, 100, 50),
                        new Rectangle(450, 300, 50, 100),
                        new Rectangle(550, 300, 50, 300),
                        new Rectangle(0, 400, 50, 200),
                        new Rectangle(100, 400, 200, 50),
                        new Rectangle(100, 500, 400, 100)
                };
                moedas = new Rectangle[]{
                        new Rectangle(570, 50, 20, 20),
                        new Rectangle(75, 75, 20, 20),
                        new Rectangle(325, 75, 20, 20),
                        new Rectangle(575, 175, 20, 20),
                        new Rectangle(175, 175, 20, 20),
                        new Rectangle(25, 275, 20, 20),
                        new Rectangle(525, 275, 20, 20)
                };
                coletadas = new boolean[moedas.length];
                inimigos = new Rectangle[]{
                        new Rectangle(225, 75, 25, 25),
                        new Rectangle(75, 275, 25, 25),
                        new Rectangle(325, 275, 25, 25),
                        new Rectangle(125, 375, 25, 25),
                        new Rectangle(425, 375, 25, 25)
                };
                // Posição inicial padrão
                x = 50;
                y = 50;
            }
            case 6 -> { // Nível 6: Sala dos Inimigos
                obstaculos = new Rectangle[]{
                        new Rectangle(0, 0, 100, 200),
                        new Rectangle(150, 0, 300, 100),
                        new Rectangle(500, 0, 100, 200),
                        new Rectangle(0, 250, 200, 100),
                        new Rectangle(250, 250, 100, 100),
                        new Rectangle(400, 250, 200, 100),
                        new Rectangle(0, 400, 150, 200),
                        new Rectangle(200, 500, 200, 100),
                        new Rectangle(450, 400, 150, 200)
                };
                moedas = new Rectangle[]{
                        new Rectangle(125, 125, 20, 20),
                        new Rectangle(475, 125, 20, 20),
                        new Rectangle(50, 375, 20, 20),
                        new Rectangle(550, 375, 20, 20),
                        new Rectangle(300, 475, 20, 20),
                        new Rectangle(300, 200, 20, 20)
                };
                coletadas = new boolean[moedas.length];
                inimigos = new Rectangle[]{
                        new Rectangle(125, 200, 25, 25),
                        new Rectangle(475, 200, 25, 25),
                        new Rectangle(225, 375, 25, 25),
                        new Rectangle(375, 375, 25, 25),
                        new Rectangle(300, 125, 25, 25),
                        new Rectangle(300, 425, 25, 25),
                        new Rectangle(175, 300, 25, 25)
                };
                // Posição inicial específica (área livre)
                x = 125;
                y = 225;
            }
            case 7 -> { // Nível 7: Círculos Concêntricos
                obstaculos = new Rectangle[]{
                        // Anel externo
                        new Rectangle(50, 50, 500, 50),
                        new Rectangle(50, 500, 500, 50),
                        new Rectangle(50, 50, 50, 500),
                        new Rectangle(500, 50, 50, 500),
                        // Anel médio
                        new Rectangle(150, 150, 300, 50),
                        new Rectangle(150, 400, 300, 50),
                        new Rectangle(150, 150, 50, 300),
                        new Rectangle(400, 150, 50, 300),
                        // Centro
                        new Rectangle(250, 250, 100, 100)
                };
                moedas = new Rectangle[]{
                        new Rectangle(25, 25, 20, 20),
                        new Rectangle(555, 25, 20, 20),
                        new Rectangle(25, 555, 20, 20),
                        new Rectangle(555, 555, 20, 20),
                        new Rectangle(125, 300, 20, 20),
                        new Rectangle(455, 300, 20, 20),
                        new Rectangle(300, 125, 20, 20),
                        new Rectangle(300, 455, 20, 20)
                };
                coletadas = new boolean[moedas.length];
                inimigos = new Rectangle[]{
                        new Rectangle(75, 300, 25, 25),
                        new Rectangle(525, 300, 25, 25),
                        new Rectangle(300, 75, 25, 25),
                        new Rectangle(300, 525, 25, 25),
                        new Rectangle(225, 225, 25, 25),
                        new Rectangle(375, 375, 25, 25)
                };
                // Posição inicial padrão
                x = 25;
                y = 25;
            }
            case 8 -> { // Nível 8: O Grande Desafio
                obstaculos = new Rectangle[]{
                        new Rectangle(0, 100, 100, 20),
                        new Rectangle(150, 0, 20, 150),
                        new Rectangle(200, 100, 150, 20),
                        new Rectangle(400, 0, 20, 150),
                        new Rectangle(450, 100, 150, 20),

                        new Rectangle(50, 200, 100, 20),
                        new Rectangle(200, 150, 20, 100),
                        new Rectangle(250, 200, 100, 20),
                        new Rectangle(400, 150, 20, 100),
                        new Rectangle(450, 200, 100, 20),

                        new Rectangle(0, 300, 150, 20),
                        new Rectangle(200, 250, 20, 100),
                        new Rectangle(250, 300, 100, 20),
                        new Rectangle(400, 250, 20, 100),
                        new Rectangle(450, 300, 150, 20),

                        new Rectangle(100, 400, 100, 20),
                        new Rectangle(250, 350, 20, 100),
                        new Rectangle(300, 400, 50, 20),
                        new Rectangle(400, 350, 20, 100),
                        new Rectangle(450, 400, 100, 20),

                        new Rectangle(0, 500, 200, 20),
                        new Rectangle(250, 450, 20, 100),
                        new Rectangle(300, 500, 50, 20),
                        new Rectangle(400, 450, 20, 100),
                        new Rectangle(450, 500, 150, 20)
                };
                moedas = new Rectangle[]{
                        new Rectangle(25, 50, 20, 20),
                        new Rectangle(575, 50, 20, 20),
                        new Rectangle(125, 175, 20, 20),
                        new Rectangle(475, 175, 20, 20),
                        new Rectangle(25, 275, 20, 20),
                        new Rectangle(575, 275, 20, 20),
                        new Rectangle(175, 375, 20, 20),
                        new Rectangle(425, 375, 20, 20),
                        new Rectangle(325, 475, 20, 20),
                        new Rectangle(300, 525, 20, 20)
                };
                coletadas = new boolean[moedas.length];
                inimigos = new Rectangle[]{
                        new Rectangle(75, 125, 25, 25),
                        new Rectangle(525, 125, 25, 25),
                        new Rectangle(175, 225, 25, 25),
                        new Rectangle(425, 225, 25, 25),
                        new Rectangle(125, 325, 25, 25),
                        new Rectangle(475, 325, 25, 25),
                        new Rectangle(225, 425, 25, 25),
                        new Rectangle(375, 425, 25, 25),
                        new Rectangle(325, 525, 25, 25)
                };
                // Posição inicial padrão
                x = 50;
                y = 50;
            }
            default -> {
                // Finalizar jogo se não há mais níveis
                jogoFinalizado = true;
                return;
            }
        }

        efeitoMoeda = false;
        contadorEfeito = 0;
    }

    @Override
    protected void paintComponent(Graphics g) {
        super.paintComponent(g);

        // Usar Graphics2D para melhor qualidade
        Graphics2D g2d = (Graphics2D) g;
        g2d.setRenderingHint(RenderingHints.KEY_ANTIALIASING, RenderingHints.VALUE_ANTIALIAS_ON);

        // Fundo
        g2d.setColor(Color.LIGHT_GRAY);
        g2d.fillRect(0, 0, getWidth(), getHeight());

        // Copiar referências para evitar mudanças durante renderização
        Rectangle[] obsLocal = obstaculos;
        Rectangle[] moedasLocal = moedas;
        boolean[] coletadasLocal = coletadas;
        Rectangle[] inimigosLocal = inimigos;

        // Obstáculos
        g2d.setColor(Color.DARK_GRAY);
        for (Rectangle obs : obsLocal) {
            g2d.fillRect(obs.x, obs.y, obs.width, obs.height);
        }

        // Moedas com efeito de "brilho"
        for (int i = 0; i < moedasLocal.length && i < coletadasLocal.length; i++) {
            if (!coletadasLocal[i]) {
                if (efeitoMoeda && contadorEfeito % 2 == 0) {
                    g2d.setColor(Color.ORANGE);
                } else {
                    g2d.setColor(Color.YELLOW);
                }
                g2d.fillOval(moedasLocal[i].x, moedasLocal[i].y, moedasLocal[i].width, moedasLocal[i].height);

                // Borda da moeda
                g2d.setColor(Color.BLACK);
                g2d.drawOval(moedasLocal[i].x, moedasLocal[i].y, moedasLocal[i].width, moedasLocal[i].height);
            }
        }

        // Jogador com efeito de borda
        g2d.setColor(Color.RED);
        g2d.fillOval(x, y, LARGURA_JOGADOR, ALTURA_JOGADOR);
        g2d.setColor(Color.BLACK);
        g2d.drawOval(x, y, LARGURA_JOGADOR, ALTURA_JOGADOR);

        // Inimigos com animação simples
        g2d.setColor(Color.BLACK);
        for (Rectangle inimigo : inimigosLocal) {
            g2d.fillOval(inimigo.x, inimigo.y, inimigo.width, inimigo.height);
            g2d.setColor(Color.RED);
            g2d.drawOval(inimigo.x, inimigo.y, inimigo.width, inimigo.height);
            g2d.setColor(Color.BLACK);
        }

        // Pontuação e nível
        g2d.setColor(Color.BLUE);
        g2d.setFont(new Font("Arial", Font.BOLD, 16));
        g2d.drawString("Pontuação: " + pontuacao + " | Nível: " + nivel + "/8", 10, 20);

        // Mostrar progresso do nível
        if (moedasLocal.length > 0) {
            int moedasColetadas = 0;
            for (boolean coletada : coletadasLocal) {
                if (coletada) moedasColetadas++;
            }
            g2d.setColor(Color.DARK_GRAY);
            g2d.setFont(new Font("Arial", Font.PLAIN, 12));
            g2d.drawString("Moedas: " + moedasColetadas + "/" + moedasLocal.length, 10, 40);
        }

        // Controles
        g2d.setColor(Color.DARK_GRAY);
        g2d.setFont(new Font("Arial", Font.PLAIN, 12));
        g2d.drawString("Use WASD para mover", 10, getHeight() - 40);
        g2d.drawString("Colete todas as moedas e evite os inimigos!", 10, getHeight() - 25);

        // Dica específica do nível
        String dica = obterDicaNivel(nivel);
        if (!dica.isEmpty()) {
            g2d.setColor(Color.ORANGE);
            g2d.drawString(dica, 10, getHeight() - 10);
        }

        // Mensagem final
        if (jogoFinalizado) {
            g2d.setFont(new Font("Arial", Font.BOLD, 24));
            g2d.setColor(Color.MAGENTA);
            String mensagem = "Parabéns! Você completou todos os níveis!";
            FontMetrics fm = g2d.getFontMetrics();
            int larguraTexto = fm.stringWidth(mensagem);
            g2d.drawString(mensagem, (getWidth() - larguraTexto) / 2, getHeight() / 2);

            g2d.setFont(new Font("Arial", Font.PLAIN, 16));
            g2d.setColor(Color.BLACK);
            String reiniciar = "Pressione R para reiniciar";
            int larguraReiniciar = g2d.getFontMetrics().stringWidth(reiniciar);
            g2d.drawString(reiniciar, (getWidth() - larguraReiniciar) / 2, getHeight() / 2 + 40);
        }
    }

    private String obterDicaNivel(int nivel) {
        return switch (nivel) {
            case 1 -> "Tutorial: Aprenda os controles básicos";
            case 2 -> "Cuidado com os cantos do labirinto!";
            case 3 -> "Use o centro como estratégia";
            case 4 -> "Zigue-zague - mova com cuidado!";
            case 5 -> "Labirinto complexo - planeje sua rota";
            case 6 -> "Muitos inimigos - seja cauteloso!";
            case 7 -> "Círculos concêntricos - ache o padrão";
            case 8 -> "Desafio final - boa sorte!";
            default -> "";
        };
    }

    private synchronized void atualizarPosicao() {
        if (jogoFinalizado) return;

        int novaX = x;
        int novaY = y;

        // Movimentação do jogador
        if (teclas.get(KeyEvent.VK_W)) novaY -= VELOCIDADE;
        if (teclas.get(KeyEvent.VK_S)) novaY += VELOCIDADE;
        if (teclas.get(KeyEvent.VK_A)) novaX -= VELOCIDADE;
        if (teclas.get(KeyEvent.VK_D)) novaX += VELOCIDADE;

        // Verificação de limites da tela
        novaX = Math.max(0, Math.min(novaX, LARGURA_TELA - LARGURA_JOGADOR));
        novaY = Math.max(0, Math.min(novaY, ALTURA_TELA - ALTURA_JOGADOR));

        Rectangle jogadorNovo = new Rectangle(novaX, novaY, LARGURA_JOGADOR, ALTURA_JOGADOR);

        // Verificar colisão com obstáculos
        boolean colisao = false;
        for (Rectangle obs : obstaculos) {
            if (jogadorNovo.intersects(obs)) {
                colisao = true;
                break;
            }
        }

        // Atualizar posição se não houver colisão
        if (!colisao) {
            x = novaX;
            y = novaY;
        }

        Rectangle jogadorAtual = new Rectangle(x, y, LARGURA_JOGADOR, ALTURA_JOGADOR);

        // Coleta de moedas com efeito
        for (int i = 0; i < moedas.length; i++) {
            if (!coletadas[i] && jogadorAtual.intersects(moedas[i])) {
                coletadas[i] = true;
                pontuacao++;
                efeitoMoeda = true;
                contadorEfeito = 20; // duração do efeito
            }
        }

        // Atualizar efeito da moeda
        if (contadorEfeito > 0) {
            contadorEfeito--;
        } else {
            efeitoMoeda = false;
        }

        // Movimentar inimigos suavemente em direção ao jogador
        for (Rectangle inimigo : inimigos) {
            double dx = x + LARGURA_JOGADOR/2.0 - (inimigo.x + inimigo.width/2.0);
            double dy = y + ALTURA_JOGADOR/2.0 - (inimigo.y + inimigo.height/2.0);
            double dist = Math.sqrt(dx*dx + dy*dy);

            if (dist > 0) {
                // Velocidade dos inimigos aumenta com o nível
                double velocidadeInimigo = 1.0 + (nivel * 0.2);
                inimigo.x += (int)(velocidadeInimigo * dx / dist);
                inimigo.y += (int)(velocidadeInimigo * dy / dist);
            }

            // Verificar colisão com inimigo
            if (inimigo.intersects(jogadorAtual)) {
                jogoFinalizado = true;
                return;
            }
        }

        // Verificar se todas as moedas foram coletadas
        boolean todasColetadas = true;
        for (boolean c : coletadas) {
            if (!c) {
                todasColetadas = false;
                break;
            }
        }

        // Passar para o próximo nível
        if (todasColetadas) {
            nivel++;
            carregarNivel(nivel);
        }
    }

    private synchronized void reiniciarJogo() {
        nivel = 1;
        pontuacao = 0;
        jogoFinalizado = false;
        efeitoMoeda = false;
        contadorEfeito = 0;
        carregarNivel(nivel);
    }

    @Override
    public void keyPressed(KeyEvent e) {
        if (teclas.containsKey(e.getKeyCode())) {
            teclas.put(e.getKeyCode(), true);
        }

        // Reiniciar jogo
        if (e.getKeyCode() == KeyEvent.VK_R && jogoFinalizado) {
            reiniciarJogo();
        }
    }

    @Override
    public void keyReleased(KeyEvent e) {
        if (teclas.containsKey(e.getKeyCode())) {
            teclas.put(e.getKeyCode(), false);
        }
    }

    @Override
    public void keyTyped(KeyEvent e) {}

    @Override
    public void run() {
        while (!Thread.currentThread().isInterrupted()) {
            atualizarPosicao();

            // Usar SwingUtilities para thread safety na GUI
            SwingUtilities.invokeLater(this::repaint);

            try {
                Thread.sleep(16); // ~60 FPS
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
                break;
            }
        }
    }

    public static void main(String[] args) {
        SwingUtilities.invokeLater(() -> {
            JFrame frame = new JFrame("Mini-Jogo Visual");
            MiniJogoVisual jogo = new MiniJogoVisual();

            frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
            frame.add(jogo);
            frame.setResizable(false);
            frame.pack(); // Ajusta o tamanho baseado no preferredSize
            frame.setLocationRelativeTo(null); // Centraliza na tela
            frame.setVisible(true);

            // Focar no painel para capturar teclas
            jogo.requestFocusInWindow();

            // Iniciar thread do jogo
            Thread gameThread = new Thread(jogo);
            gameThread.setDaemon(true);
            gameThread.start();
        });
    }
}
