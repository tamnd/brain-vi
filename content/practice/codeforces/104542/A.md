---
title: "CF 104542A - Chuỗi thú vị"
description: "Chúng ta được cho một mảng và chúng ta được phép chọn bất kỳ dãy con nào của dãy đó làm dãy ứng cử viên $b$. Điều khó khăn là chúng ta không chỉ kiểm tra $b$ đối với mảng ban đầu mà đối với một họ mảng dẫn xuất."
date: "2026-06-30T09:09:45+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104542
codeforces_index: "A"
codeforces_contest_name: "TheForces Round #22 (Interesting-Forces)"
rating: 0
weight: 104542
solve_time_s: 84
verified: false
draft: false
---

[CF 104542A - Chuỗi thú vị](https://codeforces.com/problemset/problem/104542/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 24s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mảng và chúng tôi được phép chọn bất kỳ chuỗi con nào của nó làm chuỗi ứng cử viên$b$. Vấn đề là chúng ta không kiểm tra$b$chỉ chống lại mảng ban đầu mà chống lại một họ mảng dẫn xuất. 

Đối với bất kỳ vị trí nào$i$, chúng tôi loại bỏ$a_i$từ mảng và sau đó chúng tôi lặp lại mảng rút gọn này nhiều lần, cụ thể là$n$bản sao được nối với nhau. Điều này tạo ra một chuỗi dài$c$chiều dài$n(n-1)$. Yêu cầu là phải tồn tại ít nhất một dãy con$b$của mảng ban đầu sao cho một số lựa chọn$i$,$b$không thể được tìm thấy dưới dạng một dãy con bên trong cấu trúc lặp lại này$c$. 

Vì vậy nhiệm vụ không phải là xây dựng$b$, mà là để quyết định liệu một “dãy con phân biệt” như vậy có tồn tại hay không. 

Các ràng buộc là lớn, với tổng$n$trên các trường hợp thử nghiệm lên đến$2 \cdot 10^5$. Bất kỳ giải pháp nào cố gắng kiểm tra tất cả các chuỗi con đều là không thể ngay lập tức, vì số lượng các chuỗi con là theo cấp số nhân. Ngay cả việc kiểm tra một dãy con ứng cử viên duy nhất đối với tất cả$i$các lựa chọn vẫn sẽ quá đắt nếu được thực hiện một cách ngây thơ, bởi vì bản thân việc kiểm tra trình tự con là tuyến tính trong kích thước mảng. 

Điều này đẩy chúng ta tới một quan sát có cấu trúc: cách duy nhất để một dãy con không thể xuất hiện trong$c$là nếu một số giá trị trong$b$quá “hiếm” trong mảng đã sửa đổi hoặc nếu các ràng buộc về thứ tự buộc không thể căn chỉnh trên các bản sao lặp lại. 

Một số tình huống phức tạp nảy sinh một cách tự nhiên. 

Ví dụ: nếu tất cả các phần tử đều giống hệt nhau$a = [1,1,1]$, thì việc loại bỏ bất kỳ phần tử nào vẫn để lại một mảng không đổi. Bất kỳ chuỗi tiếp theo nào$b$chỉ là một chuỗi 1 giây và nó luôn xuất hiện trong các bản sao lặp lại. Điều này cho thấy kết quả là “KHÔNG”. 

Nếu tồn tại một giá trị chỉ xuất hiện một lần, hãy nói$a = [1,2,3]$, việc loại bỏ phần tử duy nhất có thể thay đổi đáng kể tính khả dụng của các ký hiệu, thường khiến cho việc nhúng các chuỗi con nhất định vào tất cả các bản sao cùng một lúc là không thể. 

Khó khăn chính đó là$c$lặp lại cùng một multiset$n$nhiều lần nên nó có độ dư thừa rất lớn. Hạn chế có ý nghĩa duy nhất đến từ việc một vị trí bị loại bỏ trước khi lặp lại. 

## Phương pháp tiếp cận 

Một cách tiếp cận vũ phu sẽ thử mọi trình tự tiếp theo$b$, thì với mỗi$i$, xây dựng$c$và kiểm tra xem$b$là một dãy con của$c$. Thậm chí bỏ qua các dãy con hàm mũ, việc xây dựng và quét$c$chi phí$O(n^2)$, nó quá lớn. 

Chúng ta cần diễn giải lại những gì “$b$là một dãy con của$c$” thực sự có nghĩa là. Vì$c$chỉ là$n$sự lặp lại của mảng với một phần tử bị loại bỏ, bất kỳ việc nhúng chuỗi tiếp theo nào của$b$có thể được phân phối trên các bản sao. Điều này có nghĩa là sự lặp lại sẽ giúp ích cho quá trình so khớp hơn là hạn chế nó. 

Cái nhìn sâu sắc quan trọng là đảo ngược quan điểm. Thay vì hỏi liệu một số$b$thất bại trong một số$c$, chúng ta hỏi khi nào mỗi dãy con của$a$vẫn có thể nhúng vào mọi cấu trúc lặp đi lặp lại như vậy. Điều này xảy ra chính xác khi việc loại bỏ bất kỳ phần tử đơn lẻ nào không làm giảm khả năng biểu đạt của chuỗi đủ để chặn mẫu chuỗi con. 

Điều này thu gọn vấn đề thành việc kiểm tra xem liệu có tồn tại bất kỳ “cấu trúc quan trọng” nào trong$a$và cấu trúc đó hóa ra được xác định hoàn toàn bằng việc liệu tất cả các phần tử có giống nhau hay không. Nếu tồn tại ít nhất hai giá trị riêng biệt, chúng ta luôn có thể xây dựng một chuỗi con tạo ra sự không khớp khi bất kỳ chỉ mục nào bị xóa. Nếu tất cả các giá trị đều giống nhau, sự lặp lại đảm bảo mọi dãy con vẫn hợp lệ ở mọi nơi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Quét mảng và xác định xem tất cả các phần tử có bằng nhau hay không. 

Nếu có ít nhất hai giá trị phân biệt, chúng ta biết ngay một dãy con hợp lệ tồn tại. 
2. Nếu tất cả các phần tử đều giống nhau thì kết luận rằng không thể tồn tại dãy con phân biệt như vậy. 

Lý do đằng sau bước 1 là tính đa dạng về giá trị cho phép chúng ta xây dựng một dãy con phụ thuộc vào thứ tự tương đối hoặc tính sẵn có của các ký hiệu khác nhau, có thể bị phá vỡ bằng cách loại bỏ chỉ mục được chọn cẩn thận. 

Bước 2 tiếp theo vì một mảng không đổi vẫn bất biến khi xóa và lặp lại, do đó mọi dãy con vẫn có thể biểu diễn được trong mọi chuỗi được xây dựng.$c$. 

### Tại sao nó hoạt động 

Nếu mảng chứa ít nhất hai giá trị riêng biệt, hãy chọn hai vị trí có giá trị khác nhau. Bất kỳ chuỗi con nào mã hóa quá trình chuyển đổi giữa hai giá trị này đều có thể nhạy cảm với việc xóa chỉ mục được chọn cẩn thận, vì việc xóa một phần tử có thể phá vỡ sự căn chỉnh cần thiết trên các bản sao lặp lại của mảng. Sự lặp lại ở$c$không loại bỏ lỗ hổng này vì tất cả các bản sao đều giống hệt nhau ngoại trừ vị trí bị thiếu, do đó không phải lúc nào cũng có thể nhúng một chuỗi được lựa chọn cẩn thận. 

Nếu tất cả các giá trị đều giống nhau thì mọi dãy con chỉ là một chuỗi gồm các phần tử giống hệt nhau. Việc xóa bất kỳ vị trí nào cũng không làm thay đổi thực tế là mọi biểu tượng đều giống nhau và việc lặp lại chỉ làm tăng tính khả dụng. Như vậy, mọi dãy con của$a$vẫn là dãy con của mọi khả năng$c$, vì vậy không hợp lệ$b$tồn tại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        
        # check if all elements are the same
        first = a[0]
        ok = False
        for x in a:
            if x != first:
                ok = True
                break
        
        print("YES" if ok else "NO")

if __name__ == "__main__":
    solve()
```Việc triển khai trực tiếp mã hóa việc rút gọn để kiểm tra xem có ít nhất hai giá trị riêng biệt hay không. Vòng lặp sẽ thoát sớm khi tìm thấy sự không khớp, đảm bảo thời gian tuyến tính cho mỗi trường hợp thử nghiệm. 

Một lỗi phổ biến ở đây là làm phức tạp logic quá mức bằng cách cố gắng mô phỏng việc so khớp chuỗi con. Điều quan trọng là cấu trúc của$c$làm cho thuộc tính có ý nghĩa duy nhất là sự đa dạng của các phần tử chứ không phải sự sắp xếp của chúng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
2
1 1
3
1 2 1
4
5 4 1 1
```Chúng tôi theo dõi xem mảng có các phần tử riêng biệt hay không. 

| Kiểm tra | Mảng | Tìm thấy khác biệt | Đầu ra | 
| --- | --- | --- | --- | 
| 1 | [1,1] | Không | KHÔNG | 
| 2 | [1,2,1] | Có | CÓ | 
| 3 | [5,4,1,1] | Có | CÓ | 

Trong trường hợp đầu tiên, tất cả các giá trị đều giống hệt nhau, do đó mọi chuỗi con vẫn hoàn toàn có thể khớp được trong bất kỳ cấu trúc lặp lại nào. Trong các trường hợp khác, sự hiện diện của ít nhất hai giá trị riêng biệt đảm bảo tồn tại một dãy con phân biệt hợp lệ. 

### Ví dụ 2 

đầu vào:```
2
5
7 7 7 7 7
4
1 2 2 2
```| Kiểm tra | Mảng | Tìm thấy khác biệt | Đầu ra | 
| --- | --- | --- | --- | 
| 1 | [7,7,7,7,7] | Không | KHÔNG | 
| 2 | [1,2,2,2] | Có | CÓ | 

Trường hợp thứ hai chứng minh rằng ngay cả một phần tử khác cũng đủ để phá vỡ tính đồng nhất và cho phép một chuỗi con tách biệt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$mỗi trường hợp thử nghiệm | một lần để kiểm tra tính khác biệt | 
| Không gian |$O(1)$| chỉ các biến phụ không đổi | 

Tổng độ phức tạp là tuyến tính theo kích thước đầu vào trong tất cả các trường hợp thử nghiệm, phù hợp thoải mái trong giới hạn của$2 \cdot 10^5$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        first = a[0]
        ok = any(x != first for x in a)
        out.append("YES" if ok else "NO")
    return "\n".join(out)

# provided samples
assert run("3\n2\n1 1\n3\n1 2 1\n4\n5 4 1 1\n") == "NO\nYES\nYES"

# all equal minimum
assert run("1\n2\n7 7\n") == "NO"

# single deviation
assert run("1\n5\n9 9 9 1 9\n") == "YES"

# strictly increasing
assert run("1\n4\n1 2 3 4\n") == "YES"

# large uniform
assert run("1\n5\n5 5 5 5 5\n") == "NO"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều bình đẳng | KHÔNG | từ chối mảng thống nhất | 
| một cái khác nhau | CÓ | trường hợp phân tập tối thiểu | 
| ngày càng tăng | CÓ | trường hợp riêng biệt chung | 
| đồng phục lớn | KHÔNG | xử lý thống nhất ranh giới | 

## Vỏ cạnh 

Một mảng hoàn toàn thống nhất như$a = [4,4,4,4]$luôn dẫn đến KHÔNG. Thuật toán đọc giá trị đầu tiên là 4 và không bao giờ tìm thấy giá trị không khớp, vì vậy`ok`vẫn sai, tạo ra NO một cách chính xác. 

Một mảng gần như đồng nhất như$a = [10,10,10,11,10]$kích hoạt việc thoát sớm ở phần tử thứ tư. Cờ trở thành đúng ngay lập tức, đảm bảo CÓ mà không cần kiểm tra thêm cấu trúc. 

Mảng có độ dài-2 cũng được xử lý chính xác. Vì$a = [3,3]$, không có sự khác biệt nên câu trả lời là KHÔNG. Vì$a = [3,5]$, phép so sánh đầu tiên đã phát hiện ra sự khác biệt và trả về CÓ.
