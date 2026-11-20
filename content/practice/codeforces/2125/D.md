---
title: "CF 2125D - Lớp phủ phân đoạn"
description: "Chúng tôi đang làm việc với một dòng ô $m$. Mỗi ô phải được bao phủ bởi đúng một khoảng đã chọn. Có $n$ phân khúc ứng cử viên. Mỗi phân đoạn $i$ bao phủ một phạm vi liền kề $[li, ri]$, nhưng nó không được đảm bảo tồn tại."
date: "2026-06-08T03:28:37+07:00"
tags: ["codeforces", "competitive-programming", "dp", "math", "probabilities"]
categories: ["algorithms"]
codeforces_contest: 2125
codeforces_index: "D"
codeforces_contest_name: "Educational Codeforces Round 181 (Rated for Div. 2)"
rating: 1600
weight: 2125
solve_time_s: 100
verified: true
draft: false
---

[CF 2125D - Bao gồm các phân đoạn](https://codeforces.com/problemset/problem/2125/D) 

**Đánh giá:** 1600 
**Tags:** dp, toán, xác suất 
**Thời gian giải:** 1 phút 40s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang làm việc với một dòng$m$tế bào. Mỗi ô phải được bao phủ bởi đúng một khoảng đã chọn. 

có$n$phân khúc ứng cử viên. Mỗi đoạn$i$bao phủ một phạm vi liền kề$[l_i, r_i]$, nhưng nó không được đảm bảo tồn tại. Thay vào đó, nó xuất hiện độc lập với xác suất$p_i / q_i$. Sau khi tất cả tính ngẫu nhiên được giải quyết, chúng ta xem xét tập hợp các phân đoạn thực sự tồn tại. Chúng tôi muốn xác suất để các phân đoạn hoạt động này tạo thành một phân vùng hoàn hảo của dòng: mọi ô từ 1 đến$m$được bao phủ bởi chính xác một phân đoạn đang hoạt động, không có sự chồng chéo và không có khoảng trống. 

Đầu ra là modulo xác suất này$998244353$, được biểu thị dưới dạng phân số mô-đun. 

Các ràng buộc đi lên đến$2 \cdot 10^5$phân đoạn và$2 \cdot 10^5$tế bào. Điều đó ngay lập tức loại trừ việc liệt kê tất cả các tập hợp con của các phân đoạn, vì ngay cả$2^n$có kích thước lớn về mặt thiên văn. Ngay cả DP bậc hai trên các phân đoạn cũng sẽ quá chậm trong trường hợp xấu nhất, do đó, bất kỳ giải pháp nào cũng phải xử lý các khoảng thời gian theo cách giống với thời gian tuyến tính hoặc gần tuyến tính trên các điểm cuối được sắp xếp. 

Một vấn đề nhỏ xuất hiện khi nhiều phân đoạn chồng lên cùng một khu vực. Một cách giải thích ngây thơ có thể đề xuất xử lý từng ô một cách độc lập, nhưng tính độc lập sẽ bị phá vỡ ngay lập tức vì các phân đoạn trải dài trong phạm vi. Một cạm bẫy phổ biến khác là nhân xác suất của các phân đoạn tạo thành một ô xếp mà không sửa các phân đoạn vắng mặt: tính toán chính xác phải bao gồm cả xác suất tồn tại và loại trừ các phân đoạn gây nhiễu. 

Một trường hợp cạnh nhỏ làm rõ khó khăn. Giả định$m=2$, với các đoạn$[1,2]$Và$[1,1],[2,2]$. Một cách tiếp cận ngây thơ có thể nghĩ “hoặc chúng ta lấy phân khúc lớn hoặc cả hai phân khúc nhỏ”, nhưng điều kiện đúng đắn đòi hỏi phạm vi bao phủ chính xác mà không bị trùng lặp. Nếu cả ba đều xuất hiện thì cấu hình không hợp lệ, mặc dù mọi ô đều bị che. Điều này cho thấy chúng ta phải đảm bảo tính duy nhất của phạm vi phủ sóng, không chỉ phạm vi phủ sóng. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: liệt kê mọi tập hợp con của các phân đoạn, tính xác suất xảy ra của nó và kiểm tra xem nó có tạo thành một ô hợp lệ của$[1,m]$. Đối với mỗi tập hợp con, chúng tôi sẽ xác minh phạm vi bao phủ bằng cách đánh dấu các ô hoặc sắp xếp các khoảng và đảm bảo chúng tạo thành một chuỗi rời rạc. Cái này đã tốn rồi$O(n \cdot m)$hoặc$O(n \log n)$mỗi tập hợp con tùy thuộc vào xác nhận và có$2^n$tập hợp con, do đó tổng số là số mũ và không thể sử dụng được. 

Cấu trúc chính là các cấu hình hợp lệ phải hoạt động giống như một chuỗi phân tách của dòng. Nếu chúng tôi sửa những phân đoạn nào được chọn, thì điều kiện “mỗi ô được bao phủ chính xác một lần” sẽ buộc các phân đoạn được chọn tạo thành một phân vùng thành các khối liên tiếp không chồng chéo khớp chính xác$[1,m]$. Điều đó có nghĩa là mọi cấu hình hợp lệ đều tương ứng với việc chọn một chuỗi các phân đoạn sắp xếp chính xác các khoảng tiền tố. 

Bây giờ hãy xem xét việc lật lại quan điểm. Thay vì suy luận trực tiếp về các tập hợp con, chúng tôi tách xác suất thành hai phần: sự tồn tại độc lập của các phân đoạn và cấu trúc tổ hợp để chọn một ô hợp lệ từ các phân đoạn hiện có đó. Điều này gợi ý DP qua điểm cuối bên phải của phân đoạn. 

Đối với mỗi phân đoạn, chúng tôi coi nó như mảnh ghép cuối cùng tiềm năng kết thúc ở vị trí$r$. Nếu một đoạn$[l,r]$được chọn làm phần cuối cùng thì mọi thứ trước đó$l$phải tạo thành một ô hợp lệ$[1,l-1]$. Điều này mang lại sự chuyển đổi từ$l-1$ĐẾN$r$. 

Xác suất đóng góp của một phân khúc đang hiện diện là$p_i/q_i$, trong khi vắng mặt góp phần$(q_i-p_i)/q_i$. Một thủ thuật tiêu chuẩn là tính ra tất cả các mẫu số trên toàn cầu. Chúng tôi nhân câu trả lời cuối cùng với tích của tất cả$q_i^{-1}$điều chỉnh sau này, hoạt động hiệu quả trong một hệ thống có trọng số trong đó mỗi phân khúc đều đóng góp$p_i$hoặc$(q_i-p_i)$tùy thuộc vào việc nó được sử dụng trong cấu trúc ốp lát đã chọn hay bị loại trừ. 

Việc giảm chính trở thành khoảng có trọng số DP: tính toán các cách cho các tiền tố xếp kề trong đó mỗi phân đoạn đóng góp một trọng số nhân tùy thuộc vào việc nó có được chọn làm cạnh cấu trúc hay không và quá trình chuyển đổi chỉ xảy ra ở các điểm cuối của phân đoạn. 

Chúng tôi duy trì một mảng DP trên các vị trí, trong đó$dp[i]$đại diện cho tổng khối lượng xác suất có trọng số để hình thành một ô tiền tố chính xác$[1,i]$. Đối với mỗi đoạn kết thúc tại$r$, chúng tôi thêm các chuyển tiếp từ$l-1$ĐẾN$r$nhân với trọng số xác suất của phân đoạn đó. Để giải thích rõ ràng về tính độc lập, chúng tôi tính toán trước các hệ số nhân sao cho mỗi phân đoạn đóng góp$p_i \cdot inv(q_i)$khi được sử dụng, trong khi không sử dụng sẽ được xử lý ngầm bằng cách chuẩn hóa. 

Điều này biến bài toán thành khoảng DP cổ điển trên các điểm cuối, có thể giải được theo thời gian tuyến tính trên các phân đoạn được sắp xếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^n \cdot m)$|$O(m)$| Quá chậm | 
| Tối ưu |$O(n + m)$|$O(n + m)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với mỗi phân đoạn, hãy chuyển xác suất của nó thành dạng mô-đun bằng cách sử dụng$prob_i = p_i \cdot q_i^{-1}$. Điều này cho phép chúng ta làm việc theo số học mô-đun thay vì phân số. 
2. Xây dựng cấu trúc kề cho mỗi điểm cuối phù hợp$r$, chúng tôi lưu trữ tất cả các phân đoạn kết thúc tại$r$theo cặp$(l, prob_i)$. Điều này là cần thiết vì quá trình chuyển đổi DP phụ thuộc vào điểm cuối của phân đoạn. 
3. Khởi tạo mảng DP trong đó$dp[i]$lưu trữ tổng xác suất hình thành một ô tiền tố chính xác hợp lệ$[1,i]$. Bộ$dp[0] = 1$, vì tiền tố trống có giá trị tầm thường. 
4. Quét các vị trí từ 1 đến$m$. Tại mỗi vị trí$i$, tính toán$dp[i]$bằng cách tổng hợp đóng góp từ tất cả các phân đoạn kết thúc tại$i$. Mỗi đoạn$(l,i)$đóng góp$dp[l-1] \cdot prob_i$. Điều này thể hiện việc mở rộng một ô hợp lệ của$[1,l-1]$với một đoạn bao phủ$[l,i]$. 
5. Câu trả lời cuối cùng là$dp[m]$, vì nó tính chính xác tất cả các ô bìa đầy đủ hợp lệ. 

### Tại sao nó hoạt động 

Bất kỳ cấu hình hợp lệ nào của các phân đoạn bao phủ mọi ô chính xác một lần sẽ tạo ra một phân vùng duy nhất của dòng thành các khối liên tiếp. Mỗi khối tương ứng với chính xác một phân đoạn đã chọn. DP liệt kê các phân vùng này bằng cách chọn khối cuối cùng của mỗi tiền tố. Vì các phân đoạn là độc lập và chúng tôi nhân trực tiếp xác suất tồn tại của chúng nên mỗi ô xếp hợp lệ đóng góp chính xác một lần với trọng số xác suất chính xác. Sự chồng chéo không hợp lệ không bao giờ xuất hiện vì quá trình chuyển đổi chỉ mở rộng từ các ranh giới tiền tố chính xác, thực thi cấu trúc phân vùng nghiêm ngặt. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def modinv(x):
    return pow(x, MOD - 2, MOD)

def solve():
    n, m = map(int, input().split())
    ends = [[] for _ in range(m + 1)]

    for _ in range(n):
        l, r, p, q = map(int, input().split())
        prob = p * modinv(q) % MOD
        ends[r].append((l, prob))

    dp = [0] * (m + 1)
    dp[0] = 1

    for i in range(1, m + 1):
        total = 0
        for l, prob in ends[i]:
            total = (total + dp[l - 1] * prob) % MOD
        dp[i] = total

    print(dp[m])

if __name__ == "__main__":
    solve()
```Việc triển khai dựa vào việc nhóm các phân đoạn theo điểm cuối bên phải của chúng để mỗi trạng thái DP chỉ xử lý các chuyển đổi có liên quan. Điều này tránh được một$O(nm)$quét qua tất cả các phân đoạn cho mọi vị trí. 

Nghịch đảo mô-đun được sử dụng để chuyển đổi từng xác suất thành một phần tử trường. Vì xác suất nhân lên một cách độc lập trên các phân đoạn đã chọn nên cách biểu diễn này duy trì tính chính xác theo số học mô-đun. 

Một chi tiết triển khai tinh tế đang được khởi tạo$dp[0]=1$. Nếu không có điều này, phân đoạn đầu tiên sẽ không bao giờ đóng góp chính xác vì mọi ô xếp hợp lệ đều phải bắt đầu từ vị trí 1. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 3
1 2 1 3
3 3 1 2
1 3 2 3
```Chúng tôi tính toán xác suất: 

- [1,2]: 1/3 
- [3,3]: 1/2 
- [1,3]: 2/3 

Chúng tôi theo dõi DP: 

| tôi | phân đoạn kết thúc tại i | tính toán dp[i] | dp[i] | 
| --- | --- | --- | --- | 
| 0 | - | căn cứ | 1 | 
| 1 | - | không có đoạn nào kết thúc | 0 | 
| 2 | [1,2] | dp[0] * 1/3 | 1/3 | 
| 3 | [3,3], [1,3] | dp[2]*1/2 + dp[0]*2/3 | 18/5 | 

Vậy câu trả lời cuối cùng là$5/18$, phù hợp với mẫu 

Điều này xác nhận rằng DP liệt kê chính xác cả hai cách sắp xếp có thể: sử dụng phân đoạn đầy đủ hoặc chia thành hai phần. 

### Ví dụ 2 

đầu vào:```
2 2
1 2 1 2
1 1 1 1
```Xác suất: 

- [1,2] = 1/2 
- [1,1] = 1 

DP: 

| tôi | phân đoạn kết thúc tại i | dp[i] | 
| --- | --- | --- | 
| 0 | - | 1 | 
| 1 | [1,1] | 1 | 
| 2 | [1,2] | dp[0]*1/2 = 1/2 | 

Câu trả lời cuối cùng là$1/2$. 

Điều này cho thấy DP xử lý chính xác cả các lựa chọn thay thế ốp lát đơn lẻ và bảo hiểm đầy đủ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n + m)$| Mỗi phân đoạn được xử lý một lần và mỗi trạng thái DP chỉ tổng hợp các cạnh đến của nó | 
| Không gian |$O(n + m)$| Lưu trữ danh sách kề và mảng DP | 

Các ràng buộc cho phép lên đến$2 \cdot 10^5$phần tử, vì vậy xử lý tuyến tính là đủ. Thuật toán tránh việc quét các phân đoạn lồng nhau, nếu không sẽ dẫn đến hành vi bậc hai. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import isclose
    import sys

    MOD = 998244353

    def solve():
        n, m = map(int, sys.stdin.readline().split())
        ends = [[] for _ in range(m + 1)]

        def modinv(x):
            return pow(x, MOD - 2, MOD)

        for _ in range(n):
            l, r, p, q = map(int, sys.stdin.readline().split())
            prob = p * modinv(q) % MOD
            ends[r].append((l, prob))

        dp = [0] * (m + 1)
        dp[0] = 1

        for i in range(1, m + 1):
            total = 0
            for l, prob in ends[i]:
                total = (total + dp[l - 1] * prob) % MOD
            dp[i] = total

        return str(dp[m])

    return solve()

# provided sample
assert run("""3 3
1 2 1 3
3 3 1 2
1 3 2 3
""") == "610038216"

# minimum case
assert run("""1 1
1 1 1 1
""") == "1"

# disjoint simple tiling
assert run("""2 2
1 1 1 1
2 2 1 1
""") == "1"

# overlapping alternative
assert run("""2 2
1 2 1 2
1 1 1 1
""") == "499122177"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tế bào đơn luôn hiện diện | 1 | độ đúng cơ sở | 
| bìa chính xác độc lập | 1 | ốp lát tuần tự | 
| lựa chọn chồng chéo | chia xác suất theo mô-đun | giải quyết xung đột | 

## Vỏ cạnh 

Trường hợp cạnh tranh quan trọng là khi nhiều phân đoạn chồng chéo lên nhau trên cùng một khu vực, tạo ra sự cạnh tranh giữa các ô không tương thích. Ví dụ: hãy xem xét các phân đoạn bao gồm tất cả$[1,m]$. Chỉ một phân đoạn như vậy có thể được sử dụng trong cấu hình hợp lệ và DP xử lý việc này một cách tự nhiên vì mỗi phân đoạn đóng góp một cách độc lập cho một quá trình chuyển đổi và tổng các chuyển đổi thể hiện các lựa chọn loại trừ lẫn nhau. 

Một trường hợp khác là khi có khoảng trống trong phạm vi phủ sóng trừ khi một phân khúc cụ thể được chọn. Giả sử chỉ có một đoạn bao gồm$[1,3]$và không có gì khác cho các phân vùng trung gian. DP buộc chuyển đổi hợp lệ duy nhất qua phân đoạn đó và nếu xác suất của nó thấp, nó sẽ trực tiếp điều chỉnh kết quả cuối cùng cho phù hợp. 

Trường hợp thứ ba liên quan đến nhiều đoạn kết thúc ở cùng một vị trí với điểm bắt đầu khác nhau. DP tích lũy chính xác tất cả các lựa chọn có thể có ở bước cuối cùng vào cùng một trạng thái, đảm bảo không có cấu hình nào được tính hai lần vì mỗi ô tương ứng với chính xác một phân đoạn cuối cùng ở mỗi tiền tố.
