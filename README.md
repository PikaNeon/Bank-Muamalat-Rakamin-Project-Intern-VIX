# Bank-Muamalat-Rakamin-Project-Intern-VIX
Primary key merupakan identitas setiap baris sehingga bersifat unique (tidak boleh ada duplikasi), tidak memiliki NULL, dan satu tabel hanya diperbolehkan memiliki 1 primary key. Project ini menggunakan 4 dataset dengan detail berikut:

#### 1. Orders (3339 baris)

Data: [Orders.csv](Orders.csv)
- OrderID (int)
- Date (dd/mm/yyyy)
- CustomerID (int)
- ProdNumber (str)
- Quantity (int)

Primary key dari dataset Orders adalah OrderID.

#### 2. Product Category (7 baris)

Data: [ProductCategory.csv](ProductCategory.csv)
- CategoryID (int)
- CategoryNa (str)
- CategoryAb (str)

Primary key dari dataset Product Category adalah CategoryID.

#### 3. Products (70 baris)

Data: [Products.csv](Products.csv)
- ProdNumber (str)
- ProdName (str)
- Category (int)
- Price (int)

Primary key dari dataset Products adalah ProdNumber.

#### 4. Customers (2123 baris)

Data: [Customers.csv](Customers.csv)
- CustomerID (int)
- FirstName (str)
- LastName (str)
- CustomerEmail (str)
- CustomerPhone (str)
- CustomerAddress (str)
- CustomerCity (str)
- CustomerZip (str)

Primary key dari dataset Customers adalah CustomerID.


## Task 2 (Relationship Antardataset)

Berikut relationship dari keempat dataset.
<img width="638" height="455" alt="Screenshot 2025-09-29 224912" src="https://github.com/user-attachments/assets/e4b5d46e-1043-4dcd-9d4a-4a8a8c0d8563" />


- Database Orders (variabel CustomerID:Foreign Key) memiliki relationship many-to-one dengan database Customers (variabel CustomerID) karena terdapat customer yang melakukan transaksi > 1. Hal ini ditunjukkan dengan CustomerID (contoh:2114) pada database Orders yang memiliki duplikat dan dikelompokkan ke dalam CustomerID di database Customers (yaitu 2114).
- Database Orders (variabel ProdNumber:Foreign Key) memiliki relationship many-to-one dengan database Products (variabel ProdNumber) karena setiap ProdNumber memiliki OrderID masing-masing sehingga OrderID pada database Orders dapat dikelompokkan ke dalam ProdNumber di database Products.
- Database Products (variabel Category: Foreign Key) memiliki relationship many-to-one dengan database Product Category (variabel CategoryID) karena setiap CategoryID memiliki ProdNumber masing-masing sehingga ProdNumber pada database Products dapat dikelompokkan ke dalam CategoryID di database Product Category.

## Task 3 (Membuat Master Table)

Master Table yang dibentuk merupakan agregat dari keempat database. Tujuannya untuk mengetahui detail transaksi penjualan per hari yang terdiri dari tanggal transaksi, kategori produk, nama produk, harga produk, kuantitas transaksi, total penjualan, email customer, dan asal (kota) customer.

```sql
SELECT tabel1.Date AS order_date, product_category.CategoryNa AS category_name, 
    tabel1.ProdName AS product_name, tabel1.Price AS product_price, tabel1.Quantity AS order_qty, 
    tabel1.Price*tabel1.Quantity AS total_sales, tabel1.CustomerEmail AS cust_email, tabel1.CustomerCity AS cust_city
FROM (
    SELECT  * FROM orders 
        INNER JOIN customers ON orders.CustomerID = customers.CustomerID
        INNER JOIN products ON orders.ProdNumber = products.ProdNumber
    ) tabel1
    INNER JOIN product_category ON tabel1.Category = product_category.CategoryID
ORDER BY order_date, order_qty;
```
Berikut adalah tampilan 15 baris pertama dari 3339 baris Master Table)
<img width="1008" height="417" alt="Screenshot 2025-09-28 140818" src="https://github.com/user-attachments/assets/6c5a5cd6-ea4d-4e08-97ba-22a980820773" />

[Download Master Table](MasterTable.csv)

## Task 4 (Membuat Sales Performance Dashboard)

Dashboard dibuat menggunakan Looker Studio dari data Master Table dengan tujuan memahami data transaksi dengan jelas dan membantu pemecahan masalah penjualan dengan memanfaatkan celah yang ditemukan. Berikut tampilan dashboard yang telah dibuat.

![Dashboard_Rakamin (6)_page-0001](https://github.com/user-attachments/assets/17114f58-b59d-4592-93ff-4a0214d2af58)

[Akses dashboard](tinyurl.com/dashb-sales-muamalat-carrin)

Informasi yang didapatkan:
- Total sales dan quantity nya cenderung stagnan sepanjang tahun 2020-2021 meskipun terdapat lonjakan total sales pada Januari 2020, Desember 2020, lalu Juni 2021 dan lonjakan total quantity yang lebih sering terjadi dibandingkan sales. Hal ini perlu ditelaah lebih lanjut faktor yang mempengaruhinya dan bagaimana cara meningkatkannya.
- Sales setiap kategorinya tidak hanya bergantung pada kuantitas transaksi, tetapi juga harga produknya. Hal ini ditunjukkan oleh grafik Sales and Quantity by Category dimana total sales kategori Robots paling besar namun memiliki total quantity yang lebih sedikit, dst.
- Kategori Drones, Drone Kits, dan Training Videos berada di top 5 sales maupun quantity.
- Kota Washington dan Houston memiliki Sales terbesar dan Quantity terbanyak.

## Task 5 (Rekomendasi bisnis)

Diperlukan metode-metode untuk meningkatkan penjualan yang berdasarkan dashboard terlihat stagnan selama 2 tahun di antaranya:
- Melakukan survei kepuasan dan kebutuhan customer serta sosial media, metode pembayaran, dan aplikasi online shop yang sering digunakan untuk mengetahui produk dan strategi pemasaran yang cocok dengan konsumen serta channel penjualan produk yang dapat memberikan pengalaman customer yang lebih baik dan konsisten.
- Memberikan insentif lebih kepada high frequency buyers berupa cashback, diskon ekslusif, dan point rewards yang bisa ditukar dengan produk gratis.
- Menawarkan paket bundling dengan harga yang menarik untuk produk drones, drone kits, dan training videos
- Produk dengan sales rendah namun quantity tinggi seperti e-books dan blueprints dapat dibuat lebih menarik untuk dibeli customer dengan konten ekslusif (e-books menyediakan preview beberapa halaman secara gratis dan blueprints menyediakan model 3 dimensi atau video tutorialnya).
- Membuat event musiman dengan menyediakan rekomendasi produk dan memberikan diskon produk-produk tertentu. Hal ini disebabkan kemungkinan belum diadakannya event musiman karena lonjakan sales dan quantity yang terjadi selama 2 tahun tidak konstan (contoh: 25 Desember 2020 dan 2021 tidak menunjukkan adanya lonjakan kuantitas transaksi. Sekalipun terjadi kenaikan drastis pada sales pada tanggal 20 Desember 2020, tapi tidak terjadi hal yang sama pada tahun 2021).

### Referensi
https://www.geeksforgeeks.org/dbms/difference-between-primary-key-and-foreign-key/

https://www.geeksforgeeks.org/sql/relationships-in-sql-one-to-one-one-to-many-many-to-many/
