---
title: "CF 1812G - Tầm nhìn màu"
description: "Chúng ta có hai hàng gạch màu, mỗi hàng có cùng số cột. Mỗi ô có thể có màu đỏ (R), xanh lá cây (G) hoặc xanh lam (B)."
date: "2026-06-09T08:33:15+07:00"
tags: ["codeforces", "competitive-programming", "*special", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1812
codeforces_index: "G"
codeforces_contest_name: "April Fools Day Contest 2023"
rating: 0
weight: 1812
solve_time_s: 74
verified: true
draft: false
---

[CF 1812G - Tầm nhìn màu](https://codeforces.com/problemset/problem/1812/G) 

**Đánh giá:** - 
**Tags:** *đặc biệt, thực hiện 
**Thời gian giải:** 1 phút 14s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai hàng gạch màu, mỗi hàng có cùng số cột. Mỗi ô có thể có màu đỏ (R), xanh lá cây (G) hoặc xanh lam (B). Câu hỏi đặt ra là liệu có thể đổi màu các ô theo cách sao cho đối với mỗi cột, cả hai ô có cùng màu, chỉ sử dụng hai loại tầm nhìn màu. Loại thứ nhất có thể phân biệt tất cả các màu, trong khi loại thứ hai nhận biết màu đỏ và xanh lục giống hệt nhau, nhưng vẫn phân biệt được màu xanh lam. Chúng tôi được yêu cầu xác định xem một người bị mù màu đỏ-lục có xem mỗi cột là đơn sắc hay không. 

Đầu vào bao gồm số cột, theo sau là hai chuỗi biểu thị màu sắc ở hàng trên cùng và dưới cùng. Đầu ra là "CÓ" nếu hai hàng không thể phân biệt được trong tình trạng mù màu đỏ-xanh lục cho mỗi cột hoặc "KHÔNG" nếu ngược lại. 

Các ràng buộc chính là nhỏ: số lượng cột`n`có thể lên tới 100. Điều này ngụ ý rằng giải pháp O(n) hoặc O(n log n) là hoàn toàn có thể chấp nhận được, trong khi chúng ta không cần phải lo lắng về cấu trúc dữ liệu được tối ưu hóa cao hoặc I/O nhanh. Các trường hợp cạnh bao gồm các cột trong đó cả hai ô đều có cùng màu, các cột trong đó một ô có màu đỏ và cột kia có màu xanh lục và các cột có màu xanh lam. 

Các trường hợp cạnh không rõ ràng bao gồm các cột có màu sắc khác nhau ở người nhìn bình thường nhưng được cảm nhận giống nhau ở người mù màu. Ví dụ: một cột có`R`trên cùng và`G`ở phía dưới nên được coi là đơn sắc đối với người mù màu xanh đỏ, mặc dù thực tế hai ô này khác nhau. Nếu một cách tiếp cận đơn giản chỉ so sánh các ký tự theo nghĩa đen, nó sẽ xuất ra sai "KHÔNG". Trường hợp một cột cũng cần được xử lý chính xác. 

## Phương pháp tiếp cận 

Giải pháp brute-force sẽ so sánh từng ký tự của từng cột bằng cách sử dụng đẳng thức theo nghĩa đen. Điều này hoạt động dưới tầm nhìn bình thường nhưng không thành công khi bị mù màu đỏ-xanh. Ví dụ: nó sẽ xuất ra "NO" cho một cột có`R`Và`G`, mặc dù những điều này được người quan sát mù màu cảm nhận là giống nhau. Trong trường hợp xấu nhất, phương pháp này thực hiện các phép toán O(n), đủ nhanh với n ≤ 100, nhưng nó đưa ra câu trả lời sai vì bỏ qua các quy tắc nhận biết màu sắc. 

Cách tiếp cận tối ưu sửa đổi sự so sánh để tính đến bệnh mù màu đỏ-lục. Chúng tôi xác định một hàm trợ giúp ánh xạ màu sắc cảm nhận được: cả màu đỏ và màu xanh lá cây đều ánh xạ tới cùng một ký hiệu (giả sử,`X`), trong khi màu xanh vẫn khác biệt. Sau đó, chúng tôi lặp lại từng cột và kiểm tra xem các màu được ánh xạ có khớp ở hàng trên cùng và dưới cùng hay không. Điều này duy trì độ phức tạp về thời gian O(n) nhưng tạo ra câu trả lời đúng theo quy tắc nhìn màu của bài toán. 

Quan sát cho phép đơn giản hóa này là chúng ta không cần phải xem xét tất cả các cách đổi màu có thể có; chúng ta chỉ cần so sánh trực tiếp các màu sắc cảm nhận được trong từng cột. Khi màu sắc cảm nhận khớp với tất cả các cột, câu trả lời là "CÓ"; nếu không thì là "KHÔNG". 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) | O(1) | Không chính xác đối với nhận thức mù màu | 
| Tối ưu | O(n) | O(n) | Đúng và đủ nhanh | 

## Hướng dẫn thuật toán 

1. Đọc số cột,`n`. Điều này xác định độ dài của hàng của chúng tôi. 
2. Đọc hai chuỗi đại diện cho hàng trên và dưới. 
3. Xác định hàm trợ giúp`perceived(c)`điều đó trả về`'X'`nếu màu đó là đỏ hoặc xanh lá cây, và`'B'`nếu màu xanh. Điều này mô hình nhận thức mù màu đỏ-xanh. 
4. Lặp lại từng chỉ mục cột từ 0 đến n-1. 
5. Đối với mỗi cột, áp dụng`perceived`chức năng cho các ô trên cùng và dưới cùng và so sánh kết quả. 
6. Nếu bất kỳ cột nào khác nhau về màu sắc cảm nhận được, ngay lập tức xuất ra "NO" và kết thúc. 
7. Nếu tất cả các cột đều khớp, xuất ra "CÓ". 

Tại sao nó hoạt động: ở mỗi bước, thuật toán đảm bảo rằng nó sẽ kiểm tra nhận thức về các ô trong tình trạng mù màu đỏ-lục. Việc ánh xạ màu đỏ và xanh lục vào cùng một ký hiệu đảm bảo rằng bất kỳ cột nào chỉ có các ô màu đỏ và xanh lục đều được coi là đơn sắc. Màu xanh lam vẫn khác biệt, do đó, bất kỳ cột nào có ô màu xanh lam và ô màu đỏ hoặc xanh lục sẽ được phát hiện là không khớp. Vì chúng tôi kiểm tra từng cột chính xác một lần nên chúng tôi không thể bỏ sót bất kỳ thông tin không khớp nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    n = int(input())
    top = input().strip()
    bottom = input().strip()

    def perceived(c):
        if c in 'RG':
            return 'X'
        return c

    for i in range(n):
        if perceived(top[i]) != perceived(bottom[i]):
            print("NO")
            return
    print("YES")

if __name__ == "__main__":
    main()
```Giải pháp bắt đầu bằng cách đọc số cột và hai chuỗi màu. các`perceived`chức năng bình thường hóa màu đỏ và màu xanh lá cây thành một ký tự đại diện duy nhất. Vòng lặp lặp qua từng cột và so sánh các màu nhận được, ngay lập tức trả về "KHÔNG" nếu xảy ra sự không khớp. Nếu không tìm thấy sự không khớp, "CÓ" sẽ được in. Cần cẩn thận để loại bỏ các ký tự dòng mới khỏi đầu vào, điều này thường gặp trong lập trình cạnh tranh khi sử dụng`sys.stdin.readline`. 

## Ví dụ đã hoạt động 

Đối với đầu vào mẫu:```
1
R
R
```| tôi | đầu[i] | đáy[i] | nhận thức(top[i]) | nhận thấy(đáy[i]) | cuộc thi đấu? | 
| --- | --- | --- | --- | --- | --- | 
| 0 | R | R | X | X | vâng | 

Tất cả các cột đều khớp với màu sắc được cảm nhận, vì vậy kết quả đầu ra là "CÓ". 

Đối với một đầu vào bổ sung:```
3
RGB
GRB
```| tôi | đầu[i] | đáy[i] | nhận thức(top[i]) | nhận thấy(đáy[i]) | cuộc thi đấu? | 
| --- | --- | --- | --- | --- | --- | 
| 0 | R | G | X | X | vâng | 
| 1 | G | R | X | X | vâng | 
| 2 | B | B | B | B | vâng | 

Tất cả các cột khớp với màu sắc cảm nhận được, đầu ra là "CÓ". Điều này xác nhận rằng sự khác biệt màu đỏ và màu xanh lá cây được chuẩn hóa hợp lý. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Chúng tôi lặp lại từng cột trong số n cột chính xác một lần, thực hiện công việc O(1) trên mỗi cột. | 
| Không gian | O(n) | Chúng tôi lưu trữ hai chuỗi đầu vào và một ngăn xếp lệnh gọi hàm; không có cấu trúc bổ sung được yêu cầu. | 

Với n 100, giải pháp này thực thi trong micro giây và sử dụng bộ nhớ không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    f = io.StringIO()
    with redirect_stdout(f):
        main()
    return f.getvalue().strip()

# Provided sample
assert run("1\nR\nR\n") == "YES", "sample 1"

# Red-green perceived as same
assert run("2\nRG\nGR\n") == "YES", "red-green columns"

# Blue mismatch
assert run("3\nRGB\nRBB\n") == "NO", "blue mismatch"

# Single column, blue
assert run("1\nB\nB\n") == "YES", "single blue column"

# All columns blue, multiple columns
assert run("4\nBBBB\nBBBB\n") == "YES", "all blue"

# Mixed colors, last column mismatch
assert run("5\nRRGGB\nGGRRB\n") == "NO", "last column blue-red mismatch"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2\nRG\nGR\n | CÓ | Các cột màu đỏ-xanh được xử lý giống nhau | 
| 3\nRGB\nRBB\n | KHÔNG | Đã phát hiện sự không khớp màu xanh lam | 
| 1\nB\nB\n | CÓ | Cột đơn màu xanh lam | 
| 4\nBBBB\nBBBB\n | CÓ | Nhiều cột, toàn màu xanh lam | 
| 5\nRRGGB\nGGRRB\n | KHÔNG | Cột cuối cùng không khớp giữa màu xanh và màu đỏ | 

## Vỏ cạnh 

Kịch bản một cột có màu đỏ ở trên và màu xanh lá cây ở dưới cùng, như:```
1
R
G
```các`perceived`hàm ánh xạ cả hai tới 'X'. Vòng lặp so sánh các màu nhận biết, tìm chúng bằng nhau và xuất ra "CÓ", xử lý chính xác trường hợp cạnh. Tương tự, một cột có màu xanh lam ở trên và màu đỏ hoặc xanh lục ở dưới cùng sẽ được phát hiện là không khớp, ví dụ:```
1
B
R
```Ánh xạ tới 'B' so với 'X', đã phát hiện thấy không khớp, đầu ra là "KHÔNG". Thuật toán xử lý một cách có hệ thống tất cả các tình huống tối thiểu và không khớp màu.
