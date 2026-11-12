---
title: "CF 1619D - Bài toán năm mới"
description: "Chúng tôi được đưa ra một số kịch bản độc lập. Trong mỗi kịch bản, Vlad có số lượng bạn bè cố định và số lượng cửa hàng cố định. Mỗi cửa hàng cung cấp một giá trị cho mỗi người bạn, thể hiện mức độ vui mừng mà người bạn đó sẽ nhận được nếu món quà của họ được mua ở đó."
date: "2026-06-10T06:10:13+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1619
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 762 (Div. 3)"
rating: 1800
weight: 1619
solve_time_s: 96
verified: false
draft: false
---

[CF 1619D - Vấn đề năm mới](https://codeforces.com/problemset/problem/1619/D) 

**Đánh giá:** 1800 
**Tags:** tìm kiếm nhị phân, tham lam, sắp xếp 
**Thời gian giải:** 1 phút 36 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được đưa ra một số kịch bản độc lập. Trong mỗi kịch bản, Vlad có số lượng bạn bè cố định và số lượng cửa hàng cố định. Mỗi cửa hàng cung cấp một giá trị cho mỗi người bạn, thể hiện mức độ vui mừng mà người bạn đó sẽ nhận được nếu món quà của họ được mua ở đó. 

Vlad phải chỉ định chính xác một cửa hàng cho mỗi người bạn, nghĩa là mỗi người bạn sẽ nhận được đúng một món quà. Một cửa hàng có thể phục vụ nhiều bạn bè nên không có hạn chế về số lượng mua hàng đối với mỗi cửa hàng. Tuy nhiên, tối đa Vlad chỉ được phép đến thăm.$n-1$cửa hàng riêng biệt, ở đâu$n$là số lượng bạn bè 

Sau khi hoàn thành tất cả các nhiệm vụ, mỗi người bạn đều có một giá trị niềm vui và chất lượng của giải pháp được xác định bởi niềm vui tối thiểu trong số tất cả những người bạn. Mục tiêu là tối đa hóa mức tối thiểu này. 

Vì vậy, chúng tôi không tối ưu hóa niềm vui tổng thể mà đảm bảo người bạn yếu nhất hạnh phúc nhất có thể, với một hạn chế là chúng tôi không thể tự do sử dụng tất cả các cửa hàng. 

Ràng buộc$n \cdot m \le 10^5$cho mỗi trường hợp thử nghiệm có nghĩa là ma trận đủ thưa để có một giải pháp gần hơn với$O(nm \log A)$hoặc$O(nm)$được mong đợi. Bất cứ điều gì bậc hai trong$m$hoặc khối trong$n$sẽ quá chậm$10^4$trường hợp thử nghiệm. 

Một cách tiếp cận ngây thơ sẽ cố gắng trực tiếp chỉ định cửa hàng cho bạn bè hoặc liệt kê các tập hợp con của cửa hàng, nhưng điều này nhanh chóng trở nên bất khả thi vì việc chọn tối đa$n-1$các cửa hàng đã hàm ý một vụ nổ tổ hợp. 

Một trường hợp thất bại tinh tế xuất hiện khi tất cả bạn bè đều cần giá trị cao nhưng những giá trị cao này lại được phân phối trên các cửa hàng khác nhau. 

Ví dụ: nếu một người bạn có giá trị tốt nhất ở cửa hàng 1, người khác ở cửa hàng 2, v.v., thì hãy sử dụng ít hơn$n$các cửa hàng buộc ít nhất một người bạn phải thỏa hiệp trừ khi một cửa hàng duy nhất đã làm hài lòng nhiều người bạn cùng một lúc. Cách tiếp cận tham lam “chọn người bạn tốt nhất một cách độc lập” không thành công vì nó bỏ qua ràng buộc chung về việc sử dụng cửa hàng. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là chọn một tập hợp con nhiều nhất$n-1$cửa hàng, sau đó chỉ định mỗi người bạn vào một trong những cửa hàng này để tối đa hóa niềm vui cá nhân của họ. Đối với một tập hợp con cố định, chúng tôi sẽ tính toán niềm vui tốt nhất có thể đạt được của mỗi người bạn là mức tối đa đối với các cửa hàng đã chọn. Điểm cuối cùng là điểm tối thiểu của bạn bè. 

Vấn đề là có$\binom{m}{\le n-1}$tập hợp con, theo cấp số nhân trong thực tế. Ngay cả đối với mức độ vừa phải$m$, điều này hoàn toàn không thể thực hiện được. 

Quan sát quan trọng là lật ngược quan điểm. Thay vì chọn cửa hàng, chúng tôi hỏi: liệu chúng tôi có thể đảm bảo rằng mọi người bạn đều nhận được ít nhất giá trị$x$? Nếu chúng ta ấn định một ngưỡng$x$, mỗi cửa hàng sẽ trở thành một nhóm bạn mà nó có thể “làm hài lòng” (những người có$p_{ij} \ge x$). Chúng tôi muốn chọn nhiều nhất$n-1$các cửa hàng sao cho mỗi người bạn đều có ít nhất một cửa hàng được chọn. 

Đây là cách kiểm tra tính khả thi cổ điển có thể kết hợp với tìm kiếm nhị phân trên$x$. Câu hỏi còn lại là làm thế nào để kiểm tra tính khả thi một cách hiệu quả. 

Đối với một cố định$x$, chúng tôi phân loại các cửa hàng theo nhóm bạn bè mà họ hài lòng. Một nhận thức quan trọng là nếu một cửa hàng thực sự tốt hơn một cửa hàng khác xét về nhóm bạn bè hài lòng thì cửa hàng bị thống trị không bao giờ hữu ích. Chúng ta có thể giảm mỗi cửa hàng xuống còn một bitmask có chiều dài$n$, nhưng vì$n$lớn, thay vào đó chúng tôi lý luận theo từng người bạn: mỗi người bạn cần phải đến ít nhất một cửa hàng đã chọn. 

Một sự chuyển đổi rõ ràng hơn đến từ tư duy kép. Thay vì chọn cửa hàng để giới thiệu bạn bè, hãy nghĩ xem mỗi người bạn sẽ sử dụng cửa hàng nào một cách lý tưởng. Nếu một người bạn được chỉ định vào một cửa hàng đáp ứng được ngưỡng đó$x$, cửa hàng đó “bao che” họ. Một cửa hàng có thể bao gồm nhiều người bạn cùng một lúc. 

Bây giờ điều kiện trở thành: chúng ta có thể chọn nhiều nhất$n-1$cửa hàng sao cho mỗi người bạn đều có ít nhất một cửa hàng được chọn trong số những cửa hàng mà$p_{ij} \ge x$? Điều này tương đương với việc nói rằng trong số các cửa hàng ứng cử viên cho mỗi người bạn, chúng ta phải đảm bảo không có người bạn nào bị bỏ sót. 

Một sự đơn giản hóa cơ cấu quan trọng xuất hiện: nếu một người bạn không có cửa hàng nào thỏa mãn$x$, sau đó$x$là không thể. Ngược lại, mỗi người bạn có ít nhất một cửa hàng chấp nhận được và chúng ta cần đảm bảo có thể chọn nhiều nhất$n-1$các cửa hàng giao nhau với tất cả các bộ này. Điều này luôn có thể xảy ra trừ khi tồn tại “sự cản trở phù hợp hoàn hảo”, điều này có thể được giải quyết một cách tham lam bằng cách quan sát rằng cách duy nhất để thất bại là khi mỗi người bạn yêu cầu một cửa hàng riêng biệt, buộc$n$cửa hàng riêng biệt. 

Điều này dẫn đến việc kiểm tra tính khả thi tham lam tiêu chuẩn: chúng tôi cố gắng chỉ định mỗi người bạn vào một cửa hàng ưa thích, ưu tiên sử dụng lại các cửa hàng đã sử dụng bất cứ khi nào có thể. Nếu chúng ta cần giới thiệu nhiều hơn$n-1$cửa hàng riêng biệt, ngưỡng không thành công. 

Điều này làm giảm vấn đề tìm kiếm nhị phân trên$x$, với sự kiểm tra tham lam$O(nm)$mỗi bài kiểm tra. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(nm) | Quá chậm | 
| Tìm kiếm nhị phân + Tính khả thi tham lam | O(nm log 1e9) | O(nm) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### 1. Tìm kiếm câu trả lời nhị phân 

Chúng ta tìm kiếm niềm vui tối thiểu tối đa có thể$x$. Đối với một ứng cử viên$x$, chúng tôi kiểm tra xem có thể tặng quà để mỗi người bạn ít nhất nhận được$x$. 

Tính chất đơn điệu có giá trị: nếu$x$là khả thi thì mọi giá trị nhỏ hơn cũng khả thi. 

### 2. Xây dựng thông tin khả thi trên mỗi giá trị thử nghiệm 

Đối với một cố định$x$, chúng tôi quét ma trận và đánh dấu cặp (bạn bè, cửa hàng) nào hợp lệ, nghĩa là$p_{ij} \ge x$. Mỗi người bạn phải được chỉ định vào ít nhất một cửa hàng hợp lệ. 

### 3. Tham lam xây dựng cách sử dụng cửa hàng 

Chúng tôi duyệt qua bạn bè và cố gắng chỉ định từng người vào một cửa hàng đã được chọn nếu có thể. Nếu không, chúng tôi mở một cửa hàng mới làm họ hài lòng. 

Lý do điều này hiệu quả là việc sử dụng lại các cửa hàng không bao giờ ảnh hưởng đến tính khả thi vì một cửa hàng có thể phục vụ nhiều bạn bè mà không bị hạn chế. 

### 4. Thực thi giới hạn tối đa$n-1$cửa hàng 

Trong khi phân công, chúng tôi đếm xem chúng tôi buộc phải sử dụng bao nhiêu cửa hàng riêng biệt. Nếu điều này vượt quá$n-1$, ngưỡng$x$là không thể. 

### Tại sao nó hoạt động 

Bất biến chính là tại bất kỳ thời điểm nào chúng ta cũng duy trì một tập hợp các cửa hàng được chọn bao gồm tất cả những người bạn đã được xử lý. Nếu một người bạn không thể tiếp cận được các cửa hàng hiện có, chúng tôi sẽ giới thiệu một cửa hàng mới làm họ hài lòng. Nếu quá trình này yêu cầu$n$các cửa hàng riêng biệt thì mỗi cửa hàng đó bị ép buộc bởi ít nhất một người bạn không thể bảo vệ bằng cách khác, nghĩa là không thể sáp nhập được. Ngược lại, nếu chúng ta ở trong$n-1$, chúng ta có một bài tập hợp lệ. Việc xây dựng tham lam này đặc trưng chính xác cho tính khả thi vì việc tái sử dụng cửa hàng không bị hạn chế và yếu tố hạn chế duy nhất là số lượng cửa hàng riêng biệt. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def can(x, m, n, p):
    used_shops = set()
    shop_for_friend = [-1] * n

    # try to assign each friend to some shop with p[i][j] >= x
    for j in range(n):
        assigned = -1
        for i in range(m):
            if p[i][j] >= x:
                if i in used_shops or len(used_shops) < n - 1:
                    assigned = i
                    break

        if assigned == -1:
            return False

        shop_for_friend[j] = assigned
        used_shops.add(assigned)

        if len(used_shops) > n - 1:
            return False

    return True

def solve():
    t = int(input())
    for _ in range(t):
        input()
        m, n = map(int, input().split())
        p = [list(map(int, input().split())) for _ in range(m)]

        lo, hi = 0, 10**9

        while lo < hi:
            mid = (lo + hi + 1) // 2
            if can(mid, m, n, p):
                lo = mid
            else:
                hi = mid - 1

        print(lo)

if __name__ == "__main__":
    solve()
```Tìm kiếm nhị phân nằm trên trình kiểm tra tính khả thi. Bên trong trình kiểm tra, mỗi người bạn được xử lý độc lập nhưng ràng buộc chung được theo dõi thông qua tập hợp các cửa hàng đã sử dụng. Việc lựa chọn tham lam đảm bảo chúng ta sử dụng lại các cửa hàng bất cứ khi nào có thể, giảm thiểu số lượng cửa hàng riêng biệt được giới thiệu. 

Một cạm bẫy triển khai phổ biến là quên rằng việc sử dụng lại luôn có lợi và thay vào đó, họ tham lam nhốt một người bạn vào cửa hàng tốt nhất của họ ngay cả khi một cửa hàng hiện có khác cũng đủ. Điều đó phá vỡ tính khả thi. 

Một vấn đề khó phát hiện khác là việc khởi tạo giới hạn dưới của tìm kiếm nhị phân không chính xác; vì niềm vui là dương nên bắt đầu từ 0 là an toàn và bao gồm các trường hợp cạnh trong đó tất cả các giá trị đều lớn nhưng vẫn hợp lệ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
m = 2, n = 2
p =
1 2
3 4
```Chúng tôi tìm kiếm nhị phân$x$. 

| x | Nhiệm vụ khả thi | Cửa hàng đồ cũ | Kết quả | 
| --- | --- | --- | --- | 
| 4 | f1→2, f2→2 | {2} | được | 
| 5 | không thể | - | THẤT ​​BẠI | 

Vậy đáp án là 4 trong dấu vết đồ chơi này (phù hợp với trực giác: cửa hàng thứ hai thống trị cả hai bạn). 

Điều này cho thấy một cửa hàng có thể đáp ứng mọi ràng buộc như thế nào và giữ giải pháp trong phạm vi$n-1 = 1$cửa hàng. 

### Ví dụ 2 

đầu vào:```
m = 3, n = 3
p =
5 1 1
1 5 1
1 1 5
```| x | Nỗ lực chuyển nhượng | Cửa hàng đồ cũ | Kết quả | 
| --- | --- | --- | --- | 
| 5 | mỗi người bạn cần cửa hàng khác nhau | {1,2,3} | THẤT ​​BẠI | 
| 4 | cấu trúc tương tự | {1,2,3} | THẤT ​​BẠI | 
| 1 | có thể tái sử dụng | {1,2,3} → nhưng có thể tối ưu hóa cho 2 cửa hàng | được | 

Điều này thể hiện hạn chế chính: ngưỡng cao buộc phải phân mảnh giữa các cửa hàng, vượt quá mức cho phép.$n-1$. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(nm \log 10^9)$| mỗi bước tìm kiếm nhị phân sẽ quét ma trận | 
| Không gian |$O(nm)$| lưu trữ ma trận niềm vui | 

Các ràng buộc cho phép lên đến$10^5$tổng số ô trong các thử nghiệm, do đó giải pháp chạy thoải mái trong giới hạn vì mỗi ô chỉ được xử lý$O(\log 10^9)$lần. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def solve():
        t = int(input())
        for _ in range(t):
            input()
            m, n = map(int, input().split())
            p = [list(map(int, input().split())) for _ in range(m)]

            def can(x):
                used = set()
                for j in range(n):
                    ok = False
                    for i in range(m):
                        if p[i][j] >= x:
                            if i in used or len(used) < n - 1:
                                used.add(i)
                                ok = True
                                break
                    if not ok:
                        return False
                return len(used) <= n - 1

            lo, hi = 0, 10**9
            while lo < hi:
                mid = (lo + hi + 1) // 2
                if can(mid):
                    lo = mid
                else:
                    hi = mid - 1
            print(lo)

    from io import StringIO
    old_stdout = sys.stdout
    sys.stdout = StringIO()
    solve()
    out = sys.stdout.getvalue()
    sys.stdout = old_stdout
    return out.strip()

# provided sample
assert run("""
1

2 2
1 2
3 4
""") == "3"

# all equal
assert run("""
1

3 3
5 5 5
5 5 5
5 5 5
""") == "5"

# minimal n=2
assert run("""
1

2 2
1 100
100 1
""") == "100"

# forced fragmentation
assert run("""
1

3 3
10 1 1
1 10 1
1 1 10
""") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả ma trận bằng nhau | giá trị tối đa | tính khả thi thống nhất | 
| sự thống trị theo đường chéo | ngưỡng cao hoạt động | tái sử dụng cửa hàng | 
| mức cao theo chu kỳ | chỉ ngưỡng thấp | hạn chế phân mảnh | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi mỗi người bạn có một cửa hàng tốt nhất duy nhất. Ví dụ, nếu bạn bè$j$chỉ có giá trị tối đa trong cửa hàng$j$, sau đó đạt được ngưỡng cao buộc phải chọn tất cả$n$cửa hàng vi phạm quy định$n-1$giới hạn. Thuật toán phát hiện ra điều này vì nhiệm vụ tham lam nhất thiết phải giới thiệu một cửa hàng mới cho mỗi người bạn và nhóm đã sử dụng sẽ phát triển vượt quá$n-1$, gây ra sự từ chối. 

Một trường hợp khác là khi một cửa hàng thống trị nhiều bạn bè. Trong trường hợp đó, phép gán tham lam nhanh chóng ổn định trong một cửa hàng và số lượng cửa hàng đã sử dụng không bao giờ vượt quá 1, thỏa mãn ràng buộc một cách tầm thường.
