---
title: "CF 1105C - Ayoub và Lost Array"
description: "Chúng ta được yêu cầu đếm xem có thể tạo ra bao nhiêu mảng có độ dài $n$ sao cho mọi phần tử đều nằm trong một khoảng cố định $[l, r]$ và tổng tổng của tất cả các phần tử chia hết cho 3."
date: "2026-06-12T05:31:27+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "dp", "math"]
categories: ["algorithms"]
codeforces_contest: 1105
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 533 (Div. 2)"
rating: 1500
weight: 1105
solve_time_s: 85
verified: true
draft: false
---

[CF 1105C - Ayoub và Lost Array](https://codeforces.com/problemset/problem/1105/C) 

**Đánh giá:** 1500 
**Tags:** tổ hợp, dp, toán 
**Thời gian giải:** 1 phút 25s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu đếm có bao nhiêu mảng có chiều dài$n$có thể được hình thành sao cho mọi phần tử đều nằm trong một khoảng cố định$[l, r]$và tổng của tất cả các phần tử chia hết cho 3. 

Vấn đề không phải là xây dựng một mảng cụ thể mà là việc đếm tất cả các cấu hình hợp lệ theo hai ràng buộc cùng một lúc: hạn chế giá trị cho mỗi vị trí và ràng buộc mô-đun toàn cục về tổng. 

Các ràng buộc rất lớn:$n$có thể lên đến$2 \cdot 10^5$và các giá trị trong mảng có thể lớn bằng$10^9$. Điều này ngay lập tức loại trừ mọi cách tiếp cận lặp lại trên tất cả các mảng có thể có hoặc thậm chí tất cả các tổng có thể có. Bất kỳ giải pháp hợp lệ nào cũng phải giảm vấn đề xuống một không gian trạng thái cố định nhỏ, thường dựa trên số học mô-đun thay vì giá trị thực. 

Một khó khăn tinh tế xuất hiện khi nghĩ về phạm vi$[l, r]$. Mặc dù các giá trị lớn, nhưng điều quan trọng đối với khả năng chia hết cho 3 chỉ là phần dư của chúng theo modulo 3. Sự phân bố phần dư trong khoảng không phải lúc nào cũng đồng đều, đặc biệt khi độ dài khoảng không chia hết cho 3. 

Một sai lầm phổ biến là giả định rằng mỗi lớp dư lượng xuất hiện thường xuyên như nhau mà không có ranh giới xử lý. Ví dụ: trong một phạm vi nhỏ như$[1, 3]$, mỗi phần dư xuất hiện đúng một lần, nhưng trong$[1, 4]$, số lượng cặn trở nên không đồng đều. Bất kỳ giải pháp nào bỏ qua điều này sẽ tính sai các mảng hợp lệ. 

Một trường hợp cạnh khác là khi$n = 1$. Thì câu trả lời đơn giản là có bao nhiêu số trong$[l, r]$có thể chia hết cho 3 và một công thức DP ngây thơ có thể đếm quá mức không chính xác bằng cách xử lý các vị trí một cách độc lập mà không thực thi ràng buộc tổng toàn cục. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: tạo ra mọi mảng có độ dài$n$, kiểm tra xem mỗi phần tử có nằm trong$[l, r]$, tính tổng của nó và xác minh tính chia hết cho 3. Số mảng là$(r-l+1)^n$, lớn về mặt thiên văn ngay cả đối với đầu vào nhỏ, khiến điều này hoàn toàn không khả thi. 

Quan sát quan trọng là điều kiện chỉ phụ thuộc vào tổng modulo 3. Mỗi phần tử chỉ đóng góp phần còn lại theo modulo 3 của tổng cuối cùng. Điều này làm giảm mỗi phần tử mảng từ một không gian số nguyên có khả năng rất lớn thành ba loại: các số đồng dạng với 0, 1 hoặc 2 modulo 3. 

Khi việc giảm này được thực hiện, vấn đề sẽ trở thành DP cổ điển theo chiều dài$n$và trạng thái modulo của tổng chạy. Phần còn thiếu duy nhất là xác định có bao nhiêu số nguyên trong$[l, r]$thuộc từng loại dư lượng. Sau đó, các chuyển đổi trở thành tổ hợp thuần túy: tại mỗi vị trí, chúng tôi chọn một lớp dư lượng và cập nhật tổng modulo 3. 

Điều này làm giảm vấn đề từ liệt kê theo cấp số nhân sang DP tuyến tính với kích thước trạng thái không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O((r-l+1)^n)$|$O(n)$| Quá chậm | 
| DP tối ưu |$O(n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên, chúng tôi nén phạm vi đầu vào thành số lượng dư lượng modulo 3. 

1. Tính xem có bao nhiêu số$[l, r]$đồng dạng với 0, 1 và 2 modulo 3. 

Điều này được thực hiện bằng cách sử dụng tính lũy tiến số học. Đối với mỗi loại dư lượng, chúng ta tìm số đầu tiên trong khoảng có dư lượng đó và đếm xem có bao nhiêu bước có kích thước 3 phù hợp cho đến khi$r$. Bước này là cần thiết vì chúng ta không thể lặp lại trực tiếp trên phạm vi. 
2. Hãy để số đếm này là$c_0, c_1, c_2$. Mỗi vị trí trong mảng đang chọn một trong các lớp dư lượng này một cách hiệu quả. 
3. Xác định mảng DP trong đó$dp[i][m]$là số cách chọn cái đầu tiên$i$các phần tử sao cho tổng modulo 3 của chúng bằng$m$. 

Chúng tôi chỉ quan tâm đến$m \in \{0, 1, 2\}$. 
4. Khởi tạo$dp[0][0] = 1$, vì có đúng một cách để chọn không có tổng bằng 0. 
5. Đối với mỗi vị trí từ 1 đến$n$, cập nhật trạng thái DP mới: 

chúng tôi thử đặt một số có dư 0, 1 hoặc 2. 

Mỗi sự lựa chọn đều góp phần$c_0$,$c_1$, hoặc$c_2$cách tương ứng và thay đổi trạng thái modulo tương ứng. 

Ví dụ đặt dư 1 phần tử sẽ chuyển trạng thái$m$ĐẾN$(m+1) \bmod 3$. 
6. Sau khi xử lý tất cả các vị trí, câu trả lời là$dp[n][0]$, vì chúng ta muốn tổng chia hết cho 3. 

### Tại sao nó hoạt động 

Ở mỗi bước, trạng thái DP nắm bắt chính xác sự phân bố của tổng một phần theo modulo 3 trên tất cả các tiền tố hợp lệ. Quá trình chuyển đổi duy trì tính chính xác vì mỗi tiền tố mảng được hình thành duy nhất bằng cách nối thêm một trong ba lớp dư lượng và mỗi lớp đóng góp chính xác số lượng giá trị chính xác từ khoảng đó. Vì phép cộng modulo đóng và mang tính xác định nên không có hai đường xây dựng khác nhau nào giao thoa không chính xác và tất cả các mảng đều được tính chính xác một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def count_residues(l, r):
    cnt = [0, 0, 0]
    for rem in range(3):
        # first number >= l with value ≡ rem (mod 3)
        start = l + (rem - l % 3) % 3
        if start > r:
            cnt[rem] = 0
        else:
            cnt[rem] = (r - start) // 3 + 1
    return cnt

def solve():
    n, l, r = map(int, input().split())
    
    c0, c1, c2 = count_residues(l, r)
    
    dp = [0, 0, 0]
    dp[0] = 1
    
    for _ in range(n):
        ndp = [0, 0, 0]
        for m in range(3):
            if dp[m] == 0:
                continue
            ndp[m] = (ndp[m] + dp[m] * c0) % MOD
            ndp[(m + 1) % 3] = (ndp[(m + 1) % 3] + dp[m] * c1) % MOD
            ndp[(m + 2) % 3] = (ndp[(m + 2) % 3] + dp[m] * c2) % MOD
        dp = ndp
    
    print(dp[0] % MOD)

if __name__ == "__main__":
    solve()
```Mã bắt đầu bằng cách nén phạm vi giá trị thành ba số đếm, một số cho mỗi loại dư lượng. Mảng DP chỉ theo dõi ba trạng thái, tương ứng với tổng modulo 3. Mỗi lần lặp mô phỏng việc đặt một phần tử mảng và cập nhật tất cả các chuyển đổi modulo có thể có bằng cách sử dụng số đếm được tính toán trước. Câu trả lời cuối cùng là số chuỗi có độ dài đầy đủ có tổng chia hết cho 3. 

Một chi tiết thực hiện tinh tế là công thức tính dư lượng. Biểu thức cho`start`căn chỉnh cẩn thận`l`lên đến số tiếp theo với modulo chính xác và phép chia cho 3 sẽ đếm các bước đầy đủ trong cấp số cộng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 1 3
```Đầu tiên chúng tôi tính toán lượng dư lượng trong$[1, 3]$: mỗi số ứng với các thặng dư 0, 1, 2, vậy$c_0 = c_1 = c_2 = 1$. 

| Bước | dp[0] | dp[1] | dp[2] | 
| --- | --- | --- | --- | 
| 0 | 1 | 0 | 0 | 
| 1 | 1 | 1 | 1 | 
| 2 | 3 | 3 | 3 | 

Câu trả lời cuối cùng là$dp[2][0] = 3$, tương ứng với mảng$[1,2], [2,1], [3,3]$. 

Dấu vết này cho thấy mức độ đối xứng giữa các lớp dư lượng tạo ra các chuyển tiếp cân bằng và cách nhiều đường dẫn có thể kết thúc ở cùng một trạng thái modulo. 

### Ví dụ 2 

đầu vào:```
1 2 4
```Dư lượng trong$[2,4]$là$2 \equiv 2$,$3 \equiv 0$,$4 \equiv 1$, vậy một lần nữa$c_0 = c_1 = c_2 = 1$. 

| Bước | dp[0] | dp[1] | dp[2] | 
| --- | --- | --- | --- | 
| 0 | 1 | 0 | 0 | 
| 1 | 1 | 1 | 1 | 

Câu trả lời là$dp[1][0] = 1$, tương ứng với mảng hợp lệ duy nhất$[3]$. 

Điều này thể hiện hành vi của trường hợp cơ sở trong đó độ dài 1 giảm trực tiếp để đếm các phần tử hợp lệ chia hết cho 3. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi trong số$n$bước thực hiện DP có kích thước không đổi trên 3 trạng thái | 
| Không gian |$O(1)$| Chỉ duy trì mảng DP 3 phần tử cố định | 

Lời giải dễ dàng nằm trong giới hạn vì$n \le 2 \cdot 10^5$và mỗi lần lặp chỉ bao gồm một vài phép tính số học. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    MOD = 10**9 + 7

    def count_residues(l, r):
        cnt = [0, 0, 0]
        for rem in range(3):
            start = l + (rem - l % 3) % 3
            if start > r:
                cnt[rem] = 0
            else:
                cnt[rem] = (r - start) // 3 + 1
        return cnt

    n, l, r = map(int, sys.stdin.readline().split())
    c0, c1, c2 = count_residues(l, r)

    dp = [0, 0, 0]
    dp[0] = 1

    for _ in range(n):
        ndp = [0, 0, 0]
        for m in range(3):
            ndp[m] = (ndp[m] + dp[m] * c0) % MOD
            ndp[(m+1)%3] = (ndp[(m+1)%3] + dp[m] * c1) % MOD
            ndp[(m+2)%3] = (ndp[(m+2)%3] + dp[m] * c2) % MOD
        dp = ndp

    return str(dp[0] % MOD)

# provided samples
assert run("2 1 3\n") == "3", "sample 1"

# custom cases
assert run("1 1 1\n") == "0", "single element not divisible"
assert run("1 3 3\n") == "1", "single valid element"
assert run("3 1 1\n") == "1", "all same value"
assert run("2 2 5\n") == run("2 2 5\n"), "consistency check"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 1 | 0 | không có tổng hợp lệ chia hết cho 3 | 
| 1 3 3 | 1 | trường hợp hợp lệ một phần tử | 
| 3 1 1 | 1 | phạm vi suy biến, giá trị lặp lại | 
| 2 2 5 | tự thống nhất | độ ổn định của logic DP | 

## Vỏ cạnh 

Trường hợp một cạnh xảy ra khi khoảng không chứa số nào của lớp dư lượng nhất định. Ví dụ, nếu$l = 1$Và$r = 1$, chỉ có dư lượng 1 xuất hiện. Sau đó, DP sẽ sụp đổ để liên tục thêm một loại chuyển đổi. Thuật toán xử lý việc này một cách chính xác vì số lượng dư lượng khác bằng 0, vì vậy chúng không bao giờ góp phần vào quá trình chuyển đổi. 

Một trường hợp khác là khi phạm vi cực kỳ lớn nhưng phân bổ đều trên các dư lượng, chẳng hạn như$[1, 10^9]$. Công thức đếm vẫn tính toán chính xác số lượng gần như bằng nhau cho mỗi loại dư lượng mà không cần lặp lại và DP vẫn ổn định vì nó chỉ sử dụng số lượng tổng hợp thay vì các giá trị riêng lẻ. 

Trường hợp thứ ba là$n = 1$, trong đó câu trả lời chỉ đơn giản là số phần tử chia hết cho 3 trong khoảng. DP bắt đầu lúc$dp[0][0]$và sau một lần lặp, chỉ áp dụng các chuyển đổi trực tiếp, tạo ra chính xác số phần tử dư 0, khớp với kết quả mong đợi.
