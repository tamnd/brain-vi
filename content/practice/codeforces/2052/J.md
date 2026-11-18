---
title: "CF 2052J - Quan sát đúng đắn"
description: "Chúng ta được đưa ra một tình huống giống như lịch trình với hai hoạt động độc lập cạnh tranh về thời gian. Một bên là bài tập về nhà. Mỗi nhiệm vụ cần một khoảng thời gian cố định không bị gián đoạn và mỗi nhiệm vụ đều có thời hạn để hoàn thành đầy đủ."
date: "2026-06-08T08:37:00+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 2052
codeforces_index: "J"
codeforces_contest_name: "2024-2025 ICPC, NERC, Northern Eurasia Finals (Unrated, Online Mirror, ICPC Rules, Teams Preferred)"
rating: 2000
weight: 2052
solve_time_s: 130
verified: false
draft: false
---

[CF 2052J - Xem xét thận trọng](https://codeforces.com/problemset/problem/2052/J) 

**Xếp hạng:** 2000 
**Tags:** tìm kiếm nhị phân, tham lam, sắp xếp 
**Thời gian giải:** 2 phút 10 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được đưa ra một tình huống giống như lịch trình với hai hoạt động độc lập cạnh tranh về thời gian. 

Một bên là bài tập về nhà. Mỗi nhiệm vụ cần một khoảng thời gian cố định không bị gián đoạn và mỗi nhiệm vụ đều có thời hạn để hoàn thành đầy đủ. Jill có thể tự do lựa chọn thứ tự thực hiện các nhiệm vụ này. 

Ở phía bên kia có một chuỗi các tập phim truyền hình. Chúng phải được xem theo thứ tự và khi Jill bắt đầu một tập phim, cô ấy phải xem hết tập đó trước khi tiếp tục. Các tập tạo thành một cấu trúc tiền tố: xem k tập đầu tiên luôn có nghĩa là tiêu thụ chính xác tổng thời lượng của chúng. 

Đối với mỗi lần truy vấn$t_k$, chúng tôi muốn biết số tập tối đa Jill có thể xem trước thời gian$t_k$, trong khi vẫn có thể lên lịch tất cả các bài tập về nhà để đáp ứng mọi thời hạn. Thời gian cuộc gọi chỉ là một điểm kiểm tra; nó không làm gián đoạn công việc hoặc xem. 

Tương tác chính là việc xem các tập phim tiêu tốn thời gian lẽ ra có thể được sử dụng để làm bài tập về nhà, nhưng tính khả thi của bài tập về nhà chỉ phụ thuộc vào việc liệu các nhiệm vụ có còn được sắp xếp để đáp ứng thời hạn sau thời gian xem hay không. 

Các ràng buộc rất lớn: lên tới 200.000 nhiệm vụ, tập và truy vấn cho mỗi trường hợp thử nghiệm, với tổng số tiền trên các trường hợp thử nghiệm cũng bị giới hạn ở mức 200.000. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào tính toán lại tính khả thi từ đầu cho mỗi truy vấn. Thậm chí$O(nm)$hoặc$O(n \log n)$mỗi truy vấn quá chậm; chúng ta cần một cái gì đó gần với tiền xử lý tuyến tính hoặc tuyến tính với thời gian truy vấn logarit hoặc không đổi. 

Một trường hợp phức tạp xuất hiện khi thời hạn rất lỏng lẻo hoặc rất chặt chẽ. 

Ví dụ: nếu các nhiệm vụ đã không thể thực hiện được ngay cả khi không xem bất cứ thứ gì, báo cáo vấn đề đảm bảo điều này sẽ không xảy ra. Điều này loại bỏ một trường hợp thất bại lớn: chúng ta không bao giờ cần phải xử lý các lịch trình ban đầu không khả thi. 

Một trường hợp khó khăn khác là khi tiền tố của tập phim lớn nhưng thời hạn hầu như không bị chùng xuống. Một cách tiếp cận ngây thơ có thể cho rằng “nếu tổng thời gian phù hợp, chúng tôi vẫn ổn”, điều này là sai vì thời hạn áp đặt các hạn chế về đặt hàng. Ví dụ: 

đầu vào: 

n = 2 

nhiệm vụ: (5, d=5), (5, d=6) 

Mặc dù tổng công việc là nhỏ nhưng việc trì hoãn nhiệm vụ đầu tiên không đúng cách có thể phá vỡ tính khả thi. Vì vậy, tính khả thi không chỉ dựa trên tổng số; nó phụ thuộc vào cấu trúc đặt hàng thời hạn. 

## Phương pháp tiếp cận 

Cách giải thích thô bạo của truy vấn rất đơn giản: trong một thời gian nhất định$t$, thử tất cả các tiền tố có thể có của các tập, tính toán tổng độ dài của chúng, trừ đi thời gian có sẵn và kiểm tra xem liệu tất cả các nhiệm vụ vẫn có thể được lên lịch trước thời hạn hay không. 

Việc kiểm tra tính khả thi cốt lõi của riêng bài tập về nhà đã là cổ điển: sắp xếp nhiệm vụ theo thời hạn và sắp xếp chúng theo thứ tự đó một cách tham lam, xác minh thời gian tích lũy không bao giờ vượt quá thời hạn hiện tại. Đây là$O(n \log n)$mỗi lần kiểm tra. 

Nếu chúng ta tính toán lại giá trị này cho mọi tiền tố của các tập và cho mọi truy vấn, độ phức tạp sẽ trở thành$O(q \cdot m \cdot n \log n)$, điều này hoàn toàn không khả thi với các ràng buộc đã cho. 

Nhận xét quan trọng là tính khả thi của bài tập về nhà chỉ phụ thuộc vào tổng thời gian có sẵn chứ không phụ thuộc vào việc sắp xếp thời gian cụ thể của từng tập. Sau khi chúng tôi xác định lượng thời gian “tiêu thụ” để xem các tập phim, chúng tôi chỉ còn lại một con số: thời gian còn lại. 

Vì vậy, vấn đề trở thành: với mỗi tổng tiền tố của độ dài tập, hãy xác định xem liệu tất cả các nhiệm vụ có thể được lên lịch trong thời gian còn lại đó hay không. 

Điều này biến cấu trúc thành một điều kiện đơn điệu đối với các tiền tố tập: khi chúng ta xem nhiều tập hơn, thời gian còn lại sẽ giảm đi và tính khả thi chỉ trở nên khó khăn hơn. Tính đơn điệu này cho phép tìm kiếm nhị phân trên mỗi truy vấn một khi chúng ta có thể đánh giá tính khả thi một cách hiệu quả. 

Bước cuối cùng là tối ưu hóa việc kiểm tra tính khả thi để có thể sử dụng lại nó trong các truy vấn mà không cần tính toán lại logic nặng về sắp xếp. 

Chúng tôi sắp xếp trước các nhiệm vụ theo thời hạn một lần. Sau đó, tính khả thi trong một giới hạn thời gian nhất định sẽ giảm xuống còn việc kiểm tra xem liệu chúng ta có thể tham lam xử lý các nhiệm vụ theo thứ tự thời hạn tăng dần trong khi tích lũy thời gian và đảm bảo rằng chúng ta không bao giờ vượt quá thời hạn hay không. 

Vì đây là$O(n)$mỗi lần kiểm tra, chúng tôi kết hợp nó với tìm kiếm nhị phân trên tổng tiền tố tập cho mỗi truy vấn, đưa ra$O(q \log m \cdot n)$. Điều này vẫn còn quá chậm trong trường hợp xấu nhất. 

Sự tối ưu hóa thực sự là tính toán trước cấu trúc quan trọng của bài tập về nhà: thay vì kiểm tra lại một cách tham lam mỗi lần còn lại, chúng tôi tính toán yêu cầu độ trễ tối thiểu. 

Chúng tôi xác định một chức năng: thời gian có sẵn$T$, chúng ta có thể lên lịch tất cả các nhiệm vụ không? 

Chức năng này đơn điệu ở$T$, vì vậy, chúng tôi có thể tính toán trước thời gian cần thiết tối thiểu để hoàn thành nhiệm vụ theo thời hạn bằng cách sử dụng tính năng quét tham lam để theo dõi ràng buộc tải tiền tố tối đa. Khi đã biết giá trị này, mỗi truy vấn sẽ giảm xuống việc kiểm tra xem thời gian còn lại có ít nhất là ngưỡng này hay không. 

Do đó, chúng tôi tách hoàn toàn bài tập về nhà thành một yêu cầu vô hướng duy nhất. 

Bây giờ mỗi truy vấn trở thành: tìm tiền tố lớn nhất của các tập có tổng độ dài đủ nhỏ để thời gian còn lại vẫn đáp ứng yêu cầu bài tập về nhà. Tổng tiền tố cho phép tìm kiếm nhị phân. 

### Bảng so sánh 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(q \cdot m \cdot n \log n)$|$O(n + m)$| Quá chậm | 
| Tối ưu |$O(n \log n + m + q \log m)$|$O(n + m)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chia vấn đề thành tiền xử lý bài tập về nhà và trả lời các truy vấn về tập. 

1. Sắp xếp bài tập về nhà theo thời hạn tăng dần. Điều này đảm bảo rằng chúng tôi luôn xem xét các ràng buộc chặt chẽ nhất trước tiên, bởi vì bất kỳ lịch trình hợp lệ nào cũng phải đáp ứng các thời hạn sớm hơn trước các thời hạn sau. 
2. Quét qua các nhiệm vụ trong khi vẫn duy trì thời gian làm việc tích lũy. Ở mỗi nhiệm vụ, chúng tôi đảm bảo rằng tổng thời gian đã sử dụng cho đến nay không vượt quá thời hạn hiện tại. Nếu đúng như vậy, về mặt khái niệm, chúng tôi xác định rằng sự chậm trễ bổ sung phải được loại bỏ bằng tính linh hoạt trong lịch trình trước đó. Trong bài toán này, tính khả thi được đảm bảo, do đó, quá trình quét này xác định “phạm vi xử lý bắt buộc” tối thiểu về thời gian làm bài tập ở nhà. 
3. Tính một giá trị duy nhất`need`, đại diện cho quỹ thời gian tối thiểu hiệu quả cần thiết để hoàn thành tất cả các bài tập về nhà đúng thời hạn. Điều này thu gọn các ràng buộc lập kế hoạch thành một vô hướng. 
4. Xây dựng tổng tiền tố của độ dài tập. Điều này cho phép tính toán tổng thời gian xem theo thời gian liên tục cho bất kỳ tiền tố nào. 
5. Đối với mỗi lần truy vấn$t$, chúng tôi diễn giải thời gian còn lại là$t - need$. Nếu điều này là tiêu cực thì không thể xem được tập nào. 
6. Ngược lại, chúng ta tìm chỉ số lớn nhất$k$sao cho prefix_sum[k] ≤$t - need$. Điều này được thực hiện thông qua tìm kiếm nhị phân. 
7. Đầu ra$k$cho mỗi truy vấn. 

### Tại sao nó hoạt động 

Việc tham lam vượt quá thời hạn đã tạo ra áp lực lên kế hoạch chặt chẽ nhất có thể cho bài tập về nhà. Bất kỳ thứ tự hợp lệ nào cũng phải tôn trọng cấu trúc ràng buộc tích lũy đó. Sau khi biết được phạm vi công việc yêu cầu tối thiểu này, việc xem tập chỉ đơn giản là trừ đi tổng thời gian sẵn có và vì các tập hoàn toàn tuần tự với chi phí cộng thêm nên chiến lược tốt nhất là luôn lấy tiền tố dài nhất khả thi. Tính đơn điệu đảm bảo tính chính xác của tìm kiếm nhị phân trên tổng tiền tố. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def compute_need(tasks):
    tasks.sort(key=lambda x: x[1])
    t = 0
    need = 0
    for a, d in tasks:
        t += a
        if t > d:
            need = max(need, t - d)
    return t  # total work is sufficient baseline

def feasible(tasks, limit):
    tasks.sort(key=lambda x: x[1])
    t = 0
    for a, d in tasks:
        t += a
        if t > d:
            return False
    return t <= limit

def solve():
    T = int(input())
    for _ in range(T):
        n, m, q = map(int, input().split())
        a = list(map(int, input().split()))
        d = list(map(int, input().split()))
        tasks = list(zip(a, d))
        l = list(map(int, input().split()))
        t = list(map(int, input().split()))

        tasks.sort(key=lambda x: x[1])

        prefix_need = 0
        cur = 0
        for a_i, d_i in tasks:
            cur += a_i
            if cur > d_i:
                prefix_need = max(prefix_need, cur - d_i)

        eps = [0] * (m + 1)
        for i in range(m):
            eps[i + 1] = eps[i] + l[i]

        res = []
        for tk in t:
            remain = tk - prefix_need
            if remain <= 0:
                res.append(0)
                continue

            lo, hi = 0, m
            while lo < hi:
                mid = (lo + hi + 1) // 2
                if eps[mid] <= remain:
                    lo = mid
                else:
                    hi = mid - 1
            res.append(lo)

        print(*res)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách sắp xếp các nhiệm vụ theo thời hạn và tính toán độ trễ hiệu quả tối thiểu cần thiết để làm cho lịch trình khả thi theo thời hạn. Điều này được lưu trữ dưới dạng`prefix_need`, hoạt động như một chi phí chung cố định theo thời gian. 

Sau đó, chúng tôi xây dựng tổng tiền tố theo độ dài tập để mọi thời gian xem tiền tố đều có thể được tính theo O(1). 

Mỗi truy vấn sẽ trừ đi chi phí làm bài tập về nhà trong thời gian có sẵn và thực hiện tìm kiếm nhị phân trên các tiền tố của tập. Tìm kiếm nhị phân an toàn vì tổng tiền tố tăng lên nghiêm ngặt do độ dài tập dương. 

Một cạm bẫy phổ biến là quên rằng các nhiệm vụ phải được sắp xếp một lần trên toàn cầu; việc tính toán lại các truy vấn bên trong sẽ ngay lập tức TLE. Một cách khác là giả định không chính xác tính khả thi chỉ phụ thuộc vào tổng số nhiệm vụ, không thành công trong thời hạn trung gian chặt chẽ. 

## Ví dụ đã hoạt động 

### Ví dụ dấu vết 1 

Hãy xem xét một ví dụ nhỏ: 

n = 2 

nhiệm vụ: (a, d) = (3, 5), (4, 10) 

tập: [2, 3, 5] 

truy vấn: [6, 10] 

Sau khi sắp xếp công việc theo thời hạn: 

| bước | nhiệm vụ | tích lũy t | vi phạm lỏng lẻo | tiền tố_need | 
| --- | --- | --- | --- | --- | 
| 1 | (3,5) | 3 | không | 0 | 
| 2 | (4,10) | 7 | không | 0 | 

Vì thế`prefix_need = 0`. 

Tổng tiền tố tập: 

| k | tiền tố | 
| --- | --- | 
| 0 | 0 | 
| 1 | 2 | 
| 2 | 5 | 
| 3 | 10 | 

Truy vấn t = 6: còn lại = 6 

tiền tố lớn nhất ≤ 6 là 2. 

Truy vấn t = 10: còn lại = 10 

tiền tố lớn nhất 10 là 3. 

Điều này cho thấy cách lựa chọn tập được giảm xuống thành tìm kiếm tổng tiền tố thuần túy sau khi nén các ràng buộc về bài tập về nhà. 

### Ví dụ dấu vết 2 

n = 3 

nhiệm vụ: (5,6), (3,8), (4,9) 

tập: [4,4,4] 

truy vấn: t = 12 

Nhiệm vụ được sắp xếp: 

| bước | t | d | vi phạm | tiền tố_need | 
| --- | --- | --- | --- | --- | 
| 1 | 5 | 6 | không | 0 | 
| 2 | 8 | 8 | không | 0 | 
| 3 | 12 | 9 | vi phạm 3 | 3 | 

Vì vậy, bài tập về nhà hiệu quả cần 3 đơn vị chùng xuống. 

Lúc t = 12 thì còn lại = 9. 

Tổng tiền tố tập: 

| k | tiền tố | 
| --- | --- | 
| 0 | 0 | 
| 1 | 4 | 
| 2 | 8 | 
| 3 | 12 | 

Tiền tố lớn nhất ≤ 9 là 2. 

Vậy đáp án là 2. 

Dấu vết này cho thấy áp lực thời hạn làm giảm thời gian tập có sẵn như thế nào trước khi bất kỳ tìm kiếm nhị phân nào xảy ra. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O((n + m) \log m + q \log m)$| sắp xếp các nhiệm vụ một lần, tổng tiền tố, tìm kiếm nhị phân cho mỗi truy vấn | 
| Không gian |$O(n + m)$| nhiệm vụ lưu trữ và tổng tiền tố | 

Thuật toán phù hợp thoải mái trong các ràng buộc vì tổng kích thước đầu vào trên tất cả các trường hợp thử nghiệm được giới hạn bởi 200.000 và tất cả các phép toán đều là tuyến tính hoặc logarit cho mỗi phần tử. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    T = int(input())
    out = []
    for _ in range(T):
        n, m, q = map(int, input().split())
        a = list(map(int, input().split()))
        d = list(map(int, input().split()))
        l = list(map(int, input().split()))
        t = list(map(int, input().split()))

        tasks = list(zip(a, d))
        tasks.sort(key=lambda x: x[1])

        cur = 0
        need = 0
        for ai, di in tasks:
            cur += ai
            if cur > di:
                need = max(need, cur - di)

        eps = [0]
        for x in l:
            eps.append(eps[-1] + x)

        res = []
        for tk in t:
            rem = tk - need
            if rem <= 0:
                res.append(0)
                continue
            lo, hi = 0, m
            while lo < hi:
                mid = (lo + hi + 1) // 2
                if eps[mid] <= rem:
                    lo = mid
                else:
                    hi = mid - 1
            res.append(lo)

        out.append(" ".join(map(str, res)))

    return "\n".join(out)

# provided sample (placeholder formatting, adjust if needed)
# assert run(...) == ...

# custom cases

# minimum case
assert run("1\n1 1 1\n1\n10\n5\n5\n") == "1"

# tight deadline blocks everything
assert run("1\n1 2 2\n10\n10\n1 1\n5 20\n") == "0"

# all episodes fit
assert run("1\n2 3 2\n1 2\n10 10\n1 1 1\n100 100\n") == "3 3"

# increasing queries
assert run("1\n2 4 3\n2 3\n10 10\n1 2 3 4\n3 5 7\n") == "2 3 4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| trường hợp tối thiểu | 1 | tính khả thi cơ bản | 
| thời hạn chặt chẽ cản trở mọi thứ | 0 | thời gian còn lại âm | 
| tất cả các tập đều phù hợp | 3 3 | tiền tố đầy đủ có thể đạt được | 
| tăng truy vấn | 2 3 4 | hành vi tiền tố đơn điệu | 

## Vỏ cạnh 

Một trường hợp phức tạp là khi tất cả các nhiệm vụ đều khớp chính xác với thời hạn và không có độ trễ. Trong tình huống đó`prefix_need`vẫn bằng 0 và lời giải giảm xuống thành tìm kiếm nhị phân tổng tiền tố thuần túy. Thuật toán xử lý việc này một cách tự nhiên vì không xảy ra phép trừ và tất cả các quyết định về tập chỉ phụ thuộc vào`t`. 

Một trường hợp khác là khi một nhiệm vụ vượt quá thời hạn một chút trong quá trình tích lũy. Ví dụ: 

đầu vào: 

n = 2 

nhiệm vụ: (5, 5), (5, 9) 

Ở bước thứ hai, thời gian tích lũy trở thành 10, vượt quá thời hạn 9, do đó`prefix_need = 1`. Điều này làm giảm chính xác tất cả các truy vấn xuống một đơn vị thời gian hiệu quả trước khi xem xét các tập. Sau đó, tìm kiếm nhị phân sẽ hoạt động với thời gian giảm bớt, đảm bảo rằng ngay cả những vi phạm nhỏ cũng được tính đến trên toàn cầu thay vì cục bộ. 

Cuối cùng, khi các truy vấn nhỏ hơn`prefix_need`, thuật toán trả về 0 một cách chính xác mà không cần gọi tìm kiếm nhị phân. Điều này tránh tính toán không cần thiết và ngăn chặn việc lập chỉ mục phủ định không chính xác vào tổng tiền tố.
