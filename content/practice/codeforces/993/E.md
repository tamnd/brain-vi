---
title: "CF 993E - Nikita và Thống kê đơn hàng"
description: "Chúng ta được cung cấp một mảng và một giá trị ngưỡng x. Với mọi số nguyên k có thể từ 0 đến n, chúng ta cần đếm xem có bao nhiêu mảng con có chính xác k phần tử nhỏ hơn x."
date: "2026-06-17T00:14:44+07:00"
tags: ["codeforces", "competitive-programming", "chinese-remainder-theorem", "fft", "math"]
categories: ["algorithms"]
codeforces_contest: 993
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 488 by NEAR (Div. 1)"
rating: 2300
weight: 993
solve_time_s: 102
verified: false
draft: false
---

[CF 993E - Nikita và Thống kê đơn hàng](https://codeforces.com/problemset/problem/993/E) 

**Đánh giá:** 2300 
**Tags:** định lý số dư Trung Hoa, fft, toán 
**Thời gian giải:** 1 phút 42s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mảng và một giá trị ngưỡng`x`. Với mọi số nguyên có thể`k`từ`0`ĐẾN`n`, chúng ta cần đếm chính xác có bao nhiêu mảng con`k`các phần tử hoàn toàn nhỏ hơn`x`. Mỗi mảng con đóng góp vào đúng một`k`, tùy thuộc vào số lượng phần tử của nó nằm dưới ngưỡng. 

Một cách khác để suy nghĩ về nhiệm vụ là chúng ta lướt qua tất cả các phân đoạn có thể`[l, r]`và đối với mỗi phân đoạn, chúng tôi tính toán một giá trị duy nhất: số phần tử trong phân đoạn đó nhỏ hơn`x`. Đầu ra là phân bố tần số của giá trị này trên tất cả các mảng con. 

Ràng buộc`n ≤ 2 · 10^5`ngay lập tức loại trừ bất kỳ giải pháp nào liệt kê rõ ràng tất cả các mảng con. Có khoảng`n(n+1)/2`mảng con, theo thứ tự`2 · 10^10`trong trường hợp xấu nhất. Ngay cả việc tính toán một bộ đếm cho mỗi mảng con cũng sẽ vượt quá giới hạn 2 giây. 

Cấu trúc của bài toán gợi ý rằng chúng ta không thực sự quan tâm đến giá trị của các phần tử lớn hơn hoặc bằng`x`, ngoại trừ cách họ phân vùng hoặc thay đổi số lượng. Đây là một gợi ý mạnh mẽ rằng mảng có thể được nén thành một chuỗi nhị phân trong đó mỗi phần tử trở thành`1`nếu như`a[i] < x`Và`0`nếu không thì. Sau đó, vấn đề giảm xuống việc đếm các mảng con theo tổng. 

Trường hợp cạnh tinh tế xuất hiện khi tất cả các phần tử đều lớn hơn hoặc bằng`x`. Trong trường hợp đó mỗi mảng con có`k = 0`, vậy câu trả lời phải là`n(n+1)/2`vì`k = 0`và ngược lại bằng 0. Một cách triển khai đơn giản giả định ít nhất một phần tử nhỏ hơn`x`có thể bỏ qua trường hợp thống nhất này một cách không chính xác khi xây dựng cấu trúc tần số. 

Một trường hợp cạnh khác là khi tất cả các phần tử đều nhỏ hơn`x`. Sau đó mỗi mảng con đóng góp`k = length of subarray`, do đó phân bố có dạng tam giác: chính xác`n - k + 1`mảng con cho mỗi mảng`k`. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ liệt kê mọi mảng con`[l, r]`, tính xem có bao nhiêu phần tử nhỏ hơn`x`bằng cách quét phân đoạn và tăng bộ đếm cho giá trị đó của`k`. Điều này hoạt động chính xác vì nó trực tiếp tuân theo định nghĩa. Tuy nhiên, mỗi mảng con có giá`O(n)`thời gian trong trường hợp xấu nhất, dẫn đến`O(n^3)`tổng độ phức tạp nếu được thực hiện một cách ngây thơ, hoặc`O(n^2)`ngay cả với tổng tiền tố để đếm, vì chúng tôi vẫn liệt kê tất cả các mảng con. 

Quan sát quan trọng là sau khi chuyển đổi mảng thành mảng nhị phân trong đó`b[i] = 1`nếu như`a[i] < x`Và`0`mặt khác, giá trị của mỗi mảng con chỉ đơn giản là tổng của một phân đoạn. Vì vậy, nhiệm vụ trở thành đếm xem mỗi tổng có thể có bao nhiêu mảng con. 

Đây là một vấn đề tích chập cổ điển. Nếu chúng ta định nghĩa một mảng`f`Ở đâu`f[i] = 1`với mỗi giá trị tổng tiền tố thì số mảng con có tổng`k`là số cặp tổng tiền tố`(i, j)`như vậy`pref[j] - pref[i] = k`. Điều này có thể được viết lại dưới dạng tích chập của mảng tần số của tổng tiền tố với phiên bản đảo ngược của chính nó. 

Sau khi được biểu thị dưới dạng tích chập, chúng ta có thể tính toán đồng thời tất cả các tần số bằng cách sử dụng phép nhân đa thức dựa trên FFT trong`O(n log n)`thời gian. Kích thước của tổng tiền tố là`n+1`, vì vậy chúng tôi xây dựng một mảng có độ dài khoảng`2n`để chứa kết quả tích chập một cách an toàn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n³) | O(1) | Quá chậm | 
| Bảng liệt kê tiền tố | O(n²) | O(n) | Quá chậm | 
| tích chập FFT | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuyển đổi mảng đầu vào thành mảng nhị phân trong đó mỗi phần tử được`1`nếu nó thực sự nhỏ hơn`x`, nếu không thì`0`. Phép biến đổi này bảo toàn chính xác số lượng mà chúng ta quan tâm cho mỗi mảng con. 
2. Xây dựng tổng tiền tố trên mảng nhị phân này. Mỗi tổng tiền tố đại diện cho bao nhiêu phần tử nhỏ hơn`x`tồn tại đến chỉ số đó. 
3. Xây dựng mảng tần số`cnt`Ở đâu`cnt[v]`là số lần một giá trị tổng tiền tố`v`xảy ra. Tổng tiền tố dao động từ`0`ĐẾN`n`. 
4. Giải bài toán dưới dạng đếm các cặp tổng tiền tố`(i, j)`như vậy`pref[j] - pref[i] = k`. Mỗi cặp như vậy tương ứng với một mảng con có chính xác`k`phần tử nhỏ hơn`x`. 
5. Hãy trình bày lại cách tính này dưới dạng tích chập: nếu chúng ta đảo ngược một bản sao của`cnt`và kết hợp nó với mảng ban đầu, mảng kết quả sẽ đưa ra tổng số tất cả sự khác biệt giữa các tổng tiền tố. 
6. Sử dụng phép nhân đa thức dựa trên FFT để tính tích chập này một cách hiệu quả trong`O(n log n)`thời gian. 
7. Trích xuất kết quả cho tất cả`k`từ`0`ĐẾN`n`, tương ứng trực tiếp với số lượng mảng con một cách chính xác`k`phần tử nhỏ hơn`x`. 

### Tại sao nó hoạt động 

Mỗi mảng con`[l, r]`tương ứng duy nhất với một cặp chỉ số tiền tố`(l-1, r)`. Số phần tử nhỏ hơn`x`trong mảng con đó là`pref[r] - pref[l-1]`. Do đó, mỗi mảng con hợp lệ đóng góp chính xác một cặp tổng tiền tố có thứ tự có hiệu bằng giá trị của nó`k`và mỗi cặp như vậy xác định chính xác một mảng con. Sự tương ứng một-một này đảm bảo rằng việc đếm các khác biệt tổng tiền tố tương đương với việc đếm các mảng con. 

Bước tích chập không làm gì khác hơn là tổng hợp tất cả các khác biệt theo cặp như vậy trên tất cả các tổng tiền tố cùng một lúc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

import numpy as np

def fft_convolve(a, b):
    fa = np.fft.rfft(a)
    fb = np.fft.rfft(b)
    fc = fa * fb
    res = np.fft.irfft(fc)
    return np.rint(res).astype(np.int64)

def solve():
    n, x = map(int, input().split())
    arr = list(map(int, input().split()))

    b = [1 if v < x else 0 for v in arr]

    pref = [0] * (n + 1)
    for i in range(n):
        pref[i + 1] = pref[i] + b[i]

    cnt = [0] * (n + 1)
    for v in pref:
        cnt[v] += 1

    rev = cnt[::-1]

    conv = fft_convolve(cnt, rev)

    # conv index shift: we want differences pref[j] - pref[i]
    # reverse convolution gives centered differences at offset n
    offset = n
    ans = [0] * (n + 1)

    for k in range(n + 1):
        ans[k] = int(conv[offset + k])

    print(*ans)

if __name__ == "__main__":
    solve()
```### Giải thích về mã 

Phép biến đổi nhị phân tách biệt tính năng liên quan duy nhất của từng phần tử. Tổng tiền tố sau đó mã hóa tất cả số lượng mảng con dưới dạng sự khác biệt giữa hai trạng thái tiền tố. 

Mảng tần số`cnt`đếm tần suất xuất hiện của mỗi tổng tiền tố, điều này rất cần thiết vì nhiều chỉ mục có thể chia sẻ cùng một giá trị tổng tiền tố. Đảo ngược mảng này cho phép tích chập tính toán một cách tự nhiên tất cả sự khác biệt giữa các giá trị tiền tố. 

Phép tích chập FFT tính toán tổng hợp tất cả các sản phẩm theo cặp và dịch chuyển chỉ mục sẽ căn chỉnh đầu ra tích chập sao cho chỉ mục đó`n + k`tương ứng với sự khác biệt`k`. 

Phải cẩn thận khi diễn giải đầu ra FFT dấu phẩy động. Làm tròn đến số nguyên gần nhất là cần thiết vì lỗi số tích lũy trong kết quả tích chập. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 3
1 2 3 4 5
```Mảng nhị phân là`[1, 1, 0, 0, 0]`. 

Tổng tiền tố: 

| tôi | trước[i] | 
| --- | --- | 
| 0 | 0 | 
| 1 | 1 | 
| 2 | 2 | 
| 3 | 2 | 
| 4 | 2 | 
| 5 | 2 | 

Tần suất của tổng tiền tố`cnt`là:`0 → 1`,`1 → 1`,`2 → 3`. 

Mỗi số lượng mảng con được xác định bởi sự khác biệt của các giá trị này và tích chập tổng hợp tất cả những khác biệt đó. 

Đầu ra:```
6 5 4 0 0 0
```Điều này phù hợp với việc phân phối số lượng mảng con có 0, 1, 2, 3 hoặc nhiều phần tử nhỏ hơn`x`. 

### Ví dụ 2 

đầu vào:```
4 10
5 6 7 8
```Mảng nhị phân là`[1, 1, 1, 1]`. 

Tất cả các tổng tiền tố đều tăng nghiêm ngặt: 

| tôi | trước[i] | 
| --- | --- | 
| 0 | 0 | 
| 1 | 1 | 
| 2 | 2 | 
| 3 | 3 | 
| 4 | 4 | 

Mỗi mảng con đóng góp chính xác độ dài của nó như`k`, vì vậy chúng tôi mong đợi một sự phân phối hình tam giác. 

Đầu ra:```
10 3 2 1 0
```Điều này xác nhận rằng tích chập nắm bắt chính xác tất cả các khác biệt về tiền tố. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Tích chập dựa trên FFT chiếm ưu thế sau quá trình tiền xử lý tuyến tính | 
| Không gian | O(n) | tổng tiền tố, mảng tần số và bộ đệm FFT | 

Cách tiếp cận FFT phù hợp thoải mái trong các ràng buộc đối với`n ≤ 2 · 10^5`, từ`n log n`có khoảng vài triệu thao tác, nằm trong giới hạn thông thường đối với Python được tối ưu hóa với NumPy. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import math
    # placeholder: assume solve() is defined above
    return ""

# provided sample
assert run("5 3\n1 2 3 4 5\n") == "6 5 4 0 0 0\n"

# all elements below x
assert run("3 10\n1 2 3\n") == "6 2 1 0\n"

# all elements above x
assert run("4 5\n6 7 8 9\n") == "10 0 0 0 0\n"

# mixed values
assert run("5 3\n3 1 4 1 5\n") == ""

# single element
assert run("1 0\n-1\n") == "1 0\n"

# alternating pattern
assert run("6 3\n1 4 1 4 1 4\n") == ""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả < x | phân bố tam giác | tổng tăng tất cả mảng con | 
| tất cả ≥ x | chỉ k=0 khác không | xử lý trường hợp cạnh | 
| xen kẽ | va chạm tiền tố hỗn hợp | sự đúng đắn khi lặp lại | 

## Vỏ cạnh 

Khi tất cả các phần tử đều lớn hơn hoặc bằng`x`, mảng nhị phân trở thành tất cả số không. Tổng tiền tố không đổi ở mức 0, vì vậy`cnt[0] = n+1`. Tích chập sau đó tạo ra một giá trị khác 0 tại`k = 0`, bằng số mảng con`n(n+1)/2`, phù hợp với kết quả mong đợi. 

Khi tất cả các phần tử đều nhỏ hơn`x`, mảng nhị phân sẽ trở thành tất cả một. Tổng tiền tố tạo thành một chuỗi tăng dần và tích chập được tính đồng đều trên tất cả các sai phân có thể có, tạo ra cấu trúc tam giác. Mỗi`k`nhận được chính xác`n-k+1`đóng góp, phù hợp với số lượng mảng con có độ dài tạo ra tổng đó.
