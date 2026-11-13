---
title: "CF 172A - Mã Điện Thoại"
description: "Chúng tôi được cung cấp một danh sách số điện thoại từ bạn bè trong thành phố. Mỗi số điện thoại là một chuỗi chữ số và tất cả các số đều có cùng độ dài. Nhiệm vụ là tìm mã điện thoại thành phố mà Polycarpus định nghĩa là tiền tố chung dài nhất được chia sẻ bởi tất cả các số này."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "*special", "brute-force", "implementation"]
categories: ["algorithms"]
codeforces_contest: 172
codeforces_index: "A"
codeforces_contest_name: "Croc Champ 2012 - Qualification Round"
rating: 800
weight: 172
solve_time_s: 180
verified: true
draft: false
---

[CF 172A - Mã điện thoại](https://codeforces.com/problemset/problem/172/A) 

**Đánh giá:** 800 
**Tags:** *đặc biệt, vũ phu, thực hiện 
**Thời gian giải:** 3 phút 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một danh sách số điện thoại từ bạn bè trong thành phố. Mỗi số điện thoại là một chuỗi chữ số và tất cả các số đều có cùng độ dài. Nhiệm vụ là tìm mã điện thoại thành phố mà Polycarpus định nghĩa là tiền tố chung dài nhất được chia sẻ bởi tất cả các số này. Nói cách khác, mã thành phố là chuỗi các chữ số ở đầu mỗi số trước khi xảy ra sự không khớp đầu tiên. Đầu ra là độ dài của tiền tố này chứ không phải chính tiền tố đó. 

Các ràng buộc cho chúng ta biết có thể có tới 30.000 số, mỗi số dài tối đa 20 chữ số. Điều này có nghĩa là bất kỳ giải pháp nào so sánh từng ký tự số theo ký tự đều khả thi vì tổng số so sánh tối đa là 30.000 lần 20, tức là 600.000 phép tính - thoải mái trong giới hạn 2 giây. Việc đảm bảo rằng tất cả các số đều có cùng độ dài và khác biệt sẽ loại bỏ sự mơ hồ, nhưng chúng ta cần xử lý khả năng các số này không có tiền tố chung nào cả. Ví dụ, nếu các số là`123`Và`987`, đầu ra đúng là 0. Một cách tiếp cận ngây thơ giả định ký tự đầu tiên luôn khớp có thể âm thầm tạo ra câu trả lời sai. 

Một trường hợp cạnh khác là khi tất cả các số đều giống hệt nhau ngoại trừ ký tự cuối cùng. Ví dụ,`5550`,`5551`,`5552`. Tiền tố chung dài nhất là`555`, có độ dài 3. Việc triển khai bất cẩn có thể đếm thừa hoặc đếm thiếu nếu không kiểm tra chính xác sự bằng nhau của ký tự trên tất cả các số. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản nhất là vũ lực. Lấy số đầu tiên làm tham chiếu, sau đó lặp lại từng chỉ mục ký tự và so sánh ký tự đó với ký tự tương ứng trong tất cả các số khác. Dừng lại ở sự không phù hợp đầu tiên. Cách tiếp cận này hiệu quả vì chúng tôi chỉ so sánh các ký tự cho đến khi tìm thấy sự không khớp. Trong trường hợp xấu nhất, mọi ký tự của mọi số đều khớp nhau, vì vậy chúng ta thực hiện`n * length`so sánh, có thể quản lý được với các ràng buộc. 

Một cách có cấu trúc hơn một chút là sử dụng phép giảm theo cặp. Bắt đầu với số đầu tiên làm tiền tố hiện tại, sau đó lặp lại rút ngắn nó bằng cách so sánh nó với từng số tiếp theo cho đến khi nó khớp với phần đầu của số đó. Kỹ thuật này tránh việc kiểm tra nhiều lần các ký tự mà chúng ta đã biết khác nhau ở các vị trí trước đó, mặc dù đối với những độ dài nhỏ này thì hiệu suất đạt được là tối thiểu. 

Thông tin chi tiết quan trọng là vì các chuỗi ngắn (tối đa 20 chữ số) nên chúng ta có thể so sánh từng ký tự một cách an toàn trên tất cả các số mà không phải lo lắng về hiệu quả. Cấu trúc của bài toán - các chuỗi có độ dài cố định, nội dung chữ số được đảm bảo và độ dài tối đa nhỏ - làm cho cách tiếp cận bạo lực về cơ bản là tối ưu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (so sánh từng ký tự trên tất cả các chuỗi) | O(n * m), m = độ dài chuỗi | O(1) | Đã chấp nhận | 
| Giảm tiền tố lặp đi lặp lại | O(n * m) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng bạn bè`n`và danh sách số điện thoại. Tất cả các số có độ dài bằng nhau`m`. 
2. Lấy số điện thoại đầu tiên làm tiền tố tham chiếu ban đầu. 
3. Khởi tạo một biến`prefix_length`đến tổng chiều dài`m`. Điều này đại diện cho tiền tố tối đa có thể. 
4. Lặp lại các số điện thoại còn lại. 
5. Với mỗi số, lặp lại từng ký tự từ đầu đến hiện tại`prefix_length`. 
6. So sánh các ký tự ở cùng vị trí với số tham chiếu. Nếu xảy ra sự không phù hợp, hãy giảm`prefix_length`đến chỉ số không khớp và thoát ra khỏi vòng lặp bên trong. 
7. Tiếp tục đến số tiếp theo, giảm tiếp`prefix_length`nếu cần thiết. 
8. Sau khi xử lý tất cả các số,`prefix_length`giữ độ dài của tiền tố chung dài nhất. In nó. 

Điều bất biến là sau khi xử lý từng số,`prefix_length`luôn phản ánh tiền tố dài nhất được chia sẻ bởi tất cả các số được xử lý cho đến nay. Không có nhân vật nào ngoài`prefix_length`có thể là một phần của tiền tố chung hợp lệ, đảm bảo tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
numbers = [input().strip() for _ in range(n)]

prefix_length = len(numbers[0])

for num in numbers[1:]:
    for i in range(prefix_length):
        if num[i] != numbers[0][i]:
            prefix_length = i
            break

print(prefix_length)
```Trước tiên, chúng tôi đọc tất cả các số đầu vào và loại bỏ các ký tự dòng mới. Chúng tôi giả sử số đầu tiên có tiền tố tối đa có thể. Đối với mỗi số tiếp theo, chúng tôi chỉ kiểm tra tối đa`prefix_length`bởi vì bất kỳ ký tự nào ngoài nó đều không thể là một phần của tiền tố chung. Vòng lặp sẽ ngắt ngay lập tức khi không khớp, ngăn chặn những so sánh không cần thiết. Cuối cùng, chúng tôi in độ dài của tiền tố, đó chính xác là những gì vấn đề yêu cầu. 

## Ví dụ đã hoạt động 

### Đầu vào mẫu 1```
4
00209
00219
00999
00909
```| số | tôi | số[0][i] | số[i] | cập nhật prefix_length | 
| --- | --- | --- | --- | --- | 
| 00219 | 0 | 0 | 0 | 5 | 
| 00219 | 1 | 0 | 0 | 5 | 
| 00219 | 2 | 2 | 2 | 5 | 
| 00219 | 3 | 0 | 1 | 3 | 
| 00999 | 0 | 0 | 0 | 3 | 
| 00999 | 1 | 0 | 0 | 3 | 
| 00999 | 2 | 2 | 9 | 2 | 
| 00909 | 0 | 0 | 0 | 2 | 
| 00909 | 1 | 0 | 0 | 2 | 

Sau tất cả những con số`prefix_length`= 2, phù hợp với kết quả đầu ra dự kiến. 

### Đầu vào mẫu 2```
2
123
987
```| số | tôi | số[0][i] | số[i] | cập nhật prefix_length | 
| --- | --- | --- | --- | --- | 
| 987 | 0 | 1 | 9 | 0 | 

Không có ký tự nào trùng khớp, vì vậy`prefix_length`= 0. 

Những dấu vết này xác nhận rằng thuật toán dừng chính xác ở lần không khớp đầu tiên và duy trì độ dài tiền tố chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n * m) | Chúng tôi so sánh từng ký tự của mỗi số với độ dài tiền tố hiện tại; n lên tới 30.000, m lên tới 20, cho tối đa 600.000 so sánh. | 
| Không gian | O(n * m) | Chúng tôi lưu trữ tất cả các chuỗi đầu vào. Không có không gian đáng kể bổ sung được sử dụng. | 

Tổng số thao tác đều nằm trong giới hạn thời gian 2 giây và mức sử dụng bộ nhớ nằm trong khoảng 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    numbers = [input().strip() for _ in range(n)]
    prefix_length = len(numbers[0])
    for num in numbers[1:]:
        for i in range(prefix_length):
            if num[i] != numbers[0][i]:
                prefix_length = i
                break
    return str(prefix_length)

# Provided samples
assert run("4\n00209\n00219\n00999\n00909\n") == "2", "sample 1"
assert run("2\n123\n987\n") == "0", "sample 2"

# Custom cases
assert run("3\n5550\n5551\n5552\n") == "3", "common prefix just before last digit"
assert run("2\n1\n1\n") == "1", "single-digit identical numbers"
assert run("3\n12345678901234567890\n12345678901234567891\n12345678901234567892\n") == "19", "max length numbers differing at last digit"
assert run("2\n99999\n99999\n") == "5", "all numbers identical"
assert run("2\n0000\n1111\n") == "0", "no common prefix at all"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 5550, 5551, 5552 | 3 | Xử lý chính xác chữ số cuối cùng không khớp | 
| 1, 1 | 1 | Trường hợp cạnh một chữ số | 
| số dài khác nhau ở chữ số cuối | 19 | Xử lý ranh giới đầu vào có độ dài tối đa | 
| số giống nhau | 5 | Toàn bộ chuỗi khớp | 
| 0000, 1111 | 0 | Không có tiền tố chung | 

## Vỏ cạnh 

Nếu tất cả các số đều giống hệt nhau ngoại trừ ký tự cuối cùng, thì thuật toán sẽ dừng so sánh một cách chính xác ở điểm không khớp đầu tiên đối với mỗi số. Ví dụ,`5550`,`5551`,`5552`kết quả là`prefix_length`= 3. Đối với các số hoàn toàn không trùng nhau như`123`Và`987`, thuật toán phát hiện sự không khớp ngay ở ký tự đầu tiên và đặt`prefix_length`= 0. Đối với các số có một chữ số, thuật toán xử lý cả trường hợp khớp và không khớp một cách tự nhiên, trả về 1 hoặc 0. Tất cả các trường hợp cạnh đều được xử lý chính xác bằng vòng lặp so sánh từng ký tự với ngắt sớm do không khớp.
