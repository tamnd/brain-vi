---
title: "CF 102192A - Mã hóa ký tự"
description: "Một từ có độ dài m có thể được biểu diễn bằng một mảng m giá trị ký tự được mã hóa. Mỗi vị trí chọn độc lập một số nguyên từ 0 đến n - 1. Chúng ta cần đếm xem có bao nhiêu mảng như vậy có tổng tổng chính xác là k."
date: "2026-08-18T20:30:11+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102192
codeforces_index: "A"
codeforces_contest_name: "2018 Chinese Multi-University Training, Nanjing U Contest"
rating: 0
weight: 102192
solve_time_s: 162
verified: true
draft: false
---

[CF 102192A - Mã hóa ký tự](https://codeforces.com/problemset/problem/102192/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 42s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Một từ dài`m`có thể được biểu diễn bằng một mảng`m`giá trị ký tự được mã hóa. Mỗi vị trí chọn độc lập một số nguyên từ`0`bởi vì`n - 1`. Chúng ta cần đếm chính xác có bao nhiêu mảng như vậy có tổng tổng`k`. 

Ví dụ, với`n = 2`Và`m = 3`, mọi vị trí đều chứa một trong hai`0`hoặc`1`. từ`[0, 1, 1]`có tổng`2`, trong khi`[1, 1, 1]`có tổng`3`. Các mảng khác nhau biểu thị các từ khác nhau vì các lựa chọn ký tự được sắp xếp theo thứ tự. 

Câu trả lời được lấy modulo`998244353`, vì số lượng mảng hợp lệ có thể trở nên rất lớn. 

Các giới hạn loại trừ lập trình động thông thường trên cả độ dài từ và tổng mục tiêu cho mọi trường hợp thử nghiệm. Một DP với`O(mk)`hoạt động có thể yêu cầu khoảng`10^10`hoạt động cho một trường hợp thử nghiệm khi cả hai`m`Và`k`là`10^5`. Giới hạn tổng hợp, trong đó tổng của tất cả`n`,`m`, Và`k`mỗi cái nhiều nhất`5 * 10^6`, gợi ý rằng một thuật toán gần như tuyến tính trong`m + k`cho mỗi trường hợp thử nghiệm hoặc tốt hơn là bắt buộc. Từ`k <= 10^5`, giai thừa và giai thừa nghịch đảo cũng có thể được tính toán trước trên toàn cầu, cho phép đánh giá các hệ số nhị thức trong thời gian không đổi. 

Có một số trường hợp ranh giới mà việc triển khai trực tiếp có thể xử lý sai. Với`n = 1`, mọi ký tự đều phải có giá trị`0`, vì vậy đối với đầu vào`1 5 0`câu trả lời là`1`, trong khi`1 5 1`có câu trả lời`0`. Một công thức giả định mọi giá trị từ`0`ĐẾN`n - 1`đưa ra ít nhất hai lựa chọn có thể làm sai trường hợp này. 

Số tiền tối đa có thể là`m(n - 1)`. Như vậy`2 3 3`có câu trả lời`1`, bởi vì mảng duy nhất là`[1,1,1]`, trong khi`2 3 4`có câu trả lời`0`. Việc triển khai chỉ kiểm tra xem`k`không âm có thể vô tình đếm được những tổng không thể. 

Ranh giới dưới hoạt động tương tự. Đối với mọi hợp lệ`n`Và`m`, mảng duy nhất có tổng`0`là`[0,0,...,0]`. Kể từ đây`5 4 0`có câu trả lời`1`. Đây cũng là một phép thử hữu ích để phát hiện các lỗi sai sót trong công thức hệ số nhị thức. 

## Phương pháp tiếp cận 

Phương pháp brute-force liệt kê trực tiếp mọi từ có thể. Mỗi trong số`m`vị trí có`n`sự lựa chọn, vì vậy có`n^m`mảng để kiểm tra. Đối với mỗi mảng chúng ta có thể tính tổng của nó theo`O(m)`thời gian, cho`O(m n^m)`công việc. Ngay cả khi số tiền được duy trì tăng dần, bản thân việc liệt kê vẫn tốn kém`O(n^m)`. Vì`n = m = 10^5`, điều này không chỉ đơn thuần là quá chậm mà còn vượt quá bất kỳ số lượng hoạt động thực tế nào về mặt thiên văn. 

Một công thức lập trình động tiêu chuẩn sẽ tốt hơn nhiều. Cho phép`dp[i][s]`là số độ dài-`i`mảng có tổng`s`. Thêm một ký tự sẽ mang lại`dp[i][s] = dp[i-1][s] + dp[i-1][s-1] + ... + dp[i-1][s-(n-1)]`. 

Một cửa sổ trượt có thể giảm mỗi lần chuyển đổi thành`O(1)`, tạo nên toàn bộ DP`O(mk)`. Đó đã là một cải tiến đáng kể, nhưng trường hợp xấu nhất vẫn cần khoảng`10^10`hoạt động nên không phù hợp. 

Quan sát quan trọng là các lựa chọn ở mọi vị trí đều chính xác là các số nguyên trong khoảng`[0,n-1]`. Không có giới hạn trên, số nghiệm không âm của`x1 + x2 + ... + xm = k`là giá trị sao và vạch`C(k + m - 1, m - 1)`. 

Giới hạn trên`xi <= n-1`có thể được xử lý bằng cách bao gồm-loại trừ. Đối với tập hợp đã chọn`j`vị trí vi phạm giới hạn trên, trừ`n`từ mỗi người trong số họ. Nếu giá trị ban đầu của chúng ít nhất là`n`, viết`xi = yi + n`, Ở đâu`yi >= 0`. Các biến còn lại là số nguyên không âm không hạn chế và tổng mới của chúng là`k - jn`. 

có`C(m,j)`cách chọn vị trí vi phạm và số nghiệm không âm sau khi trừ`n`là`C(k - jn + m - 1, m - 1)`. 

Theo đó, câu trả lời là`sum (-1)^j C(m,j) C(k - jn + m - 1, m - 1)`tổng thể`j`vì cái gì`jn <= k`. Điều khoản với`j > m`không tồn tại bởi vì chúng ta không thể chọn nhiều hơn`m`vị trí vi phạm. 

Lực lượng vũ phu hoạt động vì nó xem xét từng mảng hợp lệ riêng lẻ, nhưng không thành công vì số lượng mảng là theo cấp số nhân. DP nhóm các mảng theo tổng một phần của chúng nhưng vẫn xử lý quá nhiều trạng thái. Nhóm quan sát loại trừ bao gồm tất cả các mảng theo vị trí nào vượt quá giá trị cho phép, giảm tính toán tối đa`min(m, floor(k/n)) + 1`thuật ngữ nhị thức. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(n^m)`hoặc`O(m n^m)`với phép tính tổng rõ ràng |`O(m)`| Quá chậm | 
| DP có cửa sổ trượt |`O(mk)`|`O(k)`| Quá chậm trong trường hợp xấu nhất | 
| Bao gồm-Loại trừ |`O(min(m, k/n))`mỗi ca kiểm thử sau khi tiền xử lý |`O(max(m+k))`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc từng test case và xác định giá trị lớn nhất của`m + k`điều đó sẽ cần thiết. Tính toán trước các giai thừa và giai thừa nghịch đảo đến mức tối đa này. Chúng ta cần những mảng này vì mọi thuật ngữ đều chứa các hệ số nhị thức. 
2. Trước khi áp dụng công thức, hãy kiểm tra xem`k > m(n-1)`. Tổng lớn nhất có thể đạt được khi mỗi ký tự có giá trị`n-1`, vì vậy mục tiêu như vậy không có từ hợp lệ và câu trả lời là ngay lập tức`0`. 
3. Đối với các trường hợp còn lại, hãy khởi tạo câu trả lời bằng`j = 0`thuật ngữ bao gồm-loại trừ,`C(k + m - 1, m - 1)`. 

Điều này đếm tất cả các mảng không âm có tổng`k`, mà không thực thi giới hạn trên. 
4. Đối với`j = 1, 2, ...`, dừng lại khi một trong hai`j > m`hoặc`jn > k`. Đối với mỗi hợp lệ`j`, tính toán`C(m,j) * C(k - jn + m - 1, m - 1)`. 

Yếu tố đầu tiên chọn cái nào`j`vị trí vi phạm giới hạn trên. Yếu tố thứ hai tính các bài tập sau khi trừ`n`từ mỗi vị trí đã chọn. 
5. Thêm thuật ngữ khi`j`là số chẵn và trừ nó khi`j`thật kỳ quặc. Đây là dấu hiệu loại trừ tương ứng với số vị trí vi phạm được lựa chọn. 
6. Giảm modulo câu trả lời đang chạy`998244353`sau mỗi lần thao tác. Cuối cùng in kết quả chuẩn hóa. 

Hệ số nhị thức được tính bằng cách sử dụng`C(a,b) = fact[a] * invfact[b] * invfact[a-b] mod MOD`. 

Các giai thừa được tính toán một lần và các giai thừa nghịch đảo được lấy từ một nghịch đảo mô-đun theo sau là một phép truy hồi ngược. 

### Tại sao nó hoạt động 

Xét tập hợp tất cả các mảng số nguyên không âm có độ dài`m`tổng của ai là`k`. Các ngôi sao và thanh đếm bộ này với`C(k+m-1,m-1)`. Chúng ta cần loại bỏ các mảng chứa một hoặc nhiều vị trí có giá trị ít nhất là`n`. 

Đối với bất kỳ bộ được chọn nào`j`vi phạm vị trí, trừ`n`từ mỗi giá trị được chọn. Điều này tạo ra sự song ánh với các mảng không âm có tổng là`k-jn`. có`C(k-jn+m-1,m-1)`mảng như vậy, và có`C(m,j)`lựa chọn cho các vị trí đã chọn. 

Loại trừ bao gồm thêm các tập hợp có số lượng vi phạm chẵn và trừ các tập hợp có số lẻ. Mỗi mảng không hợp lệ với`r`vị trí vi phạm góp phần`C(r,0) - C(r,1) + C(r,2) - ... + (-1)^r C(r,r) = 0`, 

trong khi mọi mảng hợp lệ không có vi phạm nào và đóng góp chính xác một lần. Do đó, tổng cuối cùng tính chính xác các từ hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def solve():
    t = int(input())
    tests = [tuple(map(int, input().split())) for _ in range(t)]

    max_size = 0
    for n, m, k in tests:
        max_size = max(max_size, m + k)

    fact = [1] * (max_size + 1)
    for i in range(1, max_size + 1):
        fact[i] = fact[i - 1] * i % MOD

    invfact = [1] * (max_size + 1)
    invfact[max_size] = pow(fact[max_size], MOD - 2, MOD)
    for i in range(max_size, 0, -1):
        invfact[i - 1] = invfact[i] * i % MOD

    def comb(a, b):
        if b < 0 or b > a or a < 0:
            return 0
        return fact[a] * invfact[b] % MOD * invfact[a - b] % MOD

    out = []

    for n, m, k in tests:
        if k > m * (n - 1):
            out.append("0")
            continue

        if k == 0:
            out.append("1")
            continue

        ans = 0
        max_j = min(m, k // n)

        for j in range(max_j + 1):
            remaining = k - j * n
            ways = comb(m, j) * comb(
                remaining + m - 1, m - 1
            ) % MOD

            if j & 1:
                ans -= ways
            else:
                ans += ways

        out.append(str(ans % MOD))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Lần đầu tiên vượt qua các ca kiểm thử sẽ tìm ra chỉ số giai thừa lớn nhất cần thiết. Đối số nhị thức lớn nhất là`k + m - 1`, do đó phân bổ giai thừa thông qua`m + k`là đủ và tránh được việc phân bổ riêng cho từng trường hợp thử nghiệm. 

các`comb`hàm trả về 0 cho các đối số không hợp lệ. Trong vòng lặp chính, các đối số của nó luôn hợp lệ vì`remaining = k - jn`là không âm, nhưng việc giữ các kiểm tra ranh giới sẽ làm cho trình trợ giúp an toàn và ngăn chặn hành vi chỉ số âm tinh vi. 

Việc kiểm tra tổng tối đa có thể được thực hiện trước vòng lặp loại trừ. Điều này tránh được những công việc không cần thiết và xử lý trực tiếp các mục tiêu không thể thực hiện được. 

Vòng lặp bao gồm`j = 0`. Thuật ngữ đó là số lượng sao và thanh không hạn chế. Giới hạn vòng lặp là`min(m, k // n)`, bởi vì chọn nhiều hơn`m`vị trí là không thể và việc lựa chọn`j`các vị trí yêu cầu ít nhất`jn`tổng số tiền. 

Số nguyên Python không tràn, nhưng tất cả kết quả nhân đều được giảm modulo`MOD`. Điều này giữ cho các giá trị trung gian nhỏ và phù hợp với số học cần thiết. 

Mảng giai thừa nghịch đảo được tạo ra từ một phép lũy thừa mô-đun. Từ`998244353`là số nguyên tố, định lý nhỏ Fermat cho`fact[max_size]^(MOD-2)`như nghịch đảo mô-đun của nó. Mọi giai thừa nghịch đảo nhỏ hơn đều theo sau từ`invfact[i-1] = invfact[i] * i`. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Hãy xem xét`n = 2`,`m = 3`,`k = 3`. Mỗi vị trí chỉ có thể chứa`0`hoặc`1`. Số tiền tối đa là`3`, do đó mục tiêu nằm chính xác ở ranh giới trên. 

|`j`|`remaining = k - jn`|`C(m,j)`|`C(remaining+m-1,m-1)`| Ký kết thời hạn | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 3 | 1 | 10 | +10 | 10 | 
| 1 | 1 | 3 | 3 | -9 | 1 | 

Vì`j = 2`,`2n = 4 > k`, do đó vòng lặp dừng lại. Kết quả là`1`, tương ứng với`[1,1,1]`. 

Số lượng không hạn chế bắt đầu lúc`10`, bao gồm các mảng chứa các giá trị lớn hơn`1`. các`j = 1`thuật ngữ loại bỏ các mảng không hợp lệ đó, để lại chính xác một mảng hợp lệ. 

### Mẫu 2 

Hãy xem xét`n = 2`,`m = 3`,`k = 4`. Số tiền tối đa có thể chỉ là`3`, do đó thuật toán sẽ từ chối trường hợp đó trước khi thực hiện loại trừ bao gồm. 

|`n`|`m`|`k`| Số tiền tối đa`m(n-1)`| Kết quả | 
| --- | --- | --- | --- | --- | 
| 2 | 3 | 4 | 3 | 0 | 

Điều này chứng tỏ tại sao việc kiểm tra tổng tối đa phải sử dụng`m(n-1)`, không chỉ đơn thuần là so sánh`k`với`m`hoặc`n`. 

### Mẫu 3 

cho`n = 3`,`m = 3`,`k = 3`, các ký tự có giá trị`0`,`1`, hoặc`2`. 

|`j`|`remaining`|`C(3,j)`| Số lượng sao và vạch | Ký kết thời hạn | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 3 | 1 | 10 | +10 | 10 | 
| 1 | 0 | 3 | 1 | -3 | 7 | 

Kết quả là`7`. Số lượng không hạn chế chứa chính xác ba mảng trong đó có ít nhất một vị trí`3`, và chúng sẽ bị loại bỏ bởi số hạng hiệu chỉnh đầu tiên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(M + Σ min(m, k/n))`|`M = max(m+k)`để xử lý trước giai thừa, theo sau là một vòng lặp loại trừ bao gồm cho mỗi trường hợp thử nghiệm | 
| Không gian |`O(M)`| Mảng giai thừa và nghịch đảo | 

Trên tất cả các trường hợp thử nghiệm,`Σk <= 5 * 10^6`Và`Σm <= 5 * 10^6`. Từ`min(m, k/n) <= k`đối với mỗi trường hợp thử nghiệm, tổng số lần lặp lại bao gồm-loại trừ nhiều nhất là`5 * 10^6`cho đến nhỏ`+1`đóng góp từ mỗi trường hợp thử nghiệm. Quá trình tiền xử lý giai thừa chỉ cần khoảng`max(m+k) <= 2 * 10^5`, vì vậy cả yêu cầu về thời gian và bộ nhớ đều vừa vặn trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io

MOD = 998244353

def solve(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    input = sys.stdin.readline

    t = int(input())
    tests = [tuple(map(int, input().split())) for _ in range(t)]

    max_size = max(m + k for n, m, k in tests)

    fact = [1] * (max_size + 1)
    for i in range(1, max_size + 1):
        fact[i] = fact[i - 1] * i % MOD

    invfact = [1] * (max_size + 1)
    invfact[max_size] = pow(fact[max_size], MOD - 2, MOD)
    for i in range(max_size, 0, -1):
        invfact[i - 1] = invfact[i] * i % MOD

    def comb(a, b):
        if b < 0 or b > a or a < 0:
            return 0
        return fact[a] * invfact[b] % MOD * invfact[a - b] % MOD

    out = []

    for n, m, k in tests:
        if k > m * (n - 1):
            out.append("0")
            continue

        if k == 0:
            out.append("1")
            continue

        ans = 0
        for j in range(min(m, k // n) + 1):
            remaining = k - j * n
            ways = comb(m, j) * comb(
                remaining + m - 1, m - 1
            ) % MOD

            if j & 1:
                ans -= ways
            else:
                ans += ways

        out.append(str(ans % MOD))

    result = sys.stdout.getvalue()

    sys.stdin = old_stdin
    sys.stdout = old_stdout

    return result

# Provided samples
sample = """4
2 3 3
2 3 4
3 3 3
128 3 340
"""
assert solve(sample) == "1\n0\n7\n903\n", "provided samples"

# Minimum alphabet and minimum target
assert solve("1\n1 1 0\n") == "1\n", "minimum-size valid case"

# n = 1 has only the all-zero word
assert solve("1\n1 5 1\n") == "0\n", "n=1 impossible positive sum"

# Maximum possible sum, exactly one word
assert solve("1\n2 5 5\n") == "1\n", "upper boundary"

# Just above the maximum possible sum
assert solve("1\n2 5 6\n") == "0\n", "above upper boundary"

# n=3, m=2, k=2:
# [0,2], [1,1], [2,0]
assert solve("1\n3 2 2\n") == "3\n", "small inclusion-exclusion case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 0`|`1`| Đầu vào có kích thước tối thiểu và từ có tổng bằng 0 duy nhất | 
|`1 5 1`|`0`| Điều đặc biệt`n = 1`case |
 |`2 5 5`|`1`| Chính xác số tiền tối đa có thể | 
|`2 5 6`|`0`| Mục tiêu vừa vượt quá phạm vi khả thi | 
|`3 2 2`|`3`| Trường hợp nhỏ trong đó việc sửa lỗi bao gồm-loại trừ được thực hiện | 

## Vỏ cạnh 

Khi nào`n = 1`, mọi ký tự đều buộc phải có giá trị`0`. Vì`1 5 0`, số tiền tối đa có thể là`5(1-1)=0`, vì vậy mục tiêu là khả thi và mảng duy nhất là`[0,0,0,0,0]`. Thuật toán trả về`1`. Vì`1 5 1`, mục tiêu vượt quá số tiền tối đa`0`, vì vậy nó trả về`0`ngay lập tức. Điều này ngăn vòng lặp loại trừ dựa vào giá trị ký tự dương không tồn tại. 

Để có mục tiêu chính xác ở mức tối đa, hãy xem xét`2 3 3`. Mỗi vị trí tối đa là`1`, vậy đạt được tổng`3`lực lượng`[1,1,1]`. Công thức cho`C(5,2) - C(3,1)C(2,2) = 10 - 9 = 1`. Ranh giới được tính chính xác vì`j = 1`việc hiệu chỉnh sẽ loại bỏ mọi nghiệm không hạn chế có chứa một giá trị ít nhất`2`. 

Đối với mục tiêu trên mức tối đa,`2 3 4`có số tiền tối đa có thể`3`. Thuật toán phát hiện`4 > 3`và trả về`0`mà không đánh giá bất kỳ hệ số nhị thức nào. Việc triển khai DP hoặc loại trừ bao gồm không tính đến ranh giới này một cách rõ ràng có thể lãng phí công sức đáng kể và một công thức có phần dư âm được xử lý không chính xác có thể tạo ra số đếm không hợp lệ. 

Đối với mục tiêu bằng 0, hãy xem xét`5 4 0`. Mọi giá trị ký tự đều không âm, do đó tổng bằng 0 buộc mọi giá trị đều bằng 0. các`j = 0`thời hạn là`C(3,3)=1`, Và`k // n = 0`, nên không có số hạng hiệu chỉnh. Câu trả lời là chính xác`1`. 

Đối với một trường hợp nhỏ thể hiện sự điều chỉnh giới hạn trên, hãy xem xét`3 2 2`. Không có hạn chế`xi <= 2`, có`C(3,1)=3`giải pháp:`[0,2]`,`[1,1]`, Và`[2,0]`, tất cả đều đã hợp lệ. Vòng lặp bao gồm-loại trừ có`j = 0`Và`j = 1`, nếu không có`j = 1`, tổng còn lại âm vì`k-n = -1`, Vì thế`j=1`hoàn toàn không đạt được. Kết quả vẫn còn`3`. Điều này xác nhận rằng điều kiện vòng lặp`j <= k/n`dừng chính xác trước khi đưa ra số tiền âm còn lại không hợp lệ.
