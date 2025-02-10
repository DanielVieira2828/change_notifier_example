# Exemplo de ChangeNotifier no Flutter para troca de tema

Este repositório demonstra o uso do `ChangeNotifier` no Flutter para gerenciar o tema do aplicativo. O exemplo permite alternar entre o tema claro e escuro através de um `Switch`.

---

## 📂 Estrutura do Projeto

O projeto consiste em três arquivos principais:

- `main.dart`: Arquivo principal que inicializa o aplicativo e define o tema.
- `my_home_page.dart`: Widget da página inicial que contém o `Switch` para alternar o tema.
- `theme_controller.dart`: Classe `ThemeController` que gerencia o estado do tema com um `ChangeNotifier`.

---

## 💻 Código

### `main.dart`

```dart
import 'package:change_notifier_state/presentation/controllers/theme_controller.dart';
import 'package:change_notifier_state/presentation/pages/my_home_page.dart';
import 'package:flutter/material.dart';

void main() {
  runApp(const MyApp());
}

final themeController = ThemeController();

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return ListenableBuilder(
      listenable: themeController,
      builder: (context, child) {
        return MaterialApp(
          title: 'Flutter Demo',
          theme: themeController.isDarkTheme
              ? ThemeData.dark()
              : ThemeData.light(),
          home: const MyHomePage(title: 'Flutter Demo Home Page'),
        );
      },
    );
  }
}

```

Neste arquivo, o `ListenableBuilder` escuta as mudanças no `themeController`. Sempre que o valor do tema é alterado, o `MaterialApp` é reconstruído com o novo tema.

### `my_home_page.dart`

```dart

import 'package:change_notifier_state/main.dart';
import 'package:flutter/material.dart';

class MyHomePage extends StatefulWidget {
  const MyHomePage({super.key, required this.title});
  final String title;

  @override
  State<MyHomePage> createState() => _MyHomePageState();
}

class _MyHomePageState extends State<MyHomePage> {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text("ChangeNotifier"),
      ),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            const Text("Trocar tema do app"),
            ListenableBuilder(
              listenable: themeController,
              builder: (context, child) {
                return Switch(
                  value: themeController.isDarkTheme,
                  onChanged: (value) {
                    themeController.changeTheme();
                  },
                );
              },
            )
          ],
        ),
      ),
    );
  }
}

```

Aqui, o `ListenableBuilder` escuta o `themeController` para atualizar o estado do `Switch`. Quando o `Switch` é alterado, o método `themeController.changeTheme()` é chamado.

### `theme_controller.dart`

```dart

import 'package:flutter/material.dart';

class ThemeController extends ChangeNotifier {
  bool isDarkTheme = false;

  void changeTheme() {
    isDarkTheme = !isDarkTheme;
    notifyListeners();
  }
}


```

A classe `ThemeController` estende `ChangeNotifier` e gerencia o estado do tema. O método `changeTheme()` inverte o valor atual do tema e chama `notifyListeners()`, garantindo que os widgets ouvintes sejam reconstruídos.

---

## ℹ️ `Explicação`

O `ChangeNotifier` é uma classe que permite gerenciar estados mais complexos no Flutter. Ele fornece o método `notifyListeners()`, que informa aos widgets que dependem dele que houve uma alteração no estado.

Neste exemplo, o `ThemeController` usa uma variável `isDarkTheme` para armazenar o estado do tema. Sempre que `changeTheme()` é chamado, `notifyListeners()` dispara uma atualização nos widgets que estão escutando essa mudança.
