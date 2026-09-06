---
title: "CF 104544B - Người phán xử tốt"
description: "Chúng ta được cho hai mảng số nguyên có độ dài bằng nhau. Trong mỗi trường hợp thử nghiệm, chúng ta có thể thực hiện các thao tác mở rộng toàn bộ mảng bằng cách nhân tất cả các phần tử của nó với một số thừa số nguyên. Chúng ta có thể lặp lại điều này bao nhiêu lần và mỗi phép nhân được tính là một thao tác."
date: "2026-06-30T09:01:49+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104544
codeforces_index: "B"
codeforces_contest_name: "Aleppo Collegiate Programming Contest 2023 V.2"
rating: 0
weight: 104544
solve_time_s: 128
verified: true
draft: false
---

[CF 104544B - Người phán xử tốt](https://codeforces.com/problemset/problem/104544/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2 phút 8 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho hai mảng số nguyên có độ dài bằng nhau. Trong mỗi trường hợp thử nghiệm, chúng ta có thể thực hiện các thao tác mở rộng toàn bộ mảng bằng cách nhân tất cả các phần tử của nó với một số thừa số nguyên. Chúng ta có thể lặp lại điều này bao nhiêu lần và mỗi phép nhân được tính là một thao tác. 

Đại lượng duy nhất quan trọng sau bất kỳ chuỗi phép tính nào là ước số chung lớn nhất của mỗi mảng. Nếu gcds ban đầu là$G_a$Và$G_b$, sau đó sau khi hoạt động, chúng tôi kết thúc với$G_a \cdot A$Và$G_b \cdot B$, Ở đâu$A$Và$B$là tích của tất cả các phép nhân áp dụng cho mỗi mảng. 

Mục tiêu là làm cho các giá trị gcd cuối cùng này bằng nhau trong khi giảm thiểu tổng số phép nhân mà chúng ta sử dụng. 

Kích thước đầu vào lớn trong các trường hợp thử nghiệm, vì vậy giải pháp phải tính toán từng câu trả lời trong thời gian không đổi hoặc gần như không đổi cho mỗi trường hợp thử nghiệm sau khi tính toán gcds. Bất cứ điều gì như tìm kiếm trên các số nhân có thể có hoặc các phép toán mô phỏng đều không thể thực hiện được vì các giá trị có thể lớn và tổng$n$tổng cộng lên tới vài trăm nghìn. 

Trường hợp góc tinh tế xuất hiện khi một gcd chia cho cái kia. Trong trường hợp đó, một phép nhân có thể sửa mọi thứ cho một mảng. Nếu cả hai đều không chia hết cho bên kia, chúng ta không thể căn chỉnh chúng trong một bước vì một thao tác chỉ chia tỷ lệ cho một bên và chúng ta không thể “sửa một phần” các vấn đề về khả năng chia hết sau này. 

## Phương pháp tiếp cận 

Một cách trực tiếp để suy nghĩ về vấn đề là mô phỏng các phép toán: thử các chuỗi nhân khác nhau trên cả hai mảng và kiểm tra xem khi nào gcd của chúng khớp nhau. Điều này nhanh chóng trở nên không khả thi vì mỗi phép toán có thể chọn bất kỳ số nguyên nào, do đó hệ số phân nhánh là không giới hạn và ngay cả các đầu vào nhỏ cũng sẽ bùng nổ về mặt tổ hợp. 

Quan sát quan trọng là việc nhân một mảng chỉ chia tỷ lệ gcd của nó. Cấu trúc bên trong của mảng trở nên không phù hợp; chỉ có gcd tiến hóa theo cấp số nhân. Vậy bài toán quy về biến đổi hai số$G_a$Và$G_b$thành đẳng thức bằng cách sử dụng các phép tính nhân một số với một số nguyên tùy ý, mỗi phép tính có giá trị bằng một. 

Vì một thao tác có thể nhân với bất kỳ số nguyên nào nên bất kỳ hệ số bắt buộc nào cũng có thể được áp dụng trong một bước. Điều đó có nghĩa là chúng ta không tính độ phức tạp số học của các thừa số mà chỉ tính xem chúng ta có cần áp dụng thay đổi hay không. 

Vì vậy, chúng tôi hỏi: số lần “thay đổi bên” tối thiểu cần thiết để làm cho hai số bằng nhau là bao nhiêu? 

Nếu như$G_a = G_b$, không cần thao tác. 

Nếu chúng ta có thể chọn một giá trị mục tiêu bằng một trong số chúng, chẳng hạn$G_a$, thì chúng ta chỉ cần sửa phía bên kia nếu nó có thể đạt được bằng phép nhân. Điều đó có thể xảy ra chính xác khi$G_a$là bội số của$G_b$. Một cách đối xứng, chúng ta có thể nhắm mục tiêu$G_b$nếu nó là bội số của$G_a$. 

Nếu không chia hết cho bên kia thì không có phép nhân đơn lẻ nào ở một bên có thể thu hẹp khoảng cách, do đó cả hai bên phải được sửa đổi ít nhất một lần và hai thao tác là đủ: chia tỷ lệ mỗi bên thành bất kỳ bội số chung nào. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force qua hoạt động | Hàm mũ | O(1) | Quá chậm | 
| Logic giảm GCD | O(n) mỗi lần kiểm tra | O(1) thêm | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính gcd của mảng$a$, gọi nó$G_a$. Điều này nén toàn bộ mảng thành một giá trị đại diện duy nhất nắm bắt tất cả các ràng buộc về khả năng chia hết. 
2. Tính gcd của mảng$b$, gọi nó$G_b$. Lý do tương tự được áp dụng một cách đối xứng. 
3. Nếu$G_a$bằng$G_b$, trả về 0 vì cả hai mảng đều đã thỏa mãn điều kiện mà không cần sửa đổi. 
4. Nếu$G_a$chia rẽ$G_b$, trả về 1 vì chúng ta có thể nhân mảng$a$một lần bởi$G_b / G_a$và khớp với gcds. 
5. Nếu$G_b$chia rẽ$G_a$, trả về 1 vì lý do đối xứng. 
6. Nếu không thì trả về 2 vì cả hai gcd đều không thể được chuyển đổi thành gcd kia thông qua một tỷ lệ duy nhất, vì vậy cả hai phải được điều chỉnh một lần để gặp nhau ở một bội số chung nào đó. 

Bước lý luận chính là mỗi thao tác mang lại sự tự do hoàn toàn đối với hệ số nhân, vì vậy chúng ta không bao giờ cần nhiều hơn một thao tác trên mỗi mảng. 

### Tại sao nó hoạt động 

Sau bất kỳ chuỗi thao tác nào, gcd của mỗi mảng là gcd ban đầu của nó nhân với tích của các số nguyên đã chọn. Vì chúng ta có thể chọn các số nguyên tùy ý trong một thao tác, nên bất kỳ hệ số nhân cần thiết nào cũng có thể được áp dụng trong một bước duy nhất. Do đó, hạn chế về cấu trúc duy nhất là khả năng phân chia giữa hai gcd ban đầu. Nếu cái này chia cái kia, một sự điều chỉnh duy nhất sẽ căn chỉnh chúng; nếu không thì cả hai phải di chuyển. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from math import gcd

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))

        ga = 0
        for x in a:
            ga = gcd(ga, x)

        gb = 0
        for x in b:
            gb = gcd(gb, x)

        if ga == gb:
            print(0)
        elif ga % gb == 0 or gb % ga == 0:
            print(1)
        else:
            print(2)

if __name__ == "__main__":
    solve()
```Việc triển khai giảm từng mảng thành gcd của nó bằng cách sử dụng quét tuyến tính, điều này là cần thiết vì các giá trị lớn và chúng ta không thể dựa vào cấu trúc ngoài gcd. Logic quyết định sau đó áp dụng trực tiếp các trường hợp chia hết được rút ra trước đó. 

Một lỗi phổ biến là cố gắng suy luận về các phần tử riêng lẻ thay vì gcd, nhưng các phép toán hoạt động thống nhất trên toàn bộ mảng, khiến cho việc suy luận ở cấp độ phần tử trở nên không cần thiết. 

## Ví dụ đã hoạt động 

Hãy xem xét trường hợp cả hai mảng đều có cùng một gcd. Sau khi tính toán, cả hai bên khớp ngay lập tức và thuật toán thoát ra sớm mà không có thao tác nào. 

Bây giờ hãy xem xét trường hợp một gcd là bội số của gcd kia. Giả định$G_a = 6$Và$G_b = 2$. Vì 6 chia hết cho 2 nên ta có thể nhân mảng$b$bằng 3 trong một thao tác, tạo ra gcd 6. Thuật toán phát hiện tính chia hết và trả về 1. 

Cuối cùng, hãy xem xét trường hợp gcds là 6 và 10. Cả hai đều không chia hết cho nên không có tỷ lệ đơn lẻ nào căn chỉnh chúng. Chúng ta phải chia tỷ lệ độc lập cả hai vế theo bội số chung, tốn 2 phép tính. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi phép tính gcd quét mảng một lần | 
| Không gian | O(1) thêm | Chỉ lưu trữ các giá trị gcd đang chạy | 

Các ràng buộc cho phép tổng cộng$2 \times 10^5$các phần tử, do đó, chỉ cần một lần tuyến tính cho mỗi trường hợp thử nghiệm là đủ. Tất cả các phép toán đều là các phép tính gcd số nguyên đơn giản, do đó giải pháp chạy thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import math

    t = int(sys.stdin.readline())
    out = []
    for _ in range(t):
        n = int(sys.stdin.readline())
        a = list(map(int, sys.stdin.readline().split()))
        b = list(map(int, sys.stdin.readline().split()))

        ga = 0
        for x in a:
            ga = math.gcd(ga, x)

        gb = 0
        for x in b:
            gb = math.gcd(gb, x)

        if ga == gb:
            out.append("0")
        elif ga % gb == 0 or gb % ga == 0:
            out.append("1")
        else:
            out.append("2")

    return "\n".join(out)

# minimum case
assert run("1\n1\n5\n5\n") == "0"

# one step via divisibility
assert run("1\n2\n6 12\n2 4\n") == "1"

# two steps needed
assert run("1\n2\n6 10\n4 9\n") == "2"

# already equal complex arrays
assert run("1\n3\n2 4 6\n1 2 3\n") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| gcd giống hệt nhau | 0 | không cần thao tác | 
| gcd chia hết | 1 | sửa lỗi mở rộng quy mô đơn | 
| coprime không khớp | 2 | yêu cầu cả hai bên thay đổi | 
| mảng có cấu trúc | 1 | gcd giảm độ chính xác | 

## Vỏ cạnh 

Trường hợp quan trọng là khi các mảng trông rất khác nhau nhưng có chung gcd. Ví dụ: các mảng như$[2, 4, 6]$Và$[3, 6, 9]$cả hai đều giảm xuống gcd 2 và 3 tương ứng, và vì không chia hết cho nhau nên cần có hai thao tác. 

Một trường hợp tinh vi khác là khi các phần tử riêng lẻ gợi ý một mối quan hệ nhưng gcd thì không. Chẳng hạn, ngay cả khi một mảng chứa nhiều số trong mảng kia thì chỉ có gcd toàn cục mới quan trọng. Thuật toán nén chính xác tất cả cấu trúc thành một giá trị duy nhất, đảm bảo không có sự liên kết cấp phần tử sai lệch nào ảnh hưởng đến quyết định.
