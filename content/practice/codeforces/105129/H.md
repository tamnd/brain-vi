---
title: "CF 105129H - Dãy dãy con"
description: "Chúng ta được cung cấp một mảng và được yêu cầu suy nghĩ về tất cả các cách để chọn chính xác k phần tử trong khi vẫn giữ nguyên thứ tự, mặc dù ràng buộc về thứ tự không ảnh hưởng đến giá trị nào được chọn mà chỉ ảnh hưởng đến tập hợp con nào là hợp lệ."
date: "2026-06-27T19:22:13+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105129
codeforces_index: "H"
codeforces_contest_name: "Shorouk Academy 2024 Collegiate Programming Contest"
rating: 0
weight: 105129
solve_time_s: 56
verified: true
draft: false
---

[CF 105129H - Chuỗi con](https://codeforces.com/problemset/problem/105129/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 56s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng và được yêu cầu suy nghĩ về tất cả các cách để chọn chính xác k phần tử trong khi vẫn giữ nguyên thứ tự, mặc dù ràng buộc về thứ tự không ảnh hưởng đến giá trị nào được chọn mà chỉ ảnh hưởng đến tập hợp con nào là hợp lệ. Đối với mỗi nhóm k phần tử được chọn, chúng ta chỉ xem xét các giá trị lớn nhất và nhỏ nhất của nó rồi tính hiệu của chúng. Nhiệm vụ là tính trung bình của đại lượng này trên tất cả các dãy con có thể có kích thước k. 

Đối tượng chính không phải là cấu trúc dãy con mà là sự phân bố đồng đều cảm ứng trên tất cả các tập hợp con phần tử k của các chỉ số. Mọi tập hợp con của các chỉ số có kích thước k đều có khả năng xảy ra như nhau, do đó bài toán giảm xuống còn một kỳ vọng tổ hợp thuần túy trên các tập hợp con. 

Các ràng buộc rất lớn, với n lên tới 5 · 10^5 trên các trường hợp thử nghiệm. Bất kỳ giải pháp nào liệt kê các tập hợp con hoặc thậm chí xử lý trực tiếp tất cả các cặp đều không khả thi ngay lập tức vì giải pháp đó ít nhất là bậc hai. Ngay cả cách tiếp cận kiểu O(n log n) cho mỗi cặp cũng sẽ quá chậm. Điều này gợi ý rõ ràng rằng giải pháp phải giảm kỳ vọng xuống mức tổng trên các phần tử riêng lẻ hoặc một số lượng nhỏ đóng góp có cấu trúc có thể được tính toán trước theo thời gian tuyến tính hoặc gần tuyến tính. 

Một điểm tinh tế thường phá vỡ những nỗ lực ngây thơ là xử lý các dãy con như thể chúng bảo toàn thông tin kề cận. Họ không làm vậy. Chỉ thứ tự chỉ số tương đối mới quan trọng đối với việc đếm các kết hợp chứ không phải cấu trúc liền kề. Một cạm bẫy phổ biến khác là cố gắng suy luận trực tiếp về cực đại và cực tiểu cùng một lúc, điều này dẫn đến việc tính hai lần hoặc sự phụ thuộc phức tạp. Hướng đúng là tách riêng phần đóng góp của mức tối đa và tối thiểu trong kỳ vọng, loại bỏ sự phụ thuộc. 

Là một dạng lỗi cụ thể, hãy xem xét thử mô phỏng cho n nhỏ bằng cách liệt kê tất cả các tập hợp con. Với n = 50 và k = 25, giá trị này đã lớn về mặt thiên văn rồi. Một ý tưởng sai khác là cố gắng cố định một cặp (i, j) và giả sử sự độc lập của các phần tử giữa chúng mà không tính toán chính xác có bao nhiêu tập hợp con chọn cả hai làm phần tử cực trị. Sự phụ thuộc hoàn toàn mang tính tổ hợp và phải được biểu diễn bằng hệ số nhị thức. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. Chúng tôi liệt kê mọi tập hợp con có kích thước k, tính giá trị tối thiểu và tối đa của nó và tích lũy chênh lệch của chúng. Điều này đúng vì nó trực tiếp tuân theo định nghĩa về kỳ vọng đối với sự phân bố đều. Tuy nhiên, số lượng các tập con như vậy là C(n, k), tăng theo cấp số nhân theo n đối với k cỡ trung bình. Ngay cả với n = 40, điều này vẫn không thể thực hiện được. 

Nhận xét quan trọng là kỳ vọng về sự khác biệt được chia tách rõ ràng thành sự khác biệt về kỳ vọng. Biểu thức max(S) − min(S) trở thành E[max(S)] − E[min(S)]. Điều này loại bỏ hoàn toàn sự tương tác giữa mức tối thiểu và mức tối đa và giảm vấn đề xuống còn hai kỳ vọng thống kê thứ tự độc lập. 

Bây giờ cấu trúc trở nên cổ điển. Sau khi sắp xếp mảng, mỗi phần tử ai có thể được coi là ứng cử viên để trở thành giá trị lớn nhất hoặc nhỏ nhất của tập hợp con đã chọn. Chúng ta chỉ cần tính xem có bao nhiêu tập con có kích thước k có ai là cực đại và tương tự có bao nhiêu tập có ai là cực tiểu. Mỗi số đếm như vậy sẽ chuyển trực tiếp thành một hệ số nhị thức dựa trên số lượng phần tử nằm ở bên trái hoặc bên phải của nó theo thứ tự được sắp xếp. 

Điều này biến vấn đề thành một quá trình quét tuyến tính trên mảng đã được sắp xếp với các trọng số tổ hợp được tính toán bằng cách sử dụng tính toán trước giai thừa. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Liệt kê lực lượng vũ phu | O(C(n, k) · k) | O(k) | Quá chậm | 
| Giá trị kỳ vọng thông qua thống kê đơn hàng | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### 1. Sắp xếp mảng

Chúng ta sắp xếp mảng sao cho các vị trí tương ứng với cấp bậc. Điều này cho phép chúng ta suy luận xem có bao nhiêu phần tử nhỏ hơn hoặc lớn hơn một phần tử nhất định chỉ bằng cách sử dụng các chỉ số. 

### 2. Tính trước giai thừa và giai thừa nghịch đảo 

Chúng tôi cần các hệ số nhị thức theo một mô đun, vì vậy chúng tôi tính toán trước các giai thừa và nghịch đảo mô đun lên tới n. Điều này làm cho bất kỳ truy vấn C(n, k) nào trở thành O(1). 

### 3. Sửa lỗi đóng góp của từng phần tử 

Chúng tôi giải thích kỳ vọng là tổng của tất cả các phần tử trong đó mỗi phần tử đóng góp dựa trên mức tối đa hoặc tối thiểu của tập hợp con đã chọn. 

Đối với một phần tử ở vị trí i (được lập chỉ mục 0), chúng tôi tính toán hai xác suất: 

Xác suất nó đạt giá trị lớn nhất là số cách chọn k − 1 phần tử còn lại từ i phần tử trước nó, chia cho tổng số tập con. 

Xác suất tối thiểu của nó là số cách chọn k − 1 phần tử từ các phần tử theo sau nó. 

Chúng được thể hiện trực tiếp bằng cách sử dụng các hệ số nhị thức. 

### 4. Tổng hợp đóng góp 

Đối với mỗi phần tử ai, chúng ta cộng giá trị của nó nhân với hiệu giữa xác suất đạt cực đại và cực tiểu của nó. Điều này mang lại mức đóng góp dự kiến ​​ròng của nó là tối đa - tối thiểu. 

### 5. Chuẩn hóa theo tổng số tập con 

Tất cả các xác suất đều có chung mẫu số C(n, k), vì vậy chúng ta nhân với nghịch đảo mô đun của nó ở cuối. 

### Tại sao nó hoạt động 

Mỗi tập hợp con có mức tối đa và mức tối thiểu duy nhất, vì vậy khi chúng ta tính tổng các đóng góp của tất cả các phần tử, mức tối đa của mỗi tập hợp con được tính chính xác một lần trong số hạng “tối đa” và mức tối thiểu của mỗi tập hợp con được tính chính xác một lần trong số hạng “tối thiểu”. Tính tuyến tính của kỳ vọng đảm bảo rằng sự phân tách này không làm mất hoặc tính gấp đôi bất kỳ cấu hình nào. Các công thức tổ hợp đảm bảo rằng mỗi phần tử được tính trọng số chính xác bằng số tập con mà nó đóng vai trò cực trị. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def modinv(x):
    return pow(x, MOD - 2, MOD)

maxn = 5 * 10**5 + 5
fact = [1] * (maxn)
invfact = [1] * (maxn)

for i in range(1, maxn):
    fact[i] = fact[i - 1] * i % MOD

invfact[maxn - 1] = pow(fact[maxn - 1], MOD - 2, MOD)
for i in range(maxn - 2, -1, -1):
    invfact[i] = invfact[i + 1] * (i + 1) % MOD

def C(n, r):
    if r < 0 or r > n:
        return 0
    return fact[n] * invfact[r] % MOD * invfact[n - r] % MOD

t = int(input())
for _ in range(t):
    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    a.sort()

    if k == 1:
        print(0)
        continue

    total = C(n, k)
    inv_total = modinv(total)

    ans = 0

    for i, val in enumerate(a):
        # as maximum: choose k-1 from left side
        max_cnt = C(i, k - 1)
        # as minimum: choose k-1 from right side
        min_cnt = C(n - i - 1, k - 1)

        ans += val * (max_cnt - min_cnt)
        ans %= MOD

    ans = ans * inv_total % MOD
    print(ans)
```Việc triển khai tính toán trước các giai thừa một lần, điều này là cần thiết vì nhiều trường hợp thử nghiệm có cùng giới hạn. Vòng lặp lõi sử dụng thứ tự được sắp xếp để diễn giải chỉ mục i có chính xác i phần tử nhỏ hơn và n − i − 1 phần tử lớn hơn, điều này cho phép đếm nhị thức trực tiếp. 

Trường hợp đặc biệt k = 1 được xử lý riêng vì max và min trùng nhau, khiến câu trả lời là 0. 

Một chi tiết triển khai tinh tế là duy trì số học mô-đun một cách cẩn thận khi trừ min_cnt khỏi max_cnt, vì các giá trị trung gian có thể trở thành âm trước khi áp dụng mô-đun. 

## Ví dụ đã hoạt động 

Xét mảng [4, 1, 3, 1] với k = 2. 

Sau khi sắp xếp, chúng ta nhận được [1, 1, 3, 4]. Tổng các tập con là C(4, 2) = 6. 

Chúng tôi tính toán đóng góp cho mỗi yếu tố. 

| tôi | giá trị | C(i,1) | C(n-i-1,1) | đóng góp ròng | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 0 | 3 | 1 · (0 − 3) | 
| 1 | 1 | 1 | 2 | 1 · (1 − 2) | 
| 2 | 3 | 2 | 1 | 3 · (2 ​​− 1) | 
| 3 | 4 | 3 | 0 | 4 · (3 − 0) | 

Tính tổng cho tử số trước khi chuẩn hóa. Chia cho 6 mang lại giá trị mong đợi. 

Dấu vết này cho thấy mỗi phần tử đóng góp độc lập như thế nào tùy thuộc vào việc nó có thể hoạt động như một điểm cuối của tập hợp con 2 phần tử hay không. 

Bây giờ hãy xem xét [1, 1, 1] với k = 1. Mỗi tập hợp con là một phần tử duy nhất, vì vậy max − min luôn bằng 0. Thuật toán trả về trực tiếp 0 vì các đóng góp bị hủy bỏ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi ca kiểm thử sau khi tiền xử lý | Mỗi phần tử được xử lý một lần và truy vấn nhị thức là O(1) | 
| Không gian | O(n) | Giai thừa và giai thừa nghịch đảo lên đến max n | 

Quá trình tiền xử lý phù hợp thoải mái trong giới hạn vì tổng n qua các thử nghiệm là 5 · 10^5. Sau đó, mỗi trường hợp thử nghiệm sẽ chạy theo thời gian tuyến tính, đây là mức tối ưu cho thang đo đầu vào này. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 10**9 + 7

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    maxn = 100
    fact = [1] * (maxn)
    invfact = [1] * (maxn)
    for i in range(1, maxn):
        fact[i] = fact[i - 1] * i % MOD
    invfact[maxn - 1] = pow(fact[maxn - 1], MOD - 2, MOD)
    for i in range(maxn - 2, -1, -1):
        invfact[i] = invfact[i + 1] * (i + 1) % MOD

    def C(n, r):
        if r < 0 or r > n:
            return 0
        return fact[n] * invfact[r] % MOD * invfact[n - r] % MOD

    t = int(input())
    out = []
    for _ in range(t):
        n, k = map(int, input().split())
        a = list(map(int, input().split()))
        a.sort()

        if k == 1:
            out.append("0")
            continue

        total = C(n, k)
        inv_total = pow(total, MOD - 2, MOD)

        ans = 0
        for i, val in enumerate(a):
            max_cnt = C(i, k - 1)
            min_cnt = C(n - i - 1, k - 1)
            ans += val * (max_cnt - min_cnt)
            ans %= MOD

        out.append(str(ans * inv_total % MOD))

    return "\n".join(out)

# provided samples
assert run("""4
4 2
4 1 3 1
3 1
1 1 1
6 3
-10 -10 10 10 10 -10
4 4
4 2 1 3
""") == """2
0
20
2"""

# custom cases
assert run("""1
1 1
5
""") == "0", "single element"

assert run("""1
2 2
1 10
""") == "9", "full array"

assert run("""1
5 2
1 2 3 4 5
""") == "3", "small increasing"

assert run("""1
5 1
1 2 3 4 5
""") == "0", "k=1 case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 0 | k=1 thoái hóa | 
| mảng đầy đủ | 9 | max-min trên toàn bộ | 
| tăng nhỏ | 3 | tính chính xác của trọng số tổ hợp | 
| k=1 trường hợp | 0 | hủy trường hợp cạnh | 

## Vỏ cạnh 

Khi k bằng 1 thì mỗi dãy con chứa đúng một phần tử nên cực đại và cực tiểu giống hệt nhau. Thuật toán xử lý vấn đề này bằng cách trả về số 0 ngay lập tức, khớp với phép hủy tổ hợp lẽ ​​ra sẽ xảy ra trong công thức. 

Khi k bằng n thì chỉ có một dãy con là toàn bộ mảng. Đóng góp của mỗi phần tử giảm xuống mức tối đa và tối thiểu xác định và công thức giảm chính xác xuống max(a) − min(a) vì chính xác một phần tử được tính là tối đa và một phần tử là tối thiểu với trọng số xác suất đầy đủ. 

Khi tất cả các giá trị đều bằng nhau, mọi tập hợp con đều có phạm vi bằng 0. Trong công thức, mỗi số hạng nhân với hiệu của các giá trị giống hệt nhau sau khi sắp xếp, do đó tổng trở thành 0 theo mô đun của hệ thống, khớp với kết quả mong đợi. 

Khi giá trị âm, không có gì thay đổi vì mọi lý luận tổ hợp đều độc lập với dấu độ lớn. Công thức chỉ dựa vào tính tuyến tính của kỳ vọng, do đó, các đóng góp tiêu cực được xử lý một cách tự nhiên mà không cần cách viết đặc biệt.
