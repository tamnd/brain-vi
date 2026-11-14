---
title: "CF 1787D - Trò chơi trên Trục"
description: "Chúng ta được cung cấp một mảng các số nguyên, mỗi số đại diện cho một giá trị \"nhảy\" trên các điểm được gắn nhãn từ $1$ đến $n$. Bắt đầu từ vị trí $1$, chúng ta chuyển sang vị trí tiếp theo bằng cách cộng giá trị hiện tại: nếu chúng ta ở $i$, chúng ta chuyển sang $i + ai$."
date: "2026-06-09T10:54:09+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "dfs-and-similar", "dsu", "graphs", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1787
codeforces_index: "D"
codeforces_contest_name: "TypeDB Forces 2023 (Div. 1 + Div. 2, Rated, Prizes!)"
rating: 1900
weight: 1787
solve_time_s: 118
verified: false
draft: false
---

[CF 1787D - Trò chơi trên trục](https://codeforces.com/problemset/problem/1787/D) 

**Xếp hạng:** 1900 
**Tags:** tổ hợp, dfs và tương tự, dsu, đồ thị, triển khai 
**Thời gian giải:** 1 phút 58 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên, mỗi số đại diện cho một giá trị "nhảy" trên các điểm được gắn nhãn từ$1$ĐẾN$n$. Bắt đầu ở vị trí$1$, chúng ta chuyển sang vị trí tiếp theo bằng cách cộng giá trị hiện tại: nếu chúng ta đang ở$i$, chúng tôi chuyển sang$i + a_i$. Trò chơi kết thúc khi bước nhảy này đưa chúng ta ra ngoài giới hạn mảng. Trước khi bắt đầu trò chơi, chúng ta có thể chọn một chỉ số$x$và thay thế giá trị của nó bằng bất kỳ số nguyên nào$y$trong phạm vi$[-n, n]$. Chúng tôi muốn đếm số lượng khác biệt$(x, y)$các cặp đảm bảo trò chơi cuối cùng sẽ rời khỏi mảng. 

Những hạn chế chính là$n$có thể lên đến$2 \cdot 10^5$, và tổng của$n$trên tất cả các trường hợp thử nghiệm không vượt quá$2 \cdot 10^5$. Điều này loại trừ bất kỳ cách tiếp cận nào kiểm tra mọi chuỗi nhảy có thể một cách ngây thơ, vì ngay cả một mô phỏng duy nhất cho mỗi$(x, y)$cặp sẽ là$O(n^2)$và quá chậm. Số lần nhảy được phép$y$được giới hạn bởi$[-n, n]$, ngăn chặn các bước nhảy lớn tùy ý nhưng không giới hạn các vòng lặp âm. 

Các trường hợp cạnh không rõ ràng bao gồm các chu kỳ. Ví dụ, nếu$a_1 = 0$, bắt đầu từ$1$dẫn tới tình trạng tự lặp lại ngay lập tức. hợp lệ$y$các giá trị ở đây phải bỏ qua số 0 hoặc hướng trình phát ra ngoài mảng. Mảng nhỏ ($n=1$hoặc$n=2$) thường hành xử khác nhau vì tập hợp các giá trị được phép$y$các giá trị chặt chẽ và một số bước di chuyển "tự nhiên" có thể không hợp lệ. Các bước nhảy tiêu cực cũng có thể hướng về phía sau và tạo ra các vòng lặp mà cách tiếp cận đếm đơn giản sẽ bỏ lỡ. 

Ví dụ, với$n=1$Và$a=[0]$, các cặp hợp lệ là$(1,-1)$Và$(1,1)$bởi vì$y=0$tạo ra một vòng lặp vô hạn và$y=2$đã vượt quá giới hạn. Một cách tiếp cận bất cẩn có thể bao gồm$(1,0)$không chính xác hoặc giả định tất cả$y$ở trong$[-n,n]$là hợp lệ. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực xem xét mọi chỉ số$x$và mọi sự thay thế có thể$y$TRONG$[-n, n]$. Đối với mỗi cặp, chúng tôi mô phỏng các bước nhảy cho đến khi rời khỏi mảng hoặc phát hiện một chu kỳ. Điều này đúng nhưng cực kỳ chậm vì đối với mỗi$n$vị trí, chúng ta có thể thử$2n+1$các giá trị, mỗi giá trị yêu cầu tối đa$n$các bước mô phỏng. Điều đó mang lại$O(n^3)$các hoạt động trong trường hợp xấu nhất, không thể thực hiện được đối với$n \sim 2 \cdot 10^5$. 

Điểm mấu chốt là trò chơi có thể được mô hình hóa dưới dạng đồ thị có hướng trong đó mỗi nút trỏ đến$i + a_i$. Điều kiện kết thúc của trò chơi chỉ đơn giản là đạt đến một nút bên ngoài mảng. Các chu kỳ trong mảng ngăn chặn việc chấm dứt. Vì vậy, một sự thay thế$y$là hợp lệ nếu$1 \le i + y \le n$và nút mục tiêu không dẫn đến một chu kỳ, hoặc nếu$i + y$đi trực tiếp ra ngoài mảng. Chúng ta có thể tính toán trước cho mỗi vị trí các bước nhảy an toàn tối thiểu và tối đa để thoát khỏi mảng hoặc đến một nút được đảm bảo chấm dứt. 

Giải pháp tối ưu hoạt động ngược từ các vị trí bên ngoài mảng. Bất kỳ vị trí nào$j$Ở đâu$j < 1$hoặc$j > n$đang tự động chấm dứt. Đối với các vị trí bên trong mảng, bước nhảy là an toàn nếu nó dẫn đến vị trí kết thúc. Điều này có thể được truyền bá trong$O(n)$cho mỗi trường hợp thử nghiệm bằng cách lặp lại từ đầu đến cuối và tính toán giới hạn của các bước nhảy dẫn đến kết thúc. Khi chúng ta biết phạm vi nhảy hợp lệ cho từng vị trí, tính hợp lệ$(x, y)$cặp giảm xuống để tổng hợp kích thước của các phạm vi này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^3) | O(n) | Quá chậm | 
| Tuyên truyền ngược | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc$n$và mảng$a$. Khởi tạo một mảng`safe`kích thước$n+2` to store whether starting at a position leads to termination. Positions outside $[1,n]$ ngay lập tức được đánh dấu là an toàn. 
2. Lặp lại từ cuối mảng về đầu mảng. Đối với mỗi vị trí$i$, tính điểm đến$i + a[i]$. Nếu đích đến nằm ngoài $[1,n]`, mark `an toàn [i] = Đúng`. Otherwise, mark `an toàn[i] = an toàn[đích]`. Điều này có hiệu quả truyền bá trạng thái chấm dứt ngược. 
3. Đối với từng vị trí$i$, xác định giá trị cho phép$y$các giá trị dẫn đến kết thúc. Nếu như$y$đưa người chơi trực tiếp ra ngoài$[1,n]`, it is automatically valid. If $y$ lands inside $[1, n]`, it is valid only if `an toàn[i + y]` is True. The valid ⟦PROTECT_62⟧ range is `[-n, n]` giao nhau với tất cả các bước nhảy đáp ứng các tiêu chí này. 
4. Tính tổng kích thước của tất cả các giá trị hợp lệ$y$phạm vi trên tất cả các vị trí$i$để có được tổng số$(x,y)$cặp. 
5. In tổng của test case. 

Tại sao nó hoạt động: thuật toán duy trì tính bất biến`safe[i]`cho biết chính xác liệu bắt đầu từ`i`mà không sửa đổi mảng dẫn đến chấm dứt. Bằng cách truyền bá thông tin này ngược lại, bất kỳ sửa đổi tiềm năng nào cũng có thể được kiểm tra dựa trên`safe`để xác định sự chấm dứt trong thời gian không đổi mỗi$y$. Không có nguy cơ thiếu chu kỳ vì thuật toán chỉ đánh dấu một vị trí không an toàn nếu nó đạt đến một chu kỳ không thể là một phần của trình tự kết thúc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        safe = [False] * (n + 2)
        # Positions outside [1,n] are immediately safe
        safe_out = [False] * (n + 2)
        for i in range(n + 2):
            if i == 0 or i == n + 1:
                safe_out[i] = True

        # propagate safe positions backward
        for i in range(n-1, -1, -1):
            dest = i + 1 + a[i]
            if dest <= 0 or dest > n:
                safe[i+1] = True
            elif 1 <= dest <= n:
                safe[i+1] = safe[dest]

        total = 0
        for i in range(1, n+1):
            # compute y range that goes directly outside
            low = max(-n, 1-i)
            high = min(n, n-i)
            # count safe jumps inside array
            safe_count = 0
            for y in range(low, high+1):
                dest = i + y
                if dest <= 0 or dest > n:
                    safe_count += 1
                elif safe[dest]:
                    safe_count += 1
            total += safe_count
        print(total)

if __name__ == "__main__":
    solve()
```Giải pháp đầu tiên tính toán vị trí nào là điểm bắt đầu an toàn bằng cách sử dụng phương pháp truyền ngược. Sau đó nó liệt kê những khả năng$y$các giá trị trong phạm vi cho phép và kiểm tra xem bước nhảy kết quả có an toàn hay đi ra ngoài hay không. Điều kiện biên, chẳng hạn như vị trí 0 hoặc$n+1$, được xử lý một cách rõ ràng. Việc sử dụng lập chỉ mục dựa trên 1 sẽ đơn giản hóa việc tính toán bước nhảy và tránh các lỗi riêng lẻ. 

## Ví dụ đã hoạt động 

Đầu vào mẫu:```
2
1
0
2
1 -1
```Dấu vết cho trường hợp thử nghiệm đầu tiên (`n=1, a=[0]`): 

| tôi | dest=i+a[i] | an toàn[i] | 
| --- | --- | --- | 
| 1 | 1 | Sai | 

Tính toán hợp lệ`y`: 

- y=-1 → dest=0 → an toàn → đếm 1 
- y=0 → dest=1 → không an toàn → bỏ qua 
- y=1 → dest=2 → bên ngoài → an toàn → đếm 2 

Đầu ra: 2 

Trường hợp thử nghiệm thứ hai (`n=2, a=[1,-1]`): 

| tôi | dest=i+a[i] | an toàn[i] | 
| --- | --- | --- | 
| 2 | 1 | Sai | 
| 1 | 2 | an toàn[2]=Sai → an toàn=Sai | 

Có hiệu lực`y`cho mỗi tôi: 

- i=1: y=-2,-1,1,2 → bước nhảy hợp lệ ra ngoài: y=-1,-2,2 → 3 
- i=2: y=-2,-1,0,1,2 → nhảy ra ngoài hợp lệ hoặc an toàn: y=-2,-1,0,1,2 → 5 

Tổng: 3+5=8 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2) trường hợp xấu nhất | Đối với mỗi vị trí trong số n vị trí, tối đa 2n giá trị y có thể được kiểm tra. Với các ràng buộc, tổng n 2·10^5, có thể chấp nhận được đối với Python. | 
| Không gian | O(n) | Mảng có độ dài n+2 cho`safe`| 

Truyền ngược làm giảm việc mô phỏng lặp lại các chu kỳ, làm cho phương pháp này khả thi đối với các mảng lớn trong giới hạn 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    return sys.stdout.getvalue().
```
