# LAB_5
import java.util.Scanner;
import java.util.ArrayList;
import java.util.List;

class BST<K extends Comparable<K>, V> {
    private class Nodo {
        K clave;
        V valor;
        Nodo izquierdo, de
        public Nodo(K clave, V valor) {
            this.clave = clave;
            this.valor = valor;
        }
    }
    private Nodo raiz;
    public void insertar(K clave, V valor) {
        raiz = insertar(raiz, clave, valor);
    }
    private Nodo insertar(Nodo nodo, K clave, V valor) {
        if (nodo == null) return new Nodo(clave, valor);
        int cmp = clave.compareTo(nodo.clave);
        if (cmp < 0) nodo.izquierdo = insertar(nodo.izquierdo, clave, valor);
        else if (cmp > 0) nodo.derecho = insertar(nodo.derecho, clave, valor);
        else nodo.valor = valor;
        return nodo;
    }
    public V obtener(K clave) {
        return obtener(raiz, clave);
    }
    private V obtener(Nodo nodo, K clave) {
        if (nodo == null) return null;
        int cmp = clave.compareTo(nodo.clave);
        if (cmp < 0) return obtener(nodo.izquierdo, clave);
        else if (cmp > 0) return obtener(nodo.derecho, clave);
        else return nodo.valor;
    }
    public List<K> claves() {
        List<K> claves = new ArrayList<>();
        inorden(raiz, claves);
        return claves;
    }
    private void inorden(Nodo nodo, List<K> claves) {
        if (nodo == null) return;
        inorden(nodo.izquierdo, claves);
        claves.add(nodo.clave);
        inorden(nodo.derecho, claves);
    }
}

class HashST<K, V> {
    private class Nodo {
        K clave;
        V valor;   
        public Nodo(K clave, V valor) {
            this.clave = clave;
            this.valor = valor;
        }
    }
    private static final int CAPACIDAD_INICIAL = 16;
    private int tamaño;
    private int m;
    private List<Nodo>[] tabla;
    @SuppressWarnings("unchecked")
    public HashST() {
        this(CAPACIDAD_INICIAL);
    }
    @SuppressWarnings("unchecked")
    public HashST(int capacidad) {
        this.m = capacidad;
        this.tabla = (List<Nodo>[]) new List[capacidad];
        for (int i = 0; i < capacidad; i++)
            tabla[i] = new ArrayList<>();
    }
    private int hash(K clave) {
        return (clave.hashCode() & 0x7fffffff) % m;
    }
    public void insertar(K clave, V valor) {
        int i = hash(clave);
        for (Nodo nodo : tabla[i]) {
            if (clave.equals(nodo.clave)) {
                nodo.valor = valor;
                return;
            }
        }
        tabla[i].add(new Nodo(clave, valor));
        tamaño++;
    }
    public V obtener(K clave) {
        int i = hash(clave);
        for (Nodo nodo : tabla[i]) {
            if (clave.equals(nodo.clave))
                return nodo.valor;
        }
        return null;
    }
    public boolean contiene(K clave) {
        return obtener(clave) != null;
    }
    public int tamaño() {
        return tamaño;
    }
}

class Jugador {
    private String nombre_jugador;
    private int victorias;
    private int empates;
    private int derrotas;
    public Jugador(String nombre_jugador) {
        this.nombre_jugador = nombre_jugador;
    }
    public void agregar_victoria() { victorias++; }
    public void agregar_empate() { empates++; }
    public void agregar_derrota() { derrotas++; }
    public double tasa_victorias() {
        int total = victorias + empates + derrotas;
        return total == 0 ? 0 : (double)victorias / total;
    }
    public String get_nombre() { return nombre_jugador; }
    public int get_victorias() { return victorias; }
    public int get_empates() { return empates; }
    public int get_derrotas() { return derrotas; }
}

class Conecta_Cuatro {
    private char[][] tablero;
    private char simbolo_actual;
    public Conecta_Cuatro() {
        tablero = new char[7][6];
        for (int i = 0; i < 7; i++)
            for (int j = 0; j < 6; j++)
                tablero[i][j] = ' ';
        simbolo_actual = 'X';
    }
    public boolean hacer_movimiento(int columna) {
        if (columna < 0 || columna >= 7) return false;
        for (int fila = 5; fila >= 0; fila--) {
            if (tablero[columna][fila] == ' ') {
                tablero[columna][fila] = simbolo_actual;
                simbolo_actual = (simbolo_actual == 'X') ? 'O' : 'X';
                return true;
            }
        }
        return false;
    }
    public boolean juego_terminado() {
        return verificar_ganador('X') || verificar_ganador('O') || es_empate();
    }
    public boolean verificar_ganador(char simbolo) {
        for (int fila = 0; fila < 6; fila++) {
            for (int col = 0; col < 4; col++) {
                if (tablero[col][fila] == simbolo &&
                    tablero[col+1][fila] == simbolo &&
                    tablero[col+2][fila] == simbolo &&
                    tablero[col+3][fila] == simbolo)
                    return true;
            }
        }
        for (int col = 0; col < 7; col++) {
            for (int fila = 0; fila < 3; fila++) {
                if (tablero[col][fila] == simbolo &&
                    tablero[col][fila+1] == simbolo &&
                    tablero[col][fila+2] == simbolo &&
                    tablero[col][fila+3] == simbolo)
                    return true;
            }
        }
        for (int col = 0; col < 4; col++) {
            for (int fila = 0; fila < 3; fila++) {
                if (tablero[col][fila] == simbolo &&
                    tablero[col+1][fila+1] == simbolo &&
                    tablero[col+2][fila+2] == simbolo &&
                    tablero[col+3][fila+3] == simbolo)
                    return true;
            }
        }
        for (int col = 0; col < 4; col++) {
            for (int fila = 3; fila < 6; fila++) {
                if (tablero[col][fila] == simbolo &&
                    tablero[col+1][fila-1] == simbolo &&
                    tablero[col+2][fila-2] == simbolo &&
                    tablero[col+3][fila-3] == simbolo)
                    return true;
            }
        }
        return false;
    }
    private boolean es_empate() {
        for (int col = 0; col < 7; col++) {
            if (tablero[col][0] == ' ')
                return false;
        }
        return true;
    }
    public void mostrar_tablero() {
        System.out.println(" 0 1 2 3 4 5 6");
        System.out.println("---------------");
        for (int fila = 0; fila < 6; fila++) {
            System.out.print("|");
            for (int col = 0; col < 7; col++) {
                System.out.print(tablero[col][fila] + "|");
            }
            System.out.println();
            System.out.println("---------------");
        }
    }
    public char get_simbolo_actual() {
        return simbolo_actual;
    }
}

class Marcador {
    private BST<Integer, String> arbol_victorias;
    private HashST<String, Jugador> jugadores;
    private int partidas_jugadas;
    public Marcador() {
        arbol_victorias = new BST<>();
        jugadores = new HashST<>();
        partidas_jugadas = 0;
    }
    public void agregar_resultado(String ganador, String perdedor, boolean empate) {
        Jugador jugador_ganador = jugadores.obtener(ganador);
        Jugador jugador_perdedor = jugadores.obtener(perdedor);
        if (empate) {
            jugador_ganador.agregar_empate();
            jugador_perdedor.agregar_empate();
        } else {
            jugador_ganador.agregar_victoria();
            jugador_perdedor.agregar_derrota();
            arbol_victorias.insertar(jugador_ganador.get_victorias(), ganador);
        }
        partidas_jugadas++;
    }
    public void registrar_jugador(String nombre_jugador) {
        if (!jugadores.contiene(nombre_jugador)) {
            jugadores.insertar(nombre_jugador, new Jugador(nombre_jugador));
        }
    }
    public boolean verificar_jugador(String nombre_jugador) {
        return jugadores.contiene(nombre_jugador);
    }
    public Jugador[] rango_victorias(int min, int max) {
        List<Jugador> resultado = new ArrayList<>();
        for (Integer victorias : arbol_victorias.claves()) {
            if (victorias >= min && victorias <= max) {
                String nombre = arbol_victorias.obtener(victorias);
                resultado.add(jugadores.obtener(nombre));
            }
        }
        return resultado.toArray(new Jugador[0]);
    }
    public Jugador[] sucesor_victorias(int victorias) {
        List<Jugador> resultado = new ArrayList<>();
        Integer sucesor = null;
        for (Integer clave : arbol_victorias.claves()) {
            if (clave > victorias && (sucesor == null || clave < sucesor)) {
                sucesor = clave;
            }
        }
        if (sucesor != null) {
            String nombre = arbol_victorias.obtener(sucesor);
            resultado.add(jugadores.obtener(nombre));
        }
        return resultado.toArray(new Jugador[0]);
    }
}

class Partida {
    private String estado;
    private String ganador;
    private String jugador_A;
    private String jugador_B;
    private Conecta_Cuatro juego;
    public Partida(String jugador_A, String jugador_B) {
        this.jugador_A = jugador_A;
        this.jugador_B = jugador_B;
        this.estado = "EN_PROGRESO";
        this.ganador = "";
        this.juego = new Conecta_Cuatro();
    }
    public String jugar() {
        Scanner scanner = new Scanner(System.in);
        while (estado.equals("EN_PROGRESO")) {
            juego.mostrar_tablero();
            System.out.println("Turno de " + (juego.get_simbolo_actual() == 'X' ? jugador_A : jugador_B));
            System.out.print("Ingrese columna (0-6): ");
            int col = scanner.nextInt();
            if (!juego.hacer_movimiento(col)) {
                System.out.println("Movimiento inválido. Intente nuevamente.");
                continue;
            }
            if (juego.juego_terminado()) {
                if (juego.verificar_ganador('X')) {
                    estado = "VICTORIA";
                    ganador = jugador_A;
                } else if (juego.verificar_ganador('O')) {
                    estado = "VICTORIA";
                    ganador = jugador_B;
                } else {
                    estado = "EMPATE";
                }
                juego.mostrar_tablero();
            }
        }
        return ganador;
    }
}

public class Main {
    public static void main(String[] args) {
        Marcador marcador = new Marcador();
        marcador.registrar_jugador("Jugador_1");
        marcador.registrar_jugador("Jugador_2");
        Scanner scanner = new Scanner(System.in);
        boolean jugando = true;
        while (jugando) {
            Partida partida = new Partida("Jugador_1", "Jugador_2");
            System.out.println("Nueva partida: Jugador_1 (X) vs Jugador_2 (O)");
            String ganador = partida.jugar();
            if (ganador.isEmpty()) {
                System.out.println("¡Empate!");
                marcador.agregar_resultado("Jugador_1", "Jugador_2", true);
            } else {
                System.out.println("¡Ganador: " + ganador + "!");
                String perdedor = ganador.equals("Jugador_1") ? "Jugador_2" : "Jugador_1";
                marcador.agregar_resultado(ganador, perdedor, false);
            }
            System.out.print("¿Jugar otra partida? (s/n): ");
            jugando = scanner.next().equalsIgnoreCase("s");
        }
        System.out.println("¡Gracias por jugar!");
        scanner.close();
    }
}
