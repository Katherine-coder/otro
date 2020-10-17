# otro
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Windows.Forms;
using TallerLemus.Clases;
using MySql.Data.MySqlClient;
using MySql.Data.Types;
using System.Data.SqlClient;
using System.Collections;

namespace TallerLemus
{
    public partial class Autos : Form
    {
        public Autos()
        {
            InitializeComponent();
        }

        private void btnclose_Click(object sender, EventArgs e)
        {
            this.Close();
        }

        private void btnGuardar_Click(object sender, EventArgs e)
        {

            if (txtMatricula.Text == "" || cmbPropietario.Text == "" ||
                txtModelo.Text == "" || txtAnio.Text == "" || txtVIN.Text == "")
            {
                MessageBox.Show("FALTA INFORMACIÓN");
            }
            else
            {

                Auto auto = new Auto();
                auto.Matricula = txtMatricula.Text.Trim();
                auto.Propietario = cmbPropietario.Text.Trim();
                auto.Modelo = txtModelo.Text.Trim();
                auto.Anio = int.Parse(txtAnio.Text);
                auto.NoVIN = txtVIN.Text.Trim();

                int resultado = CRUD.AgregarAut(auto);
                if (resultado > 0)
                {
                    MessageBox.Show("Vehículo Guardado Con Exito!!", "Guardado", MessageBoxButtons.OK, MessageBoxIcon.Information);
                    txtMatricula.Clear();
                    txtModelo.Clear();
                    txtAnio.Clear();
                    txtVIN.Clear();

                }
                else
                {
                    MessageBox.Show("No se pudo guardar el vehiculo", "Fallo!!", MessageBoxButtons.OK, MessageBoxIcon.Exclamation);
                }
            }
        }


        private void Autos_Load(object sender, EventArgs e)
        {
            using (MySqlConnection conn = new MySqlConnection("server=localhost;database=taller;" +
                "Uid=root;pwd=;"))
            {
                string query = "SELECT IDCliente,concat(Nombre, ' ', Apellido) as NombreCompleto from cliente";

                MySqlCommand cmd = new MySqlCommand(query, conn);

                MySqlDataAdapter da1 = new MySqlDataAdapter(cmd);
                DataTable dt = new DataTable();
                da1.Fill(dt);

                cmbPropietario.ValueMember = "IDCliente";
                cmbPropietario.DisplayMember = "NombreCompleto";
                cmbPropietario.DataSource = dt;

            }
        }
    }
}
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Windows.Forms;
using MySql.Data.MySqlClient;
using TallerLemus.Clases;

namespace TallerLemus
{
    public partial class Clientes : Form
    {
        public Clientes()
        {
            InitializeComponent();
        }

        public Cliente Seleccionado { get; set; }
        public Cliente clienteActual { get; set; }

        private void btnGuardar_Click(object sender, EventArgs e)
        {

            if (txtNombre.Text == "" || txtApellido.Text == "" ||
                txtTelefono.Text == "" || txtDireccion.Text == "" || txtCorreo.Text == "")
            {
                MessageBox.Show("FALTA INFORMACIÓN");
            }
            else
            {

                Cliente cliente = new Cliente();
                cliente.Nombre = txtNombre.Text.Trim();
                cliente.Apellido = txtApellido.Text.Trim();
                cliente.Direccion = txtDireccion.Text.Trim();
                cliente.Telefono = int.Parse(txtTelefono.Text);
                cliente.Correo = txtCorreo.Text.Trim();

                int resultado = CRUD.Agregar(cliente);
                if (resultado > 0)
                {
                    MessageBox.Show("Cliente Guardado Con Exito!!", "Guardado", MessageBoxButtons.OK, MessageBoxIcon.Information);
                    txtNombre.Clear();
                    txtDireccion.Clear();
                    txtApellido.Clear();
                    txtCorreo.Clear();
                    txtTelefono.Clear();
                    
                }
                else
                {
                    MessageBox.Show("No se pudo guardar el cliente", "Fallo!!", MessageBoxButtons.OK, MessageBoxIcon.Exclamation);
                }
            }
        }
    }
}
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace TallerLemus.Clases
{
    public class Cliente
    {
        public int IDCliente { get; set; }
        public string Nombre { get; set; }
        public string Apellido { get; set; }
        public string Direccion { get; set; }
        public int Telefono { get; set; }
        public string Correo { get; set; }


        public Cliente() { }

        public Cliente(int pId, string pnombre, string papellido, string pdireccion, int ptelefon, string pcorreo)
        {
            this.IDCliente = pId;
            this.Nombre = pnombre;
            this.Apellido = papellido;
            this.Telefono = ptelefon;
            this.Direccion = pdireccion;
            this.Correo = pcorreo;

        }
    }
}
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using MySql.Data.MySqlClient; 

namespace TallerLemus.Clases
{
    class conexion
    {
        public static MySqlConnection obtenerconexion()
        {
            MySqlConnection conexion = new MySqlConnection("server=localhost;database=taller;" +
                "Uid=root;pwd=;");
            conexion.Open();
            return conexion;
        }
    }
}
using System;
using System.Collections;
using System.Collections.Generic;
using System.Data;
using System.Data.SqlClient;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using MySql.Data.MySqlClient;

namespace TallerLemus.Clases
{
    public class CRUD
    {
        //instertar clientes
        public static int Agregar(Cliente cliente)
        {

            int retorno = 0;

            MySqlCommand comando = new MySqlCommand(string.Format("Insert into cliente (Nombre, Apellido, Direccion, Telefono, Correo) values ('{0}','{1}','{2}','{3}', '{4}')",
             cliente.Nombre, cliente.Apellido, cliente.Direccion, cliente.Telefono, cliente.Correo), conexion.obtenerconexion());
            retorno = comando.ExecuteNonQuery();
            return retorno;
        }

        //agregar autos
        public static int AgregarAut(Auto auto)
        {

            int retorno = 0;

            MySqlCommand comando = new MySqlCommand(string.Format("Insert into vehiculo (Matricula, Propietario, Modelo, Anio, NoVIN) values ('{0}','{1}','{2}','{3}','{4}')",
             auto.Matricula, auto.Propietario, auto.Modelo, auto.Anio, auto.NoVIN), conexion.obtenerconexion());
            retorno = comando.ExecuteNonQuery();
            return retorno;
        }

        //mostrar la informacion en el dtg
        public static Cliente Obtener(int pId)
        {
            Cliente cliente = new Cliente();
            MySqlConnection conectar = conexion.obtenerconexion();

            MySqlCommand _comando = new MySqlCommand(String.Format("SELECT IDCliente, Nombre, Apellido, Direccion, Telefono, Correo FROM cliente where IDCliente={0}", pId), conectar);
            MySqlDataReader _reader = _comando.ExecuteReader();
            while (_reader.Read())
            {
                cliente.IDCliente = _reader.GetInt32(0);
                cliente.Nombre = _reader.GetString(1);
                cliente.Apellido = _reader.GetString(2);
                cliente.Direccion = _reader.GetString(3);
                cliente.Telefono = _reader.GetInt32(4);
                cliente.Correo = _reader.GetString(5);

            }

            conectar.Close();
            return cliente;

        }

    }
}
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace TallerLemus.Clases
{
    public class Auto
    {
        public string Propietario { get; set; }
        public string Matricula { get; set; }
        public string Modelo { get; set; }
        public int Anio { get; set; }
        public string NoVIN { get; set; }

        public Auto() { }

        public Auto(int vId, string matricula, string propietario, string modelo, int anio, string novin)
        {
            this.Matricula = matricula;
            this.Propietario = propietario;
            this.Modelo = modelo;
            this.Anio = anio;
            this.NoVIN = novin; 

        }
    }
}
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Windows.Forms;

namespace TallerLemus
{
    public partial class Form1 : Form
    {
        public Form1()
        {
            InitializeComponent();
        }


        private Form activeForm = null;
        private void abrirformularios(Form childForm)
        {
            if (activeForm != null)
                activeForm.Close();
            activeForm = childForm;
            childForm.TopLevel = false;
            childForm.FormBorderStyle = FormBorderStyle.None;
            childForm.Dock = DockStyle.Fill;
            panelContainer.Controls.Add(childForm);
            panelContainer.Tag = childForm;
            childForm.BringToFront();
            childForm.Show();
        }

        private void btnClientes_Click(object sender, EventArgs e)
        {
            abrirformularios(new Clientes());
        }

        private void btnAutos_Click(object sender, EventArgs e)
        {
            abrirformularios(new Autos());
        }

        private void btnCerrar_Click(object sender, EventArgs e)
        {
            Application.Exit();
        }
    }
}

