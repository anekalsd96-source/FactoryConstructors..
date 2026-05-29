# 📘Factory Constructors - Sesi 10

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
[Klik di sini untuk menjalankan kode DartPad](https://dartpad.dev/9b4b28b1108d5fb1e02546d99409f402)
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
[Klik di sini untuk menjalankan kode DartPad](https://dartpad.dev/454861c2baee79060d5ee37f4bd94894)
---
