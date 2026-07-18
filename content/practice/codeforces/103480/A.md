---
title: "CF 103480A - \u89e3\u5f00\u675f\u7f1a\u7f20\u4e1d\u2161"
description: "Chúng tôi được cung cấp một số trường hợp thử nghiệm. Trong mỗi trường hợp, chúng tôi nhận được nhiều ký tự đơn. Mỗi ký tự chỉ được sử dụng tối đa một lần và chúng ta được phép sắp xếp lại các ký tự đã chọn theo thứ tự bất kỳ."
date: "2026-07-03T06:30:24+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103480
codeforces_index: "A"
codeforces_contest_name: "The 4th Hangzhou Normal University Freshman Programming Contest"
rating: 0
weight: 103480
solve_time_s: 41
verified: true
draft: false
---

[CF 103480A - \u89e3\u5f00\u675f\u7f1a\u7f20\u4e1d\u2161](https://codeforces.com/problemset/problem/103480/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 41s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số trường hợp thử nghiệm. Trong mỗi trường hợp, chúng tôi nhận được nhiều ký tự đơn. Mỗi ký tự chỉ được sử dụng tối đa một lần và chúng ta được phép sắp xếp lại các ký tự đã chọn theo thứ tự bất kỳ. Mục tiêu là chọn một số tập hợp con của các ký tự này và sắp xếp chúng thành một chuỗi là một bảng màu, tối đa hóa độ dài của bảng màu đó. 

Một bảng màu được xác định hoàn toàn bằng số lượng ký tự có thể được ghép đối xứng xung quanh tâm, cộng thêm có thể là một ký tự chưa ghép đôi được đặt ở giữa. Cấu trúc này buộc ràng buộc cốt lõi: các ký tự đóng góp vào một bảng màu chỉ thông qua các cặp, ngoại trừ nhiều nhất một ký tự còn sót lại có thể ngồi ở trung tâm. 

Kích thước đầu vào cho phép tối đa 10 trường hợp thử nghiệm, mỗi trường hợp có tối đa 100.000 ký tự. Điều này ngay lập tức loại trừ bất kỳ giải pháp nào cố gắng xây dựng các hoán vị hoặc thực hiện tìm kiếm tổ hợp. Việc quét tuyến tính cho mỗi trường hợp kiểm thử có thể được chấp nhận, nhưng bất kỳ số bậc hai nào về số lượng ký tự trên mỗi trường hợp kiểm thử sẽ vượt quá giới hạn. 

Trường hợp cạnh tinh tế xuất hiện khi tất cả các ký tự đều khác biệt. Ví dụ: nhập như`A B C D`chỉ cho phép một ký tự trong bảng màu vì không tồn tại cặp nào. Một trường hợp khác là khi một ký tự xuất hiện nhiều lần và các ký tự khác xuất hiện một lần chẳng hạn.`a a a b c`. Ở đây, palindrome tốt nhất sử dụng tất cả các cặp chẵn`a`và bỏ qua các ký tự đơn ngoại trừ một ký tự trung tâm. Một cách tiếp cận ngây thơ thay đổi một cách tham lam hoặc cố gắng “xây dựng ra bên ngoài” mà không tính đến tần suất sẽ tính sai những đóng góp này. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là xem xét tất cả các tập hợp con của các ký tự và tất cả các hoán vị của từng tập hợp con, kiểm tra xem mỗi cách sắp xếp có tạo thành một bảng màu hay không và theo dõi độ dài tối đa. Điều này đơn giản về mặt khái niệm vì nó khám phá toàn bộ không gian tìm kiếm của các công trình hợp lệ. Tuy nhiên, ngay cả việc chỉ chọn các tập hợp con cũng đã mang lại 2^n khả năng và các hoán vị sẽ nhân con số này với mức tăng trưởng giai thừa. Với n lên tới 100.000, cách tiếp cận này là không khả thi. 

Cấu trúc của một palindrome làm đơn giản hóa vấn đề một cách đáng kể. Một bảng màu được xác định hoàn toàn bởi tần số ký tự chứ không phải thứ tự. Mỗi nhân vật đóng góp càng nhiều cặp phản chiếu càng tốt. Mỗi cặp đóng góp hai ký tự cho câu trả lời cuối cùng. Sau khi hình thành tất cả các cặp có thể, nếu ít nhất một ký tự vẫn chưa được sử dụng, chúng ta có thể đặt chính xác một ký tự đó vào giữa, tăng độ dài lên một. 

Điều này làm giảm vấn đề từ sắp xếp tổ hợp đến đếm tần số. Quan sát quan trọng là cách xây dựng tối ưu không bao giờ phụ thuộc vào vị trí cụ thể nào được chọn, mà chỉ phụ thuộc vào số lượng cặp có thể được tạo thành trên tất cả các chữ cái. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Đếm tần số | O(n) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đếm số lần mỗi ký tự xuất hiện trong đầu vào. Điều này nắm bắt tất cả các cấu trúc có liên quan vì sự sắp xếp không liên quan một khi đã biết tần số. 
2. Với mỗi tần số ký tự, hãy tính xem có thể tạo được bao nhiêu cặp đầy đủ bằng cách lấy số nguyên chia cho 2. Mỗi cặp đóng góp chính xác hai ký tự vào bảng màu. 
3. Tổng tất cả các phần đóng góp của cặp này nhân với 2. Điều này cho ra tổng chiều dài đóng góp bởi các cạnh đối xứng của bảng màu. 
4. Theo dõi xem có tồn tại ít nhất một ký tự có số lẻ còn lại sau khi ghép nối hay không. Điều này chỉ ra rằng ít nhất một ký tự có thể đóng vai trò trung tâm. 
5. Nếu có số dư lẻ, hãy thêm một số vào câu trả lời để đặt ký tự trung tâm. 
6. Xuất chiều dài tính toán cuối cùng. 

Tại sao nó hoạt động: việc xây dựng một bảng màu đảm bảo tính đối xứng, vì vậy mọi ký tự được sử dụng ở phía bên trái phải có một ký tự tương ứng ở phía bên phải. Điều này phân vùng việc sử dụng thành các cặp rời rạc. Bất kỳ ký tự còn sót lại nào cũng không thể được đặt đối xứng ngoại trừ một ký tự sẽ trở thành trung tâm. Vì việc ghép nối là độc lập trên mỗi ký tự và được tối đa hóa theo tần số nên kết quả là cấu trúc tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        freq = {}
        
        arr = input().split()
        for c in arr:
            freq[c] = freq.get(c, 0) + 1
        
        length = 0
        has_odd = False
        
        for v in freq.values():
            length += (v // 2) * 2
            if v % 2 == 1:
                has_odd = True
        
        if has_odd:
            length += 1
        
        print(length)

if __name__ == "__main__":
    solve()
```Giải pháp được cấu trúc xung quanh việc tích lũy tần số truyền đơn. Các cửa hàng từ điển đếm cho mỗi ký tự, điều này là đủ vì các ký tự độc lập. 

Tính toán chính là`(v // 2) * 2`, chuyển đổi tần số thành đóng góp đối xứng có thể sử dụng được. Nhân với 2 một cách rõ ràng sẽ tránh làm mất đi sự rõ ràng về ghép nối trái-phải. 

Boolean`has_odd`rất quan trọng vì nó mã hóa liệu chúng ta có thể đặt ký tự trung tâm hay không. Bất kỳ sự xuất hiện lẻ nào trên tất cả các ký tự đều đủ vì chỉ tồn tại một vị trí ở giữa. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
3
a v a
```Chúng tôi theo dõi tần số: 

| Nhân vật | Tần số | Cặp được sử dụng | Đóng góp | Lẻ còn sót lại | 
| --- | --- | --- | --- | --- | 
| một | 2 | 1 | 2 | không | 
| v | 1 | 0 | 0 | vâng | 

Tổng chiều dài đối xứng là 2. Vì có phần dư thừa (`v`), ta thêm 1 ký tự ở giữa. 

Câu trả lời cuối cùng là 3. 

Điều này xác nhận rằng nhiều nhân vật khác nhau vẫn có thể đóng góp chính xác, ngay cả khi chỉ một trong số họ trở thành trung tâm. 

### Ví dụ 2 

đầu vào:```
1
4
A a b c
```Tần số đều là 1. 

| Nhân vật | Tần số | Cặp được sử dụng | Đóng góp | Lẻ còn sót lại | 
| --- | --- | --- | --- | --- | 
| A | 1 | 0 | 0 | vâng | 
| một | 1 | 0 | 0 | vâng | 
| b | 1 | 0 | 0 | vâng | 
| c | 1 | 0 | 0 | vâng | 

Độ dài đối xứng là 0. Có ít nhất một số lẻ tồn tại, vì vậy chúng ta đặt một ký tự ở giữa. 

Câu trả lời cuối cùng là 1. 

Điều này thể hiện trường hợp cực đoan khi không thể ghép nối và bảng màu bị thoái hóa thành một ký tự đơn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi ký tự được tính một lần và mỗi tần số được xử lý một lần | 
| Không gian | O(k) | k là số ký tự riêng biệt, giới hạn bởi kích thước bảng chữ cái | 

Các ràng buộc cho phép tối đa 100.000 ký tự cho mỗi trường hợp thử nghiệm, do đó, quá trình quét tuyến tính sẽ phù hợp thoải mái trong giới hạn thời gian. Việc sử dụng bộ nhớ là tối thiểu vì chỉ lưu trữ bản đồ tần số trên các ký tự. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdin
    input = stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        arr = input().split()
        freq = {}
        for c in arr:
            freq[c] = freq.get(c, 0) + 1

        length = 0
        has_odd = False
        for v in freq.values():
            length += (v // 2) * 2
            if v % 2:
                has_odd = True
        if has_odd:
            length += 1

        out.append(str(length))

    return "\n".join(out)

# provided samples (interpreted)
assert run("2\n1\nA\n3\na v a\n") == "1\n3"

# all same character
assert run("1\n5\na a a a a\n") == "5"

# no pairs at all
assert run("1\n4\na b c d\n") == "1"

# mixed frequencies
assert run("1\n6\na a b b b c\n") == "5"

# maximum-like balanced
assert run("1\n8\na a b b c c d d\n") == "8"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả các chữ cái giống hệt nhau | chiều dài đầy đủ | trường hợp ghép nối tối đa | 
| tất cả các chữ cái riêng biệt | 1 | trung tâm duy nhất có thể | 
| tần số hỗn hợp | tập hợp cặp đúng | phân bố không đồng đều | 
| cặp cân bằng hoàn hảo | sử dụng đầy đủ | không có thức ăn thừa | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi không có ký tự lặp lại. Đối với đầu vào như`a b c d`, bản đồ tần số tạo ra tất cả những cái một. Thuật toán tính toán số 0 từ các đóng góp cặp và phát hiện ít nhất một giá trị lẻ, do đó, nó xuất ra chính xác 1. 

Một trường hợp khác là khi có chính xác một ký tự có tần số lớn và các ký tự khác là ký tự đơn, chẳng hạn như`a a a a b c d`. Thuật toán tạo thành hai cặp từ`a`, đóng góp 4 và sử dụng một trong các đơn vị làm trung tâm, cho kết quả 5. Các đơn vị còn lại không ảnh hưởng đến kết quả vì chúng không thể tạo thành các cặp bổ sung. 

Trường hợp cạnh cuối cùng là đầu vào một phần tử. Với đầu vào`A`, tổng cặp bằng 0 và tồn tại một số lẻ, do đó câu trả lời trở thành 1, khớp với bảng màu duy nhất có thể có.
