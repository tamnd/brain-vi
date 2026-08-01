---
title: "CF 102726E - Trụ sở chính"
description: "Bài toán yêu cầu chúng ta xác định vị trí trụ sở mới ở vị trí trung bình của tất cả người dùng. Mỗi thành phố đóng góp một số lượng người dùng nhất định và mỗi người dùng trong thành phố đó được coi là đứng trên tọa độ của thành phố."
date: "2026-08-01T22:12:16+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102726
codeforces_index: "E"
codeforces_contest_name: "UTPC Contest 9-11-20 Div. 1"
rating: 0
weight: 102726
solve_time_s: 52
verified: true
draft: false
---

[CF 102726E - Trụ sở chính](https://codeforces.com/problemset/problem/102726/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 52s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Bài toán yêu cầu chúng ta xác định vị trí trụ sở mới ở vị trí trung bình của tất cả người dùng. Mỗi thành phố đóng góp một số lượng người dùng nhất định và mỗi người dùng trong thành phố đó được coi là đứng trên tọa độ của thành phố. Vị trí cuối cùng là giá trị trung bình có trọng số của tất cả các tọa độ thành phố, trong đó giá trị dân số là trọng số. 

Đầu vào mô tả một số thành phố. Một thành phố có tọa độ x, tọa độ y và số dân. Đầu ra là tọa độ của trụ sở chính, nghĩa là vị trí x trung bình và vị trí y trung bình của mỗi người dùng. 

Số lượng thành phố dưới 1000, giá trị tọa độ và dân số cũng nhỏ. Điều này có nghĩa là ngay cả việc quét O(n) đơn giản cũng đủ nhanh. Những cách tiếp cận phức tạp hơn như sắp xếp, tìm kiếm hoặc thuật toán hình học là không cần thiết. Thách thức chính không phải là hiệu suất mà là chuyển mô tả thế giới thực sang công thức toán học chính xác. 

Một lỗi phổ biến là tính tọa độ trung bình của thành phố mà không xem xét đến dân số. Ví dụ: nếu một thành phố có một người dùng và thành phố khác có một nghìn người dùng, thì việc đối xử bình đẳng với cả hai thành phố sẽ đưa ra sai trung tâm. Một sai lầm khác là chia tổng x và y cho số thành phố thay vì tổng dân số. 

Ví dụ, hãy xem xét:```
2
0 0 1
10 0 9
```Đầu ra đúng là:```
9.0000 0.0000
```bởi vì chín phần mười số người dùng ở mức x = 10. Một giải pháp bất cẩn tính trung bình các thành phố sẽ cho ra x = 5. 

Một trường hợp khác là khi tất cả các thành phố đều có cùng vị trí.```
3
5 7 1
5 7 100
5 7 50
```Câu trả lời vẫn phải là:```
5.0000 7.0000
```bởi vì mọi người dùng đều đã ở vị trí đó. Giải pháp sử dụng phép chia số nguyên hoặc làm tròn không cần thiết có thể làm mất độ chính xác. 

## Phương pháp tiếp cận 

Giải thích bạo lực là tưởng tượng từng người dùng riêng lẻ và tính trung bình tất cả tọa độ của họ. Điều này đúng vì trụ sở chính được xác định là trung tâm của tất cả người dùng. Tuy nhiên, việc mở rộng dân số là không cần thiết. Nếu một thành phố có 999 người dùng, việc lưu trữ 999 điểm giống hệt nhau chỉ làm tăng công việc mà không cần thêm thông tin. Tổng số người dùng cũng có thể lớn hơn nhiều so với số lượng thành phố, vì vậy cách trình bày này là sai. 

Quan sát quan trọng là nhiều điểm giống nhau có thể được nén thành một điểm có trọng số. Một thành phố đóng góp x nhân dân số của nó vào tổng tọa độ x và y nhân dân số của nó vào tổng tọa độ y. Sau khi xử lý tất cả các thành phố, việc chia số tiền đó cho tổng dân số sẽ cho kết quả giống hệt như tính trung bình cho từng người dùng riêng biệt. 

Lực lượng vũ phu hoạt động vì mọi người dùng đều đóng góp như nhau, nhưng nó thất bại vì nó lặp lại công việc giống hệt nhau cho những người dùng từ cùng một thành phố. Việc quan sát rằng các thành phố có thể được coi là các điểm có trọng số sẽ giảm toàn bộ nhiệm vụ xuống một đường chuyền tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(tổng số người dùng) | O(tổng số người dùng) | Quá chậm nếu dân số được mở rộng | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số thành phố và khởi tạo ba bộ tích lũy: tổng dân số, tổng tọa độ x có trọng số và tổng tọa độ y có trọng số. 

Tổng có trọng số thể hiện sự đóng góp tổng hợp của mọi người dùng mà không tạo các mục riêng biệt cho họ một cách rõ ràng. 

1. Đối với mỗi thành phố, nhân tọa độ x với dân số của thành phố đó và cộng kết quả vào tổng x. Làm tương tự cho tọa độ y. Thêm dân số vào tổng dân số. 

Một thành phố có dân số p tương đương với p người dùng giống hệt nhau, do đó đóng góp của nó chính xác là p bản sao tọa độ của nó. 

1. Sau khi tất cả các thành phố được xử lý, chia tổng tọa độ có trọng số cho tổng dân số. 

Các giá trị kết quả là tọa độ trung tâm của tất cả người dùng. 

1. In tọa độ với độ chính xác thập phân đủ để đáp ứng giới hạn sai số yêu cầu. 

Phép tính chỉ sử dụng dấu phẩy động ở phép chia cuối cùng, giúp tránh mất độ chính xác không cần thiết. 

Tại sao nó hoạt động: 

Mỗi người dùng đóng góp chính xác một bản sao tọa độ của họ vào mức trung bình. Một thành phố có dân số p đóng góp giống như p người dùng riêng biệt ở cùng một địa điểm. Thuật toán thay thế những đóng góp giống hệt p đó bằng một phép nhân với p, do đó tổng có trọng số giống với tổng từ danh sách người dùng mở rộng. Do đó, chia cho tổng số người dùng sẽ tạo ra chính xác vị trí trung bình cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    
    sum_x = 0
    sum_y = 0
    total = 0
    
    for _ in range(n):
        x, y, p = map(int, input().split())
        sum_x += x * p
        sum_y += y * p
        total += p
    
    ans_x = sum_x / total
    ans_y = sum_y / total
    
    print(f"{ans_x:.10f} {ans_y:.10f}")

if __name__ == "__main__":
    solve()
```Ba biến`sum_x`,`sum_y`, Và`total`lưu trữ thông tin toán học đầy đủ cần thiết cho câu trả lời. Không có dữ liệu thành phố nào cần được lưu sau khi được xử lý. 

Phép nhân được thực hiện trước phép cộng vì mỗi đóng góp tọa độ phải được chia tỷ lệ theo dân số của thành phố đó. Số nguyên Python xử lý các giá trị trung gian lớn một cách an toàn, do đó không có vấn đề tràn. 

Phép chia cuối cùng chuyển đổi tổng có trọng số nguyên thành tọa độ dấu phẩy động. Việc in mười chữ số sau dấu thập phân sẽ mang lại độ chính xác cao hơn nhiều so với lề yêu cầu. 

## Ví dụ đã hoạt động 

Đối với ví dụ đầu tiên:```
3
-10 6 4
1 -9 3
8 8 3
```Dấu vết là: 

| Thành phố | tổng_x | tổng_y | tổng dân số | 
| --- | --- | --- | --- | 
| Bắt đầu | 0 | 0 | 0 | 
| -10, 6, 4 | -40 | 24 | 4 | 
| 1, -9, 3 | -37 | -3 | 7 | 
| 8, 8, 3 | -13 | 21 | 10 | 

Tọa độ cuối cùng là:```
-13 / 10 = -1.3
21 / 10 = 2.1
```Điều này chứng tỏ rằng các quần thể ảnh hưởng đến kết quả thông qua các tổng có trọng số. 

Ví dụ thứ hai:```
2
0 0 1
10 10 1
```Dấu vết là: 

| Thành phố | tổng_x | tổng_y | tổng dân số | 
| --- | --- | --- | --- | 
| Bắt đầu | 0 | 0 | 0 | 
| 0, 0, 1 | 0 | 0 | 1 | 
| 10, 10, 1 | 10 | 10 | 2 | 

Câu trả lời trở thành:```
5.0000000000 5.0000000000
```Điều này xác nhận rằng khi tất cả các trọng số bằng nhau thì công thức sẽ trở thành trung bình cộng số học thông thường. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi thành phố được xử lý một lần | 
| Không gian | O(1) | Chỉ có ba tổng số đang chạy được lưu trữ | 

Các ràng buộc cho phép quét tuyến tính dễ dàng. Thuật toán không phụ thuộc vào tổng số người dùng mà chỉ phụ thuộc vào số lượng thành phố nên vẫn hiệu quả ngay cả khi giá trị dân số lớn. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve():
    input = sys.stdin.readline
    n_line = input().strip()
    if not n_line:
        return
    n = int(n_line)
    
    sx = sy = total = 0
    for _ in range(n):
        x, y, p = map(int, input().split())
        sx += x * p
        sy += y * p
        total += p
    
    print(f"{sx / total:.10f} {sy / total:.10f}")

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    old_out = sys.stdout
    sys.stdout = out
    solve()
    sys.stdin = old
    sys.stdout = old_out
    return out.getvalue()

assert run("""3
-10 6 4
1 -9 3
8 8 3
""") == "-1.3000000000 2.1000000000\n", "sample 1"

assert run("""2
0 0 1
10 10 1
""") == "5.0000000000 5.0000000000\n", "sample 2"

assert run("""1
7 -3 100
""") == "7.0000000000 -3.0000000000\n", "single city"

assert run("""2
0 0 1
10 0 9
""") == "9.0000000000 0.0000000000\n", "population weighting"

assert run("""3
5 7 1
5 7 100
5 7 50
""") == "5.0000000000 7.0000000000\n", "all equal positions"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Thành phố đơn lẻ | Cùng tọa độ thành phố | Xử lý kích thước tối thiểu | 
| Hai thành phố có dân số khác nhau | Vị trí có trọng số | Dân số phải ảnh hưởng đến câu trả lời | 
| Nhiều địa điểm giống hệt nhau | Cùng địa điểm | Tránh sai sót về độ chính xác và phân chia | 

## Vỏ cạnh 

Một thành phố duy nhất là đầu vào nhỏ nhất có thể:```
1
7 -3 100
```Thuật toán tính tổng có trọng số của 700 và -300 với tổng dân số 100, tạo ra chính xác`(7, -3)`. Không có trường hợp đặc biệt nào vì công thức chung đã xử lý được trường hợp đó rồi. 

Khi một thành phố có dân số lớn hơn nhiều so với thành phố khác:```
2
0 0 1
10 0 9
```tổng có trọng số trở thành x = 90 và dân số = 10, do đó kết quả là x = 9. Điều này nắm bắt các hoạt động triển khai tính trung bình các thành phố thay vì người dùng. 

Khi mọi thành phố có chung tọa độ:```
3
5 7 1
5 7 100
5 7 50
```tổng có trọng số chỉ đơn giản là cùng tọa độ nhân với tổng dân số. Phép chia trả về điểm ban đầu nên thuật toán xử lý trường hợp này một cách tự nhiên mà không cần thêm điều kiện.
