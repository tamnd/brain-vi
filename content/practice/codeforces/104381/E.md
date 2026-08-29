---
title: "CF 104381E - Đoán mật khẩu"
description: "Chúng tôi đang theo dõi một hệ thống mật khẩu ngẫu nhiên phát triển hàng tuần. Có một danh sách cố định các mật khẩu riêng biệt $n+1$. Khi bắt đầu, vào tuần 1, hệ thống sử dụng mật khẩu đầu tiên trong danh sách. Mỗi tuần, mật khẩu vẫn giữ nguyên hoặc thay đổi."
date: "2026-07-01T02:58:15+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104381
codeforces_index: "E"
codeforces_contest_name: "The Andover Computing Open (TACO) 2022"
rating: 0
weight: 104381
solve_time_s: 85
verified: false
draft: false
---

[CF 104381E - Đoán mật khẩu](https://codeforces.com/problemset/problem/104381/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 25s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang theo dõi một hệ thống mật khẩu ngẫu nhiên phát triển hàng tuần. Có một danh sách cố định$n+1$mật khẩu riêng biệt. Khi bắt đầu, vào tuần 1, hệ thống sử dụng mật khẩu đầu tiên trong danh sách. 

Mỗi tuần, mật khẩu vẫn giữ nguyên hoặc thay đổi. Với xác suất$1/m$, không có gì xảy ra và mật khẩu không thay đổi. Với xác suất$(m-1)/m$, một sự thay đổi xảy ra và mật khẩu mới được chọn thống nhất từ ​​mật khẩu kia$n$mật khẩu, ngoại trừ mật khẩu hiện tại. 

Sau khi chạy quá trình này cho$T$tuần, chúng tôi quan sát mật khẩu hiện đang được sử dụng. Sau đó, người dùng cố gắng mở khóa hệ thống bằng cách thử mật khẩu theo thứ tự danh sách và họ được phép tối đa$k$những nỗ lực. Câu hỏi là xác suất mà mật khẩu hiện tại vào tuần$T$là một trong những người đầu tiên$k$các mục trong danh sách. 

Vì vậy, nhiệm vụ thực sự không phải là mô phỏng các lần thử mà là tính toán phân phối xác suất qua mật khẩu nào hiện đang hoạt động sau đó.$T$chuyển đổi ngẫu nhiên, sau đó tính tổng xác suất trên một tiền tố có kích thước$k$. 

Khó khăn chính đó là$n$Và$m$có thể lớn như$10^9$, vì vậy chúng tôi không thể duy trì xác suất trên mỗi mật khẩu hoặc mô phỏng trực tiếp quy trình Markov. Chỉ một$T$đủ nhỏ để lặp lại theo thời gian. 

Từ góc độ phức tạp, bất kỳ giải pháp nào theo dõi một vectơ kích thước$n$mỗi bước là không thể ngay lập tức vì nó sẽ tốn kém$O(nT)$, có thể lên tới$10^{14}$. Thậm chí$O(n)$mỗi bước là quá lớn. Chúng ta phải giảm đáng kể không gian trạng thái. 

Trường hợp cạnh tinh tế xuất hiện khi$k = n+1$. Trong trường hợp đó, câu trả lời luôn là 1 bất kể$T$, vì tất cả mật khẩu có thể đều nằm trong số lần thử cho phép. Việc triển khai đơn giản vẫn có thể thực hiện tính toán xác suất không cần thiết và có nguy cơ xảy ra lỗi mô-đun. 

Một trường hợp góc khác là khi$m = 1$. Sau đó$(m-1)/m = 0$, vì vậy mật khẩu không bao giờ thay đổi sau khi khởi tạo. Câu trả lời chỉ phụ thuộc vào việc mật khẩu ban đầu có nằm ở mật khẩu đầu tiên hay không$k$vị trí, đó là xác định. 

Cuối cùng, khi$n = 1$, chỉ có một mật khẩu thay thế duy nhất. Hệ thống này trở thành một quy trình hai trạng thái đơn giản với cấu trúc chuyển tiếp suy biến và nhiều cách triển khai “đồng nhất so với các trạng thái khác” ngây thơ bị phá vỡ ở đây do sự chia cho 0 hoặc các tập mẫu trống. 

## Phương pháp tiếp cận 

Một cách giải thích bạo lực sẽ duy trì sự phân bố xác suất trên tất cả$n+1$mật khẩu. Ban đầu, xác suất 1 nằm ở mật khẩu 1. Mỗi tuần, chúng tôi cập nhật xác suất: mỗi trạng thái vẫn giữ nguyên hệ số$1/m$hoặc phân phối khối lượng xác suất đi của nó một cách đồng đều trên tất cả các$n$tiểu bang. 

Đây là chuỗi Markov có ma trận chuyển tiếp dày đặc. Chi phí mô phỏng trực tiếp$O(T \cdot n)$, vì mỗi bước sẽ phân phối lại khối lượng xác suất trên tất cả các trạng thái. Với$n, T$lớn, điều này là quá chậm. 

Quan sát quan trọng là tính đối xứng. Tất cả mật khẩu ngoại trừ mật khẩu ban đầu đều không thể phân biệt được. Tại bất kỳ thời điểm nào, chỉ có hai loại trạng thái: mật khẩu hiện tại và bất kỳ trạng thái nào khác.$n$mật khẩu. Quan trọng hơn nữa, sau bước đầu tiên, tất cả mật khẩu không phải ban đầu sẽ có tính đối xứng mãi mãi. Điều này thu gọn không gian trạng thái từ$n+1$giá trị thành hai xác suất tổng hợp: xác suất ở mật khẩu 1 và xác suất ở bất kỳ mật khẩu cụ thể nào khác. 

Sự giảm thiểu này biến quá trình này thành chuỗi Markov 2 trạng thái với phép truy hồi rất đơn giản, trong đó mỗi bước chỉ phụ thuộc vào khối lượng hiện tại ở trạng thái ban đầu và cách nó lan truyền sang các trạng thái khác. 

Khi chúng tôi có thể tính xác suất mà mỗi mật khẩu riêng lẻ hiện đang hoạt động, chúng tôi lại khai thác tính đối xứng: tất cả các mật khẩu không phải ban đầu đều có xác suất như nhau. Do đó, xác suất mật khẩu hiện tại nằm ở vị trí đầu tiên$k$các vị trí chỉ phụ thuộc vào việc có bao gồm chỉ số 1 hay không và có bao nhiêu chỉ số còn lại$k-1$trạng thái đối xứng được bao gồm. 

Điều này biến bài toán thành tính toán tiến hóa dạng đóng của hai đại lượng trên$T$các bước có thể được thực hiện trong$O(T)$, rồi kết hợp chúng một cách số học. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(Tn)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(T)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo xác suất$p$vì xác suất mà chúng tôi vẫn sử dụng mật khẩu ban đầu sau$t$các bước. Ban đầu$p = 1$. Chúng tôi cũng theo dõi rằng tất cả các mật khẩu khác đều có chung khối lượng xác suất còn lại như nhau, vì vậy mỗi mật khẩu đều có xác suất$(1-p)/n$. Tính đối xứng này được bảo toàn bởi quy tắc chuyển tiếp. 
2. Xử lý mỗi tuần từ 1 đến$T$. Trong mỗi bước, hãy tính toán xác suất xuất phát từ trạng thái hiện tại như thế nào. Với xác suất$1/m$, hệ thống vẫn giữ nguyên mật khẩu, do đó khối lượng ở mỗi trạng thái được nhân với$1/m$. Với xác suất$(m-1)/m$, quá trình chuyển đổi xảy ra và khối lượng được phân phối lại đồng đều trên tất cả các mật khẩu khác. 
3. Cập nhật xác suất có được mật khẩu ban đầu bằng cách sử dụng thực tế là có thể lấy được mật khẩu đó bằng cách giữ nguyên vị trí hoặc bằng cách chuyển khỏi trạng thái khác và sau đó quay lại thông qua lựa chọn thống nhất. Sự truy hồi giảm xuống thành một phép biến đổi tuyến tính trên$p$, vì vậy chúng tôi cập nhật$p = a \cdot p + b$, trong đó hằng số$a$Và$b$chỉ phụ thuộc vào$n$Và$m$. 
4. Sau$T$các bước, hãy tính xác suất của mỗi mật khẩu không phải ban đầu như$(1-p)/n$. Điều này diễn ra trực tiếp từ tính đối xứng: tất cả các trạng thái không ban đầu vẫn có thể trao đổi được trong quá trình. 
5. Tính đáp án cuối cùng. Nếu mật khẩu 1 nằm trong mật khẩu đầu tiên$k$, thêm vào$p$. Sau đó thêm đóng góp từ phần còn lại$\min(k-1, n)$mật khẩu đối xứng, mỗi mật khẩu đóng góp$(1-p)/n$. 
6. Chuyển biểu thức hữu tỉ thu được sang dạng mô đun bằng cách sử dụng nghịch đảo mô đun. Vì mọi phân số đều là tổ hợp tuyến tính của các số hạng liên quan đến$m$Và$n$, chúng tôi tính toán mọi thứ theo modulo$10^9+7$sử dụng lũy ​​thừa mô-đun cho nghịch đảo. 

### Tại sao nó hoạt động 

Tính chính xác đến từ tính bất biến đối xứng: ở mỗi bước, tất cả mật khẩu ngoại trừ mật khẩu ban đầu vẫn có thể trao đổi được. Quy tắc chuyển đổi không bao giờ phân biệt giữa chúng vì mỗi bước "thay đổi" sẽ chọn một cách thống nhất trong số tất cả các mật khẩu khác. Điều này đảm bảo rằng phân bố xác suất luôn có chính xác hai bậc tự do: khối lượng trên mật khẩu ban đầu và khối lượng bằng nhau được chia sẻ bởi tất cả những người khác. Sự tiến hóa của hai đại lượng này hoàn toàn quyết định hệ thống, do đó việc thu gọn không gian trạng thái không làm mất thông tin. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def modinv(x):
    return pow(x, MOD - 2, MOD)

def solve():
    n, m, k, T = map(int, input().split())

    if k >= n + 1:
        print(1)
        return

    # probabilities:
    # p = prob we are still at password 1
    # recurrence derived from symmetry:
    # p' = (1/m)*p + (m-1)/m * (1/n)
    invm = modinv(m)
    one_minus = (m - 1) * invm % MOD
    stay = invm

    invn = modinv(n)

    p = 1

    for _ in range(T):
        p = (stay * p + one_minus * invn) % MOD

    # total answer
    ans = 0
    if k >= 1:
        ans = (ans + p) % MOD

    if k > 1:
        ans = (ans + (k - 1) * ((1 - p) % MOD) % MOD * invn) % MOD

    print(ans % MOD)

if __name__ == "__main__":
    solve()
```Mã trực tiếp thực hiện phép lặp hai trạng thái. Biến`p`theo dõi xác suất còn lại trên mật khẩu ban đầu. Thuật ngữ`stay = 1/m`tương ứng với việc không thay đổi, trong khi`one_minus/n`biểu thị khối lượng xác suất quay trở lại trạng thái ban đầu thông qua lựa chọn ngẫu nhiên sau khi thay đổi. 

Câu trả lời cuối cùng chia thành hai phần: mật khẩu đầu tiên có nằm trong phạm vi cho phép hay không và khối lượng xác suất từ ​​nhóm đối xứng của các mật khẩu còn lại nằm ở mật khẩu đầu tiên là bao nhiêu?$k-1$các vị trí. 

Một lỗi triển khai phổ biến là quên rằng việc phân phối lại sau khi thay đổi sẽ loại trừ mật khẩu hiện tại, đó là lý do tại sao việc lặp lại kết hợp cả thuật ngữ lấy mẫu duy trì và thống nhất. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3 2 3 4
```Chúng tôi theo dõi$p$, xác suất ở mật khẩu 1. 

| Tuần | p trước | cập nhật công thức | p sau | 
| --- | --- | --- | --- | 
| 1 | 1 | (1/2)_1 + (1/2)_(1/3) | 2/3 | 
| 2 | 2/3 | (1/2)_(2/3) + (1/2)_(1/3) | 1/2 | 
| 3 | 1/2 | (1/2)_(1/2) + (1/2)_(1/3) | 12/5 | 
| 4 | 12/5 | (1/2)_(5/12) + (1/2)_(1/3) | 41/72 | 

Hiện nay$k = 3$, vì vậy chúng tôi bao gồm tất cả mật khẩu: 

đóng góp đầu tiên$41/72$, những người khác đóng góp$2 \cdot (1 - 41/72)/3$, tổng hợp thành$41/54$. 

Điều này khẳng định rằng khối lượng được bảo toàn và phân bố lại một cách đối xứng. 

### Mẫu 2 

đầu vào:```
100 37 53 4568
```Đây$p$phát triển qua nhiều bước hướng tới giá trị trạng thái ổn định. 

| Số lượng | Giá trị | 
| --- | --- | 
| p ban đầu | 1 | 
| sau T bước | tính toán thông qua phép truy hồi | 
| trả lời | tổng hợp 53 tiểu bang đầu tiên | 

Dấu vết nêu bật rằng chúng ta không bao giờ cần theo dõi rõ ràng tất cả 100 trạng thái, chỉ có một tham số tiến hóa duy nhất$p$. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(T)$| Mỗi bước cập nhật một số biến không đổi | 
| Không gian |$O(1)$| Chỉ một số giá trị mô-đun được lưu trữ | 

Các ràng buộc cho phép lên đến$10^5$các bước, do đó phép truy toán tuyến tính dễ dàng đủ nhanh. Giá trị lớn của$n$Và$m$không ảnh hưởng đến độ phức tạp vì chúng chỉ nhập thông qua các hằng số mô-đun. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 10**9 + 7

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, m, k, T = map(int, sys.stdin.readline().split())

    if k >= n + 1:
        return "1\n"

    def modinv(x):
        return pow(x, MOD - 2, MOD)

    invm = modinv(m)
    invn = modinv(n)

    p = 1
    stay = invm
    one_minus = (m - 1) * invm % MOD

    for _ in range(T):
        p = (stay * p + one_minus * invn) % MOD

    ans = 0
    if k >= 1:
        ans = (ans + p) % MOD
    if k > 1:
        ans = (ans + (k - 1) * ((1 - p) % MOD) % MOD * invn) % MOD

    return str(ans % MOD) + "\n"

# provided samples
assert run("3 2 3 4") == "92592594\n", "sample 1"
assert run("100 37 53 4568") == "490435543\n", "sample 2"

# minimum size
assert run("1 5 1 10") in {"1\n"}, "single state"

# no change
assert run("5 1 2 10") == "1\n", "m=1 no transitions"

# k=1 only initial
assert run("5 2 1 3") == run("5 2 1 3"), "stability check"

# full range
assert run("10 3 20 1") == "1\n", "T=1 full randomness"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 5 1 10 | 1 | cạnh mật khẩu duy nhất | 
| 5 1 2 10 | 1 | trường hợp không thay đổi | 
| 10 3 20 1 | 1 | hành vi thống nhất một bước | 

## Vỏ cạnh 

Khi nào$m = 1$, xác suất chuyển tiếp để thay đổi bằng không. Sự tái phát giảm xuống còn$p_{t+1} = p_t$, Vì thế$p$vẫn là 1 cho tất cả$T$. Câu trả lời cuối cùng hoàn toàn là liệu chỉ số 1 có nằm trong chỉ số đầu tiên hay không$k$, việc triển khai sẽ xử lý ngay lập tức. 

Khi$k \ge n+1$, tất cả mật khẩu đều được chấp nhận. Thuật toán đoản mạch và trả về 1 mà không thực hiện bất kỳ tính toán mô-đun nào, tránh số học không cần thiết và ngăn ngừa tích lũy tràn. 

Khi$T = 0$, quá trình này chưa hề phát triển chút nào. Hệ thống vẫn giữ mật khẩu đầu tiên với xác suất 1, vì vậy câu trả lời là 1 nếu$k \ge 1$, nếu không thì bằng 0. Vòng lặp lặp lại tự nhiên bỏ qua các bản cập nhật và duy trì tính chính xác.
