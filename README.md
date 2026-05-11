import java.util.ArrayList;
import java.util.List;
import java.util.Scanner;

class Edificio {
    private String nombre;
    private String tipo;
    private int seguridad;
    private int prevIncendios;
    private int felicidad;
    private int precio;

    public Edificio(String nombre, String tipo, int seguridad, int prevIncendios, int felicidad, int precio) {
        this.nombre = nombre;
        this.tipo = tipo;
        this.seguridad = seguridad;
        this.prevIncendios = prevIncendios;
        this.felicidad = felicidad;
        this.precio = precio;
    }

    public String getNombre() {
        return nombre;
    }

    public int getSeguridad() {
        return seguridad;
    }

    public int getPrevIncendios() {
        return prevIncendios;
    }

    public int getFelicidad() {
        return felicidad;
    }

    public int getPrecio() {
        return precio;
    }
}

class Alcalde {
    private String nombre;
    private String apellido;
    private int dinero;

    public Alcalde(String nombre, String apellido) {
        this.nombre = nombre;
        this.apellido = apellido;
        this.dinero = 1000000; 
    }

    public String getNombre() {
        return nombre;
    }

    public String getApellido() {
        return apellido;
    }

    public int getDinero() {
        return dinero;
    }

    public boolean gastarDinero(int cantidad) {
        if (cantidad <= dinero) {
            dinero -= cantidad;
            return true;
        }
        return false;
    }
}

class Ciudad {
    private String nombre;
    private Alcalde alcalde;
    private List<Edificio> edificios;

    public Ciudad(String nombre, Alcalde alcalde) {
        this.nombre = nombre;
        this.alcalde = alcalde;
        this.edificios = new ArrayList<>();
    }

    public void agregarEdificio(Edificio edificio) {
        edificios.add(edificio);
    }

    public List<Edificio> getEdificios() {
        return edificios;
    }

    public double promedioSeguridad() {
        int totalSeguridad = 0;
        for (Edificio e : edificios) {
            totalSeguridad += e.getSeguridad();
        }
        return edificios.size() > 0 ? (double) totalSeguridad / edificios.size() : 0;
    }

    public double promedioPrevIncendios() {
        int totalPrevIncendios = 0;
        for (Edificio e : edificios) {
            totalPrevIncendios += e.getPrevIncendios();
        }
        return edificios.size() > 0 ? (double) totalPrevIncendios / edificios.size() : 0;
    }

    public double promedioFelicidad() {
        int totalFelicidad = 0;
        for (Edificio e : edificios) {
            totalFelicidad += e.getFelicidad();
        }
        return edificios.size() > 0 ? (double) totalFelicidad / edificios.size() : 0;
    }

    public Edificio edificioMayorValor() {
        Edificio mayor = null;
        for (Edificio e : edificios) {
            if (mayor == null || e.getPrecio() > mayor.getPrecio()) {
                mayor = e;
            }
        }
        return mayor;
    }

    public Edificio edificioMenorValor() {
        Edificio menor = null;
        for (Edificio e : edificios) {
            if (menor == null || e.getPrecio() < menor.getPrecio()) {
                menor = e;
            }
        }
        return menor;
    }

    public int totalInvertido() {
        int total = 0;
        for (Edificio e : edificios) {
            total += e.getPrecio();
        }
        return total;
    }
}

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        
        System.out.println("Ingrese el nombre del alcalde:");
        String nombreAlcalde = scanner.nextLine();
        System.out.println("Ingrese el apellido del alcalde:");
        String apellidoAlcalde = scanner.nextLine();
        Alcalde alcalde = new Alcalde(nombreAlcalde, apellidoAlcalde);
        
        System.out.println("Ingrese el nombre de la ciudad:");
        String nombreCiudad = scanner.nextLine();
        Ciudad ciudad = new Ciudad(nombreCiudad, alcalde);
        
        boolean continuarAgregando = true;
        while (continuarAgregando) {
            System.out.println("Seleccione un edificio para agregar:");
            System.out.println("1. Rascascielos - $900000");
            System.out.println("2. Torre Eiffel - $850000");
            System.out.println("3. Planta de energía eólica - $500000");
            System.out.println("4. Mega estación de policía - $400000");
            System.out.println("5. Reserva natural - $500000");
            System.out.println("0. Terminar compra");

            int opcion = scanner.nextInt();
            Edificio edificio = null;

            switch (opcion) {
                case 1:
                    edificio = new Edificio("Rascascielos", "Maravilla", 0, 0, 10, 900000);
                    break;
                case 2:
                    edificio = new Edificio("Torre Eiffel", "Maravilla", 0, 0, 10, 850000);
                    break;
                case 3:
                    edificio = new Edificio("Planta de energía eólica", "Planta energética", 0, 0, 10, 500000);
                    break;
                case 4:
                    edificio = new Edificio("Mega estación de policía", "Seguridad", 10, 0, 10, 400000);
                    break;
                case 5:
                    edificio = new Edificio("Reserva natural", "Ecología", 0, 0, 10, 500000);
                    break;
                case 0:
                    continuarAgregando = false;
                    break;
                default:
                    System.out.println("Opción inválida.");
                    continue; // Volver al inicio del bucle
            }

            if (edificio != null) {
                if (alcalde.gastarDinero(edificio.getPrecio())) {
                    ciudad.agregarEdificio(edificio);
                    System.out.println("Edificio agregado: " + edificio.getNombre());
                } else {
                    System.out.println("No hay suficiente dinero para comprar " + edificio.getNombre());
                }
            }
        }

        mostrarMenu(ciudad);
        scanner.close();
    }

    private static void mostrarMenu(Ciudad ciudad) {
        Scanner scanner = new Scanner(System.in);
        boolean continuar = true;

        while (continuar) {
            System.out.println("Seleccione una opción:");
            System.out.println("1. Datos del alcalde y de la ciudad");
            System.out.println("2. Promedio de seguridad");
            System.out.println("3. Promedio de prevención de incendios");
            System.out.println("4. Promedio de felicidad");
            System.out.println("5. Edificio público de mayor y menor valor");
            System.out.println("6. Dinero total gastado");
            System.out.println("7. Terminar");

            int opcion = scanner.nextInt();
            switch (opcion) {
                case 1:
                    System.out.println("Alcalde: " + ciudad.getAlcalde().getNombre() + " " + ciudad.getAlcalde().getApellido());
                    System.out.println("Ciudad: " + ciudad.getNombre());
                    break;
                case 2:
                    System.out.println("Promedio de seguridad: " + ciudad.promedioSeguridad());
                    break;
                case 3:
                    System.out.println("Promedio de prevención de incendios: " + ciudad.promedioPrevIncendios());
                    break;
                case 4:
                    System.out.println("Promedio de felicidad: " + ciudad.promedioFelicidad());
                    break;
                case 5:
                    Edificio mayorValor = ciudad.edificioMayorValor();
                    Edificio menorValor = ciudad.edificioMenorValor();
                    System.out.println("Edificio de mayor valor: " + (mayorValor != null ? mayorValor.getNombre() + " - $" + mayorValor.getPrecio() : "Ninguno"));
                    System.out.println("Edificio de menor valor: " + (menorValor != null ? menorValor.getNombre() + " - $" + menorValor.getPrecio() : "Ninguno"));
                    break;
                case 6:
                    System.out.println("Dinero total gastado: $" + ciudad.totalInvertido());
                    break;
                case 7:
                    continuar = false;
                    break;
                default:
                    System.out.println("Opción inválida.");
            }
        }
    }
}
