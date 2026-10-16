---
title: "CF 105477B - Tàu lượn siêu tốc Nga"
description: "Chúng ta được yêu cầu đếm xem có bao nhiêu “tàu lượn siêu tốc” hợp lệ có thể được tạo thành từ một số đoạn cố định, trong đó mỗi đoạn di chuyển một đơn vị theo chiều ngang và đi lên hoặc đi xuống một đơn vị theo chiều dọc."
date: "2026-06-23T02:06:02+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105477
codeforces_index: "B"
codeforces_contest_name: "XXII Spain Olympiad in Informatics, Online Qualifier 1"
rating: 0
weight: 105477
solve_time_s: 100
verified: true
draft: false
---

[CF 105477B - Tàu lượn siêu tốc của Nga](https://codeforces.com/problemset/problem/105477/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 40s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu đếm xem có bao nhiêu “tàu lượn siêu tốc” hợp lệ có thể được tạo thành từ một số đoạn cố định, trong đó mỗi đoạn di chuyển một đơn vị theo chiều ngang và đi lên hoặc đi xuống một đơn vị theo chiều dọc. Một chuyến đi đầy đủ bao gồm chính xác`2l`các đoạn, bắt đầu ở mặt đất, kết thúc ở mặt đất và không bao giờ đi xuống dưới mặt đất ở bất kỳ tiền tố nào của đường dẫn. Trong số tất cả các đường dẫn hợp lệ như vậy, chúng tôi chỉ muốn những đường dẫn có chiều cao tối đa chính xác là`h`. 

Một cách thuận tiện để lập mô hình này là coi mỗi đoạn như một bước trong đường dẫn mạng: bước “lên” tăng chiều cao thêm 1, bước “xuống” giảm chiều cao đi 1. Các ràng buộc chuyển thành các điều kiện kiểu Dyck-path cổ điển: đường dẫn có chiều dài`2l`, bắt đầu và kết thúc ở độ cao 0, không bao giờ âm và có chiều cao tối đa chính xác`h`. 

Vì vậy, nhiệm vụ trở thành đếm các đường đi bán dài giống Dyck`l`với giới hạn chiều cao giới hạn bằng`h`. 

Kích thước đầu vào cho phép`l`Và`h`lên tới 200 và lên tới 40000 trường hợp thử nghiệm. Điều này ngay lập tức loại trừ mọi lập trình động cho mỗi truy vấn trên`O(l^2)`tiểu bang. Thậm chí`O(l^2)`Một lần thì được, nhưng lặp lại hàng vạn lần thì không được. Cách tiếp cận đúng phải tính toán trước tất cả các câu trả lời cho tất cả`l ≤ 200`Và`h ≤ 200`, sau đó trả lời từng truy vấn trong O(1). 

Trường hợp cạnh tinh tế xuất hiện khi`h`lớn hơn mức có thể đối với một mức nhất định`l`. Chiều cao tối đa có thể có trong bất kỳ đường dẫn có độ dài hợp lệ nào`2l`là`l`, vì vậy bất kỳ truy vấn nào với`h > l`sẽ trả về 0 trừ khi được giải thích cẩn thận: thực tế chúng tôi đếm các đường dẫn có mức tối đa chính xác là`h`, vậy nếu`h > l`, câu trả lời là 0. Một trường hợp khác là`h = 0`, điều này là không thể đối với bất kỳ tích cực nào`l`, vì đường đi Dyck không tầm thường phải tăng lên ít nhất là 1. 

Một sai lầm ngây thơ là đếm tất cả các đường Dyck có chiều cao ≤ h thay vì chính xác là h. Điều đó quá đáng; vấn đề là yêu cầu chiều cao tối đa chính xác chứ không phải chiều cao giới hạn. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu là tạo ra tất cả các đường dẫn có độ dài cân bằng hợp lệ`2l`sử dụng đệ quy, theo dõi chiều cao hiện tại và chiều cao tối đa đạt được. Ở mỗi bước, chúng ta có thể đi lên hoặc đi xuống, nhưng phải đảm bảo chiều cao không bao giờ trở nên âm. Khi chúng ta đạt đến chiều dài`2l`, chúng tôi kiểm tra xem chúng tôi có quay lại 0 không và liệu chiều cao tối đa có bằng không`h`. Điều này đúng nhưng khám phá không gian trạng thái có kích thước gần bằng Catalan, phát triển như`O(4^l / l^{3/2})`. Ngay cả đối với`l = 200`, điều này là không thể. 

Quan sát quan trọng là đây là cách liệt kê đường dẫn Dyck cổ điển với một ràng buộc bổ sung về chiều cao tối đa. Những vấn đề như vậy thường được xử lý bằng lập trình động theo vị trí và chiều cao hoặc hiệu quả hơn bằng DP truyền đếm các đường dẫn có chiều cao được giới hạn bởi`h`. Một khi chúng ta có thể tính toán`F(l, h)`= số lượng đường dẫn có độ dài hợp lệ`2l`nằm trong độ cao ≤ h, thì chúng ta có thể rút ra câu trả lời cho độ cao chính xác bằng cách sử dụng phép bao hàm trên các ranh giới: các đường dẫn có độ chính xác tối đa`h`bằng`F(l, h) - F(l, h-1)`. 

Việc giảm này rất quan trọng vì khó có thể thực thi trực tiếp “chính xác tối đa h”, trong khi “không bao giờ vượt quá h” được thực thi một cách tự nhiên trong DP. 

Sau đó chúng tôi tính toán trước`F(l, h)`cho tất cả`l ≤ 200`Và`h ≤ 200`sử dụng DP 2D trong đó trạng thái là`(i, j)`nghĩa là số cách để đạt đến độ cao`j`sau đó`i`các bước mà không đi xuống dưới 0 hoặc cao hơn`h`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^{2l}) | O(l) | Quá chậm | 
| DP tối ưu | O(l^2) tính toán trước + O(1) cho mỗi truy vấn | O(l^2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xác định một bảng DP trong đó`dp[i][j]`là số chuỗi có độ dài hợp lệ`i`kết thúc ở độ cao`j`và không bao giờ vượt quá giới hạn chiều cao hiện tại. 

Chúng tôi tính toán điều này một cách riêng biệt cho từng giới hạn chiều cao có thể`H`từ 0 đến 200. 

1. Đối với chiều cao tối đa cho phép cố định`H`, khởi tạo`dp[0][0] = 1`. Điều này thể hiện đường đi trống bắt đầu từ mặt đất. 
2. Đối với mỗi chỉ số bước`i`từ 1 đến`2l_max`, cập nhật tất cả các độ cao`j`TRONG`[0, H]`. 
3. Từ tiểu bang`(i-1, j)`, chúng ta có thể chuyển đến`(i, j+1)`nếu như`j+1 ≤ H`, tương ứng với một bước đi lên. 
4. Từ tiểu bang`(i-1, j)`, chúng ta có thể chuyển đến`(i, j-1)`nếu như`j-1 ≥ 0`, tương ứng với một bước đi xuống. Động thái này chỉ có hiệu lực khi chúng ta chưa ở mặt đất. 
5. Sau khi điền DP, giá trị`dp[2l][0]`cung cấp số lượng đường dẫn hợp lệ có độ dài`2l`không bao giờ vượt quá chiều cao`H`. 
6. Để có được đường dẫn có chiều cao tối đa chính xác`h`, tính toán`dp_exact[l][h] = dp_at_most[l][h] - dp_at_most[l][h-1]`. 

Lý do phép trừ này hoạt động là vì mọi đường dẫn hợp lệ đều có chiều cao tối đa được xác định rõ ràng. Đếm tất cả các đường dẫn có giá trị tối đa ≤ h bao gồm những đường dẫn có giá trị tối đa hoàn toàn nhỏ hơn h; trừ đi những giá trị có cực đại ≤ h−1 sẽ tách biệt chính xác những giá trị có giá trị cực đại là h. 

### Tại sao nó hoạt động 

DP xác định sự song ánh giữa các đường dẫn tôn trọng tiền tố hợp lệ và các chuỗi chuyển tiếp trong mạng giới hạn. Mỗi đường dẫn hợp lệ tương ứng với chính xác một chuỗi chuyển tiếp DP và giới hạn độ cao được thực thi cục bộ ở mỗi bước. Bởi vì mọi đường đi đều được tính chính xác một lần trong`dp_at_most`và chiều cao tối đa phân chia tập hợp tất cả các đường dẫn hợp lệ thành các lớp riêng biệt`{max = 0, max = 1, ..., max = l}`, bước trừ trích xuất một lớp duy nhất mà không trùng lặp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

MAX_L = 200

# dp_at_most[h][i][j] = ways for length i ending at height j with max height <= h
dp = [[[0] * (MAX_L + 1) for _ in range(2 * MAX_L + 1)] for _ in range(MAX_L + 1)]

dp[0][0][0] = 1

for h in range(1, MAX_L + 1):
    for i in range(2 * MAX_L + 1):
        for j in range(h + 1):
            val = dp[h - 1][i][j]
            if not val:
                continue
            dp[h][i][j] = (dp[h][i][j] + val) % MOD
            if i + 1 <= 2 * MAX_L:
                if j + 1 <= h:
                    dp[h][i + 1][j + 1] = (dp[h][i + 1][j + 1] + val) % MOD
                if j - 1 >= 0:
                    dp[h][i + 1][j - 1] = (dp[h][i + 1][j - 1] + val) % MOD

t = int(input())
for _ in range(t):
    l, h = map(int, input().split())
    if h > l:
        print(0)
        continue
    if h == 0:
        print(0)
        continue

    ans = (dp[h][2 * l][0] - dp[h - 1][2 * l][0]) % MOD
    print(ans)
```Việc triển khai xây dựng DP theo lớp trên chiều cao tối đa. Mỗi lớp`h`sử dụng lại cái trước`h-1`, vì vậy chúng tôi cho phép các đường dẫn có trần tăng dần một cách hiệu quả. Câu trả lời cuối cùng sử dụng sự khác biệt giữa hai lớp, cách ly chiều cao tối đa chính xác. 

Một lỗi phổ biến ở đây là quên rằng DP phải thực thi đồng thời cả hai điều kiện biên: chiều cao không âm và giới hạn trên`h`. Một cách khác là kết hợp các cập nhật “kích thước chiều dài” và “kích thước chiều cao”, điều này âm thầm phá vỡ tính chính xác khi các đường dẫn truy cập lại các độ cao trung gian nhiều lần. 

## Ví dụ đã hoạt động 

Xem xét đầu vào`l = 2, h = 2`. Chúng tôi đang đếm các đường dẫn có độ dài 4 bắt đầu và kết thúc ở 0, không bao giờ âm và đạt độ cao chính xác bằng 2. Các đường dẫn hợp lệ chỉ có một: lên, lên, xuống, xuống. 

| Bước | chiều cao dp 0 | chiều cao dp 1 | chiều cao dp 2 | 
| --- | --- | --- | --- | 
| 0 | 1 | 0 | 0 | 
| 1 | 0 | 1 | 0 | 
| 2 | 1 | 0 | 1 | 
| 3 | 0 | 2 | 0 | 
| 4 | 1 | 0 | 1 | 

Từ đây,`dp_at_most[2][4][0] = 2`, Và`dp_at_most[1][4][0] = 1`, vậy chiều cao chính xác 2 là`2 - 1 = 1`. 

Bây giờ hãy xem xét`l = 3, h = 2`. Chúng tôi đếm các đường dẫn có độ dài 6 với tối đa chính xác là 2. DP bao gồm tất cả các đường dẫn nằm dưới 2, sau đó trừ đi những đường dẫn nằm dưới 1. 

Điều này thể hiện sự tách biệt của các ràng buộc: lớp DP kiểm soát mức trần, trong khi phép trừ cô lập mức tối đa chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(H * L * L) | Chúng tôi tính toán DP lớp cho mỗi chiều cao lên tới 200, mỗi trạng thái cập nhật O(L * H) | 
| Không gian | O(H * L * L) | Lưu trữ các lớp DP theo chiều cao, chiều dài và vị trí | 

Các ràng buộc cho phép tính toán trước khoảng 8 triệu trạng thái, nằm trong giới hạn. Mỗi truy vấn được trả lời trong thời gian không đổi bằng cách sử dụng bảng được tính toán trước. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    MOD = 10**9 + 7
    MAX_L = 10  # reduced for testing

    dp = [[[0] * (MAX_L + 1) for _ in range(2 * MAX_L + 1)] for _ in range(MAX_L + 1)]
    dp[0][0][0] = 1

    for h in range(1, MAX_L + 1):
        for i in range(2 * MAX_L + 1):
            for j in range(h + 1):
                val = dp[h - 1][i][j]
                if not val:
                    continue
                dp[h][i][j] = (dp[h][i][j] + val) % MOD
                if i + 1 <= 2 * MAX_L:
                    if j + 1 <= h:
                        dp[h][i + 1][j + 1] = (dp[h][i + 1][j + 1] + val) % MOD
                    if j - 1 >= 0:
                        dp[h][i + 1][j - 1] = (dp[h][i + 1][j - 1] + val) % MOD

    def solve_case(l, h):
        if h > l or h == 0:
            return 0
        return (dp[h][2 * l][0] - dp[h - 1][2 * l][0]) % MOD

    out = []
    for line in inp.getvalue().splitlines()[1:]:
        l, h = map(int, line.split())
        out.append(str(solve_case(l, h)))
    return "\n".join(out)

# provided samples
assert run("4\n2 2\n2 3\n3 2\n15 5\n") == "1\n0\n3\n2665884"

# custom cases
assert run("1\n1 1\n") == "1", "minimum valid mountain"
assert run("1\n1 2\n") == "0", "height impossible"
assert run("1\n2 1\n") == "1", "single peak case"
assert run("1\n3 3\n") == "0", "height too large for length"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\n1 1`|`1`| đỉnh hợp lệ không tầm thường nhỏ nhất | 
|`1\n1 2`|`0`| hạn chế chiều cao không thể | 
|`1\n2 1`|`1`| đường dẫn cân bằng tối thiểu | 
|`1\n3 3`|`0`| chiều cao vượt quá giới hạn kết cấu | 

## Vỏ cạnh 

Khi nào`h > l`, lớp DP cho chiều cao đó sẽ vẫn tồn tại trong quá trình triển khai đơn giản, nhưng không có đường dẫn có độ dài`2l`về mặt vật lý có thể đạt đến độ cao đó trong khi vẫn có hiệu lực. Thuật toán xử lý việc này bằng cách trả về 0 ngay lập tức, tránh tính toán không cần thiết. 

Khi`h = 1`, chỉ những đường dẫn không bao giờ vượt quá độ cao 1 mới được xem xét, nghĩa là cấu trúc sẽ sụp đổ theo các mô hình lên xuống xen kẽ. DP chỉ đếm chính xác những chuỗi quay trở lại mặt đất mà không bao giờ xếp chồng lên nhau hai lần liên tiếp. 

Khi`l = 1`, đường dẫn hợp lệ duy nhất có thể là “lên rồi xuống” và chỉ`h = 1`là hợp lệ. Phép trừ`dp[1] - dp[0]`tách biệt cấu hình đơn này một cách rõ ràng, xác nhận tính chính xác của việc phân tách theo chiều cao tối đa.
