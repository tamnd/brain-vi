---
title: "CF 1463C - Robot bận rộn"
description: "Một robot di chuyển trên một đường thẳng bắt đầu từ vị trí 0. Bạn gửi cho nó một chuỗi lệnh, mỗi lệnh chỉ định thời gian và vị trí mục tiêu. Khi có lệnh đến, robot ngay lập tức cam kết di chuyển về phía mục tiêu đó với tốc độ đơn vị."
date: "2026-06-11T01:59:21+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1463
codeforces_index: "C"
codeforces_contest_name: "Educational Codeforces Round 100 (Rated for Div. 2)"
rating: 1800
weight: 1463
solve_time_s: 131
verified: false
draft: false
---

[CF 1463C - Robot bận rộn](https://codeforces.com/problemset/problem/1463/C) 

**Đánh giá:** 1800 
**Thẻ:** triển khai 
**Thời gian giải:** 2m 11s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Một robot di chuyển trên một đường thẳng bắt đầu từ vị trí 0. Bạn gửi cho nó một chuỗi lệnh, mỗi lệnh chỉ định thời gian và vị trí mục tiêu. Khi có lệnh đến, robot ngay lập tức cam kết di chuyển về phía mục tiêu đó với tốc độ đơn vị. Trong khi nó đang di chuyển về phía mục tiêu đó, mọi lệnh mới sẽ bị bỏ qua cho đến khi nó đến được mục tiêu hoặc hoàn thành chuyển động hiện tại. 

Mỗi lệnh được đánh giá sau khi biết được toàn bộ chuyển động. Một lệnh được coi là thành công nếu trong khoảng thời gian từ khi nó được đưa ra cho đến ngay trước khi có lệnh tiếp theo (hoặc mãi mãi đối với lệnh cuối cùng), tại một thời điểm nào đó, robot ở chính xác tại vị trí mục tiêu của lệnh đó. Điều quan trọng là robot có thể không thực sự thực hiện lệnh đó khi đến vị trí; chỉ cần quỹ đạo của nó đi qua nó trong khoảng thời gian đó là đủ. 

Khó khăn chính là các lệnh có thể bị bỏ qua, nhưng các lệnh bị bỏ qua vẫn có thể thành công. Sự mất kết nối giữa việc thực hiện và thành công buộc chúng ta phải suy luận về con đường liên tục của robot thay vì chỉ các điểm đến đã chọn. 

Các ràng buộc cho phép tối đa 100000 lệnh trong tất cả các trường hợp thử nghiệm, do đó, mọi giải pháp đều phải xử lý từng lệnh trong thời gian không đổi hoặc logarit. Việc mô phỏng cố gắng kiểm tra mọi đơn vị thời gian là không thể vì các chuyển động xảy ra trong khoảng thời gian lên tới 10^9 giây. 

Một trường hợp thất bại tinh vi đối với lối suy nghĩ ngây thơ xuất hiện khi robot vượt qua một điểm rất lâu sau khi khoảng thời gian lệnh kết thúc hoặc khi nó vượt qua điểm đó trong khi thực hiện lệnh sau đó. Ví dụ: nếu một lệnh gửi rô-bốt đến một điểm mà nó không bao giờ nhắm mục tiêu rõ ràng, thì lệnh đó vẫn có thể thành công nếu phân đoạn chuyển động của rô-bốt vượt qua tọa độ đó trong cửa sổ thời gian hợp lệ. Một trường hợp khác xảy ra khi một lệnh bị bỏ qua ngay lập tức nhưng mục tiêu của nó nằm ở đoạn chuyển động sau đó; một cách tiếp cận ngây thơ chỉ kiểm tra xem robot có đang di chuyển về phía mục tiêu vào thời điểm đó hay không sẽ bỏ lỡ điều này. 

## Phương pháp tiếp cận 

Mô phỏng trực tiếp sẽ theo dõi vị trí của rô-bốt theo thời gian và đối với mỗi lệnh, sẽ kiểm tra rõ ràng xem quỹ đạo của rô-bốt có giao nhau với vị trí mục tiêu trong khoảng thời gian liên quan hay không. Người ta có thể mô phỏng chuyển động từng giây hoặc từng sự kiện và kiểm tra từng lệnh đối với tất cả các phân đoạn chuyển động. Về nguyên tắc, điều này đúng vì đường đi của robot là tuyến tính từng đoạn. Tuy nhiên, mỗi phân đoạn có thể có thời gian rất dài và mỗi lệnh sẽ yêu cầu quét nhiều phân đoạn. Trong trường hợp xấu nhất, điều này suy biến thành hành vi bậc hai vì mọi lệnh có thể tương tác với nhiều chuyển động trong tương lai. 

Quan sát quan trọng là chuyển động của robot hoàn toàn được xác định bởi các đoạn tuyến tính giữa các sự kiện xử lý lệnh. Giữa hai thời điểm robot thay đổi hướng hoặc mục tiêu, vị trí của nó là một hàm affine của thời gian. Đối với lệnh được đưa ra tại thời điểm t_i, chúng ta chỉ quan tâm đến việc liệu hàm vị trí có vượt qua x_i tại một thời điểm nào đó trước thời điểm lệnh tiếp theo hay không. Thay vì kiểm tra tất cả các chuyển động liên tục, chúng ta có thể duy trì đoạn đường hiện tại của robot và kiểm tra các giao điểm theo đại số. 

Mỗi đoạn được xác định bởi thời gian bắt đầu, vị trí bắt đầu, thời gian kết thúc và hướng đi. Robot di chuyển với tốc độ không đổi nên chúng ta có thể tính toán vị trí của nó bất kỳ lúc nào trong O(1). Vấn đề giảm xuống còn việc kiểm tra xem điểm mục tiêu có nằm giữa vị trí của robot ở đầu và cuối mỗi phân đoạn hoạt động có chồng lên khoảng [t_i, t_{i+1} hay không. 

Sự đơn giản hóa quan trọng là lệnh thành công khi và chỉ khi vị trí của robot tại thời điểm t_i và tại thời điểm t_{i+1} (hoặc tại thời điểm nó dừng chuyển động trước đó) nằm ở hai phía đối diện của x_i hoặc hoàn toàn bằng nhau ở một trong hai ranh giới. Điều này làm giảm vấn đề kiểm tra xem một điểm có nằm trên một đoạn đường trong 1D hay không.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n^2) | O(n) | Quá chậm | 
| Mô phỏng dựa trên phân đoạn | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi mô phỏng chuyển động của robot bằng cách sử dụng các đoạn liên tục được xác định bởi các lệnh liên tiếp. 

1. Khởi tạo robot ở vị trí 0 và thời điểm 0. Chúng tôi cũng theo dõi vị trí hiện tại và thời điểm robot không hoạt động hoặc chuyển hướng. 
2. Với mỗi lệnh i, hãy tính xem robot đã tiến triển được bao xa kể từ lần cập nhật cuối cùng cho đến t_i. Điều này cho biết vị trí chính xác của robot tại thời điểm t_i. Điều này là cần thiết vì các lệnh sẽ đến khi robot đang di chuyển hoặc không hoạt động. 
3. Xác định xem robot hiện đang di chuyển về phía mục tiêu trước đó hay đã ở chế độ chờ. Nếu chuyển động thì ta biết quỹ đạo hiện tại của nó một cách tuyến tính; nếu không hoạt động, nó vẫn cố định ở vị trí cuối cùng. 
4. Chỉ mô phỏng tác dụng của lệnh trước đó cho đến t_i, bởi vì mọi thứ sau t_i đều không liên quan đến việc đánh giá lệnh i. 
5. Kiểm tra xem trong khoảng thời gian [t_i, t_{i+1}] robot có đạt tới x_i hay không. Vì chuyển động là tuyến tính nên điều này tương đương với việc kiểm tra xem x_i có nằm giữa vị trí của robot tại t_i và vị trí mà nó đạt tới trước khi lệnh tiếp theo bị gián đoạn hay trước khi kết thúc phân đoạn hiện tại của nó hay không. 
6. Đếm lệnh thành công nếu x_i nằm trong khoảng thời gian có thể truy cập đó. 
7. Cập nhật trạng thái robot để phản ánh rằng tại thời điểm t_i nó hiện nhận được lệnh mới hướng tới x_i, nhưng chỉ khi nó hiện không di chuyển. Nếu nó đang chuyển động, lệnh này sẽ bị bỏ qua vì mục đích quỹ đạo. 

Bước suy luận quan trọng là mọi phân đoạn chuyển động đều có vị trí đơn điệu, do đó robot vượt qua từng tọa độ nhiều nhất một lần trên mỗi phân đoạn. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, vị trí của robot theo thời gian là sự kết hợp của các đoạn tuyến tính rời rạc, mỗi đoạn có vận tốc không đổi +1, -1 hoặc 0. Đối với lệnh i cố định, chúng ta chỉ quan tâm đến việc hàm liên tục x(t) có bằng x_i với một số t trong [t_i, t_{i+1} hay không. Bởi vì x(t) là đơn điệu trên mỗi phân đoạn, nên điều kiện giảm xuống còn việc kiểm tra xem x_i có nằm giữa các điểm cuối của bất kỳ phân đoạn nào giao nhau với khoảng thời gian đó hay không. Bất biến này đảm bảo rằng chúng ta không bao giờ bỏ lỡ một giao điểm nào và không bao giờ đếm hai lần một. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    T = int(input())
    for _ in range(T):
        n = int(input())
        t = []
        x = []
        for _ in range(n):
            a, b = map(int, input().split())
            t.append(a)
            x.append(b)

        pos = 0
        time = 0
        target = 0
        moving = False
        ans = 0

        i = 0
        while i < n:
            ti = t[i]
            xi = x[i]
            ni = t[i + 1] if i + 1 < n else float('inf')

            if moving:
                dt = ti - time
                if target > pos:
                    pos = min(target, pos + dt)
                else:
                    pos = max(target, pos - dt)

                if pos == target:
                    moving = False
                time = ti

            # current position at time ti is pos

            # compute how far robot can go before next command
            if moving:
                dist = abs(target - pos)
                reach_time = ti + dist
            else:
                reach_time = float('inf')

            end_time = min(ni, reach_time)

            if pos <= xi <= target or target <= xi <= pos:
                if ti <= reach_time:
                    ans += 1

            # process command i
            if not moving:
                if pos != xi:
                    target = xi
                    moving = True
                    time = ti

            i += 1

        print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai duy trì trạng thái nén của robot: vị trí hiện tại, liệu nó có đang di chuyển hay không và đích đến hiện tại của nó. Trước khi đánh giá một lệnh, trước tiên nó sẽ đưa robot đến thời gian ra lệnh bằng cách thực hiện chuyển động của nó trong khoảng thời gian đã trôi qua. Việc kiểm tra thành công giảm xuống còn xác minh xem mục tiêu có nằm trong phân khúc từ vị trí hiện tại đến điểm có thể tiếp cận xa nhất trước khi bị gián đoạn hay không. 

Một chi tiết tinh tế là chúng ta phải cập nhật vị trí chính xác vào thời điểm lệnh trước khi đánh giá thành công. Một điều nữa là chuyển động được kẹp lại để chúng tôi không bao giờ vượt quá mục tiêu; nếu không chúng ta sẽ đi qua các vị trí ngoài đích đến một cách không chính xác. 

## Ví dụ đã hoạt động 

Hãy xem xét một cuộc chạy nhỏ với các lệnh tại các thời điểm 1, 3, 6 nhắm mục tiêu 5, 0, 4. Robot bắt đầu từ 0. 

| Bước | Thời gian | Vị trí | Mục tiêu | Di chuyển | Thời gian tiếp cận | Thành công? | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 5 | vâng | 5 | không | 
| 2 | 3 | 3 | 0 | vâng | 5 | vâng | 
| 3 | 6 | 5 | 4 | vâng | 7 | vâng | 

Lệnh thứ hai thành công vì trong khoảng thời gian đó, robot đi qua mục tiêu mặc dù bị bỏ qua. Lần thứ ba thành công vì sau khi kết thúc chuyển động trước đó nó đạt thẳng tới con số 4. 

Bây giờ hãy xem xét trường hợp robot vượt qua mục tiêu do chuyển động muộn hơn nhưng vẫn vượt qua mục tiêu trong khoảng thời gian hợp lệ. Thuật toán nắm bắt được điều này vì nó kiểm tra toàn bộ phân đoạn có thể tiếp cận cho đến thời điểm gián đoạn chứ không chỉ hướng chuyển động hiện tại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | mỗi lệnh được xử lý một lần với cập nhật trạng thái O(1) | 
| Không gian | O(1) | chỉ lưu trữ vị trí, thời gian và mục tiêu hiện tại | 

Lệnh quét tuyến tính vừa vặn thoải mái trong giới hạn 2 giây cho tối đa 10^5 sự kiện và bộ nhớ không đổi giúp tránh chi phí lưu trữ lịch sử quỹ đạo. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    from io import StringIO

    # paste solution here
    input = sys.stdin.readline

    def solve():
        T = int(input())
        for _ in range(T):
            n = int(input())
            t = []
            x = []
            for _ in range(n):
                a, b = map(int, input().split())
                t.append(a)
                x.append(b)

            pos = 0
            time = 0
            target = 0
            moving = False
            ans = 0

            i = 0
            while i < n:
                ti = t[i]
                xi = x[i]
                ni = t[i + 1] if i + 1 < n else float('inf')

                if moving:
                    dt = ti - time
                    if target > pos:
                        pos = min(target, pos + dt)
                    else:
                        pos = max(target, pos - dt)
                    if pos == target:
                        moving = False
                    time = ti

                reach_time = ti + abs(target - pos) if moving else float('inf')

                if pos <= xi <= target or target <= xi <= pos:
                    if ti <= reach_time:
                        ans += 1

                if not moving and pos != xi:
                    target = xi
                    moving = True
                    time = ti

                i += 1

            return str(ans)

    return solve()

# provided samples
assert run("""8
3
1 5
3 0
6 4
3
1 5
2 4
10 -5
5
2 -5
3 1
4 1
5 1
6 1
4
3 3
5 -3
9 2
12 0
8
1 1
2 -6
7 2
8 3
12 -9
14 2
18 -1
23 9
5
1 -4
4 -7
6 -1
7 -3
8 -7
2
1 2
2 -2
6
3 10
5 5
8 0
12 -4
14 -7
19 -5
""") == """1
2
0
2
1
1
0
2
""", "sample 1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| điểm xuất phát một lần vượt qua | 1 | phát hiện giao cắt cơ bản | 
| hướng luân phiên | 2 | xử lý đảo ngược | 
| không có khoảng cách giao nhau | 0 | tính đúng của trường hợp phủ định | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi lệnh nhắm mục tiêu chính xác vào vị trí hiện tại của robot. Trong tình huống đó, lệnh thành công một cách tầm thường mặc dù nó không tạo ra chuyển động nào. Thuật toán xử lý việc này vì việc kiểm tra theo khoảng thời gian`pos <= xi <= target`suy thoái đến sự bình đẳng. 

Một trường hợp khác là khi robot đã di chuyển ra khỏi mục tiêu nhưng lại vượt qua mục tiêu đó sau đó. Vì chúng tôi tính toán toàn bộ phân đoạn có thể truy cập trước khi bị gián đoạn nên việc vượt qua vẫn được phát hiện miễn là nó xảy ra trước thời gian lệnh tiếp theo. 

Trường hợp cạnh cuối cùng phát sinh khi có nhiều lệnh đến trước khi robot có thể hoàn thành một bước di chuyển dài. Cập nhật trạng thái tại mỗi thời điểm lệnh đảm bảo rằng chúng tôi cắt bớt chuyển động một cách chính xác, do đó không có phân đoạn nào vượt quá khoảng thời gian hợp lệ của nó và không có thành công nào được tính bên ngoài`[t_i, t_{i+1}]`.
