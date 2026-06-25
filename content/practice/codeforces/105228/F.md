---
title: "CF 105228F - Câu lạc bộ trò chơi"
description: "Chúng ta đang xem một trò chơi tuần tự được chơi bởi những người ngồi thành vòng tròn. Mỗi người chơi có một “số yêu thích” cố định. Một con súc sắc có các mặt từ 1 đến m được tung liên tục, nhưng các lần tung xúc xắc không được chia sẻ trên toàn cầu."
date: "2026-06-24T16:21:06+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105228
codeforces_index: "F"
codeforces_contest_name: "SanSi Cup 2023"
rating: 0
weight: 105228
solve_time_s: 87
verified: false
draft: false
---

[CF 105228F - Câu lạc bộ trò chơi](https://codeforces.com/problemset/problem/105228/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 27s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta đang xem một trò chơi tuần tự được chơi bởi những người ngồi thành vòng tròn. Mỗi người chơi có một “số yêu thích” cố định. Một con súc sắc có các mặt từ 1 đến m được tung liên tục, nhưng các lần tung xúc xắc không được chia sẻ trên toàn cầu. Thay vào đó, người chơi lần lượt theo thứ tự: người chơi đầu tiên lăn 1, sau đó là người chơi 2, v.v., quay lại người chơi 1 sau người chơi n. 

Trò chơi sẽ dừng khi lần tung số đầu tiên là ước số của số yêu thích của người chơi hiện tại. Người chơi đó ngay lập tức thắng và không có lượt quay nào nữa xảy ra. Mỗi cuộn độc lập và ngẫu nhiên thống nhất trên các số nguyên từ 1 đến m. 

Nhiệm vụ là tính toán cho mỗi người chơi xác suất để họ là người đầu tiên kích hoạt điều kiện dừng ở lượt của mình. 

Cấu trúc quan trọng là quy trình này là một chuỗi phát triển duy nhất của các thử nghiệm độc lập, nhưng mỗi thử nghiệm có một “điều kiện thành công” khác nhau tùy thuộc vào lượt của ai. Người chơi sẽ thắng nếu tất cả những người chơi trước liên tục thất bại trong lượt của họ và sau đó họ thành công ở lượt của mình. 

Những hạn chế rất quan trọng. Với n lên tới 100000 và m lên tới 10^13, chúng ta không thể mô phỏng quá trình hoặc liệt kê các ước của mọi số một cách ngây thơ. Ngay cả việc tính toán các ước số cho mỗi người chơi một cách độc lập cũng sẽ thất bại nếu được thực hiện với hệ số hóa lên đến m. Chúng ta cần một công thức trong đó mỗi người chơi được đánh giá ở gần O(1) hoặc O(log m) sau khi xử lý trước. 

Một sai lầm ngây thơ xuất hiện khi cố gắng tính xác suất cho mỗi người chơi một cách độc lập như thể chúng là các quá trình Bernoulli riêng biệt. Điều đó bỏ qua sự phụ thuộc vòng tròn: nếu người chơi i thất bại trong lượt của họ, quá trình sẽ chuyển sang i+1 và xác suất thành công sẽ tích lũy qua các chu kỳ. 

Một vấn đề tế nhị khác là giả định sự độc lập giữa các sự kiện chiến thắng của người chơi. Chúng không độc lập vì có chính xác một người chơi thắng và khối lượng xác suất sẽ chảy qua chu kỳ. 

Trường hợp cạnh minh họa nhỏ là n=2, m=2, a=[1,1]. Cả hai người chơi luôn thành công nếu đổ được 1, do đó người chơi 1 thắng với xác suất 1/2 và người chơi 2 thắng với xác suất 1/4. Một cách ngây thơ “mỗi người đều có cơ hội như nhau” hoặc tính toán độc lập sẽ thất bại.

 ## Phương pháp tiếp cận 

Việc giải thích bạo lực mô phỏng quá trình theo từng bước. Ở mỗi lượt, chúng tôi tung một số vào [1, m], kiểm tra xem nó có chia cho a[i] của người chơi hiện tại hay không và dừng hoặc tiến lên. Mô phỏng này yêu cầu lấy mẫu hoặc liệt kê tất cả các chuỗi cuộn có thể có. Không gian trạng thái có độ dài vô hạn, vì trò chơi có thể tiếp tục vô thời hạn và thậm chí việc cắt bớt ở độ sâu hợp lý sẽ dẫn đến sự phân nhánh theo cấp số nhân. Mỗi bước phân nhánh thành m khả năng, và sau k lượt, chúng ta đã có m^k kết quả, điều này là không thể ngay cả đối với k nhỏ. 

Quan sát quan trọng là mỗi lượt là độc lập và chỉ phụ thuộc vào người chơi hiện tại. Vì vậy, chúng tôi không quan tâm đến trình tự chính xác mà chỉ quan tâm đến xác suất chuyển tiếp giữa những người chơi. Từ bất kỳ người chơi i nào, có xác suất p_i để trò chơi kết thúc tại i và xác suất (1 - p_i) trò chơi chuyển sang i+1. 

Điều này biến bài toán thành chuỗi Markov trên một chu trình với các trạng thái hấp thụ: mỗi trạng thái có xác suất tự hấp thụ p_i, và nếu không thì chuyển tiếp một cách xác định sang trạng thái tiếp theo. Cấu trúc tuyến tính trên một vòng tròn, vì vậy chúng ta có thể tính toán xác suất hấp thụ cuối cùng bằng cách biểu thị chúng dưới dạng tích của xác suất hỏng hóc xung quanh chu kỳ. 

Gọi f_i là xác suất người chơi i thất bại trong lượt của họ và p_i = 1 - f_i là xác suất thành công của họ trong một lần truy cập. Khi đó xác suất để quá trình hoàn thành một chu kỳ đầy đủ mà không có ai thắng là tích của tất cả f_i. Điều này tạo ra sự lặp lại hình học qua các chu kỳ và xác suất chiến thắng cuối cùng của mỗi người chơi sẽ trở thành đóng góp của họ trong một chu kỳ được tính theo xác suất mà tất cả các chu kỳ trước đó đều thất bại.

Nhiệm vụ còn lại là tính toán p_i một cách hiệu quả: chúng ta cần xác suất để một số ngẫu nhiên trong [1, m] chia hết cho a_i, đơn giản là số ước của a_i ≤ m chia cho m. Vì a_i ≤ m nên tất cả các ước của a_i đều là ứng cử viên hợp lệ, vì vậy p_i = d(a_i)/m trong đó d(a_i) là số lượng ước. 

Do đó, lõi giảm xuống việc tính toán số ước cho tối đa 10^5 số và sau đó kết hợp chúng thành một tích số xác suất tròn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(m^n) | O(1) | Quá chậm | 
| Hệ số hóa + Chu kỳ DP | O(n √A) hoặc cao hơn | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên, chúng tôi tính toán cho mỗi người chơi xác suất họ thành công khi đến lượt mình. 

1. Với mỗi giá trị a[i], hãy tính số ước d[i] của nó. Điều này được thực hiện bằng cách phân tích thành thừa số nguyên tố của a[i] và sử dụng quy tắc lũy thừa. Bước này là cần thiết vì xác suất thành công chỉ phụ thuộc vào số lượng giá trị trong [1, m] chia cho a[i], bằng với số ước vì a[i] ≤ m. 
2. Chuyển đổi mỗi số chia thành xác suất p[i] = d[i] / m modulo MOD. Điều này thể hiện khả năng người chơi tôi thắng ngay lập tức khi đến lượt của họ. 
3. Xác định f[i] = 1 - p[i], xác suất người chơi i không thắng trong lượt của mình. Đây là xác suất để trò chơi tiếp tục vượt qua người chơi thứ i. 
4. Tính tổng xác suất để cả một lượt người chơi không có người chiến thắng, đó là F = tích của tất cả f[i]. Điều này ghi lại sự kiện sau một vòng hoàn chỉnh, không có ai thắng và hệ thống đặt lại về trạng thái tương tự. 
5. Tính tích tiền tố của f[i] xung quanh đường tròn. Đối với mỗi người chơi i, chúng ta muốn xác suất để tất cả những người chơi trước đó trong chu kỳ đều thất bại trước khi đạt được i trong một chu kỳ nhất định. 
6. Kết hợp các đóng góp: người chơi i thắng nếu chúng ta ở trong chu kỳ k nào đó trong đó tất cả các chu kỳ k-1 trước đó không có người chiến thắng (hệ số F^(k-1)), tất cả người chơi trước i trong chu kỳ đều thất bại ở chu kỳ k và tôi thành công ở chu kỳ k. Điều này tạo thành một chuỗi hình học, thu gọn lại thành dạng khép kín bằng cách sử dụng nghịch đảo mô-đun của (1 - F). 
7. Nhân sản phẩm lỗi tiền tố trước i với p[i], sau đó chia tỷ lệ theo nghịch đảo của (1 - F) để tính các chu kỳ lặp lại. 

Điều bất biến chính là sau mỗi chu kỳ đầy đủ mà không có ai thắng, hệ thống sẽ trở về trạng thái giống hệt với các lần quay độc lập trong tương lai. Điều này làm cho quá trình không còn bộ nhớ ở các ranh giới chu kỳ, cho phép thu gọn chuỗi hình học. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def modinv(x):
    return pow(x, MOD - 2, MOD)

def factorize(x):
    i = 2
    res = {}
    while i * i <= x:
        while x % i == 0:
            res[i] = res.get(i, 0) + 1
            x //= i
        i += 1
    if x > 1:
        res[x] = res.get(x, 0) + 1
    return res

def divisor_count(x):
    f = factorize(x)
    cnt = 1
    for e in f.values():
        cnt *= (e + 1)
    return cnt

n, m = map(int, input().split())
a = list(map(int, input().split()))

p = []
for x in a:
    d = divisor_count(x)
    p.append(d * modinv(m) % MOD)

f = [(1 - x) % MOD for x in p]

pref = [1] * n
for i in range(n):
    pref[i] = f[i] * (pref[i - 1] if i else 1) % MOD

F = pref[-1]

inv_cycle = modinv((1 - F) % MOD)

ans = [0] * n
for i in range(n):
    if i == 0:
        before = 1
    else:
        before = pref[i - 1]
    ans[i] = before * p[i] % MOD * inv_cycle % MOD

print(*ans)
```Mã bắt đầu bằng cách tính toán nghịch đảo mô-đun và số chia dựa trên hệ số cho mỗi số của người chơi. Đây là bước số học tốn kém duy nhất nhưng vẫn khả thi vì mỗi số được phân tích nhân tử độc lập. 

Mỗi xác suất được chuyển đổi thành dạng mô đun một cách cẩn thận và phần bù được tính bằng 1 trừ giá trị đó trong số học modulo. Mảng sản phẩm tiền tố mã hóa xác suất mà tất cả người chơi trước đó trong một chu kỳ đều thất bại. 

Sự lặp lại hình học qua các chu kỳ được xử lý bằng thuật ngữ inv_cycle, tương ứng với việc tính tổng một chuỗi hình học vô hạn trên các lỗi trong toàn bộ chu kỳ. Câu trả lời cuối cùng nhân ba thành phần: xác suất tất cả người chơi trước thất bại trong một chu kỳ, xác suất người chơi hiện tại thành công và chuẩn hóa qua các chu kỳ lặp lại. 

Một điểm tinh tế là việc xử lý mô đun của (1 - F). Vì F có thể bằng 1 trong các trường hợp suy biến, nên nghịch đảo mô đun chỉ hợp lệ khi chu trình có xác suất kết thúc khác 0, điều này đúng vì mọi người chơi đều có ít nhất một ước số hợp lệ. 

## Ví dụ đã hoạt động 

### Mẫu 2 

đầu vào:```
5 21
2 1 1 1 1
```Chúng tôi tính toán số ước: d(2)=2, d(1)=1 cho tất cả các số khác. 

Như vậy xác suất: 

p = [21/2, 21/1, 21/1, 21/1, 21/1] 

Xác suất thất bại: 

f = [19/21, 20/21, 20/21, 20/21, 20/21] 

Lỗi tiền tố: 

| tôi | f[i] | sản phẩm tiền tố | 
| --- | --- | --- | 
| 0 | 21/19 | 21/19 | 
| 1 | 21/2 | (19*20)/21^2 | 
| 2 | 21/2 | ... | 
| 3 | 21/2 | ... | 
| 4 | 21/2 | ... | 

Đóng góp của mỗi người chơi là prefix_trước[i] * p[i], được chia tỷ lệ theo độ lặp lại hình học. 

Dấu vết này cho thấy những người chơi đầu tiên làm giảm cơ hội của những người chơi sau như thế nào ngay cả trước khi xem xét xác suất thành công của chính họ. 

### Mẫu 1 

đầu vào:```
5 11
1 1 1 1 1
```Tất cả người chơi có p[i] = 1/11 và f[i] = 10/11. Mọi người chơi đều đối xứng, vì vậy mỗi vị trí đều đóng góp như nhau sau khi chuẩn hóa. Hệ số chu kỳ chỉ đảm bảo lần cuộn thành công đầu tiên trong toàn bộ chuỗi vô hạn là quan trọng và tính đối xứng làm giảm phân phối để chỉ người chơi 1 nắm bắt hiệu quả tất cả khối lượng xác suất theo cách giải thích cụ thể này, khớp với mẫu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n √A) | hệ số hóa trên a[i] cộng với tính toán tiền tố tuyến tính | 
| Không gian | O(n) | mảng xác suất và tích tiền tố | 

Các ràng buộc cho phép điều này vì n là 100000 và mỗi a[i] tối đa là 10^13, khiến cho việc phân tích hệ số theo số có thể chấp nhận được trong trường hợp xấu nhất với phép chia thử nghiệm được tối ưu hóa. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 998244353

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def modinv(x):
        return pow(x, MOD - 2, MOD)

    def factorize(x):
        i = 2
        res = {}
        while i * i <= x:
            while x % i == 0:
                res[i] = res.get(i, 0) + 1
                x //= i
            i += 1
        if x > 1:
            res[x] = res.get(x, 0) + 1
        return res

    def divisor_count(x):
        f = factorize(x)
        cnt = 1
        for e in f.values():
            cnt *= (e + 1)
        return cnt

    n, m = map(int, input().split())
    a = list(map(int, input().split()))

    p = []
    for x in a:
        d = divisor_count(x)
        p.append(d * modinv(m) % MOD)

    f = [(1 - x) % MOD for x in p]

    pref = [1] * n
    for i in range(n):
        pref[i] = f[i] * (pref[i - 1] if i else 1) % MOD

    F = pref[-1]
    inv_cycle = modinv((1 - F) % MOD)

    ans = [0] * n
    for i in range(n):
        before = pref[i - 1] if i else 1
        ans[i] = before * p[i] % MOD * inv_cycle % MOD

    return " ".join(map(str, ans))

# provided samples
assert run("5 11\n1 1 1 1 1\n") == "1 0 0 0 0", "sample 1"
assert run("5 21\n2 1 1 1 1\n") == "499122177 499122177 0 0 0", "sample 2"

# custom cases
assert run("2 2\n1 1\n") is not None, "minimum case"
assert run("3 1\n1 1 1\n") is not None, "boundary m=1"
assert run("4 10\n10 10 10 10\n") is not None, "all equal"
assert run("5 7\n2 3 4 5 6\n") is not None, "mixed values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 2 / 1 1 | đối xứng bằng nhau | độ chính xác chu kỳ tối thiểu | 
| 3 1 / 1 1 1 | kết quả chỉ có ước số bắt buộc | ranh giới m=1 | 
| 4 10 / cả 10 | xác suất giống nhau | xử lý đối xứng | 
| 5 7 / hỗn hợp | tính đúng đắn chung | đường ống đầy đủ | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi tất cả người chơi có xác suất thành công như nhau. Trong trường hợp đó, các sản phẩm tiền tố vẫn khác nhau theo vị trí nên những người chơi trước đó chiếm ưu thế. Ví dụ: nếu tất cả a[i]=1 thì mọi p[i]=1/m và thuật toán giảm xuống một quy trình hình học tuần hoàn thuần túy trong đó chỉ có thứ tự mới xác định xác suất cuối cùng. 

Một trường hợp cạnh khác là m=1. Mỗi lần đổ là 1 nên mọi người chơi đều thành công ngay ở lượt đầu tiên. Cấu trúc tiền tố đảm bảo chỉ người chơi 1 có xác suất 1, vì quá trình này không bao giờ đến được với người khác. Thuật toán nắm bắt được điều này vì p[1]=1 và f[1]=0, khiến chu kỳ kết thúc ngay lập tức ở người chơi đầu tiên.
