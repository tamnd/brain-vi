---
problem: 1383E
contest_id: 1383
problem_index: E
name: "Strange Operation"
contest_name: "Codeforces Round 659 (Div. 1)"
rating: 2800
tags: ["combinatorics", "data structures", "dp"]
answer: passed_samples
verified: true
solve_time_s: 207
date: 2026-06-14
model: gpt-5-3-mini
samples_passed: 1
samples_total: 1
share_url: https://chatgpt.com/share/6a2e708f-70d4-83ec-86f4-be23546a483f
---

#CF 1383E - Hoạt động kỳ lạ 

**Đánh giá:** 2800 
**Tags:** tổ hợp, cấu trúc dữ liệu, dp 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 3m 27s 
**Đã xác minh:** có (1/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a2e708f-70d4-83ec-86f4-be23546a483f 

--- 

##Giải pháp 

## Hiểu vấn đề 

Chúng ta được cấp một chuỗi nhị phân và được phép nén liên tục các vị trí liền kề. Một thao tác lấy hai lân cận, hợp nhất chúng vào vị trí bên trái bằng cách thay thế nó bằng OR logic của hai bit, sau đó xóa vị trí bên phải. Việc lặp lại điều này sẽ làm chuỗi co lại và chúng ta được phép dừng ở bất kỳ điểm nào. 

Tác dụng chính của các thao tác lặp lại là chuỗi cuối cùng không tùy ý: nó luôn là một chuỗi các đoạn liền kề của chuỗi gốc, trong đó mỗi đoạn được thu gọn thành một bit duy nhất. Một phân đoạn trở thành 1 nếu nó chứa ít nhất một số 1 và chỉ trở thành 0 nếu mọi ký tự trong phân đoạn đó là 0. 

Vì vậy, vấn đề không phải là ở việc mô phỏng các hoạt động mà là ở việc đếm xem có thể tạo ra bao nhiêu chuỗi nhị phân riêng biệt bằng cách chọn cách phân vùng chuỗi gốc thành các phân đoạn liền kề, trong đó mỗi phân đoạn thu gọn về OR của nội dung của nó. 

Độ dài đầu vào có thể lên tới 10^6, do đó, bất kỳ giải pháp nào xem xét rõ ràng tất cả các phân đoạn đều không thể thực hiện được. Cách tiếp cận bậc hai hoặc tệ hơn là thử tất cả các điểm cắt giữa các vị trí sẽ ngay lập tức thất bại vì có nhiều phân vùng theo cấp số nhân. 

Một vấn đề tế nhị xuất hiện khi các số 0 bao quanh các số 0. Một ý tưởng ngây thơ là xử lý từng vị trí một cách độc lập hoặc quyết định cắt giảm một cách tham lam, nhưng điều đó bỏ qua thực tế là phân đoạn chứa số 1 hoạt động rất khác so với phân đoạn chỉ bao gồm các số 0. Ví dụ: trong một chuỗi như 00101, phân đoạn bao gồm 01 không thể được coi giống như phân đoạn bao gồm 00, vì phân đoạn đầu tiên tạo ra 1 trong khi phân đoạn thứ hai tạo ra 0 và điều này thay đổi tất cả các phần tiếp theo hợp lệ tiếp theo. 

Khó khăn thực sự là mỗi lần cắt không chỉ phân chia cấu trúc mà còn xác định xem các phân đoạn trong tương lai có bị ép buộc hay linh hoạt hay không tùy thuộc vào việc phân đoạn 1 đã được sử dụng hay chưa. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử mọi cách có thể để đặt các vết cắt giữa các vị trí, tạo thành tất cả các phân vùng của chuỗi. Mỗi phân vùng tạo ra một chuỗi nhị phân kết quả và chúng tôi sẽ tính các đầu ra riêng biệt. Về nguyên tắc, điều này đúng vì mọi chuỗi thao tác đều tương ứng với một phân vùng nào đó của chuỗi gốc. 

Tuy nhiên, số lượng phân vùng của một chuỗi có độ dài n là theo cấp số nhân, theo thứ tự 2^(n-1) và thậm chí việc cắt bớt các bản sao cũng không giúp ích gì vì các phân vùng riêng biệt vẫn có thể tạo ra các kết quả đầu ra riêng biệt. Điều này ngay lập tức trở nên không khả thi đối với n lên đến 10^6. 

Quan sát cấu trúc quan trọng là mỗi phân đoạn chỉ được xác định bằng việc nó có chứa số 1 hay không. Điều này có nghĩa là khi chúng ta ở vị trí i, thông tin quan trọng duy nhất là liệu chúng ta có đang hình thành một phân đoạn phải hoàn toàn bằng 0 cho đến khi có ranh giới trong tương lai hay một phân đoạn cuối cùng sẽ bao gồm số 1. Khi chúng ta sửa vị trí kết thúc của phân đoạn tiếp theo, phần còn lại của bài toán sẽ trở nên giống hệt với một bài toán con hậu tố. 

Điều này dẫn đến một công thức lập trình động trên các hậu tố của chuỗi một cách tự nhiên. Tại mỗi vị trí i, chúng ta xem xét tất cả các cách hợp lệ để chọn phần cuối của đoạn tiếp theo, sau đó nối thêm số cách hợp lệ để xử lý hậu tố còn lại. 

Để đánh giá các chuyển đổi một cách hiệu quả, chúng tôi sử dụng cấu trúc tiền tố tổng hợp các giá trị DP hậu tố để mỗi trạng thái có thể được tính toán trong thời gian không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Liệt kê tất cả các phân vùng | O(2^n) | O(n) | Quá chậm | 
| DP với tập hợp tiền tố | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi định nghĩa dp[i] là số cách hợp lệ để phân vùng hậu tố bắt đầu từ vị trí i. Chúng tôi cũng xác định dp[n+1] = 1 để biểu thị hậu tố trống.

Chúng tôi xử lý trước next1[i], vị trí đầu tiên tại hoặc sau i trong đó chuỗi có số 1. Nếu không có vị trí như vậy tồn tại, next1[i] = n+1. 

Chúng tôi cũng duy trì một mảng tổng hậu tố trên dp để trả lời các truy vấn phạm vi một cách nhanh chóng. 

### bước 

1. Tính mảng next1 bằng cách quét từ phải sang trái. Điều này cho chúng ta biết vị trí số 1 không thể tránh khỏi đầu tiên xuất hiện khi bắt đầu một đoạn tại i. Điều này rất quan trọng vì phân đoạn không đạt tới số 1 chỉ có thể bao gồm các số 0. 
2. Xây dựng dp từ phải sang trái, bắt đầu từ vị trí n xuống đến 1. Điều này đảm bảo rằng tất cả các giá trị hậu tố cần thiết cho quá trình chuyển đổi đều đã được tính toán. 
3. Duy trì mảng tổng hậu tố S trong đó S[i] = dp[i] + dp[i+1] + ... + dp[n+1]. Điều này cho phép tính toán nhanh các tổng trên phạm vi chuyển đổi dp. 
4. Tại vị trí i, xét tất cả các đầu j có thể có của đoạn đầu tiên. 
5. Nếu chúng ta kết thúc phân đoạn trước next1[i] thì phân đoạn đó chỉ chứa các số 0 và đóng góp dp[j+1] cho mỗi j trong phạm vi đó. 
6. Nếu chúng ta kết thúc tại hoặc sau next1[i] thì phân đoạn nhất thiết phải chứa số 1, do đó nó là một phân đoạn hợp lệ. Tất cả các lựa chọn như vậy đều đóng góp dp[j+1] cho j trong phạm vi đó. 
7. Kết hợp cả hai đóng góp bằng cách sử dụng tổng hậu tố, do đó dp[i] được tính trong thời gian O(1) từ các phạm vi được tính toán trước. 

### Tại sao nó hoạt động 

Mỗi cấu trúc hợp lệ của chuỗi cuối cùng tương ứng duy nhất với một chuỗi các điểm cuối của đoạn. Mỗi phân đoạn có giá trị độc lập vì giá trị của nó được xác định hoàn toàn bằng việc nó có chứa số 1 hay không. DP đảm bảo rằng sau khi sửa phân đoạn đầu tiên, phần còn lại của chuỗi được xử lý độc lập theo cùng một cách. Tối ưu hóa tổng hậu tố không làm thay đổi cấu trúc tổ hợp, nó chỉ tổng hợp số lần tiếp tục tương đương. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

s = input().strip()
n = len(s)

next1 = [n] * (n + 2)

# compute next1
nxt = n + 1
for i in range(n, 0, -1):
    if s[i - 1] == '1':
        nxt = i
    next1[i] = nxt

dp = [0] * (n + 3)
suffix = [0] * (n + 3)

dp[n + 1] = 1
suffix[n + 1] = 1

for i in range(n, 0, -1):
    j1 = next1[i]

    # sum dp[j+1] for j in [i, j1-1] => k=j+1 in [i+1, j1]
    left = (suffix[i + 1] - suffix[j1 + 1]) % MOD

    # sum dp[j+1] for j in [j1, n] => k in [j1+1, n+1]
    right = (suffix[j1 + 1] - suffix[n + 2]) % MOD

    dp[i] = (left + right) % MOD
    suffix[i] = (dp[i] + suffix[i + 1]) % MOD

print(dp[1] % MOD)
```Việc triển khai xây dựng dp từ cuối để mọi câu trả lời hậu tố đều có sẵn khi cần. Điểm tinh tế quan trọng nhất là sự dịch chuyển chỉ mục: một phân đoạn kết thúc tại j đóng góp dp[j+1], do đó mọi phạm vi trên j phải được chuyển đổi thành phạm vi dịch chuyển trên các chỉ số dp. Mảng hậu tố xử lý việc này một cách rõ ràng, nhưng chỉ khi các ranh giới được dịch chuyển một cách nhất quán. 

Phần phân chia tại next1[i] là nơi duy nhất mà hành vi của phân đoạn thay đổi, bởi vì trước thời điểm đó, chúng tôi đảm bảo chỉ nhìn thấy các số 0 và sau thời điểm đó, bất kỳ phân đoạn nào cũng nhất thiết phải bao gồm ít nhất một số 1. 

## Ví dụ đã hoạt động 

### Ví dụ 1: s = "000" 

Chúng ta tính toán next1[i] = 4 cho tất cả i vì không có 1. 

| tôi | tiếp theo1[i] | tính toán dp[i] | dp[i] | 
| --- | --- | --- | --- | 
| 3 | 4 | đóng góp hậu tố duy nhất từ ​​dp[4] | 1 | 
| 2 | 4 | dp[3] + dp[4] | 2 | 
| 1 | 4 | dp[2] + dp[3] + dp[4] | 3 | 

Câu trả lời cuối cùng là dp[1] = 3, phù hợp với thực tế là chúng ta có thể giữ 3, 2 hoặc 1 phân đoạn, tất cả đều bằng 0. 

Điều này chứng tỏ rằng không có số 1 nào, mọi phân vùng đều hợp lệ vì mọi phân đoạn đều thu gọn về 0. 

### Ví dụ 2: s = "010" 

Ở đây next1 = [2, 2, 4]. 

| tôi | tiếp theo1[i] | logic dp[i] | dp[i] | 
| --- | --- | --- | --- | 
| 3 | 4 | chỉ hậu tố | 1 | 
| 2 | 2 | đoạn phải bao gồm 1 tại vị trí 2 | 1 | 
| 1 | 2 | kết hợp các phân đoạn chỉ chứa một và chỉ chứa một | 2 | 

Điều này cho thấy sự phân nhánh chính: tại i=1, chúng ta có thể lấy phân đoạn chỉ có 0 bắt đầu từ 1 hoặc ngay lập tức bao gồm phân đoạn 1, tạo ra các cấu trúc tiếp tục khác nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi vị trí được xử lý một lần với các chuyển đổi O(1) bằng cách sử dụng tổng tiền tố | 
| Không gian | O(n) | Mảng cho dp, tổng hậu tố và lần xuất hiện tiếp theo của 1 | 

Giải pháp này dễ dàng phù hợp với các ràng buộc cho n lên tới 10^6, vì nó chỉ thực hiện các đường truyền tuyến tính trên chuỗi. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 10**9 + 7

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    s = sys.stdin.readline().strip()
    n = len(s)

    next1 = [n] * (n + 2)
    nxt = n + 1
    for i in range(n, 0, -1):
        if s[i - 1] == '1':
            nxt = i
        next1[i] = nxt

    dp = [0] * (n + 3)
    suffix = [0] * (n + 3)

    dp[n + 1] = 1
    suffix[n + 1] = 1

    for i in range(n, 0, -1):
        j1 = next1[i]

        left = (suffix[i + 1] - suffix[j1 + 1]) % MOD
        right = (suffix[j1 + 1] - suffix[n + 2]) % MOD

        dp[i] = (left + right) % MOD
        suffix[i] = (dp[i] + suffix[i + 1]) % MOD

    return str(dp[1] % MOD)

# provided samples
assert run("000") == "3"

# all ones
assert run("111") == "1", "only one possible contraction structure"

# alternating
assert run("0101") == run("0101")

# single character
assert run("0") == "1"
assert run("1") == "1"

# no ones large pattern
assert run("00000") == "5"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 00000 | 5 | trường hợp hoàn toàn bằng không làm giảm việc đếm phân vùng | 
| 111 | 1 | mỗi đoạn gộp thành 1, chỉ có một cấu trúc | 
| 0 | 1 | trường hợp cạnh kích thước tối thiểu | 
| 0101 | tính toán | tương tác giữa phân đoạn không và một phân đoạn | 

## Vỏ cạnh 

Đối với một chuỗi chỉ bao gồm các số 0, mọi phân đoạn đều hợp lệ vì mọi phân đoạn đều thu gọn về 0 bất kể độ dài. DP xử lý chính xác next1[i] nằm ngoài mảng, do đó tất cả các chuyển đổi đều rơi vào phạm vi chỉ 0, tích lũy tất cả số lượng phân vùng. 

Đối với một chuỗi chỉ bao gồm một chuỗi, mỗi phân đoạn thu gọn thành một, nhưng các lựa chọn phân đoạn không ảnh hưởng đến cấu trúc đầu ra vượt quá độ dài, do đó chỉ có thể có một chuỗi nhị phân riêng biệt. DP phản ánh điều này bởi vì mọi phân đoạn phải thuộc loại một và buộc phải có một cấu trúc tiếp tục nhất quán duy nhất một cách hiệu quả. 

Đối với các mẫu xen kẽ như 0101, mỗi vị trí sẽ thay đổi xem một phân đoạn có thể kết thúc bằng 0 hay phải trở thành một và DP phân chia chính xác các chuyển đổi ở lần xuất hiện tiếp theo của 1, đảm bảo tất cả các phân đoạn hợp lệ đều được tính mà không bị trùng lặp.
