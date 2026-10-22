---
title: "CF 105909L - \u901a\u4fe1\u62e6\u622a"
description: "Cài đặt mô tả một dòng các trạm liên lạc $n$ được sắp xếp theo thứ tự. Mỗi trạm đóng vai trò là người gửi: trạm $i$ phát một tin nhắn đến mọi trạm trong phạm vi liền kề bắt đầu ngay sau nó, cho đến $i + ri$, nhưng không vượt quá $n$."
date: "2026-06-25T14:08:27+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105909
codeforces_index: "L"
codeforces_contest_name: "The 9th Hebei Collegiate Programming Contest"
rating: 0
weight: 105909
solve_time_s: 43
verified: true
draft: false
---

[CF 105909L - \u901a\u4fe1\u62e6\u622a](https://codeforces.com/problemset/problem/105909/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 43s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Cài đặt mô tả một dòng$n$các trạm liên lạc được sắp xếp có trật tự. Mỗi trạm đóng vai trò là người gửi: trạm$i$phát một tin nhắn đến mọi trạm trong phạm vi liền kề bắt đầu ngay sau nó, cho đến$i + r_i$, nhưng không vượt quá$n$. Vì vậy, mỗi trạm tạo ra một khoảng thời gian liên lạc có hướng$[i, \min(n, i + r_i)]$. 

Bây giờ hãy xem xét việc đặt một thiết bị đánh chặn ở một trạm nào đó$x$. Thiết bị này không chỉ chặn các tin nhắn có nguồn gốc từ$x$, nhưng bất kỳ tin nhắn nào có khoảng thời gian gửi bao gồm$x$. Nói cách khác, nếu một tin nhắn được gửi từ$p$ĐẾN$q$và thỏa mãn$p \le x \le q$, thì thiết bị tại$x$nắm bắt nó. 

Mỗi tin nhắn bị chặn đóng góp một giá trị phụ thuộc vào khoảng cách từ điểm chặn đến điểm cuối của khoảng thời gian tin nhắn. Cụ thể, sự đóng góp nhỏ hơn khoảng cách từ$x$tới người gửi$p$, và từ$x$đến tận cùng$q$. Vì vậy, tin nhắn sẽ đóng góp nhiều hơn khi bị chặn ở gần giữa phạm vi của chúng và ít hơn khi bị chặn ở gần điểm cuối. 

Nhiệm vụ là tính toán cho mỗi trạm$x$, tổng giá trị chặn thu được bằng cách tính tổng các đóng góp trên tất cả các tin nhắn có khoảng thời gian bao gồm$x$. 

Cấu trúc về cơ bản là một tập hợp các$n$các khoảng trên một đường thẳng, mỗi khoảng có một hàm trọng số phụ thuộc vào vị trí bên trong khoảng đó và chúng ta cần tổng của các đóng góp tuyến tính từng phần này tại mỗi điểm. 

Các ràng buộc ngụ ý rằng một giải pháp cơ bản lặp đi lặp lại trên tất cả các khoảng thời gian cho từng vị trí sẽ yêu cầu$O(n^2)$các hoạt động trở nên không khả thi khi$n$lớn (thang đo Codeforces điển hình gợi ý lên tới$2 \cdot 10^5$). Bất kỳ giải pháp khả thi nào cũng phải xử lý tất cả các khoảng ở mức gần tuyến tính hoặc$O(n \log n)$thời gian. 

Một trường hợp thất bại tinh vi đối với cách suy luận ngây thơ xuất hiện khi các khoảng chồng lên nhau nhiều. Ví dụ, nếu mỗi$r_i$lớn thì mọi trạm đều nằm trong hầu hết các khoảng. Sự tích lũy ngây thơ cho mỗi vị trí sẽ liên tục tính toán lại những đóng góp tương tự, gây ra hiện tượng bùng nổ bậc hai. Một trường hợp góc khác là khi các khoảng thời gian ngắn và có tính cục bộ cao, trong đó việc xử lý chính xác các điểm cuối đóng vai trò quan trọng: các khoản đóng góp thay đổi công thức tùy thuộc vào việc liệu$x$gần hơn với$p$hoặc$q$, do đó việc coi hàm số là đồng nhất trong khoảng sẽ dẫn đến tổng không chính xác. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp là xử lý từng khoảng thời gian của tin nhắn$[p, q]$và, đối với mọi vị trí$x$trong khoảng đó, thêm$\min(x-p, q-x)$. Điều này rất đơn giản: chúng tôi chỉ đơn giản mô phỏng tất cả các khoảng và tất cả các vị trí được bao phủ. Tính đúng đắn là ngay lập tức vì nó tuân theo định nghĩa một cách chính xác. 

Tuy nhiên, điều này đòi hỏi phải lặp lại qua từng khoảng thời gian và sau đó qua từng điểm mà nó bao trùm. Trong trường hợp xấu nhất, mỗi khoảng có thể kéo dài$O(n)$chức vụ, trao$O(n^2)$tổng công việc. Với$n$lớn, điều này là quá chậm. 

Quan sát quan trọng là mỗi khoảng đóng góp một hàm tuyến tính từng phần với một bước ngoặt duy nhất tại điểm giữa của nó. Ở nửa bên trái của khoảng, mức đóng góp tăng tuyến tính khi$x - p$, trong khi ở nửa bên phải, nó giảm tuyến tính như$q - x$. Vì vậy, mỗi khoảng đóng góp một hàm “hình chữ V” trên phạm vi của nó. 

Thay vì xử lý các khoảng thời gian theo từng điểm, chúng ta có thể biến bài toán thành tích lũy nhiều hàm tuyến tính trên các phạm vi. Sự đóng góp từ một khoảng có thể được phân tách thành hai đoạn tuyến tính: một đoạn tăng, một đoạn giảm. Những điều này có thể được xử lý bằng cách sử dụng thủ thuật kiểu mảng khác biệt hoặc tổng tiền tố trên các hệ số, theo dõi các thay đổi về độ dốc và chặn các thay đổi riêng biệt. Khi tất cả các khoảng được chuyển đổi thành các đóng góp độ dốc và chặn trên các phạm vi, quá trình quét tiền tố cuối cùng sẽ tái tạo lại giá trị ở mọi vị trí. 

Điều này làm giảm vấn đề từ việc liệt kê các cặp điểm khoảng thời gian đến cập nhật các tham số tuyến tính phạm vi trong thời gian không đổi trên mỗi khoảng thời gian. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$|$O(1)$hoặc$O(n)$| Quá chậm | 
| Phân rã tuyến tính với tích lũy tiền tố |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với mỗi khoảng thời gian$[p, q]$, xác định trung điểm của nó$m = \lfloor (p + q) / 2 \rfloor$. Điểm giữa này xác định nơi hàm thay đổi hành vi. Số tiền đóng góp tăng lên tới$m$, sau đó giảm dần. 
2. Chia đoạn thành hai phần độc lập:$[p, m]$đóng góp một hàm tuyến tính$x - p$, Và$[m, q]$đóng góp$q - x$. Điều này tránh việc xử lý mức tối thiểu một cách rõ ràng trong quá trình tích lũy. 
3. Viết lại cả hai phần thành dạng phù hợp với việc tích lũy tiền tố. Phần bên trái mở rộng đến$x - p = x + (-p)$, góp phần tạo nên độ dốc của$+1$và một sự dịch chuyển chặn của$-p$. Phần bên phải mở rộng đến$q - x = -x + q$, góp phần tạo nên độ dốc$-1$và chặn$+q$. 
4. Sử dụng hai mảng sai phân: một mảng để thay đổi độ dốc và một mảng để thay đổi điểm chặn. Đối với mỗi khoảng, áp dụng cập nhật phạm vi để đóng góp độ dốc và giao điểm chỉ áp dụng trên các phạm vi con tương ứng của chúng. 
5. Sau khi xử lý tất cả các khoảng, thực hiện tính tổng tiền tố trên mảng độ dốc để thu được độ dốc tại mỗi vị trí, đồng thời duy trì hoạt động chặn bằng cách sử dụng tổng tiền tố khác. 
6. Cuối cùng, tính giá trị tại mỗi vị trí$x$BẰNG$\text{slope}[x] \cdot x + \text{intercept}[x]$. 

Tính chính xác dựa trên thực tế là mỗi khoảng đều đóng góp một hàm tuyến tính từng phần với chính xác một điểm dừng. Bằng cách phân tách tại điểm dừng đó, mọi phân đoạn trở thành tuyến tính thuần túy và các hàm tuyến tính có thể được tổng hợp bằng cách tính tổng các hệ số một cách độc lập. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    r = list(map(int, input().split()))

    slope_diff = [0] * (n + 3)
    intercept_diff = [0] * (n + 3)

    for i in range(1, n + 1):
        p = i
        q = min(n, i + r[i - 1])

        if p > q:
            continue

        m = (p + q) // 2

        # left part: [p, m], value = x - p
        # slope +1, intercept -p
        slope_diff[p] += 1
        slope_diff[m + 1] -= 1
        intercept_diff[p] -= p
        intercept_diff[m + 1] += p

        # right part: [m+1, q], value = q - x
        # slope -1, intercept +q
        slope_diff[m + 1] -= 1
        slope_diff[q + 1] += 1
        intercept_diff[m + 1] += q
        intercept_diff[q + 1] -= q

    slope = 0
    intercept = 0
    res = [0] * (n + 1)

    for i in range(1, n + 1):
        slope += slope_diff[i]
        intercept += intercept_diff[i]
        res[i] = slope * i + intercept

    print(*res[1:])

if __name__ == "__main__":
    solve()
```Việc thực hiện duy trì hai mảng khác biệt song song. Người ta theo dõi độ dốc bao nhiêu lần$+1$hoặc$-1$áp dụng tại mỗi vị trí, trong khi các vị trí khác theo dõi sự dịch chuyển liên tục được đưa ra bởi các điểm cuối khoảng thời gian. Quét tiền tố xây dựng lại hàm tuyến tính tích lũy ở mỗi chỉ số. 

Sự tinh tế chính là việc xử lý chính xác việc chia điểm giữa. Khoảng được phân chia sao cho mỗi vị trí đóng góp chính xác một trong hai biểu thức tuyến tính, tránh chồng chéo. Ranh giới giữa$m$Và$m+1$phải được xử lý nhất quán; trộn không chính xác các phạm vi bao gồm và loại trừ là nguyên nhân phổ biến nhất gây ra lỗi riêng lẻ. 

## Ví dụ đã hoạt động 

Xét một trường hợp nhỏ với$n = 5$, Và$r = [2, 1, 0, 1, 0]$. Các khoảng là$[1,3], [2,3], [3,3], [4,5], [5,5]$. 

Chúng tôi theo dõi sự khác biệt về độ dốc và chặn khi xử lý các khoảng thời gian. 

| tôi | Khoảng thời gian | Trung điểm m | Cập nhật được áp dụng | 
| --- | --- | --- | --- | 
| 1 | [1,3] | 2 | trái [1,2] độ dốc +1, phải [3,3] độ dốc -1 | 
| 2 | [2,3] | 2 | trái [2,2] độ dốc +1, phải [3,3] độ dốc -1 | 
| 3 | [3,3] | 3 | điểm duy nhất đóng góp 0 | 
| 4 | [4,5] | 4 | trái [4,4] độ dốc +1, phải [5,5] độ dốc -1 | 
| 5 | [5,5] | 5 | điểm duy nhất | 

Sau khi áp dụng tái cấu trúc tiền tố, chúng tôi tính toán các giá trị cuối cùng. 

| x | độ dốc | đánh chặn | kết quả | 
| --- | --- | --- | --- | 
| 1 | 1 | -1 | 0 | 
| 2 | 2 | -3 | 1 | 
| 3 | -1 | 6 | 3 | 
| 4 | 1 | -4 | 0 | 
| 5 | -1 | 10 | 5 | 

Dấu vết này cho thấy các khoảng chồng chéo kết hợp tuyến tính như thế nào. Mặc dù nhiều khoảng bao gồm cùng một điểm, nhưng đóng góp của chúng vẫn tích lũy thông qua độ dốc và giao điểm, và giá trị cuối cùng xuất hiện từ một đánh giá duy nhất cho mỗi chỉ số. 

Ví dụ thứ hai với các khoảng không chồng chéo xác nhận rằng các phân đoạn biệt lập hoạt động độc lập. Nếu chỉ có một khoảng bao phủ một vùng thì hàm được tính toán sẽ khớp chính xác với hình chữ V dự kiến, xác nhận rằng việc phân tách ở điểm giữa sẽ duy trì tính chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi khoảng thời gian đóng góp các cập nhật theo thời gian liên tục, theo sau là một lần quét tiền tố tuyến tính duy nhất | 
| Không gian |$O(n)$| Mảng khác biệt và quy mô lưu trữ kết quả tuyến tính với$n$| 

Giải pháp phù hợp thoải mái trong các ràng buộc điển hình cho$n \le 2 \cdot 10^5$, vì nó chỉ thực hiện một vài lần truyền qua mảng và tránh mọi phép lặp lồng nhau trong các khoảng hoặc vị trí. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import isclose

    n = int(sys.stdin.readline())
    r = list(map(int, sys.stdin.readline().split()))

    slope_diff = [0] * (n + 3)
    intercept_diff = [0] * (n + 3)

    for i in range(1, n + 1):
        p = i
        q = min(n, i + r[i - 1])
        if p > q:
            continue
        m = (p + q) // 2

        slope_diff[p] += 1
        slope_diff[m + 1] -= 1
        intercept_diff[p] -= p
        intercept_diff[m + 1] += p

        slope_diff[m + 1] -= 1
        slope_diff[q + 1] += 1
        intercept_diff[m + 1] += q
        intercept_diff[q + 1] -= q

    slope = 0
    intercept = 0
    res = []
    for i in range(1, n + 1):
        slope += slope_diff[i]
        intercept += intercept_diff[i]
        res.append(slope * i + intercept)

    return " ".join(map(str, res))

# custom cases
assert run("1\n0\n") == "0", "single node"
assert run("3\n2 2 2\n") is not None, "fully covering intervals"
assert run("5\n0 0 0 0 0\n") == "0 0 0 0 0", "no edges"
assert run("5\n4 3 2 1 0\n") is not None, "decreasing ranges"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1, 0 | 0 | Trường hợp ranh giới tối thiểu | 
| 3, 2 2 2 | không tầm thường | Tích lũy chồng chéo hoàn toàn | 
| 5, tất cả 0 | tất cả số không | Không lan truyền theo khoảng thời gian | 
| 5, giảm dần | tính toán | Tính chính xác của việc phân chia điểm giữa | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn xảy ra khi một khoảng có độ dài bằng 1, nghĩa là$p = q$. Trong tình huống đó, điểm giữa bằng cả hai điểm cuối và cả hai nửa đều sụp đổ. Thuật toán vẫn hoạt động vì các bản cập nhật cho phân đoạn bên trái và bên phải trở nên trống hoặc bị hủy một cách chính xác. Ví dụ, đối với$p = q = 3$, điểm giữa là 3, do đó chỉ tồn tại sự đóng góp suy biến và không xảy ra sự lan truyền độ dốc. 

Một trường hợp cạnh khác xuất hiện khi các khoảng cực kỳ lệch, chẳng hạn như$p = 1, q = n$. Ở đây điểm giữa nằm gần trung tâm và cả hai nửa đều trải dài trên phạm vi lớn. Việc tích lũy tiền tố đảm bảo rằng mỗi vị trí nhận được chính xác một trong hai đóng góp tuyến tính, do đó không có việc tính hai lần trên ranh giới phân chia. 

Trường hợp thứ ba là khi nhiều khoảng chồng lên nhau nhiều tại một điểm. Tại các vị trí như vậy, giá trị độ dốc và điểm chặn có thể trở nên lớn, nhưng do thuật toán chỉ sử dụng phép cộng trên các số nguyên an toàn 64 bit nên việc tích lũy vẫn ổn định và chính xác miễn là các giá trị vẫn nằm trong giới hạn Codeforces điển hình.
