```sql
-- 1

CREATE TABLE customer (
    id_customer CHAR(6),
    nama_customer VARCHAR(20),
    CONSTRAINT Customer_PK PRIMARY KEY (id_customer)
);

CREATE TABLE pegawai (
	NIK CHAR(16),
    Nama_pegawai VARCHAR(100),
    Jenis_kelamin CHAR(1),
    Email VARCHAR(50),
    Umur INT,
    CONSTRAINT Pegawai_PK PRIMARY KEY (NIK)
);

CREATE TABLE telepon (
    NO_telp_pegawai VARCHAR(15),
    Pegawai_NIK CHAR(16),
    CONSTRAINT Telepon_PK PRIMARY KEY (NO_telp_pegawai),
    FOREIGN KEY (Pegawai_NIK) REFERENCES pegawai(NIK)
);

CREATE TABLE Menu_minuman (
    id_minuman CHAR(6),
    Nama_minuman VARCHAR(50),
    Harga_minuman FLOAT(2),
    CONSTRAINT Menu_Minuman_PK PRIMARY KEY (id_minuman)
);

CREATE TABLE Transaksi (
    id_transaksi CHAR(10),
    tanggal_transaksi DATE,
    metode_pembayaran VARCHAR(15),
    Customer_ID_customer CHAR(6),
    Pegawai_NIK CHAR(16),
    CONSTRAINT Transaksi_PK PRIMARY KEY (id_transaksi),
    CONSTRAINT Transaksi_Customer_FK FOREIGN KEY (Customer_ID_customer) REFERENCES customer(id_customer),
    CONSTRAINT Transaksi_Pegawai_FK FOREIGN KEY (Pegawai_NIK) REFERENCES pegawai(NIK)
);

CREATE TABLE transaksi_minuman (
    TM_Menu_minuman_ID CHAR(6),
    TM_Transaksi_ID CHAR(10),
    Jumlah_cup INT,
    CONSTRAINT Transaksi_minuman_PK PRIMARY KEY (TM_Menu_minuman_ID, TM_Transaksi_ID),
    CONSTRAINT TM_ID_Menu_minuman_FK FOREIGN KEY (TM_Menu_minuman_ID) REFERENCES Menu_minuman(id_minuman),
    CONSTRAINT TM_ID_Transaksi_FK FOREIGN KEY (TM_Transaksi_ID) REFERENCES transaksi(id_transaksi)
);

-- 2

CREATE TABLE membership (
    id_membership CHAR(6),
    no_telepon_customer VARCHAR(15),
    alamat_customer VARCHAR(100),
    tanggal_pembuatan_kartu_membership DATE,
    tanggal_kedaluawarsa_kartu_membership DATE NULL,
    total_point INT,
    customer_id_customer CHAR(6)
);
-- A

ALTER TABLE membership
ADD PRIMARY KEY (id_membership);

-- B

ALTER TABLE membership
ADD CONSTRAINT membership_customer_FK FOREIGN KEY (customer_id_customer) REFERENCES customer(id_customer) ON UPDATE CASCADE ON DELETE RESTRICT;

-- C

ALTER TABLE transaksi
DROP FOREIGN KEY Transaksi_Customer_FK;

ALTER TABLE transaksi
ADD CONSTRAINT Transaksi_Customer_FK FOREIGN KEY (Customer_ID_customer) REFERENCES customer(id_customer) ON DELETE CASCADE ON UPDATE CASCADE;

-- D

ALTER TABLE membership
MODIFY COLUMN tanggal_pembuatan_kartu_membership DATE DEFAULT CURRENT_DATE;

-- E

ALTER TABLE membership
ADD CONSTRAINT check_total_poin CHECK (total_point >= 0);

-- F

ALTER TABLE membership
MODIFY alamat_customer VARCHAR(150);

-- 3

DROP TABLE telepon;

ALTER TABLE pegawai
ADD telepon VARCHAR(15);

-- 4

INSERT INTO Customer (id_customer, nama_customer) VALUES
('CTR001', 'Budi Santoso'),
('CTR002', 'Sisil Triana'),
('CTR003', 'Davi Liam'),
('CTRo04', 'Sutris Ten An'),
('CTR005', 'Hendra Asto');

INSERT INTO membership (id_membership, no_telepon_customer, alamat_customer, tanggal_pembuatan_kartu_membership, tanggal_kedaluawarsa_kartu_membership, total_point, customer_id_customer) VALUES
('MBR001', '08123456789', 'Jl. Imam Bonjol', '2023-10-24', '2023-11-30', 0, 'CTR001'),
('MBR002', '0812345678', 'Jl. Kelinci', '2023-10-24', '2023-11-30', 3, 'CTR002'),
('MBR003', '081234567890', 'Jl. Abah Ojak', '2023-10-25', '2023-12-01', 2, 'CTR003'),
('MBR004', '08987654321', 'Jl. Kenangan', '2023-10-26', '2023-12-02', 6, 'CTR005');

INSERT INTO pegawai (NIK, Nama_pegawai, Jenis_kelamin, Email, Umur, telepon) VALUES
('1234567890123456', 'Naufal Raf', 'L', 'naufal@gmail.com', 19, '62123456789'),
('2345678901234561', 'Surinala', 'P', 'surinala@gmail.com', 24, '621234567890'),
('3456789012345612', 'Ben John', 'L', 'benjohn@gmail.com', 22, '6212345678');

INSERT INTO transaksi (id_transaksi, tanggal_transaksi, metode_pembayaran, Pegawai_NIK, Customer_ID_customer) VALUES
('TRX0000001', '2023-10-01', 'Kartu kredit', '2345678901234561', 'CTR002'),
('TRX0000002', '2023-10-03', 'Transfer bank', '3456789012345612', 'CTRo04'),
('TRX0000003', '2023-10-05', 'Tunai', '3456789012345612', 'CTR001'),
('TRX0000004', '2023-10-15', 'Kartu debit', '1234567890123456', 'CTR003'),
('TRX0000005', '2023-10-15', 'E-wallet', '1234567890123456', 'CTRo04'),
('TRX0000006', '2023-10-21', 'Tunai', '2345678901234561', 'CTR001');

SELECT * FROM transaksi;

INSERT INTO Menu_minuman (id_minuman, Nama_minuman, Harga_minuman) VALUES
('MNM001', 'Expresso', 18000),
('MNM002', 'Cappuccino', 20000),
('MNM003', 'Latte', 21000),
('MNM004', 'Americano', 19000),
('MNM005', 'Mocha', 22000),
('MNM006', 'Macchiato', 23000),
('MNM007', 'Cold Brew', 21000),
('MNM008', 'Iced Coffee', 18000),
('MNM009', 'Affogato', 23000),
('MNM010', 'Coffee Frappe', 22000);

SELECT * FROM menu_minuman;

INSERT INTO transaksi_minuman (TM_Transaksi_ID, TM_Menu_minuman_ID, Jumlah_cup) VALUES
('TRX0000005', 'MNM006', 2),
('TRX0000001', 'MNM010', 1),
('TRX0000002', 'MNM005', 1),
('TRX0000005', 'MNM009', 1),
('TRX0000003', 'MNM001', 3),
('TRX0000006', 'MNM003', 2),
('TRX0000004', 'MNM004', 2),
('TRX0000004', 'MNM010', 1),
('TRX0000002', 'MNM003', 2),
('TRX0000001', 'MNM007', 1),
('TRX0000005', 'MNM001', 1),
('TRX0000003', 'MNM003', 1);

SELECT * FROM transaksi_minuman;

-- 5

INSERT INTO transaksi (id_transaksi, tanggal_transaksi, metode_pembayaran, Pegawai_NIK, Customer_ID_customer) VALUES
('TRX0000007', '2023-10-03', 'Transfer bank', '2345678901234561' , 'CTRo04');

INSERT INTO transaksi_minuman (TM_Transaksi_ID, TM_Menu_minuman_ID , Jumlah_cup) VALUES
('TRX0000007', 'MNM005', 1);

-- 6

INSERT INTO pegawai (NIK, Nama_pegawai, Umur)
VALUES ('1111222233334444', 'Maimunah', 25);

-- 7

UPDATE customer
SET id_customer = 'CTR004'
WHERE id_customer = 'CTRo04';

-- 8

UPDATE pegawai
SET Jenis_kelamin = 'P', telepon = '621234567', Email = 'maimunah@gmail.com'
WHERE Nama_pegawai = 'Maimunah';

-- 9

UPDATE membership
SET total_point = 0
WHERE MONTH(tanggal_kedaluawarsa_kartu_membership) < 12 AND YEAR(tanggal_kedaluawarsa_kartu_membership) = 2023;

-- 10

DELETE FROM membership;

-- 11

DELETE FROM pegawai
WHERE Nama_pegawai = 'Maimunah';
```
