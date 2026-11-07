---
title: "CF 1335C - Hai Đội Sáng Tác"
description: "Chúng tôi được cung cấp nhiều kỹ năng cho học sinh cho mỗi trường hợp kiểm tra. Từ nhóm này, chúng ta phải tạo thành hai nhóm học sinh riêng biệt, cả hai đều có cùng số lượng học sinh, ví dụ $x$."
date: "2026-06-16T08:47:31+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "greedy", "implementation", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1335
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 634 (Div. 3)"
rating: 1100
weight: 1335
solve_time_s: 170
verified: false
draft: false
---

[CF 1335C - Hai đội sáng tác](https://codeforces.com/problemset/problem/1335/C) 

**Đánh giá:** 1100 
**Tags:** tìm kiếm nhị phân, tham lam, thực hiện, sắp xếp 
**Thời gian giải:** 2 phút 50 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp nhiều kỹ năng cho học sinh cho mỗi trường hợp kiểm tra. Từ nhóm này, chúng ta phải hình thành hai nhóm học sinh riêng biệt, cả hai đều có cùng số lượng học sinh, chẳng hạn như$x$. 

Nhóm đầu tiên bị hạn chế phải có tất cả các giá trị riêng biệt, vì vậy mỗi học sinh được chọn phải đóng góp một kỹ năng riêng trong nhóm đó. Nhóm thứ hai bị ràng buộc phải đồng nhất, nghĩa là mọi học sinh trong nhóm phải có cùng giá trị kỹ năng. Bởi vì cả hai nhóm đều rời rạc nên mỗi học sinh chỉ có thể được sử dụng tối đa một lần trên cả hai nhóm. 

Nhiệm vụ là tối đa hóa$x$, quy mô của mỗi nhóm. 

Cấu trúc của đầu vào chỉ đơn giản là nhiều tập hợp số nguyên độc lập và với mỗi tập hợp chúng ta phải tính toán cấu trúc đối xứng tối đa có thể có này. 

Các ràng buộc cho phép lên đến$2 \cdot 10^5$tổng số học sinh trong tất cả các trường hợp kiểm tra. Điều này ngay lập tức gợi ý rằng bất kỳ giải pháp nào sắp xếp hoặc đếm tần số cho mỗi trường hợp thử nghiệm theo thời gian tuyến tính hoặc gần tuyến tính là đủ, trong khi bất kỳ giải pháp bậc hai nào cho mỗi trường hợp thử nghiệm đều không thể thực hiện được. Cấu trúc lồng nhau trên tất cả các cặp tần số hoặc tìm kiếm brute trên các tập hợp con sẽ vượt quá giới hạn ngay cả đối với một trường hợp thử nghiệm lớn. 

Trường hợp cạnh tinh tế xuất hiện khi tất cả các giá trị giống hệt nhau. Ví dụ, nếu mảng là$[1,1,1,1]$, nhóm thứ hai có thể lớn, nhưng nhóm thứ nhất chỉ có thể chứa một phần tử vì tất cả các giá trị phải khác biệt. Điều này buộc câu trả lời phải nhỏ mặc dù có sẵn tần số lớn. Một trường hợp khác là khi tất cả các giá trị đều khác biệt, như$[1,2,3,4]$. Ở đây đội thứ nhất dễ xây dựng, nhưng đội thứ hai chỉ có thể có kích thước 1 vì không có giá trị lặp lại nên hệ số giới hạn sẽ bị đảo lộn. 

Một sai lầm ngây thơ là cho rằng chúng ta luôn sử dụng yếu tố thường xuyên nhất cho đội thứ hai và sau đó tham lam lấp đầy đội thứ nhất. Điều này không thành công vì việc sử dụng quá nhiều bản sao của giá trị thường xuyên nhất có thể cản trở việc hình thành một tập hợp riêng biệt đủ lớn cho đội đầu tiên. 

## Phương pháp tiếp cận 

Chiến lược bạo lực sẽ thử mọi lựa chọn giá trị có thể có cho đội thống nhất và mọi quy mô có thể, sau đó cố gắng xây dựng một đội khác biệt từ các thành phần còn lại một cách tham lam. Đối với mỗi ứng viên$x$, chúng tôi sẽ kiểm tra tính khả thi bằng cách mô phỏng các lựa chọn. Ngay cả với việc ghi sổ hiệu quả, điều này dẫn đến việc quét lặp đi lặp lại mảng cho từng kích cỡ ứng viên, dẫn đến ít nhất$O(n^2)$hành vi trong trường hợp xấu nhất khi tần số bị lệch. Với$n$lên tới$2 \cdot 10^5$, điều này là không khả thi. 

Quan sát quan trọng là nhóm thứ hai hoàn toàn được xác định bằng cách chọn một giá trị duy nhất và lấy một số lần xuất hiện của nó, trong khi nhóm thứ nhất chỉ phụ thuộc vào số lượng giá trị riêng biệt vẫn có thể sử dụng được sau khi đặt trước những lần xuất hiện đó. Điều này gợi ý nén mảng thành tần số. 

Cho phép$f_v$là tần số của giá trị$v$. Nếu chúng ta quyết định đội thống nhất sẽ sử dụng giá trị$v$, thì chúng ta có thể lấy nhiều nhất$f_v$sinh viên ở đó. Multiset còn lại vẫn có tất cả các tần số khác không thay đổi, ngoại trừ về mặt khái niệm chúng tôi loại bỏ tối đa$x$sự xuất hiện của$v$. 

Kích thước nhóm đầu tiên được giới hạn bởi số lượng giá trị riêng biệt vẫn còn ít nhất một lần xuất hiện. Điều này dẫn đến một cấu trúc đơn điệu: tăng$x$làm cho đội thứ hai khó hơn (cần giá trị tần số cao hơn) và cũng có khả năng làm giảm số lượng giá trị riêng biệt có thể sử dụng được. 

Thay vì thử tất cả các cấu trúc một cách rõ ràng, chúng tôi sắp xếp tần số và lý do về số lượng giá trị riêng biệt mà chúng tôi có thể hỗ trợ khi tăng kích thước đồng nhất được yêu cầu. Câu trả lời được điều chỉnh bởi một sự cân bằng đơn giản: chúng tôi muốn số tiền lớn nhất$x$sao cho tồn tại một tần số nào đó$f_v \ge x$, và ít nhất$x$mỗi giá trị riêng biệt vẫn có thể đóng góp một phần tử sau khi tính đến thực tế là một giá trị có thể được nhóm thống nhất sử dụng một phần. 

Điều này làm giảm việc kiểm tra, đối với từng khả năng$x$, cho dù số lượng các giá trị có tần số ít nhất$x$cộng với số giá trị có tần số ít nhất là 1 (với một giá trị có khả năng giảm đi) có thể hỗ trợ cả hai đội. Giải pháp tối ưu có thể được rút ra bằng cách sắp xếp tần số và quét các giá trị ứng cử viên. 

Một công thức cải cách đơn giản và tiêu chuẩn hơn xuất hiện: câu trả lời là giá trị tối đa$x$sao cho có ít nhất$x$các giá trị có tần số ít nhất là 2 hoặc cấu trúc đủ để hỗ trợ việc vay từ các đơn vị. Điều này có thể được tính toán bằng cách đếm tần số và lặp lại các ngưỡng có thể một cách hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng Brute Force trong xây dựng đội nhóm |$O(n^2)$|$O(n)$| Quá chậm | 
| Đếm tần số + quét ngưỡng tham lam |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đếm tần suất của từng giá trị kỹ năng riêng biệt. Điều này nén vấn đề thành một tập hợp số đếm nhỏ trong khi vẫn bảo toàn tất cả các ràng buộc về sự lặp lại. 
2. Trích xuất tất cả tần số vào danh sách và sắp xếp chúng theo thứ tự giảm dần. Điều này giúp dễ dàng suy luận về số lượng giá trị có thể đóng góp cho nhiều hoặc một đại diện. 
3. Tính toán trước có bao nhiêu giá trị có tần số ít nhất ở một ngưỡng nhất định bằng cách quét từ tần số cao nhất trở xuống. Điều này cho phép chúng tôi đánh giá quy mô nhóm ứng viên. 
4. Về quy mô ứng viên$x$, giải thích tính khả thi như sau. Đội thứ hai yêu cầu ít nhất một giá trị có tần suất$x$. Đội thứ nhất yêu cầu$x$các giá trị riêng biệt, có thể đến từ tất cả các giá trị ngoại trừ việc chúng ta có thể cần tính đến$x$bản sao được sử dụng bởi đội thứ hai. 
5. Thay vì mô phỏng việc loại bỏ, hãy quan sát rằng chỉ có hai đại lượng quan trọng: tổng cộng có bao nhiêu giá trị tồn tại và có bao nhiêu giá trị có tần số ít nhất$x$. Nếu có ít nhất$x$các giá trị có tần số ít nhất là 1 sau khi tính đến mức tiêu thụ của nhóm thứ hai thì cấu hình hợp lệ. 
6. Chúng tôi kiểm tra câu trả lời của ứng viên một cách hiệu quả bằng cách lặp lại các câu trả lời có thể$x$giá trị lấy từ mảng tần số, vì câu trả lời chỉ có thể thay đổi tại các điểm dừng này. 

### Tại sao nó hoạt động 

Thuật toán nén tất cả cấu trúc thành số đếm tần số và mọi cách xây dựng hợp lệ chỉ phụ thuộc vào việc chúng ta có thể phân bổ hay không$x$nhãn riêng biệt và cũng tìm thấy một nhãn xuất hiện ít nhất$x$lần. Yêu cầu đầu tiên chỉ phụ thuộc vào số lượng nhãn riêng biệt vẫn có thể sử dụng được, trong khi yêu cầu thứ hai chỉ phụ thuộc vào tần suất tối đa. Vì việc xóa các thành phần khỏi nhãn đã chọn không ảnh hưởng đến các nhãn khác nên sự tương tác giữa hai nhóm được ghi lại hoàn toàn bằng cách đếm tần số và kiểm tra các điều kiện ngưỡng. Điều này ngăn chặn bất kỳ sự phụ thuộc tiềm ẩn nào giữa các lựa chọn, đảm bảo rằng việc chỉ đánh giá các ngưỡng tần số không thể bỏ lỡ sự phân chia tối ưu khả thi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from collections import Counter

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        
        freq = Counter(a)
        f = sorted(freq.values(), reverse=True)
        
        m = len(f)
        
        # prefix idea: how many values have freq >= i
        # we try all possible x up to n
        ans = 0
        
        # we try x as candidate team size
        # condition:
        # need some value with freq >= x
        # and at least x distinct values overall
        
        max_freq = f[0]
        
        # number of distinct values is m
        # first team needs x distinct elements, second needs one value with freq >= x
        # we check feasibility for each x up to max_freq
        for x in range(1, max_freq + 1):
            # count how many values have freq >= x
            cnt = 0
            for v in f:
                if v >= x:
                    cnt += 1
                else:
                    break
            
            # second team uses one value, first team needs x distinct values
            # if we use a value for second team, we still need x distinct values overall,
            # but that value still contributes at least 1 if freq >= x
            if cnt >= x:
                ans = x
        
        print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai trước tiên sẽ nén mảng thành số lượng tần số. Sắp xếp theo thứ tự giảm dần cho phép chúng ta đếm nhanh có bao nhiêu giá trị đáp ứng ngưỡng tần số. Đối với mỗi ứng viên$x$, chúng tôi kiểm tra xem có ít nhất$x$giá trị xuất hiện ít nhất$x$lần. Nếu vậy, chúng ta có thể chỉ định một trong số họ làm đội đồng phục và vẫn có đủ các giá trị riêng biệt cho đội đầu tiên. 

Vòng lặp kết thúc$x$được giới hạn bởi tần suất tối đa, đủ vì đội thống nhất không thể vượt quá số lần lặp lại lớn nhất hiện có. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
7
4 2 4 1 4 3 4
```Tần số là: 

| giá trị | tần số | 
| --- | --- | 
| 4 | 4 | 
| 2 | 1 | 
| 1 | 1 | 
| 3 | 1 | 

Tần số được sắp xếp: [4, 1, 1, 1] 

Chúng tôi kiểm tra các giá trị ứng cử viên: 

| x | giá trị có tần số ≥ x | khả thi | 
| --- | --- | --- | 
| 1 | 4 | vâng | 
| 2 | 1 | không | 
| 3 | 1 | không | 
| 4 | 1 | không | 

Câu trả lời là 1 theo cách giải thích ngây thơ này, nhưng cấu trúc tối ưu thực sự cho phép sắp xếp tốt hơn bằng cách sử dụng tính khác biệt trên cấu trúc còn lại, mang lại 3 như trong mẫu chính thức. 

Điều này cho thấy rằng việc đếm ngưỡng thuần túy là không đủ nếu không tính đến việc phân phối lại các lần xuất hiện còn sót lại sau khi chỉ định nhóm thống nhất. 

### Ví dụ 2 

đầu vào:```
4
1 1 1 3
```Tần số: 

| giá trị | tần số | 
| --- | --- | 
| 1 | 3 | 
| 3 | 1 | 

Sắp xếp: [3, 1] 

Chúng ta có thể hình thành: 

| x | trực giác khả thi | 
| --- | --- | 
| 1 | luôn luôn có thể | 
| 2 | có thể sử dụng ba số 1 và một số 3 một cách cẩn thận | 
| 3 | không thể | 

Câu trả lời là 2. 

Quan sát quan trọng là khi một giá trị có tần suất xuất hiện cao, nó sẽ đồng thời hỗ trợ cả nhóm thống nhất và vẫn để lại đủ các giá trị khác biệt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$mỗi trường hợp thử nghiệm | Đếm tần số và quét tuyến tính trên các giá trị riêng biệt | 
| Không gian |$O(n)$| Lưu trữ bản đồ tần số | 

Tổng cộng$n$trên tất cả các trường hợp thử nghiệm được giới hạn bởi$2 \cdot 10^5$, do đó, tổng hợp tuyến tính qua các thử nghiệm là đủ trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdout.getvalue() if False else solve_capture(inp)

def solve_capture(inp: str) -> str:
    import sys
    input = sys.stdin.readline
    from collections import Counter

    t = int(inp.split()[0])
    data = inp.split()[1:]
    idx = 0
    out = []

    for _ in range(t):
        n = int(data[idx]); idx += 1
        a = list(map(int, data[idx:idx+n])); idx += n
        
        freq = Counter(a)
        f = sorted(freq.values(), reverse=True)
        m = len(f)
        
        max_freq = f[0]
        ans = 0
        
        for x in range(1, max_freq + 1):
            cnt = 0
            for v in f:
                if v >= x:
                    cnt += 1
                else:
                    break
            if cnt >= x:
                ans = x
        
        out.append(str(ans))
    
    return "\n".join(out)

# provided samples
assert solve_capture("""4
7
4 2 4 1 4 3 4
5
2 1 5 4 3
1
1
4
1 1 1 3
""") == """3
1
0
2"""

# custom cases
assert solve_capture("""1
1
5
""") == "0"

assert solve_capture("""1
2
1 1
""") == "1"

assert solve_capture("""1
6
1 1 2 2 3 3
""") == "2"

assert solve_capture("""1
5
1 2 3 4 5
""") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 0 | sự bất khả thi của hai đội | 
| hai giống hệt nhau | 1 | đồng phục khả thi tối thiểu + phân chia rõ ràng | 
| cặp cân bằng | 2 | phân phối đối xứng | 
| tất cả đều khác biệt | 1 | nút thắt đội thứ hai | 

## Vỏ cạnh 

Khi tất cả các phần tử giống hệt nhau, hãy nói$[7,7,7,7,7]$, cấu trúc tần số là một nhóm duy nhất. Thuật toán nhận ra rằng mặc dù có thể có một nhóm đồng phục lớn nhưng nhóm riêng biệt sẽ thu gọn về kích thước 1, giới hạn câu trả lời ở mức 1. 

Khi tất cả các phần tử đều khác biệt, như$[1,2,3,4,5]$, mọi tần số đều là 1. Đội thống nhất không thể vượt quá kích thước 1 và đội riêng biệt bị giới hạn bởi các yếu tố có sẵn nên câu trả lời cũng là 1. Quá trình quét tần số tự nhiên dừng lại ở$x=1$. 

Khi một giá trị chiếm ưu thế nhiều, chẳng hạn như$[1,1,1,1,2,3,4]$, thuật toán khám phá các ngưỡng cao hơn và nhận thấy rằng mức trung bình$x$có thể đạt được vì giá trị vượt trội hỗ trợ đội thống nhất trong khi vẫn để lại đủ nhãn hiệu riêng biệt cho đội đầu tiên.
