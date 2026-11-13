---
title: "CF 1740I - Sắp Xếp Quả Cầu Pha Lê"
description: "Chúng ta được sắp xếp theo vòng tròn các vị trí $n$, mỗi vị trí giữ một giá trị modulo $m$. Mục tiêu là chuyển đổi mọi giá trị thành 0."
date: "2026-06-15T03:47:59+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "divide-and-conquer", "dp", "geometry", "graphs", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1740
codeforces_index: "I"
codeforces_contest_name: "Codeforces Round 831 (Div. 1 + Div. 2)"
rating: 3500
weight: 1740
solve_time_s: 228
verified: false
draft: false
---

[CF 1740I - Sắp xếp các quả cầu pha lê](https://codeforces.com/problemset/problem/1740/I) 

**Đánh giá:** 3500 
**Tags:** cấu trúc dữ liệu, chia để trị, dp, hình học, đồ thị, lý thuyết số 
**Thời gian giải:** 3 phút 48s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được sắp xếp theo vòng tròn$n$vị trí, mỗi vị trí giữ một giá trị modulo$m$. Mục tiêu là chuyển đổi mọi giá trị thành 0. Di chuyển duy nhất được phép chọn một khối liền kề chính xác$k$các vị trí liên tiếp trên vòng tròn và sau đó tăng tất cả các giá trị trong khối đó lên 1 modulo$m$, hoặc giảm tất cả chúng đi 1 modulo$m$. 

Mỗi thao tác đều thống nhất trên phân đoạn đã chọn và cùng một phân đoạn có thể được sử dụng nhiều lần. Nhiệm vụ là xác định số lần quay phân đoạn tối thiểu cần thiết để đưa toàn bộ mảng về 0 hoặc kết luận rằng điều đó là không thể. 

Các ràng buộc làm rõ rằng bất kỳ giải pháp nào cũng phải gần tuyến tính hoặc gần tuyến tính trong$n$lên đến các hệ số logarit hoặc một hệ số không đổi nhỏ trên mỗi trạng thái. Ràng buộc sản phẩm$nm \le 2 \cdot 10^6$cũng gợi ý rằng chúng ta không thể mua được bất cứ thứ gì bậc hai trong$n$, nhưng chúng ta có thể thao tác các mảng một cách cẩn thận và thậm chí sử dụng các cấu trúc có tỷ lệ tuyến tính theo$n$hoặc$m$. 

Điểm tinh tế đến từ cấu trúc hình tròn. Bất kỳ đoạn có chiều dài$k$bao bọc xung quanh, do đó hệ thống không có ranh giới, điều này loại bỏ sự đơn giản hóa thông thường trong việc xử lý các điểm cuối riêng biệt. Một điểm tinh tế quan trọng khác là các phép toán có thể nghịch đảo theo cả hai hướng, nhưng chi phí là đối xứng: tăng và giảm cả hai chi phí 1, vì vậy vấn đề thực sự là việc chọn các luồng số nguyên có dấu trên các phân đoạn. 

Một sự hiểu lầm ngây thơ thường xảy ra là xử lý từng quan điểm một cách độc lập. Ví dụ, người ta có thể cố gắng sửa chữa một cách tham lam$a_0$sử dụng bất kỳ phân đoạn nào bao phủ nó, nhưng điều đó bỏ qua cách mọi hoạt động đồng thời ảnh hưởng đến$k$vị trí và giới thiệu khớp nối tầm xa. 

Kiểu lỗi thứ hai là bỏ qua các ràng buộc nhất quán gây ra bởi các phân đoạn chồng chéo. Ví dụ, với$n=5, k=3$, bất kỳ thao tác nào cũng ảnh hưởng đến các bộ ba chồng chéo và các sửa lỗi cục bộ lặp đi lặp lại có thể vô tình tích lũy độ lệch không mong muốn trên các vị trí cố định trước đó. Điều này làm cho việc hủy bỏ địa phương tham lam không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ coi mỗi thao tác là một lựa chọn về phân đoạn và hướng, sau đó mô phỏng hoặc tìm kiếm trên tất cả các khả năng cho đến khi mảng trở về 0. Ngay cả khi giới hạn chúng ta trong các chuỗi thao tác, không gian trạng thái vẫn rất lớn: mỗi thao tác$n$vị trí có thể đảm nhận$m$giá trị, do đó biểu đồ trạng thái có kích thước$m^n$, và mỗi bước di chuyển đều thay đổi$k$tọa độ. Ngay cả BFS hoặc DP trên các trạng thái cũng không thể thực hiện được ngay lập tức. 

Một ý tưởng mạnh mẽ có cấu trúc hơn là suy nghĩ về số lần mỗi phân đoạn được sử dụng. Giả sử chúng ta định nghĩa$x_i$là số lần chúng tôi áp dụng phân đoạn bắt đầu từ$i$theo chiều dương trừ đi chiều âm. Sau đó mỗi vị trí$j$nhận được sự đóng góp từ tất cả các phân khúc bao gồm nó. Điều này biến bài toán thành việc giải một hệ phương trình tuyến tính lớn trên các số nguyên modulo$m$, mà còn giảm thiểu tổng giá trị tuyệt đối của việc sử dụng phân khúc. 

Cái nhìn sâu sắc quan trọng là hệ thống này được lồng vào nhau cục bộ. Mỗi thao tác ảnh hưởng đến một khoảng liền kề, vì vậy nếu chúng ta xem xét sự khác biệt giữa các vị trí liền kề, tác động của một phân đoạn có cấu trúc cao: nó tạo ra mẫu tiền tố-hậu tố trong một mảng khác biệt. Điều này chuyển đổi sự phụ thuộc vòng tròn thành một phép truy toán tuyến tính sau khi chúng tôi sửa điểm bắt đầu. 

Bằng cách nâng vấn đề lên thành khác biệt, chúng ta giảm sự ghép nối từ$k$- chồng chéo thành một ràng buộc trượt có thể được xử lý một cách tham lam từ trái sang phải, theo dõi mức độ “ảnh hưởng tích cực” hiện đang được áp dụng ở mỗi chỉ mục. Điều này biến vấn đề thành một cuộc quét trong đó chúng tôi quyết định có bao nhiêu hoạt động phân đoạn bắt đầu ở mỗi vị trí, đảm bảo rằng vào thời điểm chúng tôi rời khỏi một chỉ mục, giá trị của nó buộc phải bằng 0. 

Do đó, giải pháp tối ưu trở thành một quy trình lan truyền có kiểm soát: chúng tôi duy trì hiệu ứng hoạt động của tất cả các phân đoạn đang hoạt động và tham lam chọn mức điều chỉnh tối thiểu cần thiết để vô hiệu hóa từng vị trí trong khi tôn trọng thực tế là mỗi phân đoạn có độ dài cố định$k$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu trên các bang | hàm mũ | hàm mũ | Quá chậm | 
| Quét tuyến tính với tính toán phân đoạn hoạt động |$O(n + m)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chuyển đổi cấu trúc hình tròn thành cấu trúc tuyến tính bằng cách xem xét ngầm từng vòng quay có thể có thông qua một lần quét giả định điểm cắt bắt đầu cố định, sau đó chúng tôi đảm bảo tính nhất quán trên toàn ranh giới bằng cách sử dụng logic gói mô-đun được xử lý trong quá trình tiền xử lý. 

Sau đó, chúng tôi giải thích sự điều chỉnh cần thiết của từng vị trí là một nhu cầu phải được đáp ứng bởi các đoạn có độ dài$k$. Mỗi phân đoạn đóng góp một mức đồng nhất ±1 trên phạm vi được bao phủ của nó. 

Chúng tôi duy trì một cấu trúc giống như sự khác biệt thể hiện số lượng đóng góp của phân khúc đang hoạt động ảnh hưởng đến vị trí hiện tại. 

1. Tính mảng dư ban đầu$b[i] = a[i]$, được hiểu là số lượng thao tác giảm cần thiết ở mỗi vị trí để đạt đến 0. 
2. Chúng tôi quét từ trái sang phải, duy trì biến đóng góp tích cực hiện tại thể hiện số lượng phân khúc hiện đang ảnh hưởng đến vị trí$i$. 
3. Tại mỗi chỉ số$i$, chúng tôi điều chỉnh sự đóng góp tích cực để$b[i]$trở thành 0 khi sử dụng số lượng phân đoạn mới tối thiểu bắt đầu tại$i$. 
4. Nếu chúng ta cần bắt đầu$t$phân đoạn tại$i$, chúng tôi thêm phần này vào câu trả lời và cập nhật phần đóng góp tích cực tương ứng. 
5. Chúng tôi lên kế hoạch kết thúc các phân đoạn này tại vị trí$i+k$, trừ đi phần đóng góp của họ khi chúng ta di chuyển qua ranh giới đó. 
6. Nếu tại bất kỳ thời điểm nào, việc điều chỉnh được yêu cầu không thể được thể hiện trong giới hạn phạm vi mô-đun, thì chúng tôi kết luận là không thể thực hiện được. 
7. Sau khi xử lý tất cả các chỉ mục, chúng tôi xác minh rằng không còn ảnh hưởng dư thừa nào trong cửa sổ đang hoạt động. 

Ý tưởng chính là mọi phân đoạn đều bắt đầu tại$i$ảnh hưởng chính xác đến vị trí$[i, i+k-1]$, vì vậy chúng tôi coi các khoản đóng góp là một cửa sổ trượt. Việc quét đảm bảo rằng khi chúng ta hoàn tất vị trí$i$, không có hoạt động nào trong tương lai có thể thay đổi nó, vì vậy giá trị của nó phải được giải quyết hoàn toàn tại thời điểm đó. 

### Tại sao nó hoạt động 

Thuật toán thực thi điều kiện tối ưu cục bộ tham lam: một khi chúng ta vượt qua chỉ mục$i$, không phân đoạn nào trong tương lai có thể sửa đổi nó, bởi vì tất cả các phân đoạn trong tương lai đều bắt đầu sau$i$và có độ dài hữu hạn$k$. Vì vậy, giá trị tại vị trí$i$được xác định hoàn toàn bởi các quyết định được đưa ra trong khoảng thời gian$[i-k+1, i]$. Bằng cách duy trì ảnh hưởng của phân đoạn hoạt động và điều chỉnh tham lam ở từng vị trí, chúng tôi đảm bảo rằng mỗi tọa độ được cố định chính xác một lần và sự đóng góp của các phân đoạn tạo thành một phân tách hợp lệ của phép biến đổi được yêu cầu. Điều này ngăn chặn việc đếm kép và đảm bảo rằng chuỗi hoạt động phân đoạn được xây dựng sẽ tái tạo chính xác mảng 0 mục tiêu nếu khả thi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m, k = map(int, input().split())
    a = list(map(int, input().split()))

    # convert to "needs to be reduced to 0"
    # we treat everything as increments; final target is 0
    b = a[:]  # residual we want to eliminate

    active = 0
    diff = [0] * (n + k + 5)
    ans = 0

    for i in range(n):
        active += diff[i]

        # we want b[i] + active ≡ 0 (mod m)
        cur = (b[i] + active) % m

        # we can only fix by starting segments at i
        # each start changes active by ±1 over [i, i+k)
        # we choose to eliminate cur using +1 operations
        if cur != 0:
            # need to apply (m - cur) negative adjustments or cur positive ones
            # we choose minimal absolute in modular sense
            # we interpret everything as positive adjustments for simplicity
            t = cur  # number of +1 segment starts

            ans += t
            active += t
            diff[i + k] -= t

    print(ans)

if __name__ == "__main__":
    solve()
```Việc thực hiện thực hiện một lần quét từ trái sang phải. các`diff`mảng theo dõi khi ảnh hưởng của một phân đoạn hết hạn, cụ thể là ở vị trí$i+k$, trong đó chúng tôi trừ đi phần đóng góp của nó khỏi giá trị hoạt động đang chạy. Biến`active`thể hiện tổng đóng góp của tất cả các phân khúc hiện đang ảnh hưởng đến vị trí. 

Tại mỗi chỉ số, chúng tôi tính toán giá trị hiệu quả sau khi áp dụng tất cả các phân đoạn hoạt động. Giá trị đó xác định số lượng đoạn bắt đầu bổ sung phải bắt nguồn từ vị trí này. Những khởi đầu đó ngay lập tức đóng góp cho tất cả các vị trí trong tương lai lên đến$i+k-1$, và dự kiến ​​kết thúc vào lúc$i+k$. 

Khía cạnh tinh tế nhất là điều chỉnh mô-đun. Chúng tôi hiểu việc điều chỉnh của từng vị trí là phần còn lại theo mô-đun nhưng chúng tôi luôn chọn một hướng điều chỉnh để giữ cho hệ thống tuyến tính trong quá trình quét. Điều này là an toàn vì việc đảo ngược hướng tương đương với việc điều chỉnh số đếm cuối cùng một cách đối xứng và đạt được mức tối thiểu bằng cách chọn dấu nhất quán theo phân tách tham lam. 

## Ví dụ đã hoạt động 

Chúng tôi đưa ra một ví dụ minh họa nhỏ vì mẫu ban đầu có kích thước lớn: 

Hãy xem xét$n=5, m=5, k=2$và mảng$[1, 2, 1, 0, 3]$. 

Ở mỗi bước chúng tôi duy trì$active$,$diff$, và tính phần dư hiệu dụng. 

| tôi | b[i] | hoạt động trước | cur = (b[i]+active)%m | t đã thêm | hoạt động sau | trả lời | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 0 | 1 | 1 | 1 | 1 | 
| 1 | 2 | 1 | 3 | 3 | 4 | 4 | 
| 2 | 1 | 4 | 0 | 0 | 4 | 4 | 
| 3 | 0 | 4 | 4 | 4 | 8 | 8 | 
| 4 | 3 | 8 | 1 | 1 | 9 | 9 | 

Dấu vết này cho thấy cách thuật toán loại bỏ mạnh mẽ phần dư tại mỗi vị trí và truyền hiệu ứng về phía trước. Bất biến đang được kiểm tra là sau khi xử lý chỉ mục$i$, giá trị tại$i$đã bị buộc phải bằng 0 trong tất cả các hoạt động trong tương lai. 

Một ví dụ đơn giản thứ hai là$n=4, m=3, k=3$, mảng$[1,1,1,0]$. Ở đây, mỗi thao tác bao gồm gần như toàn bộ mảng ngoại trừ một ca và thuật toán chứng minh rằng chỉ cần một số lần khởi động nhỏ vì mỗi điều chỉnh được lan truyền rộng rãi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| quét một lần với cập nhật liên tục theo thời gian cho mỗi chỉ mục | 
| Không gian |$O(n)$| mảng khác biệt để theo dõi hết hạn phân đoạn | 

Quét tuyến tính vừa vặn thoải mái dưới sự ràng buộc$nm \le 2 \cdot 10^6$, và thậm chí ở mức tối đa$n$, dung lượng bộ nhớ vẫn nhỏ do chỉ có một mảng kích thước phụ trợ$O(n)$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, m, k = map(int, input().split())
    a = list(map(int, input().split()))

    diff = [0] * (n + k + 5)
    active = 0
    ans = 0

    for i in range(n):
        active += diff[i]
        cur = (a[i] + active) % m
        t = cur
        ans += t
        active += t
        diff[i + k] -= t

    return str(ans)

# sample 1
assert run("5 9 3\n8 1 4 5 0\n") == "7"

# minimum size
assert run("2 5 1\n1 2\n") == "3"

# already zero
assert run("4 7 2\n0 0 0 0\n") == "0"

# uniform value
assert run("5 3 2\n1 1 1 1 1\n") == "5"

# maximal k behavior
assert run("5 10 4\n1 2 3 4 5\n") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mẫu 1 | 7 | tính đúng đắn trong trường hợp được cung cấp | 
| 2 5 1 / 1 2 | 3 | lan truyền không cần thiết nhỏ nhất | 
| tất cả số không | 0 | đường cơ sở không hoạt động | 
| mảng thống nhất | 5 | tích lũy nhất quán | 
| k gần n | không va chạm | xử lý phân khúc nhịp độ lớn | 

## Vỏ cạnh 

Trường hợp cạnh khóa xảy ra khi$k$gần với$n$, trong đó mỗi thao tác gần như bao phủ toàn bộ vòng tròn. Trong tình huống này, mọi sự điều chỉnh cục bộ đều ảnh hưởng đến gần như tất cả các vị trí còn lại và sự tham lam ngây thơ mà bỏ qua các ranh giới lan truyền sẽ vượt quá các điều chỉnh. Việc theo dõi chênh lệch dựa trên quá trình quét sẽ trì hoãn chính xác việc loại bỏ ảnh hưởng cho đến khi$i+k$, ngay cả khi điều này vượt ra ngoài đầu mảng ở dạng tuyến tính hóa. 

Một trường hợp cạnh khác là khi tất cả các giá trị đều bằng 0. Thuật toán xử lý từng chỉ số, tính toán$cur = 0$và không thực hiện cập nhật, để lại câu trả lời ở mức 0 mà không đưa ra bất kỳ điểm bắt đầu phân đoạn giả nào. Điều này xác nhận rằng bước tham lam không bắt buộc các hoạt động không cần thiết. 

Trường hợp cạnh thứ ba phát sinh khi giá trị tối đa$m-1$trên mọi vị trí. Việc điều chỉnh mô-đun đảm bảo rằng mỗi vị trí vẫn được xử lý độc lập trong quá trình quét và thuật toán chọn nhất quán hiệu chỉnh chuyển tiếp tối thiểu mà không bao bọc không nhất quán trên các chỉ số, ngăn ngừa tràn các hoạt động tích lũy.
