import 'package:firebase_auth/firebase_auth.dart';
import 'package:firebase_database/firebase_database.dart';
import 'package:flutter/material.dart';
import 'package:lite_rolling_switch/lite_rolling_switch.dart';
import 'inicio_sesion.dart';

class RegistroScreen extends StatefulWidget {
  @override
  _RegistroScreenState createState() => _RegistroScreenState();
}

class _RegistroScreenState extends State<RegistroScreen> {
  final TextEditingController emailController = TextEditingController();
  final TextEditingController passwordController = TextEditingController();
  bool esAdmin = false; // Para el switch de empleado/administrador
  final FirebaseAuth _auth = FirebaseAuth.instance;
  final DatabaseReference _databaseRef = FirebaseDatabase.instance.ref();

  String convertEmailToPath(String email) {
    return email.replaceAll(RegExp(r'[.#$[\]]'), '-').replaceAll('@', '-');
  }

  void registrarUsuario() async {
    try {
      UserCredential userCredential =
          await _auth.createUserWithEmailAndPassword(
        email: emailController.text.trim(),
        password: passwordController.text,
      );
      String userId = userCredential.user!.uid;

      // Guardar datos adicionales en Firebase Realtime Database
      _databaseRef.child('usuarios').child(userId).set({
        'correo': emailController.text.trim(),
        'esAdmin': esAdmin,
      });

      // Redirigir a la pantalla de inicio de sesión
      Navigator.pushReplacement(
        context,
        MaterialPageRoute(builder: (context) => InicioSesion()),
      );
    } catch (e) {
      print("Error al registrar usuario: $e");
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text("Registro"), backgroundColor: Colors.pink),
      body: Container(
        decoration: BoxDecoration(
          gradient: LinearGradient(
            colors: [Colors.pink.shade100, Colors.pink.shade300],
            begin: Alignment.topCenter,
            end: Alignment.bottomCenter,
          ),
        ),
        child: Padding(
          padding: EdgeInsets.all(20.0),
          child: Column(
            mainAxisAlignment: MainAxisAlignment.center,
            children: [
              TextField(
                controller: emailController,
                decoration: InputDecoration(
                  labelText: "Correo electrónico",
                  filled: true,
                  fillColor: Colors.white.withOpacity(0.8),
                  border: OutlineInputBorder(
                      borderRadius: BorderRadius.circular(10)),
                ),
                keyboardType: TextInputType.emailAddress,
              ),
              SizedBox(height: 10),
              TextField(
                controller: passwordController,
                decoration: InputDecoration(
                  labelText: "Contraseña",
                  filled: true,
                  fillColor: Colors.white.withOpacity(0.8),
                  border: OutlineInputBorder(
                      borderRadius: BorderRadius.circular(10)),
                ),
                obscureText: true,
              ),
              SizedBox(height: 20),
              LiteRollingSwitch(
                value: false,
                textOn: "Admin",
                textOff: "Empleado",
                colorOn: Colors.red,
                colorOff: Colors.blue,
                iconOn: Icons.admin_panel_settings,
                iconOff: Icons.person,
                onChanged: (bool value) {
                  setState(() {
                    esAdmin = value;
                  });
                },
                onTap: () {
                  print("Switch tocado");
                },
                onDoubleTap: () {
                  print("Doble toque en el switch");
                },
                onSwipe: () {
                  print("Swipe detectado en el switch");
                },
              ),
              SizedBox(height: 20),
              ElevatedButton(
                onPressed: registrarUsuario,
                style: ElevatedButton.styleFrom(
                  backgroundColor: Colors.pink,
                  padding: EdgeInsets.symmetric(horizontal: 40, vertical: 15),
                  shape: RoundedRectangleBorder(
                      borderRadius: BorderRadius.circular(10)),
                ),
                child:
                    Text("Registrarse", style: TextStyle(color: Colors.white)),
              ),
            ],
          ),
        ),
      ),
    );
  }
}
