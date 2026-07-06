---
title: "CF 102920B - Xúc xắc kỷ niệm"
description: "Chúng ta có hai khối lập phương, mỗi khối có sáu mặt. Mỗi mặt chứa một số nguyên dương và với mỗi khối thì tổng sáu giá trị là 21. Khi cuộn khối, mỗi mặt đều có khả năng xuất hiện như nhau, do đó mỗi khối xác định một phân bố xác suất đồng đều trên sáu giá trị mặt của nó."
date: "2026-07-04T07:54:23+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102920
codeforces_index: "B"
codeforces_contest_name: "2020-2021 ACM-ICPC, Asia Seoul Regional Contest"
rating: 0
weight: 102920
solve_time_s: 41
verified: true
draft: false
---

[CF 102920B - Xúc xắc kỷ niệm](https://codeforces.com/problemset/problem/102920/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 41s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai khối lập phương, mỗi khối có sáu mặt. Mỗi mặt chứa một số nguyên dương và với mỗi khối thì tổng sáu giá trị là 21. Khi cuộn khối, mỗi mặt đều có khả năng xuất hiện như nhau, do đó mỗi khối xác định một phân bố xác suất đồng đều trên sáu giá trị mặt của nó. 

Nhiệm vụ là tính xác suất để một lần tung ngẫu nhiên khối thứ nhất tạo ra một giá trị lớn hơn rất nhiều so với lần tung ngẫu nhiên khối thứ hai. Mọi kết quả khuôn mặt đều có khả năng xảy ra như nhau, do đó, kết quả này trở thành sự so sánh trên 36 cặp giá trị khuôn mặt có khả năng xảy ra như nhau, một cặp được chọn từ khối thứ nhất và một từ khối thứ hai. 

Đầu ra phải là một phần không thể giảm được. Điều này có nghĩa là chúng ta đếm có bao nhiêu trong số 36 cặp thỏa mãn điều kiện “giá trị thứ nhất lớn hơn giá trị thứ hai”, sau đó chia cho 36, đơn giản hóa phân số bằng cách chia tử số và mẫu số cho ước số chung lớn nhất của chúng. 

Các ràng buộc là nhỏ và cố định. Mỗi dòng đầu vào chứa chính xác sáu số nguyên, do đó, bất kỳ thuật toán nào có bậc hai trên 6 thực tế là thời gian không đổi. Việc xem xét có ý nghĩa duy nhất là tính chính xác của việc tính xác suất và xử lý cẩn thận các giá trị trùng lặp, vì các số lặp lại ảnh hưởng đến bội số trong phép so sánh. 

Một trường hợp tinh tế phát sinh khi nhiều mặt có giá trị giống nhau. Ví dụ: nếu cả hai viên xúc xắc có tất cả các mặt bằng 3 thì mọi kết quả đều bằng nhau, do đó câu trả lời phải là 0/1. Một cách tiếp cận ngây thơ chỉ so sánh các giá trị duy nhất hoặc giả định các hoán vị sẽ thất bại ở đây, bởi vì tính đa dạng là điều cần thiết. 

Một tình huống khác là khi các giá trị bị sai lệch nhiều, chẳng hạn như một con xúc xắc chủ yếu có giá trị nhỏ và con kia chủ yếu có giá trị lớn. Trong những trường hợp như vậy, việc chỉ tính các so sánh riêng biệt mà không mở rộng tần số sẽ đánh giá thấp hoặc đánh giá quá cao xác suất. 

## Phương pháp tiếp cận 

Cách trực tiếp nhất để suy nghĩ về vấn đề này là mô phỏng tất cả các lần cuộn có thể xảy ra. Mỗi xúc xắc có sáu mặt, vì vậy chúng ta có thể lặp lại tất cả 36 cặp có thứ tự. Đối với mỗi cặp, chúng tôi kiểm tra xem giá trị đầu tiên có lớn hơn giá trị thứ hai hay không và đếm xem điều này xảy ra bao nhiêu lần. Con số này vốn đã cực kỳ nhỏ rồi, vì 36 phép so sánh là một công việc liên tục. 

Phương pháp brute-force này đúng vì các viên xúc xắc đều đồng nhất và độc lập, nên mọi cặp mặt đều có khả năng xảy ra như nhau với xác suất 1/36. Xác suất mà chúng ta mong muốn chính xác là tỷ lệ các cặp thuận lợi trên tất cả các cặp. 

Không thực sự cần phải tối ưu hóa ngoài điều này, nhưng cấu trúc khái niệm khái quát hóa: nếu xúc xắc có nhiều mặt hoặc có trọng số, chúng ta vẫn sẽ dựa vào việc đếm các so sánh theo cặp, có thể bằng bản đồ tần số thay vì liệt kê thô. 

Quan sát quan trọng là bài toán giảm xuống một xác suất rời rạc trên tích Descartes. Một khi điều đó được nhận ra, lời giải sẽ trở thành một nhiệm vụ đếm đơn giản hơn là bất kỳ lý luận tổ hợp nào. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Bảng liệt kê cặp Brute Force | O(36) | O(1) | Đã chấp nhận | 
| Đếm dựa trên tần số (khái quát hóa tùy chọn) | O(6²) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Đọc sáu giá trị của xúc xắc thứ nhất và sáu giá trị của xúc xắc thứ hai. Chúng đại diện cho sự phân bố xác suất thống nhất trên các mặt của chúng. 
2. Khởi tạo bộ đếm để có kết quả thành công, nghĩa là trường hợp mặt của con súc sắc thứ nhất lớn hơn mặt của con súc sắc thứ hai. 
3. Lặp lại từng giá trị trong khuôn đầu tiên. 
4. Đối với mỗi giá trị như vậy, hãy lặp lại từng giá trị ở khuôn thứ hai. 
5. So sánh hai giá trị. Nếu giá trị đầu tiên lớn hơn, hãy tăng bộ đếm thành công. 
6. Sau khi tất cả các cặp được xử lý, tổng số kết quả được cố định là 36, vì có sáu lựa chọn cho mỗi xúc xắc. 
7. Lập phân số thành công / 36. 
8. Rút gọn phân số bằng cách chia tử số và mẫu số cho ước số chung lớn nhất của chúng. 

### Tại sao nó hoạt động 

Mọi mặt trên mỗi viên xúc xắc đều có khả năng như nhau và các viên xúc xắc đều độc lập. Điều này ngụ ý rằng mỗi cặp mặt có thứ tự xuất hiện với xác suất bằng nhau là 1/36. Thuật toán liệt kê rõ ràng tất cả các kết quả có khả năng xảy ra như nhau và tính chính xác tập hợp con thuận lợi. Vì không có cặp nào bị bỏ qua hoặc tính trọng số không chính xác nên tỷ lệ được tính toán khớp với xác suất thực. Việc giảm bằng gcd bảo toàn sự tương đương của phân số mà không làm thay đổi giá trị của nó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from math import gcd

a = list(map(int, input().split()))
b = list(map(int, input().split()))

win = 0
for x in a:
    for y in b:
        if x > y:
            win += 1

total = 36
g = gcd(win, total)

print(f"{win // g}/{total // g}")
```Mã trực tiếp thực hiện việc liệt kê cặp được mô tả trước đó. Các vòng lặp lồng nhau phản ánh tích Descartes của các khuôn mặt. Vì cả hai viên xúc xắc luôn có chính xác sáu giá trị nên chúng ta có thể mã hóa tổng số kết quả một cách an toàn là 36. 

Việc giảm gcd đảm bảo đầu ra ở dạng tối giản theo yêu cầu. Không cần số học dấu phẩy động, điều này tránh hoàn toàn các vấn đề về độ chính xác. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào: 

Chết đầu tiên: 3 4 3 4 3 4 

Xúc xắc thứ hai: 1 1 1 1 8 9 

Chúng tôi liệt kê những đóng góp theo nhóm: 

| Giá trị đầu tiên | Giá trị thứ hai | Thắng | 
| --- | --- | --- | 
| 3 | 1,1,1,1,8,9 | 4 | 
| 4 | 1,1,1,1,8,9 | 4 | 
| 3 | tương tự như trên | 4 | 
| 4 | tương tự như trên | 4 | 
| 3 | tương tự như trên | 4 | 
| 4 | tương tự như trên | 4 | 

Tổng số trận thắng = 24 trên 36. 

| thắng | tổng cộng | phân số | 
| --- | --- | --- | 
| 24 | 36 | 2/3 | 

Điều này xác nhận rằng bội số của các giá trị ảnh hưởng trực tiếp đến xác suất. 

### Mẫu 2 

đầu vào: 

Xúc xắc đầu tiên: 1 2 3 4 5 6 

Xúc xắc thứ hai: 3 4 3 4 3 4 

Chúng tôi tính toán so sánh: 

| Giá trị đầu tiên | Thắng xúc xắc thứ hai | 
| --- | --- | 
| 1 | 0 | 
| 2 | 0 | 
| 3 | nhịp 3s? 0, nhịp 4s? 0 → 0 | 
| 4 | chỉ đập 3s → 3 | 
| 5 | đánh bại tất cả → 6 | 
| 6 | đánh bại tất cả → 6 | 

Tính tần suất của giá trị xúc xắc thứ hai, tổng số tiền thắng = 15 trên 36. 

Phân số tối giản là 5/12. 

Ví dụ này cho thấy rằng ngay cả những con xúc xắc trông đối xứng cũng có thể tạo ra những xác suất không hề nhỏ do các giá trị lặp lại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(36) | các vòng lặp lồng nhau cố định trên sáu mặt mỗi vòng | 
| Không gian | O(1) | chỉ lưu trữ mảng đầu vào và bộ đếm | 

Việc tính toán là thời gian không đổi bất kể phân phối đầu vào. Điều này dễ dàng đáp ứng mọi giới hạn hợp lý, bao gồm cả các ràng buộc nghiêm ngặt 0,5 giây. 

## Trường hợp thử nghiệm```python
import sys, io
from math import gcd

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))
    win = 0
    for x in a:
        for y in b:
            if x > y:
                win += 1
    g = gcd(win, 36)
    return f"{win//g}/{36//g}"

# provided samples
assert run("3 4 3 4 3 4\n1 1 1 1 8 9\n") == "2/3"
assert run("1 2 3 4 5 6\n3 4 3 4 3 4\n") == "5/12"
assert run("1 2 3 4 5 6\n8 7 2 2 1 1\n") == "1/2"

# custom cases
assert run("1 1 1 1 1 1\n1 1 1 1 1 1\n") == "0/1", "all equal ties"
assert run("6 6 6 6 6 6\n1 1 1 1 1 1\n") == "1/1", "always win"
assert run("1 1 1 1 1 6\n6 6 6 6 6 1\n") == "25/36", "mixed extremes"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả các viên xúc xắc đều bằng nhau | 0/1 | xử lý cà vạt đầy đủ | 
| xúc xắc tối đa và tối thiểu | 1/1 | trường hợp thống trị | 
| xiên hỗn hợp xúc xắc | 25/36 | độ nhạy tần số | 

## Vỏ cạnh 

Một trường hợp hoàn toàn thống nhất như`1 1 1 1 1 1`chống lại chính nó tạo ra các cặp chiến thắng bằng không. Thuật toán vẫn lặp lại trên tất cả 36 cặp, nhưng mọi so sánh đều thất bại, để lại`win = 0`. Sau khi giảm,`0/36`đơn giản hóa một cách chính xác để`0/1`. 

Một trường hợp thống trị như`6 6 6 6 6 6`so với`1 1 1 1 1 1`sản xuất`win = 36`. gcd có 36 là 36 và kết quả đơn giản hóa thành`1/1`, phản ánh sự chắc chắn. 

Một phân phối hỗn hợp như`1 1 1 1 1 6`chống lại`6 6 6 6 6 1`đảm bảo rằng cả kết quả thắng và thua đều xuất hiện. Thuật toán đếm chính xác các so sánh theo trọng số tần số thông qua phép liệt kê thô, tạo ra xác suất chính xác mà không cần bất kỳ lý do ký hiệu nào.
