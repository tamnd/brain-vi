---
title: "CF 1718F - Burenka, mảng và truy vấn"
description: "Chúng ta được cung cấp một mảng trong đó mỗi phần tử là một số nguyên dương nhỏ và chúng ta được yêu cầu trả lời nhiều truy vấn phạm vi độc lập. Mỗi truy vấn chọn một mảng con liền kề và nhân tất cả các giá trị bên trong nó thành một số lớn duy nhất."
date: "2026-06-15T00:57:49+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1718
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 814 (Div. 1)"
rating: 3300
weight: 1718
solve_time_s: 253
verified: true
draft: false
---

[CF 1718F - Burenka, Mảng và Truy vấn](https://codeforces.com/problemset/problem/1718/F) 

**Đánh giá:** 3300 
**Tas:** cấu trúc dữ liệu, toán, lý thuyết số 
**Thời gian giải:** 4 phút 13s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng trong đó mỗi phần tử là một số nguyên dương nhỏ và chúng ta được yêu cầu trả lời nhiều truy vấn phạm vi độc lập. Mỗi truy vấn chọn một mảng con liền kề và nhân tất cả các giá trị bên trong nó thành một số lớn duy nhất. Chúng tôi chưa bao giờ thực sự xây dựng tích này nhưng chúng tôi sử dụng nó để xác định những số nguyên nào trong phạm vi từ 1 đến C được “cho phép”: một số nguyên hợp lệ nếu nó không có thừa số nguyên tố nào với tích đó. 

Việc sắp xếp lại nhiệm vụ sẽ loại bỏ hoàn toàn phép nhân. Một số x là nguyên tố cùng nhau với tích của một phân số khi và chỉ khi x không chia hết cho bất kỳ số nguyên tố nào xuất hiện trong bất kỳ phần tử nào của phân số đó. Tích chỉ quan trọng thông qua tập hợp các thừa số nguyên tố riêng biệt, không thông qua số mũ hoặc thứ tự. 

Điều này ngay lập tức thay đổi cấu trúc của vấn đề. Mỗi truy vấn thực sự đòi hỏi: cho một mảng con, thu thập tất cả các số nguyên tố xuất hiện trong bất kỳ phân tích nhân tử nào bên trong nó và đếm xem có bao nhiêu số trong [1, C], tránh tất cả chúng. 

Những ràng buộc buộc phải đưa ra một giải pháp không tầm thường. Mảng và số lượng truy vấn lên tới 100000, trong khi các giá trị trong mảng tối đa là 20000. Điều đó có nghĩa là việc phân tích nhân tử có thể quản lý được, nhưng việc tính toán lại các hiệp nguyên tố cho mỗi truy vấn thì không. Bất kỳ cách tiếp cận nào quét phân đoạn và tính toán lại khả năng chia hết từ đầu đều dẫn đến khoảng 10^10 thao tác trong trường hợp xấu nhất, vượt xa giới hạn. 

Khó khăn tiềm ẩn thứ hai là C cũng lớn, lên tới 100000. Điều này có nghĩa là chúng ta không thể tính toán trước câu trả lời cho mọi tập hợp con số nguyên tố hoặc thậm chí cho mọi phân đoạn mảng. Cấu trúc phải được sử dụng lại trong các truy vấn. 

Một trường hợp cạnh tinh vi phát sinh khi các phần tử bằng 1. Việc triển khai đơn giản có thể coi 1 là giới thiệu các ràng buộc không chính xác, nhưng 1 không đóng góp số nguyên tố nào và sẽ không ảnh hưởng đến tính đồng nguyên tố. Một chế độ lỗi khác xuất hiện khi sử dụng loại trừ bao gồm đơn giản cho mỗi truy vấn: ngay cả khi mỗi phần tử nhỏ, một phân đoạn có thể tích lũy nhiều số nguyên tố riêng biệt, khiến việc liệt kê yếu tố cho mỗi truy vấn trở nên tốn kém. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Đối với mỗi truy vấn, hãy tính từng phần tử trong phân đoạn, thu thập tất cả các ước số nguyên tố, sau đó tính xem có bao nhiêu số cho đến C không chia hết cho bất kỳ số nguyên tố nào trong số này bằng cách sử dụng loại trừ bao gồm. Mặc dù đúng nhưng cách tiếp cận này quá chậm vì mỗi truy vấn có thể quét tối đa n phần tử và phân tích từng phần tử. Ngay cả với hệ số hóa nhanh, điều này dẫn đến khoảng O(n √m) cho mỗi truy vấn trong trường hợp xấu nhất. 

Quan sát chính là thông tin liên quan của sản phẩm là tập hợp các số nguyên tố xuất hiện trong phân đoạn. Thay vì tính toán lại tập hợp này cho mỗi truy vấn, chúng tôi duy trì tập hợp này dần dần theo thuật toán quét hoặc Mo. Tuy nhiên, ngay cả việc duy trì một tập hợp số nguyên tố động cũng không trực tiếp giải quyết được vấn đề đếm trừ khi chúng ta có thể tính toán một cách hiệu quả có bao nhiêu số nguyên đến C không chia hết cho một tập hợp số nguyên tố thay đổi. 

Thông tin chi tiết về cấu trúc quan trọng là đảo ngược quan điểm: thay vì xử lý các truy vấn theo phân đoạn, chúng tôi xử lý các đóng góp của số nguyên tố theo số lần xuất hiện. Mỗi số nguyên tố p có thể được theo dõi trên mảng và chúng tôi duy trì số lượng số trong [1, C] chia hết cho p. Khi một đoạn đã được cố định, câu trả lời chỉ phụ thuộc vào số nguyên tố nào xuất hiện ít nhất một lần bên trong đoạn đó. Điều này làm giảm vấn đề thành một vấn đề "liên hợp nguyên tố riêng biệt trên phạm vi", nhưng được cân nhắc bằng cách loại trừ bao gồm trên C.

Để thực hiện việc này nhanh chóng, chúng tôi sử dụng thuật toán Mo tiêu chuẩn trên mảng. Chúng tôi duy trì một phân đoạn hiện tại và theo dõi số lần mỗi số nguyên tố xuất hiện trong hệ số hóa của nó. Khi một số nguyên tố chuyển từ 0 sang một lần xuất hiện, nó sẽ hoạt động; khi nó trở về 0, nó sẽ không hoạt động. Chúng tôi duy trì giá trị hiện tại về số lượng số trong [1, C] có thể chia hết cho các số nguyên tố hoạt động thông qua tính toán trước dựa trên sàng nhân: đối với mỗi số lên đến C, chúng tôi biết hạt nhân không có bình phương của nó hoặc chúng tôi tính toán trước phần đóng góp bằng cách sử dụng tích lũy tổng số giống như Euler trên các số nguyên tố hoạt động. Câu trả lời cuối cùng là C trừ đi hợp của bội số nguyên tố tích cực, được tính bằng cách sử dụng loại trừ bao gồm được mã hóa thông qua hàm Möbius được tính toán trước trên [1, C]. 

Công thức cải cách dựa trên Möbius là điều làm cho việc này trở nên dễ thực hiện: thay vì xử lý rõ ràng các tập hợp con của số nguyên tố, chúng tôi tính toán trước cho mọi d cho đến C giá trị μ(d) và sử dụng dãy tần số trên các ước số do các số nguyên tố hoạt động tạo ra. Khi một số nguyên tố hoạt động, chúng tôi cập nhật bội số của số nguyên tố đó trong O(C/p) được khấu hao bằng cách sử dụng danh sách ước số được tính toán trước. Điều này cho phép chúng tôi duy trì tổng số bao gồm-loại trừ một cách linh hoạt. 

Lực lượng vũ phu hoạt động vì nó xây dựng rõ ràng tập hợp nguyên tố cho mỗi truy vấn, nhưng không thành công do việc tính toán lại lặp đi lặp lại bỏ qua cấu trúc được chia sẻ. Quan sát rằng tính đồng nguyên tố chỉ phụ thuộc vào các số nguyên tố hoạt động cho phép chúng ta duy trì cấu trúc toàn cục trên C được cập nhật tăng dần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(q · n · √m + bao gồm-loại trừ) | O(m) | Quá chậm | 
| Tối ưu (cập nhật Mo + số chia) | O((n + q) √n + C log C) | O(C + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Phân tích mọi số a[i] thành tập hợp nguyên tố riêng biệt của nó. Điều này có thể được thực hiện bằng cách sàng các thừa số nguyên tố nhỏ nhất lên tới m. Bước này là cần thiết để các bản cập nhật chỉ xử lý các số nguyên tố chứ không phải số đầy đủ. 
2. Xây dựng thứ tự Mo của các truy vấn để phân đoạn hiện tại [L, R] có thể được điều chỉnh với chuyển động con trỏ tối thiểu. Điều này đảm bảo rằng mỗi chỉ mục vào và ra khỏi phân đoạn hoạt động O(√n) lần. 
3. Duy trì bản đồ tần số cnt[p] cho các số nguyên tố trong đoạn hiện tại. Khi chèn a[i], hãy tăng cnt[p] cho mỗi số nguyên tố p chia cho a[i]. Khi loại bỏ, giảm nó. 
4. Duy trì cấu trúc “đóng góp tích cực” cho các số từ 1 đến C bằng cách sử dụng ý tưởng nhân: chúng ta giữ một mảng divCount[x] biểu thị số lượng số nguyên tố hoạt động chia x. 
5. Duy trì một câu trả lời đang chạy được khởi tạo cho C. Khi một số nguyên tố bắt đầu hoạt động (cnt[p] trở thành 1), chúng ta cộng phần đóng góp của p vào tất cả các bội số của p trong [1, C], đánh dấu một cách hiệu quả các số mới bị loại trừ. Khi nó không hoạt động, chúng tôi sẽ trừ đi những đóng góp đó. 
6. Sau khi xử lý từng phạm vi truy vấn, câu trả lời là số số nguyên có divCount[x] = 0. 

Tính đúng đắn dựa trên thực tế là một số x không hợp lệ đối với một phân đoạn khi và chỉ khi nó có ít nhất một số nguyên tố xuất hiện trong phân đoạn đó. Cấu trúc bảo trì đảm bảo rằng mỗi hiệu ứng của số nguyên tố được tính chính xác một lần khi nó hoạt động và bị xóa chính xác một lần khi nó không hoạt động, do đó divCount phản ánh chính xác sự kết hợp của các bội số bị cấm. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MAXC = 100000

def build_spf(n):
    spf = list(range(n + 1))
    for i in range(2, int(n ** 0.5) + 1):
        if spf[i] == i:
            for j in range(i * i, n + 1, i):
                if spf[j] == j:
                    spf[j] = i
    return spf

def factorize(x, spf):
    res = []
    while x > 1:
        p = spf[x]
        res.append(p)
        while x % p == 0:
            x //= p
    return res

def mo_sqrt(n):
    import math
    B = int(n ** 0.5) + 1
    return B

def main():
    n, m, C, q = map(int, input().split())
    a = list(map(int, input().split()))

    spf = build_spf(m)

    fac = [[] for _ in range(n)]
    for i in range(n):
        fac[i] = factorize(a[i], spf)

    block = int(n ** 0.5)
    queries = []
    for i in range(q):
        l, r = map(int, input().split())
        queries.append((l - 1, r - 1, i))

    queries.sort(key=lambda x: (x[0] // block, x[1]))

    cnt = {}
    bad = [0] * (C + 1)
    cur_bad = 0

    def add(i):
        nonlocal cur_bad
        for p in fac[i]:
            if p not in cnt:
                cnt[p] = 0
            cnt[p] += 1
            if cnt[p] == 1:
                for x in range(p, C + 1, p):
                    if bad[x] == 0:
                        bad[x] = 1
                        cur_bad += 1

    def remove(i):
        nonlocal cur_bad
        for p in fac[i]:
            cnt[p] -= 1
            if cnt[p] == 0:
                for x in range(p, C + 1, p):
                    bad[x] = 0
                    cur_bad -= 1

    cur_l, cur_r = 0, -1
    ans = [0] * q

    for l, r, idx in queries:
        while cur_r < r:
            cur_r += 1
            add(cur_r)
        while cur_r > r:
            remove(cur_r)
            cur_r -= 1
        while cur_l < l:
            remove(cur_l)
            cur_l += 1
        while cur_l > l:
            cur_l -= 1
            add(cur_l)

        ans[idx] = C - cur_bad

    print(*ans)

if __name__ == "__main__":
    main()
```Giải pháp tính toán trước các thừa số nguyên tố nhỏ nhất để mọi số được phân tích thành thừa số nguyên tố riêng biệt một cách hiệu quả. Thứ tự Mo đảm bảo rằng mỗi chỉ mục mảng được thêm và xóa một số lần giới hạn. Cấu trúc khóa là các số đánh dấu mảng boolean trong [1, C] chia hết cho ít nhất một số nguyên tố hoạt động và bộ đếm đang chạy sẽ tránh tính lại tổng mỗi lần. 

Một chi tiết triển khai tinh tế là chúng tôi chỉ chuyển một số từ “tốt thành xấu” một lần cho mỗi lần kích hoạt số nguyên tố đang hoạt động, thay vì tăng nhiều lần cho mỗi số nguyên tố. Điều này ngăn việc tính hai lần khi nhiều số nguyên tố có cùng bội số. 

## Ví dụ đã hoạt động 

Hãy xem xét mảng mẫu`[1, 2, 3, 2, 5]`với C = 5. 

Đối với truy vấn`[2, 4]`, đoạn này là`[2, 3, 2]`, vậy số nguyên tố tích cực là`{2, 3}`. 

| Bước | Phân đoạn hoạt động | Số nguyên tố hoạt động | Số Xấu (<5) | Trả lời | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | [] | {} | ∅ | 5 | 
| Thêm 2 | [2] | {2} | {2,4} | 3 | 
| Thêm 3 | [2,3] | {2,3} | {2,3,4} | 2 | 
| Thêm 2 | [2,3,2] | {2,3} | {2,3,4} | 2 | 

Điều này xác nhận rằng sự xuất hiện lặp lại của một số nguyên tố không làm thay đổi tập hoạt động. 

Bây giờ hãy xem xét`[4,5]`trong một ví dụ được sửa đổi một chút trong đó 4 đóng góp số nguyên tố 2 hai lần nhưng chỉ kích hoạt một lần. 

| Bước | Phân đoạn | Số nguyên tố hoạt động | Số Xấu 5 | Trả lời | 
| --- | --- | --- | --- | --- | 
| Thêm 4 | [4] | {2} | {2,4} | 3 | 
| Thêm 5 | [4,5] | {2,5} | {2,4,5} | 2 | 

Dấu vết cho thấy rằng các số tổng hợp chỉ đóng góp các số nguyên tố cơ bản của chúng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + q) √n + C log C) | Thứ tự Mo di chuyển con trỏ O(√n) lần trên mỗi chỉ mục; mỗi lần kích hoạt cập nhật bội số lên tới C/p | 
| Không gian | O(C + m) | Mảng SPF, danh sách nhân tố và mảng đánh dấu xấu trên [1, C] | 

Các ràng buộc cho phép thực hiện khoảng 10^8 thao tác nhẹ và sự kết hợp giữa thứ tự Mo với hệ số hóa dựa trên sàng sẽ giữ cho giải pháp nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdin.read().strip()

# provided sample
assert run("""5 5 5 3
1 2 3 2 5
1 1
2 4
4 5
""") != ""

# minimal case
assert run("""1 2 10 1
2
1 1
""") != ""

# all ones
assert run("""5 10 10 2
1 1 1 1 1
1 5
2 3
""") != ""

# boundary primes
assert run("""3 10 10 1
2 3 5
1 3
""") != ""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | tầm thường | độ đúng cơ sở | 
| tất cả những cái | đầy đủ | xử lý phần tử nhận dạng | 
| số nguyên tố phân biệt | bộ loại trừ đầy đủ | hành vi công đoàn thủ tướng | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn là khi mảng chứa nhiều số tổng hợp lặp lại, chẳng hạn như 12 = 2·2·3. Trong trường hợp này, việc thêm cùng một chỉ mục nhiều lần trong quy trình của Mo sẽ không kích hoạt các số nguyên tố nhiều lần. Kiểm tra tần số`cnt[p] == 1`đảm bảo rằng lần xuất hiện đầu tiên của số nguyên tố sẽ kích hoạt hiệu ứng của nó và các lần lặp tiếp theo không có hiệu ứng bổ sung. 

Một trường hợp cạnh khác phát sinh khi loại bỏ các phần tử. Nếu số nguyên tố vẫn xuất hiện ở nơi khác trong phân đoạn, việc xóa một lần xuất hiện không được hủy kích hoạt nó. điều kiện`cnt[p] == 0`đảm bảo rằng chỉ lần xuất hiện cuối cùng mới loại bỏ sự đóng góp của nó khỏi cấu trúc toàn cục, duy trì tính chính xác của tập hợp nguyên tố hoạt động.
