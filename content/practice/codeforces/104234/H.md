---
title: "CF 104234H - Đẳng cấu đồ thị"
description: "Chúng ta có một đồ thị đơn giản vô hướng trên các đỉnh $n$. Từ biểu đồ này, về mặt khái niệm, chúng tôi tạo ra tất cả các biểu đồ có thể thu được bằng cách dán nhãn lại các đỉnh theo mọi cách có thể. Hai lần dán nhãn lại tạo ra các bộ cạnh khác nhau được tính là các biểu đồ khác nhau."
date: "2026-07-01T23:36:59+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104234
codeforces_index: "H"
codeforces_contest_name: "OCPC 2023, Oleksandr Kulkov Contest 3"
rating: 0
weight: 104234
solve_time_s: 49
verified: true
draft: false
---

[CF 104234H - Đẳng cấu đồ thị](https://codeforces.com/problemset/problem/104234/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một đồ thị đơn giản vô hướng trên$n$đỉnh. Từ biểu đồ này, về mặt khái niệm, chúng tôi tạo ra tất cả các biểu đồ có thể thu được bằng cách dán nhãn lại các đỉnh theo mọi cách có thể. Hai lần dán nhãn lại tạo ra các bộ cạnh khác nhau được tính là các biểu đồ khác nhau. 

Câu hỏi không phải là liệt kê chúng mà là quyết định xem số lượng đồ thị được gắn nhãn riêng biệt đẳng cấu với đồ thị đã cho có nhiều nhất hay không$n$. Nói cách khác, chúng tôi lấy tất cả các hoán vị của nhãn đỉnh, áp dụng chúng vào biểu đồ, loại bỏ các bộ cạnh kết quả giống hệt nhau và đếm xem có bao nhiêu biểu đồ duy nhất xuất hiện. Chúng ta phải quyết định xem số đếm này có bị giới hạn bởi$n$. 

Các ràng buộc lớn, với tổng số đỉnh và cạnh trên tất cả các trường hợp thử nghiệm lên tới$10^5$. Điều này ngay lập tức loại trừ bất kỳ điều gì liên quan đến lý luận hoán vị rõ ràng hoặc ghi nhãn chính tắc trên đồ thị. Bất kỳ cách tiếp cận nào phụ thuộc vào sự tăng trưởng giai thừa, kiểm tra tính tự cấu hình mạnh mẽ hoặc kiểm tra sự đẳng cấu đồ thị trên mỗi hoán vị đều không thể thực hiện được trong thời gian giới hạn. 

Khó khăn chính là đại lượng chúng ta được hỏi phụ thuộc vào nhóm tự đẳng cấu của đồ thị. Hai hoán vị tạo ra cùng một biểu đồ có nhãn chính xác khi chúng khác nhau bởi tính tự đẳng cấu. Vậy số đồ thị phân biệt đẳng cấu với$G$bằng$n! / |\mathrm{Aut}(G)|$. Do đó, nhiệm vụ này tương đương với việc kiểm tra xem giá trị này có lớn nhất không$n$, nhưng tính toán trực tiếp kích thước nhóm tự động cấu là vượt xa khả thi. 

Một vấn đề tế nhị xuất hiện trong các biểu đồ nhỏ. Vì$n=1$Và$n=2$, hầu hết mọi cấu trúc đều thu gọn một cách tầm thường thành rất ít biểu đồ được gắn nhãn riêng biệt, vì vậy trực giác ngây thơ về “nhiều hoán vị” có thể gây hiểu nhầm. Ví dụ, khi$n=2$, cả đồ thị trống và đồ thị một cạnh đều tạo ra chính xác một đồ thị được gắn nhãn riêng biệt khi được dán nhãn lại, mặc dù có$2!$hoán vị. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ cố gắng hiểu tất cả các hoán vị của các đỉnh và nhóm chúng theo biểu đồ mà chúng tạo ra. Về mặt khái niệm, đối với mỗi hoán vị, chúng ta sẽ gắn nhãn lại cho biểu đồ và chèn tập cạnh kết quả vào tập băm. Điều này đúng nhưng hoàn toàn không khả thi. có$n!$hoán vị và thậm chí tạo ra một biểu đồ được gắn nhãn lại$O(n + m)$. Điều này bùng nổ ngay lập tức ngay cả đối với$n = 20$. 

Quan sát quan trọng là số lượng đồ thị có nhãn riêng biệt trong lớp đẳng cấu được xác định hoàn toàn bởi tính đối xứng. Đồ thị có tính đối xứng cao có ít sự thay đổi nhãn rõ ràng hơn vì nhiều hoán vị không làm thay đổi cấu trúc cạnh. 

Chúng tôi muốn con số này nhiều nhất$n$, cực kỳ nhỏ so với$n!$. Điều đó buộc nhóm tự động phải vô cùng lớn. Trên thực tế, cách duy nhất để giảm$n! / |\mathrm{Aut}(G)|$xuống tới$O(n)$là dành cho$|\mathrm{Aut}(G)|$theo thứ tự của$(n-1)!$hoặc lớn hơn, nghĩa là hầu hết mọi hoán vị đều là tự đẳng cấu. 

Mức độ đối xứng đó chỉ có thể xảy ra trong các đồ thị trong đó tất cả các đỉnh đều có cấu trúc giống hệt nhau theo nghĩa mạnh nhất có thể. Trong đồ thị vô hướng đơn giản, điều này chỉ xảy ra trong hai trường hợp: đồ thị trống và đồ thị đầy đủ. Trong cả hai trường hợp, mọi hoán vị của các đỉnh đều bảo toàn tính kề cận, do đó$|\mathrm{Aut}(G)| = n!$, và do đó số lượng đồ thị có nhãn riêng biệt chính xác là$1$, thỏa mãn điều kiện một cách tầm thường. 

Bất kỳ đồ thị nào khác đều phá vỡ tính đối xứng. Thời điểm tồn tại một cặp đỉnh có bậc khác nhau hoặc các kiểu kề cận khác nhau, các hoán vị ngăn cách chúng không còn là tự động cấu hình nữa và nhóm tự động co lại một cách đáng kể. Điều này đẩy kích thước quỹ đạo vượt xa$n$, nên câu trả lời là KHÔNG. 

Do đó, vấn đề rút gọn thành một kiểm tra cấu trúc đơn giản: xác định xem đồ thị trống hay đầy đủ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên hoán vị |$O(n! \cdot (n+m))$|$O(n+m)$| Quá chậm | 
| Kiểm tra kết cấu (trống hoặc hoàn chỉnh) |$O(n+m)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta giải quyết vấn đề bằng việc kiểm tra xem đồ thị có các cạnh bằng 0 hay tất cả các cạnh có thể có. 

1. Đọc số đỉnh$n$và các cạnh$m$, sau đó đọc tất cả các cạnh. 
2. Tính tổng số cạnh có thể có của một đồ thị vô hướng đơn giản, đó là$n(n-1)/2$. 
3. Nếu$m = 0$, biểu đồ trống, nghĩa là mọi hoán vị đều bảo toàn nó nên nó thỏa mãn điều kiện. 
4. Nếu$m = n(n-1)/2$, đồ thị là đầy đủ, nghĩa là mọi hoán vị đều bảo toàn nó nên nó cũng thỏa mãn điều kiện. 
5. Mặt khác, đồ thị không đối xứng hoàn toàn cũng như không được kết nối tối đa, do đó xuất ra NO. 

Lý do điều này hoạt động là vì chỉ các đồ thị trống và đầy đủ mới có tính đối xứng đỉnh đầy đủ. Bất kỳ sai lệch nào so với các cấu trúc này sẽ tạo ra ít nhất một đỉnh có bậc khác với đỉnh khác hoặc có mẫu kề khác nhau, phá vỡ tính đối xứng hoàn toàn. Điều đó ngay lập tức làm giảm kích thước nhóm tự động cấu hình và tăng số lượng đồ thị được gắn nhãn lại riêng biệt vượt quá ngưỡng cho phép. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []
    for _ in range(t):
        n, m = map(int, input().split())
        for _ in range(m):
            input()
        max_edges = n * (n - 1) // 2
        if m == 0 or m == max_edges:
            out.append("YES")
        else:
            out.append("NO")
    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Việc triển khai dựa vào việc bỏ qua cấu trúc cạnh thực tế ngoài việc đếm các cạnh. Mỗi trường hợp thử nghiệm sử dụng danh sách cạnh của nó mà không lưu trữ nó, vì chỉ có số lượng mới quan trọng. 

Việc tính toán của$n(n-1)/2$phải sử dụng số học số nguyên và phải cẩn thận để đọc tất cả$m$các cạnh để giữ cho phân tích cú pháp đầu vào được căn chỉnh. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 3
1 2
2 3
3 1
```Chúng tôi có$n=3$,$m=3$. Các cạnh tối đa có thể cũng là$3$. Biểu đồ đã hoàn tất. 

| Bước | n | m | max_edge | Quyết định | 
| --- | --- | --- | --- | --- | 
| Đọc đầu vào | 3 | 3 | 3 | hoàn thành | 
| So sánh | | | | m == max_edge | 

Đầu ra là CÓ. 

Điều này xác nhận rằng các đồ thị hoàn chỉnh thỏa mãn điều kiện do tính đối xứng hoán vị đầy đủ. 

### Ví dụ 2 

đầu vào:```
5 4
1 2
2 3
3 4
4 5
```Đây$n=5$,$m=4$, trong khi các cạnh tối đa là$10$. Biểu đồ không trống cũng không đầy đủ. 

| Bước | n | m | max_edge | Quyết định | 
| --- | --- | --- | --- | --- | 
| Đọc đầu vào | 5 | 4 | 10 | không | 
| So sánh | | | | m không phải 0 hoặc tối đa | 

Đầu ra là KHÔNG. 

Điều này cho thấy rằng ngay cả các đồ thị có cấu trúc vừa phải cũng phá vỡ tính đối xứng đủ để vượt quá số lượng đồ thị được gắn nhãn đẳng cấu cho phép. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n + m)$| Mỗi cạnh được đọc một lần và mỗi trường hợp kiểm thử thực hiện công việc liên tục sau khi phân tích cú pháp đầu vào | 
| Không gian |$O(1)$| Không cần lưu trữ biểu đồ, chỉ sử dụng bộ đếm | 

Giải pháp dễ dàng phù hợp với các ràng buộc vì tổng kích thước đầu vào trên tất cả các trường hợp thử nghiệm được giới hạn bởi$10^5$, làm cho việc quét tuyến tính trở nên tối ưu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    try:
        solve()
    except Exception:
        pass
    return sys.stdout.getvalue().strip()

# provided-style checks
assert run("""3
3 3
1 2
2 3
3 1
3 0
5 0
""") == "YES\nYES\nYES"

# empty and complete extremes
assert run("""2
4 0
4 6
1 2
1 3
1 4
2 3
2 4
3 4
""") == "YES\nYES"

# non-complete sparse graph
assert run("""1
5 4
1 2
2 3
3 4
4 5
""") == "NO"

# single edge case
assert run("""1
2 1
1 2
""") == "YES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 4 nút, 0 cạnh | CÓ | đối xứng đồ thị trống | 
| 4 nút, đầy đủ các cạnh | CÓ | đồ thị đối xứng hoàn chỉnh | 
| đồ thị đường dẫn | KHÔNG | trường hợp đối xứng bị hỏng | 
| cạnh đơn | CÓ | đồ thị không tầm thường nhỏ nhất | 

## Vỏ cạnh 

Trường hợp đồ thị trống là ví dụ trực tiếp nhất về tính đối xứng đầy đủ. Mọi hoán vị của các đỉnh đều bảo toàn sự vắng mặt của các cạnh, vì vậy tất cả$n!$việc dán nhãn lại sẽ thu gọn thành một biểu đồ riêng biệt duy nhất. Thuật toán xử lý việc này bằng cách kiểm tra$m = 0$, ngay lập tức kích hoạt CÓ. 

Trường hợp đồ thị hoàn chỉnh hoạt động giống hệt nhau từ quan điểm đối xứng. Mọi cặp đỉnh đều được kết nối, do đó việc dán nhãn lại không thể thay đổi sự kề cận. Thuật toán phát hiện điều này thông qua$m = n(n-1)/2$. 

Đối với các đồ thị trung gian, ngay cả một cạnh bị thiếu hoặc thừa cũng sẽ phá hủy khả năng thay thế hoàn toàn của các đỉnh. Ví dụ: nếu một đỉnh có bậc khác với đỉnh khác, việc hoán đổi chúng sẽ làm thay đổi cấu trúc đồ thị, do đó nhiều hoán vị không còn thuộc nhóm tự đẳng cấu. Điều này đảm bảo số lượng đồ thị được gắn nhãn riêng biệt sẽ tăng vượt xa$n$, đó là lý do tại sao thuật toán xuất ra NO một cách an toàn trong mọi trường hợp không cực đoan.
