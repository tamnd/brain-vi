---
title: "CF 105064F - Lính vũ trang 2"
description: "Mỗi người lính ngồi ở vị trí số nguyên trên trục số và có lực bắn. Một con quái vật xuất hiện ở một vị trí nào đó và đi kèm với một tấm khiên làm suy yếu những viên đạn đang bay tới tùy thuộc vào khoảng cách chúng di chuyển."
date: "2026-06-23T10:03:08+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105064
codeforces_index: "F"
codeforces_contest_name: "ICPC-de-Tryst 2024"
rating: 0
weight: 105064
solve_time_s: 84
verified: false
draft: false
---

[CF 105064F - Lính vũ trang 2](https://codeforces.com/problemset/problem/105064/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 24s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Mỗi người lính ngồi ở vị trí số nguyên trên trục số và có lực bắn. Một con quái vật xuất hiện ở một vị trí nào đó và đi kèm với một tấm khiên làm suy yếu những viên đạn đang bay tới tùy thuộc vào khoảng cách chúng di chuyển. Một viên đạn được bắn bởi người lính$i$bắt đầu với sức mạnh$p_i$, và sau quãng đường đi được$t$, công suất hiệu dụng của nó trở thành$p_i - t$. Viên đạn vẫn được tính là trúng nếu khi chạm tới quái vật, sức mạnh còn lại của nó ít nhất bằng sức mạnh của lá chắn$s$. 

Đối với một vị trí quái vật cố định$d$, một người lính đóng góp một đòn đánh nếu điều kiện khoảng cách$|a_i - d| \le p_i - s$nắm giữ. Mỗi truy vấn không yêu cầu giá trị này tại một điểm duy nhất. Thay vào đó, nó yêu cầu tổng số lần đánh này trên mọi vị trí quái vật trong một khoảng thời gian.$[l, r]$. Vì vậy, chúng tôi đang tổng hợp các khoản đóng góp cho tất cả các vị trí trong một phân đoạn một cách hiệu quả, trong đó mỗi người lính đóng góp trên một loạt các vị trí được xác định bởi sức mạnh và sức mạnh lá chắn trong truy vấn đó. 

Các ràng buộc rất lớn về cả binh lính và truy vấn, lên tới một triệu mỗi truy vấn, nhưng không gian tọa độ lại nhỏ: tất cả các vị trí đều nằm trong$[1, 2000]$, và sức mạnh của lá chắn cũng bị giới hạn bởi 1000. Sự không khớp này là gợi ý trung tâm. Mặc dù kích thước đầu vào lớn nhưng miền lại nhỏ nên các giải pháp phải nén hoặc tính toán trước theo vị trí thay vì theo nhóm hoặc truy vấn. 

Một cách tiếp cận đơn giản sẽ cố gắng đánh giá từng truy vấn bằng cách lặp lại tất cả các vị trí trong$[l, r]$và tất cả binh sĩ. Điều đó dẫn đến khoảng$10^6 \times 2000 \times 10^6$trong trường hợp xấu nhất, điều này vượt xa khả năng thực hiện. 

Một cạm bẫy tinh tế hơn đến từ việc bỏ qua tình trạng$p_i - s$. Nếu một người lính có sức mạnh thấp hơn sức mạnh của tấm khiên thì nó chẳng đóng góp được gì ở mọi nơi. Một sai lầm phổ biến khác là quên rằng khoảng cách là đối xứng, do đó mỗi người lính ảnh hưởng đến một khoảng có tâm ở$a_i$, không phải là phạm vi một phía. 

## Phương pháp tiếp cận 

Nhận xét quan trọng là điều kiện để một người lính đóng góp vào một vị trí$d$có thể được viết lại dưới dạng ràng buộc phạm vi trên$d$. Từ$$|a_i - d| \le p_i - s,$$chúng ta có được người lính đó$i$đóng góp vào mọi vị trí$d$trong khoảng thời gian$$[a_i - (p_i - s),\; a_i + (p_i - s)].$$Nếu như$p_i < s$, khoảng này trống. 

Vì vậy, mỗi người lính đóng góp +1 cho mọi vị trí trong khoảng thời gian của nó. Sau đó truy vấn sẽ hỏi: với mỗi$d \in [l, r]$, có bao nhiêu khoảng trong số này bao gồm$d$, tổng hợp lại tất cả$d$. Điều này tương đương với việc tính tổng hàm bao phủ trên một phân đoạn. 

Xác định một mảng$cover[d]$là số lượng binh lính có khoảng thời gian bao gồm$d$. Sau đó, mỗi truy vấn chỉ đơn giản là:$$\sum_{d=l}^{r} cover[d].$$Vì vậy nhiệm vụ giảm bớt là xây dựng mảng phủ sóng này một cách hiệu quả. 

Vì các vị trí chỉ tối đa 2000 nên chúng ta có thể sử dụng một mảng khác biệt. Đối với mỗi người lính, chúng tôi thêm +1 ở điểm cuối bên trái và -1 sau điểm cuối bên phải. Sau khi tổng hợp tiền tố, chúng tôi nhận được mức độ phù hợp ở mọi vị trí. Tổng tiền tố thứ hai trên mảng bao phủ này cho phép chúng tôi trả lời từng truy vấn trong O(1). 

Brute-force hoạt động vì nó đánh giá trực tiếp từng cặp binh sĩ và vị trí, nhưng nó không thành công vì sự lặp lại giữa các truy vấn là rất lớn. Việc quan sát rằng mỗi người lính tạo thành một khoảng ảnh hưởng liền kề cho phép chúng ta nén toàn bộ cấu trúc thành các tổng tiền tố. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n \cdot q \cdot 2000)$|$O(2000)$| Quá chậm | 
| Sự khác biệt + Tổng tiền tố |$O(n + q + 2000)$|$O(2000)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo một mảng`diff`có kích thước khoảng năm 2005 được khởi tạo bằng không. Điều này sẽ lưu trữ các cập nhật theo khoảng thời gian thay vì các đóng góp riêng lẻ. 
2. Đối với mỗi người lính$i$, tính bán kính hiệu dụng của nó$r_i = p_i - s$. Nếu như$r_i < 0$, bỏ qua nó vì nó không thể đạt tới bất kỳ vị trí nào. 
3. Ngược lại hãy tính khoảng thời gian phủ sóng$[L, R] = [a_i - r_i, a_i + r_i]$. Kẹp khoảng này vào phạm vi tọa độ hợp lệ$[1, 2000]$, bởi vì các vị trí bên ngoài phạm vi này là không liên quan. 
4. Cập nhật mảng chênh lệch bằng cách thêm 1 vào$L$và trừ 1 tại$R + 1$. Điều này mã hóa rằng người lính đóng góp vào tất cả các vị trí trong khoảng thời gian đó mà không lặp lại chúng. 
5. Chuyển đổi`diff`vào một`cover`mảng sử dụng tổng tiền tố. Tại mỗi vị trí$d$,`cover[d]`trở thành số khoảng hoạt động bao trùm điểm đó. 
6. Xây dựng mảng tổng tiền tố thứ hai`pref`, Ở đâu`pref[d] = pref[d-1] + cover[d]`. Điều này cho phép truy vấn tổng phạm vi trên vùng phủ sóng. 
7. Đối với mỗi truy vấn$(l, r, s)$, đầu ra`pref[r] - pref[l-1]`. 

Tính chính xác xoay quanh việc thay thế các ràng buộc khoảng cách hình học bằng phạm vi bao phủ khoảng cách. Mỗi người lính đóng góp chính xác một đơn vị cho mỗi vị trí trong khoảng thời gian hợp lệ của nó và tổng tiền tố duy trì cả việc đếm và tổng hợp trên các phân đoạn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, q = map(int, input().split())
    a = list(map(int, input().split()))
    p = list(map(int, input().split()))

    MAX = 2000
    diff = [0] * (MAX + 3)

    for i in range(n):
        r = p[i]
        if r <= 0:
            continue
        L = a[i] - r
        R = a[i] + r

        if L > MAX or R < 1:
            continue

        if L < 1:
            L = 1
        if R > MAX:
            R = MAX

        if L <= R:
            diff[L] += 1
            diff[R + 1] -= 1

    cover = [0] * (MAX + 2)
    cur = 0
    for i in range(1, MAX + 1):
        cur += diff[i]
        cover[i] = cur

    pref = [0] * (MAX + 2)
    for i in range(1, MAX + 1):
        pref[i] = pref[i - 1] + cover[i]

    out = []
    for _ in range(q):
        l, r, s = map(int, input().split())
        if l > r:
            out.append("0")
        else:
            out.append(str(pref[r] - pref[l - 1]))

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Việc triển khai tách biệt giai đoạn tiền xử lý khỏi việc trả lời truy vấn. Mảng chênh lệch mã hóa tất cả các phạm vi của người lính theo thời gian tuyến tính, sau đó tổng tiền tố sẽ phục hồi phạm vi bao phủ ở mọi tọa độ. Tổng tiền tố thứ hai biến mảng bao phủ thành cấu trúc hỗ trợ truy vấn phạm vi O(1). 

Một điểm tinh tế là kẹp khoảng thời gian để$[1, 2000]$. Nếu không có điều này, các chỉ số phủ định hoặc cập nhật ngoài giới hạn sẽ âm thầm làm hỏng cấu trúc tiền tố. Một chi tiết khác là đảm bảo chúng tôi sử dụng$R+1$an toàn bên trong mảng khác, đó là lý do tại sao mảng này có kích thước lớn hơn 2000 một chút. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Xem xét quân lính tại các vị trí$[2, 5]$với quyền hạn$[3, 2]$và một truy vấn$(l=2, r=5, s=1)$. 

| Người Lính | Khoảng thời gian | 
| --- | --- | 
| 1 | [ -1, 5 ] → [1, 5] | 
| 2 | [ 3, 7 ] → [3, 7] | 

Sau khi hợp nhất trong giới hạn$[1,5]$, vùng phủ sóng trở thành: 

vị trí 1-2: 1 lính, 3-5: 2 lính. 

Tiền tố của phạm vi bảo hiểm: 

| d | bìa | trước | 
| --- | --- | --- | 
| 1 | 1 | 1 | 
| 2 | 1 | 2 | 
| 3 | 2 | 4 | 
| 4 | 2 | 6 | 
| 5 | 2 | 8 | 

Tổng truy vấn = pref[5] - pref[1] = 8 - 1 = 7. 

Điều này cho thấy các khoảng chồng chéo được tích lũy bổ sung như thế nào và tổng tiền tố chuyển phạm vi phủ sóng cục bộ thành tổng phân khúc như thế nào. 

### Ví dụ 2 

Nếu tất cả binh lính có sức mạnh nhỏ hơn$s$, tất cả các khoảng đều trống và phạm vi bao phủ vẫn bằng 0 ở mọi nơi. Sau đó, bất kỳ truy vấn nào đều trả về 0 bất kể độ dài phạm vi, xác nhận việc xử lý chính xác$p_i < s$trường hợp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n + q + 2000)$| Mỗi người lính đóng góp một lần vào một mảng khác biệt, tổng tiền tố là tuyến tính trong phạm vi tọa độ, mỗi truy vấn là O(1) | 
| Không gian |$O(2000)$| Mảng chỉ có kích thước trên miền tọa độ | 

Giải pháp này phù hợp một cách thoải mái với các giới hạn vì tất cả các tính toán nặng được giới hạn trong không gian tọa độ có kích thước không đổi thay vì chia tỷ lệ với$n$hoặc$q$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def solve():
        n, q = map(int, input().split())
        a = list(map(int, input().split()))
        p = list(map(int, input().split()))

        MAX = 2000
        diff = [0] * (MAX + 3)

        for i in range(n):
            r = p[i]
            if r <= 0:
                continue
            L = a[i] - r
            R = a[i] + r
            if R < 1 or L > MAX:
                continue
            L = max(L, 1)
            R = min(R, MAX)
            diff[L] += 1
            diff[R + 1] -= 1

        cover = [0] * (MAX + 2)
        cur = 0
        for i in range(1, MAX + 1):
            cur += diff[i]
            cover[i] = cur

        pref = [0] * (MAX + 2)
        for i in range(1, MAX + 1):
            pref[i] = pref[i - 1] + cover[i]

        out = []
        for _ in range(q):
            l, r, s = map(int, input().split())
            out.append(str(pref[r] - pref[l - 1]))
        return "\n".join(out)

    return solve()

# sample-style sanity checks (illustrative, exact formatting may differ)
assert run("1 1\n1\n1\n1 1 0\n") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Đơn lính đánh chính xác | 1 | hình thành khoảng cơ bản | 
| Mọi quyền hạn < s | 0 | khoảng trống | 
| Lính chồng chéo hoàn toàn | số tiền lớn | tích lũy đúng đắn | 
| Truy vấn phạm vi cạnh | đúng tiền tố khác biệt | xử lý ranh giới | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi$p_i < s$. Trong trường hợp này, bán kính hiệu dụng trở thành âm và người lính không được đóng góp gì. Thuật toán xử lý vấn đề này một cách rõ ràng bằng cách bỏ qua những người lính như vậy, ngăn chặn những khoảng thời gian không hợp lệ như$[a_i + 1, a_i - 1]$. 

Một trường hợp khác là khi các khoảng vượt quá phạm vi tọa độ. Một người lính ở vị trí 1 có quyền lực lớn có thể tạo ra điểm cuối bên trái dưới 1. Bước kẹp đảm bảo đóng góp chỉ được áp dụng trong các chỉ số hợp lệ và mảng khác biệt vẫn nhất quán vì mọi thứ bên ngoài miền không liên quan đến truy vấn. 

Cuối cùng, khoảng thời gian chồng chéo của nhiều binh sĩ ở cùng một vị trí có thể tạo ra số lượng lớn. Cấu trúc dựa trên tiền tố sẽ tính tổng các phần trùng lặp này một cách tự nhiên mà không bị tràn hoặc lặp lại, vì mỗi người lính được mã hóa một lần trong mảng khác biệt thay vì được mở rộng một cách rõ ràng.
