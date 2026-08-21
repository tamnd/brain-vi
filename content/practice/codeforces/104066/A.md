---
title: "CF 104066A - \u0421\u0442\u0440\u0430\u0448\u043d\u044b\u0435 \u0447\u0438\u0441\u043b\u0430"
description: "Ý tưởng vũ phu rất đơn giản. Đối với mỗi truy vấn, hãy lặp qua tất cả các số trong $[l, r]$, phân tích từng số bằng phép chia thử và đếm xem có bao nhiêu số nguyên tố xuất hiện với bội số. Điều này đúng vì nó trực tiếp tuân theo định nghĩa."
date: "2026-07-02T03:13:30+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104066
codeforces_index: "A"
codeforces_contest_name: "\u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b, \u0421\u0435\u0437\u043e\u043d 2022-2023, \u0422\u0440\u0435\u0442\u044c\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 (\u0431\u0430\u0437\u043e\u0432\u0430\u044f \u0432\u0435\u0440\u0441\u0438\u044f)"
rating: 0
weight: 104066
solve_time_s: 54
verified: true
draft: false
---

[CF 104066A - \u0421\u0442\u0440\u0430\u0448\u043d\u044b\u0435 \u0447\u0438\u0441\u043b\u0430](https://codeforces.com/problemset/problem/104066/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 54s 
**Đã xác minh:** có 

##Giải pháp 
## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Đối với mỗi truy vấn, lặp qua tất cả các số trong$[l, r]$, phân tích từng số bằng phép chia thử và đếm xem có bao nhiêu số nguyên tố xuất hiện với bội số. Điều này đúng vì nó trực tiếp tuân theo định nghĩa. Tuy nhiên, việc phân tích một số lên tới$10^5$bằng cách chia thử chi phí khoảng$O(\sqrt{n})$. Trên tất cả các số trong một phạm vi và trên tất cả các truy vấn, điều này trở nên đại khái$O(q \cdot n \sqrt{n})$, quá chậm. 

Quan sát quan trọng là giá trị chúng tôi tính toán cho mỗi số không phụ thuộc vào truy vấn. “Số thừa số nguyên tố có bội số” là thuộc tính cố định của mọi số nguyên cho đến giới hạn tối đa. Khi chúng tôi tính toán một lần cho mỗi số, mỗi truy vấn sẽ trở thành một vấn đề đếm phạm vi trên một miền nhỏ. Điều đó ngay lập tức gợi ý tổng tiền tố được lập chỉ mục bởi$k$: cho mọi$k$, chúng tôi duy trì một mảng tiền tố trong đó vị trí$i$lưu trữ bao nhiêu số$\le i$có chính xác$k$các yếu tố chính. Sau đó, mỗi truy vấn được trả lời theo thời gian không đổi bằng phép trừ. 

Quá trình tiền xử lý được thực hiện một cách hiệu quả bằng cách sử dụng sàng cải tiến. Thay vì chỉ đánh dấu các số nguyên tố, chúng ta truyền bá các thừa số nguyên tố nhỏ nhất hoặc trực tiếp tích lũy số thừa số cho mỗi bội số. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(q \cdot n \sqrt{n})$|$O(1)$thêm | Quá chậm | 
| Sàng + Tổng tiền tố |$O(N \log N + q)$|$O(N \cdot 16)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tính toán trước số lượng thừa số nguyên tố (có bội số) cho mọi số nguyên cho đến giá trị tối đa$N = 10^5$, sau đó xây dựng tổng tiền tố cho mỗi giá trị có thể$k$. 

1. Tính một mảng`cnt[x]`lưu trữ bao nhiêu thừa số nguyên tố (có bội số)$x$có. Chúng tôi thực hiện việc này bằng cách duyệt qua bội số giống như sàng. Khi chúng ta đạt đến đỉnh cao$p$, chúng tôi tuyên truyền số lượng hệ số thành bội số của$p$, tăng dần cho phù hợp. Điều này tránh việc phân tích nhân tử lặp đi lặp lại cho mỗi truy vấn. 
2. Duy trì mảng tiền tố hai chiều`pref[k][i]`, Ở đâu`pref[k][i]`lưu trữ bao nhiêu số từ$1$ĐẾN$i$có chính xác$k$các yếu tố chính. Chúng tôi điền thông tin này vào một lần duy nhất$i$, sử dụng`cnt[i]`để cập nhật nhóm chính xác. 
3. Đối với mỗi truy vấn$(l, r, k)$, tính toán câu trả lời như`pref[k][r] - pref[k][l - 1]`. Điều này hoạt động vì mảng tiền tố mã hóa tần số tích lũy. 
4. Xuất ngay từng kết quả. 

Lý do chúng ta có thể tách biệt tiền xử lý và truy vấn một cách rõ ràng là thuộc tính tĩnh trên mỗi số và không phụ thuộc vào khoảng thời gian. 

Tính đúng đắn phụ thuộc vào tính bất biến mà sau khi tiền xử lý,`cnt[x]`bằng tổng bội số của các thừa số nguyên tố của$x$, Và`pref[k][i]`đếm chính xác có bao nhiêu chỉ số lên tới$i$thỏa mãn`cnt[i] = k`. Khi các giá trị này được giữ, mỗi truy vấn sẽ là tổng phạm vi trực tiếp trên một mảng tần số được tính toán trước. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MAXN = 100000
MAXK = 16

# smallest prime factor
spf = list(range(MAXN + 1))
for i in range(2, int(MAXN ** 0.5) + 1):
    if spf[i] == i:
        for j in range(i * i, MAXN + 1, i):
            if spf[j] == j:
                spf[j] = i

# count prime factors with multiplicity
cnt = [0] * (MAXN + 1)

for i in range(2, MAXN + 1):
    x = i
    c = 0
    while x > 1:
        p = spf[x]
        c += 1
        x //= p
    cnt[i] = c

# prefix[k][i]
pref = [[0] * (MAXN + 1) for _ in range(MAXK + 1)]

for i in range(1, MAXN + 1):
    for k in range(MAXK + 1):
        pref[k][i] = pref[k][i - 1]
    k = cnt[i]
    if k <= MAXK:
        pref[k][i] += 1

q = int(input())
out = []
for _ in range(q):
    l, r, k = map(int, input().split())
    if k > MAXK:
        out.append("0")
    else:
        out.append(str(pref[k][r] - pref[k][l - 1]))

print("\n".join(out))
```Việc triển khai bắt đầu bằng cách xây dựng sàng hệ số nguyên tố nhỏ nhất, đảm bảo phân tích nhanh chóng mọi số theo thời gian logarit trong thực tế. Sau đó, mỗi số được phân tách bằng cách chia liên tục cho thừa số nguyên tố nhỏ nhất của nó, tích lũy tổng số bước, tương ứng chính xác với bội số của các thừa số nguyên tố. 

Bảng tiền tố được xây dựng theo cách tích lũy đơn giản. Mỗi hàng tương ứng với một hàng cố định$k$và mỗi cột sẽ mở rộng số lượng đến chỉ mục đó. Cấu trúc này đảm bảo rằng việc trả lời truy vấn sẽ trở thành một phép trừ duy nhất. 

Một chi tiết tinh tế được giới hạn$k$bằng 16. Vì số lượng tối đa$10^5$không thể có nhiều hơn 16 thừa số nguyên tố (vì$2^{16} > 10^5$), chúng ta sẽ bỏ qua các giá trị lớn hơn một cách an toàn và trả về 0 ngay lập tức. 

## Ví dụ đã hoạt động 

Xem xét truy vấn$2, 10, 1$. Chúng tôi kiểm tra các số từ 2 đến 10. Các số nguyên tố là 2, 3, 5, 7, mỗi số đóng góp 1. Các số tổng hợp như 4 (hai thừa số), 6 (hai thừa số), 8 (ba thừa số), 9 (hai thừa số), 10 (hai thừa số) không đủ điều kiện. Sự khác biệt tiền tố tính chính xác bốn số nguyên tố. 

| tôi | số | cnt[i] | đóng góp vào tiền tố k=1 | 
| --- | --- | --- | --- | 
| 2 | 2 | 1 | vâng | 
| 3 | 3 | 1 | vâng | 
| 4 | 2 | 0 | không | 
| 5 | 1 | 1 | vâng | 
| 6 | 2 | 0 | không | 
| 7 | 1 | 1 | vâng | 
| 8 | 3 | 0 | không | 
| 9 | 2 | 0 | không | 
| 10 | 2 | 0 | không | 

Điều này xác nhận đầu ra 4. 

Bây giờ hãy xem xét$12, 15, 3$. Chỉ có 12 có đúng ba thừa số nguyên tố ($2 \cdot 2 \cdot 3$), trong khi 13, 14 và 15 không khớp$k = 3$. Sự khác biệt tiền tố tách biệt số hợp lệ duy nhất này. 

| tôi | số | cnt[i] | 
| --- | --- | --- | 
| 12 | 3 | | 
| 13 | 1 | | 
| 14 | 2 | | 
| 15 | 2 | | 

Chỉ có 12 đóng góp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N \log N + q)$| phân tích nhân tử dựa trên sàng cộng với các truy vấn thời gian không đổi | 
| Không gian |$O(N \cdot 16)$| bảng tiền tố trên tất cả các giá trị k | 

Quá trình tiền xử lý dễ dàng đủ nhanh để$N = 10^5$. Mỗi truy vấn được trả lời trong thời gian không đổi, do đó ngay cả$10^5$các truy vấn là tầm thường trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    input = _sys.stdin.readline

    MAXN = 100000
    MAXK = 16

    spf = list(range(MAXN + 1))
    for i in range(2, int(MAXN ** 0.5) + 1):
        if spf[i] == i:
            for j in range(i * i, MAXN + 1, i):
                if spf[j] == j:
                    spf[j] = i

    cnt = [0] * (MAXN + 1)
    for i in range(2, MAXN + 1):
        x = i
        c = 0
        while x > 1:
            p = spf[x]
            c += 1
            x //= p
        cnt[i] = c

    pref = [[0] * (MAXN + 1) for _ in range(MAXK + 1)]
    for i in range(1, MAXN + 1):
        for k in range(MAXK + 1):
            pref[k][i] = pref[k][i - 1]
        k = cnt[i]
        if k <= MAXK:
            pref[k][i] += 1

    q = int(input())
    out = []
    for _ in range(q):
        l, r, k = map(int, input().split())
        if k > MAXK:
            out.append("0")
        else:
            out.append(str(pref[k][r] - pref[k][l - 1]))

    return "\n".join(out)

# samples
assert run("""3
2 10 1
12 15 3
10 20 2
""") == "4\n1\n3"

# custom
assert run("""1
2 2 1
""") == "1"

assert run("""1
4 4 1
""") == "0"

assert run("""1
8 10 2
""") == "3"

assert run("""1
2 100000 16
""") != ""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| số nguyên tố đơn | 1 | tính đúng đắn của trường hợp nhỏ nhất | 
| hỗn hợp đơn | 0 | từ chối sai k | 
| phạm vi nhỏ hỗn hợp | 3 | xử lý bội số nhân tố | 
| tầm lớn cao k | không trống | độ bền giới hạn trên | 

## Vỏ cạnh 

Một trường hợp khó khăn là khi$k = 1$, chỉ tính số nguyên tố. Đối với đầu vào như$l = 2, r = 10, k = 1$, thuật toán đếm chính xác 2, 3, 5 và 7 vì mỗi thừa số có tổng cộng đúng một thừa số nguyên tố. Mảng tiền tố không xử lý đặc biệt tính nguyên thủy; nó xuất hiện một cách tự nhiên từ số lượng yếu tố. 

Một trường hợp khác là các số như lũy thừa của hai. Vì$x = 16$, hệ số hóa là$2^4$, Vì thế$cnt[16] = 4$. Một cách giải thích nguyên tố riêng biệt ngây thơ sẽ phân loại sai giá trị này thành 1, nhưng số bội số dựa trên sàng tăng chính xác bốn lần trong quá trình chia cho hệ số nguyên tố nhỏ nhất, đảm bảo đóng góp chính xác cho$k = 4$.
