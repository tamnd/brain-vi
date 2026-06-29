---
title: "CF 104617A - Hãy đến Choppa!"
description: "Chúng tôi được cung cấp một bộ sưu tập các khối đá, mỗi khối được gắn thẻ tên hương vị và một con số biểu thị thời gian khối đó tan chảy."
date: "2026-06-29T17:33:40+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104617
codeforces_index: "A"
codeforces_contest_name: "UTPC Contest 09-22-23 Div. 2 (Beginner)"
rating: 0
weight: 104617
solve_time_s: 60
verified: true
draft: false
---

[CF 104617A - Đến Choppa!](https://codeforces.com/problemset/problem/104617/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một bộ sưu tập các khối đá, mỗi khối được gắn thẻ tên hương vị và một con số biểu thị thời gian khối đó tan chảy. Nhà máy cần quyết định thứ tự gửi các khối này đến máy cắt để những khối nào biến mất trước sẽ được xử lý trước. Nói cách khác, chúng tôi muốn sắp xếp lại các hương vị bằng cách tăng thời gian tan chảy. 

Đầu vào chỉ đơn giản là một danh sách các cặp. Mỗi cặp chứa một số nguyên riêng biệt và một chuỗi duy nhất. Số nguyên là khóa sắp xếp và chuỗi là nhãn chúng ta phải xuất. Nhiệm vụ là xuất ra tất cả các chuỗi được sắp xếp theo số nguyên liên quan của chúng. 

Các ràng buộc đủ lớn để bất kỳ cách tiếp cận nào tệ hơn thời gian tuyến tính sẽ gặp khó khăn. Với tối đa 100000 mục, thuật toán xung quanh O(N log N) là mức trần thực tế. Bất cứ điều gì bậc hai, chẳng hạn như liên tục chọn mức tối thiểu từ danh sách chưa được sắp xếp, đều có rủi ro theo thứ tự 10 tỷ so sánh trong trường hợp xấu nhất, điều này không thể thực hiện được trong 2 giây. 

Một vấn đề tế nhị là ở đây không cần phải có sự ổn định vì tất cả thời gian tan chảy đều khác nhau. Điều đó loại bỏ sự mơ hồ trong việc đặt hàng. 

Các trường hợp Edge chủ yếu mang tính cấu trúc hơn là logic: 

Một yếu tố đầu vào duy nhất là tầm thường. Đầu ra chỉ là một hương vị. 

Dữ liệu đầu vào đã được sắp xếp hoặc sắp xếp ngược không làm thay đổi tính chính xác nhưng có thể gây áp lực cho việc triển khai chưa thực hiện. 

Các chuỗi lớn không ảnh hưởng đến tính chính xác nhưng nhấn mạnh rằng khóa sắp xếp là số nguyên, không phải thứ tự từ vựng của hương vị. 

Một lỗi tiềm ẩn là vô tình sắp xếp theo chuỗi thay vì số nguyên. Ví dụ: nếu chúng tôi sắp xếp theo từ điển, "19 Strawberry" có thể xuất hiện không chính xác trước "3 Pineapple" vì so sánh chuỗi bỏ qua ý nghĩa số. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: quét liên tục danh sách để tìm thời gian tan chảy tối thiểu giữa các mục còn lại, đưa ra hương vị của nó và loại bỏ nó khỏi sự cân nhắc. Điều này đúng vì ở mỗi bước chúng ta chọn rõ ràng thời gian còn lại nhỏ nhất, do đó đầu ra được sắp xếp toàn cục. 

Tuy nhiên, mỗi lựa chọn đều yêu cầu quét tất cả các phần tử còn lại. Bước đầu tiên kiểm tra N mục, bước thứ hai kiểm tra N−1, v.v. Tổng số phép so sánh theo thứ tự N(N−1)/2, tăng lên khoảng 5×10^9 phép tính khi N là 100000. Điều này vượt xa những gì khả thi. 

Sự cải thiện đến từ việc nhận ra rằng chúng ta đang liên tục đặt ra cùng một câu hỏi: “phần tử nhỏ nhất tiếp theo trong số phần tử còn lại là gì?” Đây chính xác là vấn đề sắp xếp. Thay vì tính toán lại cực tiểu nhiều lần, chúng tôi tính toán toàn bộ thứ tự trong một lần sử dụng thuật toán sắp xếp. 

Bằng cách lưu trữ các cặp (thời gian tan chảy, hương vị) và sắp xếp theo khóa số nguyên, chúng ta chuyển bài toán thành một cách sắp xếp tiêu chuẩn. Điều này làm giảm chi phí quét lặp lại thành một loại O(N log N). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N^2) | O(N) | Quá chậm | 
| Tối ưu (sắp xếp) | O(N log N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các cặp (thời gian tan, hương vị). Chúng tôi lưu trữ thời gian trước để thứ tự sắp xếp mặc định trong Python có thể trực tiếp sử dụng nó làm khóa chính. 
2. Chèn từng cặp vào một danh sách. Không cần xử lý ngoài việc lưu trữ vì mọi quyết định chỉ phụ thuộc vào việc đặt hàng. 
3. Sắp xếp danh sách các cặp. Việc sắp xếp được thực hiện chủ yếu theo thời gian tan chảy, vì các bộ dữ liệu được so sánh theo từ điển trong Python. 
4. Duyệt qua danh sách đã sắp xếp và thu thập tên hương vị theo thứ tự. Điều này tạo ra trình tự đầu ra chính xác. 
5. In các hương vị thu được cách nhau bằng dấu cách. 

Lựa chọn thiết kế chính là lưu trữ thời gian trước chuỗi. Điều này đảm bảo rằng việc so sánh bộ dữ liệu sẽ ưu tiên đúng trường một cách tự nhiên mà không cần bộ so sánh tùy chỉnh. 

### Tại sao nó hoạt động

Tại mọi thời điểm, đầu ra chính xác tiếp theo là hương vị có thời gian tan chảy còn lại nhỏ nhất. Việc sắp xếp sắp xếp tất cả các phần tử theo thứ tự chung của khóa này, vì vậy mọi tiền tố của mảng được sắp xếp đã thỏa mãn thuộc tính rằng không có phần tử sau nào có khóa nhỏ hơn khóa trước. Điều này đảm bảo rằng việc xuất ra theo thứ tự được sắp xếp phù hợp với trình tự lựa chọn tham lam được yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    n = int(input())
    arr = []

    for _ in range(n):
        a, s = input().split()
        a = int(a)
        arr.append((a, s))

    arr.sort()

    print(" ".join(s for _, s in arr))

if __name__ == "__main__":
    main()
```Giải pháp đọc đầu vào hiệu quả bằng cách sử dụng I/O nhanh vì N có thể lớn. Mỗi dòng được chia thành một số nguyên và một chuỗi. Cặp này được lưu trữ với số nguyên trước tiên để việc sắp xếp bộ dữ liệu mặc định của Python hoạt động chính xác. 

Sắp xếp là hoạt động trung tâm. Phương pháp sắp xếp của Python sử dụng Timsort, chạy trong O(N log N) trong trường hợp chung và được tối ưu hóa cho đầu vào được sắp xếp một phần. 

Vòng lặp cuối cùng chỉ trích xuất tên hương vị. Điều này tránh việc in các bộ dữ liệu hoặc chi phí định dạng bổ sung. 

Một lỗi phổ biến là quên chuyển thời gian thành số nguyên. Nếu để lại dưới dạng chuỗi, việc sắp xếp sẽ trở thành từ điển và phá vỡ trật tự số. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
3 Pineapple
4 Grape
19 Strawberry
12 Lime
```Sau khi phân tích cú pháp: 

| Bước | Trạng thái mảng | 
| --- | --- | 
| Đọc đầu vào | (3,Dứa), (4,Nho), (19,Dâu), (12,Vôi) | 
| Sau khi sắp xếp | (3,Dứa), (4,Nho), (12,Chanh), (19,Dâu) | 

Đầu ra là:```
Pineapple Grape Lime Strawberry
```Điều này xác nhận rằng việc sắp xếp theo phím số sẽ sắp xếp chính xác các độ lớn hỗn hợp và không phụ thuộc vào thứ tự đầu vào. 

### Ví dụ 2 

đầu vào:```
5
10 A
1 B
7 C
3 D
5 E
```| Bước | Trạng thái mảng | 
| --- | --- | 
| Đọc đầu vào | (10,A), (1,B), (7,C), (3,D), (5,E) | 
| Sau khi sắp xếp | (1,B), (3,D), (5,E), (7,C), (10,A) | 

Đầu ra:```
B D E C A
```Điều này chứng tỏ tính chính xác ngay cả với đầu vào được xáo trộn hoàn toàn, trong đó việc lựa chọn tham lam sẽ yêu cầu quét lặp lại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N log N) | Việc sắp xếp chiếm ưu thế, mỗi phép so sánh là công việc liên tục trên các cặp chuỗi số nguyên | 
| Không gian | O(N) | Lưu trữ cho tất cả các cặp đầu vào | 

Các ràng buộc cho phép tối đa 100000 mục và khả năng sắp xếp O(N log N) vừa vặn thoải mái trong giới hạn thời gian trong Python, đặc biệt là với tính năng sắp xếp được tối ưu hóa tích hợp sẵn. Việc sử dụng bộ nhớ là tuyến tính ở kích thước đầu vào, chủ yếu là lưu trữ chuỗi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    arr = []
    for _ in range(n):
        a, s = input().split()
        arr.append((int(a), s))
    arr.sort()
    return " ".join(s for _, s in arr)

# provided sample
assert run("""4
3 Pineapple
4 Grape
19 Strawberry
12 Lime
""") == "Pineapple Grape Lime Strawberry"

# minimum size
assert run("""1
5 Solo
""") == "Solo"

# already sorted
assert run("""3
1 A
2 B
3 C
""") == "A B C"

# reverse order
assert run("""3
3 C
2 B
1 A
""") == "A B C"

# random order
assert run("""5
10 A
1 B
7 C
3 D
5 E
""") == "B D E C A"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | yếu tố đó | trường hợp ranh giới tối thiểu | 
| đầu vào được sắp xếp | cùng thứ tự | ổn định và đúng đắn trong trường hợp tốt nhất | 
| đầu vào ngược | thứ tự tăng dần | tính đúng đắn nhất | 
| đầu vào ngẫu nhiên | đặt hàng đúng | tính đúng đắn chung | 

## Vỏ cạnh 

Một trường hợp khối băng đơn lẻ là chuyện nhỏ nhưng quan trọng. Đối với đầu vào:```
1
42 Mango
```thuật toán lưu trữ một cặp, sắp xếp danh sách cỡ 1 và xuất ra ngay lập tức. Không có sự lặp lại nào xảy ra ngoài quá trình khởi tạo, xác nhận rằng không cần đến cách viết hoa đặc biệt. 

Một trường hợp khác là sắp xếp ngược lại:```
3
9 X
2 Y
5 Z
```Danh sách trở thành [(9,X),(2,Y),(5,Z)] và sắp xếp mang lại [(2,Y),(5,Z),(9,X)]. Thuật toán không phụ thuộc vào thứ tự đầu vào, do đó, ngay cả sự sắp xếp trong trường hợp xấu nhất cũng tạo ra đầu ra chính xác ở O(N log N). 

Cuối cùng, hãy xem xét những khoảng trống lớn về thời gian tan chảy. Ngay cả khi các giá trị như 1, 10^9, 500, thứ tự chỉ phụ thuộc vào so sánh tương đối chứ không phụ thuộc vào độ lớn. Việc sắp xếp không bị ảnh hưởng vì việc so sánh diễn ra liên tục trên các số nguyên.
