---
title: "CF 104393J - Salad Tiệc Của Jane"
description: "Chúng tôi được cung cấp một kho nguyên liệu cố định và một quy tắc là Jane phải chọn chính xác $K$ những nguyên liệu riêng biệt để tạo thành món salad. Mỗi người bạn có một danh sách riêng các thành phần mà họ từ chối ăn."
date: "2026-07-01T00:37:48+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104393
codeforces_index: "J"
codeforces_contest_name: "ICPC Masters Mexico LATAM 2023"
rating: 0
weight: 104393
solve_time_s: 92
verified: true
draft: false
---

[CF 104393J - Salad dự tiệc của Jane](https://codeforces.com/problemset/problem/104393/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 32s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một tủ đựng nguyên liệu cố định và một quy tắc mà Jane phải chọn chính xác.$K$thành phần riêng biệt để tạo thành món salad. Mỗi người bạn có một danh sách riêng các thành phần mà họ từ chối ăn. Một người bạn chỉ tham dự bữa tiệc nếu không ai trong số những người được chọn$K$các thành phần xuất hiện trong danh sách không thích của họ. Mục tiêu là lựa chọn các$K$thành phần sao cho số lượng bạn bè tham dự càng đông càng tốt. 

Diễn đạt lại điều này theo cách có cấu trúc hơn, mỗi thành phần là một phần tử trong một vũ trụ có kích thước$N \le 50$. Mỗi người bạn xác định một tập hợp con bị cấm của vũ trụ này. Món salad được chọn là một tập hợp con$S$kích thước$K$. Một người bạn hài lòng chính xác khi$S$không giao nhau với tập cấm của chúng. Chúng tôi muốn chọn$S$để tối đa hóa số lượng các ràng buộc này được thỏa mãn đồng thời. 

Các kích thước ràng buộc nhỏ nhưng được sắp xếp theo cách không đối xứng:$N \le 50$,$F \le 20$, Và$K \le N$. Số lượng bạn bè ít là tín hiệu cấu trúc quan trọng. Mặc dù không gian thành phần đủ lớn để liệt kê trực tiếp tất cả$K$-tập hợp con là không thể, số lượng ràng buộc đủ nhỏ để chúng ta có thể suy luận về các tập hợp con của các ràng buộc thay vì tập hợp con của các thành phần. 

Một nỗ lực ngây thơ sẽ thử tất cả$\binom{50}{K}$bộ thành phần và kiểm tra từng bộ với tất cả bạn bè. Ngay cả đối với$K \approx 25$, điều này sẽ dẫn đến một không gian tìm kiếm không thể thực hiện được. 

Một trường hợp thất bại tinh tế hơn xuất hiện khi cố gắng lựa chọn nguyên liệu một cách tham lam. Việc chọn các thành phần riêng lẻ có vẻ an toàn cho nhiều bạn bè không có tác dụng vì xung đột chỉ xuất hiện khi nhiều thành phần kết hợp với nhau. Một ví dụ nhỏ cho thấy điều này: 

đầu vào:```
3 2 2
1 1
1 2
```Nếu chúng ta tham lam chọn thành phần 1 (an toàn cho người bạn thứ hai) và sau đó chọn thành phần 2 (an toàn cho người bạn thứ nhất), thì cả hai người bạn đều thua vì mỗi người không thích một trong những thành phần đã chọn. Câu trả lời đúng là 0, đạt được bằng cách chọn hai thành phần bất kỳ, nhưng không có quy tắc tham lam nào có thể dự đoán chính xác sự tương tác đó. 

Vấn đề về cơ bản là về tính nhất quán toàn cầu đối với một tập hợp con các ràng buộc có kích thước cố định, chứ không phải việc chấm điểm cục bộ của từng thành phần riêng lẻ. 

## Phương pháp tiếp cận 

Quan điểm brute-force là liệt kê tất cả các tập hợp con của các thành phần có kích thước$K$, sau đó đếm xem có bao nhiêu người bạn không có giao điểm với tập hợp con đó. Điều này đúng vì nó trực tiếp tuân theo định nghĩa về tính hợp lệ, nhưng nó đòi hỏi phải kiểm tra tới$\binom{50}{K}$các tập hợp con, mỗi tập hợp con được kiểm tra với tối đa 20 người bạn bằng các bài kiểm tra giao nhau đã định sẵn. Ngay cả giới hạn trên cũng vượt xa giới hạn thời gian. 

Quan sát quan trọng là lật ngược quan điểm. Thay vì chọn nguyên liệu trước, chúng ta cân nhắc chọn những người bạn mà mình muốn làm hài lòng. Giả sử chúng ta sửa một nhóm bạn$T$. Cho tất cả họ tham dự, món salad đã chọn$S$phải tránh mọi thành phần xuất hiện ở bất kỳ người bạn nào trong$T$danh sách không thích của. Điều đó có nghĩa$S$phải được chứa hoàn toàn trong giao điểm của phần bù của các tập hợp bị cấm của chúng. 

Vì vậy để cố định$T$, điều kiện trở thành: tính tập hợp nguyên liệu được phép cho tất cả bạn bè trong$T$và kiểm tra xem tập hợp được phép này có chứa ít nhất$K$các phần tử. Nếu có, chúng ta luôn có thể chọn bất kỳ$K$của họ và làm hài lòng tất cả bạn bè trong$T$. 

Điều này chuyển đổi vấn đề thành việc lặp lại các tập hợp con của bạn bè thay vì tập hợp con của các thành phần. Từ$F \le 20$, chỉ có$2^F \le 10^6$tập hợp con, điều này là khả thi. Đối với mỗi tập hợp con, chúng tôi tính toán giao điểm trên tối đa 20 bitmask có kích thước 50. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force về thành phần |$O(\binom{N}{K} \cdot F \cdot N)$|$O(N)$| Quá chậm | 
| Tập hợp con DP trên bạn bè |$O(F \cdot 2^F + 2^F \cdot N/word)$|$O(2^F)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

### 1. Mã hóa bộ thành phần dưới dạng bitmask 

Danh sách không thích của mỗi người bạn được chuyển đổi thành mặt nạ 50 bit. Từ đó chúng tôi tính toán mặt nạ bổ sung đại diện cho các thành phần mà chúng cho phép. 

Lý do cho cách biểu diễn này là vì giao điểm đã thiết lập trở thành phép toán AND theo bit nhanh. 

### 2. Diễn giải từng nhóm nhỏ bạn bè như một nhóm ràng buộc 

Đối với bất kỳ nhóm bạn bè nào$T$, chúng tôi muốn tất cả họ đồng thời chấp nhận món salad. Điều này có nghĩa là món salad phải nằm hoàn toàn bên trong phần giao nhau của bộ nguyên liệu được phép. 

Vì vậy đối với mỗi$T$, chúng tôi xác định:$$A_T = \bigcap_{i \in T} A_i$$Ở đâu$A_i$là mặt nạ thành phần được phép cho bạn bè$i$. 

### 3. Tính toán giao điểm cho từng tập hợp con bạn bè bằng DP 

Chúng tôi tính toán$A_T$cho tất cả các tập con sử dụng tập con DP. Nếu như$T$không trống và chứa người bạn được thêm cuối cùng$i$, sau đó:$$A_T = A_{T \setminus \{i\}} \ \& \ A_i$$Phép lặp này cho phép chúng ta xây dựng tất cả các nút giao trong$O(F \cdot 2^F)$. 

### 4. Đánh giá tính khả thi của từng subset bạn bè 

Đối với mỗi tập hợp con$T$, chúng tôi tính toán số lượng thành phần được phép trong$A_T$. Nếu số lượng này ít nhất là$K$, thì có thể chọn$K$thành phần làm hài lòng tất cả bạn bè trong$T$. 

Chúng tôi cập nhật câu trả lời với kích thước tối đa của tập hợp con như vậy$T$. 

### Tại sao nó hoạt động 

Mỗi món salad hợp lệ tương ứng với một số tập hợp con bạn bè mà nó thỏa mãn, cụ thể là tất cả những người bạn có tập hợp không thích không giao nhau với món salad. Đối với tập hợp con đó$T$, món salad phải được chứa trong$A_T$. Ngược lại, nếu$A_T$có ít nhất$K$yếu tố, chúng ta luôn có thể xây dựng một món salad hợp lệ làm hài lòng chính xác những người bạn đó. Điều này thiết lập sự tương ứng một-một giữa các tập hợp con khả thi và các tập hợp con bạn bè khả thi, đồng thời việc tối đa hóa số lượng bạn bè hài lòng sẽ giảm xuống mức tối đa hóa kích thước tập hợp con dưới hạn chế về năng lực.$|A_T| \ge K$. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

N, K, F = map(int, input().split())

dislike = []
for _ in range(F):
    arr = list(map(int, input().split()))
    f = arr[0]
    mask = 0
    for x in arr[1:]:
        mask |= 1 << (x - 1)
    dislike.append(mask)

ALL = (1 << N) - 1
allow = [(~d) & ALL for d in dislike]

size = 1 << F
inter = [0] * size

for mask in range(size):
    if mask == 0:
        inter[mask] = ALL
    else:
        lsb = mask & -mask
        i = (lsb.bit_length() - 1)
        prev = mask ^ lsb
        inter[mask] = inter[prev] & allow[i]

ans = 0

for mask in range(size):
    cnt = inter[mask].bit_count()
    if cnt >= K:
        ans = max(ans, mask.bit_count())

print(ans)
```Giải pháp bắt đầu bằng cách chuyển đổi danh sách không thích của mỗi người bạn thành một mặt nạ bit, sau đó chuyển nó thành một mặt nạ được phép đặt. Điều này làm cho việc kiểm tra tính tương thích hoàn toàn theo bit. 

Mảng DP`inter[mask]`lưu trữ giao điểm của các bộ thành phần được phép cho tất cả bạn bè có trong`mask`. Quá trình chuyển đổi sẽ loại bỏ bit ít quan trọng nhất để sử dụng lại giao lộ đã được tính toán trước đó và tinh chỉnh nó bằng một ràng buộc bổ sung. 

Cuối cùng, mỗi tập hợp con được kiểm tra tính khả thi bằng cách kiểm tra xem phần giao của nó có chứa ít nhất$K$thành phần. Nếu đúng như vậy, kích thước tập hợp con đó sẽ trở thành câu trả lời ứng cử viên. 

Một điểm tinh tế là DP dựa vào việc lập chỉ mục nhất quán của bit được đặt thấp nhất để ánh xạ các tập hợp con tới bạn bè. Điều này đảm bảo mỗi tập hợp con được xây dựng chính xác một lần mà không bị trùng lặp hoặc thiếu chuyển tiếp. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
4 2 5
2 1 2
2 1 4
2 3 2
1 4
1 1
```Chúng tôi theo dõi một số nhóm nhỏ đại diện của bạn bè. 

| Tập hợp con bạn bè | Giao điểm của các thành phần được phép | Đếm | Khả thi ( ≥2) | Kích thước tập hợp con | 
| --- | --- | --- | --- | --- | 
| {} | {1,2,3,4} | 4 | vâng | 0 | 
| {1} | {3,4} | 2 | vâng | 1 | 
| {2,3} | {2,3} | 2 | vâng | 2 | 
| {1,2,3} | {3} | 1 | không | - | 

Kích thước tập hợp con khả thi tốt nhất là 3, phù hợp với đầu ra. 

Điều này cho thấy các ràng buộc kết hợp sẽ dần dần thu hẹp nhóm thành phần có sẵn như thế nào và chỉ các tập hợp con có phần giao nhau vẫn đủ lớn mới có thể hỗ trợ một món salad hợp lệ. 

### Mẫu 2 

đầu vào:```
2 2 3
0
1 1
1 2
```| Tập hợp con bạn bè | Giao lộ được phép | Đếm | Khả thi | Kích thước tập hợp con | 
| --- | --- | --- | --- | --- | 
| {} | {1,2} | 2 | vâng | 0 | 
| {1} | {1,2} | 2 | vâng | 1 | 
| {2} | {2} | 1 | không | 1 | 
| {3} | {1} | 1 | không | 1 | 
| {1,2} | {1,2} | 2 | vâng | 2 | 
| {1,3} | {1,2} | 2 | vâng | 2 | 
| {2,3} | {} | 0 | không | - | 

Kích thước tập hợp con tốt nhất là 1, vì không có hai người bạn nào có thể hài lòng đồng thời trong khi vẫn cho phép 2 thành phần. 

Ví dụ này nhấn mạnh rằng mặc dù các thành phần tồn tại với số lượng đủ trên toàn cầu nhưng những hạn chế có thể buộc giải pháp phải hy sinh khả năng tương thích giữa các thành phần. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(F \cdot 2^F + 2^F \cdot N/word)$| DP trên các tập hợp con bạn bè cộng với giao điểm bitmask và số lượng popcounts | 
| Không gian |$O(2^F)$| Lưu trữ mặt nạ giao lộ cho mọi nhóm nhỏ bạn bè | 

Với$F \le 20$, không gian tập hợp con là khoảng một triệu trạng thái và mỗi trạng thái chỉ sử dụng một vài phép toán theo bit trên số nguyên 50 bit. Điều này phù hợp thoải mái trong cả giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdin
    input = stdin.readline

    N, K, F = map(int, input().split())
    dislike = []
    for _ in range(F):
        arr = list(map(int, input().split()))
        f = arr[0]
        mask = 0
        for x in arr[1:]:
            mask |= 1 << (x - 1)
        dislike.append(mask)

    ALL = (1 << N) - 1
    allow = [(~d) & ALL for d in dislike]

    size = 1 << F
    inter = [0] * size

    for mask in range(size):
        if mask == 0:
            inter[mask] = ALL
        else:
            lsb = mask & -mask
            i = (lsb.bit_length() - 1)
            prev = mask ^ lsb
            inter[mask] = inter[prev] & allow[i]

    ans = 0
    for mask in range(size):
        if inter[mask].bit_count() >= K:
            ans = max(ans, mask.bit_count())

    return str(ans)

# provided samples
assert run("""4 2 5
2 1 2
2 1 4
2 3 2
1 4
1 1
""") == "3"

assert run("""2 2 3
0
1 1
1 2
""") == "1"

# minimum size
assert run("""1 1 1
0
""") == "1"

# all dislike everything except empty
assert run("""3 2 2
3 1 2 3
3 1 2 3
""") == "0"

# K = 0 edge (theoretical robustness)
assert run("""3 0 2
1 1
1 2
""") == "2"

# boundary: full compatibility
assert run("""4 2 2
0
0
""") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| hoàn toàn không thích | khả năng tương thích hoàn toàn với bạn bè | độ chính xác cơ bản | 
| xung đột đầy đủ | lựa chọn không thể | trường hợp sập ngã tư | 
| K = 0 | tính khả thi tầm thường | ổn định độ nét cạnh | 
| ràng buộc hỗn hợp | khả thi một phần | sự tương tác đúng đắn | 

## Vỏ cạnh 

Một trường hợp quan trọng xảy ra khi tất cả bạn bè không thích các tập hợp chồng chéo bao trùm toàn bộ vũ trụ thành phần. Trong tình huống đó, hầu hết các tập hợp con bạn bè nhanh chóng tạo ra các giao điểm trống và chỉ một tập hợp con nhỏ bạn bè vẫn khả thi. DP xử lý vấn đề này một cách chính xác vì bất kỳ giao lộ nào trở thành số 0 sẽ ngay lập tức thất bại.$K$-Kiểm tra thành phần, ngăn chặn việc đếm quá mức. 

Một trường hợp khác là không có người bạn nào không thích điều gì cả. Giao điểm của mỗi tập hợp con vẫn là tập hợp thành phần đầy đủ, do đó câu trả lời trở nên đơn giản là tất cả.$F$bạn bè, vì bất kỳ$K$-tập hợp con của các thành phần là hợp lệ. DP tự nhiên duy trì điều này vì các phép toán AND không bao giờ làm giảm mặt nạ.
