# Projetos
import java.time.LocalDate;
import java.time.LocalTime;
import java.time.ZoneId;
import java.time.ZonedDateTime;
import java.time.format.DateTimeFormatter;
import java.time.format.DateTimeParseException;
import java.util.Locale;
import java.util.Scanner;

public class RelogioDigital {

    static Scanner entrada = new Scanner(System.in);

    public static void main(String[] args) {

        int opcao = -1;

        do {
            mostrarCabecalho();
            mostrarMenu();

            System.out.print("Escolha uma opção: ");

            try {
                opcao = Integer.parseInt(entrada.nextLine());

                switch (opcao) {

                    case 1:
                        mostrarHoraAtual();
                        break;

                    case 2:
                        mostrarDataHora();
                        break;

                    case 3:
                        iniciarRelogio();
                        break;

                    case 4:
                        iniciarCronometro();
                        break;

                    case 5:
                        configurarAlarme();
                        break;

                    case 6:
                        mostrarInformacoes();
                        break;

                    case 7:
                        mostrarDiaSemana();
                        break;

                    case 8:
                        iniciarTemporizador();
                        break;

                    case 9:
                        mostrarHorarioMundial();
                        break;

                    case 10:
                        horarioMundialTempoReal();
                        break;

                    case 11:
                        escolherFormatoHora();
                        break;

                    case 0:
                        System.out.println();
                        System.out.println("Encerrando o relógio...");
                        System.out.println("Até mais!");
                        break;

                    default:
                        System.out.println();
                        System.out.println("Opção inválida!");
                        esperar();
                }

            } catch (NumberFormatException e) {
                System.out.println();
                System.out.println("ERRO: Digite apenas números.");
                esperar();
            }

        } while (opcao != 0);

        entrada.close();
    }

    // =========================================
    // CABEÇALHO
    // =========================================

    public static void mostrarCabecalho() {

        System.out.println();
        System.out.println("==========================================");
        System.out.println("             RELÓGIO DIGITAL");
        System.out.println("==========================================");
        System.out.println();
    }

    // =========================================
    // MENU
    // =========================================

    public static void mostrarMenu() {

        System.out.println("1  - Mostrar hora atual");
        System.out.println("2  - Mostrar data e hora");
        System.out.println("3  - Relógio em tempo real");
        System.out.println("4  - Cronômetro");
        System.out.println("5  - Configurar alarme");
        System.out.println("6  - Informações do sistema");
        System.out.println("7  - Mostrar dia da semana");
        System.out.println("8  - Temporizador");
        System.out.println("9  - Horário mundial");
        System.out.println("10 - Horário mundial em tempo real");
        System.out.println("11 - Formato 12h / 24h");
        System.out.println("0  - Sair");
        System.out.println();
    }

    // =========================================
    // HORA ATUAL
    // =========================================

    public static void mostrarHoraAtual() {

        LocalTime hora = LocalTime.now();

        DateTimeFormatter formato =
                DateTimeFormatter.ofPattern("HH:mm:ss");

        System.out.println();
        System.out.println("==========================================");
        System.out.println("Hora atual: " + hora.format(formato));
        System.out.println("==========================================");

        esperar();
    }

    // =========================================
    // DATA E HORA
    // =========================================

    public static void mostrarDataHora() {

        LocalDate data = LocalDate.now();
        LocalTime hora = LocalTime.now();

        DateTimeFormatter formatoData =
                DateTimeFormatter.ofPattern("dd/MM/yyyy");

        DateTimeFormatter formatoHora =
                DateTimeFormatter.ofPattern("HH:mm:ss");

        System.out.println();
        System.out.println("==========================================");
        System.out.println("          DATA E HORA DO SISTEMA");
        System.out.println("==========================================");

        System.out.println("Data: " + data.format(formatoData));
        System.out.println("Hora: " + hora.format(formatoHora));

        System.out.println("==========================================");

        esperar();
    }

    // =========================================
    // RELÓGIO EM TEMPO REAL
    // =========================================

    public static void iniciarRelogio() {

        System.out.println();
        System.out.println("==========================================");
        System.out.println("          RELÓGIO EM TEMPO REAL");
        System.out.println("==========================================");
        System.out.println("Pressione ENTER para sair.");
        System.out.println();

        Thread relogio = new Thread(() -> {

            DateTimeFormatter formato =
                    DateTimeFormatter.ofPattern("HH:mm:ss");

            try {

                while (!Thread.currentThread().isInterrupted()) {

                    LocalTime hora = LocalTime.now();

                    System.out.print(
                            "\rHora atual: " + hora.format(formato)
                    );

                    Thread.sleep(1000);
                }

            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
            }
        });

        relogio.start();

        entrada.nextLine();

        relogio.interrupt();

        try {
            relogio.join();
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
        }

        System.out.println();
        System.out.println("Relógio encerrado.");

        esperar();
    }

    // =========================================
    // CRONÔMETRO
    // =========================================

    public static void iniciarCronometro() {

        System.out.println();
        System.out.println("==========================================");
        System.out.println("                CRONÔMETRO");
        System.out.println("==========================================");

        System.out.println("Pressione ENTER para iniciar.");
        entrada.nextLine();

        long inicio = System.currentTimeMillis();

        System.out.println();
        System.out.println("Cronômetro iniciado!");
        System.out.println("Pressione ENTER para parar.");

        entrada.nextLine();

        long fim = System.currentTimeMillis();

        long tempoTotal = (fim - inicio) / 1000;

        long horas = tempoTotal / 3600;
        long minutos = (tempoTotal % 3600) / 60;
        long segundos = tempoTotal % 60;

        System.out.println();

        System.out.printf(
                "Tempo decorrido: %02d:%02d:%02d%n",
                horas,
                minutos,
                segundos
        );

        esperar();
    }

    // =========================================
    // ALARME
    // =========================================

    public static void configurarAlarme() {

        System.out.println();
        System.out.println("==========================================");
        System.out.println("             CONFIGURAR ALARME");
        System.out.println("==========================================");

        System.out.print("Digite o horário do alarme (HH:mm): ");

        String horario = entrada.nextLine();

        DateTimeFormatter formato =
                DateTimeFormatter.ofPattern("HH:mm");

        try {

            LocalTime horaAlarme =
                    LocalTime.parse(horario, formato);

            System.out.println();
            System.out.println(
                    "Alarme configurado para: "
                            + horaAlarme.format(formato)
            );

            System.out.println("Aguardando o horário...");
            System.out.println();

            while (true) {

                LocalTime agora = LocalTime.now();

                System.out.print(
                        "\rHora atual: "
                                + agora.format(
                                DateTimeFormatter.ofPattern("HH:mm:ss")
                        )
                );

                if (agora.getHour() == horaAlarme.getHour()
                        && agora.getMinute() == horaAlarme.getMinute()) {

                    System.out.println();
                    System.out.println();
                    System.out.println("==========================================");
                    System.out.println("                 ALARME!");
                    System.out.println("==========================================");
                    System.out.println(
                            "Horário: " + agora.format(formato)
                    );
                    System.out.println("==========================================");

                    break;
                }

                Thread.sleep(1000);
            }

        } catch (DateTimeParseException e) {

            System.out.println();
            System.out.println("Horário inválido!");
            System.out.println("Use o formato HH:mm.");
            System.out.println("Exemplo: 14:30");

        } catch (InterruptedException e) {

            System.out.println();
            System.out.println("Alarme interrompido.");
            Thread.currentThread().interrupt();
        }

        esperar();
    }

    // =========================================
    // INFORMAÇÕES
    // =========================================

    public static void mostrarInformacoes() {

        System.out.println();
        System.out.println("==========================================");
        System.out.println("          INFORMAÇÕES DO SISTEMA");
        System.out.println("==========================================");

        System.out.println("Sistema: Relógio Digital");
        System.out.println("Linguagem: Java");
        System.out.println("Versão: 4.0");
        System.out.println("Formato da hora: HH:mm:ss");
        System.out.println("Formato da data: dd/MM/yyyy");

        System.out.println();
        System.out.println("Funções:");
        System.out.println("- Hora atual");
        System.out.println("- Data e hora");
        System.out.println("- Relógio em tempo real");
        System.out.println("- Cronômetro");
        System.out.println("- Alarme");
        System.out.println("- Dia da semana");
        System.out.println("- Temporizador");
        System.out.println("- Horário mundial");
        System.out.println("- Horário mundial em tempo real");
        System.out.println("- Formato 12h e 24h");

        esperar();
    }

    // =========================================
    // DIA DA SEMANA
    // =========================================

    public static void mostrarDiaSemana() {

        LocalDate hoje = LocalDate.now();

        Locale brasil = new Locale("pt", "BR");

        DateTimeFormatter formato =
                DateTimeFormatter.ofPattern(
                        "EEEE, dd 'de' MMMM 'de' yyyy",
                        brasil
                );

        System.out.println();
        System.out.println("==========================================");
        System.out.println("              DIA DA SEMANA");
        System.out.println("==========================================");

        System.out.println(hoje.format(formato));

        esperar();
    }

    // =========================================
    // TEMPORIZADOR
    // =========================================

    public static void iniciarTemporizador() {

        System.out.println();
        System.out.println("==========================================");
        System.out.println("               TEMPORIZADOR");
        System.out.println("==========================================");

        try {

            System.out.print("Digite a quantidade de segundos: ");

            int segundos =
                    Integer.parseInt(entrada.nextLine());

            if (segundos < 0) {

                System.out.println("Digite um número positivo!");
                esperar();
                return;
            }

            while (segundos >= 0) {

                int horas = segundos / 3600;
                int minutos = (segundos % 3600) / 60;
                int segundosRestantes = segundos % 60;

                System.out.printf(
                        "\rTempo restante: %02d:%02d:%02d",
                        horas,
                        minutos,
                        segundosRestantes
                );

                Thread.sleep(1000);

                segundos--;
            }

            System.out.println();
            System.out.println();
            System.out.println("==========================================");
            System.out.println("              TEMPO ESGOTADO!");
            System.out.println("==========================================");

        } catch (NumberFormatException e) {

            System.out.println();
            System.out.println("ERRO: Digite apenas números.");

        } catch (InterruptedException e) {

            System.out.println();
            System.out.println("Temporizador interrompido.");
            Thread.currentThread().interrupt();
        }

        esperar();
    }

    // =========================================
    // HORÁRIO MUNDIAL
    // =========================================

    public static void mostrarHorarioMundial() {

        DateTimeFormatter formato =
                DateTimeFormatter.ofPattern("HH:mm:ss");

        ZonedDateTime brasil =
                ZonedDateTime.now(
                        ZoneId.of("America/Sao_Paulo")
                );

        ZonedDateTime novaYork =
                ZonedDateTime.now(
                        ZoneId.of("America/New_York")
                );

        ZonedDateTime londres =
                ZonedDateTime.now(
                        ZoneId.of("Europe/London")
                );

        ZonedDateTime paris =
                ZonedDateTime.now(
                        ZoneId.of("Europe/Paris")
                );

        ZonedDateTime toquio =
                ZonedDateTime.now(
                        ZoneId.of("Asia/Tokyo")
                );

        ZonedDateTime dubai =
                ZonedDateTime.now(
                        ZoneId.of("Asia/Dubai")
                );

        ZonedDateTime sydney =
                ZonedDateTime.now(
                        ZoneId.of("Australia/Sydney")
                );

        System.out.println();
        System.out.println("==========================================");
        System.out.println("              HORÁRIO MUNDIAL");
        System.out.println("==========================================");

        System.out.println(
                "Brasil - São Paulo:    " + brasil.format(formato)
        );

        System.out.println(
                "EUA - Nova York:       " + novaYork.format(formato)
        );

        System.out.println(
                "Reino Unido - Londres: " + londres.format(formato)
        );

        System.out.println(
                "França - Paris:        " + paris.format(formato)
        );

        System.out.println(
                "Japão - Tóquio:        " + toquio.format(formato)
        );

        System.out.println(
                "Emirados - Dubai:      " + dubai.format(formato)
        );

        System.out.println(
                "Austrália - Sydney:    " + sydney.format(formato)
        );

        System.out.println("==========================================");

        esperar();
    }

    // =========================================
    // HORÁRIO MUNDIAL EM TEMPO REAL
    // =========================================

    public static void horarioMundialTempoReal() {

        System.out.println();
        System.out.println("==========================================");
        System.out.println("       HORÁRIO MUNDIAL EM TEMPO REAL");
        System.out.println("==========================================");
        System.out.println("Pressione ENTER para sair.");
        System.out.println();

        DateTimeFormatter formato =
                DateTimeFormatter.ofPattern("HH:mm:ss");

        Thread relogioMundial = new Thread(() -> {

            try {

                while (!Thread.currentThread().isInterrupted()) {

                    String brasil =
                            ZonedDateTime.now(
                                    ZoneId.of("America/Sao_Paulo")
                            ).format(formato);

                    String novaYork =
                            ZonedDateTime.now(
                                    ZoneId.of("America/New_York")
                            ).format(formato);

                    String londres =
                            ZonedDateTime.now(
                                    ZoneId.of("Europe/London")
                            ).format(formato);

                    String paris =
                            ZonedDateTime.now(
                                    ZoneId.of("Europe/Paris")
                            ).format(formato);

                    String toquio =
                            ZonedDateTime.now(
                                    ZoneId.of("Asia/Tokyo")
                            ).format(formato);

                    String dubai =
                            ZonedDateTime.now(
                                    ZoneId.of("Asia/Dubai")
                            ).format(formato);

                    String sydney =
                            ZonedDateTime.now(
                                    ZoneId.of("Australia/Sydney")
                            ).format(formato);

                    System.out.print(
                            "\rSão Paulo: " + brasil
                                    + " | Nova York: " + novaYork
                                    + " | Londres: " + londres
                                    + " | Paris: " + paris
                                    + " | Tóquio: " + toquio
                                    + " | Dubai: " + dubai
                                    + " | Sydney: " + sydney
                    );

                    Thread.sleep(1000);
                }

            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
            }
        });

        relogioMundial.start();

        entrada.nextLine();

        relogioMundial.interrupt();

        try {
            relogioMundial.join();
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
        }

        System.out.println();
        System.out.println("Relógio mundial encerrado.");

        esperar();
    }

    // =========================================
    // FORMATO 12H / 24H
    // =========================================

    public static void escolherFormatoHora() {

        System.out.println();
        System.out.println("==========================================");
        System.out.println("            FORMATO DA HORA");
        System.out.println("==========================================");

        System.out.println("1 - Formato 24 horas");
        System.out.println("2 - Formato 12 horas");

        System.out.println();
        System.out.print("Escolha uma opção: ");

        try {

            int escolha =
                    Integer.parseInt(entrada.nextLine());

            LocalTime agora = LocalTime.now();

            if (escolha == 1) {

                DateTimeFormatter formato24 =
                        DateTimeFormatter.ofPattern("HH:mm:ss");

                System.out.println();
                System.out.println(
                        "Hora no formato 24h: "
                                + agora.format(formato24)
                );

            } else if (escolha == 2) {

                DateTimeFormatter formato12 =
                        DateTimeFormatter.ofPattern("hh:mm:ss a");

                System.out.println();
                System.out.println(
                        "Hora no formato 12h: "
                                + agora.format(formato12)
                );

            } else {

                System.out.println();
                System.out.println("Opção inválida!");
            }

        } catch (NumberFormatException e) {

            System.out.println();
            System.out.println("Digite apenas 1 ou 2.");
        }

        esperar();
    }

    // =========================================
    // ESPERAR
    // =========================================

    public static void esperar() {

        System.out.println();
        System.out.println("Pressione ENTER para continuar...");
        entrada.nextLine();
    }
}
