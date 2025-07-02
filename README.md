# ejemplo1
Persona
public class Persona {
    private String nombre;
    private String genero;
    private String fecNac;

    public Persona() {
    }

    public Persona(String nombre, String genero, String fecNac) {
        this.nombre = nombre;
        this.genero = genero;
        this.fecNac = fecNac;
    }

    public String getNombre() {
        return nombre;
    }

    public void setNombre(String nombre) {
        this.nombre = nombre;
    }

    public String getGenero() {
        return genero;
    }

    public void setGenero(String genero) {
        this.genero = genero;
    }

    public String getFecNac() {
        return fecNac;
    }

    public void setFecNac(String fecNac) {
        this.fecNac = fecNac;
    }

IUniversidad

public interface IUniversidad {
    final String nombre_U = "Universidad Tecnologica del Peru";
    final String sede_U = "Lima Norte, Los Olivos";
    final int ruc_U = 1098765415;
    
    public default String reporteU(){
        String salida="";
        salida=
        "\n Reporte de la Universidad: " + 
        "\n Nombre: " + nombre_U +
        "\n Sede: " + sede_U +
        "\n Ruc: " + ruc_U ;
        return salida;
    }
}

Alumno

public class Alumno extends Persona implements IUniversidad{
    private String codigo_alu;
    private String sede_alu;
    private double pension_alu;

    public Alumno() {
    }

    public Alumno(String codigo_alu, String sede_alu, double pension_alu) {
        this.codigo_alu = codigo_alu;
        this.sede_alu = sede_alu;
        this.pension_alu = pension_alu;
    }

    public Alumno(String nombre, String genero, String fecNac, String codigo_alu, String sede_alu, double pension_alu) {
        super(nombre, genero, fecNac);
        this.codigo_alu = codigo_alu;
        this.sede_alu = sede_alu;
        this.pension_alu = pension_alu;
    }

    public String getCodigo_alu() {
        return codigo_alu;
    }

    public void setCodigo_alu(String codigo_alu) {
        this.codigo_alu = codigo_alu;
    }

    public String getSede_alu() {
        return sede_alu;
    }

  FORMULARIO 
    
import Modelo.Alumno;
import Modelo.Persona;
import Modelo.IUniversidad;
import javax.swing.*;
import java.awt.*;
import java.awt.event.*;
import java.util.ArrayList;

public class VentanaAlumno extends JFrame {
    private JTextField txtNombre, txtGenero, txtFecNac, txtCodigoAlu, txtSedeAlu, txtPensionAlu;
    private JButton btnNuevo, btnModificar, btnEliminar, btnVerInfo, btnBuscar, btnLimpiar;
    private JButton btnInicio, btnAnterior, btnSiguiente, btnFinal;
    private JList<String> lstAlumnos;
    private DefaultListModel<String> modeloLista;
    private ArrayList<Alumno> listaAlumnos;
    private int indiceActual = -1;

    public VentanaAlumno() {
        setTitle("Gestión de Alumnos - UTP");
        setSize(800, 600);
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        setLayout(new BorderLayout());

        listaAlumnos = new ArrayList<>();
        modeloLista = new DefaultListModel<>();

        // Panel Superior (Formulario)
        JPanel pnlFormulario = new JPanel(new GridLayout(5, 2, 10, 10));
        pnlFormulario.setBorder(BorderFactory.createTitledBorder("Datos del Alumno"));

        txtNombre = new JTextField();
        txtGenero = new JTextField();
        txtFecNac = new JTextField();
        txtCodigoAlu = new JTextField();
        txtSedeAlu = new JTextField();
        txtPensionAlu = new JTextField();

        pnlFormulario.add(new JLabel("Nombre:"));
        pnlFormulario.add(txtNombre);
        pnlFormulario.add(new JLabel("Género:"));
        pnlFormulario.add(txtGenero);
        pnlFormulario.add(new JLabel("Fecha Nac.:"));
        pnlFormulario.add(txtFecNac);
        pnlFormulario.add(new JLabel("Código Alumno:"));
        pnlFormulario.add(txtCodigoAlu);
        pnlFormulario.add(new JLabel("Pensión:"));
        pnlFormulario.add(txtPensionAlu);

        add(pnlFormulario, BorderLayout.NORTH);

        // Panel Central (Lista y Botones)
        JPanel pnlCentral = new JPanel(new BorderLayout());
        lstAlumnos = new JList<>(modeloLista);
        JScrollPane scrollLista = new JScrollPane(lstAlumnos);
        pnlCentral.add(scrollLista, BorderLayout.CENTER);

        // Panel de Botones Derecha
        JPanel pnlBotonesAccion = new JPanel(new GridLayout(6, 1, 5, 5));
        btnNuevo = new JButton("Nuevo");
        btnModificar = new JButton("Modificar");
        btnEliminar = new JButton("Eliminar");
        btnVerInfo = new JButton("Ver Info");
        btnBuscar = new JButton("Buscar");
        btnLimpiar = new JButton("Limpiar");

        pnlBotonesAccion.add(btnNuevo);
        pnlBotonesAccion.add(btnModificar);
        pnlBotonesAccion.add(btnEliminar);
        pnlBotonesAccion.add(btnVerInfo);
        pnlBotonesAccion.add(btnBuscar);
        pnlBotonesAccion.add(btnLimpiar);

        pnlCentral.add(pnlBotonesAccion, BorderLayout.EAST);
        add(pnlCentral, BorderLayout.CENTER);

        // Panel Inferior (Navegación)
        JPanel pnlNavegacion = new JPanel();
        btnInicio = new JButton("Inicio");
        btnAnterior = new JButton("Anterior");
        btnSiguiente = new JButton("Siguiente");
        btnFinal = new JButton("Final");

        pnlNavegacion.add(btnInicio);
        pnlNavegacion.add(btnAnterior);
        pnlNavegacion.add(btnSiguiente);
        pnlNavegacion.add(btnFinal);

        add(pnlNavegacion, BorderLayout.SOUTH);

        // Eventos
        btnNuevo.addActionListener(e -> nuevoAlumno());
        btnModificar.addActionListener(e -> modificarAlumno());
        btnEliminar.addActionListener(e -> eliminarAlumno());
        btnVerInfo.addActionListener(e -> verInfoAlumno());
        btnBuscar.addActionListener(e -> buscarAlumno());
        btnLimpiar.addActionListener(e -> limpiarCampos());
        btnInicio.addActionListener(e -> navegarInicio());
        btnAnterior.addActionListener(e -> navegarAnterior());
        btnSiguiente.addActionListener(e -> navegarSiguiente());
        btnFinal.addActionListener(e -> navegarFinal());

        lstAlumnos.addListSelectionListener(e -> seleccionarAlumnoLista());
    }

    // Métodos de Acción
    private void nuevoAlumno() {
        try {
            Alumno alumno = new Alumno(
                txtNombre.getText(),
                txtGenero.getText(),
                txtFecNac.getText(),
                txtCodigoAlu.getText(),
                txtSedeAlu.getText(),
                Double.parseDouble(txtPensionAlu.getText())
            );
            listaAlumnos.add(alumno);
            actualizarLista();
            JOptionPane.showMessageDialog(this, "Alumno agregado correctamente.");
        } catch (NumberFormatException e) {
            JOptionPane.showMessageDialog(this, "Error: La pensión debe ser un número válido.", "Error", JOptionPane.ERROR_MESSAGE);
        }
    }

    private void modificarAlumno() {
        if (indiceActual >= 0) {
            Alumno alumno = listaAlumnos.get(indiceActual);
            alumno.setNombre(txtNombre.getText());
            alumno.setGenero(txtGenero.getText());
            alumno.setFecNac(txtFecNac.getText());
            alumno.setCodigo_alu(txtCodigoAlu.getText());
            alumno.setSede_alu(txtSedeAlu.getText());
            alumno.setPension_alu(Double.parseDouble(txtPensionAlu.getText()));
            actualizarLista();
            JOptionPane.showMessageDialog(this, "Alumno modificado correctamente.");
        } else {
            JOptionPane.showMessageDialog(this, "Seleccione un alumno primero.", "Advertencia", JOptionPane.WARNING_MESSAGE);
        }
    }

    private void eliminarAlumno() {
        if (indiceActual >= 0) {
            listaAlumnos.remove(indiceActual);
            actualizarLista();
            limpiarCampos();
            JOptionPane.showMessageDialog(this, "Alumno eliminado correctamente.");
        }
    }

    private void verInfoAlumno() {
        if (indiceActual >= 0) {
            Alumno alumno = listaAlumnos.get(indiceActual);
            JOptionPane.showMessageDialog(this, alumno.toString(), "Información del Alumno", JOptionPane.INFORMATION_MESSAGE);
        }
    }

    private void buscarAlumno() {
        String codigo = JOptionPane.showInputDialog(this, "Ingrese el código del alumno:");
        if (codigo != null && !codigo.isEmpty()) {
            for (int i = 0; i < listaAlumnos.size(); i++) {
                if (listaAlumnos.get(i).getCodigo_alu().equals(codigo)) {
                    indiceActual = i;
                    mostrarAlumno(listaAlumnos.get(i));
                    lstAlumnos.setSelectedIndex(i);
                    return;
                }
            }
            JOptionPane.showMessageDialog(this, "Alumno no encontrado.", "Búsqueda", JOptionPane.WARNING_MESSAGE);
        }
    }

    private void limpiarCampos() {
        txtNombre.setText("");
        txtGenero.setText("");
        txtFecNac.setText("");
        txtCodigoAlu.setText("");
        txtSedeAlu.setText("");
        txtPensionAlu.setText("");
        indiceActual = -1;
    }

    // Métodos de Navegación
    private void navegarInicio() {
        if (!listaAlumnos.isEmpty()) {
            indiceActual = 0;
            mostrarAlumno(listaAlumnos.get(indiceActual));
        }
    }

    private void navegarAnterior() {
        if (indiceActual > 0) {
            indiceActual--;
            mostrarAlumno(listaAlumnos.get(indiceActual));
        }
    }

    private void navegarSiguiente() {
        if (indiceActual < listaAlumnos.size() - 1) {
            indiceActual++;
            mostrarAlumno(listaAlumnos.get(indiceActual));
        }
    }

    private void navegarFinal() {
        if (!listaAlumnos.isEmpty()) {
            indiceActual = listaAlumnos.size() - 1;
            mostrarAlumno(listaAlumnos.get(indiceActual));
        }
    }

    // Métodos Auxiliares
    private void actualizarLista() {
        modeloLista.clear();
        for (Alumno alumno : listaAlumnos) {
            modeloLista.addElement(alumno.getNombre() + " (" + alumno.getCodigo_alu() + ")");
        }
    }

    private void mostrarAlumno(Alumno alumno) {
        txtNombre.setText(alumno.getNombre());
        txtGenero.setText(alumno.getGenero());
        txtFecNac.setText(alumno.getFecNac());
        txtCodigoAlu.setText(alumno.getCodigo_alu());
        txtSedeAlu.setText(alumno.getSede_alu());
        txtPensionAlu.setText(String.valueOf(alumno.getPension_alu()));
    }

    private void seleccionarAlumnoLista() {
        int seleccionado = lstAlumnos.getSelectedIndex();
        if (seleccionado != -1) {
            indiceActual = seleccionado;
            mostrarAlumno(listaAlumnos.get(seleccionado));
        }
    }

    public static void main(String[] args) {
        SwingUtilities.invokeLater(() -> {
            VentanaAlumno ventana = new VentanaAlumno();
            ventana.setVisible(true);
        });
    }
}

