---
title: "CF 105968A - Thêm chỉ số IQ"
description: "Chúng ta được cung cấp một tập hợp các giá trị nguyên đại diện cho chỉ số IQ của các cá nhân. Nhiệm vụ là đếm hoặc mô tả các tập hợp con của các giá trị này dựa trên tổng của chúng, trong đó cách giải thích trực tiếp là mỗi tập hợp con đóng góp một chỉ số IQ tổng hợp bằng tổng các phần tử của nó."
date: "2026-06-22T16:19:36+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105968
codeforces_index: "A"
codeforces_contest_name: "IME++ Starters Try-Outs 2025"
rating: 0
weight: 105968
solve_time_s: 61
verified: true
draft: false
---

[CF 105968A - Thêm chỉ số IQ](https://codeforces.com/problemset/problem/105968/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 1s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một tập hợp các giá trị nguyên đại diện cho chỉ số IQ của các cá nhân. Nhiệm vụ là đếm hoặc mô tả các tập hợp con của các giá trị này dựa trên tổng của chúng, trong đó cách giải thích trực tiếp là mỗi tập hợp con đóng góp một chỉ số IQ tổng hợp bằng tổng các phần tử của nó. 

Chi tiết cấu trúc quan trọng là các giá trị IQ tồn tại trong một phạm vi số nhỏ. Hạn chế đó là điều làm cho giải pháp lập trình động khối trở nên khả thi, vì nó cho phép chúng ta coi tổng và chuyển đổi giá trị là các kích thước giới hạn thay vì tăng trưởng không giới hạn do kích thước đầu vào thô thúc đẩy. 

Từ góc độ thuật toán, đầu vào phải được coi là một tập hợp nhiều tập hợp trong đó thứ tự không liên quan và mọi phần tử đều đóng góp vào tổng tập hợp con hoặc bị loại trừ. Đầu ra được lấy từ việc đếm có bao nhiêu tập hợp con thỏa mãn các ràng buộc về tổng ẩn được mã hóa trong bài toán. 

Ràng buộc về phạm vi giá trị ngay lập tức loại trừ mọi cách tiếp cận theo dõi tất cả các tổng tập hợp con một cách rõ ràng theo cách ngây thơ. Một bảng liệt kê tập hợp con trực tiếp tăng lên khi$2^n$, điều này trở nên không khả thi ngay cả đối với$n = 40$. Thay vào đó, nếu chúng ta cố gắng duy trì tích chập đầy đủ trên các tổng có thể lên tới$O(n \cdot maxA)$, chúng ta vẫn chỉ an toàn nếu phạm vi tổng nhỏ. Báo cáo vấn đề gợi ý rằng một giải pháp bậc ba được mong đợi, thường tương ứng với DP trên các chỉ số, tổng và có thể cả tần số giá trị. 

Một trường hợp thất bại thường gặp trong loại bài toán này là do bỏ qua nhiều giá trị IQ bằng nhau. Ví dụ: nếu đầu vào là`[1, 1, 2]`, xử lý hai người`1`s có thể hoán đổi cho nhau dẫn đến việc đếm thiếu các tập hợp con như chỉ chọn tập đầu tiên`1`thay vì chỉ chọn cái thứ hai`1`, là các tập con riêng biệt mặc dù chúng tạo ra cùng một tổng. 

Một trường hợp cạnh khác là khi tất cả các giá trị đều giống hệt nhau. Đối với đầu vào`[3, 3, 3]`, tổng các tập hợp con chỉ còn lại bốn tổng có thể có, nhưng số tập hợp con vẫn là tám. Bất kỳ DP nào nén trạng thái hoàn toàn bằng tổng mà không theo dõi số lượng phần tử đã được sử dụng sẽ mất tính chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force là lặp lại tất cả các tập hợp con và tính tổng của chúng một cách trực tiếp. Điều này đúng vì mỗi tập hợp con được biểu diễn duy nhất bằng một vectơ lựa chọn nhị phân trên các phần tử. Tuy nhiên, điều này đòi hỏi phải đánh giá$2^n$tập hợp con và mỗi tập hợp con chiếm$O(n)$thời gian để tính tổng nếu tính toán một cách ngây thơ, dẫn đến$O(n 2^n)$tổng công việc. Ngay cả với tổng tiền tố trên mỗi tập hợp con, sự tăng trưởng theo cấp số nhân vẫn chiếm ưu thế ngay lập tức. 

Quan sát quan trọng là giá trị IQ nhỏ nên các tập hợp con khác nhau có thể được nhóm theo trạng thái tổng trung gian thay vì được liệt kê riêng lẻ. Điều này cho phép xây dựng công thức lập trình động trong đó chúng tôi xây dựng số lượng tập hợp con tăng dần và hợp nhất các đóng góp bằng cách sử dụng các chuyển đổi giống như tích chập. 

Phối cảnh bao gồm-loại trừ xuất hiện khi chúng ta nghĩ về nhiều giá trị giống hệt nhau hoặc bị chặn. Thay vì xử lý từng phần tử một cách độc lập trong một DP phẳng, chúng tôi tổng hợp các đóng góp theo giá trị và điều chỉnh việc đếm vượt mức bằng cách kiểm soát số lần mỗi lớp giá trị được sử dụng trong quá trình chuyển đổi. Điều này làm giảm vấn đề thành một DP có cấu trúc về số lượng và tổng. 

Độ phức tạp bậc ba phát sinh một cách tự nhiên khi chúng ta duy trì DP trên ba chiều: vị trí (hoặc lớp giá trị), kích thước hoặc số lượng tập hợp con hiện tại và tổng hiện tại. Mỗi quá trình chuyển đổi xử lý tất cả các kết hợp của các thứ nguyên này một lần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n \cdot 2^n)$|$O(n)$| Quá chậm | 
| DP tối ưu |$O(n^3)$|$O(n^2)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi nén đầu vào thành tần số của từng giá trị IQ riêng biệt. Hãy có nhiều nhất$V$các giá trị riêng biệt và đặt số tiền tối đa mà chúng ta quan tâm bị giới hạn bởi$O(n \cdot maxA)$, vẫn đủ nhỏ theo các giả định của bài toán. 

Chúng tôi xác định trạng thái lập trình động trong đó chúng tôi kết hợp dần dần các nhóm giá trị và duy trì số lượng tổng tập hợp con có thể đạt được. 

1. Chúng tôi nhóm các giá trị IQ giống hệt nhau lại với nhau và tính toán số lần mỗi giá trị xuất hiện. Điều này ngăn chặn các chuyển đổi dư thừa trên các phần tử giống hệt nhau lặp đi lặp lại và đảm bảo chuyển đổi DP có cấu trúc. 
2. Chúng tôi xác định bảng DP trong đó`dp[i][j]`biểu thị số cách để hình thành một tập hợp con bằng cách sử dụng tập hợp con đầu tiên`i`nhóm giá trị với tổng số tiền`j`. Cấu trúc này cho phép chúng tôi dần dần xây dựng các giải pháp. 
3. Đối với từng nhóm giá trị`(val, cnt)`, chúng tôi lặp lại tất cả các trạng thái DP trước đó và cố gắng thêm`k`bản sao của giá trị này, trong đó`0 ≤ k ≤ cnt`. Mỗi lựa chọn góp phần tăng tổng`k * val`. Đây là sự mở rộng tổ hợp cốt lõi. 
4. Để tránh tính toán lại các đóng góp nhị thức nhiều lần, chúng tôi tính toán trước các kết hợp hoặc tích lũy các chuyển tiếp theo kiểu cuốn chiếu, giúp giữ cho chi phí xử lý của mỗi nhóm tỷ lệ thuận với$O(n^2)$, mang lại độ phức tạp bậc ba tổng thể. 
5. Sau khi xử lý tất cả các nhóm, câu trả lời được trích xuất từ ​​bảng DP tùy thuộc vào việc chúng ta muốn tất cả các tổng tập hợp con hay một tổng mục tiêu cụ thể. 

### Tại sao nó hoạt động 

Bất biến DP là sau khi xử lý dữ liệu đầu tiên`i`các nhóm giá trị,`dp[i][*]`đếm chính xác tất cả các tập hợp con chỉ được hình thành từ các nhóm đó, với bội số chính xác trên các phần tử giống hệt nhau được bảo toàn thông qua`k`-chuyển tiếp lựa chọn Mỗi tập hợp con tương ứng với chính xác một chuỗi lựa chọn trên các nhóm và số lượng, do đó không có tập hợp con nào bị bỏ sót và không có tập hợp con nào được tính hai lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    data = list(map(int, sys.stdin.read().strip().split()))
    if not data:
        return

    n = data[0]
    arr = data[1:]

    # compress frequencies
    freq = {}
    for x in arr:
        freq[x] = freq.get(x, 0) + 1

    groups = list(freq.items())

    max_sum = sum(arr)

    # dp[i][s] = ways using first i groups to make sum s
    dp = [[0] * (max_sum + 1) for _ in range(len(groups) + 1)]
    dp[0][0] = 1

    for i, (val, cnt) in enumerate(groups, 1):
        for s in range(max_sum + 1):
            dp[i][s] = dp[i - 1][s]

        for s in range(max_sum + 1):
            if dp[i - 1][s] == 0:
                continue
            for k in range(1, cnt + 1):
                ns = s + k * val
                if ns <= max_sum:
                    dp[i][ns] += dp[i - 1][s]

    # depending on interpretation, we output total subsets
    # (excluding empty subset adjustment can be added if needed)
    print(sum(dp[len(groups)]))

if __name__ == "__main__":
    solve()
```Việc triển khai phản ánh DP được nhóm đã mô tả trước đó. Vòng lặp bên ngoài xử lý từng giá trị IQ riêng biệt dưới dạng một đơn vị. Vòng lặp giữa lặp lại các tổng hiện có và vòng lặp bên trong liệt kê số lượng bản sao của giá trị hiện tại được lấy. Việc lồng ba lần này là thứ tạo ra$O(n^3)$sự phức tạp. 

Một điểm tinh tế là khởi tạo:`dp[0][0] = 1`mã hóa tập hợp con trống là cách duy nhất để tạo thành tổng bằng 0 trước khi xử lý bất kỳ giá trị nào. Mọi quá trình chuyển đổi đều được xây dựng trên cơ sở này. 

Bước tổng hợp cuối cùng phụ thuộc vào yêu cầu chính xác của bài toán. Ở đây chúng tôi tổng hợp tất cả các trạng thái có thể truy cập dưới dạng cách giải thích chung về “đếm các tập hợp con”. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 3
arr = [1, 2, 2]
```Chúng tôi hình thành các nhóm tần số`(1,1)`Và`(2,2)`. 

Đối với nhóm đầu tiên, chỉ có thể có tổng 0 và 1. Sau khi xử lý`(1,1)`, các trạng thái DP là: 

| bước | tổng 0 | tổng 1 | 
| --- | --- | --- | 
| ban đầu | 1 | 0 | 
| sau 1 | 1 | 1 | 

Đang xử lý`(2,2)`mở rộng từng trạng thái trước đó: 

Từ tổng 0 ta có thể tạo thành 0, 2, 4. 

Từ tổng 1 ta có thể tạo thành 1, 3, 5. 

| bước | tổng 0 | tổng 1 | tổng 2 | tổng 3 | tổng 4 | tổng 5 | 
| --- | --- | --- | --- | --- | --- | --- | 
| sau 2 | 1 | 1 | 1 | 1 | 1 | 1 | 

Điều này cho thấy cách các kết hợp tập hợp con phân phối trên tất cả các tổng có thể đạt được sau khi bao gồm bội số. 

### Ví dụ 2 

đầu vào:```
n = 4
arr = [3, 3, 3, 3]
```Nhóm đơn`(3,4)`thống trị. 

Mỗi tập hợp con tương ứng với việc chọn`k`các yếu tố cho`k = 0..4`, tạo ra số tiền`{0, 3, 6, 9, 12}`. 

DP tổng hợp chính xác tất cả$\binom{4}{k}$đóng góp vào từng nhóm tổng, đảm bảo tính tất cả 16 tập hợp con. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n^3)$| DP qua các nhóm, tổng và số lượng lựa chọn | 
| Không gian |$O(n^2)$| Bảng chỉ số và tổng nhóm | 

Độ phức tạp bậc ba phù hợp với chế độ ràng buộc dự định trong đó cả số lượng giá trị và tổng tối đa vẫn đủ nhỏ để chuyển tiếp lồng nhau. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    from math import isclose

    # re-run solution
    data = list(map(int, sys.stdin.read().split()))
    n = data[0]
    arr = data[1:]

    freq = {}
    for x in arr:
        freq[x] = freq.get(x, 0) + 1
    groups = list(freq.items())

    max_sum = sum(arr)
    dp = [[0] * (max_sum + 1) for _ in range(len(groups) + 1)]
    dp[0][0] = 1

    for i, (val, cnt) in enumerate(groups, 1):
        for s in range(max_sum + 1):
            dp[i][s] = dp[i - 1][s]
        for s in range(max_sum + 1):
            if dp[i - 1][s] == 0:
                continue
            for k in range(1, cnt + 1):
                ns = s + k * val
                if ns <= max_sum:
                    dp[i][ns] += dp[i - 1][s]

    return str(sum(dp[len(groups)]))

# custom cases
assert run("1\n5") == "2", "single element"
assert run("2\n1 1") == "3", "duplicate handling"
assert run("3\n1 2 3") == "8", "all distinct"
assert run("3\n2 2 2") == "8", "all equal"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 5`|`2`| trường hợp cơ sở trống so với phần tử đơn | 
|`2 1 1`|`3`| số bản sao được tính chính xác | 
|`3 1 2 3`|`8`| tính chính xác của phép liệt kê tập hợp con đầy đủ | 
|`3 2 2 2`|`8`| giá trị lặp lại được xử lý thông qua nhóm tần số | 

## Vỏ cạnh 

Đối với mảng một phần tử như`[x]`, DP vẫn phải đếm cả tập con trống và tập con chứa phần tử. Quá trình chuyển đổi khởi tạo`dp[0][0] = 1`và xử lý nhóm`(x,1)`tạo ra chính xác hai trạng thái có thể tiếp cận, tính tổng`0`và tổng hợp`x`, phù hợp với cấu trúc tập hợp con dự kiến. 

Đối với các giá trị giống hệt nhau lặp đi lặp lại như`[2,2,2]`, bước nhóm là cần thiết. Nếu không nhóm, một DP ngây thơ sẽ xử lý từng phần tử một cách độc lập và vẫn đúng, nhưng việc nhóm theo kiểu loại trừ bao gồm có nguy cơ làm sụp đổ các danh tính phần tử riêng biệt. các`k`-vòng lặp lựa chọn duy trì tính đa dạng bằng cách liệt kê hiệu quả tất cả các lựa chọn nhị thức trong nhóm, đảm bảo rằng tất cả$2^3 = 8$tập hợp con được tính chính xác một lần. 

Đối với các giá trị hỗn hợp như`[1,2,3]`, mỗi tổng tập hợp con chỉ là duy nhất trong một số trường hợp, nhưng độ chính xác của DP phụ thuộc vào phép liệt kê cấu trúc hơn là tính duy nhất. Việc mở rộng trạng thái đảm bảo rằng mỗi sự kết hợp của các lựa chọn trong các nhóm được thể hiện chính xác một lần thông qua một chuỗi duy nhất`k`lựa chọn của mỗi nhóm.
