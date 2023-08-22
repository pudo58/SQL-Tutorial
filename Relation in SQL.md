# Các quan hệ trong cơ sở dữ liệu

## 1. Quan hệ 1 - 1

- Một quan hệ 1 - 1 là một quan hệ trong đó một thực thể của quan hệ này tương ứng với một thực thể của quan hệ kia và
  ngược lại.
- Ví dụ: Một sinh viên chỉ có một mã sinh viên và một mã sinh viên chỉ tương ứng với một sinh viên.
- Cách biểu diễn:
	- Cách 1 : Đặt khóa chính của quan hệ này là khóa ngoại của quan hệ kia và ngược lại.
	- Cách 2 : Có thể gộp 2 quan hệ vào thành 1 bảng
		- Ví dụ:
			- Cách 1:
				- SinhVien(MaSV, HoTen, MaLop)
					- Lop(MaLop, TenLop)
                      - Code SQL :
                        ```SQL
                          CREATE TABLE SinhVien(
                              MaSV VARCHAR(10) PRIMARY KEY,
                              HoTen TEXT,
                              MaLop VARCHAR(10) FOREIGN KEY REFERENCES Lop(MaLop)
                          );
                              CREATE TABLE Lop(
                              MaLop VARCHAR(10) PRIMARY KEY,
                              TenLop TEXT
                          );
                          ```
			- Cách 2:
				- SinhVien(MaSV, HoTen, MaLop, TenLop)
				- Code SQL :
				  ```SQL
					  CREATE TABLE SinhVien(
						  MaSV VARCHAR(10) PRIMARY KEY,
						  HoTen TEXT,
						  MaLop VARCHAR(10),
						  TenLop TEXT
						  );
				  ```

## 2. Quan hệ 1 - N

- Một quan hệ 1 - N là một quan hệ trong đó một thực thể của quan hệ này tương ứng với nhiều thực thể của quan hệ kia và
  ngược lại.
- Ví dụ: Một lớp có nhiều sinh viên và một sinh viên chỉ thuộc một lớp.
	- Cách biểu diễn:
		- Cách 1 : Đặt khóa chính của quan hệ này là khóa ngoại của quan hệ kia.
- Ví dụ:
  - SinhVien(MaSV, HoTen, MaLop)
  - Lop(MaLop, TenLop)
  - Code SQL :
  ```SQL
	  CREATE TABLE SinhVien(
		  MaSV VARCHAR(10) PRIMARY KEY,
		  HoTen TEXT,
		  MaLop VARCHAR(10) FOREIGN KEY REFERENCES Lop(MaLop)
	  );
	  CREATE TABLE Lop(
		  MaLop VARCHAR(10) PRIMARY KEY,
		  TenLop TEXT
	  );
     
             --- data 
	   INSERT INTO Lop VALUES('L01', 'CNTT');
	   INSERT INTO Lop VALUES('L02', 'Kinh te');
	   INSERT INTO Lop VALUES('L03', 'Ngoai ngu');
	   INSERT INTO SinhVien VALUES('SV01', 'Nguyen Van A', 'L01');
	   INSERT INTO SinhVien VALUES('SV02', 'Nguyen Van B', 'L02');
	   INSERT INTO SinhVien VALUES('SV03', 'Nguyen Van C', 'L03');
	   INSERT INTO SinhVien VALUES('SV04', 'Nguyen Van D', 'L01');
	   INSERT INTO SinhVien VALUES('SV05', 'Nguyen Van E', 'L02');
	   INSERT INTO SinhVien VALUES('SV06', 'Nguyen Van F', 'L03');
	   INSERT INTO SinhVien VALUES('SV07', 'Nguyen Van G', 'L01');
	   --- Câu lệnh truy vấn
	   SELECT * FROM SinhVien WHERE MaLop = 'L01';
    ```
- Ví dụ thực tế : 
  - Một người có thể có nhiều tài khoản ngân hàng, nhưng một tài khoản ngân hàng chỉ thuộc về một người.
  - Một bài viết có thể có nhiều bình luận, nhưng một bình luận chỉ thuộc về một bài viết.
  - Khi một thực thể có nhiều thực thể khác thì ta sẽ tạo ra một bảng trung gian để lưu trữ các thực thể đó trong quan hệ 1 - N.

## 3. Quan hệ N - N
- Một quan hệ N - N là một quan hệ trong đó một thực thể của quan hệ này tương ứng với nhiều thực thể của quan hệ kia và ngược lại.
- Ví dụ: Một sinh viên có thể học nhiều môn và một môn có thể được học bởi nhiều sinh viên.
  - Cách biểu diễn : 
    - Cách 1 : Tạo ra một bảng trung gian để lưu trữ các thực thể của 2 quan hệ.
	  - Ví dụ : 
		- SinhVien(MaSV, HoTen)
		- MonHoc(MaMH, TenMH)
		- SinhVien_MonHoc(MaSV, MaMH)
    - Code SQL :
      ```SQL
        CREATE TABLE SinhVien(
            MaSV VARCHAR(10) PRIMARY KEY,
            HoTen TEXT
        );
        CREATE TABLE MonHoc(
            MaMH VARCHAR(10) PRIMARY KEY,
            TenMH TEXT
        );
        CREATE TABLE SinhVien_MonHoc(
            MaSV VARCHAR(10) FOREIGN KEY REFERENCES SinhVien(MaSV),
            MaMH VARCHAR(10) FOREIGN KEY REFERENCES MonHoc(MaMH),
            PRIMARY KEY(MaSV, MaMH)
        );
      ```
      - Lưu ý :
        - Khi tạo bảng trung gian thì không cần tạo khóa chính cho bảng trung gian.
        - Khóa chính của bảng trung gian là sự kết hợp của 2 khóa ngoại của 2 bảng mà nó tham chiếu đến.
        - Đã là khóa chính thì không thể trùng lặp được.
## 4. Quan hệ đệ quy
- Một quan hệ đệ quy là một quan hệ trong đó một thực thể của quan hệ này tương ứng với nhiều thực thể của chính nó.
  - Ví dụ: Một người có thể có nhiều con và một con có thể có nhiều con.
    - Cách biểu diễn : 
      - Cách 1 : Tạo ra một bảng trung gian để lưu trữ các thực thể của 2 quan hệ.
        - Ví dụ : 
          - NhanVien(MaNV, HoTen, MaQL)
          - NhanVien(MaNV, HoTen, MaQL)
          - Code SQL :
            ```SQL
                CREATE TABLE NhanVien(
                    MaNV VARCHAR(10) PRIMARY KEY,
                    HoTen TEXT,
                    MaQL VARCHAR(10) FOREIGN KEY REFERENCES NhanVien(MaNV)
                );
            ```
        - Lưu ý : 
          - MaQL là khóa ngoại của bảng NhanVien tham chiếu đến khóa chính của bảng NhanVien, tức là khóa chính của bảng NhanVien là khóa ngoại của chính nó.
          - Quan hệ này để phân cấp độ, và có thể có rất nhiều cấp độ, không thể dùng truy vấn thông thường để lấy dữ liệu từ quan hệ này
          - Có thể dùng các câu lệnh đệ quy để lấy dữ liệu từ quan hệ này.
            - Câu lệnh đệ quy : 
              ```SQL
                WITH RECURSIVE ten_bien AS (
                  SELECT * FROM ten_bang WHERE dieu_kien
                  UNION ALL
                  SELECT * FROM ten_bang INNER JOIN ten_bien ON dieu_kien
                )
                SELECT * FROM ten_bien;
              ```
            - Ví dụ : 
              ```SQL
                WITH RECURSIVE NhanVien_Cap1 AS (
                  SELECT * FROM NhanVien WHERE MaQL = 'NV01'
                  UNION ALL
                  SELECT * FROM NhanVien INNER JOIN NhanVien_Cap1 ON NhanVien.MaQL = NhanVien_Cap1.MaNV
                )
                SELECT * FROM NhanVien_Cap1;
              ```
          - Lưu ý : Quan hệ đệ quy thường sử dụng ở các cây phân cấp như, Danh mục phân cấp cha con cháu chắt ,...
            - Thực Hành quan hệ đệ quy : 
            - Tạo bảng : 
              ```SQL
                CREATE TABLE category(
                  id INT NOT NULL PRIMARY KEY,
                  name TEXT,
                  parent_id VARCHAR(10) FOREIGN KEY REFERENCES category(id)
                );
                INSERT INTO category VALUES(1, 'Danh mục 1', NULL);
                INSERT INTO category VALUES(2, 'Danh mục 2', 1);
                INSERT INTO category VALUES(3, 'Danh mục 3', 1);
                INSERT INTO category VALUES(4, 'Danh mục 4', 3);
                ```
              - Câu lệnh đệ quy :
                - Lấy tất cả các danh mục cấp 1 :
                  ```SQL
                        WITH RECURSIVE category_cap1 AS (
                          SELECT * FROM category WHERE parent_id IS NULL
                          UNION ALL
                          SELECT * FROM category INNER JOIN category_cap1 ON category.parent_id = category_cap1.id
                        )
                        SELECT * FROM category_cap1;
                  ```
              - Lưu ý : 
                - parent_id nếu là NULL thì là danh mục cấp 1 hay là danh mục cha.
   				
    
    
    