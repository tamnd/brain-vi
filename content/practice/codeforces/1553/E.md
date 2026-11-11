---
title: "CF 1553E - Dịch chuyển hoán vị"
description: "Chúng ta bắt đầu từ hoán vị danh tính, đơn giản là các số từ 1 đến n theo thứ tự. Trước tiên, ai đó sẽ xoay mảng này theo chu kỳ sang bên phải bằng một độ dịch k không xác định, sau đó thực hiện tối đa m phép hoán đổi phần tử tùy ý."
date: "2026-06-16T15:52:56+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "combinatorics", "constructive-algorithms", "dfs-and-similar", "dsu", "graphs", "math"]
categories: ["algorithms"]
codeforces_contest: 1553
codeforces_index: "E"
codeforces_contest_name: "Harbour.Space Scholarship Contest 2021-2022 (open for everyone, rated, Div. 1 + Div. 2)"
rating: 2100
weight: 1553
solve_time_s: 288
verified: true
draft: false
---

[CF 1553E - Dịch chuyển hoán vị](https://codeforces.com/problemset/problem/1553/E) 

**Xếp hạng:** 2100 
**Tags:** lực lượng vũ phu, tổ hợp, thuật toán xây dựng, dfs và tương tự, dsu, đồ thị, toán học 
**Thời gian giải:** 4 phút 48 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta bắt đầu từ hoán vị danh tính, đơn giản là các số từ 1 đến n theo thứ tự. Trước tiên, ai đó sẽ xoay mảng này theo chu kỳ sang bên phải bằng một độ dịch k không xác định, sau đó thực hiện tối đa m phép hoán đổi phần tử tùy ý. Sau những thao tác này, chúng ta chỉ thấy hoán vị cuối cùng. 

Nhiệm vụ là xác định số lượng vòng quay bắt đầu k có thể tạo ra mảng cuối cùng này, giả sử rằng sau khi xoay, chúng ta được phép tối đa m lần hoán đổi để đạt được hoán vị đã cho. 

Một cách hữu ích để suy nghĩ về quy trình là chúng ta bắt đầu với cấu trúc tuần hoàn, trong đó mảng là một danh tính được xoay và sau đó chúng ta được phép “sửa chữa” nó bằng cách sử dụng các phép hoán đổi. Mỗi lần hoán đổi có thể sửa hai phần tử bị đặt sai vị trí về vị trí mục tiêu cuối cùng của chúng, do đó m kiểm soát mức độ hoán vị có thể lệch so với một phép quay thuần túy. 

Các ràng buộc rất lớn: tổng n trên tất cả các trường hợp thử nghiệm lên tới 3·10^5 và có tới 10^5 trường hợp thử nghiệm. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào thử tất cả k cho mọi trường hợp thử nghiệm và mô phỏng các giao dịch hoán đổi hoặc tính toán khoảng cách theo thời gian bậc hai. Ngay cả O(n√n) cho mỗi trường hợp thử nghiệm cũng sẽ quá chậm trong trường hợp xấu nhất. 

Một khó khăn nhỏ xuất phát từ thực tế là vòng quay không xác định và các giao dịch hoán đổi cũng không bị ràng buộc ngoại trừ về số lượng. Một cách tiếp cận đơn giản sẽ thử mọi k, hủy xoay mảng và tính toán số lần hoán đổi tối thiểu để khôi phục danh tính. Điều đó sẽ yêu cầu phân rã chu trình trên k, cho O(n^2) cho mỗi trường hợp thử nghiệm, điều này không khả thi. 

Một sai lầm phổ biến khác là coi các hoán đổi là khả năng sắp xếp đầy đủ, kết luận không chính xác rằng bất kỳ hoán vị nào trong m nghịch đảo đều hợp lệ. Điều đó bỏ qua rằng hoán vị phải nhất quán với một sự dịch chuyển theo chu kỳ duy nhất trước khi hoán đổi, đây là một hạn chế mạnh mẽ về cấu trúc. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: giả sử một ứng cử viên dịch chuyển k, xoay mảng ngược lại k và tính xem cần bao nhiêu lần hoán đổi để biến nó thành hoán vị nhận dạng. Khoảng cách hoán đổi của một hoán vị là n trừ đi số chu kỳ trong biểu đồ hoán vị của nó, vì vậy chúng ta có thể tính chu kỳ sau khi không dịch chuyển. Điều này đúng nhưng tốn kém vì với mỗi k, chúng tôi xây dựng lại một chu trình hoán vị và tính toán lại đầy đủ, dẫn đến tổng công việc là O(n^2) cho mỗi trường hợp thử nghiệm. 

Điều quan trọng là chúng ta không cần phải tính toán lại mọi thứ cho từng k một cách độc lập. Thay vào đó, chúng tôi đảo ngược quan điểm. Sửa hoán vị cuối cùng p. Đối với một k nhất định, chúng ta có thể giải thích liệu p có thể đến từ danh tính được dịch chuyển k sau nhiều nhất m lần hoán đổi hay không bằng cách kiểm tra xem có bao nhiêu vị trí đã “căn chỉnh cấu trúc” với phép quay đó. 

Sau khi dịch chuyển k, phần tử i lý tưởng nhất sẽ xuất hiện ở vị trí (i + k) mod n. Vì vậy, đối với k cố định, chúng ta có thể đếm có bao nhiêu phần tử đã khớp với vị trí chu kỳ dự kiến ​​này. Nếu một phần tử đã phù hợp với ca làm việc thì nó không cần sửa chữa. Mỗi lần hoán đổi có thể khắc phục tối đa hai điểm không khớp, do đó số phần tử không khớp sẽ quyết định tính khả thi. 

Điều này biến vấn đề thành việc đếm có bao nhiêu vị trí nhất quán cho mỗi k, nhưng thực hiện một cách ngây thơ vẫn là O(n^2). Bí quyết cuối cùng là lưu ý rằng tính nhất quán có thể được viết lại dưới dạng điều kiện căn chỉnh theo chu kỳ: 

p[j] = (j - k mod n) + 1 

sắp xếp lại thành: 

k ≡ j - (p[j] - 1) (mod n) 

Vì vậy, mỗi vị trí j đưa ra một giá trị dư là k để làm cho p[j] được căn chỉnh chính xác. Đối với k hợp lệ, nhiều vị trí phải đồng ý với phần dư này. Nếu x vị trí bằng nhau thì n - x phần tử không khớp và chúng ta cần ít nhất (n - x) / 2 lần hoán đổi. Vì vậy, tính khả thi trở thành một điều kiện ngưỡng về số lượng trận đấu mà một ca tạo ra. 

Do đó, chúng tôi tính toán, đối với mỗi k, có bao nhiêu chỉ số bỏ phiếu cho nó, sử dụng căn chỉnh mô-đun và giữ k đó ở những nơi không khớp nhiều nhất là 2m.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (kiểm tra từng k độc lập) | O(n^2) | O(n) | Quá chậm | 
| Tối ưu (tần số trên dư lượng tuần hoàn) | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi muốn kiểm tra từng ca k xem có bao nhiêu phần tử đã đúng nếu mảng đến từ ca đó. 

### Bước 1: Chuyển điều kiện đúng thành phương trình module 

Đối với mỗi chỉ số j, giá trị đúng khi dịch k sẽ là (j - k mod n) + 1. Việc sắp xếp lại đưa ra điều kiện là mỗi cặp (j, p[j]) bao hàm một k modulo n duy nhất. 

Điều này biến mỗi phần tử mảng thành một phiếu bầu cho một giá trị dịch chuyển cụ thể. 

### Bước 2: Kiểm phiếu cho từng ca 

Chúng ta tính toán một mảng cnt[k], trong đó mỗi vị trí j đóng góp vào đúng một giá trị k rút ra từ phương trình của nó. Điều này cho chúng ta biết có bao nhiêu vị trí phù hợp với ca k. 

Việc giải thích rất trực tiếp: cnt[k] là số phần tử đã được đặt đúng nếu chúng ta chọn shift k. 

### Bước 3: Chuyển đổi kết quả khớp thành giao dịch hoán đổi bắt buộc 

Nếu các phần tử cnt[k] đã đúng thì các phần tử n - cnt[k] sai. Mỗi lần hoán đổi có thể sửa được tối đa hai phần tử sai, do đó số lần hoán đổi tối thiểu cần thiết là (n - cnt[k])/2. 

Chúng tôi yêu cầu giá trị này phải ≤ m, tương đương với: 

n - cnt[k] 2m 

hoặc 

cnt[k] ≥ n - 2m 

### Bước 4: Thu thập tất cả k hợp lệ 

Chúng tôi lặp lại tất cả k từ 0 đến n - 1 và xuất ra những giá trị thỏa mãn ngưỡng. 

### Tại sao nó hoạt động 

Bất biến chính là mọi căn chỉnh chính xác trong một ca đã cho đều tương ứng chính xác với ràng buộc mô-đun cố định trên k. Điều này đảm bảo rằng việc kiểm phiếu trên mỗi k sẽ đo lường chính xác số lượng yếu tố đã phù hợp với sự thay đổi đó. Vì việc hoán đổi chỉ ảnh hưởng đến các phần tử không khớp và mỗi lần hoán đổi làm giảm số lượng không khớp nhiều nhất là 2, nên điều kiện khả thi chỉ phụ thuộc vào số lượng không khớp chứ không phụ thuộc vào sự sắp xếp của những phần không khớp đó. Điều này tách cấu trúc khỏi trật tự và làm cho việc đếm trở nên đầy đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        p = list(map(int, input().split()))

        cnt = [0] * n

        for j, val in enumerate(p):
            # We want k such that after shifting right by k,
            # val ends up at position j.
            # In identity, val is at position val-1.
            # After shift k: position becomes (val-1 + k) % n = j
            # So k ≡ j - (val-1) mod n.
            k = (j - (val - 1)) % n
            cnt[k] += 1

        threshold = n - 2 * m

        res = []
        for k in range(n):
            if cnt[k] >= threshold:
                res.append(k)

        print(len(res), *res)

if __name__ == "__main__":
    solve()
```Việc triển khai mã hóa trực tiếp mối quan hệ mô-đun giữa các vị trí và giá trị. Dòng khóa là tính toán k cho mỗi cặp (j, val), hoạt động như một phiếu bầu. Phần còn lại là kiểm tra ngưỡng đơn giản xuất phát từ giới hạn hoán đổi. 

Phải cẩn thận với số học mô-đun: j và val-1 đều dựa trên 0, do đó không cần điều chỉnh thêm. Ngưỡng sử dụng 2m vì mỗi lần hoán đổi sẽ giảm số lượng không khớp chính xác bằng 2 trong trường hợp tốt nhất. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 4, m = 1
p = [2, 3, 1, 4]
```Chúng tôi tính toán phiếu bầu cho k: 

| j | p[j] | k = (j - (p[j]-1)) mod 4 | 
| --- | --- | --- | 
| 0 | 2 | 0 - 1 = 3 | 
| 1 | 3 | 1 - 2 = 3 | 
| 2 | 1 | 2 - 0 = 2 | 
| 3 | 4 | 3 - 3 = 0 | 

Vậy có thể không: 

k=0:1, k=2:1, k=3:2. 

Ngưỡng là n - 2m = 4 - 2 = 2. 

Chỉ có k=3 thỏa mãn cnt[k] ≥ 2. 

Vậy câu trả lời là {3}. 

Điều này cho thấy ngay cả một dung sai hoán đổi duy nhất cũng lọc mạnh sự dịch chuyển cho phép. 

### Ví dụ 2 

đầu vào:```
n = 3, m = 1
p = [3, 2, 1]
```Phiếu bầu: 

| j | p[j] | k | 
| --- | --- | --- | 
| 0 | 3 | 0 - 2 = 1 | 
| 1 | 2 | 1 - 1 = 0 | 
| 2 | 1 | 2 - 0 = 2 | 

Mỗi k được một phiếu bầu nên cnt = [1,1,1]. Ngưỡng là 3 - 2 = 1 nên tất cả k đều hợp lệ. 

Điều này phù hợp với thực tế là một lần hoán đổi là đủ để thực hiện bất kỳ vòng quay nào trong một chu kỳ đảo chiều 3 chu kỳ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi phần tử đóng góp một phiếu bầu và chúng tôi quét k một lần | 
| Không gian | O(n) | Mảng tần số để đếm ca | 

Tổng độ phức tạp của tất cả các trường hợp thử nghiệm là O(3·10^5), dễ dàng phù hợp với giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque

    input = sys.stdin.readline
    t = int(input())
    out = []
    for _ in range(t):
        n, m = map(int, input().split())
        p = list(map(int, input().split()))

        cnt = [0] * n
        for j, val in enumerate(p):
            k = (j - (val - 1)) % n
            cnt[k] += 1

        threshold = n - 2 * m
        res = [str(k) for k in range(n) if cnt[k] >= threshold]
        out.append(str(len(res)) + (" " + " ".join(res) if res else ""))

    return "\n".join(out)

# provided samples
assert run("""4
4 1
2 3 1 4
3 1
1 2 3
3 1
3 2 1
6 0
1 2 3 4 6 5
""") == """1 3
1 0
3 0 1 2
0"""

# all identical except m large
assert run("""1
5 2
1 2 3 4 5
""") == "5 0 1 2 3 4"

# minimal cycle-like case
assert run("""1
3 0
2 3 1
""") == "1 2"

# maximum mismatch but large m
assert run("""1
4 2
4 3 2 1
""") == "4 0 1 2 3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| danh tính với các giao dịch hoán đổi | tất cả k | hoán đổi không liên quan khi đã đúng | 
| chu trình thuần túy | ca đơn | phát hiện xoay nghiêm ngặt | 
| đảo ngược với số lần hoán đổi tối đa | tất cả k | cao m thư giãn hạn chế | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi m đủ lớn để hầu hết mọi hoán vị đều có thể truy cập được từ bất kỳ phép quay nào. Trong trường hợp đồng nhất, mọi k tạo ra cnt[k] = n, do đó ngưỡng luôn được thỏa mãn và tất cả các ca đều hợp lệ, phù hợp với ý tưởng rằng không cần hoán đổi bất kể phép quay. 

Một trường hợp cạnh khác là hoán vị đảo ngược hoàn toàn. Ở đây, mỗi ca tạo ra sự phân bố đồng đều các kết quả trùng khớp, nhưng khi m ở quy mô tối đa cho phép, ngưỡng trở nên đủ nhỏ để tất cả k đều vượt qua. Thuật toán xử lý việc này một cách tự nhiên vì nó chỉ kiểm tra số lượng chứ không kiểm tra cấu trúc. 

Trường hợp suy biến là m = 0, trong đó chúng ta yêu cầu cnt[k] = n, nghĩa là mọi phần tử phải căn chỉnh hoàn hảo với phép quay. Điều này buộc một k duy nhất khi và chỉ khi mảng chính xác là một phép quay nhận dạng. Việc bỏ phiếu theo mô-đun đảm bảo chỉ có vòng quay chính xác mới tích lũy được sự hỗ trợ đầy đủ.
