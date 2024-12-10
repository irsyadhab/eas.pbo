# EAS PBO

Nama  : Muhammad Irsyad Habibi

NRP  : 5025221150

Kelas  : PBO A

- __Apa yang dimaksud dengan Package Library dalam Java? Jelaskan kegunaan dan contoh penggunaannya dalam pemrograman.__
  
  Java Package Library merupakan sekumpulan Java class yang terkumpul dalam satu direktori. Java package sendiri terbagi menjadi 2, yaitu Built-in Package dan User-defined package.
  - Built-in Package
    Package ini merupakan package bawaan yang sudah tersedia setelah menginstall Java dan dapat langsung diambil dari library. Misal, kita dapat mengimport `Scanner` class dari Package `java.util` supaya dapat melakukan operasi input.
    ```
    import java.util.Scanner;
    ```
  - User-defined Package
    User-defined package biasanya merujuk ke nama direktori tempat file Java berada. Sebagai contoh, project saya merupakan Hotel Reservation System yang terdiri dari 3 file java, yaitu Main.java, Reservation.java, ReservationController.java. Ketiga file tersebut berada dalam direktori `project`, sehingga pada header ketiga file tersebut, sebelum deklarasi class, terdapat:
    ```
    package project;
    ```
    
- __Buatlah Rancangan Aplikasi yang diambil dalam Final Project__
  
  Project yang diambil adalah Hotel Reservation System, yang terdiri dari 3 file Java, yaitu Main.java, Reservation.java, ReservationController.java. Terdapat pula file ReservationView.fxml untuk desain UI.
  Pada Main.java, terdapat `FXMLLoader` yang akan mengarah ke ReservationView.fxml sebagai desain UI. Lalu, pada ReservationView.fxml, terdapat `fx:controller` yang mengarah ke ReservationController.java.
  Pada sistem reservasi hotel ini, terdapat 4 variabel pada setiap transaksi, yaitu nama customer, jenis kamar, tanggal check-in, dan tanggal check-out.
  Berikut merupakan desain UI berdasarkan file fxml yang telah dibuat.

  ![Screenshot (119)](https://github.com/user-attachments/assets/5dda73c2-aa56-45e8-ad0c-e12e4f2f3112)

- __Deskripsikan kegunaan dan ruang lingkup aplikasi__
  
  Kegunaan dari Hotel Reservation System ini adalah untuk mengorganisasikan pemesanan tiket hotel.
- __Implementasikan Aplikasi yang telah didesain dengan menggunakan Pemrograman Berbasis Obyek Java__
  
  Main.java
  ```
  package project;

  import javafx.application.Application;
  import javafx.fxml.FXMLLoader;
  import javafx.scene.Scene;
  import javafx.stage.Stage;
  
  public class Main extends Application {
      @Override
      public void start(Stage stage) throws Exception {
          FXMLLoader loader = new FXMLLoader(getClass().getResource("ReservationView.fxml"));
          Scene scene = new Scene(loader.load());
          stage.setScene(scene);
          stage.setTitle("Hotel Reservation System");
          stage.show();
      }
  
      public static void main(String[] args) {
          launch(args);
      }
  }

  ```

  Reservation.java

  ```
  package project;

  public class Reservation {
      private String name;
      private String roomType;
      private String checkInDate;
      private String checkOutDate;
  
      public Reservation(String name, String roomType, String checkInDate, String checkOutDate) {
          this.name = name;
          this.roomType = roomType;
          this.checkInDate = checkInDate;
          this.checkOutDate = checkOutDate;
      }
  
      public String getName() {
          return name;
      }
  
      public String getRoomType() {
          return roomType;
      }
  
      public String getCheckInDate() {
          return checkInDate;
      }
  
      public String getCheckOutDate() {
          return checkOutDate;
      }
  }
  ```

  ReservationController.java

  ```
  package project;

  import javafx.collections.FXCollections;
  import javafx.collections.ObservableList;
  import javafx.fxml.FXML;
  import javafx.scene.control.*;
  import javafx.scene.control.cell.PropertyValueFactory;
  
  public class ReservationController {
  
      @FXML
      private TextField nameField;
      @FXML
      private ComboBox<String> roomTypeCombo;
      @FXML
      private DatePicker checkInDate;
      @FXML
      private DatePicker checkOutDate;
      @FXML
      private TableView<Reservation> reservationTable;
      @FXML
      private TableColumn<Reservation, String> nameColumn;
      @FXML
      private TableColumn<Reservation, String> roomTypeColumn;
      @FXML
      private TableColumn<Reservation, String> checkInColumn;
      @FXML
      private TableColumn<Reservation, String> checkOutColumn;
  
      private ObservableList<Reservation> reservationList = FXCollections.observableArrayList();
  
      @FXML
      public void initialize() {
          // Inisialisasi ComboBox
          roomTypeCombo.setItems(FXCollections.observableArrayList("Single", "Double", "Suite"));
  
          // Set kolom tabel
          nameColumn.setCellValueFactory(new PropertyValueFactory<>("name"));
          roomTypeColumn.setCellValueFactory(new PropertyValueFactory<>("roomType"));
          checkInColumn.setCellValueFactory(new PropertyValueFactory<>("checkInDate"));
          checkOutColumn.setCellValueFactory(new PropertyValueFactory<>("checkOutDate"));
  
          // Hubungkan data ke tabel
          reservationTable.setItems(reservationList);
      }
  
      @FXML
      private void handleAddReservation() {
          String name = nameField.getText();
          String roomType = roomTypeCombo.getValue();
          String checkIn = checkInDate.getValue() != null ? checkInDate.getValue().toString() : "";
          String checkOut = checkOutDate.getValue() != null ? checkOutDate.getValue().toString() : "";
  
          if (name.isEmpty() || roomType == null || checkIn.isEmpty() || checkOut.isEmpty()) {
              Alert alert = new Alert(Alert.AlertType.WARNING, "Harap isi semua bidang!", ButtonType.OK);
              alert.showAndWait();
              return;
          }
  
          reservationList.add(new Reservation(name, roomType, checkIn, checkOut));
          clearForm();
      }
  
      private void clearForm() {
          nameField.clear();
          roomTypeCombo.setValue(null);
          checkInDate.setValue(null);
          checkOutDate.setValue(null);
      }
  }
  ```

  ReservationView.fxml

  ```
  <?xml version="1.0" encoding="UTF-8"?>

  <?import javafx.scene.control.*?>
  <?import javafx.scene.layout.*?>
  <?import javafx.scene.layout.VBox?>
  
  <VBox xmlns:fx="http://javafx.com/fxml" fx:controller="ReservationController" spacing="10" alignment="CENTER" prefWidth="600" prefHeight="400">
      <Label text="Hotel Reservation System" style="-fx-font-size: 18px;" />
      <GridPane hgap="10" vgap="10">
          <Label text="Name:" />
          <TextField fx:id="nameField" GridPane.columnIndex="1" />
          <Label text="Room Type:" GridPane.rowIndex="1" />
          <ComboBox fx:id="roomTypeCombo" GridPane.columnIndex="1" GridPane.rowIndex="1" />
          <Label text="Check-In Date:" GridPane.rowIndex="2" />
          <DatePicker fx:id="checkInDate" GridPane.columnIndex="1" GridPane.rowIndex="2" />
          <Label text="Check-Out Date:" GridPane.rowIndex="3" />
          <DatePicker fx:id="checkOutDate" GridPane.columnIndex="1" GridPane.rowIndex="3" />
      </GridPane>
      <Button text="Add Reservation" onAction="#handleAddReservation" />
      <TableView fx:id="reservationTable">
          <columns>
              <TableColumn fx:id="nameColumn" text="Name" />
              <TableColumn fx:id="roomTypeColumn" text="Room Type" />
              <TableColumn fx:id="checkInColumn" text="Check-In Date" />
              <TableColumn fx:id="checkOutColumn" text="Check-Out Date" />
          </columns>
      </TableView>
  </VBox>

  ```
  
- __Buat PPT presentasi yang menunjang Demo Aplikasi__
- __Demokan aplikasi dengan membuat video dan diupload di Youtube.__

  Link Channel Youtube:
  https://www.youtube.com/@irsyadhabibi2554
