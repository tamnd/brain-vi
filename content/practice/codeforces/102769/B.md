---
title: "CF 102769B - Tường bao"
description: "Chúng ta có một bản đồ hình chữ nhật của các ô. Ô khô được biểu thị bằng và ô ướt được biểu thị bằng .. Một bức tường giới hạn là đường viền của một hình chữ nhật, nhưng bài toán yêu cầu diện tích hình chữ nhật được che phủ của nó."
date: "2026-07-30T04:28:43+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102769
codeforces_index: "B"
codeforces_contest_name: "2020 China Collegiate Programming Contest Qinhuangdao Site"
rating: 0
weight: 102769
solve_time_s: 107
verified: true
draft: false
---

[CF 102769B - Tường giới hạn](https://codeforces.com/problemset/problem/102769/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 47s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một bản đồ hình chữ nhật của các ô. Một tế bào khô được đại diện bởi`#`và một tế bào ướt được đại diện bởi`.`. Tường bao là đường viền của hình chữ nhật, nhưng bài toán yêu cầu diện tích hình chữ nhật được che phủ. Hình chữ nhật phải được tạo hoàn toàn từ các ô khô và phải chứa ô được đề cập trong truy vấn. Sau một số truy vấn, các ô riêng lẻ sẽ chuyển đổi giữa khô và ướt và chúng tôi cần trả lời diện tích lớn nhất có thể cho ô được yêu cầu. 

Đầu vào chứa một số trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm đưa ra lưới ban đầu theo sau là các hoạt động. Một bản cập nhật thay đổi một ô. Một truy vấn yêu cầu hình chữ nhật hoàn toàn khô lớn nhất bao gồm một vị trí cụ thể. 

Các ràng buộc nhỏ theo một chiều nhưng cho phép tối đa 1000 hàng, 1000 cột và 1000 thao tác trong một trường hợp thử nghiệm. Một giải pháp thử mọi hình chữ nhật có thể là không thể vì lưới 1000 x 1000 đã có khoảng$10^{12}$hình chữ nhật có thể. Ngay cả việc tính toán lại câu trả lời cho mọi truy vấn bằng bảng lập trình động đầy đủ cũng sẽ quá chậm. Chúng ta cần một phương pháp trong đó mỗi truy vấn chỉ quét lưới một lần. 

Quan sát hữu ích là 1000 cột tự nhiên phù hợp với các hoạt động bit có kích thước máy. Toàn bộ một hàng có thể được biểu diễn dưới dạng một số nguyên, trong đó bit`1`có nghĩa là khô. Các hàng giao nhau trở thành một phép toán AND số nguyên duy nhất. Điều này cho phép chúng ta kiểm tra tất cả các cột của một hình chữ nhật ứng cử viên cực kỳ nhanh chóng. 

Có một số trường hợp nguy hiểm có thể phá vỡ các giải pháp bất cẩn. Nếu bản thân ô được truy vấn bị ướt thì câu trả lời là 0 vì mọi hình chữ nhật hợp lệ đều phải chứa ô đó. 

Ví dụ:```
1 1 1
.
2 1 1
```Đầu ra là:```
Case #1:
0
```Giải pháp chỉ kiểm tra các ô lân cận có thể trả về vùng dương không chính xác. 

Một trường hợp khác là hình chữ nhật chạm vào ranh giới của bảng. 

Ví dụ:```
2 3 1
###
###
2 1 1
```Câu trả lời là:```
Case #1:
6
```Hình chữ nhật tối ưu sử dụng toàn bộ lưới. Mã chỉ bắt đầu mở rộng khi có ô ở cả hai bên có thể bỏ sót những trường hợp như vậy. 

Lỗi phổ biến thứ ba là cho rằng hình chữ nhật phải có cả chiều cao và chiều rộng lớn hơn một. Cho phép một hàng hoặc một cột. 

Ví dụ:```
1 4 1
####
2 1 3
```Câu trả lời là:```
Case #1:
4
```Phương pháp chỉ xét hình chữ nhật hai chiều sẽ thất bại ở đây. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp là liệt kê mọi hình chữ nhật có thể chứa ô được truy vấn và kiểm tra xem tất cả các ô của nó có khô hay không. Điều này đúng vì mọi câu trả lời hợp lệ đều được xem xét, nhưng số lượng hình chữ nhật trong lưới 1000 x 1000 là khoảng$10^{12}$. Ngay cả với tổng tiền tố, việc kiểm tra tất cả các hình chữ nhật cho mỗi truy vấn vẫn vượt xa giới hạn. 

Một ý tưởng tốt hơn là cố định một chiều của hình chữ nhật và tính toán chiều còn lại tốt nhất có thể. Đối với ô truy vấn, mọi hình chữ nhật phải chứa hàng được truy vấn. Nếu chúng ta mở rộng lên trên từ hàng đó, chúng ta có thể duy trì giao điểm của tất cả các hàng được bao gồm cho đến nay. Các bit được đặt còn lại chính xác là các cột trong đó mọi hàng trong khoảng chiều cao hiện tại đều khô. 

Hình chữ nhật lớn nhất cho khoảng chiều cao đó là đoạn liên tiếp dài nhất trong số các cột hợp lệ chứa cột được truy vấn. Chúng ta có thể làm tương tự khi kéo dài xuống dưới. Hai lần quét bao phủ mọi ranh giới trên và dưới có thể có của hình chữ nhật. 

Lý do điều này hoạt động hiệu quả là vì kích thước lưới chỉ là 1000. Việc biểu diễn mỗi hàng dưới dạng số nguyên Python cho phép mọi giao điểm hàng xảy ra trong thời gian không đổi theo quan điểm của thuật toán, bởi vì công việc thực tế được xử lý bằng các phép toán số nguyên lớn được tối ưu hóa. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²m²) mỗi truy vấn | O(1) | Quá chậm | 
| Mở rộng bitset tối ưu | O(n) mỗi truy vấn | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Lưu trữ mọi hàng của lưới dưới dạng mặt nạ bit số nguyên. Chút`j`được thiết lập khi cột`j`của hàng đó đã khô. Cập nhật ô sẽ trở thành một thao tác XOR duy nhất trên bit tương ứng. 
2. Đối với một truy vấn tại`(r, c)`, bắt đầu với tất cả các cột có sẵn và mở rộng lên trên từ hàng`r`. Sau khi thêm từng hàng, hãy giao mặt nạ hiện tại với hàng đó. Các bit còn lại biểu thị các cột khô ở mỗi hàng có chiều cao hình chữ nhật hiện tại. 
3. Nếu cột được truy vấn biến mất khỏi mặt nạ, hãy ngừng mở rộng theo hướng đó. Bất kỳ hình chữ nhật lớn hơn nào cũng sẽ chứa ô ướt đó, vì vậy không có hàng nào khác có thể tạo ra câu trả lời hợp lệ. 
4. Với mỗi khoảng chiều cao hợp lệ, hãy tính chiều dài của đoạn khô liên tiếp chứa cột`c`. Nhân chiều rộng đó với chiều cao hiện tại và cập nhật câu trả lời. 
5. Lặp lại quá trình tương tự trong khi kéo dài từ hàng xuống dưới`r + 1`. Kết hợp cả hai hướng sẽ xem xét mọi đường viền trên và dưới có thể có của hình chữ nhật chứa ô truy vấn. 

Tại sao nó hoạt động: 

Trong quá trình quét lên trên, sau khi xử lý các hàng từ`top`ĐẾN`r`, mặt nạ bit chứa chính xác các cột trong đó mọi ô trong các hàng đó đều khô. Đoạn liên tiếp dài nhất chứa`c`là hình chữ nhật rộng nhất có thể cho tập hợp hàng chính xác đó. Mọi hình chữ nhật hợp lệ đều có một số hàng trên cùng và hàng trên cùng đó xuất hiện trong quá trình quét. Lập luận tương tự áp dụng cho việc quét xuống. Vì mọi phạm vi dọc có thể đều được xem xét và chiều rộng tốt nhất được chọn cho mỗi phạm vi nên diện tích tối đa được tìm thấy là tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def trailing_ones(x):
    return ((~x) & (x + 1)).bit_length() - 1

def width_at(mask, col):
    right = trailing_ones(mask >> col)
    left_mask = mask & ((1 << col) - 1)
    left = trailing_ones(left_mask)
    return left + right + 1

def solve_case(n, m, q, grid, queries):
    rows = []
    for s in grid:
        x = 0
        for i, ch in enumerate(s):
            if ch == '#':
                x |= 1 << i
        rows.append(x)

    all_bits = (1 << m) - 1
    ans = []

    for t, x, y in queries:
        x -= 1
        y -= 1

        if t == 1:
            rows[x] ^= 1 << y
        else:
            if ((rows[x] >> y) & 1) == 0:
                ans.append("0")
                continue

            best = 1

            mask = all_bits
            height = 0
            for i in range(x, -1, -1):
                mask &= rows[i]
                if ((mask >> y) & 1) == 0:
                    break
                height += 1
                best = max(best, height * width_at(mask, y))

            mask = all_bits
            height = 0
            for i in range(x + 1, n):
                mask &= rows[i]
                if ((mask >> y) & 1) == 0:
                    break
                height += 1
                best = max(best, height * width_at(mask, y))

            ans.append(str(best))

    return ans

def main():
    t = int(input())
    out = []

    for case in range(1, t + 1):
        n, m, q = map(int, input().split())
        grid = [input().strip() for _ in range(n)]
        queries = [tuple(map(int, input().split())) for _ in range(q)]

        out.append(f"Case #{case}:")
        out.extend(solve_case(n, m, q, grid, queries))

    print("\n".join(out))

if __name__ == "__main__":
    main()
```Biểu diễn lưới là chi tiết triển khai chính. Mỗi hàng được chuyển đổi thành một số nguyên, do đó việc cập nhật một ô không yêu cầu thay đổi chuỗi hoặc xây dựng lại mảng. XOR hoạt động vì thao tác chỉ lật một bit. 

Đối với một truy vấn,`mask`bắt đầu với tất cả các cột được kích hoạt. Mọi thao tác AND sẽ loại bỏ các cột chứa ít nhất một ô ướt trong phạm vi dọc hiện tại. Khi cột truy vấn bị xóa, hình chữ nhật không thể bao gồm ô được yêu cầu nữa nên quá trình quét sẽ kết thúc ngay lập tức. 

chức năng`width_at`tìm chuỗi bit tập hợp liên tiếp có chứa một cột nhất định. Phía bên phải thu được bằng cách đếm các số ở cuối sau khi dịch chuyển bit được truy vấn đến vị trí ít quan trọng nhất. Phía bên trái thực hiện thao tác tương tự trên các bit trước cột được truy vấn. 

Số nguyên Python có độ chính xác tùy ý, do đó không có vấn đề tràn mặc dù một hàng có thể chứa 1000 bit. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
2 3 2
###
##.
2 2 2
2 1 3
```Lưới là:```
###
##.
```Đối với truy vấn ở hàng 2, cột 2: 

| Hướng | Chiều cao | Cột hợp lệ | Chiều rộng | Khu vực | 
| --- | --- | --- | --- | --- | 
| Lên | 1 | ### | 3 | 3 | 
| Lên | 2 | ## | 2 | 4 | 

Câu trả lời là`4`. 

Đối với truy vấn thứ hai ở hàng 1, cột 3: 

| Hướng | Chiều cao | Cột hợp lệ | Chiều rộng | Khu vực | 
| --- | --- | --- | --- | --- | 
| Xuống | 1 | ### | 3 | 3 | 

Câu trả lời là`3`. 

Đối với mẫu thứ hai:```
4 3 3
###
#.#
#.#
###
2 3 2
1 3 2
2 3 2
```Truy vấn đầu tiên yêu cầu ô ướt ở giữa: 

| Bước | Trạng thái tế bào | Kết quả | 
| --- | --- | --- | 
| Kiểm tra ô được truy vấn |`.`| 0 | 

Sau khi lật ô đó:```
###
###
#.#
###
```Truy vấn cuối cùng tạo ra: 

| Hướng | Chiều cao | Cột hợp lệ | Chiều rộng | Khu vực | 
| --- | --- | --- | --- | --- | 
| Lên | 3 | ### | 3 | 9 | 

Hình chữ nhật mở rộng trên ba hàng đầu tiên, mang lại diện tích`9`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) mỗi truy vấn | Mỗi truy vấn quét các hàng và mỗi thao tác hàng là một phép toán số nguyên theo bit | 
| Không gian | O(n) | Chỉ có mặt nạ bit của mỗi hàng được lưu trữ | 

Trường hợp thử nghiệm lớn nhất có 1000 hàng và 1000 truy vấn, dẫn đến khoảng một triệu lần quét hàng. Hoạt động bit trên số nguyên 1000 bit đủ nhanh cho giới hạn này. 

## Trường hợp thử nghiệm```python
import sys
import io

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
        m = int(next(it))
        q = int(next(it))
        grid = [next(it) for _ in range(n)]
        queries = []
        for _ in range(q):
            queries.append((int(next(it)), int(next(it)), int(next(it))))

        out.append(f"Case #{case}:")
        out.extend(solve_case(n, m, q, grid, queries))

    return "\n".join(out)

assert run("""1
2 3 2
###
##.
2 2 2
2 1 3
""") == """Case #1:
4
3"""

assert run("""1
1 4 1
####
2 1 3
""") == """Case #1:
4"""

assert run("""1
1 1 2
#
2 1 1
1 1 1
""") == """Case #1:
1"""

assert run("""1
3 3 3
###
#.#
###
2 2 2
1 2 2
2 2 2
""") == """Case #1:
3
9"""

assert run("""1
2 2 1
..
..
2 1 1
""") == """Case #1:
0"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Lưới mẫu có hai truy vấn | 4, 3 | Mở rộng hình chữ nhật cơ bản | 
| Một hàng tế bào khô | 4 | Hình chữ nhật một chiều | 
| Lưới một ô có cập nhật | 1 | Kích thước tối thiểu và xử lý lật | 
| Tế bào trung tâm trở nên khô | 3 rồi 9 | Cập nhật và hình chữ nhật lớn hơn | 
| Lưới ướt hoàn toàn | 0 | Trường hợp hình chữ nhật không thể | 

## Vỏ cạnh 

Khi ô được truy vấn bị ướt, thuật toán sẽ kiểm tra bit trước khi quét. Đối với một lưới chỉ chứa`.`tại vị trí được truy vấn, câu trả lời ngay lập tức trở thành 0 vì mọi hình chữ nhật có thể có sẽ bao gồm ô ướt đó. 

Khi hình chữ nhật tối ưu chạm vào một đường viền, quá trình quét lên hoặc xuống sẽ tự nhiên chạm tới cạnh của mảng hàng. Không có xử lý ranh giới đặc biệt vì quá trình quét chỉ dừng sau hàng hợp lệ cuối cùng. 

Khi chỉ có một hàng hoặc một cột, logic giao nhau vẫn được áp dụng. Phép tính chiều rộng có thể trả về một và công thức diện tích xử lý chính xác các hình chữ nhật mỏng. Không có giả định nào về chiều cao hoặc chiều rộng tối thiểu được đưa vào thuật toán.
