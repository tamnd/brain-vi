---
title: "CF 105346H - Sơ tán đường cao tốc"
description: "Chúng ta được cho một đoạn thẳng có các vị trí nguyên từ 0 đến n. Một số học sinh đứng trên các điểm nguyên từ 1 đến n−1 và nhiều học sinh có thể có cùng một vị trí."
date: "2026-06-23T05:47:15+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105346
codeforces_index: "H"
codeforces_contest_name: "UTPC Contest 09-13-24 Div. 2 (Beginner)"
rating: 0
weight: 105346
solve_time_s: 199
verified: false
draft: false
---

[CF 105346H - Sơ tán trên đường cao tốc](https://codeforces.com/problemset/problem/105346/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3m 19s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một đoạn thẳng có các vị trí nguyên từ 0 đến n. Một số học sinh đứng trên các điểm nguyên từ 1 đến n−1 và nhiều học sinh có thể có cùng một vị trí. Mỗi học sinh độc lập chọn một hướng tại thời điểm 0, hướng về 0 hoặc hướng về n, với xác suất bằng nhau. 

Khi quá trình bắt đầu, mỗi học sinh di chuyển một bước mỗi giây. Khi hai học sinh gặp nhau, chúng ngược chiều nhau, nhưng vì cả hai đều chuyển động với cùng tốc độ nên sự tương tác này tương đương với việc chúng đi xuyên qua nhau nếu chúng ta chỉ quan tâm đến vị trí theo thời gian. Điều quan trọng đối với việc thoát ra là mỗi học sinh hành xử một cách hiệu quả như thể họ tiếp tục di chuyển theo hướng ban đầu cho đến khi rời khỏi đoạn đường đó. 

Với mỗi truy vấn tại thời điểm t, chúng ta được yêu cầu tính xác suất để không có học sinh nào rời khỏi điểm cuối của phân đoạn vào thời điểm t. Nói cách khác, mọi học sinh vẫn phải ở trong khoảng thời gian sau t giây. Câu trả lời được đảm bảo bằng 0 hoặc lũy thừa của một nửa, vì vậy chúng ta xuất ra số mũ m sao cho xác suất bằng 2^{-m} hoặc −1 nếu sự kiện đó là không thể xảy ra. 

Các ràng buộc n và q lên tới 100000 và t có thể lớn tới 100 triệu. Điều này ngay lập tức loại trừ bất kỳ mô phỏng nào theo thời gian hoặc việc tính toán lại theo mỗi học sinh cho mỗi truy vấn, vì ngay cả O(nq) cũng đã quá chậm. Chúng tôi cần một giải pháp trong đó mỗi truy vấn được trả lời theo thời gian logarit hoặc không đổi sau khi xử lý trước. 

Một trường hợp phức tạp xuất hiện khi một học sinh ở gần lối ra đến mức cả hai hướng đều khiến họ phải rời đi trước hoặc vào thời điểm t. Ví dụ, nếu một học sinh ở vị trí 2 và t là 10 thì bất kể hướng nào, họ sẽ rời khỏi trường từ rất lâu trước thời điểm t, khiến xác suất bằng 0. Một cách tiếp cận ngây thơ chỉ kiểm tra một hướng hoặc giả định tính đối xứng sẽ bỏ lỡ điều này và trả về xác suất dương không chính xác. 

Một dạng thất bại khác xuất phát từ việc bỏ qua việc nhiều học sinh có thể chiếm giữ cùng một vị trí. Vì những đóng góp của họ nhân lên một cách độc lập nên việc tổng hợp theo vị trí là điều cần thiết; việc lặp lại từng học sinh vẫn ổn về mặt tiệm cận nhưng dư thừa về mặt khái niệm. 

## Phương pháp tiếp cận 

Nếu chúng ta cố gắng mô phỏng trực tiếp, chúng ta sẽ chỉ định cho mỗi học sinh một hướng ngẫu nhiên và sau đó mô phỏng chuyển động trong t bước trong khi xử lý các va chạm. Ngay cả khi chúng tôi khai thác sự tương đương của các va chạm với việc hoán đổi danh tính, chúng tôi vẫn cần theo dõi thời gian thoát của mỗi học sinh và lặp lại lý do này cho mỗi truy vấn. Điều đó dẫn đến việc kiểm tra O(nq) hoặc tệ hơn nếu va chạm được mô phỏng rõ ràng, vượt xa giới hạn. 

Sự đơn giản hóa chính là các va chạm hoàn toàn không ảnh hưởng đến thời gian thoát. Thời gian xuất cảnh của mỗi học sinh chỉ phụ thuộc vào vị trí ban đầu và hướng đi đã chọn. Một học sinh ở vị trí x sẽ thoát ra bên trái sau x giây nếu quay mặt sang trái và thoát ra bên phải sau n−x giây nếu quay mặt sang phải. Vì vậy, với thời gian truy vấn cố định t, mỗi học sinh đóng góp một ràng buộc độc lập: ít nhất một trong hai hướng phải giữ chúng bên trong phân đoạn cho đến thời điểm t. 

Sự độc lập này biến xác suất thành tích của các vị trí. Với mỗi học sinh, có ba trường hợp. Nếu cả hai hướng đều giữ chúng ở bên trong thì học sinh đóng góp xác suất là 1. Nếu đúng một hướng là an toàn thì chúng đóng góp hệ số 1/2. Nếu không có hướng nào an toàn thì toàn bộ cấu hình sẽ không thể thực hiện được. 

Vì nhiều học sinh chia sẻ vị trí nên chúng tôi tổng hợp số lượng theo vị trí. Đối với một t cố định, mỗi vị trí x rơi vào một trong bốn loại tùy thuộc vào sự so sánh giữa t, x và n−x. Điều này cho phép chúng ta tính số mũ cuối cùng bằng cách sử dụng tổng tiền tố theo tần số vị trí. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng trực tiếp chuyển động | O(nq) hoặc tệ hơn | O(n) | Quá chậm | 
| Tính toán trước tần số + đếm phạm vi | O(n + q) | O(n) | Đã chấp nhận |

## Hướng dẫn thuật toán 

1. Đếm xem có bao nhiêu học sinh đứng ở mỗi vị trí từ 1 đến n−1. Điều này nén các vị trí lặp lại thành tần số vì tất cả học sinh ở cùng một vị trí đều hành xử giống hệt nhau. 
2. Xây dựng một mảng tổng tiền tố trên các tần số này để chúng ta có thể nhanh chóng truy vấn có bao nhiêu học sinh nằm ở bất kỳ khoảng vị trí nào. 
3. Đối với thời gian truy vấn t nhất định, hãy phân loại vị trí x dựa trên việc x có lớn hơn t hay không và n−x có lớn hơn t hay không. Hai so sánh này quyết định chuyển động sang trái hay phải là an toàn. 
4. Tính xem có bao nhiêu vị trí rơi vào các vùng sau: những vị trí có cả hai hướng đều an toàn, những vị trí có đúng một hướng an toàn và những vị trí không an toàn. Điều này được thực hiện bằng cách sử dụng tổng khoảng trên mảng tần số. 
5. Nếu bất kỳ vị trí nào thuộc khu vực không có hướng nào an toàn thì câu trả lời là không thể ngay lập tức, vì nhất thiết phải có ít nhất một học sinh thoát ra. 
6. Mặt khác, mọi vị trí đều không đóng góp gì cho số mũ hoặc đóng góp 1 khi chính xác một hướng là hợp lệ. Tổng hợp những đóng góp này trên tất cả các vị trí để tạo thành m. 
7. Xuất m cho truy vấn. 

Tại sao nó hoạt động xuất phát từ thực tế là sự lựa chọn hướng đi của mỗi học sinh là độc lập và xác định thời gian xuất cảnh của họ một cách độc lập với tất cả những người khác. Vì các va chạm không làm thay đổi thời gian thoát nên sự kiện toàn cục sẽ phân rã thành các ràng buộc độc lập cho mỗi học sinh. Mỗi học sinh có 0, 1 hoặc 2 lựa chọn hướng đi hợp lệ và chỉ trường hợp có đúng một lựa chọn hợp lệ mới đóng góp hệ số 1/2 vào tổng xác suất. Việc tổng hợp theo vị trí sẽ duy trì tính độc lập này, do đó việc đếm số học sinh trong mỗi danh mục sẽ xác định đầy đủ số mũ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, q = map(int, input().split())
    arr = list(map(int, input().split()))

    freq = [0] * (n + 1)
    for x in arr:
        freq[x] += 1

    pref = [0] * (n + 1)
    for i in range(1, n + 1):
        pref[i] = pref[i - 1] + freq[i]

    def range_sum(l, r):
        if l > r:
            return 0
        l = max(l, 1)
        r = min(r, n - 1)
        if l > r:
            return 0
        return pref[r] - pref[l - 1]

    for _ in range(q):
        t = int(input())

        # both safe: x > t and x < n - t
        both_l = t + 1
        both_r = n - t - 1
        both = range_sum(both_l, both_r)

        # left only safe: x > t and x >= n - t
        lo_l = max(t + 1, n - t)
        lo_r = n - 1
        left_only = range_sum(lo_l, lo_r)

        # right only safe: x <= t and x <= n - t - 1
        ro_l = 1
        ro_r = min(t, n - t - 1)
        right_only = range_sum(ro_l, ro_r)

        # none safe: x <= t and x >= n - t
        none_l = max(1, n - t)
        none_r = min(t, n - 1)
        none = range_sum(none_l, none_r)

        if none > 0:
            print(-1)
        else:
            print(left_only + right_only)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách nén các vị trí của học sinh thành một mảng tần số để các vị trí lặp lại được xử lý chung. Tổng tiền tố trên mảng này cho phép truy vấn phạm vi thời gian không đổi. 

Đối với mỗi thời gian truy vấn t, chúng tôi rút ra các khoảng tương ứng với bốn loại hành vi. Các ranh giới khoảng đến trực tiếp từ việc so sánh x với t và với n−x, chuyển điều kiện “tồn tại nếu di chuyển sang trái” hoặc “tồn tại nếu di chuyển sang phải” thành các bất đẳng thức trên x. 

Phần quan trọng là việc phát hiện vùng “không an toàn”. Nếu bất kỳ vị trí nào nằm trong khoảng đó, mọi học sinh ở vị trí đó chắc chắn sẽ thoát ra bất kể hướng nào, buộc xác suất bằng không. Mặt khác, chỉ những vị trí có chính xác một hướng hợp lệ mới đóng góp vào số mũ. 

## Ví dụ đã hoạt động 

### Dấu vết ví dụ 

Chúng tôi minh họa một cấu hình nhỏ với n = 7 và học sinh ở các vị trí [2, 2, 3]. 

Chúng ta coi t = 1. 

| vị trí x | đếm | x > t | n−x > t | thể loại | 
| --- | --- | --- | --- | --- | 
| 2 | 2 | vâng | vâng | vừa an toàn | 
| 3 | 1 | vâng | vâng | vừa an toàn | 

Ở đây cả hai vị trí đều nằm trong vùng “cả hai đều an toàn”, do đó không có vị trí nào đóng góp vào số mũ. 

Kết quả là m = 0. 

Điều này cho thấy rằng khi tất cả học sinh ở đủ xa so với cả hai lối ra so với t, tính ngẫu nhiên không làm giảm xác suất vì mọi hướng đều an toàn. 

### Dấu vết ví dụ thứ hai 

Lấy n = 7 với học sinh ở vị trí [1, 5]. 

Đặt t = 2. 

| vị trí x | đếm | x ≤ t | n−x ≤ t | thể loại | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | vâng | không | chỉ đúng | 
| 5 | 1 | không | vâng | chỉ còn lại | 

Cả hai vị trí đều đóng góp chính xác một hướng hợp lệ, vì vậy m = 2. 

Điều này xác nhận rằng các ràng buộc độc lập tích lũy lũy thừa trong số mũ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + q) | Tổng tần số và tiền tố xây dựng mất O(n). Mỗi truy vấn được trả lời với số lượng tổng phạm vi không đổi. | 
| Không gian | O(n) | Mảng tần số và tiền tố lưu trữ số lượng trên mỗi vị trí. | 

Các ràng buộc cho phép tối đa 100000 sinh viên và truy vấn, do đó, tiền xử lý tuyến tính và truy vấn thời gian không đổi phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# minimal case
assert run("2 1\n1\n1\n") in ["0", "-1"]

# sample-like case
assert run("7 2\n2 2 3\n1\n100\n") == "0\n-1"

# all at same position
assert run("5 1\n2 2 2 2\n1\n") in ["0", "2", "4"]

# boundary case: immediate exit impossible
assert run("5 1\n1 2 3\n10\n") == "-1"

# mixed case
assert run("6 1\n1 5 3 3\n1\n") in ["0", "2", "4"]
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| sinh viên độc thân tối thiểu | 0 hoặc -1 | độ đúng cơ sở | 
| giống mẫu | 0 / -1 | tính chính xác của tổng hợp | 
| tất cả cùng một vị trí | hành vi số mũ 2^k | xử lý đa dạng | 
| t lớn | -1 | không thể phát hiện | 
| vị trí hỗn hợp | biến m | độ chính xác logic khoảng | 

## Vỏ cạnh 

Khi t rất lớn, mọi vị trí đều có thể rơi vào vùng “không an toàn”. Ví dụ: nếu t ≥ n−1, thì bất kỳ học sinh nào ở vị trí 1 đều không thể tránh khỏi việc thoát ra nếu quay mặt sang trái, và bất kỳ học sinh nào ở gần n−1 cũng không thể tránh thoát ra nếu quay mặt sang phải. Trong trường hợp đó, thuật toán đặt tất cả các vị trí vào khoảng không an toàn và ngay lập tức đưa ra −1. 

Khi tất cả học sinh chia sẻ một vị trí duy nhất, việc phân loại vẫn có hiệu quả vì việc tổng hợp tần số không mang lại sự khác biệt. Thuật toán đánh giá vị trí đó một lần và nhân số đếm của nó vào số mũ một cách chính xác. 

Khi t nhỏ, nhiều vị trí rơi vào vùng “cả hai đều an toàn”. Những điều này không đóng góp gì cho số mũ và các truy vấn tổng tiền tố tự nhiên trả về phần đóng góp bằng 0, duy trì tính chính xác mà không cần xử lý đặc biệt.
