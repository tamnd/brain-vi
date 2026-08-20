---
title: "CF 102261A - \u0411\u0443\u0434\u0438\u043b\u044c\u043d\u0438\u043a\u0438"
description: "Mỗi báo thức (N) bắt đầu đổ chuông vào thời điểm ban đầu (ti), sau đó lặp lại sau mỗi (X) phút. Nếu nhiều chuông báo thức đổ chuông đồng thời, Alexey chỉ nghe thấy khoảnh khắc đó một lần. Anh ấy tỉnh dậy ngay sau khi nghe thấy tiếng chuông thứ (K) vang lên."
date: "2026-08-19T02:09:53+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102261
codeforces_index: "A"
codeforces_contest_name: "\u0427\u0435\u043c\u043f\u0438\u043e\u043d\u0430\u0442 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e - \u043a\u0432\u0430\u043b\u0438\u0444\u0438\u043a\u0430\u0446\u0438\u044f (\u042f\u043d\u0434\u0435\u043a\u0441)"
rating: 0
weight: 102261
solve_time_s: 337
verified: true
draft: false
---

[CF 102261A - \u0411\u0443\u0434\u0438\u043b\u044c\u043d\u0438\u043a\u0438](https://codeforces.com/problemset/problem/102261/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 5 phút 37 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi báo thức (N) bắt đầu đổ chuông vào thời điểm ban đầu (t_i), sau đó lặp lại sau mỗi (X) phút. Nếu nhiều chuông báo thức đổ chuông đồng thời, Alexey chỉ nghe thấy khoảnh khắc đó một lần. Anh ấy tỉnh dậy ngay sau khi nghe thấy tiếng chuông thứ (K) vang lên. 

Nhiệm vụ là tìm thời điểm phân biệt thứ (K) đó. 

Các ràng buộc đủ lớn để loại trừ việc mô phỏng mọi sự kiện đổ chuông. Có thể có (10^5) cảnh báo, trong khi cả (X) và (K) đều có thể đạt tới (10^9). Bản thân câu trả lời có thể là khoảng (10^{18}), vì một báo thức có thể cần đổ chuông (K) lần. Một giải pháp tạo ra các sự kiện (K) đầu tiên một cách rõ ràng có thể yêu cầu (O(KN)) hoạt động trong trường hợp xấu nhất, điều này hoàn toàn không khả thi. Chúng ta cần sự phụ thuộc logarit vào kích thước của câu trả lời, cùng với quá trình tiền xử lý tuyến tính gần đúng. 

Các trường hợp nguy hiểm chính đến từ các báo động trùng khớp. Coi như```
2 5 3
1 6
```Cả hai báo thức đều đổ chuông vào cùng thời điểm: (1,6,11,\ldots). Thời gian đổ chuông riêng biệt thứ ba là (11). Một giải pháp bất cẩn coi mỗi tiếng chuông báo thức là một sự kiện riêng biệt sẽ đếm (1) và (6) hai lần và đưa ra câu trả lời sai. 

Một trường hợp tinh vi khác là khi hai thời gian ban đầu có cùng phần dư modulo (X), nhưng không bằng nhau:```
2 5 4
1 11
```Chuông báo thức đầu tiên đổ chuông lúc (1,6,11,16,\ldots), trong khi chuông báo thứ hai đổ chuông lúc (11,16,\ldots). Báo thức thứ hai không tạo ra thời gian đổ chuông mới nào cả, vì vậy thời gian riêng biệt thứ tư là (16). Chỉ cần giữ mọi (t_i) như một cấp số cộng độc lập sẽ được tính gấp đôi (11,16,\ldots). 

Tình huống ngược lại cũng có vấn đề. Các dư lượng khác nhau modulo (X) không bao giờ va chạm. Ví dụ,```
2 5 4
1 2
```tạo ra (1,2,6,7,\ldots). Hai cấp số phân biệt mãi mãi, vì các số có số dư khác nhau theo modulo (5) không thể bằng nhau. 

## Phương pháp tiếp cận 

Một giải pháp trực tiếp có thể liên tục tìm ra thời gian đổ chuông tiếp theo sớm nhất trong số tất cả các báo thức, đếm một lần rồi nâng cao mọi báo thức đổ chuông vào thời điểm đó. Về mặt khái niệm, điều này đơn giản và chính xác vì nó mô phỏng theo đúng nghĩa đen chuỗi khoảnh khắc đổ chuông đã hợp nhất. Tuy nhiên, ngay cả một cảnh báo cũng có thể tạo ra sự kiện (K) và (K) có thể là (10^9). Với (N=10^5), việc duy trì tất cả các cảnh báo có thể yêu cầu hoạt động theo thứ tự (NK), tối đa (10^{14}). Ngay cả một mô phỏng cẩn thận hơn sử dụng hàng đợi ưu tiên vẫn phải xử lý (K) các khoảnh khắc riêng biệt, cho ra ít nhất (O(K\log N)), con số này quá lớn. 

Cấu trúc hữu ích là mọi tiến trình đều có cùng một bước (X). Hai báo thức có thể đổ chuông cùng lúc một cách chính xác khi thời gian bắt đầu của chúng có cùng mô-đun dư (X). Giả sử hai điểm bắt đầu trong cùng một lớp dư lượng là (a) và (b), với (a<b). Vì (b-a) là bội số của (X) nên mỗi lần đổ chuông của chuông báo thứ hai cũng là thời gian đổ chuông của chuông báo thứ nhất. Do đó, chỉ có cảnh báo bắt đầu sớm nhất trong mỗi loại dư lượng mới quan trọng. 

Sau khi thay thế tất cả các cảnh báo bằng (t_i) sớm nhất cho mỗi modulo còn lại (X), chúng ta có nhiều nhất (X) tiến trình liên quan và mỗi tiến trình là 

[ 
s,\ s+X,\ s+2X,\ldots 
] 

trong đó (s) là thời điểm bắt đầu sớm nhất của nó. 

Bây giờ hãy xem xét thời gian ứng cử viên (T). Đối với một tiến trình bắt đầu từ (s), số khoảnh khắc đổ chuông không vượt quá (T) là 

[ 
\left\lfloor\frac{T-s}{X}\right\rfloor+1 
] 

khi (s\le T) và bằng 0 nếu không. Vì các cấp số giữ lại khác nhau có dư lượng modulo (X) khác nhau nên chúng không bao giờ trùng nhau. Do đó, chúng ta có thể tính tổng các số đếm này để có được số khoảnh khắc đổ chuông riêng biệt chính xác lên tới (T). 

Số đếm này đơn điệu: tăng (T) chỉ có thể thêm khoảnh khắc đổ chuông. Điều đó làm cho tìm kiếm nhị phân có thể áp dụng được. Chúng tôi tìm kiếm nhị phân (T) nhỏ nhất mà ít nhất (K) khoảnh khắc đổ chuông riêng biệt đã xảy ra. (T) nhỏ nhất chính là khoảnh khắc đổ chuông thứ (K). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(K\log N)) với hàng đợi ưu tiên | (O(N)) | Quá chậm | 
| Tối ưu | (O(N\log A)) | (O(N)) | Đã chấp nhận | 

Ở đây (A) là độ lớn của câu trả lời, nhiều nhất là khoảng (10^{18}), do đó việc tìm kiếm nhị phân chỉ mất khoảng 60 lần lặp. 

## Hướng dẫn thuật toán 

1. Đọc (N), (X), (K) và thời gian bắt đầu (t_i). 
2. Nhóm thời gian bắt đầu theo modulo còn lại (X). Đối với mỗi phần còn lại, chỉ giữ lại thời gian bắt đầu nhỏ nhất. 

Nếu hai lần bắt đầu có cùng số dư thì tiến trình sau sẽ được chứa hoàn toàn bên trong tiến trình trước đó. Việc giữ ở mức tối thiểu sẽ loại bỏ tất cả các khoảnh khắc đổ chuông trùng lặp trong tương lai trước khi quá trình tìm kiếm nhị phân bắt đầu. 
3. Xác định hàm đếm (count(T)). Với mỗi (các) thời gian bắt đầu được giữ lại, nếu (s\le T), hãy thêm 

[ 
\frac{T-s}{X}+1 
] 

dùng phép chia số nguyên. 

Giá trị thu được chính xác là số khoảnh khắc đổ chuông riêng biệt tại hoặc trước (T), bởi vì các tiến trình được giữ lại có các dư lượng khác nhau và do đó không thể giao nhau. 
4. Chọn khoảng tìm kiếm nhị phân chứa câu trả lời. Thời gian đổ chuông sớm nhất có thể là (\min(t_i)). Giới hạn trên an toàn là 

[ 
\min(t_i)+(K-1)X. 
] 

Ngay cả một tiến trình đơn lẻ bắt đầu tại thời điểm tối thiểu cũng chứa các sự kiện (K) tại thời điểm đó, vì vậy câu trả lời thực tế không thể lớn hơn. 
5. Tìm kiếm nhị phân để tìm (T) nhỏ nhất thỏa mãn (count(T)\ge K). Nếu số đếm ở điểm giữa đã ít nhất là (K), thì câu trả lời nhiều nhất là điểm giữa đó, vì vậy hãy di chuyển ranh giới bên phải sang trái. Ngược lại, đáp án sẽ lớn hơn, do đó hãy di chuyển ranh giới bên trái sang phải. 
6. Xuất ra ranh giới bên trái cuối cùng. Đây là lần đầu tiên có ít nhất (K) sự kiện riêng biệt xuất hiện, nghĩa là đây chính xác là thời điểm đổ chuông phân biệt thứ (K). 

### Tại sao nó hoạt động

Sau khi chỉ giữ lại cảnh báo sớm nhất cho mỗi modulo dư lượng (X), mọi cảnh báo bị loại bỏ chỉ tạo ra các khoảnh khắc đã được tạo ra bởi cảnh báo được giữ lại từ cùng một dư lượng. Do đó, tiến trình được giữ lại sẽ tạo ra tập hợp thời gian đổ chuông giống hệt như các báo thức ban đầu. 

Các cấp số được giữ lại có các dư lượng modulo (X) khác nhau theo từng cặp, do đó không có hai cấp số nào trong số chúng có thể đổ chuông đồng thời. Do đó, tổng hợp số lượng sự kiện từ mỗi tiến trình sẽ cho ra số lượng chính xác các sự kiện riêng biệt tại bất kỳ thời điểm nào (T). Số đếm này là đơn điệu, do đó (T) nhỏ nhất với (count(T)\ge K) chính xác là thời gian của sự kiện phân biệt thứ (K). 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    N, X, K = map(int, input().split())
    t = list(map(int, input().split()))

    INF = 10**30
    earliest = {}

    for value in t:
        r = value % X
        if r not in earliest or value < earliest[r]:
            earliest[r] = value

    starts = list(earliest.values())
    first = min(starts)

    def count_events(T):
        total = 0

        for s in starts:
            if s <= T:
                total += (T - s) // X + 1
                if total >= K:
                    return total

        return total

    lo = first
    hi = first + (K - 1) * X

    while lo < hi:
        mid = (lo + hi) // 2

        if count_events(mid) >= K:
            hi = mid
        else:
            lo = mid + 1

    print(lo)

if __name__ == "__main__":
    solve()
```Từ điển`earliest`thực hiện nén lớp dư lượng. Chìa khóa là`value % X`và giá trị của nó là thời điểm bắt đầu nhỏ nhất có số dư đó. Chúng tôi không cần phải lưu giữ danh tính cảnh báo thực tế vì chỉ khoảnh khắc đổ chuông của chúng mới quan trọng. 

chức năng`count_events`đánh giá vị từ đơn điệu được sử dụng bởi tìm kiếm nhị phân. điều kiện`s <= T`là cần thiết vì cảnh báo chưa tạo ra sự kiện nào khi thời gian đổ chuông đầu tiên của nó là sau (T). Để có sự tiến triển tích cực,`(T - s) // X + 1`đếm sự kiện tại chính (các) sự kiện, đây là nguồn gốc của một lỗi phổ biến. 

Sự trở về sớm khi`total >= K`là một sự tối ưu hóa. Khi số đếm đã đạt tới (K), giá trị chính xác lớn hơn của nó không liên quan đến vị từ tìm kiếm nhị phân. 

Số nguyên Python có độ chính xác tùy ý, do đó giá trị tiềm năng lớn`first + (K - 1) * X`không tràn. Trong ngôn ngữ có loại số nguyên có chiều rộng cố định, ở đây bắt buộc phải có số nguyên 64 bit. 

Giới hạn trên có hiệu lực ngay cả khi có nhiều lớp dư lượng, bởi vì một cấp số bắt đầu tại`first`riêng chứa (K) khoảnh khắc rung chuông bởi`first + (K - 1) * X`. Nhiều cảnh báo hơn chỉ có thể thực hiện sự kiện thứ (K) sớm hơn. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên,```
6 5 10
1 2 3 4 5 6
```phần dư modulo (5) là (1,2,3,4,0,1). Hai giá trị (1) và (6) có cùng một số dư và lũy tiến từ (6) đã được chứa trong lũy ​​tiến từ (1). Do đó, số lần bắt đầu được giữ lại là (1,2,3,4,5). 

| Trạng thái tìm kiếm nhị phân | Giá trị | 
| --- | --- | 
| Bắt đầu được giữ lại | 1, 2, 3, 4, 5 | 
| (K) | 10 | 
| Lần đầu tiên có thể | 1 | 
| Giới hạn trên | 46 | 
| (đếm(23)) | 24 | 
| (đếm(12)) | 12 | 
| (đếm(6)) | 6 | 
| (đếm(9)) | 9 | 
| (đếm(10)) | 10 | 

Mười sự kiện riêng biệt chính xác là (1,2,3,4,5,6,7,8,9,10), vì vậy câu trả lời là (10). Sự bắt đầu trùng lặp ở (6) không bao giờ tạo ra sự kiện bổ sung. 

Đối với mẫu thứ hai,```
5 7 12
5 22 17 13 8
```phần dư modulo (7) là (5,1,3,6,1). Các điểm bắt đầu (22) và (8) chia sẻ dư lượng (1), do đó cảnh báo bắt đầu từ (22) là dư thừa. Số lần bắt đầu được giữ lại là (5,8,17,13). 

| Ứng viên (T) | Sự kiện lên tới (T) | 
| --- | --- | 
| 20 | 10 | 
| 25 | 11 | 
| 27 | 12 | 
| 26 | 11 | 

Cấp số bắt đầu từ (5) đóng góp (5,12,19,26,\ldots), cấp số bắt đầu từ (8) đóng góp (8,15,22,\ldots), cấp độ bắt đầu từ (13) đóng góp (13,20,27,\ldots) và cấp độ bắt đầu tại (17) đóng góp (17,24,31,\ldots). Sự kiện riêng biệt thứ mười hai là (27), phù hợp với mẫu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(N\log A)) | Chi phí của các lớp dư lượng xây dựng (O(N)) và mỗi lần lặp trong khoảng (O(\log A)) lần quét tìm kiếm nhị phân sẽ quét tối đa (N) lần bắt đầu được giữ lại. | 
| Không gian | (O(N)) | Từ điển lưu trữ tối đa một thời điểm bắt đầu cho mỗi phần dư riêng biệt. | 

Với (N\le10^5), thuật toán thực hiện khoảng (10^5) thao tác trong quá trình tiền xử lý và nhiều nhất là khoảng 60 lần quét để có câu trả lời có kích thước 64 bit. Điều này là thực tế trong các giới hạn nhất định, trong khi việc mô phỏng rõ ràng tối đa (K) sự kiện là không thể khi (K) tiến tới (10^9). 

## Trường hợp thử nghiệm```python
import sys
import io

def solve_data(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    N, X, K = map(int, sys.stdin.readline().split())
    t = list(map(int, sys.stdin.readline().split()))

    earliest = {}

    for value in t:
        r = value % X
        if r not in earliest or value < earliest[r]:
            earliest[r] = value

    starts = list(earliest.values())
    first = min(starts)

    def count_events(T):
        total = 0
        for s in starts:
            if s <= T:
                total += (T - s) // X + 1
                if total >= K:
                    return total
        return total

    lo = first
    hi = first + (K - 1) * X

    while lo < hi:
        mid = (lo + hi) // 2
        if count_events(mid) >= K:
            hi = mid
        else:
            lo = mid + 1

    print(lo)

    result = sys.stdout.getvalue()

    sys.stdin = old_stdin
    sys.stdout = old_stdout

    return result.strip()

# Sample 1
assert solve_data("""\
6 5 10
1 2 3 4 5 6
""") == "10"

# Sample 2
assert solve_data("""\
5 7 12
5 22 17 13 8
""") == "27"

# Minimum-size input
assert solve_data("""\
1 1 1
1
""") == "1"

# All alarms are identical, so there is only one distinct progression
assert solve_data("""\
5 10 7
3 3 3 3 3
""") == "63"

# Same residue, later alarm is completely redundant
assert solve_data("""\
2 5 4
1 11
""") == "16"

# Different residues and an exact boundary at an initial ringing time
assert solve_data("""\
2 5 4
1 2
""") == "7"

# Large answer, checks 64-bit-sized arithmetic
assert solve_data("""\
100000 1000000000 1000000000
1 1 1 1 1 1 1 1 1 1
""") == "999999999000000001"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 1 / 1`|`1`| Đầu vào có kích thước tối thiểu và sự kiện đầu tiên có thể xảy ra | 
|`5 10 7 / 3 3 3 3 3`|`63`| Tất cả các báo động hoàn toàn trùng khớp | 
|`2 5 4 / 1 11`|`16`| Các lớp dư lượng trùng lặp phải được nén | 
|`2 5 4 / 1 2`|`7`| Dư lượng khác nhau và ranh giới sự kiện chính xác | 
|`100000 1000000000 1000000000 / all 1`|`999999999000000001`| Số học lớn và lớn (K) | 

## Vỏ cạnh 

Khi mọi báo thức đều có thời gian bắt đầu giống nhau, chẳng hạn như```
5 10 7
3 3 3 3 3
```chỉ có một cấp số phân biệt duy nhất, (3,13,23,33,43,53,63,\ldots). Từ điển chỉ có một mục, vì vậy`count_events(63)`là (7) và đáp án là (63). Một mô phỏng đếm các cảnh báo thay vì thời gian riêng biệt sẽ cho rằng có năm sự kiện xảy ra cùng một lúc (3). 

Khi hai điểm bắt đầu khác nhau bởi bội số của (X), chẳng hạn như```
2 5 4
1 11
```cả hai khởi đầu đều có dư lượng (1). Cấp số tiến từ (11) là (11,16,21,\ldots), đã có trong (1,6,11,16,\ldots). Từ điển chỉ giữ lại (1), đưa ra sự kiện thứ tư (16). Đây là lý do tại sao việc nhóm theo số dư, thay vì chỉ loại bỏ thời gian bắt đầu bằng nhau, là cần thiết. 

Khi hai cảnh báo có dư lượng khác nhau, các sự kiện của chúng sẽ không bao giờ xung đột. Vì```
2 5 4
1 2
```chuỗi hợp nhất bắt đầu (1,2,6,7), vì vậy câu trả lời là (7). Hàm đếm cho (count(6)=3) và (count(7)=4), do đó tìm kiếm nhị phân trả về (7). Điều này cũng kiểm tra ranh giới giữa một ứng cử viên không đủ và ứng cử viên đủ đầu tiên. 

Trường hợp tối thiểu```
1 1 1
1
```chỉ có một tiến trình và yêu cầu sự kiện đầu tiên của nó. Giới hạn tìm kiếm nhị phân dưới và trên đều là (1), do đó vòng lặp không thực hiện lặp lại và trả về ngay lập tức (1). Điều này xác nhận rằng khoảng thời gian tìm kiếm bao gồm chính câu trả lời thay vì bắt đầu ngay sau nó.
