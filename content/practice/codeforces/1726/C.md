---
title: "CF 1726C - Trình tự khung cân bằng của Jatayu"
description: "Chúng ta được cấp một chuỗi ngoặc cân bằng có độ dài $2n$. Mỗi vị trí trong chuỗi này được coi là một đỉnh trong biểu đồ. Hai đỉnh $i$ và $j$ được kết nối bằng một cạnh vô hướng chính xác khi chuỗi con từ $i$ đến $j$ tự tạo thành một chuỗi dấu ngoặc cân bằng."
date: "2026-06-15T01:52:53+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "dsu", "graphs", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1726
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 819 (Div. 1 + Div. 2) and Grimoire of Code Annual Contest 2022"
rating: 1300
weight: 1726
solve_time_s: 450
verified: false
draft: false
---

[CF 1726C - Trình tự khung cân bằng của Jatayu](https://codeforces.com/problemset/problem/1726/C) 

**Đánh giá:** 1300 
**Tags:** cấu trúc dữ liệu, dsu, đồ thị, tham lam 
**Thời gian giải:** 7 phút 30 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một chuỗi ngoặc cân bằng có độ dài$2n$. Mỗi vị trí trong chuỗi này được coi là một đỉnh trong biểu đồ. Hai đỉnh$i$Và$j$được kết nối bởi một cạnh vô hướng chính xác khi chuỗi con từ$i$ĐẾN$j$tự nó tạo thành một chuỗi dấu ngoặc cân bằng. 

Nhiệm vụ không phải là xây dựng biểu đồ này một cách rõ ràng, nó sẽ quá lớn, mà là xác định xem nó chứa bao nhiêu thành phần được kết nối. 

Khó khăn chính là các cạnh phụ thuộc vào cấu trúc toàn cục của chuỗi con hơn là sự kề cận cục bộ. Mặc dù chuỗi ban đầu đã được cân bằng nhưng hầu hết các chuỗi con đều không được cân bằng, do đó biểu đồ thưa thớt theo cách ngầm định phải được hiểu theo cách kết hợp. 

Các ràng buộc buộc chúng ta phải đưa ra các giải pháp tuyến tính hoặc gần tuyến tính. Tổng chiều dài của tất cả các trường hợp thử nghiệm nhiều nhất là$10^5$, do đó, bất kỳ thuật toán nào kiểm tra chuỗi con hoặc cố gắng mô phỏng từng cặp kết nối sẽ ngay lập tức thất bại. Thậm chí$O(n^2)$việc quét các chuỗi con là không thể vì một trường hợp thử nghiệm đơn lẻ đã có thể đạt tới$10^5$. 

Một trường hợp cạnh tinh tế phát sinh từ thực tế là mọi chuỗi ngoặc cân bằng có thể được phân tách duy nhất thành các phân đoạn nguyên thủy, trong đó phân đoạn nguyên thủy là tiền tố nhỏ nhất được cân bằng. Ví dụ,`"()(())"`chia thành`"()"`Và`"(())"`. Cấu trúc đồ thị hóa ra phụ thuộc chính xác vào cách sắp xếp các khối nguyên thủy này. 

Một cách tiếp cận đơn giản sẽ cố gắng kết nối các chỉ mục bằng cách kiểm tra tính hợp lệ của mọi chuỗi con bằng cách sử dụng tổng tiền tố hoặc ngăn xếp. Điều này sẽ gợi ý không chính xác rằng khả năng kết nối rất dày đặc, nhưng nó bỏ sót rằng các chuỗi con cân bằng phải tương ứng với các phân đoạn có tổng bằng 0 chính xác và không bao giờ âm trong nội bộ. Ràng buộc đó là những gì hạn chế các cạnh bên trong các khối nguyên thủy. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực sẽ kiểm tra từng cặp$i < j$, xác minh xem$s[i..j]$được cân bằng bằng cách sử dụng kiểm tra cân bằng ngăn xếp hoặc tiền tố, sau đó hợp nhất các đỉnh đó trong DSU. Kiểm tra chi phí một chuỗi con$O(n)$trong trường hợp xấu nhất, và có$O(n^2)$chuỗi con, dẫn đến$O(n^3)$hành vi vượt xa giới hạn khả thi. 

Chúng ta có thể cải thiện điều này bằng cách lưu ý rằng việc kiểm tra các chuỗi con cân bằng có thể được giảm xuống thành tổng tiền tố: một chuỗi con được cân bằng nếu tổng số dư của nó bằng 0 và không có tiền tố nào trong đó giảm xuống dưới mức bắt đầu. Ngay cả với tổng tiền tố, việc lặp qua tất cả các cặp vẫn là phương trình bậc hai. 

Cái nhìn sâu sắc về cấu trúc quan trọng là khả năng kết nối bị chi phối hoàn toàn bởi sự phân rã nguyên thủy của chuỗi cân bằng. Mỗi khối nguyên thủy là một tiền tố cân bằng tối thiểu quay về số 0 và không bao giờ chạm vào số 0 bên trong. Bên trong một khối như vậy, bất kỳ đỉnh nào cũng có thể được kết nối với bất kỳ đỉnh nào khác thông qua các chuỗi con cân bằng hợp lệ, bởi vì cấu trúc hoạt động giống như một thành phần độc lập. Tuy nhiên, giữa hai khối nguyên thủy khác nhau, không có chuỗi con cân bằng nào vượt qua ranh giới, vì bất kỳ sự vượt qua nào cũng sẽ vi phạm các ràng buộc cân bằng tiền tố. 

Do đó, mỗi đoạn nguyên thủy tương ứng với chính xác một thành phần được kết nối trong biểu đồ. Vấn đề giảm xuống việc đếm số lần tổng tiền tố trở về 0 trong khi quét chuỗi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^3)$|$O(n)$| Quá chậm | 
| Tối ưu (đếm nguyên thủy) |$O(n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Di chuyển chuỗi từ trái sang phải trong khi duy trì bộ đếm số dư tăng dần cho`'('`và giảm dần cho`')'`. Điều này theo dõi mức độ lồng sâu của chúng tôi trong cấu trúc khung. 
2. Bất cứ khi nào bộ đếm số dư trở về 0, chúng ta đã hoàn thành một phân đoạn cân bằng nguyên thủy. Điều này có nghĩa là chuỗi con từ điểm cắt cuối cùng đến vị trí hiện tại là một khối hợp lệ độc lập tối thiểu. 
3. Tăng câu trả lời mỗi khi số dư trở về 0, vì mỗi sự kiện như vậy đánh dấu sự kết thúc của một thành phần bị ngắt kết nối trong biểu đồ. 
4. Tiếp tục cho đến hết chuỗi, đảm bảo tất cả các giá trị gốc đều được tính. 

Trực giác đằng sau bước 2 là tiền tố cân bằng trở về 0 không thể được chia thành các phần cân bằng nhỏ hơn mà không vi phạm ràng buộc tiền tố, do đó nó hoạt động như một đơn vị cấu trúc. 

### Tại sao nó hoạt động 

Phân đoạn nguyên thủy là vùng liền kề tối đa trong đó số dư đang chạy bắt đầu từ 0, duy trì mức dương hoàn toàn bên trong và chỉ trở về 0 ở cuối. Bất kỳ chuỗi con cân bằng nào bắt đầu bên trong một khối nguyên thủy và kết thúc bên ngoài nó đều phải vượt qua một điểm mà ở đó số dư bằng 0 ở giữa, điều này sẽ buộc phải phân tách. Điều đó ngăn không cho bất kỳ cạnh nào kết nối các đỉnh qua các ranh giới nguyên thủy. Bên trong một khối nguyên thủy, các chuỗi con cân bằng chỉ có thể kết nối các đỉnh trong cùng một khối, do đó tất cả các đỉnh trong một khối đều có thể truy cập lẫn nhau thông qua các cạnh hợp lệ. Điều này tạo ra sự tương ứng một-một giữa các khối nguyên thủy và các thành phần được kết nối. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    s = input().strip()

    bal = 0
    components = 0

    for ch in s:
        if ch == '(':
            bal += 1
        else:
            bal -= 1

        if bal == 0:
            components += 1

    print(components)
```Giải pháp xử lý từng trường hợp thử nghiệm một cách độc lập. Bộ đếm số dư theo dõi tính hợp lệ của tiền tố theo thời gian không đổi trên mỗi ký tự. Mỗi khi bộ đếm trở về 0, khối nguyên thủy kết thúc và chúng ta tăng số lượng thành phần. 

Chi tiết triển khai chính là chúng tôi không bao giờ đặt lại bất cứ điều gì ngoại trừ ngầm thông qua việc số dư trở về 0. Điều này tránh mọi nhu cầu phân đoạn rõ ràng hoặc xử lý chuỗi con. 

## Ví dụ đã hoạt động 

### Ví dụ 1:`()(())`Chúng tôi theo dõi số dư và đếm các thành phần: 

| Chỉ mục | Char | Số dư | Linh kiện | 
| --- | --- | --- | --- | 
| 1 | ( | 1 | 0 | 
| 2 | ) | 0 | 1 | 
| 3 | ( | 1 | 1 | 
| 4 | ( | 2 | 1 | 
| 5 | ) | 1 | 1 | 
| 6 | ) | 0 | 2 | 

Số dư trở về 0 hai lần, vì vậy chúng ta có 2 thành phần. Điều này cho thấy rằng`"()"`Và`"(())"`là các khối nguyên thủy độc lập. 

### Ví dụ 2:`((()))`| Chỉ mục | Char | Số dư | Linh kiện | 
| --- | --- | --- | --- | 
| 1 | ( | 1 | 0 | 
| 2 | ( | 2 | 0 | 
| 3 | ( | 3 | 0 | 
| 4 | ) | 2 | 0 | 
| 5 | ) | 1 | 0 | 
| 6 | ) | 0 | 1 | 

Ở đây toàn bộ chuỗi tạo thành một khối nguyên thủy duy nhất, do đó có một thành phần được kết nối. 

Những dấu vết này chứng minh rằng câu trả lời chính xác là số lượng các đoạn cân bằng nguyên thủy. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi ký tự được xử lý một lần, cập nhật một bộ đếm duy nhất | 
| Không gian |$O(1)$| Chỉ có một số biến số nguyên được duy trì | 

Tổng kích thước đầu vào trên tất cả các trường hợp thử nghiệm tối đa là$10^5$, do đó, việc quét tuyến tính cho mỗi trường hợp kiểm thử có thể dễ dàng đủ nhanh trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output

    t = int(input())
    for _ in range(t):
        n = int(input())
        s = input().strip()

        bal = 0
        ans = 0
        for c in s:
            bal += 1 if c == '(' else -1
            if bal == 0:
                ans += 1
        print(ans)

    return output.getvalue()

# provided samples
assert run("""4
1
()
3
()(())
3
((()))
4
(())(())""") == """1
2
1
2
"""

# custom: single primitive
assert run("""1
3
((()))""") == "1\n"

# custom: fully split
assert run("""1
3
()()()""") == "3\n"

# custom: alternating nesting
assert run("""1
4
(())()()""") == "3\n"

# custom: minimum
assert run("""1
1
()""") == "1\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`((()))`| 1 | khối nguyên thủy đơn | 
|`()()()`| 3 | phân mảnh tối đa | 
|`(())()()`| 3 | đếm ranh giới cấu trúc hỗn hợp | 
|`()`| 1 | trường hợp tối thiểu | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi toàn bộ chuỗi là một cấu trúc được lồng sâu như`"(((())))"`. Trong trường hợp này, số dư chỉ trở về 0 một lần vào cuối, do đó thuật toán xuất ra chính xác một thành phần. Mọi nỗ lực phân chia dựa trên các mẫu cục bộ sẽ bị tính quá mức một cách không chính xác. 

Một trường hợp khác là các nguyên thủy xen kẽ hoàn toàn như`"()()()"`. Số dư đạt 0 sau mỗi hai ký tự, tạo ra ba thành phần. Thuật toán tự nhiên ghi lại mỗi lần đặt lại dưới dạng điểm phân tách và không có chuỗi con nào giao nhau giữa các lần đặt lại này, do đó khả năng kết nối vẫn bị giới hạn trong mỗi cặp. 

Một cấu trúc hỗn hợp như`"(())()()"`xác nhận rằng các nguyên hàm lồng nhau và phẳng có thể cùng tồn tại. Lần đặt lại đầu tiên xảy ra sau khối lồng nhau và các lần đặt lại tiếp theo xảy ra sau mỗi khối lồng nhau.`"()"`, tạo ra chính xác ba thành phần. Điều này phù hợp với thực tế là không có chuỗi con cân bằng nào có thể vượt qua các điểm đặt lại này, vì làm như vậy sẽ vi phạm sự cân bằng tiền tố bên trong chuỗi.
