# ProyectoFinal
package Proyecto_Final.conexion;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;

public class Coneccion {
	private Connection conn = null;
	private PreparedStatement sentencia = null;
	private static final String JDBC_DRIVER = "org.mariadb.jdbc.Driver";
	private static final String DB_URL = "jdbc:mariadb://localhost/micro_mercado";

	public Coneccion() {

		//Credencial de la base de datos
		String USER = "root";
		String PASS = "";
		try {
			// Paso 2: Registrar JDBC driver
			Class.forName(JDBC_DRIVER);

			// Paso 3: Abrir la coneccion
			System.out.println("Conectando a la base de datos...");
			conn = DriverManager.getConnection(DB_URL, USER, PASS);
		} catch (SQLException se) {
			// Errores de JDBC
			se.printStackTrace();
		} catch (Exception e) {
			// Errores de Class.forName
			e.printStackTrace();
		}

	}

	public void consulta(String sql) throws Throwable {
		sentencia = conn.prepareStatement(sql);
	}

	public ResultSet resultado() throws Throwable {
		return sentencia.executeQuery();
	}
	
	public int modificacion() throws Throwable {
		return sentencia.executeUpdate();
		
	}
	
	public void close() throws SQLException {
		if (sentencia != null) {
			sentencia.close();
		}
		
		if (conn != null) {
			conn.close();
		}
	}

	public PreparedStatement getSentencia() {
		return sentencia;
	}
	}
ackage com.proyecto_final.control;

import java.sql.ResultSet;
import java.util.ArrayList;

import com.proyecto_final.entity.Categoria;

public class CategoriaCtrl implements Control <Categoria> {


	private conexion conection;
	
	public CategoriaCtrl(conexion conection) {
		this.conection = conection;
	}

	/*
	 * *****************************************************************************
	 * List
	 ******************************************************************************/
	public ArrayList<Categoria> list() throws Throwable {
		ArrayList<Categoria> categoria = new ArrayList<Categoria>();
		ResultSet rs;
		int codigo_categoria;
		String nombre;
		String descripcion;
		
		conection.SQL("Select * from categoria");

		rs = conection.resultSet();

		while (rs.next()) {			
			codigo_categoria = rs.getInt("codigo_categoria");
			nombre = rs.getString("nombre");// son varchart su equivalente es el string 
			descripcion = rs.getString("descripcion");
			categoria.add(new Categoria(codigo_categoria,nombre,descripcion));
		}

		return categoria;

	}

	/*
	 * *****************************************************************************
	 * Insert
	 ******************************************************************************/
	public void insert(Categoria categoria) throws Throwable {

		conection.SQL("Insert into cliente(NIT,nombre) VALUES(?,?)");
		conection.preparedStatement().setInt(1, categoria.getCodigoCategoria());
		conection.preparedStatement().setString(2, categoria.getNombre());
		conection.preparedStatement().setString(3, categoria.getDescripcion());
		conection.CUD();

	}

	/*
	 * *****************************************************************************
	 * Search
	 ******************************************************************************/

	public void search(Categoria cliente) throws Throwable {

		ResultSet rs;

		conection.SQL("Select * from categoria where categoria=?");
		conection.preparedStatement().setString(1, cliente.getNombre());
		rs = conection.resultSet();

		while (rs.next()) {

			cliente.setNombre(rs.getString("nombre"));
		}

		rs.close();

	}

	/*
	 * *****************************************************************************
	 * Update
	 ******************************************************************************/

	public void update(Categoria categoria) throws Throwable {
		String nombre;
		String descripcion;
		if (categoria != null) {
			nombre = categoria.getNombre();
			descripcion = categoria.getDescripcion();

			conection.SQL("Update cliente set nombre = ? where descripcion=?");
			conection.preparedStatement().setString(1, nombre);
			conection.preparedStatement().setString(2, descripcion);
			conection.CUD();
		}
	}

	
}


package com.proyecto_final.control;

import java.sql.ResultSet;
import java.util.ArrayList;

import com.proyecto_final.entity.Categoria;

public class CategoriaCtrl implements Control <Categoria> {


	private conexion conection;
	
	public CategoriaCtrl(conexion conection) {
		this.conection = conection;
	}

	/*
	 * *****************************************************************************
	 * List
	 ******************************************************************************/
	public ArrayList<Categoria> list() throws Throwable {
		ArrayList<Categoria> categoria = new ArrayList<Categoria>();
		ResultSet rs;
		int codigo_categoria;
		String nombre;
		String descripcion;
		
		conection.SQL("Select * from categoria");

		rs = conection.resultSet();

		while (rs.next()) {			
			codigo_categoria = rs.getInt("codigo_categoria");
			nombre = rs.getString("nombre");// son varchart su equivalente es el string 
			descripcion = rs.getString("descripcion");
			categoria.add(new Categoria(codigo_categoria,nombre,descripcion));
		}

		return categoria;

	}

	/*
	 * *****************************************************************************
	 * Insert
	 ******************************************************************************/
	public void insert(Categoria categoria) throws Throwable {

		conection.SQL("Insert into cliente(NIT,nombre) VALUES(?,?)");
		conection.preparedStatement().setInt(1, categoria.getCodigoCategoria());
		conection.preparedStatement().setString(2, categoria.getNombre());
		conection.preparedStatement().setString(3, categoria.getDescripcion());
		conection.CUD();

	}

	/*
	 * *****************************************************************************
	 * Search
	 ******************************************************************************/

	public void search(Categoria cliente) throws Throwable {

		ResultSet rs;

		conection.SQL("Select * from categoria where categoria=?");
		conection.preparedStatement().setString(1, cliente.getNombre());
		rs = conection.resultSet();

		while (rs.next()) {

			cliente.setNombre(rs.getString("nombre"));
		}

		rs.close();

	}

	/*
	 * *****************************************************************************
	 * Update
	 ******************************************************************************/

	public void update(Categoria categoria) throws Throwable {
		String nombre;
		String descripcion;
		if (categoria != null) {
			nombre = categoria.getNombre();
			descripcion = categoria.getDescripcion();

			conection.SQL("Update cliente set nombre = ? where descripcion=?");
			conection.preparedStatement().setString(1, nombre);
			conection.preparedStatement().setString(2, descripcion);
			conection.CUD();
		}
	}
package com.proyecto_final.control;

import java.sql.Date;
import java.sql.ResultSet;
import java.util.ArrayList;

import com.proyecto_final.entity.Compra;



public class CompraCtrl implements Control <Compra>{
	
	private conexion conection;
	
	public CompraCtrl (conexion conection) {
		this.conection = conection;
	}

	/*
	 * *****************************************************************************
	 * List
	 ******************************************************************************/
	public ArrayList<Compra> list() throws Throwable {
		ArrayList<Compra> compra = new ArrayList<Compra>();
		ResultSet rs;
		Date fecha;
		int codigoProveedor;

		conection.SQL("Select * from compra");

		rs = conection.resultSet();

		while (rs.next()) {
			fecha = rs.getDate("fecha");
			codigoProveedor = rs.getInt("CodigoProveedor");// son varchart su equivalente es el string 
			compra.add(new Compra(fecha, codigoProveedor));
		}

		return compra;

	}

	/*
	 * *****************************************************************************
	 * Insert
	 ******************************************************************************/
	public void insert(Compra compra) throws Throwable {

		conection.SQL("Insert into compra( fecha,codigoProveedor) VALUES(?,?)");
		conection.preparedStatement().setDate(1, compra.getFecha());
		conection.preparedStatement().setInt(2, compra.getCodigoProveedor());
		conection.CUD();

	}

	/*
	 * *****************************************************************************
	 * Search
	 ******************************************************************************/

	public void search(Compra compra) throws Throwable {

		ResultSet rs;

		conection.SQL("Select * from compra where fecha=?");
		conection.preparedStatement().setDate(1, compra.getFecha());
		rs = conection.resultSet();

		while (rs.next()) {

			compra.setFecha(rs.getDate("fecha"));
		}

		rs.close();

	}

	/*
	 * *****************************************************************************
	 * Update
	 ******************************************************************************/

	public void update(Compra compra) throws Throwable {
		Date fecha;
		int codigoProveedor;
		if (compra != null) {
			fecha = compra.getFecha();
			 codigoProveedor= compra.getCodigoProveedor();

			conection.SQL("Update compra set fecha = ? where codigoProveedor=?");
			conection.preparedStatement().setDate(1, fecha);
			conection.preparedStatement().setInt(2, codigoProveedor);
			conection.CUD();
		}
	}

	
}
package com.proyecto_final.control;

import java.util.ArrayList;

import javax.swing.text.html.parser.Entity;

public interface Control<Entity> {

	public ArrayList<Entity> list() throws Throwable;
	public void insert(Entity entity) throws Throwable;
	public void search(Entity entity) throws Throwable;
	public void update(Entity entity) throws Throwable;
}
package com.proyecto_final.control;

import java.sql.ResultSet;
import java.util.ArrayList;
import com.proyecto_final.entity.DetalleVenta;


public class DetalleVentaCtrl implements Control<DetalleVenta> {

private conexion conection;
	
	public DetalleVentaCtrl (conexion conection) {
		this.conection = conection;
	}

	/*
	 * *****************************************************************************
	 * List
	 ******************************************************************************/
	public ArrayList<DetalleVenta> list() throws Throwable {
		ArrayList<DetalleVenta> detalleVenta = new ArrayList<DetalleVenta>();
		ResultSet rs;
		int codigo;
		int codigoProducto;
		int cantidad;
		int codigoVenta;

		conection.SQL("Select * from detalleventa");

		rs = conection.resultSet();

		while (rs.next()) {
			codigo = rs.getInt("código");
			codigoProducto = rs.getInt("codigoProducto");
			cantidad = rs.getInt("cantidad");
			codigoVenta = rs.getInt("codigoVenta");
			detalleVenta.add(new DetalleVenta(codigo,codigoProducto, cantidad));
		}
		return detalleVenta;
	}	/*
	 * *****************************************************************************
	 * Insert
	 ******************************************************************************/
	public void insert(DetalleVenta detalleVenta) throws Throwable {

		conection.SQL("Insert into detalleventa(codigoProducto,cantidad) VALUES(?,?)");
		conection.preparedStatement().setInt(1, detalleVenta.getCodigoProducto());
		conection.preparedStatement().setInt(2, detalleVenta.getCantidad());
		conection.preparedStatement().setInt(3, detalleVenta.getCodigoVenta());
		conection.CUD();
	}
	/*
	 * *****************************************************************************
	 * Search
	 ******************************************************************************/
	public void search(DetalleVenta detalleVenta) throws Throwable {

		ResultSet rs;

		conection.SQL("Select * from detalleventa where codigo=?");
		conection.preparedStatement().setInt(1, detalleVenta.getCodigoVenta());
		detalleVenta.getCodigoProducto();
		detalleVenta.getCantidad();
	
		rs = conection.resultSet();

		while (rs.next()) {

			detalleVenta.setCodigoVenta(rs.getInt("codigoProducto"));
			detalleVenta.setCantidad(rs.getInt("cantidad"));			
		}
		rs.close();
	}
	/*
	 * *****************************************************************************
	 * Update
 ******************************************************************************/
	public void update(DetalleVenta detalleVenta) throws Throwable {
		int codigo;
		int codigoProducto;
		int cantidad;
		if (detalleVenta != null) {
			codigo = detalleVenta.getCodigoDetalleVenta();
			codigoProducto = detalleVenta.getCodigoProducto();
			cantidad = detalleVenta.getCantidad();

			conection.SQL("Update detalleventa set codigoProducto = ?, cantidad = ? where codigoVenta=?");
			conection.preparedStatement().setInt(1,  codigo);
			conection.preparedStatement().setInt(2, codigoProducto);
			conection.preparedStatement().setInt(3, cantidad);
			conection.CUD();
		}
	}

}
package com.proyecto_final.control;


import java.sql.Date;
import java.sql.ResultSet;
import java.util.ArrayList;


import com.proyecto_final.entity.Empleados;

public class EmpleadoCtrl implements Control<Empleados>{
	
	private conexion conection;
	
	public EmpleadoCtrl (conexion conection) {
		this.conection = conection;
	}

	/*
	 * *****************************************************************************
	 * List
	 ******************************************************************************/
	public ArrayList<Empleados> list() throws Throwable {
		ArrayList<Empleados> empleado = new ArrayList<Empleados>();
		ResultSet rs;
		int codigo;
		int carnet_de_identidad;
		String nombre;
		Date fechaNacimiento;
		String  nacionalidad;
		int telefono;
		String direccion;
		String correo;

		conection.SQL("Select * from empleados");

		rs = conection.resultSet();

		while (rs.next()) {
			codigo = rs.getInt("codigo");
			carnet_de_identidad = rs.getInt("carnet_de_identidad");// son varchart su equivalente es el string 
			nombre= rs.getString("nombre");
			fechaNacimiento = rs.getDate("fechaNacimiento");
			nacionalidad = rs.getString("nacionalidad");
			telefono = rs.getInt("telefono");
			direccion = rs.getString("direccion");
			correo = rs.getString("correo");
			empleado.add(new Empleados());
		}

		return empleado;

	}

	/*
	 * *****************************************************************************
	 * Insert
	 ******************************************************************************/
	public void insert(Empleados empleado) throws Throwable {

		conection.SQL("Insert into empleado(carnet_de_identidad,nombre, fechaNacimiento, nacionalidad, telefono, direccion, correo) VALUES(?,?)");
		conection.preparedStatement().setInt(1, empleado.getCarnetIdentidad());
		conection.preparedStatement().setString(2,empleado.getNombre());
		conection.preparedStatement().setDate(3, empleado.getFechaNacimiento());
		conection.preparedStatement().setString(4, empleado.getNacionalidad());
		conection.preparedStatement().setInt(5, empleado.getTelefono());
		conection.preparedStatement().setString(6, empleado.getDireccion());
		conection.preparedStatement().setString(7, empleado.getEmail());
		conection.CUD();

	}

	/*
	 * *****************************************************************************
	 * Search
	 ******************************************************************************/

	public void search(Empleados empleado) throws Throwable {

		ResultSet rs;

		conection.SQL("Select * from empleados where carnet=?");
		conection.preparedStatement().setInt(1, empleado.getCarnetIdentidad());
		rs = conection.resultSet();

		while (rs.next()) {

			empleado.setNombre(rs.getString("nombre"));
		}

		rs.close();
		}
	/*
	 * *****************************************************************************
	 * Update
	 ******************************************************************************/

	public void update(Empleados empleado) throws Throwable {
		int carnet_de_identidad;
		String nombre;
		Date fechaNacimiento;
		String nacionalidad;
		if (empleado != null) {
			carnet_de_identidad= empleado.getCarnetIdentidad();
			nombre = empleado.getNombre();
			fechaNacimiento= empleado.getFechaNacimiento();
			nacionalidad= empleado.getNacionalidad();

			conection.SQL("Update empleado set nombre = ? where carnet de identidad=?");
			conection.preparedStatement().setString(1, nombre);
			conection.preparedStatement().setInt(2, carnet_de_identidad);
			conection.CUD();
		}
	} 
package com.proyecto_final.control;

import java.sql.ResultSet;
import java.util.ArrayList;
import java.util.Date;
import com.proyecto_final.entity.Venta;

public class VentaCtrl implements Control <Venta>{
private conexion conection;
	
	public VentaCtrl (conexion conection) {
		this.conection = conection;
	}	/*
	 * *****************************************************************************
	 * List
	 ******************************************************************************/
	public ArrayList<Venta> list() throws Throwable {
		ArrayList<Venta> Ventas = new ArrayList<Venta>();
		ResultSet rs;
		int codigo;
		int NIT;
		String nombre;
		Date fecha;
		int codigoProducto;
		int cantidad;
		

		conection.SQL("Select * from Venta");

		rs = conection.resultSet();

		while (rs.next()) {
			codigo = rs.getInt("codigo");
			NIT = rs.getInt("NIT");
			nombre = rs.getString("nombre");
			fecha = rs.getDate("fecha");
			codigoProducto = rs.getInt("codigoProducto");
			cantidad = rs.getInt("cantidad");			
				
			Ventas.add(new Venta (codigo, NIT,nombre,(java.sql.Date) fecha, codigoProducto,cantidad));
		}

		return Ventas;

	}

	/*
	 * *****************************************************************************
	 * Insert
	 ******************************************************************************/
	public void insert(Venta Venta) throws Throwable {

		conection.SQL("Insert into Venta(NIT,nombre, codigoProducto,fecha, cantidad) VALUES(?,?)");
		conection.preparedStatement().setInt(1, Venta.getNIT());
		conection.preparedStatement().setString(2, Venta.getNombre());
		conection.preparedStatement().setDate(3, (java.sql.Date) Venta.getFecha());
		conection.preparedStatement().setInt(4, Venta.getCodigoProducto());
		conection.preparedStatement().setInt(5, Venta.getCantidad());
		conection.CUD();

	}

	/*
	 * *****************************************************************************
	 * Search
	 ******************************************************************************/

	public void search(Venta Venta) throws Throwable {

		ResultSet rs;

		conection.SQL("Select * from Venta where NIT=?");
		conection.preparedStatement().setInt(1, Venta.getCodigoVenta());
		Venta.setNIT(0);
		Venta.setNombre(null);
		
		rs = conection.resultSet();

		while (rs.next()) {

			Venta.setNIT(rs.getInt("NIT"));
			Venta.setNombre(rs.getString("nombre"));
			
		}

		rs.close();

	}

	/*
	 * *****************************************************************************
	 * Update
	 ******************************************************************************/

	public void update(Venta Venta) throws Throwable {
		int código;
		Date fecha;
		int NIT;
		if (Venta != null) {
			código = Venta.getCodigoVenta();
			fecha = Venta.getFecha();
			NIT = Venta.getNIT();

			conection.SQL("Update Venta set fecha = ?, NIT = ? where codigo=?");
			conection.preparedStatement().setDate(1, (java.sql.Date) fecha);
			conection.preparedStatement().setInt(2, NIT);
			conection.preparedStatement().setInt(3, código);
			conection.CUD();
		}
	}

	
}




package com.proyecto_final.entity;

public class Categoria {
	private int codigo_categoria;
	private String nombre;
	private String descripcion;
	
	
	
		

	public Categoria(int codigo_categoria, String nombre, String descripcion) {
		super();
		this.codigo_categoria = codigo_categoria;
		this.nombre = nombre;
		this.descripcion = descripcion;
	}

	
	public int getCodigoCategoria() {
		return codigo_categoria;
	}

	public void setCodigoCategoria(int codigoCategoria) {
		this.codigo_categoria = codigoCategoria;
	}

	public String getNombre() {
		return nombre;
	}

	public void setNombre(String nombre) {
		this.nombre = nombre;
	}
	
	
	public String getDescripcion() {
		return descripcion;
	}

	public void setDescripcion(String descripcion) {
		this.descripcion = descripcion;
	}


	public boolean es(int codigo){
		return this.codigo_categoria == codigo;
	}
	
	@Override
	public String toString() {
		
		return "Categoria [codigo_categoria=" + codigo_categoria + ", nombre=" + nombre + "]";
	
	
		

}

}
package com.proyecto_final.entity;

public class cliente {
	private String NIT;
	private String nombre;
	private String direccion;
	private String correo;
	private String telefono;
	private String ciudad;
	private String departamento;
	private String apellido;
	

	public cliente (String NIT, String nombre){
		this.NIT = "";
		this.nombre = "";
		this.apellido="";
		this.direccion ="" ;
		this.correo = "";
		this.telefono="";
		this.ciudad = "";
		this.departamento= "";
				
	}
	public String getNIT() {
		return NIT;
	}
	public void setNIT(String NIT) {
		this.NIT = NIT;
	}

	
	public String getNombre() {
		return nombre;
	}
	public void setNombre(String nombre) {
		this.nombre = nombre;
	}

	
	
	public String getApellido() {
		return apellido;
	}
	public void setApellido(String apellido) {
		this.apellido = apellido;
	}
	

	
	public String getDireccion() {
		return direccion;
	}
	public void setDireccion(String direccion) {
		this.direccion = direccion;
	}

	
	
	public String getCorreo() {
		return correo;
	}
	public void setCorreo(String correo) {
		this.correo = correo;
	}
	
	
	
	public String getCiudad() {
		return ciudad;
	}
	public void setCiudad(String direccion) {
		this.direccion = direccion;
	}

	
	
		public String getTelefono() {
		return telefono;
		}
		public void setTelefono(String telefono) {
		this.telefono = telefono;
		}

		
	
		public String getDepartamento() {
			return departamento;
		}
		public void setDepartamento(String departamento) {
			this.departamento = departamento;
		}
		
		@Override
		public String toString() {
			return "Cliente [NIT=" + NIT + ", nombre=" + nombre + "]";
		}
		

		
	
}package com.proyecto_final.entity;

import java.sql.Date;
import java.text.SimpleDateFormat;

public class Compra {
	private int codigo;
	private Date fecha;
	private int codigoProveedor;

	public Compra (Date fecha, int codigoProveedor){
		this.codigo = 0;
		this.fecha=null;
		this.codigoProveedor=0;

	}
	
	public int getCodigoCompra() {
		return codigo;
	}

	public void setCodigoCompra(int codigo) {
		this.codigo = codigo;
	}

	public Date getFecha() {
		return fecha;
	}

	public void setFecha(Date fecha) {
		this.fecha = fecha;
	}

	public int getCodigoProveedor() {
		return codigoProveedor;
	}

	public void setCodigoProveedor(int CodigoProveedores) {
		codigoProveedor = codigoProveedor;
	}

	public boolean es(int codigo){
		return this.codigo == codigo;
	}	
	
	
	@Override
	public String toString() {
		SimpleDateFormat formato = new SimpleDateFormat ("dd/MM/yyyy");
		
		return "Compra [codigo_compra=" + codigo + ", fecha=" + formato.format(fecha) + "]";
	}
	
	

}
package com.proyecto_final.entity;

import java.sql.Date;
import java.text.SimpleDateFormat;

public class Empleados {
	private int codigo;
	private int carnet_de_identidad;
	private String nombre;
	private Date fechaNacimiento;
	private String nacionalidad;
	private int telefono;
	private String direccion;
	private String correo;
	

public Empleados (){
		this.codigo = 0;
		this.nombre = "";
		this.direccion="";
		this.fechaNacimiento=null;
		this.nacionalidad ="";
		this.telefono=0;
		this.correo="";
		this.carnet_de_identidad=0;
	}

	public int getCodigoEmpleado() {
		return codigo;
	}
	public void setCodigoEmpleado(int codigo) {
		this.codigo = codigo;
	}
	
	public int getCarnetIdentidad() {
		return carnet_de_identidad;
	}
	public void setCarnetIdentidad(int carnet_de_identidad) {
		this.carnet_de_identidad = carnet_de_identidad;
	}

	public String getNombre() {
		return nombre;
	}
	public void setNombre(String nombre) {
		this.nombre = nombre;
	}

	
	public String getNacionalidad() {
		return nacionalidad;
	}
	public void setNacionalidad(String nacionalidad) {
		this.nacionalidad = nacionalidad;
	}

	
	public String getDireccion() {
		return direccion;
	}
	public void setDireccion(String direccion) {
		this.direccion = direccion;
	}

	
	public Date getFechaNacimiento() {
		return fechaNacimiento;
	}
	public void setFechaNacimiento(Date fechaNacimiento) {
		this.fechaNacimiento = fechaNacimiento;
	}

	

	public int getTelefono() {
		return telefono;
	}
	public void setTelefono(int telefono) {
		this.telefono = telefono;
	}

	
	public String getEmail() {
		return correo;
	}
	public void setEmail(String correo) {
		this.correo =correo;
	}
	
	public boolean es(int codigo){
		return this.codigo == codigo;
	}
	
	@Override
	public String toString() {
		SimpleDateFormat formato = new SimpleDateFormat ("dd/MM/yyyy");
		
		return "empleado [codigo_empleado=" + codigo + ", nombre=" + nombre +  ",direccion=" + direccion
  			+ ", fecha_de_nacimiento=" + formato.format(fechaNacimiento)  + ", telefono=" + telefono + ",email=" + correo
  			+ "]"  ;
	}
	
}
ackage com.proyecto_final.entity;


import java.text.SimpleDateFormat;

import java.util.Date;

public class Producto {
	private int codigoProducto;
	private String producto;
	private double precio_unidad;
	private Date fechaVencimiento;
	private int stock;
	private int codigoCategoria;
	
	

	public Producto (String producto2, int precio_unidad2, int codigoCategoria2){
		this.codigoProducto = 0;
		this.producto = "";
		this.precio_unidad = 0.0;
		this.stock=0;
		this.fechaVencimiento=null;
		this.codigoCategoria= 0;
	}
	
	public int getCodigoProducto() {
		return codigoProducto;
	}

	public void setCodigoProducto(int códigoProducto) {
		this.codigoProducto = códigoProducto;
	}

	
	public String getNombre() {
		return producto;
	}
	public void setNombre(String producto) {
		this.producto = producto;
	}

	public double getPrecio() {
		return precio_unidad;
	}

	public void setPrecio(double precio) {
		this.precio_unidad = precio;
	}

	public int getStock() {
		return stock;
	}

	public void setStock(int stock) {
		this.stock = stock;
	}

	
	public Date getFechaVencimiento() {
		return fechaVencimiento;
	}
	public void setFechaVencimiento(Date fechaVencimiento) {
		this.fechaVencimiento = fechaVencimiento;
	}
	
	
	public int getCodigoCategoria() {
		return codigoCategoria;
	}
	public void setCodigoCategoria(int codigoCategoria) {
		this.codigoCategoria = codigoCategoria;
	}

	
	public boolean es(int codigo){
		return this.codigoProducto == codigo;
	}
	
	public String toString() {
		SimpleDateFormat formato = new SimpleDateFormat ("dd/MM/yyyy");
		
		return "Producto [códigoProducto=" + codigoProducto + ", nombre=" + producto + ", precio=" + precio_unidad
				+  ", fechaVencimiento=" + formato.format(fechaVencimiento) + "]";
	}
	
	
}
package com.proyecto_final.entity;

import java.sql.Date;
import java.text.SimpleDateFormat;

public class Venta {
	private int codigo;
	private int NIT;
	private String nombre;
	private Date fecha;
	private int codigoProducto;
	private int cantidad;
	

	public Venta(int codigo, int NIT, String nombre, Date fecha, int codigoProducto, int cantidad) {
		
		this.codigo = codigo;
		NIT = NIT;
		this.nombre = nombre;
		this.fecha = fecha;
		this.codigoProducto = codigoProducto;
		this.cantidad = cantidad;
	}


	public int getCodigoVenta() {
		return codigo;
	}
	public void setCodigoVenta(int codigoVenta) {
		this.codigo = codigoVenta;
	}
	

	public String getNombre() {
		return nombre;
	}
	public void setNombre(String nombre) {
		this.nombre= nombre;
	}
	
	
	
	public Date getFecha() {
		return fecha;
	}
	public void setFecha(Date fecha) {
		this.fecha = fecha;
	}
	
	
	
	public int getNIT() {
		return NIT;
	}
	public void setNIT(int NIT) {
		this.NIT = NIT;
	}
	
	

	public int getCodigoProducto() {
		return codigoProducto;
	}
	public void setCodigoProducto(int codigoProducto) {
		this.codigoProducto = codigoProducto;
	}
	
	
	public int getCantidad() {
		return cantidad;
	}
	public void setCantidad(int cantidad) {
		this.cantidad = cantidad;
	}
	

	public boolean es(int codigo){
		return this.codigo == codigo;
	}
	
	@Override
	public String toString() {
		SimpleDateFormat formato = new SimpleDateFormat ("dd/MM/yyyy");
		
		return "Venta [codigoVenta=" + codigo + ", fecha=" + formato.format(fecha) + "]";
	}
	
	


}
package com.proyecto_final.entity;

import java.sql.Date;
import java.text.SimpleDateFormat;

public class Venta {
	private int codigo;
	private int NIT;
	private String nombre;
	private Date fecha;
	private int codigoProducto;
	private int cantidad;
	

	public Venta(int codigo, int NIT, String nombre, Date fecha, int codigoProducto, int cantidad) {
		
		this.codigo = codigo;
		NIT = NIT;
		this.nombre = nombre;
		this.fecha = fecha;
		this.codigoProducto = codigoProducto;
		this.cantidad = cantidad;
	}


	public int getCodigoVenta() {
		return codigo;
	}
	public void setCodigoVenta(int codigoVenta) {
		this.codigo = codigoVenta;
	}
	

	public String getNombre() {
		return nombre;
	}
	public void setNombre(String nombre) {
		this.nombre= nombre;
	}
	
	
	
	public Date getFecha() {
		return fecha;
	}
	public void setFecha(Date fecha) {
		this.fecha = fecha;
	}
	
	
	
	public int getNIT() {
		return NIT;
	}
	public void setNIT(int NIT) {
		this.NIT = NIT;
	}
	
	

	public int getCodigoProducto() {
		return codigoProducto;
	}
	public void setCodigoProducto(int codigoProducto) {
		this.codigoProducto = codigoProducto;
	}
	
	
	public int getCantidad() {
		return cantidad;
	}
	public void setCantidad(int cantidad) {
		this.cantidad = cantidad;
	}
	

	public boolean es(int codigo){
		return this.codigo == codigo;
	}
	
	@Override
	public String toString() {
		SimpleDateFormat formato = new SimpleDateFormat ("dd/MM/yyyy");
		
		return "Venta [codigoVenta=" + codigo + ", fecha=" + formato.format(fecha) + "]";
	}
	
	


}
ackage com.proyecto_final.view;

import java.text.DateFormat;
import java.text.ParseException;
import java.text.SimpleDateFormat;
import java.util.Date;
import java.util.Scanner;

public class ReadTypes {
	public static Date leerFecha(Scanner scanner, String msg) {
		Date fecha;
		DateFormat formato = new SimpleDateFormat("dd/MM/yyyy");

		while (true) {
			System.out.print(msg);
			try {
				fecha = formato.parse(scanner.nextLine());
				return fecha;
			} catch (ParseException e) {
				System.out.println("Error en el formato de fecha");
			}
		}
	}

	public static java.sql.Date cASqlDate (java.util.Date uDate) {
		return new java.sql.Date(uDate.getTime());
        
    }
	
	public static int leerEntero(Scanner scanner, String msg) {
		int numero = 0;

		while (true) {
			System.out.print(msg);
			try {
				numero = scanner.nextInt();
				scanner.nextLine();
				return numero;
			} catch (java.util.InputMismatchException e) {
				scanner.nextLine();
				System.out.println(" ERROR EN FORMATO NUMÉRICO!! ");
			}
		}
	}
	
	public static double leerReal(Scanner scanner, String msg) {
		double numero = 0;

		while (true) {
			System.out.print(msg);
			try {
				numero = scanner.nextDouble();
				scanner.nextLine();
				return numero;
			} catch (java.util.InputMismatchException e) {
				scanner.nextLine();
				System.out.println(" ERROR EN FORMATO NUMÉRICO!! ");
			}
		}
	}
	
	public static String leerCadena(Scanner scanner, String msg) {
		String cadena = "";

		while (true) {
			System.out.print(msg);
			try {
				cadena = scanner.nextLine();
				return cadena;
			} catch (java.util.InputMismatchException e) {
				System.out.println("ERROR EN FORMATO DE CADENA!!");
			}
		}
	}


}
package com.proyecto_final.view;

import java.sql.SQLException;
import java.util.Scanner;

import com.proyecto_final.control.conexion;
import com.proyecto_final.view.menu.Principal;

public class Pantalla {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		Scanner scanner = new Scanner(System.in);
		conexion conection = new conexion();
		Principal.subMenu(scanner,  "");		
			
		
		scanner.close();
		
	}

}
