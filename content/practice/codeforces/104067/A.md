---
title: "CF 104067A - \u0421\u0442\u0440\u0430\u0448\u043d\u044b\u0435 \u0447\u0438\u0441\u043b\u0430"
description: "Chúng tôi được cung cấp nhiều truy vấn độc lập trên một phạm vi số nguyên nhỏ. Mỗi truy vấn cung cấp một khoảng $[l, r]$ và một số $k$. Với mỗi số nguyên $x$, chúng ta phân tích nó thành các số nguyên tố và đếm xem có bao nhiêu thừa số nguyên tố xuất hiện trong phép phân tích đó, tính bội số."
date: "2026-07-02T03:09:19+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104067
codeforces_index: "A"
codeforces_contest_name: "\u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b, \u0421\u0435\u0437\u043e\u043d 2022-2023, \u0422\u0440\u0435\u0442\u044c\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 (\u043f\u0440\u043e\u0434\u0432\u0438\u043d\u0443\u0442\u0430\u044f \u0432\u0435\u0440\u0441\u0438\u044f)"
rating: 0
weight: 104067
solve_time_s: 63
verified: true
draft: false
---

[CF 104067A - \u0421\u0442\u0440\u0430\u0448\u043d\u044b\u0435 \u0447\u0438\u0441\u043b\u0430](https://codeforces.com/problemset/problem/104067/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 3s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp nhiều truy vấn độc lập trên một phạm vi số nguyên nhỏ. Mỗi truy vấn cung cấp một khoảng$[l, r]$và một số$k$. Với mọi số nguyên$x$, chúng ta phân tích nó thành các số nguyên tố và đếm xem có bao nhiêu thừa số nguyên tố xuất hiện trong phép phân tích đó, tính bội số. Vì thế$12 = 2^2 \cdot 3$đóng góp$3$các yếu tố, trong khi$18 = 2 \cdot 3^2$cũng góp phần$3$. 

Một số được gọi$k$-“đáng sợ” nếu số thừa số nguyên tố này chính xác$k$. Đối với mỗi truy vấn, chúng ta phải đếm xem có bao nhiêu số nguyên trong phân đoạn đã cho có chính xác$k$các yếu tố chính. 

Các ràng buộc đầu vào đã định hình giải pháp một cách mạnh mẽ. Các giá trị của$l$Và$r$nhiều nhất là$10^5$, do đó vũ trụ số đủ nhỏ để chúng ta có thể xử lý trước các thuộc tính cho mọi số nguyên một lần. Tuy nhiên, số lượng truy vấn rất lớn, có thể lên tới$10^5$, do đó việc tính toán lại hệ số cho mỗi truy vấn sẽ quá chậm. Một cách tiếp cận đơn giản giúp phân tích mọi số trong mỗi truy vấn dẫn đến kết quả gần đúng$10^5 \times 10^5$hoạt động vượt quá giới hạn có thể chấp nhận được. 

tham số$k$cũng nhỏ, giới hạn bởi 16, điều này cho thấy rằng sự phân bố số lượng thừa số nguyên tố là nông và có thể được tính toán trước một cách hiệu quả. 

Trường hợp cạnh tinh tế xuất phát từ các số nhỏ như 1. Số 1 không có thừa số nguyên tố. Nếu một truy vấn yêu cầu$k = 0$, nó nên được tính, nhưng nhiều triển khai ngây thơ bỏ qua hoàn toàn 1 vì nó không có phân tách số nguyên tố. Một vấn đề khác là tính bội số một cách chính xác. Ví dụ: 8 nên đóng góp 3 chứ không phải 1, vì$8 = 2^3$. Một sai lầm ở đây dẫn tới việc đếm thiếu một cách có hệ thống. 

## Phương pháp tiếp cận 

Một giải pháp brute-force xử lý trực tiếp từng truy vấn bằng cách phân tích mọi số trong$[l, r]$dùng phép chia thử. Điều này rất đơn giản về mặt khái niệm: đối với mỗi số, hãy chia liên tục cho các số nguyên tố và các thừa số đếm. Điều đó đúng, nhưng chi phí của nó bị chi phối bởi việc nhân tố hóa lặp đi lặp lại. Ngay cả với một cái sàng, làm điều này$10^5$lần trên phạm vi kích thước$10^5$kết quả là khoảng$10^{10}$các hoạt động trong trường hợp xấu nhất không thể vượt qua. 

Quan sát quan trọng là tất cả các số đều nằm trong một giới hạn nhỏ cố định, do đó số lượng thừa số nguyên tố của chúng có thể được tính toán trước một lần. Thay vì tính toán lại hệ số cho mỗi truy vấn, chúng tôi tính toán một hàm$f[x]$bằng số thừa số nguyên tố của$x$. Khi đã biết mảng này, mỗi truy vấn sẽ trở thành vấn đề đếm phạm vi trên một mảng tĩnh. 

Việc đếm phạm vi trên nhiều truy vấn được xử lý hiệu quả bằng cách sử dụng tổng tiền tố. Với mọi giá trị có thể có của$k$, chúng tôi xây dựng một mảng tiền tố$pref[k][x]$lưu trữ bao nhiêu số lên tới$x$có chính xác$k$các yếu tố chính. Mỗi truy vấn sau đó giảm xuống thành một phép trừ theo thời gian không đổi. 

Điều này chuyển đổi vấn đề từ việc tính toán lý thuyết số lặp đi lặp lại thành bước tiền xử lý cộng với tra cứu nhanh. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(q \cdot (r-l+1)\sqrt{n})$|$O(1)$| Quá chậm | 
| Tối ưu (sàng + tổng tiền tố) |$O(N \log \log N + q)$|$O(NK)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý trước tất cả các số lên đến$N = 10^5$. 

1. Xây dựng một mảng`spf`hoặc tính trực tiếp các số nguyên tố nhỏ nhất bằng phương pháp sàng, để mỗi số có thể được phân tích thành thừa số một cách nhanh chóng. Điều này tránh việc tính toán lại việc kiểm tra tính chia hết từ đầu cho mọi số. 
2. Với mỗi số nguyên$x$từ 2 đến$N$, tính tổng số thừa số nguyên tố của nó với bội số. Chúng tôi liên tục chia$x$bằng thừa số nguyên tố nhỏ nhất của nó và tích lũy số đếm. Điều này tạo ra một giá trị`cnt[x]`. 
3. Vì$k$nhiều nhất là 16, chúng tôi xây dựng tổng tiền tố cho mỗi giá trị có thể$k$. Chúng tôi duy trì`pref[k][i]`, nơi lưu trữ bao nhiêu số trong$[1, i]$có chính xác$k$các yếu tố chính. Chúng tôi điền nó tăng dần bằng cách truyền bá các giá trị trước đó và thêm 1 khi`cnt[i] == k`. 
4. Đối với mỗi truy vấn$(l, r, k)$, chúng tôi trả lời trong thời gian không đổi bằng cách sử dụng:$$pref[k][r] - pref[k][l-1]$$5. Xuất kết quả cho từng truy vấn. 

Mối quan tâm thực hiện chính là giữ cho hệ số tuyến tính hoặc gần tuyến tính. Việc sử dụng tiền xử lý hệ số nguyên tố nhỏ nhất đảm bảo rằng mỗi số được phân tách theo thời gian logarit trong thực tế. 

### Tại sao nó hoạt động 

Tính đúng đắn dựa trên hai bất biến. Đầu tiên,`cnt[x]`chính xác là tổng số thừa số nguyên tố của$x$, bởi vì mỗi bước chia sẽ loại bỏ chính xác một thừa số nguyên tố và tất cả các thừa số đều được tính bằng phép chia lặp lại cho các số nguyên tố nhỏ nhất. Thứ hai, mảng tiền tố duy trì số lần xuất hiện tích lũy của mỗi$k$-lớp học. Vì mỗi số nguyên đóng góp độc lập vào đúng một$k$, sự khác biệt về tiền tố sẽ tách biệt chính xác số lượng trên bất kỳ phân đoạn nào mà không bị chồng chéo hoặc bỏ sót. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

N = 100000

spf = list(range(N + 1))

for i in range(2, int(N ** 0.5) + 1):
    if spf[i] == i:
        for j in range(i * i, N + 1, i):
            if spf[j] == j:
                spf[j] = i

cnt = [0] * (N + 1)

for i in range(2, N + 1):
    x = i
    c = 0
    while x > 1:
        p = spf[x]
        while x % p == 0:
            x //= p
            c += 1
    cnt[i] = c

MAXK = 16
pref = [[0] * (N + 1) for _ in range(MAXK + 1)]

for i in range(1, N + 1):
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
```Giải pháp trước tiên xây dựng các thừa số nguyên tố nhỏ nhất để việc phân tích nhân tử trở nên xác định và nhanh chóng. Sau đó, mỗi số được rút gọn thành các thành phần nguyên tố của nó, tích lũy các bội số một cách chính xác thay vì chỉ các số nguyên tố riêng biệt. Bảng tiền tố được xây dựng theo cách mỗi hàng tương ứng với một giá trị cố định$k$, vì vậy các truy vấn không bao giờ yêu cầu phạm vi quét. 

Một cạm bẫy triển khai phổ biến là quên rằng 1 đóng góp 0 yếu tố. Ở đây nó được xử lý một cách tự nhiên vì`cnt[1]`vẫn bằng 0 và được bao gồm trong tiền tố của$k = 0$. 

Một vấn đề tế nhị khác là cập nhật mảng tiền tố một cách hiệu quả. Tính toán lại số lượng đầy đủ cho mỗi$k$độc lập cho mỗi chỉ mục sẽ quá chậm; thay vào đó, chúng tôi sao chép các giá trị tiền tố trước đó và chỉ cập nhật các giá trị có liên quan$k$xô cho mỗi chỉ mục. 

## Ví dụ đã hoạt động 

Hãy xem xét một phân khúc nhỏ nơi chúng ta có thể thấy rõ số lượng yếu tố. 

### Ví dụ 1 

đầu vào:```
l = 2, r = 10, k = 2
```Chúng tôi tính toán số lượng yếu tố nguyên tố: 

| x | nhân tử hóa | cnt[x] | 
| --- | --- | --- | 
| 2 | 2 | 1 | 
| 3 | 3 | 1 | 
| 4 | 2^2 | 2 | 
| 5 | 5 | 1 | 
| 6 | 2·3 | 2 | 
| 7 | 7 | 1 | 
| 8 | 2^3 | 3 | 
| 9 | 3^2 | 2 | 
| 10 | 2·5 | 2 | 

Đếm những người có$k = 2$cho 4 số: 4, 6, 9, 10. 

Điều này khớp với sự khác biệt về tiền tố sẽ trả về:`pref[2][10] - pref[2][1]`. 

### Ví dụ 2 

đầu vào:```
l = 12, r = 15, k = 3
```| x | nhân tử hóa | cnt[x] | 
| --- | --- | --- | 
| 12 | 2^2·3 | 3 | 
| 13 | 13 | 1 | 
| 14 | 2·7 | 2 | 
| 15 | 3·5 | 2 | 

Chỉ có 12 thỏa mãn$k = 3$, vậy đáp án là 1 

Điều này chứng tỏ số bội quan trọng như thế nào vì 12 tích lũy ba thừa số. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N \log \log N + q)$| tiền xử lý dạng sàng cộng với các truy vấn có thời gian cố định | 
| Không gian |$O(NK)$| bảng tiền tố cho mỗi$k \le 16$| 

Quá trình tiền xử lý là tuyến tính trong một giới hạn nhỏ$10^5$và mỗi truy vấn được trả lời trong thời gian không đổi, phù hợp thoải mái trong giới hạn ngay cả đối với$10^5$truy vấn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from math import isclose

    N = 100000
    spf = list(range(N + 1))
    for i in range(2, int(N ** 0.5) + 1):
        if spf[i] == i:
            for j in range(i * i, N + 1, i):
                if spf[j] == j:
                    spf[j] = i

    cnt = [0] * (N + 1)
    for i in range(2, N + 1):
        x = i
        c = 0
        while x > 1:
            p = spf[x]
            while x % p == 0:
                x //= p
                c += 1
        cnt[i] = c

    MAXK = 16
    pref = [[0] * (N + 1) for _ in range(MAXK + 1)]

    for i in range(1, N + 1):
        for k in range(MAXK + 1):
            pref[k][i] = pref[k][i - 1]
        k = cnt[i]
        if k <= MAXK:
            pref[k][i] += 1

    q = int(_sys.stdin.readline())
    out = []
    for _ in range(q):
        l, r, k = map(int, _sys.stdin.readline().split())
        if k > MAXK:
            out.append("0")
        else:
            out.append(str(pref[k][r] - pref[k][l - 1]))
    return "\n".join(out)

# sample-style sanity checks
assert run("1\n2 10 2\n") == "4"
assert run("1\n12 15 3\n") == "1"
assert run("1\n1 1 0\n") == "1"
assert run("1\n1 10 10\n") == "0"
assert run("3\n2 10 1\n12 15 3\n10 20 2\n") == "4\n1\n3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\n1 1 0\n`|`1`| xử lý số 1 dưới dạng thừa số nguyên tố 0 | 
|`1\n1 10 10\n`|`0`| k lớn hơn số lượng yếu tố có thể | 
|`3 queries sample`|`4\n1\n3`| tính chính xác trên nhiều truy vấn | 

## Vỏ cạnh 

Đối với giá trị 1, thuật toán gán`cnt[1] = 0`ngầm thông qua việc khởi tạo. Khi một truy vấn yêu cầu$k = 0$, mảng tiền tố bao gồm chính xác 1, vì nó đóng góp chính xác một lần vào nhóm loại 0. 

Đối với lớn$k$các giá trị như 15 hoặc 16, nhiều số trong phạm vi không đạt đến số lượng hệ số đó. Bảng tiền tố vẫn xử lý chúng một cách an toàn vì các nhóm đó vẫn bằng 0 trong suốt quá trình tiền xử lý, do đó phép trừ mang lại kết quả bằng 0 nếu không có logic trong trường hợp đặc biệt. 

Truy vấn với$l = 1$dựa vào việc truy cập`pref[k][0]`. Mảng tiền tố được khởi tạo bằng 0 tại chỉ số 0, do đó phép trừ`pref[k][r] - pref[k][0]`trả về chính xác số lượng phạm vi đầy đủ mà không có lỗi tràn hoặc lập chỉ mục.
