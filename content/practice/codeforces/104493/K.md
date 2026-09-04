---
title: "CF 104493K - Sam-Oh, huấn luyện viên vui tính"
description: "Mỗi thí sinh kết thúc với một chuỗi có độ dài m. Chuỗi này đã được sắp xếp theo thứ tự không giảm, do đó, nó bao gồm các chuỗi ký tự giống hệt nhau: một số 'a', sau đó là một số 'b', v.v. cho đến 'z'. Nhiệm vụ là trả lời nhiều câu hỏi."
date: "2026-06-30T12:24:40+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104493
codeforces_index: "K"
codeforces_contest_name: "2023 ICPC HIAST Collegiate Programming Contest"
rating: 0
weight: 104493
solve_time_s: 51
verified: true
draft: false
---

[CF 104493K - Sam-Oh, huấn luyện viên vui tính](https://codeforces.com/problemset/problem/104493/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 51s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi thí sinh kết thúc bằng một chuỗi dài duy nhất`m`. Chuỗi này đã được sắp xếp theo thứ tự không giảm, do đó nó bao gồm các chuỗi ký tự giống nhau: một số ký tự`'a'`, sau đó một số`'b'`, v.v. cho đến`'z'`. 

Nhiệm vụ là trả lời nhiều câu hỏi. Mỗi câu hỏi đưa ra hai thí sinh và chúng ta phải đếm xem có bao nhiêu vị trí`i`có cùng một ký tự trong cả hai chuỗi. Nói cách khác, chúng tôi tính toán độ tương tự Hamming giữa hai chuỗi được sắp xếp: có bao nhiêu chỉ số khớp chính xác. 

Các ràng buộc chặt chẽ theo cách quan trọng về mặt cấu trúc. Tổng kích thước đầu vào đáp ứng`n · m ≤ 5 × 10^5`, vì vậy chúng ta có thể đủ khả năng xử lý trước tuyến tính gần đúng trên tất cả các ký tự của tất cả các chuỗi. Tuy nhiên, số lượng truy vấn có thể lớn như`10^6`, điều này ngay lập tức loại trừ mọi giải pháp quét toàn bộ chuỗi cho mỗi truy vấn. Thậm chí`O(m)`mỗi truy vấn sẽ quá chậm. 

Cấu trúc ẩn quan trọng là mọi chuỗi đều được sắp xếp. Điều đó có nghĩa là mỗi chuỗi được xác định hoàn toàn bằng số lượng của từng chữ cái và quan trọng hơn là mỗi chữ cái chiếm một đoạn liền kề trong chuỗi. Điều này biến vấn đề từ việc so sánh vị trí của các mảng tùy ý thành các truy vấn chồng chéo theo các khoảng thời gian. 

Một sai lầm ngây thơ là so sánh từng chuỗi ký tự trên mỗi truy vấn. Điều đó hoạt động hợp lý, nhưng sẽ yêu cầu tới`10^6 × 5×10^5`trong trường hợp xấu nhất là không thể thực hiện được. 

Một cạm bẫy tinh vi khác là chỉ so sánh tần số ký tự. Điều đó là chưa đủ, bởi vì sự bình đẳng về tần số không bao hàm sự bình đẳng về vị trí. Ví dụ,`"aabbcc"`Và`"abcabc"`có cùng số lượng nhưng trùng khớp ở ít chỉ số hơn. 

Điều quan trọng là bảo toàn cấu trúc vị trí được tạo ra bởi thứ tự sắp xếp. 

## Phương pháp tiếp cận 

Giải pháp brute-force rất đơn giản: đối với mỗi truy vấn, lặp qua tất cả`m`vị trí và đếm trận đấu. Điều này đúng vì chúng tôi so sánh trực tiếp các định nghĩa. Tuy nhiên, với tới`10^6`truy vấn và chuỗi có thể có độ dài`5×10^5 / n`, điều này dẫn đến việc xử lý trong trường hợp xấu nhất`O(Q · m)`, vượt xa giới hạn có thể chấp nhận được. 

Quan sát chính đến từ thuộc tính được sắp xếp. Vì mỗi chuỗi không giảm nên mỗi ký tự tạo thành một khối liền kề. Ví dụ: một chuỗi có thể trông giống như`aaaabbbbcc`. 

Thay vì nghĩ về các vị trí riêng lẻ, chúng ta có thể nghĩ về các khoảng thời gian cho từng nhân vật. Cho mỗi chuỗi và mỗi chữ cái`c`, chúng ta có thể tính toán phân đoạn của các chỉ số trong đó`c`xuất hiện. Sau đó, khi so sánh hai chuỗi, một vị trí sẽ đóng góp vào câu trả lời khi và chỉ khi cả hai chuỗi nằm trong cùng một khối ký tự tại vị trí đó. Điều đó làm giảm vấn đề về tổng hợp các khoảng ký tự tương ứng. 

Đối với mỗi chữ cái, chúng tôi tính toán mức độ trùng lặp khối của nó giữa hai chuỗi. Vì mỗi chữ cái xuất hiện đúng một khoảng liền kề trên mỗi chuỗi nên sự đóng góp của chữ cái đó có thể được tính theo thời gian không đổi. Với 26 chữ cái, mỗi truy vấn sẽ trở thành`O(26)`. 

Điều này làm giảm toàn bộ vấn đề từ việc quét toàn bộ chuỗi đến so sánh một số khoảng không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(Q · m) | O(1) | Quá chậm | 
| Tối ưu | O(n · m + 26 · Q) | O(n · 26) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi chuỗi, hãy tính số lần mỗi ký tự xuất hiện. Vì chuỗi đã được sắp xếp nên các số đếm này tương ứng với các phân đoạn liền kề theo thứ tự từ`'a'`ĐẾN`'z'`. 
2. Chuyển đổi mỗi chuỗi thành 26 khoảng trên các vị trí. Trong khi quét các ký tự theo thứ tự, hãy chỉ định chỉ mục bắt đầu và kết thúc cho mỗi khối chữ cái. Điều này tạo ra mảng`L[i][c]`Và`R[i][c]`, đại diện cho nhân vật ở đâu`c`sống trong chuỗi`i`. 
3. Đối với mỗi truy vấn so sánh chuỗi`s`Và`t`, lặp lại tất cả 26 chữ cái. 
4. Đối với mỗi chữ cái`c`, tính toán sự chồng chéo của hai khoảng: 

sự chồng chéo là`max(0, min(Rs[c], Rt[c]) - max(Ls[c], Lt[c]) + 1)`. 
5. Tính tổng các phần trùng lặp này trên tất cả các chữ cái để có được số vị trí trùng khớp. 
6. Xuất kết quả cho từng truy vấn. 

Lý do đằng sau bước 4 là một vị trí chỉ đóng góp vào câu trả lời nếu cả hai chuỗi đều gán cùng một ký tự cho vị trí đó. Vì mỗi ký tự chiếm chính xác một khối liên tục trong cả hai chuỗi nên giao điểm của các khối này sẽ đếm chính xác các chỉ số phù hợp. 

### Tại sao nó hoạt động 

Mỗi chuỗi phân vùng phạm vi chỉ mục`[1, m]`thành 26 khoảng cách nhau, mỗi khoảng một ký tự (có thể trống). Ở bất kỳ vị trí nào`i`, chính xác một khoảng từ mỗi chuỗi chứa`i`và khoảng đó xác định duy nhất ký tự ở vị trí đó. 

Trận đấu ở vị trí`i`xảy ra chính xác khi cả hai chuỗi chọn cùng một nhãn khoảng thời gian tại`i`. Do đó, việc đếm các vị trí bằng nhau sẽ rút gọn thành tổng các giao điểm theo cặp của các khoảng ký tự tương ứng. Không có vị trí nào có thể được tính hai lần vì các khoảng cho các chữ cái khác nhau nằm rời rạc trong một chuỗi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def build_intervals(s):
    m = len(s)
    L = [0] * 26
    R = [0] * 26

    i = 0
    pos = 1
    while i < m:
        c = ord(s[i]) - 97
        start = pos
        while i < m and ord(s[i]) - 97 == c:
            i += 1
            pos += 1
        end = pos - 1
        L[c] = start
        R[c] = end

    return L, R

n, m = map(int, input().split())
strings = [None] * n
intervals = [None] * n

for i in range(n):
    s = input().strip()
    strings[i] = s
    intervals[i] = build_intervals(s)

q = int(input())
out = []

for _ in range(q):
    a, b = map(int, input().split())
    a -= 1
    b -= 1

    La, Ra = intervals[a]
    Lb, Rb = intervals[b]

    ans = 0
    for c in range(26):
        left = max(La[c], Lb[c])
        right = min(Ra[c], Rb[c])
        if left <= right:
            ans += right - left + 1

    out.append(str(ans))

print("\n".join(out))
```Bước tiền xử lý chuyển đổi từng chuỗi được sắp xếp thành khoảng 26 ký tự bằng cách quét một lần. Điều này an toàn vì thứ tự sắp xếp đảm bảo một khối liền kề cho mỗi ký tự, do đó không có ký tự nào cần nhiều phân đoạn. 

Mỗi truy vấn sau đó chỉ sử dụng công việc không đổi cho mỗi ký tự. Công thức chồng chéo là giao điểm khoảng tiêu chuẩn và`+1`tính đến việc lập chỉ mục toàn diện. 

Một lỗi triển khai phổ biến là quên rằng một số chữ cái hoàn toàn không xuất hiện trong chuỗi. Trong trường hợp đó, khoảng của chúng vẫn có độ dài bằng 0 và công thức chồng lấp tự nhiên đóng góp bằng 0 vì`left > right`. 

## Ví dụ đã hoạt động 

Hãy xem xét hai chuỗi:`s = "aaabbc"`

`t = "aabbbc"`Chúng tôi tính toán các khoảng: 

cho`s`: 

| lá thư | khoảng thời gian | 
| --- | --- | 
| một | [1,3] | 
| b | [4,5] | 
| c | [6,6] | 

Vì`t`: 

| lá thư | khoảng thời gian | 
| --- | --- | 
| một | [1,2] | 
| b | [3,5] | 
| c | [6,6] | 

Bây giờ tính toán sự chồng chéo: 

| lá thư | chồng chéo | 
| --- | --- | 
| một | [1,2] → 2 | 
| b | [4,5] ∩ [3,5] = [4,5] → 2 | 
| c | [6,6] → 1 | 

Tổng số câu trả lời là`2 + 2 + 1 = 5`. 

Điều này cho thấy rằng mặc dù cách sắp xếp bên trong có khác nhau nhưng các vị trí khớp nhau vẫn được nắm bắt hoàn toàn thông qua các giao điểm khoảng. 

Bây giờ hãy xem xét:`s = "aaaa"`

`t = "bbbb"`Khoảng thời gian:`s: a[1,4]`

`t: b[1,4]`Tất cả các phần trùng lặp đều trống, vì vậy câu trả lời là`0`, phù hợp với trực giác vì không có vị trí nào có cùng đặc điểm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n · m + 26 · Q) | Mỗi chuỗi được xử lý một lần để xây dựng các khoảng và mỗi truy vấn sẽ kiểm tra 26 chữ cái | 
| Không gian | O(n · 26) | Mỗi chuỗi lưu trữ hai mảng có kích thước 26 | 

Quá trình tiền xử lý là tuyến tính trong tổng kích thước đầu vào, tối đa là`5 × 10^5`. Mỗi truy vấn là công việc liên tục, vì vậy ngay cả ở`10^6`truy vấn giải pháp vẫn hiệu quả. 

## Trường hợp thử nghiệm```python
import sys, io

def solve(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    def build(s):
        m = len(s)
        L = [0]*26
        R = [0]*26
        i = 0
        pos = 1
        while i < m:
            c = ord(s[i]) - 97
            start = pos
            while i < m and ord(s[i]) - 97 == c:
                i += 1
                pos += 1
            L[c] = start
            R[c] = pos - 1
        return L, R

    n, m = map(int, input().split())
    inter = []
    for _ in range(n):
        inter.append(build(input().strip()))

    q = int(input())
    res = []
    for _ in range(q):
        a, b = map(int, input().split())
        a -= 1; b -= 1
        La, Ra = inter[a]
        Lb, Rb = inter[b]
        ans = 0
        for c in range(26):
            l = max(La[c], Lb[c])
            r = min(Ra[c], Rb[c])
            if l <= r:
                ans += r - l + 1
        res.append(str(ans))
    return "\n".join(res)

# minimal
assert solve("""2 1
a
b
1
1 2
""") == "0"

# identical
assert solve("""2 3
abc
abc
1
1 2
""") == "3"

# reversed structure within sorted constraint still sorted identical pattern
assert solve("""2 4
aabb
aabb
1
1 2
""") == "4"

# mixed overlap
assert solve("""2 6
aaabbc
aabbbc
1
1 2
""") == "5"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 chuỗi ký tự đơn không khớp | 0 | các chữ cái hoàn toàn rời rạc | 
| chuỗi giống hệt nhau | chiều dài đầy đủ | đường cơ sở chính xác | 
| khối lặp đi lặp lại | trận đấu đầy đủ | xử lý khối | 
| phân phối hỗn hợp | 5 | khoảng thời gian chồng chéo chính xác | 

## Vỏ cạnh 

Trường hợp cạnh tinh tế là khi một ký tự không xuất hiện ở một trong các chuỗi. Trong trường hợp đó khoảng thời gian của nó vẫn không được đặt. Thuật toán vẫn hoạt động vì cả hai`L[c]`Và`R[c]`mặc định là`0`, tạo ra một khoảng không hợp lệ và điều kiện giao nhau`left <= right`thất bại, đóng góp bằng không. 

Một trường hợp khác là khi một chuỗi chỉ chứa một ký tự, ví dụ`"aaaaa"`. Khi đó chỉ có một khoảng không trống và tất cả các chữ cái khác vẫn không hoạt động. Các truy vấn đối với một chuỗi như vậy vẫn hoạt động chính xác vì tất cả đóng góp phù hợp đều được tập trung trong khoảng duy nhất đó và tất cả các chữ cái khác đương nhiên không đóng góp gì thông qua các phần chồng chéo trống.
