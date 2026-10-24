---
title: "CF 106015J - Lưới Cà Phê Halzoom"
description: "Lưới biểu thị một trường giá trị, trong đó mỗi ô chứa một số không âm mô tả “cường độ mùi”."
date: "2026-06-22T16:48:02+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106015
codeforces_index: "J"
codeforces_contest_name: "Game of Coders 4 - Over the Garden Wall"
rating: 0
weight: 106015
solve_time_s: 66
verified: true
draft: false
---

[CF 106015J - Lưới cà phê của Halzoom](https://codeforces.com/problemset/problem/106015/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 6s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Lưới biểu thị một trường giá trị, trong đó mỗi ô chứa một số không âm mô tả “cường độ mùi”. Nhiệm vụ không phải là tìm một hình chữ nhật tốt nhất mà là kiểm tra mọi hình chữ nhật con có thể có của lưới và kiểm tra xem tổng của nó có bằng giá trị mục tiêu k cho trước hay không. 

Mỗi khi một hình chữ nhật như vậy tồn tại, tất cả các ô bên trong nó đều trở nên “quan trọng”. Ở kết quả đầu ra cuối cùng, một ô được giữ nguyên nếu nó thuộc về ít nhất một hình chữ nhật hợp lệ có tổng chính xác là k. Nếu nó không bao giờ xuất hiện trong bất kỳ hình chữ nhật nào như vậy thì nó sẽ được thay thế bằng số 0. 

Khó khăn chính là các hình chữ nhật chồng lên nhau rất nhiều, do đó, một ô có thể được xác thực bằng nhiều lựa chọn khác nhau về góc trên cùng bên trái và góc dưới cùng bên phải. 

Các ràng buộc n, m ≤ 100 đủ nhỏ để các phương pháp bậc hai hoặc bậc ba trong mỗi chiều có thể vượt qua, nhưng bất kỳ phương pháp nào tiếp cận việc liệt kê trực tiếp tất cả các hình chữ nhật đều trở nên quá chậm. Kích thước lưới chỉ cho phép khoảng 10^8 ô trong tổng công việc nếu mỗi thao tác cực kỳ rẻ và được tối ưu hóa tốt, nếu không nó sẽ hết thời gian chờ trong Python. 

Một ý tưởng ngây thơ là tính tổng của mọi hình chữ nhật có thể bằng cách sử dụng tổng tiền tố và sau đó đánh dấu các ô, nhưng điều đó dẫn đến bốn vòng lặp lồng nhau trên n và m, tạo ra khoảng 10^8 hình chữ nhật và đối với mỗi hình chữ nhật, chúng ta vẫn cần đánh dấu diện tích của nó, điều này khiến nó vượt xa các giới hạn khả thi. 

Vấn đề tế nhị thứ hai xuất hiện khi k bằng 0. Vì tất cả các giá trị đều không âm nên các hình chữ nhật hợp lệ trong trường hợp này được tạo thành hoàn toàn bằng số không. Bất kỳ giải pháp nào dựa vào việc thu nhỏ hoặc mở rộng cửa sổ đều phải xử lý cẩn thận các đoạn 0 dài, nếu không nó có thể bỏ sót các mảng con hợp lệ hoặc đếm quá mức chúng. 

## Phương pháp tiếp cận 

Phương pháp brute-force là liệt kê tất cả các cặp hàng trên cùng và dưới cùng, tất cả các cặp cột bên trái và bên phải, tính tổng của hình chữ nhật bằng cách sử dụng bảng tổng tiền tố được tính toán trước và nếu nó khớp với k, hãy đánh dấu tất cả các ô trong hình chữ nhật đó. 

Ngay cả với tổng tiền tố cho phép truy vấn tổng hình chữ nhật O(1), số hình chữ nhật vẫn là O(n^2 m^2). Với n = m = 100, đây là 10^8 hình chữ nhật. Vì mỗi hình chữ nhật thành công yêu cầu công việc đánh dấu bổ sung trên các ô có khả năng O(nm) trong trường hợp xấu nhất, nên tổng chi phí nhanh chóng trở nên không khả thi. 

Quan sát quan trọng là chúng ta thực sự không cần phải kiểm tra trực tiếp các hình chữ nhật 2D tùy ý. Nếu chúng ta sửa các hàng trên cùng và dưới cùng thì bài toán sẽ trở thành bài toán một chiều trên các cột. Đối với mỗi cột, chúng tôi tích lũy tổng giữa hai hàng này, tạo ra một mảng nén. Mọi hình chữ nhật hợp lệ giữa các hàng này bây giờ chỉ là một mảng con liền kề có tổng bằng k. 

Bởi vì tất cả các số trong lưới đều không âm nên vấn đề mảng con 1D này có thể được giải quyết một cách hiệu quả bằng cách sử dụng cửa sổ trượt hai con trỏ. Điều này tránh việc băm hoặc tra cứu tổng tiền tố trên mỗi cặp và giảm tính toán từng cặp hàng thành thời gian tuyến tính tính bằng m. 

Khi chúng tôi tìm thấy tất cả các mảng con hợp lệ cho một cặp hàng cố định, chúng tôi vẫn cần đánh dấu toàn bộ hình chữ nhật một cách hiệu quả. Việc điền trực tiếp vào từng ô trên mỗi hình chữ nhật sẽ tạo lại hệ số khối. Thay vào đó, chúng tôi lưu trữ các bản cập nhật trong một mảng khác biệt 2D để mỗi hình chữ nhật đóng góp vào O(1) và việc tái cấu trúc tổng tiền tố cuối cùng sẽ xác định ô nào đã từng được che phủ. 

Điều này biến đổi vấn đề từ việc liệt kê các hình chữ nhật một cách rõ ràng sang việc liệt kê các dải hàng và tìm các khoảng cột hợp lệ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu trên tất cả các hình chữ nhật | O(n^2 m^2 (n+m)) | O(1) | Quá chậm | 
| Sửa hàng + tổng tiền tố | O(n^2 m^2) | O(nm) | Đường biên giới quá chậm | 
| Sửa hàng + cửa sổ trượt + khác biệt 2D | O(n^2 m) | O(nm) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Cố định chỉ mục hàng trên cùng và dần dần mở rộng chỉ mục hàng dưới cùng xuống dưới. Đối với mỗi tiện ích mở rộng, hãy duy trì tổng giá trị theo cột đang chạy giữa hai hàng. Thao tác này nén lưới 2D thành mảng 1D biểu thị tổng dọc cho dải hiện tại. 
2. Đối với dải hàng hiện tại, tìm kiếm tất cả các mảng con liền kề trong mảng 1D này có tổng bằng k bằng cửa sổ trượt. Vì tất cả các giá trị đều không âm nên việc mở rộng con trỏ bên phải sẽ làm tăng tổng và việc thu nhỏ con trỏ bên trái sẽ làm giảm tổng, đảm bảo hành vi tuyến tính. 
3. Mỗi khi tìm thấy một mảng con có tổng k, hãy hiểu nó như một hình chữ nhật kéo dài từ hàng trên cùng hiện tại đến hàng dưới cùng và từ cột l bên trái đến cột r bên phải. 
4. Thay vì đánh dấu trực tiếp từng ô trong hình chữ nhật đó, hãy áp dụng cập nhật chênh lệch 2D để hình chữ nhật được ghi lại theo thời gian không đổi. Điều này đảm bảo rằng ngay cả khi hình chữ nhật lớn thì đóng góp của nó vẫn là O(1). 
5. Sau khi xử lý tất cả các cặp hàng và tất cả các khoảng cột hợp lệ, hãy chuyển đổi mảng sai phân thành lưới tổng tiền tố. Mỗi ô trong lưới cuối cùng này cho biết có bao nhiêu hình chữ nhật hợp lệ bao gồm nó. 
6. Đưa ra câu trả lời cuối cùng bằng cách giữ nguyên các giá trị lưới ban đầu cho các ô có độ bao phủ dương và đặt các ô khác về 0. 

Tính chính xác phụ thuộc vào thực tế là mỗi hình chữ nhật được xác định duy nhất bởi một cặp hàng và một mảng con hợp lệ trong biểu diễn cột nén. Cửa sổ trượt liệt kê tất cả các mảng con như vậy chính xác một lần cho mỗi cặp hàng và mảng khác biệt đảm bảo rằng tất cả các ô trong mỗi hình chữ nhật được đánh dấu nhất quán mà không bị thiếu sót hoặc trùng lặp ảnh hưởng đến tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m, k = map(int, input().split())
    a = [list(map(int, input().split())) for _ in range(n)]

    diff = [[0] * (m + 1) for _ in range(n + 1)]

    for top in range(n):
        col_sum = [0] * m

        for bot in range(top, n):
            for j in range(m):
                col_sum[j] += a[bot][j]

            l = 0
            cur = 0

            for r in range(m):
                cur += col_sum[r]

                while l <= r and cur > k:
                    cur -= col_sum[l]
                    l += 1

                if cur == k:
                    diff[top][l] += 1
                    diff[top][r + 1] -= 1
                    diff[bot + 1][l] -= 1
                    diff[bot + 1][r + 1] += 1

    for i in range(n):
        for j in range(m):
            diff[i][j] += diff[i - 1][j] if i > 0 else 0
    for i in range(n):
        for j in range(m):
            diff[i][j] += diff[i][j - 1] if j > 0 else 0

    out = []
    for i in range(n):
        row = []
        for j in range(m):
            if diff[i][j] > 0:
                row.append(str(a[i][j]))
            else:
                row.append("0")
        out.append(" ".join(row))

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Giải pháp duy trì mảng tổng cột động cho từng cặp hàng, sau đó sử dụng cửa sổ trượt để liệt kê tất cả các khoảng cột hợp lệ. Mỗi khoảng được chuyển đổi thành bản cập nhật hình chữ nhật bằng cấu trúc sai phân 2D. Tổng tiền tố kép cuối cùng sẽ xây dựng lại số lượng vùng phủ sóng và bước đầu ra chỉ cần lọc lưới ban đầu dựa trên việc một ô có từng được đưa vào bất kỳ hình chữ nhật hợp lệ nào hay không. 

Một điểm tinh tế là cửa sổ trượt chỉ hoạt động vì tất cả các giá trị lưới đều không âm. Nếu các giá trị âm được cho phép, tính đơn điệu cần thiết cho việc thu nhỏ và mở rộng cửa sổ sẽ bị phá vỡ và thay vào đó, cách tiếp cận tổng tiền tố dựa trên hàm băm sẽ là cần thiết. 

Mảng khác biệt cũng tránh được lỗi phổ biến khi cố gắng vẽ trực tiếp các hình chữ nhật. Nếu không có nó, giải pháp sẽ chuyển sang cập nhật các ô O(nm) trên mỗi hình chữ nhật, tốc độ này quá chậm. 

## Ví dụ đã hoạt động 

Hãy xem xét một lưới nhỏ trong đó các hình chữ nhật hợp lệ chỉ tồn tại ở một số vùng. Chúng tôi theo dõi một dải hàng đơn và quá trình nén cột kết quả. 

Đối với một cặp hàng cố định, giả sử mảng nén trở thành [1, 0, 1, 3]. 

Chúng tôi theo dõi cửa sổ trượt: 

| r | col_sum[r] | cur_sum | tôi | hành động | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 1 | 0 | mở rộng | 
| 1 | 0 | 1 | 0 | mở rộng | 
| 2 | 1 | 2 | 0 | mở rộng | 
| 3 | 3 | 5 | 0 | thu nhỏ l=1 | 
| 3 | 3 | 5 | 1 | co lại l=2 | 
| 3 | 3 | 3 | 2 | trận đấu | 

Tại r = 3, chúng tôi phát hiện một mảng con hợp lệ [2,3] khi k = 3. Điều này tạo ra một hình chữ nhật bao gồm các hàng top..bot và các cột 2..3. 

Dấu vết này cho thấy rằng ngay cả khi các số 0 xuất hiện trong mảng, cửa sổ vẫn bảo toàn chính xác các tổng hợp lệ và không bỏ qua các kết quả khớp có thể có. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n² m) | Có các cặp hàng O(n²) và mỗi cặp xử lý các cột trong O(m) bằng cửa sổ trượt | 
| Không gian | O(nm) | Được sử dụng để lưu trữ lưới và mảng sai phân 2D | 

Với n, m ≤ 100, tổng số thao tác là khoảng 10^6, phù hợp thoải mái trong giới hạn thời gian trong Python. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    old_stdout = sys.stdout
    sys.stdout = io.StringIO()
    solve()
    out = sys.stdout.getvalue().strip()
    sys.stdout = old_stdout
    return out

# sample
assert run("""5 5 5
1 1 1 1 5
1 7 1 1 1
1 1 1 1 1
1 1 1 1 1
1 1 1 1 5
""") == """1 0 1 1 5
1 0 1 1 0
1 1 1 1 1
1 1 1 1 1
1 0 1 1 5"""

# all zeros, k=0
assert run("""2 3 0
0 0 0
0 0 0
""") == """0 0 0
0 0 0"""

# no valid rectangles
assert run("""2 2 10
1 1
1 1
""") == """0 0
0 0"""

# single row
assert run("""1 5 3
1 1 1 0 0
""") == """1 1 1 0 0"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả số không | tất cả số không | hành vi k=0 và xử lý bằng 0 | 
| không có giải pháp | tất cả lưới không | thiếu hình chữ nhật hợp lệ | 
| hàng đơn | tiền tố một phần được giữ | Độ chính xác giảm 1D | 

## Vỏ cạnh 

Khi k bằng 0, mọi hình chữ nhật hợp lệ phải bao gồm toàn số 0. Cửa sổ trượt vẫn hoạt động chính xác vì tổng bắt đầu bằng 0 và chỉ mở rộng khi gặp giá trị dương. Đối với đầu vào như một hàng số 0, mọi mảng con đều hợp lệ và mảng sai phân đánh dấu lưới đầy đủ, điều này đúng vì mọi ô đều nằm trong một hình chữ nhật có tổng bằng 0 nào đó. 

Đối với các lưới có các gai khác 0 bị cô lập, chỉ những hình chữ nhật cân bằng hoàn hảo với k mới được phát hiện. Vì cửa sổ trượt co lại bất cứ khi nào tổng vượt quá k, nên nó tránh được việc mở rộng sai các hình chữ nhật vượt quá ranh giới hợp lệ, ngay cả khi các giá trị lớn xuất hiện ở giữa một dải hàng. 

Các lưới nhỏ như 1x1 được xử lý một cách tự nhiên vì vòng lặp cặp hàng thoái hóa thành một lần lặp duy nhất và cửa sổ trượt đánh giá trực tiếp mảng con duy nhất có thể.
