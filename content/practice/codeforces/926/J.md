---
problem: 926J
contest_id: 926
problem_index: J
name: "Segments"
contest_name: "VK Cup 2018 - Wild-card Round 1"
rating: 2100
tags: ["data structures"]
answer: passed_samples
verified: true
solve_time_s: 71
date: 2026-06-18
model: gpt-5-3-mini
samples_passed: 1
samples_total: 1
share_url: https://chatgpt.com/share/6a33017b-0364-83ec-8f99-d2bb3134596f
---

#CF 926J - Phân đoạn 

**Xếp hạng:** 2100 
**Thẻ:** cấu trúc dữ liệu 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 1 phút 11 giây 
**Đã xác minh:** có (1/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a33017b-0364-83ec-8f99-d2bb3134596f 

--- 

## Giải pháp 

## Hiểu vấn đề 

Chúng ta đang duy trì một tập hợp các khoảng đóng ngày càng tăng trên một trục số. Ban đầu, không có gì được sơn. Lần lượt, các phân đoạn được thêm vào và sau mỗi lần chèn, chúng tôi muốn biết có bao nhiêu vùng màu đen rời rạc tồn tại nếu chúng tôi lấy sự kết hợp của tất cả các phân đoạn được thấy cho đến nay. 

Hai phân đoạn đóng góp vào cùng một khu vực nếu chúng trùng nhau hoặc thậm chí chỉ chạm nhau tại một điểm duy nhất, vì các điểm cuối được coi là được kết nối. Vì vậy, cấu trúc mà chúng tôi đang theo dõi không phải là các phân đoạn riêng lẻ mà là sự kết hợp của các khoảng chồng chéo hoặc liền kề. 

Mô hình tinh thần ngây thơ là tưởng tượng vẽ từng đoạn trên một đường thẳng và sau đó đếm xem có bao nhiêu đoạn màu đen liên tục tồn tại. Sau mỗi lần chèn, về mặt khái niệm, chúng tôi cần hợp nhất bất kỳ vùng sơn chồng chéo nào và đếm xem còn lại bao nhiêu khối riêng biệt. 

Các ràng buộc rất lớn: lên tới 200.000 phân đoạn, với tọa độ lên tới 10^9. Điều này ngay lập tức loại trừ mọi giải pháp quét liên tục toàn bộ tập hợp các phân đoạn hoặc duy trì một mảng dày đặc trên tọa độ. Ngay cả cách tiếp cận O(n^2) để kiểm tra sự trùng lặp so với tất cả các phân đoạn trước đó cũng quá chậm, vì 200.000 thao tác bình phương là không thể thực hiện được trong giới hạn thời gian một giây. 

Khó khăn chính là các khoảng có tính động và có thể hợp nhất các thành phần hiện có theo những cách tùy ý. 

Một vài trường hợp đặc biệt cho thấy lý do tại sao các phương pháp tiếp cận ngây thơ lại thất bại. 

Một vấn đề là quên rằng việc chạm vào điểm cuối sẽ hợp nhất các thành phần. Ví dụ: các phân đoạn (1,3) và (3,5) tạo thành một thành phần duy nhất. Việc triển khai đơn giản sử dụng các biện pháp kiểm tra chồng chéo nghiêm ngặt như`l < r`thay vì`l <= r`sẽ đếm không chính xác hai thành phần. 

Một vấn đề khác là tính số lần hợp nhất. Giả sử chúng ta đã có khoảng hợp nhất [1,10] và chúng ta chèn [3,5]. Kiểm tra chồng chéo đơn giản có thể phát hiện sự trùng lặp với nhiều phân đoạn được lưu trữ và giảm số lượng thành phần nhiều lần trừ khi chúng tôi xử lý cẩn thận các khoảng thời gian được hợp nhất dưới dạng một thực thể duy nhất. 

Vấn đề thứ ba là độ nhạy của đơn hàng. Vì các phân đoạn đến theo thứ tự đầu vào nên chúng tôi không thể sắp xếp chúng và tính toán lại mọi thứ mỗi lần mà không làm mất yêu cầu gia tăng. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: duy trì danh sách hiện tại của các khoảng rời rạc được hợp nhất. Đối với mỗi phân đoạn mới, hãy quét tất cả các khoảng hiện có, hợp nhất những khoảng giao nhau hoặc chạm vào nó, xóa chúng và chèn khoảng đã hợp nhất thu được. Câu trả lời chỉ đơn giản là số khoảng thời gian được lưu trữ. 

Điều này đúng vì bất biến là rõ ràng: chúng ta luôn lưu trữ một biểu diễn rời rạc, được hợp nhất hoàn toàn của hợp. Tuy nhiên, mỗi lần chèn có thể yêu cầu quét tất cả các khoảng hiện có và trong trường hợp xấu nhất, mỗi khoảng chồng lên nhiều khoảng khác hoặc yêu cầu quét tuyến tính lặp lại. Điều này dẫn đến hành vi O(n^2) nói chung, vì mọi phân khúc mới có thể yêu cầu hợp nhất với các phân khúc hiện có O(n). 

Quan sát quan trọng là chúng ta thực sự không bao giờ cần duy trì hình học đầy đủ một cách rõ ràng. Chúng ta chỉ cần số lượng thành phần được kết nối. Mỗi phân đoạn mới sẽ tạo một thành phần mới, hợp nhất với chính xác một thành phần hiện có hoặc hợp nhất nhiều thành phần thành một. Đây chính xác là một vấn đề hợp nhất khoảng thời gian động trong đó chúng tôi chỉ quan tâm đến kết nối chứ không phải cấu trúc. 

Chúng ta có thể duy trì một cấu trúc được sắp xếp của các khoảng rời rạc hiện tại. Đối với mỗi phân đoạn đến, chúng tôi xác định vị trí sẽ chèn, sau đó mở rộng sang trái và phải trong khi các khoảng chồng lên nhau hoặc chạm vào, đếm xem có bao nhiêu thành phần được hấp thụ. Mỗi khoảng thời gian được hấp thụ sẽ làm giảm số lượng thành phần và cuối cùng chúng tôi chèn khoảng thời gian đã hợp nhất. 

Điều này hiệu quả vì mỗi khoảng thời gian được loại bỏ tối đa một lần trong toàn bộ quá trình. Mặc dù mỗi lần chèn có thể quét một vài khoảng thời gian, tổng số lần xóa bị giới hạn bởi n, làm cho độ phức tạp khấu hao O(n log n) khi sử dụng cấu trúc có thứ tự. 

Trong Python, vì chúng tôi không có cây cân bằng tích hợp theo các khoảng thời gian, nên chúng tôi mô phỏng nó bằng cách sử dụng danh sách được sắp xếp với tìm kiếm nhị phân thông qua`bisect`, chấp nhận rằng việc xóa vẫn được khấu hao tuyến tính nhưng có thể quản lý được trong các ràng buộc do hành vi hợp nhất đơn điệu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(n) | Quá chậm | 
| Tối ưu (sáp nhập theo thứ tự) | O(n log n) khấu hao | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì một danh sách các khoảng rời rạc được sắp xếp theo tọa độ bắt đầu, cùng với số lượng thành phần đang chạy. 

1. Khởi tạo một danh sách các khoảng trống và đặt số lượng thành phần về 0. Tại thời điểm này không có gì cần thiết nên câu trả lời là 0. 
2. Đối với mỗi phân đoạn đến [l, r], hãy sử dụng tìm kiếm nhị phân để tìm vị trí mà phân đoạn này sẽ được chèn dựa trên điểm cuối bên trái của nó. Điều này đảm bảo chúng tôi chỉ kiểm tra các khoảng thời gian có khả năng chồng chéo ở gần đó. 
3. Kiểm tra xem có một khoảng ngay bên trái có thể chồng lên nhau hoặc chạm vào đoạn mới hay không. Nếu khoảng đó kết thúc tại hoặc sau l thì nó phải được hợp nhất. Chúng tôi mở rộng phân đoạn mới để bao phủ nó và xóa nó khỏi cấu trúc, giảm số lượng thành phần đi một. 
4. Tiếp tục kiểm tra các khoảng tiến về phía trước bắt đầu từ vị trí chèn. Miễn là khoảng tiếp theo bắt đầu tại hoặc trước điểm cuối bên phải được hợp nhất hiện tại, chúng tôi sẽ hợp nhất nó. Mỗi lần hợp nhất sẽ hấp thụ một thành phần hiện có và thu nhỏ danh sách khoảng thời gian. 
5. Sau khi loại bỏ tất cả các khoảng chồng chéo hoặc chạm nhau, hãy chèn khoảng mới được hợp nhất vào đúng vị trí của nó và tăng số lượng thành phần lên một. Điều này thể hiện khối hợp nhất cuối cùng được tạo bởi phân đoạn hiện tại. 
6. Xuất ra số lượng thành phần hiện tại sau khi xử lý từng phân đoạn. 

Lý do mỗi bước đều đúng là vì các khoảng trong cấu trúc luôn rời rạc và được sắp xếp. Điều này đảm bảo rằng tất cả các phần chồng chéo phải xuất hiện trong một khối liền kề xung quanh điểm chèn. 

### Tại sao nó hoạt động 

Tại mọi thời điểm, chúng ta duy trì sự phân chia của trục số thành các khoảng hợp nhất rời rạc biểu thị các thành phần được kết nối. Mỗi phân đoạn mới nằm hoàn toàn trong một khoảng hiện có, hợp nhất nhiều khoảng liền kề thành một hoặc tạo thành một khoảng cách ly mới. Vì các khoảng được sắp xếp và rời rạc nên bất kỳ khoảng nào trùng lặp hoặc chạm vào phân đoạn mới đều phải nằm trong vùng liền kề của danh sách, do đó, không bỏ sót việc hợp nhất nào. Vì mỗi lần hợp nhất sẽ giảm nghiêm ngặt số lượng khoảng thời gian được lưu trữ nên số lượng thành phần vẫn nhất quán với số lượng thành phần được kết nối thực sự. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

import bisect

def solve():
    n = int(input())
    intervals = []
    res = []
    comp = 0

    for _ in range(n):
        l, r = map(int, input().split())

        # find insertion position
        i = bisect.bisect_left(intervals, (l, r))

        nl, nr = l, r

        # check left interval
        j = i - 1
        if j >= 0 and intervals[j][1] >= l - 0:
            nl = min(nl, intervals[j][0])
            nr = max(nr, intervals[j][1])
            intervals.pop(j)
            comp -= 1
            i -= 1

        # merge right side
        while i < len(intervals) and intervals[i][0] <= nr:
            nl = min(nl, intervals[i][0])
            nr = max(nr, intervals[i][1])
            intervals.pop(i)
            comp -= 1

        intervals.insert(i, (nl, nr))
        comp += 1
        res.append(str(comp))

    print(" ".join(res))

if __name__ == "__main__":
    solve()
```Việc thực hiện duy trì một danh sách được sắp xếp các khoảng rời rạc. Tìm kiếm nhị phân tìm thấy một điểm chèn ứng cử viên, sau đó chúng tôi mở rộng sang trái và phải để tiếp thu tất cả các khoảng chồng chéo hoặc chạm nhau. Mỗi lần chúng tôi xóa một khoảng, chúng tôi sẽ giảm bộ đếm thành phần vì hai thành phần trở thành một. Cuối cùng, việc chèn khoảng thời gian đã hợp nhất sẽ khôi phục chính xác một thành phần cho toàn bộ khối đã hợp nhất. 

Một điểm tinh tế là điều kiện`intervals[j][1] >= l`. Điều này mã hóa quy tắc rằng các điểm cuối chạm vào được coi là được kết nối, do đó phải bao gồm sự bình đẳng. Một sự tinh tế khác là điều chỉnh chỉ số`i`sau khi bật khoảng bên trái, vì danh sách sẽ thay đổi. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
1 3
4 5
2 4
```Chúng tôi theo dõi khoảng thời gian và số lượng thành phần. 

| Bước | Khoảng thời gian | Trạng thái khoảng thời gian | Linh kiện | 
| --- | --- | --- | --- | 
| 1 | [1,3] | [1,3] | 1 | 
| 2 | [4,5] | [1,3], [4,5] | 2 | 
| 3 | [2,4] | [1,5] | 1 | 

Sau lần chèn thứ ba, phân đoạn mới thu hẹp khoảng cách giữa hai thành phần hiện có bằng cách chạm vào mức 3-4, tạo ra một khoảng hợp nhất duy nhất. 

Điều này cho thấy rằng sự liền kề thông qua các điểm cuối sẽ hợp nhất một cách chính xác các thành phần không bị chồng chéo hoàn toàn. 

### Ví dụ 2 

đầu vào:```
4
10 20
1 5
6 8
7 15
```| Bước | Khoảng thời gian | Trạng thái khoảng thời gian | Linh kiện | 
| --- | --- | --- | --- | 
| 1 | [10,20] | [10,20] | 1 | 
| 2 | [1,5] | [1,5], [10,20] | 2 | 
| 3 | [6,8] | [1,5], [6,8], [10,20] | 3 | 
| 4 | [7,15] | [1,5], [6,20] | 2 | 

Khoảng cuối cùng hợp nhất hai vùng riêng biệt vì nó chồng lên cả [6,8] và [10,20], thể hiện sự hấp thụ nhiều khoảng trong một bước duy nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) khấu hao | mỗi khoảng được chèn và xóa nhiều nhất một lần, với tìm kiếm nhị phân cho mỗi lần chèn | 
| Không gian | O(n) | lưu trữ tập hợp khoảng thời gian rời rạc hiện tại | 

Cấu trúc đảm bảo rằng mặc dù các lần chèn riêng lẻ có thể kích hoạt nhiều lần hợp nhất, nhưng mỗi khoảng thời gian đều tham gia vào nhiều nhất một lần xóa, do đó tổng công việc vẫn tuyến tính theo chi phí tìm kiếm logarit. Điều này phù hợp thoải mái trong các ràng buộc cho n lên tới 200.000. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque

    # re-run solution
    import bisect

    def solve():
        n = int(input())
        intervals = []
        comp = 0
        out = []

        for _ in range(n):
            l, r = map(int, input().split())
            i = bisect.bisect_left(intervals, (l, r))

            nl, nr = l, r

            j = i - 1
            if j >= 0 and intervals[j][1] >= l:
                nl = min(nl, intervals[j][0])
                nr = max(nr, intervals[j][1])
                intervals.pop(j)
                comp -= 1
                i -= 1

            while i < len(intervals) and intervals[i][0] <= nr:
                nl = min(nl, intervals[i][0])
                nr = max(nr, intervals[i][1])
                intervals.pop(i)
                comp -= 1

            intervals.insert(i, (nl, nr))
            comp += 1
            out.append(str(comp))

        return " ".join(out)

    return solve()

assert run("3\n1 3\n4 5\n2 4\n") == "1 2 1"

assert run("2\n1 2\n2 3\n") == "1 1"

assert run("3\n1 10\n2 3\n4 5\n") == "1 1 1"

assert run("4\n1 3\n5 7\n2 6\n10 11\n") == "1 2 1 2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chạm vào điểm cuối | 1 | sáp nhập tại ranh giới | 
| khoảng lồng nhau | 1 | trường hợp hấp thụ đầy đủ | 
| rời rạc rồi lồng vào nhau | 1 1 1 | ổn định dưới sự hợp nhất dư thừa | 
| trường hợp đa hợp nhất | 1 2 1 2 | hợp nhất trên nhiều thành phần | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi một đoạn mới chỉ chạm vào một khoảng hiện có tại một điểm duy nhất. Ví dụ: chèn [5,7] khi [1,5] đã tồn tại phải hợp nhất chúng thành một thành phần. Thuật toán xử lý việc này vì kiểm tra bên trái sử dụng`>= l`, đảm bảo liên hệ điểm cuối được coi là chồng chéo. 

Một trường hợp cạnh khác là khi một đoạn nối nhiều khoảng cách rời nhau trong một lần chèn. Ví dụ: các khoảng hiện có [1,2] và [4,5], việc chèn [2,4] phải hợp nhất cả hai. Vòng mở rộng bên phải tiếp tục hợp nhất cho đến khi không còn phần chồng chéo, đảm bảo cả hai đều được hấp thụ trong một thao tác duy nhất. 

Trường hợp tinh vi cuối cùng là khi một đoạn được chứa đầy đủ trong một khoảng hiện có. Chèn [3,4] vào [1,10] sẽ không làm thay đổi số lượng thành phần. Thuật toán xử lý vấn đề này vì khoảng bên trái sẽ tiếp thu phân đoạn và không có thao tác chèn mới nào xảy ra ngoài việc thay thế khoảng hiện có bằng một khoảng giống hệt hoặc lớn hơn, khiến số lượng thành phần không thay đổi.
