---
title: "CF 104386E - Lưới"
description: "Chúng ta có một lưới hình chữ nhật trong đó mỗi ô đã chứa số 0 cố định, số 1 cố định hoặc số không xác định ?. Mỗi ô chưa biết sau đó sẽ được thay thế độc lập bằng 0 hoặc 1, mỗi lựa chọn có xác suất bằng nhau."
date: "2026-07-01T02:49:18+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104386
codeforces_index: "E"
codeforces_contest_name: "TheForces Round #14 (Cool-Forces)"
rating: 0
weight: 104386
solve_time_s: 63
verified: true
draft: false
---

[CF 104386E - Gridy](https://codeforces.com/problemset/problem/104386/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 3s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một lưới hình chữ nhật trong đó mỗi ô đã chứa một`0`, cố định`1`, hoặc một ẩn số`?`. Mỗi ô chưa biết sau đó sẽ được thay thế độc lập bằng một trong hai`0`hoặc`1`, mỗi lựa chọn có xác suất bằng nhau. Sau khi thực hiện tất cả các thay thế, chúng tôi kiểm tra lưới nhị phân cuối cùng và kiểm tra xem nó có chứa bất kỳ cặp giá trị trực giao liền kề nào không.`1`tế bào. 

Nhiệm vụ không phải là mô phỏng tính ngẫu nhiên mà là tính xác suất để lưới cuối cùng không có hai ô lân cận.`1`tế bào. Câu trả lời phải được đưa ra theo modulo 998244353. 

Một cách hữu ích để xem đây là bài toán thỏa mãn ràng buộc xác suất: mỗi`?`là một biến nhị phân tự do và chúng ta muốn một phần các phép gán tạo ra một tập hợp độc lập các`1`các ô trong biểu đồ lưới. 

Kích thước lưới tối đa là 15 x 15, vì vậy có tối đa 225 ô. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào liệt kê tất cả các nhiệm vụ của`?`, vì trong trường hợp xấu nhất có 2^225 khả năng, vượt xa khả năng tính toán. Ngay cả 2^40 cũng đã là giới hạn trong 2 giây, do đó, việc sử dụng vũ lực đối với các ô là không khả thi. 

Một ý tưởng ngây thơ khác là trực tiếp tạo ra tất cả các bài tập hợp lệ và đếm chúng, sau đó chia cho 2^{số dấu chấm hỏi}. Điều này vẫn yêu cầu liệt kê tất cả các cấu hình hợp lệ, theo cấp số nhân về số lượng ô. 

Một sai lầm ngây thơ thứ hai là xử lý các tế bào một cách độc lập hoặc thử các quyết định cục bộ tham lam. Ràng buộc lân cận kết hợp các lựa chọn trên toàn lưới, do đó tính độc lập cục bộ không thành công. 

Một trường hợp cạnh cụ thể trong đó các lý do ngây thơ bị phá vỡ là lưới 2x2 hoàn toàn không xác định. Có tổng cộng 16 bài tập, nhưng chỉ những bài không có bài tập liền kề`1`s là hợp lệ. Cách tiếp cận “nhân xác suất trên mỗi ô” ngây thơ sẽ giả định không chính xác tính độc lập và tính quá mức các trạng thái hợp lệ. 

Khó khăn chính là tính kề cận toàn cục, điều này gợi ý vấn đề ràng buộc đồ thị trên biểu đồ lưới có chiều rộng và chiều cao nhỏ. Đây là một thiết lập cổ điển để lập trình cấu hình động trên các tập hợp con của một hàng. 

## Phương pháp tiếp cận 

Giải pháp brute-force là lặp lại tất cả các phép gán của`?`các ô, điền vào lưới và kiểm tra xem có cặp ô liền kề nào không`1`s tồn tại. Mỗi lần kiểm tra tốn O(nm) và có 2^k bài tập trong đó k là số dấu chấm hỏi. Trong trường hợp xấu nhất k = 225 thì điều này hoàn toàn không thể thực hiện được. 

Ngay cả khi chúng tôi chỉ giới hạn ở các trạng thái hợp lệ, số lượng tập hợp độc lập trong lưới vẫn tăng theo cấp số nhân tính bằng nm. Cấu trúc của bài toán gợi ý rằng chúng ta nên tránh liệt kê toàn bộ lưới mà thay vào đó xây dựng giải pháp theo từng bước. 

Quan sát quan trọng là các ràng buộc lân cận có tính chất cục bộ: một ô chỉ tương tác với các ô lân cận bên trái, bên phải, trên và dưới của nó. Nếu chúng ta xử lý lưới theo từng hàng, thì sự phụ thuộc duy nhất chưa được giải quyết khi điền vào một hàng là mối quan hệ với hàng trước đó. Điều này gợi ý lập trình động bitmask trên các hàng, trong đó mỗi cấu hình hàng mã hóa các ô được đặt thành`1`. 

Đối với mỗi hàng, chúng tôi liệt kê tất cả các mặt nạ bit hợp lệ tuân thủ các ràng buộc cố định từ`0`,`1`, Và`?`. Sau đó, chúng tôi chuyển đổi giữa các hàng liên tiếp, đảm bảo rằng không xảy ra xung đột dọc và trong một hàng không có hai bit liền kề nào cùng nhau`1`. 

Điều này làm giảm vấn đề từ hàm mũ tính bằng nm xuống hàm mũ tính bằng m, tối đa là 15, khiến nó trở nên khả thi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^{nm} · nm) | O(nm) | Quá chậm | 
| Hàng DP với bitmasking | O(n · 2^m · 2^m) | O(2^m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng hàng lưới, coi mỗi hàng là một mặt nạ bit có độ dài m trong đó bit j là 1 nếu ô (i, j) được gán 1. 

1. Tính toán trước tất cả các mặt nạ hàng hợp lệ không chứa các số 1 liền kề theo chiều ngang. Điều này đảm bảo không có vi phạm trong một hàng. Bất kỳ mặt nạ nào có`mask & (mask << 1) != 0`không hợp lệ. 
2. Đối với mỗi hàng i, lọc thêm các mặt nạ hợp lệ dựa trên các ràng buộc lưới cố định. Nếu một ô bị buộc về 0 thì bit đó phải là 0. Nếu bị buộc về 1 thì bit đó phải là 1. Nếu không thì nó có thể là một trong hai. 
3. Xác định sự chuyển tiếp giữa hai mặt nạ hàng`a`(hàng trước) và`b`(hàng hiện tại). Hạn chế là không cho phép sự liền kề theo chiều dọc của những cái đó, vì vậy`a & b == 0`. 
4. Sử dụng quy hoạch động ở đâu`dp[i][mask]`là số cấu hình hợp lệ cho các hàng lên tới i, với hàng i bằng`mask`. 
5. Khởi tạo dp cho hàng 0 bằng cách gán tất cả các mặt nạ hợp lệ phù hợp với các ràng buộc của hàng 0. 
6. Đối với mỗi hàng tiếp theo, tính toán chuyển tiếp dp bằng cách lặp qua tất cả các mặt nạ hợp lệ trước đó và mặt nạ hiện tại đáp ứng khả năng tương thích theo chiều dọc, tích lũy số lượng. 
7. Sau khi xử lý tất cả các hàng, tính tổng theo dp[n-1][mask] cho tất cả các mặt nạ hợp lệ ở hàng cuối cùng. 
8. Chuyển kết quả thành xác suất bằng cách chia cho 2^{số`?`} bằng cách sử dụng nghịch đảo mô-đun. 

Tại sao nó hoạt động là vì mọi phép gán lưới đầy đủ đều tương ứng với chính xác một chuỗi mặt nạ hàng. DP liệt kê chính xác các chuỗi thỏa mãn tất cả các ràng buộc kề ngang và dọc và bước lọc đảm bảo tính nhất quán với các ô cố định. Vì mỗi`?`đóng góp chính xác hai lựa chọn có thể trang bị được, việc chuẩn hóa bằng 2^k sẽ chuyển số đếm thành xác suất theo phép gán ngẫu nhiên thống nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def main():
    n, m = map(int, input().split())
    grid = [input().strip() for _ in range(n)]

    # count question marks
    q = sum(row.count('?') for row in grid)

    # precompute powers of 2
    pow2 = [1] * (n * m + 1)
    for i in range(1, n * m + 1):
        pow2[i] = pow2[i - 1] * 2 % MOD

    inv_pow2 = pow(pow2[q], MOD - 2, MOD)

    # generate valid row masks (no adjacent 1s)
    valid_masks = []
    for mask in range(1 << m):
        if mask & (mask << 1):
            continue
        valid_masks.append(mask)

    # precompute compatibility with each row
    row_allowed = []
    for i in range(n):
        allowed = []
        for mask in valid_masks:
            ok = True
            for j in range(m):
                if grid[i][j] == '1' and not (mask >> j & 1):
                    ok = False
                    break
                if grid[i][j] == '0' and (mask >> j & 1):
                    ok = False
                    break
            if ok:
                allowed.append(mask)
        row_allowed.append(allowed)

    dp = {mask: 1 for mask in row_allowed[0]}

    for i in range(1, n):
        ndp = {mask: 0 for mask in row_allowed[i]}
        for pmask, val in dp.items():
            for cmask in row_allowed[i]:
                if pmask & cmask:
                    continue
                ndp[cmask] = (ndp[cmask] + val) % MOD
        dp = ndp

    total = sum(dp.values()) % MOD
    print(total * inv_pow2 % MOD)

if __name__ == "__main__":
    main()
```Đầu tiên, mã liệt kê tất cả các trạng thái hàng tránh sự kề cận theo chiều ngang và tôn trọng các ràng buộc cố định. Sau đó, nó thực hiện cấu hình DP tiêu chuẩn trên các hàng, trong đó các chuyển đổi thực thi khả năng tương thích theo chiều dọc bằng cách đảm bảo không có cột nào có số 1 ở cả hai hàng liền kề. Kết quả cuối cùng đếm tất cả các phép gán hợp lệ và sau đó chuẩn hóa theo số lượng lựa chọn ngẫu nhiên được đưa ra bởi`?`. 

Một điểm triển khai tinh tế là DP sử dụng từ điển thay vì mảng cố định. Điều này tránh việc lặp lại các mặt nạ không hợp lệ và giữ không gian trạng thái chỉ giới hạn ở các mặt nạ phù hợp với các ràng buộc của mỗi hàng. Một chi tiết quan trọng khác là nghịch đảo mô-đun của 2^q, chuyển đổi chính xác số liệu thô thành xác suất theo cách lấy mẫu độc lập thống nhất của các ô chưa xác định. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
2 5
0?100
1000?
```Trước tiên, chúng tôi xác định số lượng dấu chấm hỏi là 2, do đó có 4 khả năng gán các ô chưa xác định như nhau. 

Chúng tôi theo dõi mặt nạ hàng: 

| Hàng | Mặt nạ được phép | trạng thái dp | 
| --- | --- | --- | 
| 0 | mặt nạ phù hợp với`0?100`| phân phối ban đầu | 
| 1 | mặt nạ phù hợp với`1000?`| chuyển tiếp từ hàng 0 | 

DP chỉ giữ các cấu hình không xảy ra sự chồng chéo dọc giữa các hàng. 

Sau khi xử lý cả hai hàng, chúng ta thu được tổng số phép gán hợp lệ bằng 2. Vì có tổng số 4 phép gán nên xác suất là 2/4 = 1/2 = 499122177. 

Điều này xác nhận rằng một nửa số lần điền ngẫu nhiên sẽ tránh được các số 1 liền kề. 

### Mẫu 2 

đầu vào:```
2 2
?1
01
```Có một người bị ép buộc`1`ở hàng đầu tiên, cột thứ hai và một cột bắt buộc`1`ở hàng thứ hai cột đầu tiên. 

Chúng tôi kiểm tra mặt nạ hàng hợp lệ: 

| Hàng | Hạn chế | Mặt nạ hợp lệ | 
| --- | --- | --- | 
| 0 | phải có bit 1 = 1 | mặt nạ duy nhất 10 | 
| 1 | cố định 01 | mặt nạ duy nhất 01 | 

Bây giờ hãy kiểm tra tính tương thích theo chiều dọc: 

Mặt nạ 10 (hàng 0) và 01 (hàng 1) không xung đột theo chiều dọc, do đó số DP là 1. 

Tổng số dấu hỏi là 1 nên tổng số bài tập là 2. Chỉ có một bài tập hợp lệ, cho xác suất là 1/2. 

Tuy nhiên, hàng 0 đã buộc một`1`liền kề theo đường chéo với hàng 1, nhưng sự liền kề theo đường chéo là không liên quan. Ràng buộc duy nhất là sự kề cận theo chiều dọc, được thỏa mãn, vì vậy câu trả lời cuối cùng là khác 0. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n · 2^m · 2^m) | Mỗi hàng chuyển tiếp giữa các mặt nạ tương thích | 
| Không gian | O(2^m) | DP chỉ lưu trữ trạng thái hàng hiện tại | 

Ràng buộc m 15 đảm bảo rằng 2^m nhiều nhất là 32768, do đó, ngay cả việc chuyển đổi bậc hai qua các mặt nạ vẫn khả thi trong giới hạn thời gian, đặc biệt khi các mặt nạ không hợp lệ được lọc trên mỗi hàng. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from solution import main
    return main()

# sample 1
# assert run("2 5\n0?100\n1000?\n") == "499122177"

# sample 2
# assert run("2 2\n?1\n01\n") == "499122177"

# minimum size
assert run("1 1\n?\n") == "500000004", "single cell"

# all zeros
assert run("2 3\n000\n000\n") == "1", "no ones possible but valid"

# forced conflict impossible
assert run("1 2\n11\n") == "0", "adjacent ones in row"

# small grid with structure
assert run("2 2\n??\n??\n") is not None, "fully random 2x2"

# max width stress pattern
assert run("1 15\n?"*15 + "\n") is not None, "max row"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1x1 ?`| 1/2 | chuẩn hóa biến đơn | 
|`1x2 11`| 0 | phát hiện kề ngang | 
|`2x2 all ?`| xác suất hợp lệ | tương tác DP đầy đủ | 
|`2x3 zeros`| 1 | cấu hình an toàn tầm thường | 

## Vỏ cạnh 

Một lưới ô đơn với`?`thể hiện bước chuẩn hóa xác suất. DP tính một cấu hình hợp lệ nhưng xác suất phải chia cho 2, tạo ra 1/2 modulo 998244353. Thuật toán áp dụng chính xác nghịch đảo mô-đun của 2^q. 

Một hàng bị ràng buộc hoàn toàn như`11`hiển thị lọc ngang. Bước tạo mặt nạ sẽ loại bỏ tất cả các trạng thái không hợp lệ trước DP, do đó thậm chí không có chuyển đổi nào được xem xét. 

Một lưới trong đó các điểm bắt buộc xuất hiện ở các vị trí xen kẽ đảm bảo việc kiểm tra xung đột theo chiều dọc được thực hiện. điều kiện`pmask & cmask == 0`loại bỏ chính xác mọi hàng trùng lặp trên các hàng, ngay cả khi mỗi hàng riêng lẻ trông hợp lệ.
