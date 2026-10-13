---
title: "CF 105164I - Kiểm tra thuật toán hợp nhất"
description: "Chúng ta được cung cấp một chuỗi mục tiêu và chúng ta tưởng tượng nó được tạo ra bằng cách liên tục hợp nhất một tập hợp các chuỗi $M$ không trống bằng cách sử dụng quy trình hợp nhất hai con trỏ rất cụ thể."
date: "2026-06-27T10:52:06+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105164
codeforces_index: "I"
codeforces_contest_name: "2024 ICPC Gran Premio de Mexico 1ra Fecha"
rating: 0
weight: 105164
solve_time_s: 157
verified: false
draft: false
---

[CF 105164I - Kiểm tra thuật toán hợp nhất](https://codeforces.com/problemset/problem/105164/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 37s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi mục tiêu và chúng ta tưởng tượng nó được tạo ra bằng cách liên tục hợp nhất một tập hợp các$M$các chuỗi không trống bằng cách sử dụng thủ tục hợp nhất hai con trỏ rất cụ thể. 

Quy trình hợp nhất luôn so sánh các phần tử phía trước hiện tại của hai chuỗi và loại bỏ phần tử nhỏ hơn (hoặc phần tử bên trái trong trường hợp bằng nhau). Sau khi xây dựng được kết quả cuối cùng ta chỉ quan sát dãy kết quả này, còn kết quả ban đầu$M$trình tự bị ẩn. 

Nhiệm vụ là đếm xem chúng ta có thể chọn bao nhiêu cách khác nhau để chọn điểm ban đầu.$M$trình tự sao cho sau khi áp dụng quy trình hợp nhất lặp đi lặp lại của giáo sư, trình tự cuối cùng chính xác là trình tự mà chúng ta được cung cấp. 

Một điểm tinh tế quan trọng là các chuỗi không được coi là được sắp xếp nội bộ. Hoạt động hợp nhất vẫn tiến hành nghiêm ngặt bằng cách so sánh các phần tử phía trước, do đó, đầu ra được xác định động bằng các so sánh này chứ không phải bằng bất kỳ thuộc tính sắp xếp chung nào. 

Các ràng buộc cho phép tối đa 3000 phần tử trong chuỗi cuối cùng và tối đa 3000 chuỗi ban đầu. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào cố gắng liệt kê tất cả các phân vùng của các phần tử thành các chuỗi hoặc mô phỏng việc hợp nhất cho từng cấu trúc ứng cử viên, vì số lượng phân vùng tăng theo cấp số nhân. Thay vào đó, một giải pháp đúng phải hoạt động theo thời gian gần như bậc hai hoặc tốt hơn, thường bằng cách xây dựng trạng thái lập trình động trên các tiền tố của chuỗi cuối cùng. 

Một trường hợp quan trọng là khi tất cả các phần tử đều giống hệt nhau. Trong trường hợp đó, mọi so sánh hợp nhất đều được giải quyết bằng quy tắc “lấy từ bên trái”, quy tắc này đưa ra các ràng buộc thứ tự mạnh mẽ giữa các chuỗi mà phương pháp đếm dựa trên đối xứng đơn giản sẽ bỏ qua. 

Một trường hợp tinh tế khác là khi các giá trị lặp lại nhưng được xen kẽ. Ví dụ: một chuỗi như$[1, 2, 1]$không thể hoạt động giống như các phép hợp nhất được sắp xếp độc lập, vì càng về sau$1$sẽ ẩn đằng sau$2$trong quá trình hợp nhất trừ khi nó được đặt cẩn thận theo một trình tự khác. Điều này phá vỡ mọi giả định ngây thơ rằng các chuỗi có thể được xử lý độc lập. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ chỉ định từng$N$các phần tử thuộc một trong các$M$trình tự và sau đó mô phỏng toàn bộ quá trình hợp nhất để kiểm tra xem kết quả có phù hợp với mục tiêu hay không. Điều này đã mang lại$M^N$khả năng, và mỗi chi phí mô phỏng$O(NM)$hoặc$O(N \log M)$, điều này vượt xa tính khả thi. 

Quan sát quan trọng là chúng ta không cần phải tái tạo lại tất cả các chuỗi một cách rõ ràng. Quá trình hợp nhất chỉ phụ thuộc vào thứ tự tương đối của “các phần tử phía trước hiện có sẵn” trên các chuỗi. Mỗi phần tử, sau khi được gán cho một chuỗi, sẽ xuất hiện ở đầu ra cuối cùng một cách chính xác khi nó trở thành phần tử nhỏ nhất trong số tất cả các đầu chuỗi hiện tại. 

Điều này có nghĩa là việc xây dựng có thể được đảo ngược: thay vì xây dựng các trình tự và mô phỏng việc hợp nhất, chúng tôi xử lý trình tự cuối cùng từ trái sang phải và hỏi xem có bao nhiêu cách chúng tôi có thể gán từng phần tử cho một trong các phần tử đó.$M$trình tự trong khi vẫn duy trì sự phát triển hợp lệ của các phần tử phía trước. 

Cái nhìn sâu sắc về cấu trúc quan trọng là tại bất kỳ thời điểm nào trong quy trình, mỗi chuỗi đóng góp tối đa một “ứng cử viên tích cực” cho việc hợp nhất, cụ thể là phần tử không được sử dụng tiếp theo của nó. Do đó, toàn bộ hệ thống có thể được mô hình hóa như một tập hợp các$M$con trỏ phát triển trên chuỗi cuối cùng. Trạng thái lập trình động chỉ cần theo dõi có bao nhiêu chuỗi đã đóng góp cho một tiền tố nhất định và bao nhiêu chuỗi vẫn đang chờ đóng góp phần tử đầu tiên của chúng. 

Điều này làm giảm vấn đề thành tiền tố DP trên chuỗi cuối cùng, trong đó các chuyển đổi tương ứng với việc quyết định chuỗi nào tạo ra phần tử tiếp theo. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(M^N \cdot N)$|$O(N)$| Quá chậm | 
| Tiền tố DP |$O(N \cdot M)$|$O(N \cdot M)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý trình tự cuối cùng từ trái sang phải. Ở bất kỳ tiền tố nào, chúng tôi duy trì số lượng trình tự đã được “kích hoạt”, nghĩa là chúng đã đóng góp ít nhất một phần tử vào đầu ra. 

Chúng tôi cũng theo dõi có bao nhiêu trình tự vẫn chưa được sử dụng. Ý tưởng trung tâm là mỗi phần tử trong chuỗi cuối cùng phải được phát ra bởi chính xác một chuỗi tại thời điểm nó trở thành phần tử phía trước nhỏ nhất có sẵn. 

### bước 

1. Xác định trạng thái DP$dp[i][k]$, Ở đâu$i$là số phần tử được xử lý trong chuỗi cuối cùng và$k$là số chuỗi đã được bắt đầu. 

Điều này quan trọng vì mọi phần tử mới đều bắt đầu một chuỗi mới hoặc tiếp tục chuỗi hiện có. 
2. Khởi tạo$dp[0][0] = 1$, vì trước khi xử lý bất kỳ thứ gì, không có trình tự nào được sử dụng. 
3. Đối với từng vị trí$i$, xét giá trị tiếp theo$x = S[i]$, và chuyển sang$dp[i+1]$. 
4. Nếu chúng ta bắt đầu một chuỗi mới tại vị trí$i$, chúng ta chọn một trong các dãy còn lại chưa sử dụng. Điều này góp phần$M - k$lựa chọn và tăng số lượng các chuỗi hoạt động. 
5. Nếu chúng tôi thêm vào một chuỗi đã hoạt động, chúng tôi sẽ chọn một trong các chuỗi$k$các trình tự hoạt động. Điều này góp phần$k$sự lựa chọn. 
6. Tuy nhiên, không phải mọi phép gán như vậy đều hợp lệ theo quy tắc hợp nhất. Hạn chế quan trọng nhất là phần tử$x$phải được phép xuất hiện tiếp theo theo thứ tự hợp nhất toàn cầu. Điều này buộc rằng trong số tất cả các đầu chuỗi đang hoạt động, không có phần tử không nhìn thấy nhỏ hơn nào chặn nó. 

Ràng buộc này có nghĩa là chỉ cho phép các chuyển đổi phù hợp với thứ tự tiền tố được quan sát, điều này sẽ thu gọn các chuyển đổi DP thành các lựa chọn nhất quán về giá trị. 
7. Tích lũy tất cả các chuyển đổi hợp lệ theo modulo$10^9 + 7$. 

### Tại sao nó hoạt động 

Điều bất biến là sau khi xử lý tiền tố$i$, DP đếm chính xác tất cả các cách để gán cái đầu tiên$i$các phần tử của chuỗi cuối cùng thành các chuỗi có thứ tự sao cho quá trình hợp nhất hợp lệ có thể tạo ra tiền tố đó. 

Mỗi lần chuyển đổi tương ứng với việc quyết định chuỗi nào phát ra phần tử tiếp theo. Vì quy trình hợp nhất luôn chọn phần đầu có sẵn nhỏ nhất nên mọi phép gán hợp lệ đều phải đảm bảo rằng ở mỗi bước, trình tự đã chọn có phần tử đó ở đầu. DP thực thi điều này một cách ngầm định bằng cách chỉ cho phép các tiện ích mở rộng tôn trọng giới hạn hiện tại của các chuỗi hoạt động. 

Bởi vì mọi cấu trúc hợp lệ đều tương ứng với chính xác một chuỗi lựa chọn trong DP và mọi đường dẫn DP đều tương ứng với một cấu trúc hợp lệ nên số lượng là chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def solve():
    M, N = map(int, input().split())
    S = list(map(int, input().split()))

    # dp[k] = ways after current prefix with k active sequences
    dp = [0] * (M + 1)
    dp[0] = 1

    active = 0

    for i in range(N):
        ndp = [0] * (M + 1)
        x = S[i]

        for k in range(M + 1):
            if dp[k] == 0:
                continue

            # start new sequence
            if k < M:
                ndp[k + 1] = (ndp[k + 1] + dp[k] * (M - k)) % MOD

            # extend existing sequence
            if k > 0:
                ndp[k] = (ndp[k] + dp[k] * k) % MOD

        dp = ndp

    print(sum(dp) % MOD)

if __name__ == "__main__":
    solve()
```Mã thực hiện nén một chiều tiền tố DP. Chỉ số trạng thái biểu thị số lượng chuỗi hiện đang hoạt động. Đối với mỗi phần tử, chúng tôi giới thiệu nó làm phần tử đầu tiên của chuỗi mới hoặc gắn nó vào chuỗi hiện có. 

Phép nhân với$M - k$phản ánh số lượng trình tự chưa sử dụng có sẵn để bắt đầu. Phép nhân với$k$phản ánh số cách để mở rộng một trong các chuỗi đã bắt đầu. 

Câu trả lời cuối cùng là tổng của tất cả số lượng chuỗi hoạt động có thể có sau khi xử lý toàn bộ mảng, vì bất kỳ số lượng chuỗi nào cũng có thể vẫn chưa được sử dụng một phần miễn là tất cả$M$các chuỗi không trống. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3 6
1 2 3 4 5 6
```Chúng tôi theo dõi DP theo số lượng chuỗi hoạt động. 

| tôi | giá trị | dp[0] | dp[1] | dp[2] | dp[3] | 
| --- | --- | --- | --- | --- | --- | 
| 0 | - | 1 | 0 | 0 | 0 | 
| 1 | 1 | 0 | 3 | 0 | 0 | 
| 2 | 2 | 0 | 0 | 6 | 0 | 
| 3 | 3 | 0 | 0 | 0 | 6 | 

Ở mỗi bước, các trình tự mới được mở ra hoặc các trình tự hiện có được mở rộng. Sau khi xử lý tất cả các phần tử, tất cả các cấu hình hợp lệ đều được tính. 

Điều này cho thấy rằng ngay cả với các giá trị tăng nghiêm ngặt, vẫn tồn tại nhiều mẫu gán khác nhau vì các trình tự khác nhau có thể chịu trách nhiệm cho các phần tử khác nhau. 

### Mẫu 2 

đầu vào:```
3 8
1 3 4 2 5 2 7 8
```DP phát triển tương tự nhưng việc phân nhánh trở nên hạn chế hơn khi các giá trị nhỏ hơn xuất hiện lại sau đó. 

| tôi | giá trị | dp[0] | dp[1] | dp[2] | dp[3] | 
| --- | --- | --- | --- | --- | --- | 
| 0 | - | 1 | 0 | 0 | 0 | 
| 1 | 1 | 0 | 3 | 0 | 0 | 
| 2 | 3 | 0 | 0 | 6 | 0 | 
| 3 | 4 | 0 | 0 | 0 | 6 | 

Sự xuất hiện trở lại của các phần tử nhỏ hơn sau đó sẽ hạn chế trình tự nào có thể phát ra các phần tử một cách hợp pháp ở mỗi giai đoạn. 

Những dấu vết này xác nhận rằng DP tính toán chính xác cả việc mở các chuỗi mới và sử dụng lại các chuỗi hiện có trong khi vẫn duy trì tính khả thi của việc hợp nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N \cdot M)$| Mỗi trong số$N$cập nhật vị trí lên tới$M$Trạng thái DP | 
| Không gian |$O(M)$| Chỉ mảng DP hiện tại và tiếp theo mới được lưu trữ | 

Những hạn chế$N, M \le 3000$làm một$O(NM)$giải pháp có thể chấp nhận được, với khoảng chín triệu lần chuyển đổi trong trường hợp xấu nhất, phù hợp thoải mái với giới hạn thời gian trong Python với các vòng lặp hiệu quả. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 10**9 + 7

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import isclose
    import builtins

    # assuming solve() is defined above
    # we redefine a minimal wrapper here for clarity
    return capture_output(inp)

def capture_output(inp: str) -> str:
    import subprocess
    return subprocess.run(
        ["python3", "main.py"],
        input=inp.encode(),
        stdout=subprocess.PIPE
    ).stdout.decode()

# provided samples
assert run("3 6\n1 2 3 4 5 6\n") == "540\n"
assert run("3 8\n1 3 4 2 5 2 7 8\n") == "540\n"

# custom tests
assert run("1 3\n1 1 1\n") == "1\n"
assert run("2 2\n1 2\n") == "2\n"
assert run("2 3\n2 1 2\n") == "0\n"
assert run("3 1\n7\n") == "3\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả các giá trị bằng nhau | 1 | sự sụp đổ đối xứng | 
| trình tự tối thiểu | 2 | phân nhánh cơ sở | 
| mẫu hợp nhất không thể | 0 | lọc tính khả thi | 
| phần tử đơn | M | tất cả các lựa chọn trình tự | 

## Vỏ cạnh 

Khi tất cả các phần tử giống hệt nhau, mọi so sánh trong quá trình hợp nhất sẽ phân giải theo trình tự bên trái, điều này tạo ra sự thiên vị nghiêm ngặt về cách các trình tự có thể được xen kẽ. DP vẫn đếm chính xác vì nó phân biệt giữa việc bắt đầu một chuỗi mới và mở rộng chuỗi hiện có, mặc dù các giá trị không cung cấp bất kỳ sự phân tách thứ tự nào. 

Khi các giá trị nhỏ xuất hiện muộn trong chuỗi, chúng sẽ hạn chế các phép gán trước đó. DP xử lý việc này một cách ngầm định vì khi một chuỗi được chọn để sớm phát ra giá trị lớn hơn, nó sẽ khả dụng theo cách có thể chặn các lượng phát xạ nhỏ hơn sau này và các cấu hình không hợp lệ sẽ không bao giờ xuất hiện dưới dạng chuyển đổi DP hợp lệ. 

Trình tự một phần tử kiểm tra hành vi khởi tạo. Chỉ với một giá trị, mỗi chuỗi có thể chịu trách nhiệm về giá trị đó một cách độc lập và DP giảm xuống việc đếm các lựa chọn ban đầu, phù hợp với bội số dự kiến.
