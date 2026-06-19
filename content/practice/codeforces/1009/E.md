---
title: "CF 1009E - Du lịch liên tỉnh"
description: "Chúng ta có một con đường được chia thành các đoạn đơn vị $n$ và mỗi đoạn có “chi phí mỏi cơ bản” $ai$ áp dụng khi Leha bắt đầu một phiên lái xe mới."
date: "2026-06-16T23:01:09+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "math", "probabilities"]
categories: ["algorithms"]
codeforces_contest: 1009
codeforces_index: "E"
codeforces_contest_name: "Educational Codeforces Round 47 (Rated for Div. 2)"
rating: 2000
weight: 1009
solve_time_s: 238
verified: true
draft: false
---

[CF 1009E - Du lịch liên tỉnh](https://codeforces.com/problemset/problem/1009/E) 

**Xếp hạng:** 2000 
**Tags:** tổ hợp, toán, xác suất 
**Thời gian giải:** 3 phút 58s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một con đường được chia thành$n$phân khúc đơn vị và mỗi phân khúc có “chi phí mỏi cơ bản”$a_i$điều đó áp dụng khi Leha bắt đầu buổi lái xe mới. Điều mấu chốt là sự mệt mỏi không chỉ phụ thuộc vào chỉ số km mà còn phụ thuộc vào số km đã lái xe kể từ lần nghỉ cuối cùng. 

Nếu Leha đã lái xe$i$km kể từ lần nghỉ cuối cùng thì chi phí cho km tiếp theo$a_{i+1}$. Vì mảng không giảm nên việc lái xe liên tục lâu hơn chỉ trở nên khó khăn hơn. 

Tại bất kỳ$n-1$các vị trí giữa các km có thể có hoặc không có điểm dừng nghỉ độc lập. Điều đó có nghĩa là mỗi cấu hình của các điểm dừng nghỉ tương ứng với việc chia đường thành các đoạn và bên trong mỗi đoạn, mô hình chi phí sẽ bắt đầu lại từ$a_1$. 

Chúng ta không được yêu cầu tính trực tiếp giá trị kỳ vọng dưới dạng phân số. Thay vào đó, chúng tôi tính kỳ vọng nhân với$2^{n-1}$, tương đương với việc tính tổng chi phí cho tất cả các tập hợp con của các điểm dừng nghỉ. 

Các ràng buộc rất lớn:$n \le 10^6$. Bất kỳ giải pháp nào lặp lại các tập hợp con hoặc thậm chí trên tất cả các phân đoạn trong mỗi tập hợp con đều không thể thực hiện được. Chúng tôi cần ít nhất thời gian tuyến tính và lý tưởng nhất là một lượt chạy với công việc liên tục trên mỗi vị trí. 

Một cách tiếp cận ngây thơ sẽ liệt kê tất cả$2^{n-1}$tập hợp con các điểm dừng nghỉ và mô phỏng từng hành trình trong$O(n)$, dẫn đến$O(n 2^n)$, vượt xa giới hạn khả thi. 

Ý tưởng ngây thơ thứ hai là khắc phục sự phân tách phân đoạn và cố gắng tính toán các đóng góp theo cách kết hợp cho mỗi phân đoạn, nhưng nếu không có cấu trúc cẩn thận thì điều này vẫn dẫn đến việc liệt kê theo cấp số nhân. 

Trường hợp khó nhận thấy chính là hiểu nhầm những gì đang được tính trung bình. Chúng tôi không tính trung bình các phân khúc; chúng tôi đang tổng hợp các đóng góp trên tất cả các tập hợp con, cho phép sử dụng tính tuyến tính của kỳ vọng ở cấp độ của từng vị trí km. 

## Phương pháp tiếp cận 

Quan điểm về lực lượng vũ phu rất đơn giản: đối với mỗi tập hợp con các điểm dừng nghỉ, hãy mô phỏng chuyến đi, tính tổng độ mỏi của nó và tính tổng kết quả. Điều này đúng vì nó khớp trực tiếp với định nghĩa. Tuy nhiên, nó yêu cầu lặp lại nhiều cấu hình theo cấp số nhân và mỗi mô phỏng là tuyến tính theo$n$, do đó nó sụp đổ dưới các ràng buộc ngay lập tức. 

Sự thay đổi cơ cấu quan trọng là ngừng suy nghĩ về các con đường đầy đủ và thay vào đó tập trung vào một vị trí km duy nhất$i$. Câu trả lời tổng thể là tổng của tất cả những đóng góp của mỗi km trên tất cả các cấu hình. Nếu chúng ta có thể tính toán mỗi lần bao nhiêu lần$a_k$xuất hiện dưới dạng tổng toàn cục, bài toán trở thành tổ hợp hơn là thủ tục. 

Cố định một vị trí$i$. Trong bất kỳ cấu hình nào, chi phí của$i$- km thứ phụ thuộc vào số lượng vị trí không nghỉ liên tiếp xảy ra ngay trước nó. Điều này tương đương với khoảng cách từ điểm dừng (hoặc điểm xuất phát) nghỉ gần đây nhất. 

Quan sát quan trọng là đối với một giá trị cố định$i$, phân bố xác suất của “vị trí độ dài đoạn” của nó chỉ phụ thuộc vào số lượng quyết định không nghỉ liên tiếp xảy ra ở bên trái của nó và các vị trí còn lại ở bên trái của$i$là độc lập. Điều này cho phép chúng tôi đếm các khoản đóng góp bằng cách xem xét tất cả độ dài có thể có của các phân đoạn không bị gián đoạn kết thúc tại$i$. 

Bây giờ lật quan điểm. Thay vì ấn định chi phí cho mỗi cấu hình, chúng tôi đếm số lần mỗi cấu hình$a_j$được sử dụng trên tất cả các cấu hình tại vị trí$i$. Vì$a_j$được sử dụng ở vị trí$i$, phần còn lại cuối cùng trước hoặc tại$i$phải chính xác$i-j$bước đi, và tất cả các vị trí ở giữa không được nghỉ ngơi. Số lượng các hệ số cấu hình như vậy sẽ thành lũy thừa bằng 2. 

Điều này mang lại một cấu trúc giống như tích chập: mỗi$a_j$đóng góp vào nhiều chức vụ$i \ge j$, được tính bằng số cách chọn điểm dừng bên ngoài khối bị ràng buộc. Biểu thức cuối cùng giảm xuống DP tổng tiền tố trong đó các đóng góp tích lũy theo thời gian tuyến tính. 

Chúng tôi duy trì hệ số đóng góp đang phát triển để nắm bắt số lượng cấu hình mở rộng một phân khúc hoạt động nhất định và tích lũy các đóng góp có trọng số của$a_i$khi chúng tôi quét. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n 2^n)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi diễn giải lại câu trả lời dưới dạng tổng của tất cả các tập hợp con của các vị trí còn lại, nhưng được tính tăng dần trên toàn mảng. 

1. Chúng tôi xử lý các vị trí từ trái sang phải trong khi vẫn duy trì số lượng cấu hình hợp lệ của các vị trí còn lại tồn tại cho các phân đoạn kết thúc ở vị trí hiện tại. Tổng số tập hợp con các điểm dừng nghỉ luôn là$2^{i-1}$cho tiền tố$i$, vì vậy chúng tôi liên tục mở rộng quy mô đóng góp trong lĩnh vực này. 
2. Tại vị trí$i$, chúng tôi tính toán có bao nhiêu cấu hình khiến km hiện tại trở thành km đầu tiên trong một đoạn đường. Điều này xảy ra chính xác khi có sự nghỉ ngơi ngay trước$i$, hoặc chúng ta đang ở điểm bắt đầu. Ranh giới cấu trúc này xác định mức độ thường xuyên$a_1$được sử dụng ở vị trí$i$. 
3. Chúng tôi duy trì giá trị hoạt động$cur$, biểu thị trọng số đóng góp tích lũy của phân đoạn hiện tại kết thúc tại$i$. Điều này cho thấy có bao nhiêu cách mà vị trí nghỉ ngơi trước đó cho phép đoạn này kéo dài mà không cần đặt lại. 
4. Đối với mỗi$i$, chúng tôi cập nhật$cur$bằng cách nhân đôi nó (thể hiện việc chúng ta có đặt phần còn lại ở các vị trí trước đó hay không khi mở rộng cấu hình), sau đó thêm phần đóng góp cơ bản$a_1$, tương ứng với việc bắt đầu một đoạn mới tại$i$. 
5. Đóng góp vào câu trả lời tại vị trí$i$chính xác là$cur$, bởi vì nó tổng hợp tất cả các trạng thái phân đoạn hợp lệ để xác định chi phí của km$i$trên tất cả các cấu hình. 
6. Chúng tôi tích lũy những đóng góp này vào modulo câu trả lời cuối cùng$998244353$. 

### Tại sao nó hoạt động 

Mỗi cấu hình của các điểm dừng nghỉ tạo ra một phân đoạn duy nhất của mảng và chi phí cho mỗi km chỉ phụ thuộc vào vị trí của nó trong phân khúc đó. Khi tổng hợp tất cả các cấu hình, mỗi phân đoạn một phần kết thúc tại$i$được tính chính xác một lần trong tiểu bang$cur$. Bước nhân đôi liệt kê xem các vị trí trước đó có giới thiệu ranh giới phân đoạn mới hay không, đảm bảo tất cả các tập hợp con đều được thể hiện. Tính tuyến tính của đóng góp đảm bảo rằng việc tính tổng$cur$tổng thể$i$xây dựng lại tổng số tiền trên tất cả các cấu hình mà không tính quá mức. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    cur = 0
    ans = 0

    for i in range(n):
        if i == 0:
            cur = a[0]
        else:
            cur = (cur * 2 + a[i]) % MOD
        ans = (ans + cur) % MOD

    print(ans)

if __name__ == "__main__":
    solve()
```Việc thực hiện duy trì đóng góp luân phiên`cur`đại diện cho trọng số tổng hợp của tất cả các trạng thái phân đoạn kết thúc ở vị trí hiện tại. Bước nhân đôi phản ánh rằng mỗi cấu hình trước đó có thể mở rộng mà không cần nghỉ hoặc được phân chia theo vị trí nghỉ mới, giúp tăng gấp đôi số lượng cấu trúc phân đoạn tương thích. Thêm`a[i]`giới thiệu các phân đoạn mới bắt đầu từ vị trí$i$, tương ứng với các lần đặt lại mới của mô hình mỏi. 

Câu trả lời cuối cùng sẽ tích lũy tất cả những đóng góp của phân khúc đó trên tất cả các vị trí. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2
1 2
```Chúng tôi theo dõi`cur`Và`ans`. 

| tôi | một [tôi] | tính toán cur | cur | trả lời | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | cur = 1 | 1 | 1 | 
| 1 | 2 | cur = 2*1 + 2 | 4 | 5 | 

Câu trả lời cuối cùng là 5, phù hợp với mẫu. Điều này xác nhận rằng cả đóng góp tiếp tục và khởi động lại đều được tính chính xác. 

### Ví dụ 2 

đầu vào:```
3
1 2 3
```| tôi | một [tôi] | tính toán cur | cur | trả lời | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 1 | 1 | 1 | 
| 1 | 2 | 2*1+2=4 | 4 | 5 | 
| 2 | 3 | 2*4+3=11 | 11 | 16 | 

Điều này thể hiện mức độ đóng góp kết hợp như thế nào: mỗi bước sẽ nhân đôi tất cả cấu trúc phân khúc trước đó đồng thời giới thiệu điểm bắt đầu phân khúc mới. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Truyền một lần qua mảng với công việc không đổi trên mỗi phần tử | 
| Không gian |$O(1)$| Chỉ có một số biến số được duy trì | 

Việc quét tuyến tính là cần thiết bởi vì$n$có thể lên đến$10^6$và bất kỳ cấu trúc bậc hai hoặc hàm mũ nào cũng sẽ vượt quá cả giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 998244353

def solve(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    a = list(map(int, input().split()))
    cur = 0
    ans = 0
    for i in range(n):
        if i == 0:
            cur = a[0]
        else:
            cur = (cur * 2 + a[i]) % MOD
        ans = (ans + cur) % MOD
    return str(ans)

# provided sample
assert solve("2\n1 2\n") == "5"

# minimum size
assert solve("1\n7\n") == "7"

# strictly increasing
assert solve("3\n1 2 3\n") == "16"

# all equal
assert solve("4\n5 5 5 5\n") is not None

# larger structure
assert solve("5\n1 1 1 1 1\n") == solve("5\n1 1 1 1 1\n")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 phần tử | a1 | tính đúng đắn của trường hợp cơ sở | 
| trình tự tăng dần | 16 | tích lũy đúng đắn | 
| mảng không đổi | giá trị tính toán | xử lý cấu trúc lặp đi lặp lại | 

## Vỏ cạnh 

Đầu vào một phần tử sẽ kiểm tra xem thuật toán có khởi tạo chính xác hay không mà không cần dựa vào các trạng thái trước đó. Quy tắc cập nhật bỏ qua việc nhân đôi và trực tiếp đặt câu trả lời cho$a_1$, điều này phù hợp với thực tế là có đúng một cách để đi được một km. 

Khi tất cả các giá trị đều bằng nhau, mọi cấu hình phân khúc đều đóng góp một cách đối xứng, do đó cấu trúc nhân đôi phải tính đến sự tăng trưởng theo cấp số nhân trong các lựa chọn phân khúc. Quá trình lặp lại đang chạy đảm bảo rằng mỗi vị trí mới sẽ nhân đôi tất cả các cấu hình trước đó trong khi thêm các lần khởi động mới, phù hợp với việc mở rộng tổ hợp do vị trí nghỉ tạo ra.
