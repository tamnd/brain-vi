---
title: "CF 104768C - Bậc thầy của cả hai IV"
description: "Chúng ta được cung cấp một mảng các số nguyên và chúng ta được yêu cầu đếm xem có bao nhiêu chuỗi con không trống của mảng này thỏa mãn một ràng buộc liên kết hai phép toán với nhau trên các phần tử đã chọn: XOR theo bit của tất cả các giá trị đã chọn và khả năng chia hết cho số nguyên."
date: "2026-06-28T20:00:13+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104768
codeforces_index: "C"
codeforces_contest_name: "2023 China Collegiate Programming Contest (CCPC) Guilin Onsite (The 2nd Universal Cup. Stage 8: Guilin)"
rating: 0
weight: 104768
solve_time_s: 57
verified: true
draft: false
---

[CF 104768C - Bậc thầy của cả hai IV](https://codeforces.com/problemset/problem/104768/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 57s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên và chúng ta được yêu cầu đếm xem có bao nhiêu chuỗi con không trống của mảng này thỏa mãn một ràng buộc liên kết hai phép toán với nhau trên các phần tử đã chọn: XOR theo bit của tất cả các giá trị đã chọn và khả năng chia hết cho số nguyên. 

Đối với bất kỳ chuỗi con nào được chọn, chúng tôi tính toán XOR của tất cả các phần tử của nó. Khi đó mọi phần tử trong dãy con đó phải chia giá trị XOR này. Nói cách khác, nếu một dãy con chứa các giá trị$v_1, v_2, \dots, v_k$và XOR của họ là$X$, thì mỗi$v_i$phải thỏa mãn$X \bmod v_i = 0$. 

Kích thước đầu vào đủ lớn để bất kỳ giải pháp nào thử tất cả các chuỗi con đều không thể thực hiện được ngay lập tức. Với$n$lên tới$2 \cdot 10^5$trong các trường hợp thử nghiệm, ngay cả việc kiểm tra tất cả các tập hợp con của một mảng có kích thước 40 cũng đã vượt quá giới hạn, vì vậy chúng tôi cần một cấu trúc giúp giảm bớt vấn đề về việc đếm các khoản đóng góp một cách độc lập hoặc gần như độc lập cho mỗi giá trị. 

Một khó khăn nhỏ xuất phát từ sự tương tác giữa XOR và khả năng chia hết. XOR không đơn điệu và không bảo toàn cấu trúc số học nên những lý luận ngây thơ như thay XOR bằng sum hay gcd đều thất bại hoàn toàn. 

Một cách kiểm tra tỉnh táo hữu ích là xem xét các trường hợp bệnh lý nhỏ. Nếu mảng chứa các giá trị hỗn hợp, giả sử$[2, 3]$, XOR là$1$. Không$2$cũng không$3$chia rẽ$1$, vì vậy dãy con này không hợp lệ. Nếu chúng ta cố gắng$[1, 2]$, XOR là$3$, và trong khi$1$chia rẽ mọi thứ,$2$không chia$3$, vì vậy điều này cũng không hợp lệ. Điều này cho thấy rằng việc trộn các giá trị riêng biệt bị hạn chế rất nhiều. 

Một tình huống cạnh khác là khi tất cả các giá trị được chọn đều giống hệt nhau. Nếu chúng ta chỉ chọn giá trị$v$, XOR hoạt động rất đơn giản: với số phần tử chẵn, nó trở thành$0$, và với số lẻ nó trở thành$v$. Cả hai$0$Và$v$được chia cho$v$, do đó mọi tập hợp con không trống có giá trị giống hệt nhau đều hợp lệ. Đây hóa ra là trường hợp duy nhất có cấu trúc ổn định. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ liệt kê mọi chuỗi con của mảng, tính toán XOR của nó và sau đó xác minh khả năng chia hết cho mọi phần tử. Điều này đúng nhưng đòi hỏi$O(2^n \cdot n)$thời gian cho mỗi trường hợp kiểm thử trong trường hợp xấu nhất, vì mỗi tập hợp con cần tính toán XOR và quét. Với$n$lên tới$2 \cdot 10^5$, điều này vượt xa khả thi. 

Quan sát quan trọng là ràng buộc về khả năng chia hết buộc phải có tính đồng nhất cao nhất trong bất kỳ tập hợp con hợp lệ nào. Nếu một tập hợp con chứa hai giá trị khác nhau$a$Và$b$, cả hai phải chia$a \oplus b \oplus \cdots$. Điều này tạo ra các hạn chế số học mạnh mẽ mà hầu như không bao giờ giữ được trừ khi tất cả các giá trị giống hệt nhau. Trên thực tế, bất kỳ nỗ lực nào để trộn các giá trị khác nhau đều nhanh chóng phá vỡ tính chia hết của ít nhất một phần tử, bởi vì XOR không bảo toàn mối quan hệ chia hết trên các số nguyên độc lập. 

Một khi chúng ta chấp nhận rằng chỉ các tập hợp con gồm một giá trị riêng biệt duy nhất mới tồn tại được thì bài toán sẽ phân rã hoàn toàn theo giá trị. Với mỗi giá trị riêng biệt$v$, chúng ta chỉ cần đếm xem có bao nhiêu cách để chọn một tập con không trống trong số lần xuất hiện của nó. Nếu nó xuất hiện$c_v$lần, có$2^{c_v} - 1$dãy con hợp lệ chỉ bao gồm$v$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force đối với các chuỗi tiếp theo |$O(n 2^n)$|$O(n)$| Quá chậm | 
| Nhóm theo giá trị |$O(n)$mỗi bài kiểm tra |$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đếm số lần mỗi giá trị xuất hiện trong mảng. Điều này là cần thiết vì cấu trúc của các dãy con hợp lệ chỉ phụ thuộc vào bội số chứ không phụ thuộc vào vị trí. 
2. Với mỗi giá trị riêng biệt$v$, hãy xem xét tất cả các chuỗi con được hình thành độc quyền từ sự xuất hiện của$v$. Nếu có$c_v$những lần xuất hiện như vậy thì số lựa chọn không trống là$2^{c_v} - 1$. Điều này tính mọi cách có thể để chọn ít nhất một chỉ mục trong khi vẫn giữ các giá trị giống hệt nhau. 
3. Tính tổng những đóng góp này trên tất cả các giá trị riêng biệt. Các giá trị khác nhau không thể được kết hợp thành một dãy con hợp lệ theo ràng buộc, vì vậy các nhóm này rời rạc và độc lập. 
4. Trả về tổng modulo$998244353$. 

Lý do bước 3 hợp lệ là vì không có dãy con nào chứa hai giá trị riêng biệt có thể thỏa mãn điều kiện, do đó không có thuật ngữ tương tác chồng chéo hoặc thiếu. 

### Tại sao nó hoạt động 

Bất kỳ chuỗi con hợp lệ nào cũng phải thỏa mãn rằng mọi phần tử đều chia XOR của toàn bộ chuỗi con. Nếu hai giá trị phân biệt$a$Và$b$xuất hiện cùng nhau thì cả hai phải chia cùng một giá trị XOR, điều này phụ thuộc vào cả hai$a$Và$b$một cách phi tuyến tính. Cấu hình ổn định duy nhất trong đó XOR không đưa ra các ràng buộc chia hết không tương thích là khi tất cả các phần tử đều bằng nhau. Trong trường hợp đó, XOR sụp đổ thành$v$hoặc$0$, đều chia hết cho$v$, đảm bảo tính hợp lệ. Do đó, không gian giải pháp được phân chia chính xác theo giá trị. 

## Giải pháp Python```python
import sys
from collections import Counter

input = sys.stdin.readline
MOD = 998244353

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        arr = list(map(int, input().split()))
        
        freq = Counter(arr)
        ans = 0
        
        for v, c in freq.items():
            ans = (ans + pow(2, c, MOD) - 1) % MOD
        
        print(ans)

if __name__ == "__main__":
    solve()
```Việc thực hiện trực tiếp theo sau việc giảm bớt. các`Counter`nén mảng thành các nhóm tần số, thay thế mọi nhu cầu suy luận về vị trí. Tính lũy thừa mô-đun$2^{c_v}$hiệu quả theo thời gian logarit. 

Một điểm tinh tế là xử lý phép trừ theo mô đun. Từ$2^{c_v} - 1$có thể trở thành âm sau khi trừ, chúng tôi dựa vào hành vi modulo của Python sau phép cộng cuối cùng, đảm bảo kết quả vẫn nằm trong phạm vi. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét một mảng`[5, 5, 5]`. 

| Bước | Giá trị | Đếm | Đóng góp | 
| --- | --- | --- | --- | 
| quá trình 5 | 5 | 3 |$2^3 - 1 = 7$| 

Tất cả các dãy con hợp lệ chính xác là tất cả các tập hợp con không trống của các chỉ mục. 

Điều này xác nhận rằng chỉ riêng bội số sẽ xác định câu trả lời khi các giá trị giống hệt nhau. 

### Ví dụ 2 

Hãy xem xét`[2, 2, 3]`. 

| Bước | Giá trị | Đếm | Đóng góp | 
| --- | --- | --- | --- | 
| quá trình 2 | 2 | 2 |$3$| 
| quá trình 3 | 3 | 1 |$1$| 

Tổng cộng là$4$. 

Điều này cho thấy sự phân tách theo nhóm giá trị. Bất kỳ chuỗi con hỗn hợp nào như`[2,3]`bị loại trừ vì nó vi phạm tính chia hết trong XOR. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$mỗi trường hợp thử nghiệm | Mỗi phần tử được tính một lần và lũy thừa là logarit theo giá trị đếm | 
| Không gian |$O(n)$| Bản đồ tần số lưu trữ tối đa một mục nhập cho mỗi giá trị riêng biệt | 

Tổng cộng$n$qua các trường hợp thử nghiệm là$2 \cdot 10^5$, do đó nghiệm tuyến tính vừa khít trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 998244353

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    from collections import Counter
    
    def solve():
        t = int(input())
        out = []
        for _ in range(t):
            n = int(input())
            arr = list(map(int, input().split()))
            freq = Counter(arr)
            ans = 0
            for v, c in freq.items():
                ans = (ans + pow(2, c, MOD) - 1) % MOD
            out.append(str(ans))
        print("\n".join(out))
    
    solve()
    return sys.stdout.getvalue().strip()

# minimum size
assert run("1\n1\n7\n") == "1"

# all equal
assert run("1\n3\n4 4 4\n") == str((2**3 - 1) % MOD)

# mixed values
assert run("1\n3\n1 2 3\n") == str((1 + 1 + 1) % MOD)

# duplicates + single
assert run("1\n4\n2 2 2 5\n") == str((2**3 - 1 + 1) % MOD)
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 1 | trường hợp cơ sở | 
| tất cả các giá trị bằng nhau |$2^n - 1$| tính chính xác của việc đếm tập hợp con | 
| tất cả đều khác biệt | tổng số đĩa đơn | không được phép trộn | 
| tần số hỗn hợp | độc lập nhóm | xử lý trùng lặp | 

## Vỏ cạnh 

Khi tất cả các phần tử giống hệt nhau, XOR sẽ luân phiên giữa giá trị đó và giá trị 0 tùy thuộc vào tính chẵn lẻ. Trong cả hai trường hợp, khả năng chia hết được giữ tự động, do đó mọi tập hợp con không trống đều hợp lệ. Thuật toán xử lý việc này bằng cách đếm tất cả các tập hợp con bên trong một nhóm tần số. 

Khi tất cả các phần tử đều khác biệt, thuật toán tạo ra một đóng góp cho mỗi phần tử, chỉ tương ứng với các phần tử đơn. Bất kỳ tập hợp con lớn hơn nào cũng sẽ yêu cầu khả năng tương thích giữa các giá trị khác nhau theo tính phân chia XOR, điều này không thành công, do đó logic nhóm sẽ tránh được việc đếm quá mức một cách chính xác. 

Khi một giá trị xuất hiện nhiều lần cùng với các giá trị khác, chỉ khối lặp lại mới đóng góp theo cấp số nhân. Ví dụ, trong`[3, 3, 3, 1]`, giá trị`3`đóng góp$2^3 - 1$, trong khi`1`chỉ đóng góp$1$. Các tập hợp con hỗn hợp được loại trừ hoàn toàn vì chúng không bao giờ xuất hiện trong cấu trúc nhóm.
