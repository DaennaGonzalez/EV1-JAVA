Archivo README
El sistema de administración de citas médicas desarrollado en Java es una herramienta diseñada para facilitar la gestión de doctores, pacientes y citas médicas. Este documento proporciona detalles sobre la instalación y configuración del programa, su uso, los créditos correspondientes y la licencia bajo la cual se distribuye.

Instalación y Configuración
Para comenzar, asegúrese de tener instalados los siguientes componentes en su sistema: Java Development Kit (JDK) 11, que puede descargar e instalar desde el sitio oficial de Oracle; IntelliJ IDEA, el entorno de desarrollo integrado (IDE) recomendado, disponible en el sitio oficial de JetBrains; y Git, necesario para clonar el repositorio y manejar el control de versiones, disponible en su sitio oficial.

Para obtener el código fuente del proyecto, abra su terminal o consola de comandos y clone el repositorio desde GitHub utilizando el comando: git clone https://github.com/tu_usuario/sistema-administracion-citas.git y luego navegue al directorio del proyecto con cd sistema-administracion-citas.

En IntelliJ IDEA, abra el proyecto clonado seleccionando "Open". Asegúrese de que el proyecto esté utilizando el JDK 11 configurado previamente, lo cual puede verificar y ajustar en "File" > "Project Structure" > "Project Settings" > "Project", seleccionando el SDK adecuado. Configure la estructura de artefactos para crear un archivo JAR ejecutable, yendo a "File" > "Project Structure" > "Artifacts", haciendo clic en el botón "+" y seleccionando "JAR" > "From modules with dependencies". Asegúrese de que el punto de entrada sea la clase Main.

Para crear el JAR ejecutable, vaya a "Build" > "Build Artifacts", seleccione el artefacto configurado y haga clic en "Build". El archivo JAR se generará en la carpeta out/artifacts.

Uso del Programa
Para ejecutar el programa desde el JAR, abra una terminal y navegue hasta la carpeta que contiene el archivo JAR generado, luego ejecute el comando java -jar sistema-administracion-citas.jar.

Una vez que el programa esté en ejecución, se presentará un menú principal con las siguientes opciones: Registrar Doctor, que permite registrar un nuevo doctor en el sistema solicitando su ID, nombre y especialidad; Registrar Paciente, que permite registrar un nuevo paciente en el sistema solicitando su ID y nombre; Crear Cita, que permite crear una nueva cita médica solicitando detalles como ID de la cita, fecha, hora, motivo, y los identificadores del doctor y paciente; Relacionar Cita, que permite asociar una cita existente con un doctor y un paciente específicos; Acceder como Administrador, que permite iniciar sesión como administrador proporcionando un identificador y una contraseña para acceder a funcionalidades restringidas; y Salir, que termina la ejecución del programa.

El usuario puede interactuar con el programa seleccionando la opción deseada y proporcionando la información solicitada en cada caso. El sistema valida las entradas y confirma las operaciones realizadas, asegurando una gestión eficiente y segura de los datos.

Créditos
Este proyecto ha sido desarrollado por mi.

Licencia
Este proyecto está licenciado bajo la Licencia MIT. Consulte el archivo LICENSE para más detalles.

Código Fuente del Programa:
import java.text.ParseException;
import java.text.SimpleDateFormat;
import java.util.Date;
import java.util.Scanner;

public class SistemaDeCitas {
    private BaseDeDatos baseDeDatos = new BaseDeDatos();
    private Scanner scanner = new Scanner(System.in);

    public void iniciar() {
        inicializarDatos();
        mostrarMenu();
    }

    private void inicializarDatos() {
        baseDeDatos.agregarAdministrador(new Administrador("admin", "admin123"));
    }

    private void mostrarMenu() {
        while (true) {
            System.out.println("Menú Principal");
            System.out.println("1. Registrar Doctor");
            System.out.println("2. Registrar Paciente");
            System.out.println("3. Crear Cita");
            System.out.println("4. Relacionar Cita");
            System.out.println("5. Acceder como Administrador");
            System.out.println("6. Salir");
            System.out.print("Seleccione una opción: ");
            String opcion = scanner.nextLine();

            switch (opcion) {
                case "1":
                    registrarDoctor();
                    break;
                case "2":
                    registrarPaciente();
                    break;
                case "3":
                    crearCita();
                    break;
                case "4":
                    relacionarCita();
                    break;
                case "5":
                    iniciarSesion();
                    break;
                case "6":
                    System.out.println("Programa terminado");
                    return;
                default:
                    System.out.println("Opción no válida. Intente de nuevo.");
            }
        }
    }

    private void registrarDoctor() {
        System.out.print("Ingrese el ID del Doctor: ");
        int id = leerEntero();
        System.out.print("Ingrese el nombre del Doctor: ");
        String nombre = scanner.nextLine();
        System.out.print("Ingrese la especialidad del Doctor: ");
        String especialidad = scanner.nextLine();

        Doctor doctor = new Doctor(id, nombre, especialidad);
        baseDeDatos.agregarDoctor(doctor);
        System.out.println("Doctor registrado exitosamente");
    }

    private void registrarPaciente() {
        System.out.print("Ingrese el ID del Paciente: ");
        int id = leerEntero();
        System.out.print("Ingrese el nombre del Paciente: ");
        String nombre = scanner.nextLine();

        Paciente paciente = new Paciente(id, nombre);
        baseDeDatos.agregarPaciente(paciente);
        System.out.println("Paciente registrado exitosamente");
    }

    private void crearCita() {
        try {
            System.out.print("Ingrese el ID de la Cita: ");
            int id = leerEntero();
            System.out.print("Ingrese la fecha de la Cita (YYYY-MM-DD): ");
            Date fecha = new SimpleDateFormat("yyyy-MM-dd").parse(scanner.nextLine());
            System.out.print("Ingrese la hora de la Cita (HH:MM): ");
            String hora = scanner.nextLine();
            System.out.print("Ingrese el motivo de la Cita: ");
            String motivo = scanner.nextLine();
            System.out.print("Ingrese el ID del Doctor: ");
            int doctorId = leerEntero();
            System.out.print("Ingrese el ID del Paciente: ");
            int pacienteId = leerEntero();

            Cita cita = new Cita(id, fecha, hora, motivo, doctorId, pacienteId);
            baseDeDatos.agregarCita(cita);
            System.out.println("Cita creada exitosamente");
        } catch (ParseException e) {
            System.out.println("Formato de fecha u hora no válido. Intente de nuevo.");
        }
    }

    private void relacionarCita() {
        System.out.print("Ingrese el ID de la Cita: ");
        int idCita = leerEntero();
        System.out.print("Ingrese el ID del Doctor: ");
        int idDoctor = leerEntero();
        System.out.print("Ingrese el ID del Paciente: ");
        int idPaciente = leerEntero();

        Cita cita = baseDeDatos.obtenerCita(idCita);
        if (cita == null) {
            System.out.println("Cita no encontrada");
            return;
        }
        Doctor doctor = baseDeDatos.obtenerDoctor(idDoctor);
        if (doctor == null) {
            System.out.println("Doctor no encontrado");
            return;
        }
        Paciente paciente = baseDeDatos.obtenerPaciente(idPaciente);
        if (paciente == null) {
            System.out.println("Paciente no encontrado");
            return;
        }
        cita.asignarDoctor(idDoctor);
        cita.asignarPaciente(idPaciente);
        System.out.println("Cita relacionada exitosamente");
    }

    private void iniciarSesion() {
        System.out.print("Ingrese su identificador: ");
        String identificador = scanner.nextLine();
        System.out.print("Ingrese su contraseña: ");
        String contraseña = scanner.nextLine();

        Administrador administrador = baseDeDatos.obtenerAdministrador(identificador);
        if (administrador != null && administrador.autenticar(identificador, contraseña)) {
            System.out.println("Acceso concedido");
        } else {
            System.out.println("Acceso denegado");
        }
    }

    private int leerEntero() {
        while (true) {
            try {
                return Integer.parseInt(scanner.nextLine());
            } catch (NumberFormatException e) {
                System.out.print("Entrada no válida. Ingrese un número entero: ");
            }
        }
    }
}

import java.util.HashMap;
import java.util.Map;

public class BaseDeDatos {
    private Map<Integer, Doctor> doctores = new HashMap<>();
    private Map<Integer, Paciente> pacientes = new HashMap<>();
    private Map<Integer, Cita> citas = new HashMap<>();
    private Map<String, Administrador> administradores = new HashMap<>();

    public void agregarDoctor(Doctor doctor) {
        doctores.put(doctor.getId(), doctor);
    }

    public Doctor obtenerDoctor(int id) {
        return doctores.get(id);
    }

    public void agregarPaciente(Paciente paciente) {
        pacientes.put(paciente.getId(), paciente);
    }

    public Paciente obtenerPaciente(int id) {
        return pacientes.get(id);
    }

    public void agregarCita(Cita cita) {
        citas.put(cita.getId(), cita);
    }

    public Cita obtenerCita(int id) {
        return citas.get(id);
    }

    public void agregarAdministrador(Administrador admin) {
        administradores.put(admin.getIdentificador(), admin);
    }

    public Administrador obtenerAdministrador(String id) {
        return administradores.get(id);
    }
}

public class Administrador {
    private String identificador;
    private String contraseña;

    public Administrador(String identificador, String contraseña) {
        this.identificador = identificador;
        this.contraseña = contraseña;
    }

    public String getIdentificador() {
        return identificador;
    }

    public String getContraseña() {
        return contraseña;
    }

    public boolean autenticar(String id, String pass) {
        return this.identificador.equals(id) && this.contraseña.equals(pass);
    }
}

public abstract class Persona implements Identificable {
    private int id;
    private String nombre;

    public Persona(int id, String nombre) {
        this.id = id;
        this.nombre = nombre;
    }

    @Override
    public int getId() {
        return id;
    }

    public String getNombre() {
        return nombre;
    }
}

public interface Identificable {
    int getId();
}

import java.util.Date;

public class Cita {
    private int id;
    private Date fecha;
    private String hora;
    private String motivo;
    private int doctorId;
    private int pacienteId;

    public Cita(int id, Date fecha, String hora, String motivo, int doctorId, int pacienteId) {
        this.id = id;
        this.fecha = fecha;
        this.hora = hora;
        this.motivo = motivo;
        this.doctorId = doctorId;
        this.pacienteId = pacienteId;
    }

    public int getId() {
        return id;
    }

    public Date getFecha() {
        return fecha;
    }

    public String getHora() {
        return hora;
    }

    public String getMotivo() {
        return motivo;
    }

    public int getDoctorId() {
        return doctorId;
    }

    public int getPacienteId() {
        return pacienteId;
    }

    public void asignarDoctor(int doctorId) {
        this.doctorId = doctorId;
    }

    public void asignarPaciente(int pacienteId) {
        this.pacienteId = pacienteId;
    }
}

public class Doctor extends Persona {
    private String especialidad;

    public Doctor(int id, String nombre, String especialidad) {
        super(id, nombre);
        this.especialidad = especialidad;
    }

    public String getEspecialidad() {
        return especialidad;
    }
}

public class Paciente extends Persona {
    public Paciente(int id, String nombre) {
        super(id, nombre);
    }
}

public class Main {
    public static void main(String[] args) {
        SistemaDeCitas sistema = new SistemaDeCitas();
        sistema.iniciar();
    }
}
