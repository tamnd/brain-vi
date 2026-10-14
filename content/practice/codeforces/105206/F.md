---
title: "CF 105206F - CON ĐƯỜNG TRỞ THÀNH HUYỀN THOẠI"
description: "Chúng tôi được cung cấp một số trường hợp thử nghiệm độc lập. Trong mỗi trường hợp thử nghiệm có $N$ cô gái và mỗi cô gái $i$ được gán một phân đoạn $[li, ri]$."
date: "2026-06-27T02:40:43+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105206
codeforces_index: "F"
codeforces_contest_name: "VitebskOpen Junior"
rating: 0
weight: 105206
solve_time_s: 56
verified: true
draft: false
---

[CF 105206F - ĐƯỜNG TRỞ THÀNH HUYỀN THOẠI](https://codeforces.com/problemset/problem/105206/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 56s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số trường hợp thử nghiệm độc lập. Trong mỗi trường hợp thử nghiệm có$N$các cô gái và mỗi cô gái$i$được chỉ định một phân đoạn$[l_i, r_i]$. Nhiệm vụ là quyết định xem liệu chúng ta có thể gán cho mỗi cô gái một số nguyên riêng biệt “số cửa hàng” sao cho cô gái đó không$i$nhận được một cửa hàng nằm trong phân khúc được phép của cô ấy và không có hai cô gái nào ở cùng một cửa hàng. 

Mặc dù các cửa hàng đều được dán nhãn từ 1 đến$10^9$, chỉ có thứ tự tương đối mới quan trọng, vì chúng ta thực sự không bao giờ cần xây dựng tất cả các cửa hàng có sẵn. Điều quan trọng là liệu chúng ta có thể đặt$N$điểm, mỗi điểm một khoảng, không có va chạm. 

Các ràng buộc rất lớn: tổng số khoảng thời gian trên tất cả các trường hợp thử nghiệm lên tới$2 \cdot 10^5$. Điều này ngay lập tức loại trừ mọi suy luận bậc hai cho mỗi trường hợp thử nghiệm. Bất kỳ cách tiếp cận nào liên tục quét tất cả các khoảng thời gian còn lại hoặc thử tất cả các bài tập một cách tham lam với các vòng lặp lồng nhau sẽ thất bại. Chúng ta cần một cái gì đó gần gũi hơn$O(N \log N)$cho mỗi trường hợp thử nghiệm hoặc khấu hao tuyến tính trên tất cả các trường hợp thử nghiệm. 

Một dạng thất bại phổ biến ở đây xuất phát từ các chiến lược tham lam bỏ qua cấu trúc khoảng thời gian. 

Một ý tưởng sai lầm là luôn gán cửa hàng nhỏ nhất có thể có cho mỗi khoảng theo thứ tự tùy ý. Hãy xem xét các khoảng:$[1, 100]$,$[1, 1]$,$[2, 2]$. 

Nếu chúng ta xử lý theo thứ tự đầu vào và gán một cách tham lam, chúng ta có thể gán 1 cho khoảng đầu tiên, chặn khoảng thứ hai, ngay cả khi tồn tại phép gán hợp lệ: gán 1 cho$[1,1]$, 2 đến$[2,2]$, và 100 đến$[1,100]$. Sai lầm là xử lý các khoảng thời gian một cách độc lập mà không tôn trọng tính cấp thiết. 

Một lỗi khác là sắp xếp theo điểm cuối bên trái và tham lam gán các vị trí ngày càng tăng. Điều đó cũng thất bại vì khoảng thời gian bắt đầu sớm không nhất thiết là hạn chế nhất. 

Khó khăn chính là mỗi khoảng thời gian đều có cấu trúc “giống như thời hạn” do điểm cuối bên phải của nó áp đặt. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng chỉ định các cửa hàng một cách đệ quy hoặc thông qua việc quay lui. Ở mỗi bước, chúng tôi chọn một cô gái và thử mọi cửa hàng có sẵn trong khoảng thời gian của cô ấy. Trong trường hợp xấu nhất, điều này khám phá các cấu hình theo cấp số nhân, vì mỗi phép gán làm giảm tính khả dụng đi một nhưng phân nhánh vẫn lớn. Ngay cả khi cắt tỉa, trường hợp xấu nhất sẽ thoái hóa nhanh chóng khi các khoảng thời gian chồng chéo lên nhau. 

Cấu trúc của bài toán gợi ý cách giải thích về lịch trình: mỗi cô gái phải được gán một số nguyên duy nhất trong một phạm vi, tương đương với việc đặt các công việc có độ dài đơn vị trên một trục số có ràng buộc. Điều này liên quan chặt chẽ đến việc lập kế hoạch ngắt quãng với thời hạn. 

Cái nhìn sâu sắc quan trọng là xử lý các khoảng thời gian theo thứ tự tăng dần của điểm cuối bên phải của chúng. Một khi chúng ta ở vị trí$x$, chúng tôi muốn gán nó cho khoảng thời gian hạn chế nhất hiện có tại thời điểm đó, cụ thể là khoảng thời gian kết thúc sớm nhất trong số những khoảng thời gian vẫn có thể chấp nhận$x$. Điều này tránh lãng phí các điểm cuối nhỏ trong khoảng thời gian linh hoạt. 

Chúng tôi mô phỏng việc quét các vị trí cửa hàng có thể có từ trái sang phải, nhưng chúng tôi không lặp lại tất cả các số nguyên. Thay vào đó, chúng tôi duy trì một tập hợp các khoảng hoạt động có điểm cuối bên trái đã đạt đến và chúng tôi luôn chỉ định vị trí sẵn có tiếp theo cho khoảng có điểm cuối bên phải nhỏ nhất. Nếu tại một thời điểm nào đó, điểm cuối bên phải nhỏ nhất nhỏ hơn vị trí hiện tại, điều đó có nghĩa là khoảng đó không thể được thỏa mãn nữa. 

Điều này làm giảm vấn đề xuống còn kiểm tra tính khả thi tham lam cổ điển bằng cách sử dụng min-heap. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Quay lại vũ phu | Hàm mũ | O(N) | Quá chậm | 
| Sắp xếp + Min Heap tham lam |$O(N \log N)$| O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giải thích mỗi khoảng thời gian sẽ có sẵn tại$l_i$và hết hạn vào lúc$r_i$. Chúng tôi mô phỏng việc chỉ định các chỉ số cửa hàng ngày càng tăng. 

1. Sắp xếp tất cả các khoảng theo điểm cuối bên trái của chúng. Điều này cho phép chúng tôi kích hoạt các khoảng thời gian chính xác khi họ đủ điều kiện nhận một vị trí. 
2. Duy trì một con trỏ trong các khoảng đã được sắp xếp và một heap tối thiểu được sắp xếp theo đúng điểm cuối. Heap đại diện cho tất cả các khoảng thời gian hiện đủ điều kiện để gán. 
3. Quét một biến`x`đại diện cho chỉ mục cửa hàng tiếp theo mà chúng tôi đang cố gắng chỉ định. Thay vì lặp lại tất cả các số nguyên lên đến$10^9$, chúng tôi chỉ di chuyển`x`khi có ít nhất một khoảng thời gian hoạt động. Nếu không có khoảng thời gian hoạt động, chúng tôi sẽ nhảy`x`đến điểm cuối bên trái của khoảng tiếp theo. 
4. Trước khi phân công`x`, chèn vào heap tất cả các khoảng với$l_i \le x$. Đây là những khoảng thời gian có thể sử dụng vị trí này. 
5. Nếu heap trống sau khi kích hoạt, chúng ta sẽ nhảy`x`chuyển tiếp lần nữa vì không có khoảng thời gian nào có thể sử dụng vị trí này. Điều này đảm bảo chúng tôi chỉ xử lý các vị trí có ý nghĩa. 
6. Lấy khoảng có số nhỏ nhất$r_i$từ đống. Khoảng thời gian này là cấp bách nhất vì nó có ràng buộc chặt chẽ nhất đối với các nhiệm vụ trong tương lai. 
7. Nếu$r_i < x$, khoảng thời gian này không thể chỉ định bất kỳ cửa hàng hợp lệ nào nữa nên câu trả lời là không thể. Ngược lại, chỉ định$x$vào nó và loại bỏ nó khỏi heap. 
8. Tăng`x`và lặp lại cho đến khi tất cả các khoảng được ấn định hoặc xảy ra mâu thuẫn. 

Lựa chọn thứ tự ở bước 6 là quyết định quan trọng: luôn đáp ứng khoảng thời gian kết thúc sớm nhất sẽ ngăn cản việc cản trở tính khả thi sau này. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, tất cả các khoảng thời gian hoạt động đều có$l_i \le x$, vì vậy họ có đủ điều kiện để nhận được vị trí$x$hoặc muộn hơn. Trong số đó, nếu chúng ta chọn một khoảng có điểm cuối bên phải lớn hơn trong khi bỏ qua điểm cuối bên phải nhỏ hơn, thì chúng ta có nguy cơ trì hoãn ràng buộc chặt chẽ hơn và mất đi tính khả thi sau này. Bất kỳ sự phân công khả thi nào cũng có thể được chuyển đổi sao cho tại mỗi vị trí$x$, khoảng được chỉ định là khoảng có giá trị tối thiểu$r_i$trong số những thứ vẫn có sẵn, bởi vì việc hoán đổi các bài tập giữa khoảng thời gian lỏng lẻo và chặt chẽ không bao giờ phá vỡ tính hợp lệ mà chỉ có thể cải thiện tình trạng chùng xuống. Đối số trao đổi này đảm bảo sự lựa chọn tham lam sẽ duy trì tính khả thi bất cứ khi nào có giải pháp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
import heapq

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        intervals = [tuple(map(int, input().split())) for _ in range(n)]
        intervals.sort()

        heap = []
        i = 0
        x = 1
        ok = True

        while i < n or heap:
            if not heap and i < n and x < intervals[i][0]:
                x = intervals[i][0]

            while i < n and intervals[i][0] <= x:
                heapq.heappush(heap, intervals[i][1])
                i += 1

            if not heap:
                continue

            r = heapq.heappop(heap)
            if r < x:
                ok = False
                break

            x += 1

        print("Yes" if ok else "No")

if __name__ == "__main__":
    solve()
```Cấu trúc cốt lõi là một đường quét qua các nhiệm vụ có thể thực hiện được. Mảng được sắp xếp đảm bảo các khoảng thời gian được đưa vào đúng thời điểm. Heap chỉ lưu trữ các điểm cuối bên phải vì các điểm cuối bên trái đã được đáp ứng khi chèn vào. Biến`x`chỉ tiến lên khi chúng ta gán thành công hoặc khi chúng ta phải nhảy qua các vùng trống. 

Một điểm tinh tế là bước nhảy khi không có khoảng thời gian hoạt động. Nếu không có nó, thuật toán có thể lặp lại một cách không cần thiết qua những khoảng trống lớn lên đến$10^9$, gây ra thời gian chờ. Bước nhảy duy trì tính chính xác vì không có khoảng thời gian nào có thể được chỉ định trong khoảng cách đó. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3
1 2
2 3
3 3
```| Bước | x | Khoảng hoạt động (giá trị r) | Được chọn r | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | [2] | 2 | giao | 
| 2 | 2 | [3,3] | 3 | giao | 
| 3 | 3 | [3] | 3 | giao | 

Tất cả các khoảng được khớp chính xác một lần, vì vậy nhiệm vụ thành công. 

Dấu vết này cho thấy heap luôn chọn ràng buộc chặt chẽ nhất như thế nào, nhưng tính khả thi vẫn được duy trì vì mỗi khoảng có đủ độ trễ. 

### Mẫu 2 

đầu vào:```
4
1 2
2 3
3 3
1 4
```| Bước | x | Khoảng hoạt động (giá trị r) | Được chọn r | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | [2,4] | 2 | giao | 
| 2 | 2 | [3,4] | 3 | giao | 
| 3 | 3 | [3,4] | 3 | giao | 
| 4 | 4 | [4] | 4 | giao | 

Tất cả các khoảng có thể được chỉ định vị trí riêng biệt. 

Điều này chứng tỏ rằng mặc dù khoảng thời gian dài$[1,4]$tồn tại, nó không ảnh hưởng đến tính khả thi vì các khoảng thời gian chặt chẽ hơn được ưu tiên trước tiên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N \log N)$mỗi trường hợp thử nghiệm | sắp xếp cộng với mỗi khoảng thời gian được đẩy và xuất hiện một lần từ heap | 
| Không gian |$O(N)$| đống và lưu trữ trong khoảng thời gian | 

Tổng cộng$N$trên các trường hợp thử nghiệm được giới hạn bởi$2 \cdot 10^5$, do đó giải pháp chạy thoải mái trong giới hạn. Hệ số logarit từ các phép toán heap vẫn đủ nhỏ đối với Python. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import contextlib

    out = io.StringIO()
    with contextlib.redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# sample 1
assert run("""2
3
1 2
2 3
3 3
5
1 2
2 3
3 3
1 3
999999999 1000000000
""") == """Yes
No"""

# sample 2
assert run("""1
4
1 2
2 3
3 3
1 4
""") == "Yes"

# minimum case
assert run("""1
1
5 5
""") == "Yes"

# impossible small overlap
assert run("""1
2
1 1
1 1
""") == "No"

# disjoint intervals
assert run("""1
3
1 1
3 3
2 2
""") == "Yes"

# large gap test
assert run("""1
3
100 100
1 1
200 200
""") == "Yes"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| khoảng điểm đơn | Có | độ đúng tối thiểu | 
| khoảng thời gian chặt chẽ giống hệt nhau | Không | phát hiện va chạm | 
| xáo trộn thứ tự rời rạc | Có | đặt hàng mạnh mẽ | 
| khoảng trống lớn thưa thớt | Có | tính đúng đắn qua các bước nhảy | 

## Vỏ cạnh 

Trường hợp cạnh đầu tiên là khi tất cả các khoảng thu gọn thành một điểm. Đối với đầu vào$[1,1], [1,1]$, heap chỉ chứa một phép gán hợp lệ duy nhất tại$x=1$và khoảng thứ hai thất bại ngay lập tức vì không có sẵn vị trí thứ hai. 

Một trường hợp khác là khi các khoảng cách đều nhau. Vì$[1,1], [100,100], [200,200]$, thuật toán nhảy liên tục`x`đến điểm cuối bên trái có sẵn tiếp theo. Mỗi khoảng thời gian được tách biệt, do đó khoảng trống trong đống sẽ kích hoạt việc bỏ qua chính xác mà không lãng phí các lần lặp lại. 

Trường hợp cuối cùng là sự chồng chéo nặng nề như$[1,10]$lặp lại nhiều lần. Vùng heap tăng dần theo tất cả các khoảng và thuật toán gán các số nguyên liên tiếp bắt đầu từ 1. Nếu số khoảng vượt quá độ dài liên kết của các phạm vi thì vùng heap cuối cùng sẽ tạo ra một khoảng với$r < x$, gây ra sự từ chối chính xác khi hết dung lượng.
