---
title: "CF 918B - Đài phát thanh"
description: "Chúng tôi được cấp một sổ đăng ký nhỏ gồm các máy chủ, trong đó mỗi máy chủ được xác định bằng một địa chỉ IP duy nhất và cũng có tên mà con người có thể đọc được. Sau đó, chúng ta được cung cấp một danh sách các lệnh cấu hình và mỗi lệnh chỉ tham chiếu đến một máy chủ thông qua IP của nó."
date: "2026-06-13T02:29:03+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "strings"]
categories: ["algorithms"]
codeforces_contest: 918
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 459 (Div. 2)"
rating: 900
weight: 918
solve_time_s: 563
verified: true
draft: false
---

[CF 918B - Đài phát thanh](https://codeforces.com/problemset/problem/918/B) 

**Xếp hạng:** 900 
**Tags:** triển khai, chuỗi 
**Thời gian giải:** 9m 23s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một sổ đăng ký nhỏ gồm các máy chủ, trong đó mỗi máy chủ được xác định bằng một địa chỉ IP duy nhất và cũng có tên mà con người có thể đọc được. Sau đó, chúng ta được cung cấp một danh sách các lệnh cấu hình và mỗi lệnh chỉ tham chiếu đến một máy chủ thông qua IP của nó. 

Nhiệm vụ là làm phong phú từng dòng lệnh bằng cách thêm một nhận xét chứa tên máy chủ tương ứng. Cụ thể, bất cứ khi nào một lệnh chứa địa chỉ IP, chúng ta phải thay thế dòng đó bằng dòng tương tự, theo sau là`#name`, Ở đâu`name`là máy chủ có IP trùng khớp. 

Cấu trúc của đầu vào làm cho ý định trở nên rất trực tiếp: trước tiên hãy xây dựng một ánh xạ từ địa chỉ IP đến tên máy chủ, sau đó sử dụng ánh xạ đó để dịch từng dòng lệnh. 

Các ràng buộc rất nhỏ, tối đa là 1000 máy chủ và 1000 lệnh. Điều này có nghĩa là ngay cả cách tiếp cận đơn giản thực hiện tìm kiếm tuyến tính cũng đã nằm trong giới hạn có thể chấp nhận được, vì việc quét trong trường hợp xấu nhất đối với tất cả các máy chủ cho mỗi lệnh sẽ đưa ra khoảng 1.000.000 so sánh, điều này không đáng kể trong Python dưới giới hạn 2 giây. Tuy nhiên, cấu trúc tra cứu IP gợi ý rõ ràng về giải pháp dựa trên từ điển. 

Một điểm tinh tế là tên máy chủ không được đảm bảo là duy nhất nhưng địa chỉ IP thì có. Điều này rất quan trọng vì khóa chính xác để tra cứu hoàn toàn là IP chứ không phải tên. Một chi tiết khác là phân tích cú pháp: IP xuất hiện trong dòng lệnh với dấu chấm phẩy ở cuối, vì vậy nó phải được loại bỏ cẩn thận trước khi tra cứu. 

Không có trường hợp phức tạp nào phát sinh ngoài việc xử lý và ánh xạ chuỗi chính xác, nhưng một giải pháp bất cẩn có thể thất bại theo một số cách. Một là quên xóa dấu chấm phẩy ở cuối, dẫn đến việc tra cứu từ điển không thành công. Một cách khác là phân tách lệnh không chính xác nếu giả sử có nhiều khoảng trắng hoặc các biến thể định dạng. Vì định dạng nhất quán nên việc phân tách theo dấu cách và cắt bớt dấu chấm phẩy là đủ. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất là, đối với mỗi dòng lệnh, quét tất cả các máy chủ đã biết và tìm máy chủ có IP khớp với IP được truy vấn. Điều này hiệu quả vì tập dữ liệu nhỏ và mỗi lần tra cứu đều độc lập. Tuy nhiên, cách tiếp cận này lặp lại những so sánh giống nhau cho mọi truy vấn. Với 1000 lệnh và 1000 máy chủ, nó thực hiện tới một triệu so sánh chuỗi, điều này vẫn có thể chấp nhận được nhưng không cần thiết. 

Quan sát chính là mối quan hệ giữa IP và tên máy chủ là tĩnh. Sau khi đọc danh sách máy chủ, chúng tôi có thể tính toán trước một từ điển ánh xạ trực tiếp từng chuỗi IP tới tên của nó. Điều này làm giảm mỗi truy vấn thành một tra cứu bảng băm duy nhất thay vì quét tuyến tính. 

Việc chuyển đổi trở nên hoàn toàn cơ học: xây dựng bản đồ, phân tích từng lệnh, trích xuất IP, tra cứu tên và nối thêm nó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Quét Brute Force cho mỗi truy vấn | O(nm) | O(1) | Được chấp nhận nhưng không cần thiết | 
| Tra cứu bản đồ băm | O(n + m) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tách giải pháp thành tiền xử lý và xử lý truy vấn. 

1. Đọc tất cả các mục nhập trên máy chủ và lưu trữ chúng trong từ điển được khóa theo địa chỉ IP. Mỗi mục ánh xạ chuỗi IP trực tiếp tới tên máy chủ. Điều này đảm bảo việc truy xuất liên tục sau này. 
2. Lặp lại từng dòng lệnh cấu hình. Mỗi dòng chứa một từ lệnh, IP và dấu chấm phẩy ở cuối. 
3. Chia dòng thành các mã thông báo bằng khoảng trắng. Mã thông báo thứ hai chứa IP có dấu chấm phẩy được đính kèm. 
4. Xóa dấu chấm phẩy khỏi mã thông báo IP. Điều này là cần thiết vì khóa từ điển được lưu trữ dưới dạng chuỗi IP thuần túy không có dấu chấm câu. 
5. Sử dụng IP đã được làm sạch để lấy tên máy chủ tương ứng từ từ điển. 
6. Xây dựng dòng đầu ra bằng cách nối lệnh gốc, mã thông báo IP gốc có dấu chấm phẩy và nhận xét`#name`. 

Lựa chọn thiết kế quan trọng là chúng tôi không bao giờ cố gắng diễn giải lại hoặc định dạng lại IP ngoài việc loại bỏ dấu chấm phẩy. Chúng tôi giữ nguyên định dạng ban đầu của dòng lệnh và chỉ thêm nhận xét. 

### Tại sao nó hoạt động 

Từ điển lưu trữ một sự phân biệt hoàn chỉnh từ IP đến tên và mọi IP truy vấn được đảm bảo tồn tại trong ánh xạ này. Mỗi dòng lệnh là độc lập và việc chuyển đổi chỉ phụ thuộc vào ánh xạ cố định này. Vì bước phân tích luôn trích xuất chuỗi IP chính xác (sau khi loại bỏ`;`), việc tra cứu chính xác và rõ ràng, đảm bảo mọi lệnh đều được chú thích bằng tên máy chủ chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    n, m = map(int, input().split())
    mp = {}

    for _ in range(n):
        name, ip = input().split()
        mp[ip] = name

    out = []
    for _ in range(m):
        parts = input().split()
        command = parts[0]
        ip_with_semicolon = parts[1]

        ip = ip_with_semicolon[:-1]
        name = mp[ip]

        out.append(f"{command} {ip_with_semicolon} #{name}")

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    main()
```Ý tưởng cốt lõi trong mã là từ điển`mp`, cung cấp dịch trực tiếp từ IP sang tên máy chủ. Logic phân tích cú pháp dựa trên cấu trúc cố định của từng dòng lệnh, vì vậy chúng tôi chia thành hai mã thông báo một cách an toàn và loại bỏ chính xác một ký tự cuối khỏi mã thông báo IP. Định dạng đầu ra giữ nguyên định dạng lệnh ban đầu và chỉ thêm nhận xét được yêu cầu. 

Một lỗi phổ biến là cố gắng phân tích IP mạnh mẽ hơn, chẳng hạn như phân tách bằng dấu chấm hoặc dấu chấm phẩy một cách không cần thiết. Điều đó là không bắt buộc và chỉ làm tăng nguy cơ xảy ra lỗi định dạng. 

## Ví dụ đã hoạt động 

Chúng tôi sử dụng đầu vào mẫu được cung cấp để minh họa việc thực hiện. 

### Dấu vết ví dụ 

Máy chủ đầu vào:```
main -> 192.168.0.2
replica -> 192.168.0.1
```Lệnh:```
block 192.168.0.1;
proxy 192.168.0.2;
```| Bước | Dòng đầu vào | IP được phân tích | Kết quả tra cứu | Đầu ra | 
| --- | --- | --- | --- | --- | 
| 1 | khối 192.168.0.1; | 192.168.0.1 | bản sao | khối 192.168.0.1; #bản sao | 
| 2 | proxy 192.168.0.2; | 192.168.0.2 | chính | proxy 192.168.0.2; #chính | 

Điều này xác nhận rằng mỗi dòng được chuyển đổi độc lập bằng cách sử dụng cùng một ánh xạ cố định. 

Một ví dụ nhỏ thứ hai giúp xác minh tính chắc chắn. 

đầu vào:```
1 2
alpha 1.1.1.1
ping 1.1.1.1;
pong 1.1.1.1;
```Cả hai lệnh đều ánh xạ tới cùng một máy chủ, cho thấy nhiều truy vấn có thể chia sẻ cùng một IP mà không gặp vấn đề gì vì việc tra cứu từ điển không có trạng thái. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Một lượt để xây dựng từ điển, một lượt để xử lý các lệnh với số lần tra cứu trung bình O(1) cho mỗi lệnh | 
| Không gian | O(n) | Lưu trữ một mục ánh xạ trên mỗi máy chủ | 

Các ràng buộc giới hạn cả n và m ở mức 1000, vì vậy giải pháp này chạy thoải mái trong giới hạn. Ngay cả trong Python, các thao tác từ điển ở quy mô này diễn ra tức thời. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, m = map(int, input().split())
    mp = {}

    for _ in range(n):
        name, ip = input().split()
        mp[ip] = name

    res = []
    for _ in range(m):
        parts = input().split()
        cmd = parts[0]
        ip_semicolon = parts[1]
        ip = ip_semicolon[:-1]
        res.append(f"{cmd} {ip_semicolon} #{mp[ip]}")

    return "\n".join(res)

# provided sample
assert run("""2 2
main 192.168.0.2
replica 192.168.0.1
block 192.168.0.1;
proxy 192.168.0.2;
""") == """block 192.168.0.1; #replica
proxy 192.168.0.2; #main"""

# single server reused
assert run("""1 2
alpha 1.1.1.1
ping 1.1.1.1;
pong 1.1.1.1;
""") == """ping 1.1.1.1; #alpha
pong 1.1.1.1; #alpha"""

# minimal case
assert run("""1 1
x 0.0.0.0
cmd 0.0.0.0;
""") == "cmd 0.0.0.0; #x"

# multiple distinct mappings
assert run("""3 3
a 1.1.1.1
b 2.2.2.2
c 3.3.3.3
x 3.3.3.3;
y 1.1.1.1;
z 2.2.2.2;
""") == """x 3.3.3.3; #c
y 1.1.1.1; #a
z 2.2.2.2; #b"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| máy chủ duy nhất được sử dụng lại | tra cứu lặp đi lặp lại chính xác | truy vấn IP lặp đi lặp lại | 
| trường hợp tối thiểu | độ chính xác của ánh xạ đơn | xử lý ranh giới | 
| nhiều ánh xạ | hoán vị đầy đủ đúng đắn | tính chính xác của từ điển qua các mục | 

## Vỏ cạnh 

Một trường hợp khó nhận thấy là khi nhiều lệnh tham chiếu cùng một IP. Vì ánh xạ hoàn toàn mang tính kết hợp và không được sử dụng hoặc sửa đổi nên việc tra cứu lặp lại luôn trả về cùng một tên. Thuật toán xử lý việc này một cách tự nhiên vì việc truy cập từ điển không thay đổi trạng thái. 

Một cạm bẫy tiềm ẩn khác là trích xuất IP không chính xác. Nếu quên bỏ dấu chấm phẩy, khóa tra cứu sẽ trở thành`192.168.0.1;`, không tồn tại trong từ điển. Điều này dẫn đến lỗi thời gian chạy. Bản sửa lỗi liên tục loại bỏ chính xác ký tự cuối cùng của mã thông báo thứ hai, được đảm bảo là`;`theo định dạng đầu vào. 

Trường hợp cuối cùng là giả sử tên là duy nhất hoặc sử dụng chúng làm khóa. Vì tên có thể lặp lại nên việc sử dụng IP làm khóa duy nhất là cần thiết. Thiết kế từ điển hoàn toàn tránh được sự mơ hồ vì tính duy nhất của IP được đảm bảo.
