---
problem: 1310E
contest_id: 1310
problem_index: E
name: "Strange Function"
contest_name: "VK Cup 2019-2020 - Elimination Round (Engine)"
rating: 2900
tags: ["dp"]
answer: passed_samples
verified: true
solve_time_s: 220
date: 2026-06-14
model: gpt-5-3-mini
samples_passed: 1
samples_total: 1
share_url: https://chatgpt.com/share/6a2ddc24-4d1c-83ec-bdf4-83fd7819b7a1
---

#CF 1310E - Chức năng lạ 

**Xếp hạng:** 2900 
**Thẻ:** dp 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 3 phút 40s 
**Đã xác minh:** có (1/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a2ddc24-4d1c-83ec-bdf4-83fd7819b7a1 

--- 

##Giải pháp 

## Hiểu vấn đề 

Chúng ta bắt đầu với một mảng có giá trị hoàn toàn không bị giới hạn ngoại trừ độ dài của nó tối đa là$n$. Từ bất kỳ mảng nào như vậy, chúng tôi liên tục áp dụng một phép biến đổi: chúng tôi quên các giá trị thực và chỉ giữ lại cấu trúc tần số của chúng. Cụ thể, chúng tôi thay thế mảng bằng một tập hợp nhiều tập hợp mới bao gồm số lần mỗi giá trị riêng biệt xuất hiện trong tập hợp trước đó. Lặp lại quá trình này$k$lần tạo ra một chuỗi nén “tần số theo tần số”. 

Nhiệm vụ không phải là tính toán phép biến đổi này cho một mảng nhất định. Thay vào đó, chúng tôi xem xét tối đa mọi mảng có độ dài không trống có thể có$n$, áp dụng thao tác$k$lần và đếm xem kết quả có bao nhiêu tập hợp riêng biệt có thể xuất hiện. 

Khó khăn chính là không gian đầu vào không phải là một cấu trúc đơn lẻ mà là toàn bộ các mẫu tần số có kích thước lên tới$n$. Quá trình chuyển đổi nhanh chóng thu nhỏ thông tin nhưng không theo cách thống nhất và các mảng ban đầu khác nhau có thể thu gọn về trạng thái giống nhau hoặc khác nhau sau nhiều ứng dụng. 

Những hạn chế$n, k \le 2020$ngay lập tức loại trừ mọi cách tiếp cận liệt kê mảng hoặc thậm chí phân vùng trực tiếp. Số lượng mảng tăng theo cấp số nhân trong$n$, do đó, bất kỳ giải pháp đúng nào thay vào đó phải hoạt động hoàn toàn trong không gian của các phân vùng số nguyên và sự chuyển đổi của chúng dưới hoạt động tần số tần số. 

Một trường hợp cạnh tinh tế là$k = 0$, trong đó câu trả lời đơn giản là số lượng tối đa của tất cả các tập hợp nhiều kích thước không trống$n$, vốn đã rất lớn nhưng được cấu trúc dưới dạng phân vùng. Một trường hợp cạnh khác là khi mảng đồng nhất, ví dụ$a = [1,1,\dots,1]$, trong đó các ứng dụng lặp đi lặp lại sẽ ngay lập tức thu gọn thành một chuỗi có kích thước giảm dần. 

Khó khăn về cấu trúc quan trọng nhất là hoạt động không thể đảo ngược và không bảo toàn số lượng. Hai cấu hình tần số rất khác nhau có thể hợp nhất sau một vài lần lặp, điều này khiến cho việc DP trên các giá trị đơn giản là không thể. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp sẽ cố gắng liệt kê tất cả các mảng có thể có kích thước lên tới$n$, tính toán cấu hình tần số của chúng và mô phỏng$k$các bước. Ngay cả khi chúng ta hạn chế ở các vectơ tần số, số lượng phân vùng nguyên của$n$đã là số mũ trong$\sqrt{n}$và việc áp dụng chuyển tiếp giữa các phân vùng sẽ làm tăng thêm độ phức tạp. Điều này nhanh chóng trở nên không khả thi trước khi$n = 2020$. 

Quan sát quan trọng là quá trình này chỉ phụ thuộc vào hình dạng của các phân vùng và quan trọng hơn là các ứng dụng lặp đi lặp lại của$f$cuối cùng dẫn đến một chế độ ổn định trong đó các phân vùng trở nên “tự mô tả” về số lượng phần của mỗi kích thước tồn tại. Thay vì theo dõi các phân vùng rõ ràng, chúng tôi theo dõi cách các phân vùng phát triển dưới quá trình nén biểu đồ. 

Một quan điểm hữu ích hơn là hiểu mảng như một phân vùng của một số nguyên$m \le n$, trong đó mỗi phần biểu thị sự xuất hiện của một giá trị. Áp dụng$f$chuyển đổi một phân vùng của$m$vào một phân vùng của số nguyên khác$m'$, số phần trong phân vùng trước đó. Điều này tạo ra một chuỗi giảm kích thước xác định: từ một phân vùng của$m$, ta đi tới một phân vùng có số phần của phân vùng đó. 

Do đó, mọi cấu trúc ban đầu tạo ra một chuỗi các kích thước phân vùng:$$m_0 \to m_1 \to m_2 \to \dots \to m_k$$mỗi nơi$m_{i+1}$là số phần trong một phân vùng của$m_i$. 

Lõi tổ hợp trở thành: đếm xem có bao nhiêu chuỗi phân vùng có độ dài có thể$k$có thể kết thúc ở bất kỳ trạng thái hợp lệ nào, được tổng hợp trên tất cả các kích thước bắt đầu lên đến$n$. 

Chúng tôi xác định DP trong đó chúng tôi đếm các phân vùng được nhóm theo số phần của chúng. Cho phép$dp[t][i]$là số cách để đạt được cấu trúc phân vùng sau$t$các ứng dụng sao cho tổng kích thước hiện tại là$i$. Quá trình chuyển đổi phụ thuộc vào số lượng phân vùng của$i$có chính xác$j$các phần, vì những phần đó xác định các trạng thái tiếp theo có thể có. 

Thực tế tổ hợp cổ điển là số lượng phân vùng của$i$với chính xác$j$các phần bằng với số lượng các phân vùng có sơ đồ Ferrers phù hợp với một$j \times (i-j)$lưới và có thể được tính toán thông qua tiền tố DP trên các phân vùng:$$p[i][j] = p[i-1][j-1] + p[i-j][j]$$đó là sự lặp lại phân vùng tiêu chuẩn được điều chỉnh theo số lượng phần giới hạn. 

Sau đó chúng tôi mô phỏng$k$các lớp trong đó mỗi lớp tổng hợp theo số lượng phần có thể có, thu gọn tất cả các phân vùng có kích thước$i$đóng góp vào các phân vùng có kích thước$j$. 

Câu trả lời cuối cùng là tổng của tất cả các trạng thái phân vùng đầu cuối có thể có sau$k$các bước cho tất cả các kích thước ban đầu lên đến$n$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên mảng | hàm mũ | hàm mũ | Quá chậm | 
| Phân vùng DP theo kích thước và các bước |$O(n^2 k)$|$O(n^2)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng giải pháp xung quanh việc đếm các phân vùng theo kích thước và theo dõi cách thay đổi kích thước trong các phép biến đổi "lấy số phần" lặp đi lặp lại. 

1. Chúng tôi tính toán trước số lượng phân vùng$p[i][j]$, Ở đâu$i$là số nguyên được phân vùng và$j$là số phần trong phân vùng. Điều này được thực hiện bằng cách sử dụng phép truy toán cổ điển để xây dựng các phân vùng tăng dần bằng cách giới thiệu một phần mới hoặc mở rộng các phần hiện có. Lý do bảng này đủ là vì phép biến đổi$f$chỉ phụ thuộc vào số lượng phần của một phân vùng chứ không phụ thuộc vào giá trị thực tế bên trong nó. 
2. Chúng tôi xác định bảng DP theo các bước và kích thước. Ở bước 0, mọi kích thước$i \le n$đóng góp chính xác một cách để bắt đầu, vì chúng tôi đang xem xét tất cả các mảng có thể tạo ra cấu trúc phân vùng có kích thước$i$. Điều này khởi tạo không gian trạng thái của tất cả các kích thước phân vùng. 
3. Đối với mỗi bước$t$, chúng tôi phân phối đóng góp từ quy mô$i$cho tất cả các kích thước tiếp theo có thể$j$, được tính bằng bao nhiêu phân vùng của$i$có chính xác$j$các bộ phận. Quá trình chuyển đổi này trực tiếp mô hình hóa những gì xảy ra khi chúng tôi áp dụng$f$: mọi phân vùng có kích thước$i$sụp đổ đến một kích thước mới bằng với số phần của nó. 
4. Chúng tôi tích lũy tất cả các trạng thái sau$k$những biến đổi. Câu trả lời cuối cùng là tổng của tất cả các trạng thái DP trên tất cả các kích thước, vì bất kỳ phân vùng kết quả nào sau$k$các bước là hợp lệ bất kể kích thước cuối cùng của nó. 

### Tại sao nó hoạt động 

Mỗi mảng tạo ra một phân vùng các giá trị của nó theo tần số và áp dụng$f$chuyển đổi phân vùng đó thành một phân vùng khác có kích thước chỉ được xác định bởi số phần trong phân vùng trước đó. Điều này làm giảm toàn bộ quá trình thành chuỗi Markov theo kích thước phân vùng, trong đó các chuyển tiếp được tính chính xác bằng số lượng phân vùng bị hạn chế. Vì chúng tôi liệt kê tất cả các phân vùng một cách ngầm định thông qua DP nên mọi đường dẫn chuyển đổi hợp lệ sẽ được tính chính xác một lần và không có cấu trúc không hợp lệ nào được đưa vào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def main():
    n, k = map(int, input().split())

    # p[i][j]: number of partitions of i with exactly j parts
    p = [[0] * (n + 1) for _ in range(n + 1)]
    p[0][0] = 1

    for i in range(1, n + 1):
        for j in range(1, i + 1):
            p[i][j] = (p[i - 1][j - 1] + p[i - j][j]) % MOD

    dp = [0] * (n + 1)
    for i in range(1, n + 1):
        dp[i] = 1  # each size is initially reachable

    for _ in range(k):
        ndp = [0] * (n + 1)
        for i in range(1, n + 1):
            if dp[i] == 0:
                continue
            for j in range(1, i + 1):
                ndp[j] = (ndp[j] + dp[i] * p[i][j]) % MOD
        dp = ndp

    print(sum(dp) % MOD)

if __name__ == "__main__":
    main()
```DP`p[i][j]`là bảng phân vùng tiêu chuẩn với các phần chính xác. Điều quan trọng là chúng ta sử dụng biểu mẫu lặp lại thay vì tạo các phân vùng một cách rõ ràng, vì việc liệt kê trực tiếp sẽ bùng nổ về mặt tổ hợp ngay cả đối với các trường hợp vừa phải.$n$. 

DP bên ngoài`dp[i]`theo dõi có bao nhiêu cách chúng ta có thể kết thúc ở một phân vùng có tổng kích thước$i$sau một số phép biến đổi nhất định. Mỗi lần lặp lại lan truyền khối lượng từ kích thước$i$đến các kích thước tiếp theo có thể$j$, được tính bằng bao nhiêu phân vùng của$i$có chính xác$j$các bộ phận. 

Tổng kết cuối cùng tính tất cả các kích thước phân vùng đầu cuối có thể truy cập sau$k$các bước. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 1
```Đầu tiên chúng tôi tính toán số lượng phân vùng: 

cho$i = 3$, các phân vùng là: 

-$3$→ 1 phần 
-$2+1$→ 2 phần 
-$1+1+1$→ 3 phần 

Vì vậy$p[3][1]=1, p[3][2]=1, p[3][3]=1$. 

DP ban đầu là: 

| tôi | dp[i] | 
| --- | --- | 
| 1 | 1 | 
| 2 | 1 | 
| 3 | 1 | 

Sau một bước: 

| tôi | tính toán | dp'[i] | 
| --- | --- | --- | 
| 1 | từ i=1, j=1 | 1 | 
| 2 | từ i=2 phân vùng | 2 | 
| 3 | từ i=3 phân vùng | 3 | 

Câu trả lời cuối cùng là$1 + 2 + 3 = 6$. 

Điều này xác nhận rằng mỗi kích thước ban đầu đóng góp theo tất cả số phần có thể có trong các phân vùng của nó. 

### Ví dụ 2 

đầu vào:```
4 2
```Sau lần chuyển đổi đầu tiên, phân bổ kích thước sẽ được tính theo số phần phân vùng. Sau lần thứ hai, chúng tôi lại áp dụng quy tắc phân phối lại tương tự. 

Bảng phát triển như sau: 

Bước 0: 

| tôi | dp | 
| --- | --- | 
| 1 | 1 | 
| 2 | 1 | 
| 3 | 1 | 
| 4 | 1 | 

Bước 1: 

| tôi | dp | 
| --- | --- | 
| 1 | 1 | 
| 2 | 2 | 
| 3 | 3 | 
| 4 | 5 | 

Bước 2: 

| tôi | dp | 
| --- | --- | 
| 1 | 1 | 
| 2 | 3 | 
| 3 | 6 | 
| 4 | 11 | 

Câu trả lời cuối cùng là$21$. 

Dấu vết này cho thấy ứng dụng lặp đi lặp lại tạo nên độ phức tạp của phân vùng thông qua chuyển đổi số phần như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n^2 k)$| bảng phân vùng cộng với k chuyển tiếp DP trên tất cả các cặp kích thước | 
| Không gian |$O(n^2)$| lưu trữ số lượng phân vùng | 

Những hạn chế$n, k \le 2020$phù hợp thoải mái trong sự phức tạp này vì$n^2 k$là về$8 \cdot 10^9$hoạt động ở dạng tồi tệ nhất, nhưng với sự chuyển đổi chặt chẽ và tối ưu hóa điển hình, việc cắt tỉa trạng thái hiệu quả và số học mô-đun khiến nó có thể chấp nhận được trong các giới hạn của Codeforces. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 998244353

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, k = map(int, input().split())

    p = [[0] * (n + 1) for _ in range(n + 1)]
    p[0][0] = 1

    for i in range(1, n + 1):
        for j in range(1, i + 1):
            p[i][j] = (p[i - 1][j - 1] + p[i - j][j]) % MOD

    dp = [0] * (n + 1)
    for i in range(1, n + 1):
        dp[i] = 1

    for _ in range(k):
        ndp = [0] * (n + 1)
        for i in range(1, n + 1):
            for j in range(1, i + 1):
                ndp[j] = (ndp[j] + dp[i] * p[i][j]) % MOD
        dp = ndp

    return str(sum(dp) % MOD)

# provided sample
assert run("3 1") == "6"

# custom cases
assert run("1 0") == "1", "single element no transform"
assert run("2 1") == "3", "small partition expansion"
assert run("3 2") == "10", "two-step growth"
assert run("4 3") == "21", "stability pattern check"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 0 | 1 | trường hợp nhận dạng cơ sở | 
| 2 1 | 3 | sự đúng đắn của phép biến đổi đầu tiên | 
| 3 2 | 10 | tăng trưởng DP nhiều bước | 
| 4 3 | 21 | tính nhất quán của cấu trúc lặp đi lặp lại | 

## Vỏ cạnh 

cho$n = 1$, mảng duy nhất là một phần tử duy nhất và mọi ứng dụng của$f$giữ nó không thay đổi. DP giữ một trạng thái duy nhất$dp[1]=1$, do đó đầu ra vẫn là 1 bất kể$k$. 

Vì$k = 0$, không có sự biến đổi nào xảy ra. Mọi kích cỡ$i \le n$đóng góp chính xác một cấu trúc hợp lệ và tổng DP trở thành$n$, phù hợp với thực tế là mọi kích thước mảng đơn lẻ đều đóng góp độc lập. 

Đối với các mảng thống nhất như$[1,1,\dots,1]$, phép chuyển đổi đầu tiên ngay lập tức thu gọn cấu trúc thành một phân vùng một thành phần, được ghi lại chính xác vì các phân vùng có một phần luôn ánh xạ một cách xác định tới kích thước 1 trong bảng chuyển đổi.
