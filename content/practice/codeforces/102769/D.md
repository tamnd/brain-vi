---
title: "CF 102769D - Bảo Vệ Thành Phố"
description: "Thành phố là một vùng hình vuông từ (0,0) đến (n+1,n+1). Mỗi tháp phòng thủ nằm ở tọa độ nguyên bên trong hình vuông và bảo vệ một trong bốn góc phần tư kéo dài từ vị trí của nó."
date: "2026-07-30T04:26:53+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102769
codeforces_index: "D"
codeforces_contest_name: "2020 China Collegiate Programming Contest Qinhuangdao Site"
rating: 0
weight: 102769
solve_time_s: 96
verified: true
draft: false
---

[CF 102769D - Bảo vệ Thành phố](https://codeforces.com/problemset/problem/102769/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 36 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Thành phố là một khu vực hình vuông từ`(0,0)`ĐẾN`(n+1,n+1)`. Mỗi tháp phòng thủ nằm ở tọa độ nguyên bên trong hình vuông và bảo vệ một trong bốn góc phần tư kéo dài từ vị trí của nó. Tháp phía đông bắc bảo vệ mọi thứ ở bên phải và phía trên nó, tháp phía tây bắc bảo vệ mọi thứ ở bên trái và phía trên nó, tháp phía tây nam bảo vệ mọi thứ ở bên trái và bên dưới nó, và tháp phía đông nam bảo vệ mọi thứ ở bên phải và bên dưới nó. 

Nhiệm vụ là chọn nhóm tháp nhỏ nhất có thể có khu vực được bảo vệ bao phủ toàn bộ thành phố. Nếu không có tập hợp như vậy tồn tại thì câu trả lời là không thể. 

Tọa độ rất lớn, với tổng số tháp trong tất cả các trường hợp thử nghiệm lên tới vài triệu. Điều này ngay lập tức loại trừ các phương pháp kiểm tra tất cả các cặp tháp hoặc mô phỏng toàn bộ`n x n`thành phố. Thậm chí một`O(n log n)`Cách tiếp cận cần được quan tâm và giải pháp dự định phải tuyến tính cho mỗi trường hợp thử nghiệm. 

Nhận xét đầu tiên là bốn góc của thành phố buộc sự tồn tại của cả bốn hướng. Góc dưới bên trái chỉ được che bởi tháp hướng tây nam, góc trên bên trái chỉ được che bởi tháp hướng tây bắc, góc trên bên phải chỉ được che bởi tháp hướng đông bắc và góc dưới bên phải chỉ được che bởi tháp hướng đông nam. 

Việc triển khai đơn giản có thể thất bại nếu chỉ kiểm tra xem liệu tất cả bốn hướng có tồn tại hay không. Ví dụ:```
1
4
1 1 1
1 4 2
4 4 3
4 1 4
```Bốn góc được bốn tòa tháp bao bọc nhưng khu vực trung tâm thì không. Câu trả lời là không`4`bởi vì các góc phần tư được chọn để lại một hình chữ nhật lớn không được che chắn. 

Một cái bẫy khác là cho rằng lấy tháp gần nhất ở mọi hướng luôn là tối ưu. Ví dụ, hai tòa tháp cùng hướng có thể tạo thành ranh giới hình cầu thang. Việc loại bỏ một trong số chúng có thể làm lộ ra một khu vực mà không một tháp thay thế nào có thể che phủ được. 

Giải pháp cần phải tính đến các ranh giới cầu thang này hơn là các tòa tháp riêng lẻ. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ thử các tập hợp con của tháp. Điều này đúng vì bất kỳ câu trả lời hợp lệ nào cũng là tập hợp con của các tháp có sẵn, nhưng số lượng tập hợp con là theo cấp số nhân. Ngay cả việc hạn chế tìm kiếm trong sự kết hợp của bốn hoặc năm tòa tháp cũng không đủ vì câu trả lời có thể yêu cầu nhiều tòa tháp hơn trong những trường hợp được xây dựng đặc biệt. 

Thay vào đó, cấu trúc hữu ích đến từ việc xem xét vùng không được che chắn. Để có một hướng của bài toán, hãy xem xét tất cả các tòa tháp hướng Tây Nam. Sự kết hợp của họ tạo ra một ranh giới cầu thang. Chỉ có các góc của cầu thang này là quan trọng. Nếu một góc của cầu thang này đã được che bởi một hướng khác thì một trong những tháp phía Tây Nam góp phần tạo nên góc đó là không cần thiết. Nếu không thì mọi góc cầu thang còn lại phải được bảo vệ bởi một tháp hướng đông bắc. 

Quan sát này làm giảm vấn đề để tuân theo các trạng thái cầu thang có thể. Mỗi trạng thái được mô tả bằng một tọa độ biên duy nhất, cho phép quét tuyến tính với thông tin tiền tố và hậu tố. 

Việc thực hiện giải quyết một định hướng và sau đó phản ánh toàn bộ thành phố. Sự phản chiếu xử lý các trường hợp đối xứng trong đó cầu thang quan trọng nằm ở phía đối diện. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Lưu trữ các tháp theo hướng chuẩn hóa. Chỉ đường được chuyển đổi thành bốn trạng thái sao cho bước đầu tiên xử lý một cạnh của hình học. 
2. Xây dựng mảng tiền tố và hậu tố mô tả ranh giới tháp liên quan mạnh nhất. Ví dụ: đối với các tòa tháp phía tây nam, chúng tôi giữ mức thấp nhất có thể tiếp cận`y`giá trị cho mọi`x`, trong khi đối với các tháp phía tây bắc và đông bắc, chúng tôi duy trì khả năng tiếp cận tối đa`x`các giá trị. 
3. Nén cầu thang hướng Tây Nam vào các trạng thái chuyển tiếp có thể. Quá trình chuyển đổi thể hiện việc di chuyển từ góc cầu thang này sang góc cầu thang khác và ghi lại số lượng tháp bổ sung tối thiểu cần thiết. 
4. Quét qua các vị trí cầu thang có thể. Quá trình quét duy trì câu trả lời tốt nhất cho từng ranh giới hiện hoạt, bởi vì các lựa chọn trong tương lai chỉ phụ thuộc vào một tọa độ của ranh giới hiện tại. 
5. Kiểm tra tất cả các điểm cuối có thể có của cầu thang cuối cùng và cập nhật số lượng tháp tối thiểu. 
6. Suy ngẫm tọa độ và hướng rồi lặp lại phép tính tương tự. Sự phản chiếu bao gồm trường hợp đối xứng không được biểu thị bằng hướng đầu tiên. 
7. Nếu không có hướng nào tìm thấy cấu trúc hợp lệ, xuất ra`Impossible`. 

Tại sao nó hoạt động: 

Bất biến quan trọng là sau khi xử lý ranh giới cầu thang, mọi giải pháp từng phần tối ưu có thể được biểu thị bằng một trạng thái được lưu trữ. Bất kỳ tòa tháp nào không được sử dụng trên cầu thang chỉ có thể quan trọng qua góc nơi nó thay đổi ranh giới. Việc quét là cách rẻ nhất để tiếp cận mọi góc như vậy, vì vậy khi tìm thấy cấu hình che phủ hoàn chỉnh, số lượng tháp là tối thiểu. Bước phản chiếu xử lý tính đối xứng duy nhất còn lại, do đó không bỏ sót trường hợp hình học nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

INF = 10 ** 9

def calc(n, x, y, d):
    mny = [INF] * (n + 2)
    Mxx = [0] * (n + 2)
    mxx = [0] * (n + 2)
    mxy = [0] * (n + 2)

    for i in range(n):
        if d[i] == 0:
            mny[x[i]] = min(mny[x[i]], y[i])
        elif d[i] == 1:
            Mxx[y[i]] = max(Mxx[y[i]], x[i])
        elif d[i] == 2:
            mxx[y[i]] = max(mxx[y[i]], x[i])
        else:
            mxy[x[i]] = max(mxy[x[i]], y[i])

    for i in range(2, n + 1):
        mny[i] = min(mny[i - 1], mny[i])
        Mxx[i] = max(Mxx[i - 1], Mxx[i])
        mxy[i] = max(mxy[i - 1], mxy[i])

    for i in range(n - 1, 0, -1):
        mxx[i] = max(mxx[i + 1], mxx[i])

    fx = [INF] * (n + 2)
    fy = [INF] * (n + 2)

    for i in range(n):
        if d[i] == 1:
            x0 = x[i]
            y0 = mny[min(x0, Mxx[y[i]])]
            if y0 > y[i]:
                continue
            if y0 <= mxy[x0]:
                return 4
            fx[x0] = min(fx[x0], 3 + (y0 > mny[x0]))
            fy[y0] = min(fy[y0], 3 + (x0 < mxx[y0]))

    ans = INF
    yy = n

    for xx in range(1, n + 1):
        while yy > mny[xx]:
            if mxx[yy]:
                fx[mxx[yy]] = min(
                    fx[mxx[yy]],
                    fy[yy] + (yy > mny[mxx[yy]])
                )
            yy -= 1
        if mny[xx] != INF:
            fy[mny[xx]] = min(
                fy[mny[xx]],
                fx[xx] + (xx < mxx[mny[xx]])
            )

    while yy:
        if mxx[yy]:
            fx[mxx[yy]] = min(
                fx[mxx[yy]],
                fy[yy] + (yy > mny[mxx[yy]])
            )
        yy -= 1

    for xx in range(1, n + 1):
        if mny[xx] <= mxy[xx]:
            ans = min(ans, fx[xx] + 1)

    for yy in range(1, n + 1):
        if mxx[yy] and yy <= mxy[mxx[yy]]:
            ans = min(ans, fy[yy] + 1)

    return ans

def solve_case(n, towers):
    x = []
    y = []
    d = []

    for a, b, c in towers:
        x.append(a)
        y.append(b)
        d.append(c - 1)

    ans = calc(n, x, y, d)

    for i in range(n):
        y[i] = n - y[i] + 1
        d[i] ^= 3

    ans = min(ans, calc(n, x, y, d))

    return "Impossible" if ans > n else str(ans)

def main():
    t = int(input())
    out = []

    for case in range(1, t + 1):
        n = int(input())
        towers = []
        for _ in range(n):
            x, y, d = map(int, input().split())
            towers.append((x, y, d))
        out.append(f"Case #{case}: {solve_case(n, towers)}")

    print("\n".join(out))

if __name__ == "__main__":
    main()
```các`calc`chức năng là quét lõi. Các mảng`mny`,`mxy`,`mxx`, Và`Mxx`lưu trữ ranh giới cầu thang tốt nhất có thể. Việc truyền tiền tố và hậu tố làm cho mọi truy vấn về một phạm vi tháp có thời gian không đổi. 

Hai mảng`fx`Và`fy`đại diện cho phần kết thúc xây dựng một phần rẻ nhất tại tọa độ cầu thang cụ thể. Khi quá trình quét vượt qua một ranh giới, trạng thái có thể được chuyển sang ranh giới tiếp theo với chi phí bổ sung cho tháp cần thiết. 

Cuộc gọi thứ hai tới`calc`thay đổi`y`vào trong`n-y+1`và lật hướng bằng XOR`3`. Đây là một cách nhỏ gọn để phản chiếu hình vuông theo chiều dọc và sử dụng lại lý luận hình học tương tự. 

Không có hoạt động dấu phẩy động. Tọa độ là các vị trí lưới số nguyên và tất cả các bộ đếm được giới hạn bởi`n`, vì vậy số nguyên Python bình thường là đủ. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
2
3
1 1 1
2 2 2
3 3 3
4
1 1 1
2 2 3
2 1 2
1 2 4
```Thử nghiệm đầu tiên chỉ có ba tòa tháp. 

| Bước | Chỉ đường tích cực | Kết quả | 
| --- | --- | --- | 
| Đọc tháp | ĐB, Tây Bắc, Tây Nam | Thiếu SE | 
| Kiểm tra hướng đầu tiên | Không thể che góc dưới bên phải | Không thể | 
| Kiểm tra phản ánh | Vẫn thiếu hướng đi cần thiết | Không thể | 

Bài kiểm tra thứ hai có tất cả bốn hướng. 

| Bước | Chỉ đường tích cực | Kết quả | 
| --- | --- | --- | 
| Xây dựng cầu thang | Mọi ranh giới đều tồn tại | Tiếp tục | 
| Trạng thái quét | Mỗi góc không được che chắn đều nhận được một tòa tháp | 4 | 
| Phản ánh | Không có giải pháp nào tốt hơn | Giữ 4 | 

Những dấu vết này chứng minh tại sao bốn hướng lại cần thiết và tại sao bốn tòa tháp vẫn có thể là mức tối thiểu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi tháp và mỗi tọa độ được xử lý một số lần không đổi trong cả hai lần | 
| Không gian | O(n) | Thuật toán lưu trữ một số mảng được lập chỉ mục theo tọa độ | 

Tổng số tháp trong tất cả các trường hợp thử nghiệm là có hạn, do đó giải pháp tuyến tính vừa vặn thoải mái trong giới hạn bộ nhớ và thời gian. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    data = sys.stdin.read().split()
    sys.stdin = old

    it = iter(data)
    t = int(next(it))
    out = []

    for case in range(1, t + 1):
        n = int(next(it))
        towers = []
        for _ in range(n):
            towers.append((int(next(it)), int(next(it)), int(next(it))))
        out.append(f"Case #{case}: {solve_case(n, towers)}")

    return "\n".join(out)

assert run("""2
3
1 1 1
2 2 2
3 3 3
4
1 1 1
2 2 3
2 1 2
1 2 4
""") == """Case #1: Impossible
Case #2: 4"""

assert run("""1
1
1 1 1
""") == "Case #1: Impossible"

assert run("""1
4
2 2 1
2 2 2
2 2 3
2 2 4
""") == "Case #1: 4"

assert run("""1
8
1 8 1
2 8 1
8 8 2
8 7 2
8 1 3
7 1 3
1 1 4
1 2 4
""") == "Case #1: 4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Một tòa tháp | Không thể | Thiếu hướng ba góc | 
| Bốn tòa tháp tại một điểm | 4 | Thi công che phủ tối thiểu có thể | 
| Nhiều tòa tháp tạo thành ranh giới bên ngoài | 4 | Tháp dự phòng bị bỏ qua | 

## Vỏ cạnh 

Một thành phố có đúng một tòa tháp không thể hoạt động được vì một góc phần tư không thể bao phủ hết bốn góc. Thuật toán nắm bắt được điều này vì các trạng thái cầu thang được yêu cầu không bao giờ hoàn chỉnh. 

Trường hợp có cả bốn hướng nhưng tháp đặt xa nhau cũng được xử lý chính xác. Ví dụ, bốn tòa tháp ở các góc của hình vuông đều để lại một lỗ ở giữa. Quá trình quét phát hiện ra rằng cần có thêm tháp hoặc các lựa chọn khác thay vì cho rằng bốn hướng có nghĩa là bốn tháp là đủ. 

Bố cục đối xứng có thể xuất hiện hợp lệ từ một hướng nhưng không thành công trong lần quét đầu tiên. Đường chuyền thứ hai được phản ánh sẽ kiểm tra hướng cầu thang đối diện và ngăn ngừa việc bỏ sót những trường hợp như vậy.
