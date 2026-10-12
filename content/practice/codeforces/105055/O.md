---
title: "CF 105055O - Một chuyến đi khác"
description: "Tình huống là một chuyến đi đơn giản với một sai lầm về phương hướng. Hai khoảng cách có liên quan: thứ nhất là quãng đường mà du khách đã đi được sau khi đi sai hướng và thứ hai là khoảng cách còn lại từ điểm xuất phát đến đích dọc theo lộ trình đúng."
date: "2026-06-28T00:27:13+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105055
codeforces_index: "O"
codeforces_contest_name: "UDESC Selection Contest 2023-2"
rating: 0
weight: 105055
solve_time_s: 53
verified: true
draft: false
---

[CF 105055O - Một chuyến đi khác](https://codeforces.com/problemset/problem/105055/O) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 53s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Tình huống là một chuyến đi đơn giản với một sai lầm về phương hướng. Hai khoảng cách có liên quan: thứ nhất là quãng đường mà du khách đã đi được sau khi đi sai hướng và thứ hai là khoảng cách còn lại từ điểm xuất phát đến đích dọc theo lộ trình đúng. 

Sau khi nhận ra sai lầm, họ hoàn tác một cách hiệu quả một phần hành trình của mình bằng cách quay lại, sau đó tiếp tục đi theo con đường đúng đắn. Tổng quãng đường họ thực sự đi được là tổng của đoạn đi sai hướng và lộ trình đã sửa để đến đích. 

Vì vậy, nhiệm vụ giảm xuống còn tính toán tổng số km đã đi khi kết hợp đường vòng và chuyến đi chính xác cuối cùng. 

Đầu vào cung cấp hai số nguyên A và B. A là quãng đường đi sai hướng trước khi quay đầu lại. B là khoảng cách chính xác từ điểm xuất phát đến điểm đích. Đầu ra là tổng quãng đường đã đi, bao gồm cả đường đi sai và đường đi đúng. 

Các ràng buộc nhỏ, với mỗi giá trị lên tới 10^6. Điều này ngay lập tức loại trừ mọi nhu cầu về cấu trúc dữ liệu nâng cao hoặc tối ưu hóa. Một biểu thức số học theo thời gian không đổi là đủ. 

Không có trường hợp cạnh tinh vi nào liên quan đến thứ tự, tính chẵn lẻ hoặc cấu trúc. Trường hợp cạnh có ý nghĩa duy nhất là khi A hoặc B bằng 1, nhưng ngay cả khi đó phép tính vẫn là tổng số học trực tiếp. Điều quan trọng là phải hiểu rằng đoạn đi sai hướng không bị hủy bỏ, nó được di chuyển hai lần: một lần đi tiếp không chính xác và một lần quay lại đúng đường. 

## Phương pháp tiếp cận 

Một cách giải thích thô bạo sẽ mô phỏng cuộc hành trình theo từng bước. Người ta có thể tưởng tượng việc di chuyển A km về phía trước theo hướng sai, sau đó A km quay lại điểm xuất phát và cuối cùng là B km về phía đích. Điều này tạo ra tổng khoảng cách di chuyển là A + A + B. Mặc dù điều này hợp lệ về mặt khái niệm nhưng nó đưa ra sự mô phỏng không cần thiết. 

Cấu trúc của vấn đề làm cho việc mô phỏng trở nên dư thừa. Việc di chuyển sai hướng được phản ánh chính xác khi quay đầu xe, nghĩa là đường vòng đóng góp gấp đôi chiều dài của nó vào khoảng cách cuối cùng. Đường dẫn chính xác sau đó sẽ được thêm một lần. Điều này thu gọn quy trình thành một công thức trực tiếp mà không cần lặp lại. 

Quan sát quan trọng là hành trình bao gồm ba đoạn tuyến tính có độ dài được xác định đầy đủ bởi các giá trị đầu vào. Không có sự phụ thuộc giữa các bước yêu cầu mô phỏng hoặc theo dõi trạng thái. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(A + B) | O(1) | Về mặt khái niệm quá chậm, không cần thiết | 
| Công thức trực tiếp (2A + B) | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên A biểu thị quãng đường đi sai hướng. Đoạn này sẽ được duyệt tổng cộng hai lần, một lần chuyển tiếp và một lần khi quay lại. 
2. Đọc số nguyên B biểu thị khoảng cách chính xác từ điểm xuất phát đến đích. Đoạn này được đi qua đúng một lần vào cuối chuyến đi. 
3. Tính tổng quãng đường là 2 × A + B. Hệ số của hai tính cho quãng đường khứ hồi do đi nhầm hướng. 
4. Xuất trực tiếp giá trị tính toán. 

### Tại sao nó hoạt động 

Đường di chuyển phân tách thành ba đoạn rời rạc: một đoạn sai về phía trước ban đầu có độ dài A, một đoạn quay về có độ dài A và một đường truyền chính xác cuối cùng có độ dài B. Các đoạn này độc lập và tuần tự, do đó tổng khoảng cách có tính cộng. Không có sự trùng lặp hoặc hủy bỏ vì khoảng cách đo lường quãng đường vật lý chứ không phải độ dịch chuyển thực. Điều này đảm bảo rằng việc tính tổng các độ dài đoạn sẽ tạo ra tổng khoảng cách chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

a = int(input().strip())
b = int(input().strip())

print(2 * a + b)
```Giải pháp đọc hai số nguyên và áp dụng trực tiếp biểu thức dẫn xuất. Việc sử dụng`strip()`đảm bảo phân tích cú pháp an toàn ngay cả khi có dòng mới. Phép nhân được thực hiện trước phép cộng trong một biểu thức, tránh các biến trung gian nhưng vẫn giữ logic rõ ràng. 

Điểm tinh tế duy nhất là nhận ra rằng việc đi sai hướng không bị loại bỏ mà được tính hai lần. Điều này được mã hóa trực tiếp trong`2 * a`. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
A = 4
B = 2
```| Bước | A | B | Tính toán | Kết quả | 
| --- | --- | --- | --- | --- | 
| Đọc đầu vào | 4 | 2 | - | - | 
| Áp dụng công thức | 4 | 2 | 2×4 + 2 | 10 | 

Hành trình đi sai hướng 4 km, quay lại 4 km, sau đó đi đúng 2 km. Bảng xác nhận việc phân tách thành ba phân đoạn. 

### Mẫu 2 

đầu vào:```
A = 5
B = 7
```| Bước | A | B | Tính toán | Kết quả | 
| --- | --- | --- | --- | --- | 
| Đọc đầu vào | 5 | 7 | - | - | 
| Áp dụng công thức | 5 | 7 | 2×5 + 7 | 17 | 

Trường hợp này cho thấy khi đường dẫn chính xác dài hơn thì cấu trúc vẫn giữ nguyên. Việc đóng góp đường vòng vẫn độc lập với B. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ có hai phép tính số học được thực hiện | 
| Không gian | O(1) | Không có bộ nhớ phụ ngoài các biến đầu vào | 

Các ràng buộc cho phép tối đa 10^6, nhưng việc tính toán hoàn toàn không phụ thuộc vào kích thước đầu vào. Giải pháp chạy ngay lập tức trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    a = int(input().strip())
    b = int(input().strip())
    return str(2 * a + b)

# provided samples
assert run("4\n2\n") == "10", "sample 1"
assert run("5\n7\n") == "17", "sample 2"
assert run("2\n4\n") == "8", "sample 3"

# minimum values
assert run("1\n1\n") == "3", "min case"

# asymmetric small values
assert run("1\n10\n") == "12", "small A, large B"

# large values
assert run("1000000\n1000000\n") == "3000000", "max case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1, 1 | 3 | độ đúng ranh giới tối thiểu | 
| 1, 10 | 12 | mất cân đối giữa các phân khúc | 
| 10^6, 10^6 | 3×10^6 | số học an toàn tràn | 

## Vỏ cạnh 

Dữ liệu đầu vào nhỏ nhất có thể xảy ra khi cả hai khoảng cách đều bằng 1. Trong trường hợp đó, người du lịch đi sai 1 km, quay lại 1 km, sau đó đi đúng 1 km, tổng cộng là 3. Công thức 2A + B tạo ra chính xác 3, xác nhận rằng ngay cả những con đường tối thiểu cũng bảo toàn được cấu trúc. 

Khi A lớn và B nhỏ, chẳng hạn như A = 10^6 và B = 1, đường vòng sẽ chiếm ưu thế trong kết quả. Việc tính toán vẫn giảm xuống còn một biểu thức duy nhất và không có vấn đề tràn nào phát sinh trong Python do các số nguyên có độ chính xác tùy ý. 

Khi B lớn và A nhỏ nhất, đường vòng trở nên không đáng kể so với đoạn cuối, nhưng nó vẫn đóng góp chính xác gấp đôi giá trị của nó. Điều này xác nhận rằng đường vòng không phụ thuộc vào độ dài tuyến đường cuối cùng và việc phân tách cộng vẫn có hiệu lực trong tất cả các chế độ.
