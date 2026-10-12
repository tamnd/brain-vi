---
title: "CF 105023B - Giao thông xe tải"
description: "Chúng ta có một biểu diễn một chiều về những gì Bob nhìn thấy dọc theo đường cao tốc, được mã hóa dưới dạng một chuỗi có độ dài $N$. Mỗi vị trí là đoạn xe tải được đánh dấu là T hoặc đoạn đường trống được đánh dấu là .."
date: "2026-06-28T01:43:22+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105023
codeforces_index: "B"
codeforces_contest_name: "HPI 2024 Novice"
rating: 0
weight: 105023
solve_time_s: 78
verified: false
draft: false
---

[CF 105023B - Giao thông xe tải](https://codeforces.com/problemset/problem/105023/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 18s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một biểu diễn một chiều về những gì Bob nhìn thấy dọc theo đường cao tốc, được mã hóa dưới dạng một chuỗi có độ dài$N$. Mỗi vị trí là một đoạn xe tải được đánh dấu là`T`hoặc đoạn đường trống được đánh dấu là`.`. Chúng ta có thể coi đây là một mảng nhị phân trong đó một số vị trí đóng góp khả năng hiển thị còn những vị trí khác thì không. 

Bob đang xem xét bổ sung thêm một đoạn xe tải liền kề có chiều dài$l_i$trên cùng dòng này. Điểm tự do chính là đoạn mới này có thể được đặt bắt đầu ở bất kỳ vị trí hợp lệ nào, miễn là nó vừa với chiều dài$N$. Sau khi được đặt, các vị trí chồng chéo với xe tải hiện có đương nhiên vẫn là xe tải, do đó tổng số xe tải có thể nhìn thấy sẽ trở thành số lượng vị trí được xếp vào vị trí đó.`T`ban đầu hoặc được bao phủ bởi phân khúc mới. 

Đối với mỗi độ dài truy vấn$l_i$, chúng ta phải chọn vị trí tối ưu của phân khúc được thêm này để tối đa hóa tổng số`T`vị trí sau khi chèn. 

Các hạn chế là nhỏ:$N, Q \le 100$. Điều này ngay lập tức cho chúng ta biết rằng bất kỳ giải pháp nào gần đúng$O(N^3)$vẫn sẽ trôi qua một cách thoải mái, vì tổng số hoạt động được giới hạn xung quanh$10^6$. 

Một sai lầm ngây thơ là cho rằng vị trí tốt nhất luôn ở giữa hoặc luôn thẳng hàng với các xe tải hiện có. Ví dụ, đã cho`T..T`và một đoạn dài, việc đặt tham lam gần các cụm hiện có có thể bỏ sót việc che phủ nhiều khoảng trống nhỏ mang lại tổng số tốt hơn. 

Một trường hợp thất bại tinh tế khác là sự hiểu lầm chồng chéo: nếu chúng ta thêm một phân đoạn lên trên`T`, chúng tôi không tính gấp đôi, do đó, bất kỳ cách tiếp cận nào chỉ đơn giản là thêm số lượng mà không che giấu sự chồng chéo sẽ đánh giá quá cao. 

Một trường hợp cạnh minh họa nhỏ: 

đầu vào:```
5 1
T.T.T
3
```Đặt đoạn trên các vị trí 2-4 bao gồm hai dấu chấm nhưng cũng chồng lên một chiếc xe tải ở vị trí 3. Câu trả lời đúng không phải là “3 xe tải hiện có + 3 chiếc được thêm vào”, mà là quy mô của liên minh. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. Đối với mỗi độ dài truy vấn$l$, chúng tôi thử mọi vị trí xuất phát có thể có của phân khúc xe tải được bổ sung. Đối với mỗi vị trí, chúng tôi tính toán lại tổng số`T`các vị trí trong sự kết hợp của chuỗi gốc và đoạn được đặt. Điều này đòi hỏi phải quét toàn bộ chuỗi cho từng vị trí, dẫn đến$O(N)$làm việc theo vị trí và$O(N)$vị trí, đưa ra$O(N^2)$mỗi truy vấn và$O(N^3)$tổng thể trên tất cả các truy vấn. 

Từ$N \le 100$, điều này đã có tác dụng nhưng nó lặp đi lặp lại một cách không cần thiết. Quan sát quan trọng là khi chúng ta đặt một đoạn có chiều dài$l$bắt đầu ở vị trí$i$, khu vực duy nhất bị ảnh hưởng là cửa sổ đó. Mọi thứ bên ngoài nó vẫn cố định và bên trong nó chúng tôi đang thay thế một số`.`với`T`. 

Vì vậy, thay vì tính toán lại từ đầu, chúng tôi có thể tính toán trước tổng tiền tố của các xe tải hiện có rồi đánh giá từng vị trí trong$O(1)$. Lợi ích là mỗi điểm cửa sổ có thể tính toán được trong thời gian không đổi, giảm mỗi truy vấn xuống còn$O(N)$. 

Chúng tôi tính toán cho một phân khúc đã chọn$[i, i+l-1]$, phần đóng góp như sau: 

xe tải hiện có bên ngoài cửa sổ + chiều dài cửa sổ đầy đủ (vì tất cả các vị trí bên trong trở nên`T`sau khi chèn, nhưng các xe tải chồng chéo đã được tính nên chúng tôi sửa thông qua tổng tiền tố). 

Điều này làm giảm mỗi truy vấn để quét tất cả các vị trí hợp lệ và đánh giá một công thức đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(QN^3)$|$O(1)$| Quá chậm ở dạng chung | 
| Cửa sổ tổng tiền tố |$O(QN)$|$O(N)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuyển đổi chuỗi thành mảng nhị phân trong đó`T = 1`Và`.`= 0. Điều này cho phép đếm số học xe tải. 
2. Xây dựng một mảng tổng tiền tố để chúng ta có thể truy vấn số lượng xe tải hiện có trong bất kỳ khoảng thời gian nào trong thời gian không đổi. 
3. Đối với mỗi độ dài truy vấn$l$, lặp lại tất cả các vị trí bắt đầu có thể$i$từ$0$ĐẾN$N-l$. 
4. Đối với từng vị trí$i$, tính xem có bao nhiêu xe tải ban đầu nằm bên trong cửa sổ$[i, i+l-1]$. 
5. Tính tổng số xe tải thu được nếu chúng ta đặt đoạn mới ở đó như sau: 

tổng số xe hiện có + (chiều dài cửa sổ - xe tải bên trong cửa sổ). 

Công thức này phản ánh rằng mọi ô trống trong cửa sổ đều trở thành xe tải, trong khi các xe tải hiện có đã được tính. 
6. Theo dõi giá trị tối đa trên tất cả các vị trí và xuất giá trị đó cho truy vấn. 

Điểm tinh tế là bước 5. Chúng tôi không tính toán lại toàn bộ chuỗi; chúng tôi chỉ đang đo xem có bao nhiêu số 0 được chuyển đổi thành số 0 trong cửa sổ đó. 

### Tại sao nó hoạt động 

Thuật toán ngầm duy trì tính bất biến rằng mọi vị trí bên ngoài cửa sổ đã chọn đều đóng góp chính xác giá trị ban đầu của nó, trong khi bên trong cửa sổ, mọi vị trí đều trở thành một chiếc xe tải bất kể trạng thái ban đầu của nó. Do đó, số đếm cuối cùng là tổng số ban đầu cộng với chính xác số ô trống bên trong cửa sổ đã chọn. Vì mỗi vị trí được đánh giá chính xác một lần và tất cả các phép biến đổi đều được tính toán chính xác mà không cần tính hai lần, nên mức tối đa trên tất cả các cửa sổ là câu trả lời đúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    N, Q = map(int, input().split())
    s = input().strip()

    a = [1 if c == 'T' else 0 for c in s]

    pref = [0] * (N + 1)
    for i in range(N):
        pref[i + 1] = pref[i] + a[i]

    total = pref[N]

    def range_sum(l, r):
        return pref[r] - pref[l]

    for _ in range(Q):
        L = int(input())
        best = 0

        for i in range(N - L + 1):
            j = i + L
            inside = range_sum(i, j)
            candidate = total + (L - inside)
            if candidate > best:
                best = candidate

        print(best)

if __name__ == "__main__":
    solve()
```Giải pháp trước tiên nén dữ liệu đầu vào thành biểu diễn nhị phân, giúp đơn giản hóa việc suy luận về sự chồng chéo. Mảng tổng tiền tố cho phép chúng tôi tính toán số lượng xe tải đã tồn tại bên trong bất kỳ cửa sổ sắp xếp ứng viên nào trong thời gian không đổi. 

Đối với mỗi truy vấn, chúng tôi đánh giá mọi vị trí có thể có của phân khúc xe tải mới. biểu hiện`total + (L - inside)`thu được lợi nhuận ròng: mỗi ô trống sẽ trở thành một chiếc xe tải, trong khi những chiếc xe tải hiện có trong cửa sổ không tăng tổng số vì chúng đã được tính một lần. 

The nested loop structure is safe because both$N$Và$Q$nhỏ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
N = 5, s = T.T.T
L = 3
```Tổng tiền tố: 

| tôi | s[i] | trước | 
| --- | --- | --- | 
| 0 | T | 1 | 
| 1 | . | 1 | 
| 2 | T | 2 | 
| 3 | . | 2 | 
| 4 | T | 3 | 

Bây giờ chúng tôi đánh giá các cửa sổ: 

| bắt đầu tôi | cửa sổ | bên trong xe tải | ứng cử viên | 
| --- | --- | --- | --- | 
| 0 | T.T | 2 | 3 + (3 - 2) = 4 | 
| 1 | .T. | 1 | 3 + (3 - 1) = 5 | 
| 2 | T.T | 2 | 4 | 

Tốt nhất là 5. 

Điều này cho thấy vị trí bố trí tối ưu không nhất thiết phải phù hợp với các xe tải hiện có; bao gồm hai vị trí trống mang lại sự cải thiện tốt nhất. 

### Ví dụ 2 

đầu vào:```
N = 6, s = TT..T.
L = 2
```Tổng số xe tải = 3. 

| bắt đầu tôi | cửa sổ | bên trong | ứng cử viên | 
| --- | --- | --- | --- | 
| 0 | TT | 2 | 3 + 0 = 3 | 
| 1 | T. | 1 | 3 + 1 = 4 | 
| 2 | .. | 0 | 3 + 2 = 5 | 
| 3 | .T | 1 | 3 + 1 = 4 | 
| 4 | T. | 1 | 4 | 

Tốt nhất là 5. 

Điều này xác nhận rằng chiến lược tốt nhất là tối đa hóa số lượng ô trống được bao phủ bởi phân đoạn được thêm vào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(QN)$| Mỗi truy vấn sẽ quét tất cả$N$vị trí, mỗi vị trí được tính theo O(1) bằng cách sử dụng tổng tiền tố | 
| Không gian |$O(N)$| Mảng tổng tiền tố trên chuỗi | 

Được cho$N, Q \le 100$, tổng số thao tác nhiều nhất là$10^4$, đó là thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    N, Q = map(int, input().split())
    s = input().strip()

    a = [1 if c == 'T' else 0 for c in s]
    pref = [0] * (N + 1)
    for i in range(N):
        pref[i + 1] = pref[i] + a[i]
    total = pref[N]

    out = []
    for _ in range(Q):
        L = int(input())
        best = 0
        for i in range(N - L + 1):
            inside = pref[i + L] - pref[i]
            best = max(best, total + (L - inside))
        out.append(str(best))

    return "\n".join(out) + "\n"

# provided sample
assert run("8 3\nTT..T.T.\n1\n3\n8\n") == "5\n6\n8\n"

# minimum case
assert run("1 1\n.\n1\n") == "1\n"

# all trucks
assert run("5 1\nTTTTT\n2\n") == "5\n"

# all empty
assert run("5 1\n.....\n3\n") == "3\n"

# alternating
assert run("6 1\nT.T.T.\n2\n") == "4\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều trống rỗng | L | thu được toàn bộ lợi ích từ vị trí | 
| tất cả các xe tải | N | không thể cải thiện được | 
| xen kẽ | 4 | xử lý chồng chéo đúng cách | 
| kích thước tối thiểu | 1 | độ đúng ranh giới | 

## Vỏ cạnh 

Một chuỗi hoàn toàn trống là trường hợp nhạy cảm nhất đối với các lỗi riêng lẻ. Ví dụ,`.....`với$L=3$sẽ trả về 3 vì bất kỳ vị trí nào cũng chuyển đổi hoàn toàn phân khúc đã chọn. Thuật toán xử lý việc này vì`inside = 0`cho mọi cửa sổ, do đó mức tăng luôn chính xác$L$. 

Một chuỗi đầy đủ là cực đoan ngược lại. Vì`TTTTT`, bất kỳ vị trí nào cũng không mang lại sự cải thiện nào, vì`L - inside = 0`ở khắp mọi nơi. Thuật toán giữ chính xác câu trả lời tại$N$vì tổng tiền tố luôn khớp với độ dài cửa sổ. 

Một mô hình xen kẽ hỗn hợp như`T.T.T.`đảm bảo rằng sự chồng chéo một phần được xử lý chính xác. Phép trừ tổng tiền tố sẽ tách biệt chính xác số lượng xe tải đã có mặt bên trong mỗi cửa sổ, ngăn chặn việc đếm hai lần và làm cho mức tăng được tính toán chính xác.
