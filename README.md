# 📘LATIHAN
## Factory Constructors - Sesi 10

## 👤 Identitas
### Nama       : Aneka Lisda 
### NIM        : 25141013P
### Kelas      : SI2KR
### Mata Kuliah: Pemrograman Berbasis Objek  
---
### Factory Constructor pada Dart adalah constructor khusus yang menggunakan keyword factory dan tidak selalu membuat object baru.

## 📌Factory Constructors bisa:

#### Mengembalikan object yang sudah ada (cache/singleton)
#### Mengembalikan subclass
#### Melakukan validasi sebelum object dibuat

## 🎯Kegunaan Factory Constructor
#### Singleton Pattern
#### Cache Object
#### Object Pooling
#### Validation
#### Subclass Selection

---

## Latihan
---
### 1. ConnectionFactory dengan Pooling Koneksi Database
```dart
class ConnectionFactory {
  final String host;
  final int port;

  static final Map<String, ConnectionFactory> _pool = {};

  // Private constructor
  ConnectionFactory._(this.host, this.port);

  // Factory constructor
  factory ConnectionFactory(String host, int port) {
    String key = '$host:$port';

    if (_pool.containsKey(key)) {
      print('Menggunakan koneksi dari pool');
      return _pool[key]!;
    }

    print('Membuat koneksi baru');
    var connection = ConnectionFactory._(host, port);
    _pool[key] = connection;

    return connection;
  }

  void connect() {
    print('Terhubung ke database $host:$port');
  }
}

void main() {
  var db1 = ConnectionFactory('localhost', 3306);
  var db2 = ConnectionFactory('localhost', 3306);

  print(identical(db1, db2)); // true

  db1.connect();
}
```
[Klik di sini untuk menjalankan kode DartPad](https://dartpad.dev/cfdc332f7285067dff82d8c9d9e64965)

---
### 2. NotificationFactory berdasarkan Platform
```dart
abstract class NotificationService {
  void send(String message);

  factory NotificationService(String platform) {
    switch (platform.toLowerCase()) {
      case 'android':
        return AndroidNotification();

      case 'ios':
        return IOSNotification();

      case 'web':
        return WebNotification();

      default:
        throw ArgumentError('Platform tidak dikenali');
    }
  }
}

class AndroidNotification implements NotificationService {
  @override
  void send(String message) {
    print('Android Notification: $message');
  }
}

class IOSNotification implements NotificationService {
  @override
  void send(String message) {
    print('IOS Notification: $message');
  }
}

class WebNotification implements NotificationService {
  @override
  void send(String message) {
    print('Web Notification: $message');
  }
}

void main() {
  var notif = NotificationService('android');
  notif.send('Halo pengguna Android');
}
```
[Klik di sini untuk menjalankan kode DartPad](https://dartpad.dev/bc031434fd93090545d456f7aa5a1a83)
---

### 3. ShapeFactory dengan Cache
```dart
abstract class Shape {
  void draw();

  static final Map<String, Shape> _cache = {};

  factory Shape(String type) {
    if (_cache.containsKey(type)) {
      print('Mengambil shape dari cache');
      return _cache[type]!;
    }

    late Shape shape;

    switch (type.toLowerCase()) {
      case 'circle':
        shape = Circle();
        break;

      case 'square':
        shape = Square();
        break;

      default:
        throw ArgumentError('Shape tidak tersedia');
    }

    _cache[type] = shape;

    return shape;
  }
}

class Circle implements Shape {
  @override
  void draw() {
    print('Menggambar Circle');
  }
}

class Square implements Shape {
  @override
  void draw() {
    print('Menggambar Square');
  }
}

void main() {
  var s1 = Shape('circle');
  var s2 = Shape('circle');

  print(identical(s1, s2)); // true
}
```
[Klik di sini untuk menjalankan kode DartPad](https://dartpad.dev/d79c23ce03a57299795b9b4447006385)
---
# 📘CHALLENGE

## Berikut AnimalFactory dengan :
### Vactory
### Validation
### Chace
### Subclass Selection

```dart
abstract class Animal {
  String name;
  int age;
  String category;

  Animal(this.name, this.age, this.category);

  void sound();
}

// ================= FACTORY =================

class AnimalFactory {

  // Cache
  static final Map<String, Animal> _cache = {};

  static Animal createAnimal(
    String type,
    String name,
    int age,
    String category,
  ) {

    // ================= VALIDATION =================

    if (name.isEmpty) {
      throw ArgumentError('Nama tidak boleh kosong');
    }

    if (age < 0) {
      throw ArgumentError('Umur tidak valid');
    }

    if (category.isEmpty) {
      throw ArgumentError('Kategori tidak boleh kosong');
    }

    // Key cache
    String key = '$type-$name-$age-$category';

    // ================= CACHE =================

    if (_cache.containsKey(key)) {
      print('Mengambil hewan dari cache');
      return _cache[key]!;
    }

    late Animal animal;

    // ================= SUBCLASS SELECTION =================

    switch (type.toLowerCase()) {

      // Mamalia
      case 'cat':
        animal = Cat(name, age, category);
        break;

      case 'dog':
        animal = Dog(name, age, category);
        break;

      // Unggas
      case 'bird':
        animal = Bird(name, age, category);
        break;

      // Hewan air
      case 'fish':
        animal = Fish(name, age, category);
        break;

      default:
        throw ArgumentError('Jenis hewan tidak tersedia');
    }

    // Simpan ke cache
    _cache[key] = animal;

    return animal;
  }
}

// ================= SUBCLASS =================

// Mamalia
class Cat extends Animal {
  Cat(String name, int age, String category)
      : super(name, age, category);

  @override
  void sound() {
    print('$name ($category) berkata: Meow');
  }
}

class Dog extends Animal {
  Dog(String name, int age, String category)
      : super(name, age, category);

  @override
  void sound() {
    print('$name ($category) berkata: Woof');
  }
}

// Unggas
class Bird extends Animal {
  Bird(String name, int age, String category)
      : super(name, age, category);

  @override
  void sound() {
    print('$name ($category) berkata: Tweet');
  }
}

// Hewan Air
class Fish extends Animal {
  Fish(String name, int age, String category)
      : super(name, age, category);

  @override
  void sound() {
    print('$name ($category) berkata: Blub Blub');
  }
}

// ================= MAIN =================

void main() {

  var cat1 = AnimalFactory.createAnimal(
    'cat',
    'Milo',
    2,
    'Mamalia',
  );

  var cat2 = AnimalFactory.createAnimal(
    'cat',
    'Milo',
    2,
    'Mamalia',
  );

  var bird = AnimalFactory.createAnimal(
    'bird',
    'Rio',
    1,
    'Unggas',
  );

  var fish = AnimalFactory.createAnimal(
    'fish',
    'Nemo',
    1,
    'Hewan Air',
  );

  // Test cache
  print(identical(cat1, cat2));

  // Output suara
  cat1.sound();
  bird.sound();
  fish.sound();
}
```
[Klik di sini untuk menjalankan kode DartPad](https://dartpad.dev/454861c2baee79060d5ee37f4bd94894)
---
