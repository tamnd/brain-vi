---
title: "CF 470G - Khoảng cách Hamming"
description: "Nhiệm vụ là tính khoảng cách Hamming giữa hai chuỗi có độ dài bằng nhau. Trong thực tế, bạn đang so sánh hai chuỗi chữ cái và đếm xem có bao nhiêu vị trí chứa các ký tự khác nhau. Mỗi sự không phù hợp đóng góp chính xác một vào khoảng cách."
date: "2026-05-30T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "*special"]
categories: ["algorithms"]
codeforces_contest: 470
codeforces_index: "G"
codeforces_contest_name: "Surprise Language Round 7"
rating: 2300
weight: 470
solve_time_s: 57
verified: true
draft: false
---

[CF 470G - Khoảng cách Hamming](https://codeforces.com/problemset/problem/470/G) 

**Đánh giá:** 2300 
**Thẻ:** *đặc biệt 
**Thời gian giải:** 57s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ là tính khoảng cách Hamming giữa hai chuỗi có độ dài bằng nhau. Trong thực tế, bạn đang so sánh hai chuỗi chữ cái và đếm xem có bao nhiêu vị trí chứa các ký tự khác nhau. Mỗi sự không phù hợp đóng góp chính xác một vào khoảng cách. Ví dụ, so sánh`CODECHEF`Và`TOPCODER`, mỗi vị trí mà các chữ cái khác nhau sẽ tăng tổng số lên một, mang lại khoảng cách là sáu. 

Các ràng buộc đầu vào rất đơn giản: các chuỗi có độ dài từ 1 đến 100. Điều này có nghĩa là kích thước vấn đề rất nhỏ, do đó, ngay cả một giải pháp so sánh rõ ràng từng ký tự một cũng sẽ chạy thoải mái trong giới hạn thời gian. Bộ nhớ cũng không phải là vấn đề đáng lo ngại vì chúng ta chỉ cần lưu trữ hai chuỗi ngắn và một bộ đếm. Các trường hợp cạnh chính liên quan đến các chuỗi ngắn nhất có độ dài 1, các chuỗi giống hệt nhau hoặc các chuỗi trong đó mỗi ký tự khác nhau. Ví dụ, các dây`A`Và`A`nên tạo ra một khoảng cách`0`, trong khi`A`Và`B`sản xuất`1`. Việc triển khai bất cẩn có thể vô tình bỏ qua ký tự đầu tiên hoặc cuối cùng hoặc đếm sai khi các chuỗi giống hệt nhau. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực cũng là cách tiếp cận tối ưu ở đây do kích thước đầu vào nhỏ. Bạn lặp song song các chuỗi, so sánh các ký tự ở cùng một chỉ mục. Mỗi khi các ký tự khác nhau, bạn sẽ tăng một bộ đếm. Ở cuối vòng lặp, bộ đếm chứa tổng khoảng cách Hamming. Phương pháp này đúng vì nó trực tiếp thực hiện định nghĩa về khoảng cách Hamming: số lượng các vị trí khác nhau. 

Không có thuật toán tiệm cận nhanh hơn cho vấn đề này vì bạn phải kiểm tra từng ký tự ít nhất một lần để xác định xem nó khớp hay khác. Bất kỳ nỗ lực nào nhằm tối ưu hóa hơn nữa bằng cách xử lý trước hoặc thao tác bit thông minh sẽ chỉ làm phức tạp giải pháp mà không cải thiện thời gian chạy, vì n ≤ 100. Những cạm bẫy tiềm ẩn duy nhất là các lỗi riêng lẻ trong việc lập chỉ mục và quên xử lý chính xác các chuỗi có độ dài 1. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) | O(1) | Đã chấp nhận | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc hai chuỗi đầu vào. Vì các chuỗi được đảm bảo có độ dài bằng nhau nên không cần kiểm tra bổ sung. 
2. Khởi tạo một biến đếm về 0. Điều này sẽ giữ khoảng cách Hamming. 
3. Lặp lại các chỉ số của chuỗi từ 0 đến n-1. Đối với mỗi chỉ mục, hãy so sánh các ký tự từ cả hai chuỗi. 
4. Nếu các ký tự khác nhau, hãy tăng bộ đếm lên một. Điều này nắm bắt trực tiếp định nghĩa về khoảng cách Hamming. 
5. Sau khi quá trình lặp hoàn tất, hãy in bộ đếm. Điều này thể hiện tổng số vị trí không khớp. 

Tại sao nó hoạt động: thuật toán duy trì một bất biến rằng bộ đếm luôn bằng số lượng ký tự không khớp được nhìn thấy cho đến nay. Bằng cách lặp qua mọi vị trí và kiểm tra các ký tự chính xác một lần, chúng tôi đảm bảo rằng không có sự trùng khớp nào bị bỏ sót hoặc bị tính hai lần. Vì mọi chỉ số đều được xem xét nên bộ đếm cuối cùng chính xác là khoảng cách Hamming. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

s1 = input().strip()
s2 = input().strip()

distance = 0
for c1, c2 in zip(s1, s2):
    if c1 != c2:
        distance += 1

print(distance)
```Giải pháp đọc hai chuỗi bằng cách sử dụng đầu vào nhanh. các`strip()`cuộc gọi sẽ xóa mọi ký tự dòng mới ở cuối có thể gây ra sự không khớp sai. Vòng lặp sử dụng`zip`để lặp lại cả hai chuỗi cùng một lúc, điều này sẽ sạch hơn so với việc sử dụng bộ đếm chỉ mục. Mỗi sự không khớp sẽ làm tăng bộ đếm khoảng cách. Cuối cùng, chúng tôi in kết quả. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:`CODECHEF`Và`TOPCODER`| Chỉ mục | s1[i] | s2[i] | Cuộc thi đấu? | Khoảng cách | 
| --- | --- | --- | --- | --- | 
| 0 | C | T | Không | 1 | 
| 1 | Ồ | Ồ | Có | 1 | 
| 2 | D | P | Không | 2 | 
| 3 | E | C | Không | 3 | 
| 4 | C | Ồ | Không | 4 | 
| 5 | H | D | Không | 5 | 
| 6 | E | E | Có | 5 | 
| 7 | F | R | Không | 6 | 

Điều này chứng tỏ rằng thuật toán chỉ tăng chính xác bộ đếm khi không khớp và tạo ra 6. 

### Mẫu tùy chỉnh 

đầu vào:`AAAA`Và`ABAA`| Chỉ mục | s1[i] | s2[i] | Cuộc thi đấu? | Khoảng cách | 
| --- | --- | --- | --- | --- | 
| 0 | A | A | Có | 0 | 
| 1 | A | B | Không | 1 | 
| 2 | A | A | Có | 1 | 
| 3 | A | A | Có | 1 | 

Khoảng cách là 1, xác nhận rằng các điểm không khớp đơn được tính chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ký tự được truy cập đúng một lần để so sánh. n 100 nên tốc độ này rất nhanh. | 
| Không gian | O(1) | Chỉ có một bộ đếm số nguyên duy nhất được sử dụng ngoài chuỗi đầu vào. | 

Vì n ≤ 100 và các thao tác trên mỗi lần lặp không đổi nên giải pháp chạy thoải mái trong dưới 2 giây và sử dụng bộ nhớ tối thiểu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    s1 = input().strip()
    s2 = input().strip()
    distance = sum(1 for c1, c2 in zip(s1, s2) if c1 != c2)
    return str(distance)

# provided sample
assert run("CODECHEF\nTOPCODER\n") == "6", "sample 1"

# minimum-size input, identical
assert run("A\nA\n") == "0", "single char equal"

# minimum-size input, different
assert run("A\nB\n") == "1", "single char different"

# all characters equal
assert run("XYZXYZ\nXYZXYZ\n") == "0", "all equal"

# all characters different
assert run("AAAA\nBBBB\n") == "4", "all different"

# mixed case
assert run("ABCD\nACBD\n") == "2", "mixed differences"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| A\nA | 0 | Chuỗi ký tự đơn giống hệt nhau | 
| A\nB | 1 | Chuỗi ký tự đơn khác nhau | 
| XYZXYZ\nXYZXYZ | 0 | Chuỗi dài hơn trong đó tất cả các ký tự đều bằng nhau | 
| AAAA\nBBBB | 4 | Chuỗi dài hơn trong đó tất cả các ký tự khác nhau | 
| ABCD\nACBD | 2 | Các chuỗi khớp một phần để kiểm tra cách đếm chính xác | 

## Vỏ cạnh 

Đối với các chuỗi nhỏ nhất, như`A`Và`B`, thuật toán khởi tạo`distance = 0`, so sánh ký tự duy nhất, tăng một lần vì chúng khác nhau và in`1`. Đối với các chuỗi giống hệt nhau như`XYZXYZ`Và`XYZXYZ`, bộ đếm không bao giờ tăng, mang lại`0`. Các chuỗi trong đó mỗi ký tự khác nhau, như`AAAA`Và`BBBB`, tăng bộ đếm ở mọi chỉ mục, tạo ra độ dài của chuỗi làm khoảng cách. Trong mọi trường hợp, bất biến lặp lại đảm bảo tính chính xác và giải pháp xử lý đầy đủ các dữ liệu đầu vào có thể có.
