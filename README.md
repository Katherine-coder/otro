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

