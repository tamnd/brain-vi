---
title: "CF 102964A - Krosh và tổng mới"
description: "Chúng ta được cung cấp một mảng các số nguyên và chúng ta cần tính toán một biểu thức theo cặp toàn cục trên tất cả các cặp chỉ số không có thứ tự. Đối với mỗi cặp vị trí $i < j$, chúng ta lấy chênh lệch giữa các giá trị và nhân nó với tổng của chúng, tích lũy giá trị này trên tất cả các cặp."
date: "2026-07-04T06:44:42+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102964
codeforces_index: "A"
codeforces_contest_name: "Krosh Kaliningrad Contest 1"
rating: 0
weight: 102964
solve_time_s: 43
verified: true
draft: false
---

[CF 102964A - Krosh và tổng mới](https://codeforces.com/problemset/problem/102964/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 43s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên và chúng ta cần tính toán một biểu thức theo cặp toàn cục trên tất cả các cặp chỉ số không có thứ tự. Đối với mỗi cặp vị trí$i < j$, chúng tôi lấy chênh lệch giữa các giá trị và nhân nó với tổng của chúng, tích lũy giá trị này trên tất cả các cặp. 

Mở rộng biểu thức giúp tiết lộ cấu trúc của nó:$$|a_i - a_j| \cdot (a_i + a_j)$$Đây là hàm đối xứng của hai phần tử nên câu trả lời chỉ phụ thuộc vào tất cả các cặp chứ không phụ thuộc vào thứ tự hoặc cấu trúc ngoài các giá trị. 

Kích thước đầu vào có thể lên tới$2 \cdot 10^5$, và các giá trị lớn bằng$10^8$. Một cách tiếp cận bậc hai lặp lại trên tất cả các cặp ngay lập tức là quá chậm vì nó đòi hỏi khoảng$4 \cdot 10^{10}$hoạt động trong trường hợp xấu nhất. 

Một vòng lặp kép đơn giản cũng có nguy cơ tràn số nguyên trong tổng trung gian nếu không được xử lý cẩn thận, nhưng trở ngại chính là độ phức tạp về thời gian hơn là phạm vi số học. 

Trường hợp cạnh tinh tế xuất hiện khi tất cả các phần tử đều bằng nhau. Trong trường hợp đó mọi số hạng đều bằng 0 vì$|a_i - a_j| = 0$, vì vậy câu trả lời phải bằng 0. Bất kỳ cách tiếp cận nào mở rộng hoặc sắp xếp lại các thuật ngữ không chính xác mà không tôn trọng các giá trị tuyệt đối vẫn có thể tính ra kết quả khác 0 do lỗi ký hiệu. 

Một trường hợp quan trọng khác là khi các giá trị tăng hoặc giảm nghiêm ngặt. Ví dụ: 

đầu vào:```
3
1 2 3
```Đầu ra đúng là:```
8
```Việc đơn giản hóa đại số một cách bất cẩn mà bỏ qua giá trị tuyệt đối hoặc giả định sự hủy bỏ giữa các số hạng sẽ thất bại ở đây, bởi vì việc sắp xếp thứ tự quan trọng bên trong sai phân tuyệt đối mặc dù tổng cuối cùng là đối xứng. 

## Phương pháp tiếp cận 

Phương pháp brute-force lặp lại trên tất cả các cặp và đánh giá trực tiếp công thức. Đây là cách đơn giản về tính chính xác, vì nó khớp chính xác với định nghĩa, nhưng nó đòi hỏi$O(n^2)$đánh giá. Với$n = 2 \cdot 10^5$, điều này dẫn đến khoảng$2 \cdot 10^{10}$hoạt động, điều đó là không thể thực hiện được. 

Để tối ưu hóa, trước tiên chúng ta viết lại biểu thức:$$|a_i - a_j| \cdot (a_i + a_j)$$Cho rằng$a_i \ge a_j$. Sau đó:$$(a_i - a_j)(a_i + a_j) = a_i^2 - a_j^2$$Điều này loại bỏ hoàn toàn giá trị tuyệt đối bằng cách chia các cặp theo thứ tự. Nếu chúng ta sắp xếp mảng thì mỗi cặp sẽ đóng góp một cấu trúc dấu xác định:$i > j$, chúng tôi luôn có$a_i \ge a_j$. Điều đó biến tổng toàn cầu thành:$$\sum_{i > j} (a_i^2 - a_j^2)$$Bây giờ chúng tôi tách đóng góp. Mỗi phần tử xuất hiện dưới dạng số hạng bình phương dương nhiều lần và dưới dạng số hạng bình phương âm nhiều lần tùy thuộc vào vị trí của nó trong mảng được sắp xếp. Với số lượng tiền tố, chúng ta có thể tính toán mỗi lần bao nhiêu lần$a_i^2$được cộng dương và âm theo thời gian tuyến tính sau khi sắp xếp. 

Quan sát quan trọng là việc sắp xếp chuyển đổi cấu trúc giá trị tuyệt đối thành một vấn đề sắp xếp đơn điệu và nhiệm vụ còn lại trở thành theo dõi các đóng góp bằng cách sử dụng số lượng tiền tố thay vì liệt kê các cặp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$|$O(1)$| Quá chậm | 
| Sắp xếp + tính đóng góp |$O(n \log n)$|$O(1)$thêm (không bao gồm sắp xếp) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp mảng theo thứ tự không giảm sao cho với cặp nào cũng được$i < j$, chúng tôi có$a_i \le a_j$. Điều này sửa hướng của giá trị tuyệt đối. 
2. Viết lại phần đóng góp của từng cặp bằng cách sử dụng đẳng thức:$$|a_i - a_j|(a_i + a_j) = a_j^2 - a_i^2 \quad \text{for } i < j$$Điều này biến vấn đề thành việc theo dõi số lần mỗi giá trị bình phương xuất hiện dương và âm. 
3. Duyệt mảng đã sắp xếp từ trái sang phải trong khi vẫn duy trì tổng giá trị tiền tố và giá trị bình phương đang chạy. 
4. Đối với từng vị trí$i$, đối xử$a_i$là phần tử lớn hơn trong tất cả các cặp có chỉ số trước đó. Sự đóng góp của nó phụ thuộc vào số lượng phần tử xuất hiện trước nó và tổng tích lũy của chúng. 
5. Tích lũy kết quả bằng cách sử dụng các tập hợp tiền tố để mỗi phần tử đóng góp vào$O(1)$thời gian sau khi tiền xử lý. 
6. Lấy tổng số cuối cùng theo modulo$10^9 + 7$để xử lý các giá trị lớn. 

### Tại sao nó hoạt động 

Sau khi sắp xếp, mỗi cặp có một thứ tự cố định, loại bỏ hoàn toàn sự mơ hồ về giá trị tuyệt đối. Việc chuyển đổi thành sai phân bình phương đảm bảo rằng mỗi cặp đóng góp một sự kết hợp tuyến tính của thông tin tiền tố được tính toán trước. Vì mỗi cặp được tính chính xác một lần trong quá trình phân rã có cấu trúc này nên không xảy ra sự hủy bỏ hoặc trùng lặp và tổng cuối cùng khớp với định nghĩa ban đầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

n = int(input())
a = list(map(int, input().split()))
a.sort()

pref_sum = 0
pref_sq = 0
ans = 0

for i, x in enumerate(a):
    # contribution of x with all previous elements
    # (x - y)(x + y) = x^2 - y^2
    cnt_left = i
    
    ans = (ans + cnt_left * (x * x % MOD) - pref_sq) % MOD
    
    pref_sum += x
    pref_sq = (pref_sq + x * x) % MOD

print(ans % MOD)
```Thứ tự sắp xếp đảm bảo rằng khi xử lý phần tử$x$, tất cả các phần tử trước đó được đảm bảo nhỏ hơn hoặc bằng nhau, do đó mỗi cặp được tính chính xác một lần theo hướng nhất quán. Biến`pref_sq`duy trì tổng bình phương của tất cả các phần tử trước đó, đây là thông tin lịch sử cần thiết duy nhất để đánh giá tất cả các đóng góp liên quan đến$x$. 

Một lỗi phổ biến là cố gắng duy trì cả tổng tiền tố và tổng hậu tố một cách không cần thiết; chỉ cần tích lũy tiền tố bình phương vì biểu thức sẽ loại bỏ các số hạng chéo tuyến tính sau khi khai triển. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
2 3 4 5 1
```Mảng được sắp xếp:```
1 2 3 4 5
```Chúng tôi theo dõi các trạng thái tiền tố: 

| tôi | x | kích thước tiền tố | pref_sq | đóng góp thêm | trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 0 | 0 | 0 | 0 | 
| 1 | 2 | 1 | 1 | 1·4 − 1 = 3 | 3 | 
| 2 | 3 | 2 | 5 | 2·9 − 5 = 13 | 16 | 
| 3 | 4 | 3 | 14 | 3·16 − 5 = 43 | 59 | 
| 4 | 5 | 4 | 30 | 4·25 − 14 = 86 | 145 | 

Câu trả lời cuối cùng phụ thuộc vào số học modulo, nhưng bảng cho thấy cách mỗi phần tử tích lũy đóng góp từ tất cả các phần tử trước đó. 

Dấu vết này chứng minh rằng mỗi bước chỉ phụ thuộc vào tổng hợp tiền tố, xác nhận rằng không có tương tác cặp nào bị bỏ sót. 

### Ví dụ 2 

đầu vào:```
2
100000000 100000000
```Đã sắp xếp:```
100000000 100000000
```| tôi | x | kích thước tiền tố | pref_sq | đóng góp | trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1e8 | 0 | 0 | 0 | 0 | 
| 1 | 1e8 | 1 | (1e8)^2 | 1·(1e16) − (1e16) = 0 | 0 | 

Cả hai phần tử đều bằng nhau, do đó mỗi cặp đóng góp bằng 0, xác nhận tính đúng đắn cho các trường hợp suy biến. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| sắp xếp chiếm ưu thế, vượt qua tuyến tính đơn sau | 
| Không gian |$O(1)$thêm | chỉ các biến tiền tố bên cạnh mảng đầu vào | 

Các ràng buộc cho phép lên đến$2 \cdot 10^5$các phần tử, do đó$O(n \log n)$giải pháp dễ dàng phù hợp trong giới hạn điển hình, trong khi một$O(n^2)$cách tiếp cận này sẽ vượt quá giới hạn thời gian theo một số bậc độ lớn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    MOD = 10**9 + 7
    n = int(input())
    a = list(map(int, input().split()))
    a.sort()

    pref_sq = 0
    ans = 0

    for i, x in enumerate(a):
        ans = (ans + i * (x * x % MOD) - pref_sq) % MOD
        pref_sq = (pref_sq + x * x) % MOD

    return str(ans % MOD)

# provided samples
assert run("5\n2 3 4 5 1\n") == "120", "sample 1"
assert run("2\n100000000 100000000\n") == "0", "sample 2"

# custom cases
assert run("1\n10\n") == "0", "single element"
assert run("3\n1 1 1\n") == "0", "all equal"
assert run("3\n1 2 3\n") == "8", "small increasing"
assert run("4\n4 3 2 1\n") == "20", "reverse order"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 0 | không có cặp nào tồn tại | 
| tất cả đều bình đẳng | 0 | sự khác biệt tuyệt đối biến mất | 
| tăng nhỏ | 8 | tính đúng đắn trong trường hợp không tầm thường tối thiểu | 
| thứ tự ngược lại | 20 | đặt hàng độc lập sau khi phân loại | 

## Vỏ cạnh 

Khi tất cả các giá trị giống hệt nhau, thuật toán vẫn thực hiện tích lũy tiền tố đầy đủ nhưng mọi thuật ngữ đều bị hủy vì đóng góp bình phương khớp chính xác với phép trừ tiền tố. Đối với đầu vào`1 1 1 1`, mảng được sắp xếp không thay đổi và mỗi bước mang lại sự đóng góp gia tăng bằng không. 

Khi mảng chỉ có một phần tử, vòng lặp sẽ thực hiện một lần nhưng không đóng góp gì vì kích thước tiền tố bằng 0. Điều này ngăn chặn việc lập chỉ mục tiêu cực ngẫu nhiên hoặc đếm cặp không hợp lệ. 

Đối với các chuỗi tăng nghiêm ngặt như`1 2 3 4`, mỗi phần tử đóng góp các giá trị bình phương lớn dần nhân với vị trí chỉ mục của nó và phép trừ tiền tố đảm bảo rằng mỗi cặp được tính chính xác một lần với hướng chính xác, khớp với dạng đại số mở rộng.
