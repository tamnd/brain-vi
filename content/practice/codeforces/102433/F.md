---
title: "CF 102433F - Nhà ảo thuật Carny"
description: "Chúng ta cần sắp xếp các số từ (1) đến (n) thành một hoán vị. Vị trí (i) được gọi là cố định khi giá trị đặt ở đó cũng là (i). Trong số tất cả các hoán vị có chính xác (m) vị trí cố định, chúng ta phải xuất ra hoán vị thứ (k) theo thứ tự từ điển."
date: "2026-08-14T15:36:35+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102433
codeforces_index: "F"
codeforces_contest_name: "2019-2020 ACM-ICPC Pacific Northwest Regional Contest (Div. 1)"
rating: 0
weight: 102433
solve_time_s: 137
verified: true
draft: false
---

[CF 102433F - Nhà ảo thuật Carny](https://codeforces.com/problemset/problem/102433/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 17s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta cần sắp xếp các số từ (1) đến (n) thành một hoán vị. Vị trí (i) được gọi là cố định khi giá trị đặt ở đó cũng là (i). Trong số tất cả các hoán vị có chính xác (m) vị trí cố định, chúng ta phải xuất ra hoán vị thứ (k) theo thứ tự từ điển. Nếu tồn tại ít hơn (k) hoán vị hợp lệ thì câu trả lời là (-1). 

Các ràng buộc nhỏ trong (n), với (n\le 50), nhưng số lượng hoán vị là rất lớn. Ngay cả ở (n=50), vẫn có (50!\approx3.04\cdot10^{64}) hoán vị, vì vậy bất kỳ phương pháp nào liệt kê các hoán vị đều hoàn toàn không khả thi. Thứ hạng (k) có thể đạt tới (10^{18}), nghĩa là số nguyên 64 bit thông thường là đủ cho thứ hạng đầu vào, nhưng các giá trị đếm trung gian có thể lớn hơn nhiều. Chúng ta chỉ cần phân biệt số lượng bên dưới (k) với số lượng ít nhất (k), vì vậy tất cả các giá trị DP có thể được giới hạn một cách an toàn ở mức (10^{18}). 

Có một số trường hợp khó khăn mà việc xây dựng trực tiếp có thể xử lý sai. Đối với đầu vào`1 0 1`, hoán vị duy nhất là`[1]`, nhưng nó có một điểm cố định, nên kết quả đúng là`-1`. Một cấu trúc chỉ điền vào giá trị sẵn có duy nhất sẽ chấp nhận nó một cách không chính xác. 

Đối với đầu vào`3 2 1`, số điểm cố định mong muốn là hai. Nếu hai vị trí cố định thì vị trí cuối cùng cũng buộc phải chứa giá trị riêng của nó, do đó không thể có chính xác hai điểm cố định. Đầu ra đúng là`-1`. Phương pháp chọn các vị trí cố định một cách độc lập mà không tính các cách sắp xếp còn lại có thể mắc phải sai lầm này. 

Đối với đầu vào`4 0 3`, tất cả các điểm cố định đều bị cấm. Có chín sai lệch, và sai lệch thứ ba nhỏ nhất về mặt từ điển là`2 4 1 3`. Một quy tắc tham lam chỉ tránh đặt (i) vào vị trí (i) không biết liệu các giá trị còn lại có còn có thể hoàn thành thành một loạn trí hay không, nên nó có thể chọn một tiền tố mà sau này trở thành không thể. 

Trường hợp cạnh cuối cùng là (m=n). Có chính xác một hoán vị hợp lệ, hoán vị danh tính. Ví dụ,`4 4 1`phải sản xuất`1 2 3 4`, trong khi`4 4 2`phải sản xuất`-1`. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là tạo ra mọi hoán vị theo thứ tự từ điển, đếm các điểm cố định của nó và dừng khi tìm thấy điểm hợp lệ (k). Điều này đúng vì thứ tự được tạo chính xác là thứ tự được yêu cầu, nhưng trong trường hợp xấu nhất, chúng tôi kiểm tra tất cả (n!) hoán vị và kiểm tra (n) vị trí trong mỗi hoán vị. Công việc trong trường hợp xấu nhất là (O(n\cdot n!)), với (n=50) là khoảng (50\cdot50!\approx1.52\cdot10^{66}) kiểm tra vị trí. Ngay cả việc tạo ra các hoán vị thôi cũng là không thể. 

Quan sát hữu ích là tương lai không phụ thuộc vào danh tính chính xác của tất cả các giá trị còn lại. Điều quan trọng là có bao nhiêu vị trí còn lại vẫn còn giá trị riêng. 

Giả sử còn lại (các) vị trí và (các) giá trị không được sử dụng. Gọi một vị trí có thể so khớp nếu số của chính nó nằm trong số các giá trị không được sử dụng. Gọi (a) là số vị trí có thể so sánh được. Với mục đích tính số lần hoàn thành với số điểm cố định được chỉ định, mọi trạng thái có cùng (các) điểm, (a) và số điểm cố định được yêu cầu là tương đương. Các nhãn thực tế không quan trọng. 

Xác định (dp[s][a][r]) là số cách để hoàn thành trạng thái đó bằng cách sử dụng (các) vị trí, trong đó chính xác (a) vị trí có sẵn giá trị riêng và chính xác (r) trong số các vị trí đó phải cố định. 

Để rút ra sự truy hồi, hãy chọn một vị trí phù hợp cụ thể. Giá trị của nó có thể được gán theo ba cách. Nó có thể nhận giá trị riêng của mình, tạo một điểm cố định và giảm (a) đi một. Nó có thể nhận một giá trị có vị trí tương ứng không nằm trong số các vị trí còn lại. Có (s-a) các giá trị như vậy và việc loại bỏ chúng sẽ làm giảm (a) một. Cuối cùng, nó có thể nhận giá trị thuộc về một vị trí phù hợp khác. Có (a-1) những lựa chọn như vậy và việc loại bỏ cả vị trí đã chọn và giá trị đó sẽ làm giảm (a) đi hai. 

Điều này mang lại 

dp[s-1][a-1][r-1] 
+ 
(s-a),dp[s-1][a-1][r] 
+ 
(a-1),dp[s-1][a-2][r]. 
] 

Khi (a=0), không vị trí còn lại nào có thể cố định được, do đó (dp[s][0][0]=s!), while (dp[s][0][r]=0) cho (r>0). 

Sau đó, DP tương tự có thể hướng dẫn việc xây dựng từ điển. Ở mọi vị trí, hãy thử các giá trị không sử dụng từ nhỏ nhất đến lớn nhất. Đối với mỗi ứng viên, hãy tính giá trị mới của (a), sau đó hỏi DP xem còn lại bao nhiêu lần hoàn thành hợp lệ. Nếu số đó ít nhất là (k), ứng viên thuộc về hoán vị mong muốn. Mặt khác, mọi hoán vị bắt đầu với ứng cử viên đó đều đứng trước câu trả lời, vì vậy chúng tôi trừ số đếm khỏi (k) và thử ứng viên tiếp theo. 

Phương pháp vũ lực hoạt động vì nó liệt kê rõ ràng mọi khả năng tiếp tục có thể xảy ra, nhưng nó không thành công vì có rất nhiều giai đoạn trong số đó. Quan sát cho thấy số lần hoàn thành chỉ phụ thuộc vào (s), (a) và (r) nén tất cả các phần tiếp theo đó thành DP có kích thước đa thức. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(n\cdot n!)) | (O(n)) | Quá chậm | 
| Tối ưu | (O(n^3+n^3\log n)) | (O(n^3)) | Đã chấp nhận | 

Hệ số logarit trong cách xây dựng xuất phát từ việc sắp xếp liên tục tối đa 50 giá trị không sử dụng. Nó cũng có thể tránh được bằng một cấu trúc có trật tự được duy trì, nhưng nó không liên quan ở kích thước ràng buộc này. 

## Hướng dẫn thuật toán 

1. Xây dựng bảng DP (dp[s][a][r]). Trạng thái cơ sở là (dp[0][0][0]=1). Với mọi trạng thái có (a=0), đặt (dp[s][0][0]=s!). Với (a>0), hãy sử dụng phép truy toán ba trường hợp ở trên. Mỗi số lượng được giới hạn ở (10^{18}), vì các giá trị lớn hơn không thể phân biệt được nhằm mục đích so sánh với (k). 
2. Bắt đầu với tất cả các vị trí và tất cả giá trị chưa được sử dụng. Ban đầu (s=n), (a=n), vì mọi vị trí vẫn có sẵn giá trị riêng. Tổng số hoán vị hợp lệ là (dp[n][n][m]). Nếu nhỏ hơn (k) thì in ngay`-1`. 
3. Xử lý các vị trí từ (1) đến (n). Tại vị trí (i), xem xét mọi giá trị hiện không được sử dụng (x) theo thứ tự tăng dần. Thứ tự này chính xác là những gì việc xếp hạng từ điển yêu cầu. 
4. Xác định xem việc chọn (x) có tạo ra điểm cố định hay không. Nó hoạt động chính xác khi (x=i). Số điểm cố định vẫn cần thiết sẽ trở thành (m-[x=i]). 
5. Tính số vị trí khớp mới sau khi loại bỏ vị trí (i) và giá trị (x). Nếu (x=i), một vị trí có thể so khớp biến mất, do đó số đếm mới là (a-1). Mặt khác, việc xóa vị trí (i) sẽ xóa chính xác một vị trí có thể khớp khi (i) vẫn là giá trị không được sử dụng và xóa giá trị (x) sẽ xóa chính xác một vị trí có thể khớp khi vị trí (x) vẫn chưa được xử lý. 
6. Ứng viên rời khỏi vị trí (n-i). Truy vấn DP với các vị trí còn lại đó, số lượng khớp mới được tính toán và số điểm cố định bắt buộc còn lại. Số lượng này chính xác là số hoán vị hợp lệ có tiền tố bằng với tiền tố hiện tại theo sau là (x). 
7. Nếu số đếm đó nhỏ hơn (k), hãy bỏ qua toàn bộ khối từ điển này và trừ số đếm từ (k). Ngược lại, xác nhận (x), xóa (i) và (x) khỏi các tập hợp còn lại, cập nhật (a) và (m) và tiếp tục với vị trí tiếp theo. 
8. Sau khi tất cả các vị trí đã được xử lý, chuỗi được xây dựng là hoán vị hợp lệ thứ (k). 

Điều bất biến là trước khi xử lý vị trí (i), (k) là thứ hạng của câu trả lời mong muốn trong số tất cả các lần hoàn thành hợp lệ của tiền tố hiện tại và (dp) tính chính xác những lần hoàn thành đó cho mỗi lần tiếp tục ứng cử viên. Khi một khối ứng cử viên bị bỏ qua, toàn bộ khối của nó đứng trước câu trả lời, do đó, việc trừ đi kích thước của nó sẽ giữ nguyên bất biến. Khi một ứng cử viên được chấp nhận, câu trả lời phải nằm bên trong khối đó, do đó, bất biến tương tự cũng đúng đối với trạng thái nhỏ hơn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

LIMIT = 10**18

def build_dp(n):
    # dp[s][a][r]:
    # number of completions with s positions left,
    # a matchable positions, and exactly r fixed points.
    dp = [[[0] * (n + 1) for _ in range(n + 1)]
          for _ in range(n + 1)]

    dp[0][0][0] = 1

    fact = [1] * (n + 1)
    for i in range(1, n + 1):
        fact[i] = min(LIMIT, fact[i - 1] * i)

    for s in range(1, n + 1):
        dp[s][0][0] = fact[s]

    for s in range(1, n + 1):
        for a in range(1, s + 1):
            for r in range(0, a + 1):
                value = 0

                # The chosen matchable position is fixed.
                if r >= 1:
                    value += dp[s - 1][a - 1][r - 1]

                # It receives a value whose position is not matchable.
                if s - a > 0:
                    value += (s - a) * dp[s - 1][a - 1][r]

                # It receives the value of another matchable position.
                if a >= 2:
                    value += (a - 1) * dp[s - 1][a - 2][r]

                dp[s][a][r] = min(LIMIT, value)

    return dp

def solve_instance(n, m, k):
    dp = build_dp(n)

    if dp[n][n][m] < k:
        return "-1"

    remaining_positions = set(range(1, n + 1))
    remaining_values = set(range(1, n + 1))

    a = n
    answer = []

    for pos in range(1, n + 1):
        s = n - pos + 1

        chosen = False

        for x in sorted(remaining_values):
            fixed = (x == pos)
            remaining_fixed = m - fixed

            if x == pos:
                new_a = a - 1
            else:
                new_a = a
                if pos in remaining_values:
                    new_a -= 1
                if x in remaining_positions:
                    new_a -= 1

            if remaining_fixed < 0:
                count = 0
            else:
                count = dp[s - 1][new_a][remaining_fixed]

            if count < k:
                k -= count
                continue

            answer.append(x)
            remaining_positions.remove(pos)
            remaining_values.remove(x)

            a = new_a
            m = remaining_fixed
            chosen = True
            break

        if not chosen:
            return "-1"

    return " ".join(map(str, answer))

def main():
    n, m, k = map(int, input().split())
    print(solve_instance(n, m, k))

if __name__ == "__main__":
    main()
```các`build_dp`Hàm thực hiện trạng thái được mô tả trong thuật toán. Sự riêng biệt`a=0`việc khởi tạo là cần thiết vì phép lặp sẽ chọn một vị trí có thể so khớp, vị trí này không tồn tại ở trạng thái đó. Các giá trị giai thừa biểu thị tất cả các hoán vị không hạn chế khi không thể có điểm cố định trong tương lai. 

Ba số hạng trong phép truy hồi tương ứng trực tiếp với ba đích đến có thể có của vị trí có thể so khớp đã chọn. Các hệ số nhân đếm có bao nhiêu giá trị thuộc về mỗi loại. Bảng chỉ cần các chỉ số tối đa (n) nên kích thước của nó nhỏ. 

các`LIMIT`cap ngăn chặn sự tăng trưởng không cần thiết của số nguyên Python. Số lượng hoán vị thực tế có thể lớn tới (50!), nhưng câu hỏi duy nhất được đặt ra trong quá trình xây dựng là liệu một khối có chứa ít hơn (k) hoán vị hay không. Vì (k\le10^{18}), việc thay thế mọi số lượng lớn hơn bằng (10^{18}) sẽ bảo toàn mọi quyết định. 

Trong quá trình xây dựng,`remaining_positions`Và`remaining_values`đại diện cho trạng thái chính xác. Biến`a`lưu trữ số lượng vị trí mà giá trị riêng của nó vẫn chưa được sử dụng. Biểu thức cho`new_a`tài khoản riêng biệt để loại bỏ vị trí hiện tại và loại bỏ giá trị ứng cử viên. Khi`x == pos`, cả hai lần loại bỏ đều liên quan đến cùng một cặp có thể so khớp, vì vậy nó chỉ được trừ một lần. 

Sự so sánh là có chủ ý`count < k`còn hơn là`count <= k`. Nếu một khối ứng cử viên chứa chính xác (k) sự hoàn thành hợp lệ thì hoán vị mong muốn nằm bên trong khối đó và ứng cử viên phải được chọn. Đây là lỗi thường gặp nhất trong cấu trúc từ điển thứ (k)-th. 

Số nguyên Python không bị tràn, nhưng giới hạn rõ ràng giữ cho giá trị DP ở mức nhỏ và làm cho ngữ nghĩa so sánh dự kiến ​​trở nên rõ ràng. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đối với đầu vào`3 1 1`, chúng ta cần một điểm cố định và muốn hoán vị hợp lệ đầu tiên. 

| Vị trí | Ứng viên | Các vị trí còn lại | Có thể so khớp (a) | Điểm cố định cần thiết | Hoàn thành | Hành động | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 2 | 2 | 0 | 1 | Chọn | 
| 2 | 2 | 1 | 1 | -1 | 0 | Bỏ qua | 
| 2 | 3 | 1 | 0 | 0 | 1 | Chọn | 
| 3 | 2 | 0 | 0 | 0 | 1 | Chọn | 

Ban đầu có (dp[3][3][1]=3) hoán vị hợp lệ. Giá trị đầu tiên nhỏ nhất có thể là`1`. Nếu chúng ta chọn thì hai vị trí còn lại không được chứa thêm điểm cố định nào và việc hoàn thành duy nhất là`1 3 2`. Vì khối đó chứa hoán vị mong muốn đầu tiên nên chúng tôi chọn`1`. 

Ở vị trí 2, chọn`2`sẽ tạo điểm cố định thứ hai, do đó số lần hoàn thành của nó bằng 0. Lựa chọn`3`để lại sự hoàn thành duy nhất có thể`2`ở vị trí cuối cùng. Kết quả là`1 3 2`. 

### Mẫu 2 

Đối với đầu vào`3 2 1`, trạng thái ban đầu có ba vị trí có thể khớp và yêu cầu chính xác hai điểm cố định. 

| Tiểu bang | (các) | (a) | Điểm cố định bắt buộc | Đếm | 
| --- | --- | --- | --- | --- | 
| Ban đầu | 3 | 3 | 2 | 0 | 

Số đếm bằng 0 vì việc chọn chính xác hai điểm cố định từ hoán vị ba phần tử là không thể. Khi hai vị trí đã được cố định, giá trị còn lại sẽ bị ép vào vị trí còn lại, tạo ra điểm cố định thứ ba. 

Vì số đếm ban đầu đã nhỏ hơn (k=1), nên thuật toán sẽ in`-1`mà không cố gắng xây dựng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n^3+n^3\log n)) | DP có trạng thái (O(n^3)) và mỗi trạng thái thực hiện công việc không đổi. Bộ phận xây dựng thử tối đa (n) ứng viên ở mỗi (n) vị trí, sắp xếp tối đa (n) giá trị mỗi lần. | 
| Không gian | (O(n^3)) | Bảng DP có (O(n^3)) mục nhập, trong khi trạng thái xây dựng sử dụng (O(n)) không gian bổ sung. | 

Với (n\le50), DP chỉ chứa khoảng (51^3) mục và việc xây dựng kiểm tra tối đa vài nghìn trạng thái ứng cử viên. Điều này dễ dàng đủ nhỏ cho giới hạn một giây, trong khi lực lượng vũ phu được tách ra khỏi phạm vi khả thi bởi hàng chục bậc độ lớn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

LIMIT = 10**18

def build_dp(n):
    dp = [[[0] * (n + 1) for _ in range(n + 1)]
          for _ in range(n + 1)]

    dp[0][0][0] = 1

    fact = [1] * (n + 1)
    for i in range(1, n + 1):
        fact[i] = min(LIMIT, fact[i - 1] * i)

    for s in range(1, n + 1):
        dp[s][0][0] = fact[s]

    for s in range(1, n + 1):
        for a in range(1, s + 1):
            for r in range(a + 1):
                value = 0

                if r >= 1:
                    value += dp[s - 1][a - 1][r - 1]

                if s - a > 0:
                    value += (s - a) * dp[s - 1][a - 1][r]

                if a >= 2:
                    value += (a - 1) * dp[s - 1][a - 2][r]

                dp[s][a][r] = min(LIMIT, value)

    return dp

def solve_instance(n, m, k):
    dp = build_dp(n)

    if dp[n][n][m] < k:
        return "-1"

    remaining_positions = set(range(1, n + 1))
    remaining_values = set(range(1, n + 1))

    a = n
    answer = []

    for pos in range(1, n + 1):
        s = n - pos + 1

        for x in sorted(remaining_values):
            fixed = (x == pos)
            remaining_fixed = m - fixed

            if x == pos:
                new_a = a - 1
            else:
                new_a = a
                if pos in remaining_values:
                    new_a -= 1
                if x in remaining_positions:
                    new_a -= 1

            count = 0
            if remaining_fixed >= 0:
                count = dp[s - 1][new_a][remaining_fixed]

            if count < k:
                k -= count
                continue

            answer.append(x)
            remaining_positions.remove(pos)
            remaining_values.remove(x)

            a = new_a
            m = remaining_fixed
            break

    return " ".join(map(str, answer))

def run(inp: str) -> str:
    n, m, k = map(int, inp.split())
    return solve_instance(n, m, k)

# Provided samples
assert run("3 1 1") == "1 3 2", "sample 1"
assert run("3 2 1") == "-1", "sample 2"
assert run("5 3 7") == "2 1 3 4 5", "sample 3"

# Minimum size, but the requested fixed-point count is impossible.
assert run("1 0 1") == "-1", "single element with zero fixed points"

# Minimum size with the only possible fixed-point count.
assert run("1 1 1") == "1", "single element identity"

# Third lexicographically smallest derangement of size 4.
assert run("4 0 3") == "2 4 1 3", "derangement ranking"

# Maximum n, all positions fixed.
assert run("50 50 1") == " ".join(map(str, range(1, 51))), "maximum size identity"

# n - 1 fixed points are impossible for n > 1.
assert run("4 3 1") == "-1", "n-1 fixed points"

# Smallest derangement of size 2.
assert run("2 0 1") == "2 1", "two-element derangement"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 0 1`|`-1`| Kích thước tối thiểu và mục tiêu điểm cố định không thể | 
|`1 1 1`|`1`| Kích thước tối thiểu với hoán vị hợp lệ duy nhất | 
|`4 0 3`|`2 4 1 3`| (k)-thứ hạng trong số các loạn trí | 
|`50 50 1`|`1 2 3 ... 50`| Tối đa (n), tất cả các vị trí cố định | 
|`4 3 1`|`-1`| Điểm cố định không thể (n-1) | 
|`2 0 1`|`2 1`| Sự xáo trộn không đáng kể nhỏ nhất và chuyển tiếp ranh giới | 

## Vỏ cạnh 

cho`1 0 1`, trạng thái ban đầu là (s=1,a=1,r=0). Giá trị duy nhất có thể là`1`, nhưng việc chọn nó sẽ tạo ra một điểm cố định, để lại số điểm cố định bắt buộc là (-1). Số lần hoàn thành của nó bằng 0, do đó trạng thái ban đầu không có hoán vị hợp lệ và kết quả đầu ra của thuật toán`-1`. 

Vì`3 2 1`, DP bắt đầu tại (dp[3][3][2]). Không có hoán vị của ba phần tử với đúng hai điểm cố định, vì vậy mục này bằng 0. Thuật toán từ chối toàn bộ phiên bản trước khi xây dựng tiền tố và xuất ra`-1`. 

Vì`4 4 1`, trạng thái ban đầu là (dp[4][4][4]=1). Mọi vị trí đều phải cố định nên ứng cử viên đầu tiên`1`được chọn thì`2`, sau đó`3`, sau đó`4`. Kết quả là`1 2 3 4`. Thay vào đó, nếu đầu vào yêu cầu`4 4 2`, số đếm ban đầu là một, nhỏ hơn (k=2), do đó thuật toán đưa ra`-1`. 

Vì`4 3 1`, mục tiêu ít hơn một so với số lượng vị trí. Nếu ba vị trí đầu tiên được cố định thì giá trị cuối cùng không được sử dụng nhất thiết phải là`4`, tạo một điểm cố định khác. DP nắm bắt sự phụ thuộc này thay vì xử lý các vị trí cố định một cách độc lập, do đó (dp[4][4][3]=0) và thuật toán sẽ in chính xác`-1`. 

Vì`4 0 3`, việc thi công phải tránh mọi điểm cố định. Ứng cử viên đầu tiên`1`bị từ chối vì nó ngay lập tức tạo ra một điểm cố định. Tiền tố hợp lệ đầu tiên là`2`và sau đó DP so sánh các lần hoàn thành hợp lệ bắt đầu bằng`2`theo thứ tự từ điển. Ba cái đầu tiên là`2 1 4 3`,`2 3 4 1`, Và`2 4 1 3`, vậy đáp án thứ ba là`2 4 1 3`. Điều này chứng tỏ tại sao việc tính số lần hoàn thành là cần thiết ngay cả khi bản thân ứng viên không tạo ra điểm cố định.
