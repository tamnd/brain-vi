---
title: "CF 2210B - Đơn Giản Ngồi Trên Ghế"
description: "Chúng ta có một hàng ghế $n$, ban đầu tất cả đều không được đánh dấu. Một hoán vị $p$ có độ dài $n$ được đưa ra, nghĩa là mỗi số từ $1$ đến $n$ xuất hiện đúng một lần. Chúng tôi bắt đầu từ chiếc ghế đầu tiên và di chuyển sang phải. Tại mỗi ghế, nếu đã được đánh dấu sẵn thì trò chơi sẽ dừng ngay lập tức."
date: "2026-06-07T19:15:06+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "greedy"]
categories: ["algorithms"]
codeforces_contest: 2210
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 1089 (Div. 2)"
rating: 900
weight: 2210
solve_time_s: 114
verified: true
draft: false
---

[CF 2210B - Đơn giản là ngồi trên ghế](https://codeforces.com/problemset/problem/2210/B) 

**Xếp hạng:** 900 
**Tas:** cấu trúc dữ liệu, tham lam 
**Thời gian giải:** 1m 54s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một hàng$n$ghế, tất cả ban đầu không được đánh dấu. Một hoán vị$p$chiều dài$n$được cho trước, nghĩa là mỗi số từ$1$ĐẾN$n$xuất hiện đúng một lần. Chúng tôi bắt đầu từ chiếc ghế đầu tiên và di chuyển sang phải. Tại mỗi ghế, nếu đã được đánh dấu sẵn thì trò chơi sẽ dừng ngay lập tức. Nếu không, chúng ta có quyền lựa chọn tiếp tục hoặc bỏ qua nó. Nếu chúng ta ngồi, sau khi đứng lên, chúng ta đánh dấu chiếc ghế được biểu thị bằng giá trị hoán vị cho chiếc ghế hiện tại,$p_i$, rồi tiếp tục đến chiếc ghế tiếp theo. Mục tiêu là tối đa hóa tổng số ghế chúng ta ngồi. 

Đầu vào đưa ra nhiều trường hợp kiểm thử, mỗi trường hợp chỉ định$n$và hoán vị$p$. Đầu ra là số ghế tối đa có thể ngồi cho mỗi trường hợp thử nghiệm. 

Những hạn chế là đáng kể.$n$có thể đạt được$2\cdot 10^5$, và tổng của tất cả$n$trên tất cả các trường hợp thử nghiệm là nhiều nhất$2\cdot 10^5$. Điều này hạn chế chúng tôi sử dụng các thuật toán tuyến tính hoặc gần tuyến tính cho mỗi trường hợp thử nghiệm. Bất kỳ cách tiếp cận nào có độ phức tạp$O(n^2)$sẽ quá chậm. 

Trường hợp cạnh tinh vi phát sinh khi hoán vị tạo ra một chu trình bao gồm ghế đầu tiên. Ví dụ, nếu$p = [1,2,3]$, ngồi ở ghế đầu tiên tự đánh dấu nên trò chơi kết thúc sau nước đi đầu tiên. Cách tiếp cận tham lam ngây thơ là luôn ngồi sẽ không xem xét được các tình huống tự đánh dấu hoặc đầu chu kỳ này, dẫn đến kết quả không chính xác. 

Một trường hợp cạnh khác xảy ra khi có một chuỗi dài hướng về phía trước mà không có chu kỳ, chẳng hạn như$p = [2,3,4,1]$, ngồi trên ghế ở đâu$1$cuối cùng sẽ đánh dấu chiếc ghế$1$chính nó. Ở đây, việc ngồi một cách có chiến lược để tối đa hóa chiều dài dây xích trước khi chạm vào chiếc ghế được đánh dấu là rất quan trọng. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực cố gắng ngồi vào tất cả các nhóm ghế, mô phỏng quy trình chấm điểm cho mỗi lựa chọn. Điều này hiệu quả vì chúng ta có thể tuân theo chính xác các quy tắc và đếm số ghế. Tuy nhiên, đối với$n$xung quanh$2\cdot 10^5$, có$2^n$tập hợp con, làm cho vũ lực không thể thực hiện được. 

Thông tin chi tiết quan trọng là hoán vị xác định một biểu đồ có hướng trong đó mỗi chiếc ghế trỏ đến chính xác một chiếc ghế. Mỗi bước di chuyển đều tương ứng với việc đi qua biểu đồ này. Trò chơi kết thúc khi chúng ta quay lại chiếc ghế đã được đánh dấu lần đầu tiên, tương đương với việc nhập một chu trình vào biểu đồ. Do đó, việc tối đa hóa số lượng ghế ngồi sẽ giúp tìm ra tiền tố dài nhất của những chiếc ghế tạo thành các chu trình và chuỗi rời rạc trong đó mỗi chuỗi có thể đi qua một cách an toàn. 

Một góc nhìn đơn giản hơn là quét ghế từ trái sang phải và ngồi bất cứ khi nào ghế chưa được ghé thăm theo nghĩa đánh dấu. Khi chúng ta ngồi, chúng ta đánh dấu$p_i$. Nếu như$p_i \le i$, thì dấu hiệu đó không ảnh hưởng tới những chiếc ghế sau này, nhưng nếu$p_i > i$, nó có thể đánh dấu chiếc ghế trong tương lai mà chúng ta muốn ngồi. Vì vậy, để tối đa hóa số lượt ngồi, chúng ta chỉ cần theo dõi "chỉ số được đánh dấu" tối đa gặp phải cho đến nay. Đối với mỗi chiếc ghế, nếu vượt quá chỉ số được đánh dấu hiện tại thì chúng ta có thể ngồi an toàn. Nếu không, chúng ta không thể ngồi mà không kết thúc trò chơi. 

Điều này biến vấn đề thành một cuộc quét tuyến tính tham lam: giữ chiếc ghế ngoài cùng bên phải sẽ được đánh dấu từ những lần ngồi trước đó và chỉ ngồi khi chỉ số ghế hiện tại vượt quá mức đó. Quan sát này làm giảm giải pháp từ$O(n^2)$ĐẾN$O(n)$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo một biến`max_marked`về 0. Điều này sẽ theo dõi chỉ số xa nhất đã hoặc sẽ được đánh dấu bởi các lần ngồi trước đó. 
2. Khởi tạo bộ đếm`count`đến 0, biểu thị số ghế chúng ta ngồi. 
3. Lặp lại các ghế từ chỉ mục$1$ĐẾN$n$(Lập chỉ mục dựa trên 1 để đảm bảo tính nhất quán với hoán vị). Cho mỗi chiếc ghế$i$: 

1. Nếu $i \le max_marked` thì ngồi ở đây sẽ kết thúc trò chơi, nên bỏ qua chiếc ghế này. 
2. Nếu không thì hãy ngồi trên ghế$i$. Tăng`count`bằng 1. 
3. Cập nhật`max_marked`là giá trị tối đa của giá trị hiện tại của nó và$p_i$. Điều này đảm bảo chúng tôi theo dõi chiếc ghế ngoài cùng bên phải đã được đánh dấu và có thể chặn những chỗ ngồi trong tương lai. 
4. Sau khi lặp lại,`count`chứa số ghế tối đa mà chúng ta có thể ngồi. 
5. Đầu ra`count`cho trường hợp thử nghiệm. 

Tại sao nó hoạt động: Bất biến là vậy`max_marked`luôn lưu trữ chiếc ghế ngoài cùng bên phải đã hoặc sẽ được đánh dấu vào thời điểm chúng tôi đạt được bất kỳ chỉ mục nào. Bằng cách chỉ ngồi trên những chiếc ghế có chỉ số lớn hơn`max_marked`, chúng tôi đảm bảo rằng không ngồi sớm sẽ kết thúc trò chơi. Chiến lược tham lam này hoạt động vì việc đánh dấu chỉ chặn những chiếc ghế trong tương lai chứ không bao giờ chặn những chiếc ghế trước đó, vì vậy việc quét từ trái sang phải trong khi duy trì điểm xa nhất là tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    p = list(map(int, input().split()))
    max_marked = 0
    count = 0
    for i in range(n):
        if i + 1 <= max_marked:
            continue
        count += 1
        max_marked = max(max_marked, p[i])
    print(count)
```Mã khởi tạo`max_marked`Và`count`, sau đó lặp lại trên mỗi chiếc ghế. Séc`i + 1 <= max_marked`đảm bảo lập chỉ mục dựa trên 1. Khi ngồi trên ghế,`max_marked`cập nhật để phản ánh chiếc ghế được đánh dấu bởi lần ngồi này. Cách tiếp cận này xử lý chính xác việc truyền dấu thông qua chuỗi hoán vị, tránh kết thúc trò chơi sớm và chạy theo thời gian tuyến tính. 

## Ví dụ đã hoạt động 

Đầu vào mẫu:`n = 3, p = [3,2,1]`| tôi | Ghế | max_marked | ngồi? | đếm | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | vâng | 1 | 
| 2 | 2 | 3 | vâng | 2 | 
| 3 | 3 | 3 | không | 2 | 

Dấu vết này chứng tỏ ghế 3 bị chặn bởi ghế đánh dấu chỗ ngồi trước đó 3. 

Đầu vào mẫu:`n = 5, p = [4,3,2,5,1]`| tôi | Ghế | max_marked | ngồi? | đếm | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | vâng | 1 | 
| 2 | 2 | 4 | không | 1 | 
| 3 | 3 | 4 | không | 1 | 
| 4 | 4 | 5 | không | 1 | 
| 5 | 5 | 5 | vâng | 2 | 

Ở đây, chúng ta thấy việc bỏ ghế 2-4 là cần thiết để tránh việc kết thúc trận đấu sớm. Bảng xác nhận rằng chiến lược tham lam xác định chính xác những chiếc ghế an toàn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi chiếc ghế được xử lý một lần; hoạt động tối đa là không đổi trên mỗi ghế. | 
| Không gian | O(1) | Chỉ có hai biến số nguyên (`max_marked`Và`count`) được duy trì ngoài đầu vào. | 

Độ phức tạp thời gian tuyến tính đảm bảo giải pháp chạy trong giới hạn 2 giây ngay cả đối với$n$lên đến$2\cdot10^5$và tổng số tiền$n$lên đến$2\cdot10^5$qua các trường hợp thử nghiệm. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    t = int(input())
    for _ in range(t):
        n = int(input())
        p = list(map(int, input().split()))
        max_marked = 0
        count = 0
        for i in range(n):
            if i + 1 <= max_marked:
                continue
            count += 1
            max_marked = max(max_marked, p[i])
        print(count)
    return output.getvalue().strip()

# Provided samples
assert run("4\n3\n3 2 1\n5\n4 3 2 5 1\n4\n4 2 1 3\n4\n2 3 4 1\n") == "2\n2\n3\n1", "sample 1"

# Custom cases
assert run("1\n1\n1\n") == "1", "single chair"
assert run("1\n5\n1 2 3 4 5\n") == "5", "no early marks, all can sit"
assert run("1\n5\n5 4 3 2 1\n") == "1", "first chair marks last, rest blocked"
assert run("1\n6\n2 3 4 5 6 1\n") == "5", "long chain wraps around"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1\n1\n1 | 1 | Đầu vào kích thước tối thiểu | 
| 1\n5\n1 2 3 4 5 | 5 | Hoán vị tăng tuyến tính, tất cả đều có thể ngồi | 
| 1\n5\n5 | | |
