---
title: "CF 226A - Phân đoạn đĩa bay"
description: "Chúng ta được yêu cầu tính thời gian tối thiểu cần thiết để một nhóm n người ngoài hành tinh di chuyển từ phần thứ ba của một con tàu vũ trụ gồm ba phần đến phần đầu tiên."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "math"]
categories: ["algorithms"]
codeforces_contest: 226
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 140 (Div. 1)"
rating: 1400
weight: 226
solve_time_s: 343
verified: true
draft: false
---

[CF 226A - Phân đoạn đĩa bay](https://codeforces.com/problemset/problem/226/A) 

**Đánh giá:** 1400 
**Thẻ:** toán 
**Thời gian giải:** 5 phút 43s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu tính thời gian tối thiểu cần thiết để một nhóm _n_ người ngoài hành tinh di chuyển từ phần thứ ba của một con tàu vũ trụ gồm ba phần đến phần đầu tiên. Các phần được liên kết tuyến tính: phần 1 kết nối với phần 2, phần 2 kết nối với cả phần 1 và 3, phần 3 chỉ kết nối với phần 2. Mỗi lần chỉ có một người ngoài hành tinh có thể di chuyển và một người ngoài hành tinh chỉ có thể di chuyển giữa các phần liền kề nếu có cấp bậc cao hơn so với mọi người ngoài hành tinh hiện có trong phần nguồn và đích. Mỗi lần di chuyển mất đúng một phút và mục tiêu là xác định tổng thời gian để tất cả người ngoài hành tinh đến được phần 1, modulo _m_. 

Dữ liệu nhập chứa hai số _n_ và _m_, với cả hai số đều lên tới 10^9. Bởi vì _n_ có thể cực kỳ lớn nên mọi giải pháp mô phỏng từng bước di chuyển riêng lẻ sẽ quá chậm. Chúng ta cần một phương pháp tính toán tổng thời gian mà không cần lặp lại từng người ngoài hành tinh. 

Các trường hợp đặc biệt cần chú ý bao gồm khi chỉ có một người ngoài hành tinh. Trong trường hợp đó, người ngoài hành tinh di chuyển từ phần 3 sang phần 2, rồi phần 2 sang phần 1, mất hai phút. Một trường hợp khác là khi _n_ rất lớn, trong đó phép lặp đơn giản sẽ vượt quá thời gian tính toán hợp lý. Phép toán modulo giới thiệu một sự tinh tế khác: phải cẩn thận áp dụng nó vào đúng điểm để tránh tràn số nguyên. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng mô phỏng mọi chuyển động của người ngoài hành tinh theo quy tắc. Đối với mỗi người ngoài hành tinh, chúng tôi sẽ kiểm tra thứ hạng của tất cả người ngoài hành tinh trong phần nguồn và đích trước khi di chuyển và chỉ di chuyển người ngoài hành tinh nếu người đó cấp cao hơn những người khác. Đếm từng nước đi cuối cùng sẽ cho ra tổng thời gian. Phương pháp này đúng về mặt logic, nhưng nó liên quan đến các hoạt động O(n^2) trong trường hợp xấu nhất vì mỗi lần di chuyển đều yêu cầu kiểm tra những người hiện đang cư trú trong các phần liên quan. Với n lên tới 10^9 thì điều này là không thể. 

Cái nhìn sâu sắc quan trọng để tăng tốc giải pháp là nhận ra rằng vấn đề tương đương với một chuỗi tổ hợp đã biết. Nếu chúng ta đánh số người ngoài hành tinh từ 1 đến n với thâm niên tăng dần, thời gian để tất cả người ngoài hành tinh di chuyển từ phần 3 đến phần 1 sẽ tuân theo mô hình đệ quy. Cho phép`T(n)`là thời gian tối thiểu đối với _n_ người ngoài hành tinh. Người ngoài hành tinh cuối cùng di chuyển là người cao cấp nhất, người có thể di chuyển tự do. Trước đó, tất cả n-1 người ngoài hành tinh khác phải đạt đến phần 2 để cho phép người ngoài hành tinh cấp cao đi qua. Sử dụng đệ quy, người ta có thể rút ra được điều đó`T(n) = 2*T(n-1) + 2`, đơn giản hóa thành công thức đóng`T(n) = 2^(n+1) - 2`. Khi đã biết công thức này, chúng ta có thể tính kết quả bằng cách sử dụng lũy ​​thừa mô-đun để xử lý _n_ lớn một cách hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(n) | Quá chậm | 
| Tối ưu | O(log n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xác định mối quan hệ lặp lại. Định nghĩa`T(n)`là số bước di chuyển tối thiểu cần thiết cho n người ngoài hành tinh. Bằng cách quan sát cấu trúc, người ngoài hành tinh cuối cùng di chuyển (người cao cấp nhất) chỉ có thể làm như vậy sau khi tất cả n-1 người ngoài hành tinh khác đều ở phần 2. Các động thái di chuyển n-1 người ngoài hành tinh đến phần 2 cũng tuân theo mô hình đệ quy tương tự. 
2. Thiết lập phép truy hồi:`T(n) = 2*T(n-1) + 2`. các`+2`tính đến việc di chuyển của người ngoài hành tinh cao cấp nhất từ ​​phần 3 đến 2 rồi từ 2 đến 1. Hệ số 2 phát sinh do n-1 người ngoài hành tinh phải di chuyển sang một bên, sau đó quay trở lại phần 2 sau khi người ngoài hành tinh cao cấp đi qua, phản ánh bài toán "Tháp Hà Nội" kinh điển. 
3. Giải quyết tình trạng tái phát. Khai triển đệ quy, ta thấy công thức đóng là`T(n) = 2^(n+1) - 2`. Điều này mang lại những bước di chuyển tối thiểu trực tiếp mà không cần mô phỏng. 
4. Tính toán`T(n) mod m`. Với n lớn, sử dụng lũy ​​thừa mô đun để tính`2^(n+1) % m`hiệu quả trong thời gian O(log n). 
5. Trả về kết quả. Từ`T(n) = 2^(n+1) - 2`, tính toán`(pow(2, n+1, m) - 2) % m`để xử lý modulo và ngăn chặn kết quả tiêu cực. 

Tại sao nó hoạt động: Điều bất biến chính là ở mọi giai đoạn, người ngoài hành tinh cấp cao nhất chưa di chuyển phải được phép đi qua, điều này buộc tất cả những người ngoài hành tinh nhỏ hơn phải tạm thời di dời. Điều này thực thi hành vi nhân đôi trong công thức đệ quy. Công thức khép kín đếm chính xác từng bước di chuyển và số học mô-đun đảm bảo tính chính xác cho số lượng lớn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m = map(int, input().split())

# calculate 2^(n+1) % m
result = pow(2, n + 1, m) - 2
# ensure non-negative result modulo m
print(result % m)
```Giải pháp này đọc n và m, tính toán`2^(n+1) mod m`bằng cách sử dụng hàm lũy thừa nhanh tích hợp của Python, trừ 2 để tính các bước di chuyển cơ sở và cuối cùng áp dụng lại modulo m để đảm bảo kết quả không âm. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:`1 10`| Biến | Giá trị | 
| --- | --- | 
| n | 1 | 
| công thức T(1) | 2^(1+1) - 2 = 4 - 2 | 
| kết quả % 10 | 2 | 

Bảng xác nhận rằng một người ngoài hành tinh mất đúng hai phút: từ phân đoạn 3 đến 2, sau đó từ 2 đến 1. 

### Ví dụ tùy chỉnh 

đầu vào:`3 100`| Biến | Giá trị | 
| --- | --- | 
| n | 3 | 
| công thức T(3) | 2^(3+1) - 2 = 16 - 2 | 
| kết quả % 100 | 14 | 

Điều này phù hợp với lý luận đệ quy: hai người ngoài hành tinh đầu tiên phải di chuyển sang một bên để cho phép người ngoài hành tinh thứ ba, dẫn đến tổng cộng 14 bước đi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(log n) | lũy thừa mô đun với số mũ n+1 là logarit theo n. | 
| Không gian | O(1) | Chỉ có một vài biến số nguyên được sử dụng. | 

Giải pháp dễ dàng phù hợp với các ràng buộc: ngay cả với n lên đến 10^9, các thao tác O(log n) có khoảng 30 bước trong thực tế, được thực thi ngay lập tức. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, m = map(int, input().split())
    return str((pow(2, n+1, m) - 2) % m)

# provided samples
assert run("1 10\n") == "2", "sample 1"
# custom cases
assert run("3 100\n") == "14", "3 aliens"
assert run("0 5\n") == "0", "no aliens"
assert run("5 1\n") == "0", "mod 1"
assert run("10 1000\n") == "46", "10 aliens modulo 1000"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 10 | 2 | Trường hợp người ngoài hành tinh đơn | 
| 3 100 | 14 | Nhiều người ngoài hành tinh, modulo nhỏ | 
| 0 5 | 0 | Trường hợp cạnh: không có người ngoài hành tinh | 
| 5 1 | 0 | Vỏ Modulo 1 cạnh | 
| 10 1000 | 46 | N lớn hơn, giảm modulo | 

## Vỏ cạnh 

Với n = 0, công thức`2^(0+1)-2 = 0`trả về chính xác 0, vì không có người ngoài hành tinh nào cần di chuyển. Đối với n = 1, công thức mang lại 2, khớp với việc di chuyển trực tiếp từ phần 3 đến 2 và sau đó từ 2 đến 1. Các giá trị n lớn, chẳng hạn như 10^9, được xử lý chính xác bằng phép lũy thừa mô-đun, giúp tránh tính toán trực tiếp toàn bộ 2^(n+1) và ngăn ngừa tràn số nguyên. Hoạt động modulo đảm bảo kết quả luôn nằm trong [0, m-1], ngay cả khi trừ 2.
