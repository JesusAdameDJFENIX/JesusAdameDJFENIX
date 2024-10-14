1. Diseño de la Interfaz (activity_main.xml)

ConstraintLayout: Este es el contenedor principal que permite colocar los elementos de la interfaz de manera flexible y adaptativa.

EditText: Permite a los usuarios ingresar texto para buscar contactos. Está restringido por las reglas de diseño del ConstraintLayout para adaptarse a diferentes tamaños de pantalla.

ListView: Se utiliza para mostrar la lista de contactos. Está diseñado para ocupar el espacio restante de la pantalla bajo el campo de búsqueda.


<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    
    <EditText
        android:id="@+id/searchInput"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:hint="Buscar contacto..."
        android:padding="10dp"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintBottom_toTopOf="@id/listViewContactos"/>

    
    <ListView
        android:id="@+id/listViewContactos"
        android:layout_width="0dp"
        android:layout_height="0dp"
        app:layout_constraintTop_toBottomOf="@id/searchInput"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintEnd_toEndOf="parent"/>

</androidx.constraintlayout.widget.ConstraintLayout>

![image](https://github.com/user-attachments/assets/3ee1af32-0390-4441-8f81-5a34292c9fe5)


2. Código de la Actividad Principal (MainActivity.kt)

MainActivity: Esta clase es la actividad principal que controla la lógica de la aplicación.

onCreate: Este método se ejecuta cuando se crea la actividad. Se inicializa la lista de contactos y se establece el adaptador para el ListView.

listaContactos: Una lista de objetos ContactoModel que contiene información de los contactos.

TextWatcher: Se agrega a EditText para filtrar los contactos en tiempo real mientras el usuario escribe.

filtrarContactos: Este método toma la consulta de búsqueda y filtra la lista de contactos, actualizando el adaptador para mostrar solo los contactos que coinciden con la búsqueda.


package com.example.customadapter


import android.os.Bundle
import android.text.Editable
import android.text.TextWatcher
import android.widget.EditText
import android.widget.ListView
import androidx.appcompat.app.AppCompatActivity

class MainActivity : AppCompatActivity() {

    private lateinit var adapter: ContactAdapter
    private lateinit var listaContactos: List<ContactoModel>
    private lateinit var listView: ListView

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        
        listaContactos = listOf(
            ContactoModel("Jesus Adame", "123-456-7890", "jesus@gmail.com", R.drawable.juan),
            ContactoModel("Sofia Hernandez", "987-654-3210", "sofia@gmail.com", R.drawable.perfil_ana),
            ContactoModel("Gabriel Torres", "456-789-1234", "gabriel@gmail.com", R.drawable.perfil_carlos),
            ContactoModel("Ashley Mendoza", "123-456-7890", "ashley@gmail.com", R.drawable.perfil_sofia) // Duplicado
            
        )

        
        listView = findViewById(R.id.listViewContactos)

        
        adapter = ContactAdapter(this, listaContactos)
        listView.adapter = adapter

        
        val searchInput = findViewById<EditText>(R.id.searchInput)
        searchInput.addTextChangedListener(object : TextWatcher {
            override fun afterTextChanged(s: Editable?) {
                filtrarContactos(s.toString())
            }

            override fun beforeTextChanged(s: CharSequence?, start: Int, count: Int, after: Int) {}

            override fun onTextChanged(s: CharSequence?, start: Int, before: Int, count: Int) {}
        })
    }

    
    private fun filtrarContactos(query: String) {
        val contactosFiltrados = listaContactos.filter {
            it.nombre.contains(query, ignoreCase = true) || it.telefono.contains(query)
        }
        adapter = ContactAdapter(this, contactosFiltrados)
        listView.adapter = adapter
    }
}

![image](https://github.com/user-attachments/assets/d4636350-9c11-4aef-87c1-3e80ff061018)

![image](https://github.com/user-attachments/assets/35c222b6-38fe-476d-81f8-cd3c08bc084d)


3. Diseño del Elemento de la Lista (list_item_contact.xml)

list_item_contact.xml: Define cómo se verá cada elemento de la lista de contactos.

ImageView: Muestra la imagen de perfil del contacto.

TextViews: Muestran el nombre, teléfono y correo del contacto.

Button: Permite al usuario enviar un correo electrónico al contacto.


<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:padding="10dp">

    <ImageView
        android:id="@+id/imagenPerfil"
        android:layout_width="50dp"
        android:layout_height="50dp"
        android:src="@drawable/perfil"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintBottom_toBottomOf="parent"
        android:layout_marginEnd="16dp"
        tools:ignore="ContentDescription" />

    <TextView
        android:id="@+id/nombreContacto"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:textSize="18sp"
        android:textColor="#000000"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintStart_toEndOf="@id/imagenPerfil"
        app:layout_constraintEnd_toEndOf="parent"
        tools:text="Nombre del Contacto" />

    <TextView
        android:id="@+id/telefonoContacto"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:textSize="14sp"
        android:textColor="#555555"
        app:layout_constraintTop_toBottomOf="@id/nombreContacto"
        app:layout_constraintStart_toEndOf="@id/imagenPerfil"
        app:layout_constraintEnd_toEndOf="parent"
        tools:text="Teléfono" />

    <TextView
        android:id="@+id/emailContacto"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:textSize="14sp"
        android:textColor="#555555"
        app:layout_constraintTop_toBottomOf="@id/telefonoContacto"
        app:layout_constraintStart_toEndOf="@id/imagenPerfil"
        app:layout_constraintEnd_toEndOf="parent"
        tools:text="Correo" />

    <!-- Botón para enviar correo electrónico -->
    <Button
        android:id="@+id/btnEnviarCorreo"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Enviar Correo"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintTop_toBottomOf="@id/emailContacto"/>
</androidx.constraintlayout.widget.ConstraintLayout>


![image](https://github.com/user-attachments/assets/e17d4733-cd5c-4072-8403-934ffef161a0)

![image](https://github.com/user-attachments/assets/dc1182e7-d29e-46d8-932e-26a09ebf62bb)


4. Adaptador de Contactos (ContactAdapter.kt)

ContactAdapter: Esta clase extiende BaseAdapter y se encarga de gestionar la visualización de los contactos en el ListView.

getView: Este método crea la vista para cada contacto en la lista. Configura los textos, imágenes y la acción del botón.

Resaltado de Duplicados: Se verifica si hay contactos con el mismo número de teléfono. Si se encuentran duplicados, el nombre del contacto se resalta en rojo.

Enviar Correo: Al hacer clic en el botón de "Enviar Correo", se crea un Intent para abrir la aplicación de correo predeterminada, 
rellenando los campos de destinatario, asunto y mensaje. Si no hay aplicaciones de correo instaladas, se muestra un Toast informando al usuario.


![image](https://github.com/user-attachments/assets/996a8f92-0091-45b2-a7b6-7526f246639b)

![image](https://github.com/user-attachments/assets/fa763142-099f-42c0-8ad0-ea0d1d2c2c53)


5. Modelo de Contacto (ContactoModel.kt)

ContactoModel: Esta es una clase de datos que define las propiedades de un contacto: 
nombre, teléfono, correo electrónico e imagen de perfil.

![image](https://github.com/user-attachments/assets/22b859e4-b919-48d0-845a-dee159194ade)

package com.example.customadapter

data class ContactoModel(
    val nombre: String,
    val telefono: String,
    val email: String,
    val imagenPerfil: Int
)



Y por ultimo esta es el resultado
se puede observar que los que son de color gris son numeros de telefono repetidos 


Para Buscar contacto:

![ezgif-1-c455e730f8](https://github.com/user-attachments/assets/d9a04fe7-f759-4673-a869-3dcbea1c1705)



Paea darle click en el boton enviar correo:


![ezgif-1-9bda032618](https://github.com/user-attachments/assets/f3a984ec-1e2a-4eef-971e-06bdd94a9858)


Video De Explicación:




