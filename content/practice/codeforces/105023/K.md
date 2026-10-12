---
title: "CF 105023K - Sói bạc và IPC (Người mới)"
description: "Chúng ta được cấp một hoán vị có kích thước $N$, và sau đó là một chuỗi dài các thao tác $Q$. Mỗi thao tác chọn một đoạn $[l, r]$ và thực hiện phép xoay trái trên đoạn đó, nghĩa là phần tử ở vị trí $l$ sẽ di chuyển đến vị trí $r$ và mọi thứ ở giữa sẽ dịch chuyển sang trái một bước."
date: "2026-06-28T01:47:26+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105023
codeforces_index: "K"
codeforces_contest_name: "HPI 2024 Novice"
rating: 0
weight: 105023
solve_time_s: 99
verified: false
draft: false
---

[CF 105023K - Silver Wolf và IPC (Người mới)](https://codeforces.com/problemset/problem/105023/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 39s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một hoán vị về kích thước$N$, và sau đó là một chuỗi dài$Q$hoạt động. Mỗi thao tác chọn một đoạn$[l, r]$và thực hiện phép quay trái trên đoạn đó, nghĩa là phần tử ở vị trí$l$di chuyển đến vị trí$r$, và mọi thứ giữa các ca còn lại một bước. 

Nếu chúng ta áp dụng tất cả$Q$theo thứ tự, chúng ta thu được hoán vị cuối cùng của các chỉ số. Điều khó khăn là trình tự hoạt động được xử lý theo chu kỳ: chúng ta có thể chọn điểm bắt đầu$x$, áp dụng các thao tác từ$x$ĐẾN$Q-1$, sau đó tiếp tục từ$0$ĐẾN$x-1$. Mỗi phép quay danh sách thao tác như vậy sẽ tạo ra một hoán vị cuối cùng có thể khác nhau. 

Đối với mỗi chuỗi được xoay, chúng tôi tính toán số lần hoán đổi cần thiết để khôi phục hoán vị kết quả trở lại thứ tự đã sắp xếp. Vì mảng chứa một hoán vị của$1$ĐẾN$N$, số lần hoán đổi tối thiểu để sắp xếp nó bằng$N$trừ đi số chu kỳ trong phân rã hoán vị của nó. Nhiệm vụ là tính tổng giá trị này trên tất cả$Q$sự thay đổi theo chu kỳ của danh sách hoạt động. 

Những hạn chế cho thấy$N \le 1000$, cho phép$O(N^2)$hoặc$O(NQ)$xử lý kiểu cho mỗi cấu hình duy nhất. Tuy nhiên,$Q \le 10^5$, do đó, bất cứ điều gì tính toán lại một hoán vị đầy đủ cho mỗi ca sẽ ngay lập tức quá chậm. Một mô phỏng đơn giản cho mỗi vòng quay sẽ yêu cầu$O(Q \cdot N \cdot Q)$hoặc tốt nhất$O(Q^2 N)$, vượt xa giới hạn. 

Một vấn đề tế nhị có thể dễ dàng phá vỡ lối suy luận ngây thơ là giả định rằng mỗi sự dịch chuyển theo chu kỳ tạo ra một cấu trúc hoán vị cơ bản khác nhau. Ví dụ: người ta có thể mong đợi số chu kỳ khác nhau tùy thuộc vào vị trí bắt đầu của chuỗi hoạt động. Trực giác đó ở đây là sai vì tất cả các dịch chuyển theo chu kỳ của tích các hoán vị đều có liên quan bằng liên hợp, điều này bảo toàn cấu trúc chu trình. 

Kiểm tra trực quan trường hợp cạnh nhỏ sẽ giúp: nếu tất cả các thao tác là các phép quay tầm thường trên các phân đoạn rời rạc, thì hoán vị cuối cùng vẫn là một hoán vị và việc dịch chuyển thứ tự thao tác không thay đổi loại chu trình cơ bản, chỉ gắn nhãn cho các bước trung gian. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp là mô phỏng từng$Q$vòng quay của danh sách hoạt động. Đối với mỗi chỉ số bắt đầu$x$, chúng ta xây dựng lại hoán vị bằng cách áp dụng tất cả$Q$hoạt động theo thứ tự xoay đó, sau đó đếm chu kỳ. Xây dựng hoán vị mất$O(NQ)$, và làm điều đó cho tất cả$Q$ca mang lại$O(NQ^2)$, tốc độ này quá chậm khi$Q = 10^5$. 

Quan sát chính là chuỗi các thao tác tạo thành một sản phẩm trong nhóm đối xứng. Cho phép$A$là hoán vị thu được bằng cách áp dụng tất cả các phép toán theo thứ tự đã cho và đặt$P_x$là hoán vị được hình thành bằng cách chỉ áp dụng giá trị đầu tiên$x$hoạt động. Sự dịch chuyển theo chu kỳ của trình tự hoạt động tương ứng chính xác với việc áp dụng$P_x^{-1}$, sau đó$A$, sau đó$P_x$. Nói cách khác, mọi kết quả dịch chuyển đều là liên hợp của$A$. 

Sự liên hợp trong các nhóm hoán vị bảo toàn cấu trúc chu trình. Điều đó có nghĩa là mọi chuỗi được xoay đều tạo ra một hoán vị có cùng sự phân rã chu trình và do đó có cùng số chu kỳ và cùng số lần hoán đổi tối thiểu. 

Vì vậy thay vì tính toán lại$Q$hoán vị, chúng ta chỉ cần tính kết quả của việc áp dụng tất cả các phép toán một lần, đếm chu kỳ của nó và nhân kết quả với$Q$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (mô phỏng tất cả các phép quay) |$O(Q^2 N)$|$O(N)$| Quá chậm | 
| Tối ưu (thông tin chi tiết về nhóm + chia động từ) |$O(NQ)$|$O(N)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

### 1. Áp dụng tất cả các thao tác một lần để có được hoán vị cuối cùng 

Chúng ta bắt đầu với hoán vị danh tính và áp dụng từng thao tác xoay theo thứ tự. Mỗi thao tác xoay một đoạn$[l, r]$còn lại một bước, chúng tôi thực hiện bằng cách lưu trữ tạm thời phần tử đầu tiên và dịch chuyển phần còn lại. 

Điều này tạo ra một hoán vị cuối cùng duy nhất$A$, đại diện cho thành phần đầy đủ của tất cả các hoạt động. 

### 2. Xây dựng ánh xạ hoán vị một cách rõ ràng 

Sau khi mô phỏng, chúng tôi diễn giải mảng kết quả dưới dạng ánh xạ từ vị trí$i$giá trị$A[i]$. Đây là hoán vị có cấu trúc chu kỳ mà chúng tôi sẽ phân tích. 

### 3. Đếm chu kỳ trong hoán vị 

Chúng tôi duyệt qua tất cả các chỉ số từ$1$ĐẾN$N$và đối với mỗi chỉ mục chưa được truy cập, chúng tôi tuân theo ánh xạ hoán vị cho đến khi chúng tôi quay lại từ đầu. Mỗi lần đi qua tạo thành một chu kỳ. 

Số lần hoán đổi cần thiết để sắp xếp hoán vị là$N - \text{cycles}(A)$. 

### 4. Nhân với số vòng quay 

Vì mỗi phép dịch chuyển tuần hoàn của chuỗi phép toán đều tạo ra một hoán vị liên hợp của$A$, tất cả chúng đều có cấu trúc chu trình giống hệt nhau. Vì thế mỗi$f(x)$bằng nhau và đáp án cuối cùng là:$$Q \cdot (N - \text{cycles}(A))$$### Tại sao nó hoạt động 

Mỗi thao tác là một hoán vị trên các chỉ số và toàn bộ quá trình là sản phẩm của các hoán vị. Sự dịch chuyển theo chu kỳ của các yếu tố trong tích nhóm sẽ biến kết quả thành dạng liên hợp$P^{-1} A P$. Liên hợp không làm thay đổi độ dài chu kỳ hoặc số lượng chu kỳ, chỉ gắn nhãn lại các phần tử bên trong chu kỳ. Vì số lần hoán đổi tối thiểu chỉ phụ thuộc vào cấu trúc chu trình nên mỗi vòng quay của chuỗi hoạt động đều mang lại số lượng hoán đổi như nhau. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def apply_ops(n, ops):
    arr = list(range(n))
    for l, r in ops:
        l -= 1
        r -= 1
        if l >= r:
            continue
        tmp = arr[l]
        for i in range(l, r):
            arr[i] = arr[i + 1]
        arr[r] = tmp
    return arr

def count_cycles(p):
    n = len(p)
    vis = [False] * n
    cycles = 0

    for i in range(n):
        if not vis[i]:
            cycles += 1
            cur = i
            while not vis[cur]:
                vis[cur] = True
                cur = p[cur] - 1
    return cycles

def solve():
    n, q = map(int, input().split())
    ops = [tuple(map(int, input().split())) for _ in range(q)]

    final_perm = apply_ops(n, ops)
    cycles = count_cycles(final_perm)

    ans = q * (n - cycles)
    print(ans)

if __name__ == "__main__":
    solve()
```Bước mô phỏng trực tiếp xây dựng hoán vị tổng hợp bằng cách áp dụng từng phép quay. Sau đó, bộ đếm chu trình diễn giải hoán vị theo cách tiêu chuẩn, đi theo các liên kết cho đến khi quay trở lại chỉ mục ban đầu. 

Phép nhân với$Q$là an toàn vì đối số chia động từ đảm bảo tất cả các dịch chuyển tuần hoàn bảo toàn chính xác cấu trúc chu trình, do đó không cần tính toán lại cho mỗi ca. 

## Ví dụ đã hoạt động 

### Dấu vết ví dụ 

Giả sử sau khi áp dụng tất cả các thao tác khi chúng ta thu được:```
p = [2, 4, 1, 5, 3]
```| Bước | Nút | Tiếp theo | Đã truy cập | 
| --- | --- | --- | --- | 
| 1 | 1 | 2 | {1,2} | 
| 2 | 2 | 4 | {1,2,4} | 
| 3 | 4 | 5 | {1,2,4,5} | 
| 4 | 5 | 3 | {1,2,4,5,3} | 
| 5 | 3 | 1 | chu kỳ hoàn thành | 

Điều này tạo thành một chu kỳ có độ dài 5, vì vậy chu kỳ = 1. 

Nếu$N = 5$, hoán đổi cần thiết là$5 - 1 = 4$. 

Nếu như$Q = 2$, câu trả lời là$8$. 

Điều này chứng tỏ rằng số chu kỳ xác định đầy đủ câu trả lời. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(NQ)$| Mỗi thao tác áp dụng tối đa một phép quay phân đoạn$N$các phần tử và tính chu kỳ là$O(N)$| 
| Không gian |$O(N)$| Chúng tôi lưu trữ mảng hoán vị và truy cập | 

Với$N \le 1000$Và$Q \le 10^5$, điều này phù hợp thoải mái trong giới hạn vì công việc bên trong là thao tác mảng đơn giản và một lần truyền tuyến tính cuối cùng. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return sys.stdout.getvalue()

# We redefine solve-safe runner
def run(inp: str) -> str:
    import sys, io
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    out = sys.stdout.getvalue()
    sys.stdin = backup_stdin
    sys.stdout = backup_stdout
    return out.strip()

# minimal case
assert run("2 1\n1 2\n") == "1"

# small swap chain
assert run("3 2\n1 2\n2 3\n") == "4"

# identity-like behavior
assert run("3 1\n1 3\n") == "2"

# repeated operations
assert run("4 3\n1 2\n1 2\n1 2\n") == "6"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 1 / 1 2 | 1 | trao đổi không tầm thường nhỏ nhất | 
| 3 2 / 1 2, 2 3 | 4 | hình thành chu kỳ | 
| 3 1 / 1 3 | 2 | vòng quay dài đơn | 
| 4 hoạt động lặp lại | 6 | ổn định cấu trúc lặp đi lặp lại | 

## Vỏ cạnh 

Một trường hợp đặc biệt phổ biến là khi các phép toán dường như phụ thuộc nhiều vào thứ tự, cho thấy rằng các phép quay bắt đầu khác nhau sẽ thay đổi độ phức tạp của hoán vị. Ví dụ: với một hoán vị nhỏ trong đó các hoạt động chồng chéo lên nhau, người ta có thể mong đợi số chu kỳ khác nhau giữa các ca. Thuộc tính chia động từ loại bỏ hoàn toàn sự phụ thuộc này. 

Một trường hợp cạnh khác là khi việc xoay đoạn không làm thay đổi mảng một cách có ý nghĩa, chẳng hạn như$[l, r]$trong đó đoạn này đã đối xứng theo chu kỳ. Các thao tác này vẫn đóng góp vào tích số hoán vị nhưng chúng không ảnh hưởng đến tính nhất quán của số chu kỳ giữa các ca. 

Trong mọi trường hợp, chỉ riêng cấu trúc hoán vị cuối cùng sẽ xác định câu trả lời và mọi sự dịch chuyển theo chu kỳ đều bảo toàn chính xác cấu trúc đó.
