---
title: "CF 106103H - Hình vuông"
description: "Chúng ta được cung cấp nhiều trường hợp thử nghiệm và mỗi trường hợp thử nghiệm bao gồm bốn thanh dài. Mỗi cây gậy đều cứng: chúng ta không thể cắt, kéo dài hay uốn cong nó. Nhiệm vụ là xác định xem bốn cây gậy này có thể được sắp xếp để tạo thành một hình vuông hay không."
date: "2026-06-25T11:44:36+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106103
codeforces_index: "H"
codeforces_contest_name: "AGM 2025, Final Round, Day 2"
rating: 0
weight: 106103
solve_time_s: 36
verified: true
draft: false
---

[CF 106103H - Hình vuông](https://codeforces.com/problemset/problem/106103/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 36s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp nhiều trường hợp thử nghiệm và mỗi trường hợp thử nghiệm bao gồm bốn thanh dài. Mỗi cây gậy đều cứng: chúng ta không thể cắt, kéo dài hay uốn cong nó. Nhiệm vụ là xác định xem bốn cây gậy này có thể được sắp xếp để tạo thành một hình vuông hay không. 

Hình vuông ở đây có nghĩa là một vòng khép kín có bốn cạnh bằng nhau và các góc vuông. Vì chúng ta chỉ được phép sử dụng các que như đoạn thẳng nên quyền tự do duy nhất mà chúng ta có là gán mỗi que cho một cạnh của hình vuông. 

Bước mô hình hóa quan trọng là chuyển yêu cầu hình học thành một ràng buộc về độ dài thanh. Hình vuông có bốn cạnh, tất cả đều bằng nhau. Bởi vì mỗi thanh là một đoạn cứng duy nhất nên mỗi cạnh của hình vuông phải tương ứng với chính xác một thanh. Không có thao tác nào cho phép chúng ta chia hoặc ghép các que thành các đoạn nhỏ hơn nên cấu trúc của hình dạng cuối cùng hoàn toàn được xác định bởi sự bằng nhau của bốn giá trị đầu vào. 

Kích thước đầu vào cho mỗi trường hợp thử nghiệm nhỏ nhưng có thể có tới 10^4 trường hợp thử nghiệm. Điều đó có nghĩa là bất kỳ giải pháp nào thực hiện công việc liên tục cho mỗi trường hợp kiểm thử đều ổn, trong khi mọi giải pháp liên quan đến việc sắp xếp các cấu trúc lớn hoặc các vòng lặp lồng nhau trong các trường hợp kiểm thử vẫn ổn nhưng không cần thiết. Các ràng buộc đẩy chúng ta tới việc kiểm tra trực tiếp theo thời gian liên tục. 

Một trường hợp phức tạp phát sinh khi một người cố gắng suy nghĩ quá nhiều về việc nhóm. Ví dụ, người ta có thể tưởng tượng việc kết hợp các que như 1, 2, 1, 2 để tạo thành bốn cạnh có chiều dài 2 hoặc 3. Điều này không hợp lệ vì các que không thể bị gãy hoặc nối lại. Một sai lầm khác là giả sử chúng ta chỉ cần tổng số chia hết cho 4, điều này đúng trong bài toán cắt linh hoạt nhưng không đúng ở đây. 

Trực giác thất bại cụ thể: 

đầu vào:```
1
1 2 1 2
```Một số người có thể đoán chúng ta có thể tạo thành một hình vuông có cạnh 1,5 hoặc việc ghép đôi đó sẽ giúp ích, nhưng chúng ta không thể tách các que, vì vậy không có cấu hình nào tạo ra bốn cạnh bằng nhau. Đầu ra đúng là`NO`. 

đầu vào:```
1
5 5 5 5
```Tất cả các que đều đã khớp nhau nên chúng ta có thể trực tiếp gán từng que cho một bên. Đầu ra là`YES`. 

## Phương pháp tiếp cận 

Một cách giải thích bạo lực sẽ cố gắng gán các que vào bốn cạnh và xác minh tính khả thi về mặt hình học. Vì chúng ta chỉ có bốn que, nên người ta có thể thử tất cả các hoán vị của bài tập và thậm chí xem xét các quyết định nhóm nếu được phép chia tách. Điều đó nhanh chóng trở nên phức tạp không cần thiết vì cấu trúc của hình dạng mục tiêu cực kỳ cứng nhắc: mỗi mặt phải khớp chính xác với một cây gậy duy nhất. Ngay cả một bước xây dựng hình học đơn giản vẫn sẽ thất bại trong việc kiểm tra sự bằng nhau của độ dài các cạnh sau khi nhận ra rằng không có cách lắp ráp thay thế nào là hợp pháp. 

Quan sát quan trọng là ràng buộc bình phương loại bỏ tất cả các bậc tự do. Khi chúng ta chấp nhận rằng mỗi bên phải có chính xác một cây gậy, vấn đề sẽ giảm xuống còn việc xác minh xem tất cả bốn giá trị có giống nhau hay không. Bất kỳ sai lệch nào đều phá vỡ điều kiện bình đẳng ít nhất một bên, khiến việc xây dựng không thể thực hiện được. 

Vì vậy, phần “hình học” biến mất hoàn toàn và phần còn lại là một bài kiểm tra đẳng thức đơn giản trên bốn số nguyên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Sắp xếp Brute Force | O(1) (hoán vị không đổi) | O(1) | Quá mức cần thiết | 
| Kiểm tra tất cả bằng nhau | O(1) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc chiều dài bốn thanh cho một ca kiểm thử. Chúng đại diện cho các phân đoạn có sẵn duy nhất có thể tạo thành hình vuông. 
2. So sánh tất cả bốn giá trị với thanh đầu tiên. Lý do là nếu có thể có một hình vuông thì mọi cạnh phải giống nhau, vì vậy tất cả phải khớp với một giá trị tham chiếu duy nhất. 
3. Nếu bất kỳ cây gậy nào khác với cây gậy đầu tiên, hãy kết luận ngay rằng việc tạo thành một hình vuông là không thể. 
4. Nếu cả bốn giá trị đều khớp nhau, hãy kết luận rằng chúng trực tiếp tạo thành bốn cạnh của hình vuông. 

Quyết định này hoàn toàn mang tính cục bộ đối với từng trường hợp thử nghiệm, do đó không có sự tương tác giữa các trường hợp và không có trạng thái tích lũy. 

### Tại sao nó hoạt động 

Một hình vuông cần có bốn cạnh bằng nhau. Vì gậy không thể sửa đổi nên mỗi bên phải chính xác là một gậy. Điều đó có nghĩa là tập hợp các độ dài cạnh phải chứa một giá trị duy nhất được lặp lại bốn lần. Bất kỳ sai lệch nào cũng gây ra ít nhất một cạnh có độ dài khác nhau, phá vỡ định nghĩa về hình vuông. Không có cách xây dựng thay thế nào bù đắp cho độ dài không khớp, vì vậy sự bình đẳng vừa cần vừa đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []
    for _ in range(t):
        a, b, c, d = map(int, input().split())
        if a == b == c == d:
            out.append("YES")
        else:
            out.append("NO")
    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Giải pháp đọc từng trường hợp kiểm thử một cách độc lập và thực hiện kiểm tra tính bằng nhau theo chuỗi đơn. Sự so sánh có dây chuyền`a == b == c == d`là một cách nhỏ gọn để đảm bảo tất cả các giá trị giống hệt nhau mà không cần viết rõ ràng các điều kiện theo cặp. Đầu ra được đệm để tránh lặp lại chi phí I/O. 

Một lỗi triển khai phổ biến là chỉ kiểm tra sự bằng nhau một phần, chẳng hạn như`a == b and c == d`, chấp nhận không chính xác các trường hợp như`1 1 2 2`. Một sai lầm khác là sắp xếp và kiểm tra đẳng thức liền kề, điều này vẫn đúng nhưng lại gây tốn kém không cần thiết đối với đầu vào có kích thước cố định nhỏ như vậy. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào:```
1
1 2 3 4
```| Bước | một | b | c | d | Quyết định | 
| --- | --- | --- | --- | --- | --- | 
| Đọc | 1 | 2 | 3 | 4 | - | 
| So sánh | 1 | 2 | 3 | 4 | tìm thấy không khớp | 
| Đầu ra | - | - | - | - | KHÔNG | 

Điều này cho thấy trường hợp tất cả các giá trị khác nhau nên không thể tạo được hình vuông. 

Bây giờ hãy xem xét:```
1
2 2 2 2
```| Bước | một | b | c | d | Quyết định | 
| --- | --- | --- | --- | --- | --- | 
| Đọc | 2 | 2 | 2 | 2 | - | 
| So sánh | 2 | 2 | 2 | 2 | tất cả đều bình đẳng | 
| Đầu ra | - | - | - | - | CÓ | 

Điều này xác nhận rằng các que giống hệt nhau tạo thành một hình vuông một cách tầm thường. 

Hai dấu vết này bao gồm cả đường dẫn chấp nhận và từ chối, cho thấy thuật toán chỉ phụ thuộc vào cấu trúc đẳng thức chứ không phụ thuộc vào thứ tự hoặc ghép nối. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi trường hợp thử nghiệm thực hiện một số lượng so sánh không đổi trên bốn số nguyên | 
| Không gian | O(1) | Chỉ có một vài biến được sử dụng, bỏ đệm đầu ra | 

Giải pháp này nằm trong giới hạn vì ngay cả đối với 10^4 trường hợp thử nghiệm, chúng tôi chỉ thực hiện một vài phép so sánh số nguyên cho mỗi trường hợp. Không có sự sắp xếp, đệ quy hoặc xử lý đồ thị nào được thực hiện. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()

    def solve():
        input = sys.stdin.readline
        t = int(input())
        res = []
        for _ in range(t):
            a, b, c, d = map(int, input().split())
            res.append("YES" if a == b == c == d else "NO")
        print("\n".join(res))

    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided samples
assert run("7\n1 2 3 4\n1 1 1 1\n2 2 2 2\n1 2 1 2\n1 1 5 5\n5 5 5 5\n4 10 5 9\n") == \
"NO\nYES\nYES\nNO\nNO\nYES\nNO"

# all equal minimum case
assert run("1\n1 1 1 1\n") == "YES", "all equal"

# one differing element
assert run("1\n1 1 1 2\n") == "NO", "single mismatch"

# alternating pairs
assert run("1\n3 3 4 4\n") == "NO", "pair symmetry not enough"

# larger identical values
assert run("1\n100 100 100 100\n") == "YES", "large equal values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều bình đẳng | CÓ | hình vuông hợp lệ đơn giản nhất | 
| không khớp đơn | KHÔNG | phát hiện vi phạm tối thiểu | 
| cặp đối xứng | KHÔNG | ngăn chặn giả định ghép nối không chính xác | 
| giá trị lớn bằng nhau | CÓ | không có vấn đề về cạnh số | 

## Vỏ cạnh 

Một trường hợp thất bại phổ biến là cho rằng việc ghép nối là đủ, chẳng hạn như xử lý`(a == b and c == d)`như vậy là đủ. Đối với đầu vào`1 1 2 2`, logic đó trả về không chính xác CÓ. Thuật toán đúng sẽ từ chối nó ngay lập tức vì không phải tất cả bốn giá trị đều khớp. 

đầu vào:```
1
1 1 2 2
```Thực thi: 

Sự so sánh đầu tiên vượt qua cho`a == b`, Nhưng`a == c`không thành công nên thuật toán dừng và trả về`NO`. 

Một trường hợp tinh tế khác là khi tất cả các giá trị đều khác biệt nhưng tạo thành một tập hợp “cân bằng” về mặt trực quan như`2 3 4 5`. Không có cấu trúc từng phần nào giúp ích được; việc kiểm tra thất bại ngay lập tức ở lần so sánh thứ hai. 

đầu vào:```
1
2 3 4 5
```Sự không khớp được phát hiện trong lần quét đẳng thức đầu tiên, tạo ra`NO`mà không cần phải lý luận gì thêm.
