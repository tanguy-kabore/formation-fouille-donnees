# Module 10: Java pour le Big Data

## 🎯 Objectifs
- Manipuler des fichiers en Java
- Utiliser le multithreading
- Se connecter aux bases de données avec JDBC
- Introduction à Hadoop

---

## 1. I/O et Fichiers

```java
import java.io.*;
import java.nio.file.*;
import java.util.*;

// Lire un fichier
BufferedReader reader = new BufferedReader(new FileReader("data.txt"));
String line;
while ((line = reader.readLine()) != null) {
    System.out.println(line);
}
reader.close();

// Écrire un fichier
BufferedWriter writer = new BufferedWriter(new FileWriter("output.txt"));
writer.write("Ligne 1\n");
writer.write("Ligne 2\n");
writer.close();

// Try-with-resources (fermeture automatique)
try (BufferedReader br = new BufferedReader(new FileReader("data.txt"))) {
    while ((line = br.readLine()) != null) {
        System.out.println(line);
    }
}

// NIO (Java moderne)
List<String> lines = Files.readAllLines(Paths.get("data.txt"));
Files.write(Paths.get("output.txt"), lines);
```

---

## 2. Multithreading

```java
// Méthode 1: Extends Thread
class MonThread extends Thread {
    public void run() {
        for (int i = 0; i < 5; i++) {
            System.out.println(Thread.currentThread().getName() + ": " + i);
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}

// Méthode 2: Implements Runnable (recommandé)
class MonRunnable implements Runnable {
    public void run() {
        // Code du thread
    }
}

// Utilisation
Thread t1 = new MonThread();
t1.start();

Thread t2 = new Thread(new MonRunnable());
t2.start();

// Avec Lambda
Thread t3 = new Thread(() -> {
    System.out.println("Thread lambda");
});
t3.start();
```

---

## 3. JDBC - Connexion aux BD

```java
import java.sql.*;

public class JDBCDemo {
    public static void main(String[] args) {
        String url = "jdbc:mysql://localhost:3306/universite";
        String user = "root";
        String password = "password";
        
        try (Connection conn = DriverManager.getConnection(url, user, password)) {
            // SELECT
            String query = "SELECT * FROM etudiants WHERE age > ?";
            PreparedStatement pstmt = conn.prepareStatement(query);
            pstmt.setInt(1, 20);
            
            ResultSet rs = pstmt.executeQuery();
            while (rs.next()) {
                String nom = rs.getString("nom");
                int age = rs.getInt("age");
                System.out.println(nom + ": " + age);
            }
            
            // INSERT
            String insert = "INSERT INTO etudiants (nom, age) VALUES (?, ?)";
            PreparedStatement insertStmt = conn.prepareStatement(insert);
            insertStmt.setString(1, "Alice");
            insertStmt.setInt(2, 20);
            int rows = insertStmt.executeUpdate();
            
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
}
```

---

## 4. Introduction à Hadoop

### 4.1 Concepts

**Hadoop Ecosystem:**
- HDFS: Distributed File System
- MapReduce: Traitement parallèle
- YARN: Resource Manager

### 4.2 MapReduce Simple

```java
// Mapper
public class WordCountMapper extends Mapper<LongWritable, Text, Text, IntWritable> {
    private final static IntWritable one = new IntWritable(1);
    private Text word = new Text();
    
    public void map(LongWritable key, Text value, Context context) 
            throws IOException, InterruptedException {
        String line = value.toString();
        String[] words = line.split("\\s+");
        
        for (String w : words) {
            word.set(w);
            context.write(word, one);
        }
    }
}

// Reducer
public class WordCountReducer extends Reducer<Text, IntWritable, Text, IntWritable> {
    public void reduce(Text key, Iterable<IntWritable> values, Context context) 
            throws IOException, InterruptedException {
        int sum = 0;
        for (IntWritable val : values) {
            sum += val.get();
        }
        context.write(key, new IntWritable(sum));
    }
}
```

---

## 🎯 Points Clés

✅ Java I/O pour fichiers volumineux  
✅ Multithreading pour parallélisation  
✅ JDBC pour connexion BD  
✅ Hadoop pour Big Data distribué  

---

## ➡️ Prochaine Étape

[Module 11: Apache Spark →](./module11-spark.md)

---

*© 2025 - Formation Data Mining Professionnelle*
