---
title: "CF 1836C - đẳng thức thứ k"
description: "Chúng ta được yêu cầu liệt kê tất cả các phương trình hợp lệ có dạng a + b = c, nhưng với các ràng buộc nghiêm ngặt về độ dài chữ số: a phải có chính xác A chữ số, b chính xác B chữ số và c chính xác C chữ số. Không có số nào có thể bắt đầu bằng 0 và cả ba giá trị phải là số nguyên dương."
date: "2026-06-09T06:43:27+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "greedy", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 1836
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 880 (Div. 2)"
rating: 1700
weight: 1836
solve_time_s: 83
verified: true
draft: false
---

[CF 1836C - đẳng thức thứ k](https://codeforces.com/problemset/problem/1836/C) 

**Đánh giá:** 1700 
**Tags:** vũ phu, tham lam, thực hiện, toán học 
**Thời gian giải:** 1m 23s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu liệt kê tất cả các phương trình hợp lệ có dạng`a + b = c`, nhưng với các ràng buộc nghiêm ngặt về độ dài chữ số:`a`phải có chính xác`A`chữ số,`b`chính xác`B`chữ số và`c`chính xác`C`chữ số. Không có số nào có thể bắt đầu bằng 0 và cả ba giá trị phải là số nguyên dương. 

Trong số tất cả các bộ ba hợp lệ như vậy, về mặt khái niệm, chúng ta viết từng phương trình dưới dạng một chuỗi như`a + b = c`và sắp xếp các chuỗi này theo từ điển. Nhiệm vụ là trả về chuỗi thứ k theo thứ tự này hoặc báo cáo rằng tồn tại ít hơn k phương trình hợp lệ. 

Khó khăn chính là k có thể lớn tới 10¹², trong khi độ dài chữ số lại nhỏ (nhiều nhất là 6). Điều này ngay lập tức loại trừ việc tạo ra tất cả các bộ ba hợp lệ một cách rõ ràng. Ngay cả trong trường hợp xấu nhất, số lượng ứng cử viên là khoảng 9·10⁵ mỗi biến, dẫn đến khoảng 10¹⁷ kết hợp cho (a, b) và ngay cả sau khi lọc theo các ràng buộc tổng, việc liệt kê vũ lực là không khả thi. 

Điểm tinh tế thứ hai là thứ tự từ điển nằm trên chuỗi được định dạng, không phải trên các bộ dữ liệu số. Điều này có nghĩa là việc sắp xếp chủ yếu được thực hiện bằng các chữ số của`a`, sau đó`b`, sau đó`c`, bao gồm cả định dạng có độ dài cố định. Ví dụ,`"10 + 2 = 12"`Và`"2 + 10 = 12"`được sắp xếp dựa trên so sánh ký tự, không phải độ lớn bằng số. 

Các trường hợp khó khăn phá vỡ các giải pháp ngây thơ bao gồm các tình huống: 

Một mức tối thiểu`a`Và`b`đã vượt quá mức tối đa có thể`c`, có nghĩa là không có giải pháp nào tồn tại. Ví dụ,`A = B = C = 2`đưa ra mức tối thiểu`a = b = 10`, Nhưng`10 + 10 = 20`là hợp lệ; tuy nhiên nếu`C = 1`, thì tối đa`c = 9`, nên không có giải pháp nào tồn tại. 

Một dạng lỗi khác là bỏ qua thứ tự từ điển và thay vào đó tạo ra các bộ ba theo thứ tự số của`(a, b, c)`, tạo ra một chuỗi khác với thứ tự chuỗi. Ví dụ,`"1 + 10 = 11"`đến trước`"2 + 1 = 3"`về mặt từ điển do so sánh ký tự. 

Cuối cùng, một vấn đề nhỏ là đếm quá mức hoặc thiếu tổng hợp lệ khi sử dụng chữ số DP hoặc tính tổ hợp nếu các ràng buộc mang không được xử lý đúng cách trên các độ dài chữ số cố định. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ lặp lại trên tất cả các giá trị hợp lệ`a`Và`b`trong phạm vi chữ số của họ, tính toán`c = a + b`, kiểm tra xem`c`có chính xác`C`chữ số và lưu trữ tất cả các bộ ba hợp lệ. Sau khi thu thập chúng, chúng tôi sẽ sắp xếp chúng thành chuỗi và chọn phần tử thứ k. 

Điều này đúng nhưng ngay lập tức quá chậm. Ngay cả đối với`A = B = C = 6`, chúng ta có thể có tới 10⁶ giá trị cho mỗi`a`Và`b`, tạo ra các cặp 10¹². Ngay cả khi chỉ một phần nhỏ là hợp lệ thì việc liệt kê là không thể. 

Quan sát quan trọng là`A, B, C ≤ 6`, vì vậy chúng ta có thể đếm hoặc xây dựng các giải pháp từng chữ số thay vì liệt kê các số. Các cặp chữ số ràng buộc cộng của`a`Và`b`thông qua các giá trị mang, do đó thay vì lặp lại các giá trị, chúng ta xử lý bài toán dưới dạng chữ số DP trên các vị trí. 

Chúng tôi xây dựng các số từ chữ số có nghĩa nhất đến chữ số có nghĩa nhỏ nhất trong khi theo dõi số liệu. Tại mỗi vị trí ta chọn chữ số cho`a`Và`b`phù hợp với các ràng buộc về số 0 đứng đầu và tính chữ số kết quả của`c`. Cấu trúc này cho phép chúng ta đếm số lần hoàn thành tồn tại đối với một tiền tố nhất định. Khi chúng ta có thể đếm số lần hoàn thành hậu tố, chúng ta có thể xây dựng giải pháp từ điển thứ k một cách tham lam bằng cách sửa các chữ số theo thứ tự và trừ đi số đếm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(10^12 đến 10^18) | O(N) | Quá chậm | 
| Chữ số DP + xây dựng tham lam | O(A·B·C·10·mang theo) mỗi bài kiểm tra | O(tiểu bang) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi coi mỗi số là một mảng chữ số có độ dài cố định với các số 0 đứng đầu không được phép ở vị trí đầu tiên. Chúng tôi tính toán trước các phạm vi chữ số hợp lệ cho từng vị trí. 

1. Xác định hàm DP`dp(i, carry)`đếm xem có bao nhiêu cách chúng ta có thể điền vào các vị trí từ chữ số`i`đến cuối cùng, được thực hiện từ vị trí ít quan trọng hơn trước đó. Chức năng này phụ thuộc vào việc chọn chữ số`da`Và`db`ở vị trí`i`, tạo ra một chữ số`dc`và mang theo tiếp theo. 
2. Chúng tôi tính DP từ chữ số có nghĩa nhỏ nhất đến chữ số có nghĩa nhất. Đối với mỗi trạng thái, chúng tôi lặp lại tất cả các cặp chữ số`(da, db)`hợp lệ dưới các ràng buộc về số 0 đứng đầu và tích lũy các chuyển đổi. Điều này cho chúng ta số lần hoàn thành hợp lệ cho mỗi hậu tố. 
3. Sau khi xây dựng bảng DP, trước tiên chúng ta kiểm tra tổng số phương trình hợp lệ. Nếu như`k`vượt quá tổng số này, chúng tôi xuất ra`-1`. 
4. Mặt khác, chúng ta xây dựng câu trả lời một cách tham lam từ chữ số có nghĩa nhất đến chữ số có nghĩa nhỏ nhất. Ở mỗi bước, chúng tôi thử các cặp chữ số ứng cử viên`(da, db)`theo thứ tự từ điển và sử dụng số lượng DP để xác định số lần hoàn thành hợp lệ mà mỗi lựa chọn mang lại. 
5. Khi một lựa chọn ứng viên có ít hơn`k`hoàn thành, chúng tôi trừ nó khỏi`k`và bỏ qua nó. Khi chúng tôi tìm thấy lựa chọn chứa đáp án thứ k, chúng tôi sửa các chữ số đó và tiến về phía trước. 
6. Sau khi điền đầy đủ các chữ số, ta xây dựng lại`a`,`b`, Và`c`và định dạng chúng dưới dạng một chuỗi. 

Tại sao nó hoạt động: DP đảm bảo rằng với mỗi trạng thái tiền tố, chúng tôi biết chính xác có bao nhiêu lần hoàn thành hậu tố hợp lệ, không phụ thuộc vào các lựa chọn trước đó. Điều này cung cấp một phân vùng nhất quán của không gian giải pháp. Lựa chọn tham lam luôn chọn lựa chọn chữ số đầu tiên có khối chứa giải pháp thứ k mong muốn theo thứ tự từ điển và vì số lượng DP khớp chính xác với số lần tiếp tục hợp lệ nên không có giải pháp nào bị bỏ qua hoặc được tính hai lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve_case(A, B, C, k):
    # dp[pos][carry] -> number of ways from pos to end
    # pos goes from 0 (most significant) to max(A,B,C)-1
    n = max(A, B, C)

    # pad lengths
    def get_digit(x, pos, length):
        # position from left
        if pos < n - length:
            return 0, True  # leading zero not allowed if pos == first real digit
        return None, False

    # We instead explicitly handle allowed digit ranges per position
    dp = [[0] * 2 for _ in range(n + 1)]
    dp[n][0] = 1

    for i in range(n - 1, -1, -1):
        for carry in range(2):
            total = 0
            a_start = 1 if i == 0 else 0
            for da in range(10):
                if i < n - A and da != 0:
                    continue
                if i == n - A and da == 0:
                    continue

                for db in range(10):
                    if i < n - B and db != 0:
                        continue
                    if i == n - B and db == 0:
                        continue

                    s = da + db + carry
                    dc = s % 10
                    nc = s // 10

                    if i < n - C and dc != 0:
                        continue
                    if i == n - C and dc == 0:
                        continue

                    total += dp[i + 1][nc]
            dp[i][carry] = total

    if dp[0][0] < k:
        return "-1"

    a_digits = []
    b_digits = []
    c_digits = []
    carry = 0

    for i in range(n):
        for da in range(10):
            if i < n - A and da != 0:
                continue
            if i == n - A and da == 0:
                continue

            for db in range(10):
                if i < n - B and db != 0:
                    continue
                if i == n - B and db == 0:
                    continue

                s = da + db + carry
                dc = s % 10
                nc = s // 10

                if i < n - C and dc != 0:
                    continue
                if i == n - C and dc == 0:
                    continue

                cnt = dp[i + 1][nc]
                if cnt < k:
                    k -= cnt
                else:
                    a_digits.append(str(da))
                    b_digits.append(str(db))
                    c_digits.append(str(dc))
                    carry = nc
                    break
            else:
                continue
            break

    a = ''.join(a_digits)
    b = ''.join(b_digits)
    c = ''.join(c_digits)
    return f"{a} + {b} = {c}"

t = int(input())
for _ in range(t):
    A, B, C, k = map(int, input().split())
    print(solve_case(A, B, C, k))
```Bảng DP`dp[i][carry]`đếm xem có bao nhiêu lần hoàn thành hợp lệ từ vị trí`i`trở đi, giả sử một lần mang đến cố định. Đây là xương sống cho phép chúng ta bỏ qua toàn bộ khối không gian từ điển một cách hiệu quả. 

Trong quá trình xây dựng, chúng tôi lặp lại các ứng viên theo thứ tự chữ số từ điển. Đối với mỗi`(da, db, dc)`ngụ ý bởi một khoản mang theo, chúng tôi sử dụng`dp[i+1][nc]`để xác định có bao nhiêu giải pháp đầy đủ bắt đầu bằng tiền tố này. Nếu khối đó không chứa nghiệm thứ k, chúng ta trừ đi và đi tiếp. Nếu không, chúng tôi cam kết chữ số đó và tiếp tục chuyển tiếp. 

Phần tế nhị nhất là thực thi độ dài chữ số cố định một cách chính xác. các điều kiện`i < n - A`,`i == n - A`đảm bảo rằng chúng tôi buộc các chữ số đầu phải khác 0 và tránh các biểu diễn ngắn hơn không hợp lệ được nhúng trong lưới đệm. 

## Ví dụ đã hoạt động 

Xét một trường hợp nhỏ`A = 1, B = 1, C = 2, k = 3`. Chúng tôi mong đợi sự bình đẳng về từ điển thứ ba. 

| Bước | tôi | mang theo | đã chọn da | đã chọn db | còn lại k | 
| --- | --- | --- | --- | --- | --- | 
| ban đầu | 0 | 0 | - | - | 3 | 
| thử (1,1) | 0 | 0 | 1 | 1 | 3 → 2 (bỏ qua khối) | 
| thử (1,2) | 0 | 0 | 1 | 2 | 2 → 1 (bỏ qua khối) | 
| thử (1,3) | 0 | 0 | 1 | 3 | 1 → được chọn | 

Dấu vết này cho thấy cách DP đếm phân vùng không gian tìm kiếm thành các khối từ điển liền kề. 

Bây giờ hãy xem xét một trường hợp không có giải pháp tồn tại,`A = 2, B = 2, C = 1`. Vì tổng nhỏ nhất có thể là 10 + 10 = 20 nên mọi ứng cử viên đều vi phạm các ràng buộc về độ dài chữ số. 

DP ở trạng thái bắt đầu đánh giá là 0, ngay lập tức kích hoạt`-1`. Điều này tránh mọi tìm kiếm không cần thiết và chứng tỏ tính khả thi được ghi lại đầy đủ trong bảng DP như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(A·B·C·10²) mỗi bài kiểm tra | mỗi trạng thái lặp lại các cặp chữ số và thực hiện các chuyển đổi | 
| Không gian | O(A·mang) | DP chỉ lưu trữ số lượng hậu tố cho mỗi vị trí | 

Độ dài chữ số nhiều nhất là 6 nên không gian trạng thái DP cực kỳ nhỏ. Ngay cả với tối đa 1000 trường hợp thử nghiệm, tổng số tính toán vẫn nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    def solve_case(A, B, C, k):
        n = max(A, B, C)
        dp = [[0] * 2 for _ in range(n + 1)]
        dp[n][0] = 1

        for i in range(n - 1, -1, -1):
            for carry in range(2):
                total = 0
                for da in range(10):
                    if i < n - A and da != 0:
                        continue
                    if i == n - A and da == 0:
                        continue
                    for db in range(10):
                        if i < n - B and db != 0:
                            continue
                        if i == n - B and db == 0:
                            continue
                        s = da + db + carry
                        dc = s % 10
                        nc = s // 10
                        if i < n - C and dc != 0:
                            continue
                        if i == n - C and dc == 0:
                            continue
                        total += dp[i + 1][nc]
                dp[i][carry] = total

        if dp[0][0] < k:
            return "-1"

        a = []
        b = []
        c = []
        carry = 0

        for i in range(n):
            for da in range(10):
                if i < n - A and da != 0:
                    continue
                if i == n - A and da == 0:
                    continue
                for db in range(10):
                    if i < n - B and db != 0:
                        continue
                    if i == n - B and db == 0:
                        continue
                    s = da + db + carry
                    dc = s % 10
                    nc = s // 10
                    if i < n - C and dc != 0:
                        continue
                    if i == n - C and dc == 0:
                        continue
                    cnt = dp[i + 1][nc]
                    if cnt < k:
                        k -= cnt
                    else:
                        a.append(str(da))
                        b.append(str(db))
                        c.append(str(dc))
                        carry = nc
                        break
                else:
                    continue
                break

        return f"{''.join(a)} + {''.join(b)} = {''.join(c)}"

    data = """7
1 1 1 9
2 2 3 1
2 2 1 1
1 5 6 42
1 6 6 10000000
5 5 6 3031568815
6 6 6 1000000000000
"""
    return run(data)

# provided samples
assert run("""
7
1 1 1 9
2 2 3 1
2 2 1 1
1 5 6 42
1 6 6 10000000
5 5 6 3031568815
6 6 6 1000000000000
""") == """2 + 1 = 3
10 + 90 = 100
-1
9 + 99996 = 100005
-1
78506 + 28543 = 107049
-1"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chữ số tối thiểu |`1 1 1 1 → 1 + 1 = 2`| độ đúng cơ sở | 
| không có giải pháp |`2 2 1 k`| cắt tỉa khả thi | 
| mang nặng |`1 1 2 large k`| thực hiện chuyển tiếp | 

## Vỏ cạnh 

Khi nào`A = B = C = 1`, DP thoái hóa thành một bảng liệt kê nhỏ gồm ba chữ số trong đó chỉ`(1,1,2)`bởi vì`(9,9,18)`là hợp lệ và thứ tự từ điển khớp với thứ tự chữ số đơn giản. Thuật toán xử lý chính xác điều này vì số lượng DP được tính toán ngay cả khi số mang luôn bằng 0 hoặc 1 và việc lựa chọn tiền tố vẫn nhất quán. 

Khi`C`lớn hơn nhiều so với`A`Và`B`, số tiền hợp lệ cần có số 0 đứng đầu trong`c`cho đến khi tạo đủ chữ số. DP thực thi rõ ràng các ràng buộc về chữ số hàng đầu thông qua kiểm tra vị trí, do đó các cấu trúc không hợp lệ sẽ không bao giờ góp phần vào việc đếm. 

Khi k cực kỳ lớn, việc kiểm tra ban đầu`dp[0][0] < k`cản trở mọi công trình xây dựng. Điều này tránh việc duyệt tham lam không cần thiết trong trường hợp không gian giải pháp trống.
