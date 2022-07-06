# test_webdev_06072022
Các kiến thức cần lưu ý trong buổi học database ngày 06/07/2022:
  1. Hiểu rõ về các kiểu dữ liệu trong MySQL/MariaDB: char, varchar, int, date, datetime, decimal, ...
     - char(x): Chuỗi có x ký tự (cấp phát bao nhiêu dùng hết bấy nhiêu) 
     - varchar(x): Chuỗi có tối đa x ký tự (cấp phát x dùng bao nhiêu miễn nhỏ hơn x là được)
     - int, tinyint, smallint: tinyint, smallint là dạng nhỏ của int
     - date: Dạng ngày (Chỉ có ngày, giờ nếu get ra mặc định là 0h)
     - datetime: Có cả ngày lẫn giờ
     - decimal(x,y): Kiểu số thập phân, trong đó phần nguyên có x ký tự, phần thập phân có y ký tự (VD: 20000 lưu trong db sẽ là 20000.0000)
     - ...
  2. Các lưu ý khi tạo database:
     - Charset: utf8 (cái này sẽ giúp lưu được dữ liệu kiểu tiếng Việt)
  3. Các bước để tạo bảng và cách chọn kiểu dữ liệu :
     Trước khi tạo bảng cần phải tìm hiểu kỹ yêu cầu đề bài xem yêu cầu lưu những trường dữ liệu gì để phục vụ bài
        + Note tất cả các trường nhìn thấy ra notepad
        + Đối với các trường dạng chuỗi thì sẽ lưu kiểu varchar. Trong đó Code lưu varchar(20), Tên người lưu varchar(100), Các loại tên khác lưu varchar(255)
        + Đối với các dữ liệu dạng số thì tùy yêu cầu sẽ lưu int, smallint, tinyint
        + Đối với só lượng, tiền sẽ lưu kiểu decimal(18,4)
        + Đối với ngày tháng sẽ lưu dạng date/datetime
        + Cần xác định xem trường dữ liệu cần lưu khóa ngoại và tạo thêm bảng khác hay không
           x Trường hợp dữ liệu có nhiều và có thể tác động được như thêm, sửa, xóa thì nên tạo 1 bảng khác lưu dữ liệu này và trỏ khóa ngoại đến bảng đó (VD như bảng positions và bảng department trong bài tập)
           x Đối với dữ liệu chỉ giới hạn 1 vài giá trị thì sẽ lưu kiểu smallint(x) để xuống code backend, frontend sử dụng enum get resources (VD như gender, workstatus)
  4. Cách generate dummy data:
     Sử dụng tool dbForge Data Generator
     Thực hiện custom lại dữ liệu các cột ID, Name, Code, ... theo ý muốn
     Có thể sử dụng dữ liệu từ file Guid.txt, Position.txt, Department.txt đã gửi trên (cấu trúc $"Guid.txt" -> điền vào phần regex) => Kiểm tra dữ liệu sau khi thay đổi regex ở dưới xem đúng ý mong muốn chưa
  5. Tìm hiểu cách sử dụng store procedure, function trong MySQL
     - Create Procedure:
       + Create Procedure có thể qua New SQL hoặc vào phần Store Procedure chuột phải chọn Create Procedure
       + Cấu trúc đã có sẵn, tên store đặt theo cú pháp: Proc_<Bảng đang muốn tác động dữ liệu>_<Tên hành động> (VD: Proc_Employee_GetPaging)
       + Khi muốn gọi 1 store sẽ dùng lệnh CALL (VD: CALL Proc_Employee_GetPaging())
     - Create Function:
       + Create Function có thể qua New SQL hoặc vào phần Function chuột phải chọn Create Function
       + Cấu trúc đã có sẵn, tên store đặt theo cú pháp: Func_<Bảng đang muốn tác động dữ liệu>_<Tên hành động> (VD: Func_Employee_abcxyz)
       + Dưới chỗ Returns varchar/int khi khai báo trên đầu phải bổ sung DETERMINISTIC và READS SQL DATA
         VD: 
            CREATE DEFINER = 'root'@'localhost'
            FUNCTION Func_Get_Auto_EmployeeCode ()
            RETURNS varchar(20) CHARSET utf8mb4
            DETERMINISTIC
            READS SQL DATA
            COMMENT 'Function lấy mã nhân viên tự tăng theo quy tắc Mã mới = Mã mới nhất hiện tại + 1'
            BEGIN
        + Gọi function như sau: SELECT Func_Employee_abcxyz();
        
    6. Lưu ý cách sử dụng join: INNER JOIN, LEFT JOIN, RIGHT JOIN, CROSS JOIN, FULL OUTER JOIN
    (tìm hiểu trên stackoverflow cho dễ, giải thích hơi mất công :V) 
