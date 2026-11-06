---
title: "CF 1302H - Ai cần cấu trúc hậu tố?"
description: "Chúng ta được cho một dãy có độ dài n. Mặc dù câu lệnh nói về một chuỗi nhưng các ký tự được biểu thị bằng số nguyên và bảng chữ cái cực kỳ lớn. Kích thước bảng chữ cái lớn có nghĩa là chúng ta không thể dựa vào các thủ thuật phụ thuộc vào một số lượng nhỏ các ký hiệu riêng biệt."
date: "2026-06-11T18:13:05+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 1302
codeforces_index: "H"
codeforces_contest_name: "AIM Tech Poorly Prepared Contest (unrated, funny, Div. 1 preferred)"
rating: 0
weight: 1302
solve_time_s: 112
verified: true
draft: false
---

[CF 1302H - Ai cần cấu trúc hậu tố?](https://codeforces.com/problemset/problem/1302/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 52s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy có độ dài`n`. Mặc dù câu lệnh nói về một chuỗi nhưng các ký tự được biểu thị bằng số nguyên và bảng chữ cái cực kỳ lớn. Kích thước bảng chữ cái lớn có nghĩa là chúng ta không thể dựa vào các thủ thuật phụ thuộc vào một số lượng nhỏ các ký hiệu riêng biệt. 

Mỗi truy vấn chỉ định hai vị trí bắt đầu và độ dài. Chúng ta phải xác định xem hai đoạn liền kề có độ dài đó có giống nhau hay không. 

Cụ thể hơn, đối với một truy vấn`(len, pos1, pos2)`, ta so sánh: 

-`a[pos1 ... pos1 + len - 1]`-`a[pos2 ... pos2 + len - 1]`và in`"Yes"`nếu mọi phần tử tương ứng đều khớp, nếu không`"No"`. 

Những hạn chế là phần then chốt của vấn đề. Cả hai`n`Và`q`có thể đạt tới 200.000. Việc so sánh trực tiếp hai chuỗi con cần`O(len)`thời gian. Trong trường hợp xấu nhất,`len`cũng là 200.000, vì vậy một truy vấn có thể yêu cầu 200.000 so sánh. Với 200.000 truy vấn, điều đó gần như trở thành`4 × 10^10`so sánh phần tử, vượt xa những gì có thể được thực hiện trong giới hạn 2 giây. 

Nhiệm vụ thực sự là một số lượng lớn các kiểm tra tính bằng nhau của chuỗi con trên một chuỗi tĩnh. Cấu trúc đó gợi ý mạnh mẽ việc xây dựng cấu trúc dữ liệu tiền xử lý có thể trả lời từng truy vấn theo thời gian không đổi hoặc logarit. 

Một số trường hợp đáng chú ý. 

Hãy xem xét các vị trí bắt đầu giống hệt nhau:```
n = 5
a = [1,2,3,4,5]

query: len=5, pos1=1, pos2=1
```Câu trả lời rõ ràng là`"Yes"`. Bất kỳ phương pháp so sánh chuỗi con chính xác nào cũng phải xử lý việc này mà không có trường hợp đặc biệt. 

Hãy xem xét các so sánh đạt đến cuối mảng:```
n = 5
a = [1,2,3,4,5]

query: len=2, pos1=4, pos2=4
```Đoạn so sánh là`[4,5]`. Những lỗi ngẫu nhiên trong tính toán tiền tố thường xuất hiện ở đây. 

Một trường hợp quan trọng khác là khi hai chuỗi con có chung tiền tố dài nhưng khác nhau ở cuối:```
a = [7,7,7,7,8]
query: len=5, pos1=1, pos2=?
```Việc tối ưu hóa bất cẩn chỉ kiểm tra một phần tiền tố có thể kết luận sai sự bằng nhau. Toàn bộ chiều dài phải được xác minh thông qua biểu diễn chống va chạm. 

Cuối cùng, bảng chữ cái chứa các giá trị lên tới 987,898,788. Bất kỳ cách tiếp cận nào giả định các ký tự vừa với một bảng chữ cái nhỏ, chẳng hạn như mảng tần số có kích thước cố định hoặc bảng chuyển tiếp được lập chỉ mục theo giá trị ký tự, đều không phù hợp. 

## Phương pháp tiếp cận 

Giải pháp brute-force rất đơn giản. Đối với mỗi truy vấn, hãy so sánh hai chuỗi con được yêu cầu theo từng phần tử. Nếu tất cả các vị trí khớp nhau, xuất ra`"Yes"`; nếu không thì xuất ra`"No"`. 

Phương pháp brute-force là đúng vì đẳng thức chuỗi con được xác định chính xác bằng đẳng thức ở mọi vị trí. Vấn đề là thời gian chạy của nó. Một truy vấn có thể yêu cầu`O(len)`làm việc và`len`Có lẽ`O(n)`. Với`q = 200000`Và`n = 200000`, trường hợp xấu nhất trở thành`O(nq)`, khoảng bốn mươi tỷ so sánh. 

Mảng không bao giờ thay đổi và mọi truy vấn đều hỏi cùng một loại câu hỏi: hai phạm vi có bằng nhau không? Thay vì liên tục kiểm tra các phần tử cơ bản, chúng ta có thể xử lý trước chuỗi và gán dấu vân tay nhỏ gọn cho mọi tiền tố. Sau đó, dấu vân tay của bất kỳ chuỗi con nào có thể được trích xuất trong thời gian không đổi. 

Băm lăn đa thức là một sự phù hợp tự nhiên. Cho phép$$H[i]$$biểu thị hàm băm của đầu tiên`i`các phần tử. Sử dụng lũy ​​thừa của một cơ sở cố định, hàm băm của bất kỳ chuỗi con nào có thể được phục hồi từ hai hàm băm tiền tố. 

Nếu hai chuỗi con bằng nhau thì giá trị băm của chúng bằng nhau. Với phép băm kép, xác suất các chuỗi con khác nhau tạo ra cùng một cặp băm sẽ không đáng kể. Sau khi tiền xử lý, mỗi truy vấn giảm xuống còn so sánh hai cặp băm, việc này mất thời gian không đổi. 

Giải pháp brute-force liên tục kiểm tra các yếu tố giống nhau. Băm nén từng chuỗi con thành một biểu diễn có kích thước không đổi, cho phép mỗi truy vấn được trả lời ngay lập tức. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nq) trường hợp xấu nhất | O(1) | Quá chậm | 
| Tối ưu (băm lăn đôi) | O(n + q) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc trình tự. 
2. Chọn hai mô đun lớn và một đế cố định. 
3. Tính toán trước lũy thừa của modulo cơ sở cả hai moduli. 

Những quyền hạn này cho phép chúng ta bình thường hóa băm chuỗi con trong thời gian không đổi. 
4. Xây dựng mảng băm tiền tố cho cả hai mô-đun. 

Đối với mỗi vị trí, hãy mở rộng hàm băm tiền tố trước đó bằng cách thêm giá trị hiện tại. 
5. Xác định hàm`get_hash(l, r)`trả về cặp băm của mảng con từ`l`ĐẾN`r`bao gồm. 

Sử dụng băm tiền tố:$$H(l,r)=H[r]-H[l-1]\cdot P^{r-l+1}$$modulo tính toán cho mỗi mô đun. 
6. Đối với mỗi truy vấn, hãy chuyển đổi các vị trí dựa trên 1 thành các điểm cuối khoảng tương ứng. 
7. Tính cặp băm của chuỗi con đầu tiên. 
8. Tính cặp băm của chuỗi con thứ hai. 
9. Nếu cả hai cặp băm giống hệt nhau, hãy in`"Yes"`, nếu không thì in`"No"`. 

### Tại sao nó hoạt động 

Cấu trúc băm tiền tố đảm bảo rằng mọi chuỗi con được biểu diễn bằng một biểu thức đa thức trên các phần tử của nó. Việc loại bỏ phần đóng góp của tiền tố trước chuỗi con sẽ mang lại một giá trị chỉ phụ thuộc vào chính chuỗi con đó. 

Hai chuỗi con bằng nhau tạo ra cùng một đa thức và do đó có cùng một cặp băm. Mỗi truy vấn so sánh các biểu diễn hàm băm của hai phạm vi được yêu cầu. Vì cả hai giá trị băm đều được trích xuất bằng cùng một công thức nên các chuỗi con bằng nhau luôn so sánh bằng nhau. Băm kép làm cho khả năng xảy ra va chạm vô tình là không đáng kể, đây là đảm bảo lập trình cạnh tranh tiêu chuẩn được sử dụng cho các truy vấn đẳng thức chuỗi con. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, q = map(int, input().split())
    a = list(map(int, input().split()))

    MOD1 = 1_000_000_007
    MOD2 = 1_000_000_009
    BASE = 911382323

    pow1 = [1] * (n + 1)
    pow2 = [1] * (n + 1)

    for i in range(1, n + 1):
        pow1[i] = (pow1[i - 1] * BASE) % MOD1
        pow2[i] = (pow2[i - 1] * BASE) % MOD2

    pref1 = [0] * (n + 1)
    pref2 = [0] * (n + 1)

    for i in range(n):
        pref1[i + 1] = (pref1[i] * BASE + a[i] + 1) % MOD1
        pref2[i + 1] = (pref2[i] * BASE + a[i] + 1) % MOD2

    def get_hash(l, r):
        h1 = (
            pref1[r]
            - pref1[l - 1] * pow1[r - l + 1]
        ) % MOD1

        h2 = (
            pref2[r]
            - pref2[l - 1] * pow2[r - l + 1]
        ) % MOD2

        return h1, h2

    ans = []

    for _ in range(q):
        length, p1, p2 = map(int, input().split())

        h_first = get_hash(p1, p1 + length - 1)
        h_second = get_hash(p2, p2 + length - 1)

        ans.append("Yes" if h_first == h_second else "No")

    sys.stdout.write("\n".join(ans))

if __name__ == "__main__":
    solve()
```Giai đoạn tiền xử lý xây dựng sức mạnh của hàm băm cơ sở và tiền tố. Cả hai mảng đều được lập chỉ mục từ 0, trong khi các truy vấn chuỗi con được biểu thị một cách tự nhiên bằng cách sử dụng các vị trí dựa trên 1, do đó công thức trích xuất hàm băm được viết trực tiếp ở dạng dựa trên 1. 

Thêm`1`vào từng phần tử trước khi chèn nó vào hàm băm sẽ ngăn các giá trị 0 trở nên vô hình ở các vị trí dẫn đầu. Nếu không có sự điều chỉnh này, một số biểu diễn đa thức sẽ trở nên kém hiệu quả hơn. 

các`get_hash`chức năng là phần quan trọng. Nó loại bỏ sự đóng góp của mọi thứ trước vị trí`l`và chỉ để lại đa thức tương ứng với khoảng được yêu cầu. Độ dài của khoảng thời gian xác định hệ số công suất nào phải được loại bỏ. 

Mỗi truy vấn tính toán hai cặp băm và so sánh chúng. Không cần phải duyệt qua chuỗi con cơ bản. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
5 2
1 2 3 1 2
2 1 4
3 1 3
```Các chuỗi con liên quan là: 

| Truy vấn | Chuỗi con đầu tiên | Chuỗi con thứ hai | Kết quả | 
| --- | --- | --- | --- | 
| (2,1,4) | [1,2] | [1,2] | Có | 
| (3,1,3) | [1,2,3] | [3,1,2] | Không | 

So sánh băm hoạt động như sau: 

| Truy vấn | Phạm vi 1 | Phạm vi 2 | Băm bằng nhau? | Đầu ra | 
| --- | --- | --- | --- | --- | 
| 1 | [1,2] | [4,5] | Có | Có | 
| 2 | [1,3] | [3,5] | Không | Không | 

Ví dụ này thể hiện ý tưởng chính: kiểm tra tính bằng nhau trở thành so sánh hàm băm thay vì so sánh phần tử. 

### Ví dụ tùy chỉnh 

đầu vào:```
6 3
5 5 5 5 5 5
1 1 6
3 1 4
6 1 1
```Tất cả các chuỗi con bao gồm toàn bộ giá trị`5`. 

| Truy vấn | Phạm vi 1 | Phạm vi 2 | Băm bằng nhau? | Đầu ra | 
| --- | --- | --- | --- | --- | 
| 1 | [1,1] | [6,6] | Có | Có | 
| 2 | [1,3] | [4,6] | Có | Có | 
| 3 | [1,6] | [1,6] | Có | Có | 

Ví dụ này cho thấy các giá trị lặp lại không gây khó khăn gì. Các phạm vi bằng nhau luôn tạo ra các giá trị băm giống hệt nhau bất kể chúng xuất hiện ở đâu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + q) | Tiền xử lý O(n) và O(1) hoạt động trên mỗi truy vấn | 
| Không gian | O(n) | Bảng băm tiền tố và bảng lũy ​​thừa | 

Với`n, q ≤ 200000`, tiền xử lý tuyến tính và truy vấn thời gian không đổi dễ dàng đủ nhanh. Việc sử dụng bộ nhớ cũng thoải mái trong giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    MOD1 = 1_000_000_007
    MOD2 = 1_000_000_009
    BASE = 911382323

    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    input = sys.stdin.readline

    n, q = map(int, input().split())
    a = list(map(int, input().split()))

    pow1 = [1] * (n + 1)
    pow2 = [1] * (n + 1)

    for i in range(1, n + 1):
        pow1[i] = (pow1[i - 1] * BASE) % MOD1
        pow2[i] = (pow2[i - 1] * BASE) % MOD2

    pref1 = [0] * (n + 1)
    pref2 = [0] * (n + 1)

    for i in range(n):
        pref1[i + 1] = (pref1[i] * BASE + a[i] + 1) % MOD1
        pref2[i + 1] = (pref2[i] * BASE + a[i] + 1) % MOD2

    def get_hash(l, r):
        return (
            (pref1[r] - pref1[l - 1] * pow1[r - l + 1]) % MOD1,
            (pref2[r] - pref2[l - 1] * pow2[r - l + 1]) % MOD2,
        )

    ans = []

    for _ in range(q):
        length, p1, p2 = map(int, input().split())
        ans.append(
            "Yes"
            if get_hash(p1, p1 + length - 1)
            == get_hash(p2, p2 + length - 1)
            else "No"
        )

    sys.stdin = old_stdin
    sys.stdout = old_stdout

    return "\n".join(ans) + ("\n" if ans else "")

# provided sample
assert run(
"""5 2
1 2 3 1 2
2 1 4
3 1 3
"""
) == "Yes\nNo\n", "sample 1"

# minimum size
assert run(
"""1 1
7
1 1 1
"""
) == "Yes\n"

# all equal
assert run(
"""5 2
4 4 4 4 4
3 1 3
2 2 4
"""
) == "Yes\nYes\n"

# boundary at end of array
assert run(
"""5 1
1 2 3 4 5
2 4 4
"""
) == "Yes\n"

# off-by-one check
assert run(
"""5 1
1 2 3 4 5
2 1 2
"""
) == "No\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mảng phần tử đơn | Có | Đầu vào pháp lý nhỏ nhất | 
| Tất cả các giá trị bằng nhau | Có, Có | Xử lý mẫu lặp lại | 
| Truy vấn kết thúc ở vị trí n | Có | Độ đúng ranh giới | 
| Các chuỗi con không bằng nhau liền kề | Không | Bảo vệ từng người một | 

## Vỏ cạnh 

Hãy xem xét trường hợp nhỏ nhất có thể:```
1 1
7
1 1 1
```Chuỗi con duy nhất là`[7]`. Hàm băm của phạm vi`[1,1]`được tính hai lần và so sánh với chính nó. Cả hai giá trị băm đều khớp nhau, vì vậy câu trả lời là`"Yes"`. 

Hãy xem xét một truy vấn chạm vào cuối mảng:```
5 1
1 2 3 4 5
2 4 4
```Các phạm vi được so sánh là`[4,5]`Và`[4,5]`. Hàm băm trích xuất chính xác hai phần tử vì độ dài được sử dụng trong số hạng lũy ​​thừa là`r - l + 1 = 2`. Không có phần tử bổ sung nào được đưa vào và không có phần tử nào bị bỏ qua. 

Hãy xem xét hai phạm vi có tiền tố chung dài:```
5 1
7 7 7 7 8
4 1 2
```Các chuỗi con được so sánh là:```
[7,7,7,7]
[7,7,7,8]
```Ba vị trí đầu tiên khớp nhau, nhưng vị trí cuối cùng thì khác. Băm đa thức của chúng khác nhau vì hệ số cuối cùng đóng góp một giá trị khác. Đầu ra của thuật toán`"No"`. 

Hãy xem xét các vị trí bắt đầu giống hệt nhau:```
5 1
1 2 3 4 5
5 1 1
```Cả hai khoảng thời gian được truy vấn đều có cùng phạm vi. Cặp băm được trích xuất giống hệt nhau và thuật toán trả về`"Yes"`ngay lập tức thông qua logic so sánh thông thường. Không cần xử lý đặc biệt.
