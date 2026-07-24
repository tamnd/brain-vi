---
title: "CF 103765F - \u6458\u6a58\u5b50"
description: "Chúng ta được đưa ra một số kịch bản độc lập về một vườn cam. Trong mỗi kịch bản, có $n$ cây và cây $i$-th chứa cam $ai$. Từ mỗi cây, chúng ta được phép chọn bất kỳ số nguyên cam nào trong khoảng từ $0$ đến $ai$, độc lập với các cây khác."
date: "2026-07-02T08:55:49+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103765
codeforces_index: "F"
codeforces_contest_name: "2022 Collegiate Programming Contest of Xiangtan University"
rating: 0
weight: 103765
solve_time_s: 50
verified: true
draft: false
---

[CF 103765F - \u6458\u6a58\u5b50](https://codeforces.com/problemset/problem/103765/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được đưa ra một số kịch bản độc lập về một vườn cam. Trong mỗi kịch bản đều có$n$cây cối và$i$-cây thứ có chứa$a_i$cam. Từ mỗi cây, chúng ta được phép chọn bất kỳ số nguyên cam nào giữa$0$Và$a_i$, độc lập với các cây khác. Sau khi chọn tất cả các cây, chúng ta thu được tổng số cam đã hái. 

Ngoài ra còn có một số cố định$m$, đại diện cho đồng đội. Một kế hoạch hái hợp lệ là một kế hoạch trong đó tổng số cam đã hái có thể được chia đều cho tất cả các đồng đội, nghĩa là tổng của tất cả các giá trị đã chọn sẽ chia hết cho$m$. Nhiệm vụ là đếm xem có bao nhiêu phương án hái khác nhau thỏa mãn điều kiện chia hết này, trong đó hai phương án khác nhau nếu có ít nhất một cây đóng góp số lượng cam hái khác nhau. 

Mỗi cây hoạt động giống như một biến bị chặn đóng góp vào một tổng và chúng ta đang đếm xem có bao nhiêu cách chọn các biến này sao cho tổng nằm trong một lớp đồng dư cụ thể modulo$m$. 

Những ràng buộc ngụ ý rằng$n$có thể lên đến$10^5$mỗi bộ thử nghiệm và$m \le 50$. Số lượng ca kiểm thử rất lớn, có thể lên tới$10^3$, nhưng tổng cộng$n$trên tất cả các bài kiểm tra cũng bị giới hạn bởi$10^5$. Điều này gợi ý mạnh mẽ một giải pháp gần tuyến tính trong$n$cho mỗi bài kiểm tra, với một hệ số bổ sung tùy thuộc vào$m$, trong khi mọi thứ bậc hai trong$n$là không thể. 

Một bảng liệt kê ngây thơ về tất cả các lựa chọn sẽ được xem xét$\prod (a_i + 1)$cấu hình lớn về mặt thiên văn ngay cả đối với đầu vào vừa phải. Ngay cả việc lưu trữ hoặc lặp lại tất cả các khoản tiền cũng không thể thực hiện được. 

Trường hợp cạnh tinh tế hơn xuất hiện khi tất cả$a_i$lớn và$m = 1$. Trong trường hợp đó mọi lựa chọn đều hợp lệ, vì vậy câu trả lời phải là$\prod (a_i + 1)$modulo$998244353$. Bất kỳ phương pháp nào chỉ theo dõi dư lượng mà không tính đến tổng số lượng vẫn phải duy trì hành vi này. 

Một trường hợp góc khác là khi tất cả$a_i = 0$. Chỉ có một cách để chọn không có gì, và tổng là$0$, luôn chia hết cho$m$. Câu trả lời phải luôn là$1$, độc lập với$m$. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp là coi mỗi cây đóng góp một đa thức nhỏ: từ cây$i$, chúng ta có thể chọn$0,1,\dots,a_i$, do đó đóng góp của nó cho hàm tạo là$x^0 + x^1 + \dots + x^{a_i}$. Tổng số cách hợp lệ là tổng hệ số của tất cả các đơn thức có số mũ chia hết cho$m$trong tích của tất cả các đa thức này. 

Việc mở rộng trực tiếp sản phẩm này là không thể vì độ lớn lên tới$10^5$mỗi thuật ngữ và số lượng thuật ngữ bùng nổ theo kiểu tổ hợp. 

Quan sát quan trọng là chúng ta không quan tâm đến tổng chính xác mà chỉ quan tâm đến giá trị modulo của nó.$m$. Điều này thu gọn không gian trạng thái từ tiềm năng$10^5 \cdot n$số tiền có thể chỉ$m \le 50$các lớp dư lượng. Điều này ngay lập tức gợi ý một chương trình động trên dư lượng. 

Chúng tôi duy trì một mảng DP trong đó`dp[r]`đếm xem có bao nhiêu cách để đạt được tổng số bằng$r \bmod m$. Ban đầu,`dp[0] = 1`. Đối với mỗi cây, chúng tôi tính toán xem nó đóng góp bao nhiêu cách cho mỗi lớp dư lượng, sau đó kết hợp nó với DP toàn cầu. 

Đối với một cây có giá trị$a$, định nghĩa`cnt[t]`bằng số số nguyên trong$[0,a]$phù hợp với$t \bmod m$. Điều này có thể được tính theo thời gian không đổi trên mỗi dư lượng bằng cách sử dụng phương pháp đếm lũy tiến số học. Sau đó, cây đóng góp một đa thức nhỏ cho phần dư và chúng tôi cập nhật DP bằng tích chập trên modulo$m$. 

Chi phí cập nhật mỗi cây$O(m^2)$, cho tổng cộng$O(n m^2)$, điều này có thể chấp nhận được vì$m \le 50$và tổng cộng$n \le 10^5$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Liệt kê lực lượng vũ phu |$O(\prod (a_i+1))$|$O(n)$| Quá chậm | 
| DP trên dư lượng |$O(n m^2)$|$O(m)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập. 

1. Khởi tạo một mảng DP có kích thước$m$, với tất cả các giá trị bằng 0 ngoại trừ`dp[0] = 1`. Điều này thể hiện rằng trước khi chọn bất kỳ cây nào, tổng duy nhất có thể đạt được là 0. 
2. Đối với mỗi cây$i$, tính xem có bao nhiêu số nguyên$[0, a_i]$rơi vào từng lớp dư lượng modulo$m$. Chúng tôi làm điều này bằng cách viết đầu tiên$a_i = q \cdot m + r$. Mỗi dư lượng xuất hiện chính xác$q$thời gian và dư lượng$0$bởi vì$r$xuất hiện thêm một lần nữa. Điều này mang lại một mảng tần số`cnt`. 

Bước này quan trọng vì nó nén một phạm vi lớn thành một biểu diễn nhỏ gọn trên các phần dư mà không làm mất thông tin tổ hợp. 
3. Tạo mảng DP mới`ndp`được khởi tạo về 0. Chúng tôi sẽ tính toán sự chuyển đổi từ tổng trước sang tổng mới sau khi xử lý cây này. 
4. Đối với mọi dư lượng$j$ở trạng thái DP hiện tại và mọi dư lượng$k$từ sự đóng góp của cây hiện tại, hãy thêm:$$ndp[(j+k)\bmod m] += dp[j] \cdot cnt[k]$$Điều này thể hiện việc chọn một phần tổng có dư$j$, sau đó thêm lựa chọn từ cây hiện tại có phần dư$k$. 
5. Sau khi đổ đầy`ndp`, thay thế`dp`với`ndp`. 
6. Sau khi xử lý xong tất cả các cây, câu trả lời là`dp[0]`, vì chúng tôi chỉ muốn tổng chia hết cho$m$. 

Tính chính xác phụ thuộc vào việc theo dõi tất cả các tổng từng phần được nhóm theo phần dư. Ở mỗi bước,`dp[r]`đếm chính xác số cách để có được một tổng bằng$r$. Quá trình chuyển đổi bảo toàn tính bất biến này vì mọi phần mở rộng của cấu hình một phần hợp lệ được tính chính xác một lần và mod bổ sung dư lượng$m$cập nhật chính xác lớp học. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def solve():
    T = int(input())
    for _ in range(T):
        n, m = map(int, input().split())
        a = list(map(int, input().split()))

        dp = [0] * m
        dp[0] = 1

        for x in a:
            q, r = divmod(x, m)

            cnt = [q] * m
            for i in range(r + 1):
                cnt[i] += 1

            ndp = [0] * m
            for j in range(m):
                if dp[j] == 0:
                    continue
                for k in range(m):
                    if cnt[k] == 0:
                        continue
                    ndp[(j + k) % m] = (ndp[(j + k) % m] + dp[j] * cnt[k]) % MOD

            dp = ndp

        print(dp[0] % MOD)

if __name__ == "__main__":
    solve()
```Mảng DP`dp`lưu trữ số lượng dư lượng có thể đạt được. Đối với mỗi cây, mảng`cnt`nén phạm vi lựa chọn thành tần số dư lượng. Các vòng lặp lồng nhau thực hiện phép tích chập trên các lớp modulo. Mô-đun này được áp dụng trong quá trình chuyển đổi để tránh tràn và phù hợp với định dạng đầu ra được yêu cầu. 

Một điểm tinh tế là xử lý`cnt`: nó luôn có tổng bằng$a_i + 1$, do đó tổng số cách được giữ nguyên trong suốt quá trình chuyển đổi. Hoạt động modulo chỉ được áp dụng cho chỉ số dư lượng cuối cùng, không áp dụng cho chính cấu trúc đếm lựa chọn. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào:```
n = 3, m = 2
a = [1, 1, 1]
```Mỗi cây cho phép chọn 0 hoặc 1. Vì vậy, mỗi cây đóng góp một số 0 và một số 1. 

Sau cây đầu tiên, DP là: 

| dư lượng | dp | 
| --- | --- | 
| 0 | 1 | 
| 1 | 1 | 

Sau cây thứ hai: 

| j (dp cũ) | cnt | đóng góp | 
| --- | --- | --- | 
| 0 | {0:1,1:1} | thêm vào 0 và 1 | 
| 1 | {0:1,1:1} | thêm vào 1 và 0 | 

Kết quả dp: 

| dư lượng | dp | 
| --- | --- | 
| 0 | 2 | 
| 1 | 2 | 

Sau cây thứ ba, phép biến đổi tương tự được áp dụng, mang lại: 

| dư lượng | dp | 
| --- | --- | 
| 0 | 4 | 
| 1 | 4 | 

Vậy câu trả lời là`dp[0] = 4`. 

Dấu vết này cho thấy rằng DP vẫn đối xứng vì mỗi cây đóng góp cấu trúc phần dư giống hệt nhau và tính bất biến mà dp thể hiện sự tích chập đầy đủ đối với các lựa chọn được giữ nguyên. 

Bây giờ hãy xem xét:```
n = 1, m = 3
a = [4]
```Chúng ta có thể chọn từ 0 đến 4, cho dư lượng: 

0,1,2,0,1. Vậy số đếm là: 

| dư lượng | cnt | 
| --- | --- | 
| 0 | 2 | 
| 1 | 2 | 
| 2 | 1 | 

Bắt đầu từ dp = [1,0,0], sau khi tích chập chúng ta nhận được dp = [2,2,1]. Đáp án là 2, tương ứng với việc chọn 0 hoặc 3 quả cam. 

Điều này xác nhận rằng việc nén cặn chính xác giải thích sự phân bố không đồng đều khi$a_i$không phải là bội số của$m$. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n m^2)$| Mỗi trong số$n$cây thực hiện phép xoắn trên$m$trạng thái dư lượng | 
| Không gian |$O(m)$| Chỉ có hai mảng DP có kích thước$m$được duy trì | 

Với$m \le 50$và tổng cộng$n \le 10^5$, tổng số hoạt động là khoảng$2.5 \times 10^8$trong trường hợp xấu nhất ở dạng hệ số không đổi, nhưng với các vòng lặp bên trong nhỏ và các phép toán số nguyên chặt chẽ, điều này phù hợp với giới hạn 2,5 giây thông thường trong PyPy hoặc Python được tối ưu hóa. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 998244353

def solve():
    input = sys.stdin.readline
    T = int(input())
    for _ in range(T):
        n, m = map(int, input().split())
        a = list(map(int, input().split()))
        dp = [0] * m
        dp[0] = 1
        for x in a:
            q, r = divmod(x, m)
            cnt = [q] * m
            for i in range(r + 1):
                cnt[i] += 1
            ndp = [0] * m
            for j in range(m):
                if dp[j] == 0:
                    continue
                for k in range(m):
                    if cnt[k] == 0:
                        continue
                    ndp[(j + k) % m] = (ndp[(j + k) % m] + dp[j] * cnt[k]) % MOD
            dp = ndp
        print(dp[0])

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    old_stdout = sys.stdout
    sys.stdout = out
    solve()
    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return out.getvalue().strip()

# sample-style tests
assert run("1\n3 2\n1 1 1\n") == "4"
assert run("1\n1 3\n4\n") == "2"

# edge cases
assert run("1\n3 1\n1000000000 1000000000 1000000000\n") == str((1000000001*1000000001*1000000001)%MOD)
assert run("1\n1 5\n0\n") == "1"
assert run("1\n2 2\n0 0\n") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả những cái, m=2 | 4 | tính đúng tích chập cơ bản | 
| phạm vi lớn duy nhất | 2 | phân bố cặn không đồng đều | 
| m=1 giá trị lớn | tích của (a_i+1) | hành vi cạnh mô đun | 
| tất cả số không | 1 | cấu hình tầm thường | 
| nhiều cây số 0 | 1 | sự ổn định không có lựa chọn | 

## Vỏ cạnh 

Khi nào$m = 1$, mọi tổng đều chia hết cho$m$, vì vậy DP sẽ tính toán tất cả các lựa chọn có thể một cách hiệu quả. Thuật toán xử lý việc này vì`cnt`luôn có tổng bằng$a_i + 1$, Và`dp`chỉ có một trạng thái, do đó nó nhân tất cả các đóng góp một cách chính xác mà không có bất kỳ sự trộn lẫn dư lượng nào. 

Khi tất cả$a_i = 0$, mỗi`cnt`mảng có`cnt[0] = 1`và tất cả các mục khác bằng không. DP không bao giờ thay đổi so với trạng thái ban đầu, vì vậy`dp[0]`vẫn bằng 1, phù hợp với thực tế là có đúng một cách để không chọn gì cả. 

Khi$n$lớn nhưng tất cả$a_i < m$, sự phân bố bên trong`cnt`trở nên rất thưa thớt, nhưng phép tích chập vẫn hoạt động chính xác vì nó lặp lại rõ ràng trên tất cả các phần dư và giữ nguyên các mục không.
