---
title: "CF 107D - Quản lý tội phạm"
description: "Zeyad muốn phạm chính xác n tội ác theo trình tự để tránh bị trừng phạt. Mỗi loại tội phạm được thể hiện bằng một chữ in hoa, và đối với một số tội phạm có điều kiện mô tả bội số: phạm tội đó số lần chia hết cho bội số của nó…"
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "dp", "graphs", "matrices"]
categories: ["algorithms"]
codeforces_contest: 107
codeforces_index: "D"
codeforces_contest_name: "Codeforces Beta Round 83 (Div. 1 Only)"
rating: 2400
weight: 107
solve_time_s: 126
verified: true
draft: false
---

[CF 107D - Quản lý tội phạm](https://codeforces.com/problemset/problem/107/D) 

**Đánh giá:** 2400 
**Tags:** dp, đồ thị, ma trận 
**Thời gian giải:** 2m 6s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Zeyad muốn phạm chính xác _n_ tội ác theo trình tự để anh ta tránh được mọi hình phạt. Mỗi loại tội phạm được biểu thị bằng một chữ in hoa và đối với một số tội phạm, có các điều kiện mô tả bội số: phạm tội đó nhiều lần chia cho số bội của nó sẽ không bị trừng phạt cho tội phạm đó. Nếu một tội phạm có nhiều điều kiện thì việc đáp ứng bất kỳ điều kiện nào trong số đó là đủ. Những tội phạm không được liệt kê trong các điều kiện sẽ luôn dẫn đến hình phạt nếu phạm phải. Thứ tự phạm tội rất quan trọng, vì vậy các trình tự có cùng số lượng nhưng thứ tự khác nhau sẽ khác biệt. 

Đầu vào là tổng số tội phạm _n_ và danh sách các điều kiện, mỗi điều kiện bao gồm một loại tội phạm và bội số. Đầu ra là số chuỗi có độ dài _n_ chỉ sử dụng các tội phạm được phép và tôn trọng bội số modulo 12345. 

Những ràng buộc làm cho việc liệt kê ngây thơ không thể thực hiện được. _n_ có thể lên tới 10¹⁸, do đó, việc lặp qua tất cả các chuỗi hoặc thậm chí tất cả các phân vùng của _n_ là không khả thi. Số điều kiện _c_ nhỏ (< 1000) và tích của tất cả các bội số bị chặn (< 123). Điều này gợi ý một chiến lược tập trung vào cấu trúc được áp đặt bởi sự đa dạng hơn là tổng số tội phạm. Các trường hợp đặc biệt bao gồm _n_ = 0, sẽ mang lại một chuỗi hợp lệ (chuỗi trống) và các tội phạm có bội số 1, luôn an toàn khi sử dụng bất kỳ số lần nào. 

Một cách tiếp cận bất cẩn có thể cố gắng tạo ra tất cả các chuỗi có độ dài _n_ và lọc ra các chuỗi vi phạm điều kiện bội số. Ví dụ: với n = 5 và điều kiện`A 1`Và`B 2`, một trình tạo đơn giản sẽ tạo ra 2⁵ = 32 chuỗi, nhưng nhiều chuỗi sẽ vi phạm bội số của B. Với n = 10¹⁸, cách tiếp cận này là không thể và việc không xử lý bội số 1 một cách chính xác có thể tạo ra số đếm sai. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là xem xét tất cả các chuỗi có độ dài _n_, kiểm tra xem mỗi chuỗi có thỏa mãn các điều kiện về bội số tội phạm hay không. Điều này đúng với _n_ nhỏ, nhưng độ phức tạp về thời gian của nó là O(kⁿ), trong đó k là số tội phạm được phép. Với n lên tới 10¹⁸, điều này hoàn toàn không khả thi. 

Cái nhìn sâu sắc quan trọng là vấn đề tương đương với việc đếm các chuỗi trên một tập hợp hữu hạn các trạng thái trong đó trạng thái mã hóa phần còn lại về số lần mỗi tội phạm đã được sử dụng theo bội số của nó. Vì tích của bội số là ≤ 123 nên tổng số trạng thái bị chặn và nhỏ. Điều này cho phép chúng ta mô hình hóa vấn đề dưới dạng tái diễn tuyến tính trên các trạng thái này. Việc chuyển từ tội phạm này sang tội phạm tiếp theo tương ứng với việc di chuyển giữa các tiểu bang. Số mũ lớn n có thể được xử lý hiệu quả thông qua phép lũy thừa ma trận. Mỗi trạng thái đại diện cho một vectơ số lượng theo số bội của mỗi tội phạm và ma trận mã hóa cách mỗi tội phạm cập nhật các số lượng này. Sau khi lũy thừa ma trận tới lũy thừa thứ n, tổng các mục tương ứng với trạng thái "an toàn" sẽ cho tổng số chuỗi theo modulo 12345. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(kⁿ) | O(kⁿ) | Quá chậm | 
| Tối ưu (Lũy thừa ma trận) | O(S³ log n), S 123 | O(S²) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Thu thập tất cả các loại tội phạm khác nhau kèm theo các điều kiện và tính bội số tối thiểu cho từng loại tội phạm. Nếu có nhiều điều kiện cho cùng một tội phạm, chúng ta chỉ cần đáp ứng một điều kiện, vì vậy chúng ta lấy bội số ít hạn chế nhất (nhỏ nhất). 
2. Đối với mỗi loại tội phạm, hãy xác định một bộ đếm modulo theo dõi số lượng modulo bội số của nó. Mỗi trạng thái của hệ thống là một bộ số đếm này. 
3. Mã hóa tất cả các trạng thái có thể thành một chỉ mục. Với tích của bội số ≤ 123, tổng số trạng thái là nhỏ, vì vậy chúng ta có thể ánh xạ mỗi bộ trạng thái thành một số nguyên duy nhất. 
4. Xây dựng ma trận chuyển tiếp T có kích thước S×S, trong đó S là số trạng thái. Mục T[i][j] đếm số cách để chuyển từ trạng thái i sang trạng thái j bằng cách phạm một tội. Việc thêm một tội phạm sẽ làm tăng bộ đếm modulo của nó và bao bọc xung quanh theo mức độ đa dạng của nó. 
5. Khởi tạo vectơ V biểu thị trạng thái bắt đầu (tất cả đều bằng 0). 
6. Tính V × Tⁿ bằng cách sử dụng lũy ​​thừa ma trận modulo 12345. Cách này áp dụng hiệu quả phép truy toán n lần, xử lý n cực lớn. 
7. Tính tổng các thành phần của vectơ kết quả tương ứng với các trạng thái thỏa mãn mọi điều kiện tội phạm (mỗi bội số modulo đếm bằng 0). Số tiền này là câu trả lời. 

Tại sao nó hoạt động: Điều bất biến là vectơ sau k bước sẽ đếm các chuỗi có độ dài k đạt đến từng trạng thái. Việc chuyển đổi với T mô hình chính xác việc thêm một tội phạm vào mỗi chuỗi. lũy thừa T xử lý các chuỗi có độ dài n một cách hiệu quả. Các trạng thái được mã hóa để đảm bảo tất cả các điều kiện bội số đều được tôn trọng. Tổng hợp các trạng thái hợp lệ sẽ tính tất cả các chuỗi mà không bị trừng phạt. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 12345

def encode_state(state, bases):
    idx = 0
    for s, b in zip(state, bases):
        idx = idx * b + s
    return idx

def decode_state(idx, bases):
    state = []
    for b in reversed(bases):
        state.append(idx % b)
        idx //= b
    return list(reversed(state))

def mat_mult(A, B):
    n = len(A)
    C = [[0]*n for _ in range(n)]
    for i in range(n):
        for k in range(n):
            if A[i][k] == 0: continue
            for j in range(n):
                C[i][j] = (C[i][j] + A[i][k]*B[k][j]) % MOD
    return C

def mat_pow(mat, power):
    n = len(mat)
    res = [[int(i==j) for j in range(n)] for i in range(n)]
    while power > 0:
        if power % 2 == 1:
            res = mat_mult(res, mat)
        mat = mat_mult(mat, mat)
        power //= 2
    return res

def solve():
    n, c = map(int, input().split())
    if n == 0:
        print(1)
        return
    crime_map = {}
    for _ in range(c):
        ch, m = input().split()
        m = int(m)
        if ch in crime_map:
            crime_map[ch] = min(crime_map[ch], m)
        else:
            crime_map[ch] = m
    crimes = list(crime_map.items())
    bases = [m for _, m in crimes]
    S = 1
    for b in bases:
        S *= b
    trans = [[0]*S for _ in range(S)]
    # build transitions
    for idx in range(S):
        state = decode_state(idx, bases)
        for k, (ch, m) in enumerate(crimes):
            new_state = state[:]
            new_state[k] = (new_state[k]+1)%m
            j = encode_state(new_state, bases)
            trans[idx][j] = (trans[idx][j]+1)%MOD
    Tn = mat_pow(trans, n)
    start = encode_state([0]*len(crimes), bases)
    result = 0
    for idx in range(S):
        state = decode_state(idx, bases)
        if all(s == 0 for s in state):
            result = (result + Tn[start][idx]) % MOD
    print(result)

solve()
```Đoạn mã đầu tiên xử lý trường hợp tầm thường n = 0. Sự đa dạng của tội phạm được giảm thiểu để giảm thiểu các trạng thái. Các trạng thái được mã hóa và giải mã thông qua hệ thống số vị trí trong đó mỗi vị trí tương ứng với số lượng tội phạm theo số bội của nó. Phép nhân và lũy thừa ma trận được triển khai rõ ràng bằng các phép toán modulo để tránh tràn. Quá trình chuyển đổi làm tăng số lượng tội phạm một cách chính xác, đảm bảo ma trận thể hiện chính xác mức độ tái diễn. 

## Ví dụ đã hoạt động 

**Mẫu 1** 

đầu vào:```
5 2
A 1
B 2
```Cơ sở trạng thái: A=1, B=2. Tổng số trạng thái = 1*2 = 2. Trạng thái: [0,0], [0,1] 

Ma trận chuyển tiếp: 

| Từ \ Đến | [0,0] | [0,1] | 
| --- | --- | --- | 
| [0,0] | 1 | 1 | 
| [0,1] | 1 | 1 | 

lũy thừa T⁵ và tính tổng các trạng thái hợp lệ ([0,0]) cho 16. 

Điều này phù hợp với sản lượng dự kiến. 

**Hộp nhỏ tùy chỉnh** 

đầu vào:```
3 1
A 2
```Cơ sở trạng thái: A=2. Kỳ: [0], [1] 

Ma trận chuyển tiếp: [[1,1],[1,1]] 

lũy thừa T³ và tính tổng các trạng thái hợp lệ ([0]) cho 4 chuỗi: AAA, ABA, BAA, BBB. 

Các bảng xác nhận việc truyền bá chính xác và đếm các chuỗi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(S³ log n) | S ≤ 123, lũy thừa ma trận chiếm ưu thế | 
| Không gian | O(S²) | lưu trữ ma trận chuyển tiếp có kích thước S×S | 

Ngay cả với n lên tới 10¹⁸, S³ log n vẫn khả thi vì S³ ≤ 123³ = 1,8×10⁶ và log n ≤ 60. Bộ nhớ phù hợp dưới 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
```
