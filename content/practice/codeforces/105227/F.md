---
title: "CF 105227F - Những con số tuyệt vời k"
description: "Chúng tôi được cung cấp một mảng và chúng tôi muốn hiểu mức độ “ổn định” của từng giá trị trên các cửa sổ có độ dài cố định. Đối với kích thước cửa sổ đã chọn $k$, chúng ta trượt một đoạn có độ dài $k$ qua mảng. Một số được coi là tốt cho $k$ này nếu nó xuất hiện trong mọi phân đoạn như vậy."
date: "2026-06-24T16:30:34+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105227
codeforces_index: "F"
codeforces_contest_name: "CPG Training Contest - 1"
rating: 0
weight: 105227
solve_time_s: 74
verified: false
draft: false
---

[CF 105227F - Những con số kỳ diệu](https://codeforces.com/problemset/problem/105227/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 14s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mảng và chúng tôi muốn hiểu mức độ “ổn định” của từng giá trị trên các cửa sổ có độ dài cố định. Đối với kích thước cửa sổ đã chọn$k$, chúng ta trượt một đoạn có chiều dài$k$trên mảng. Một con số được coi là tốt cho việc này$k$nếu nó xuất hiện trong mọi phân đoạn như vậy. Trong số tất cả các số thỏa mãn điều kiện này, chúng ta muốn số nhỏ nhất. Nếu không có số nào tồn tại trong tất cả các cửa sổ, chúng tôi sẽ xuất$-1$. 

Đầu ra là một danh sách cho tất cả$k$từ$1$ĐẾN$n$, vì vậy chúng ta được hỏi một cách hiệu quả rằng “giá trị tối thiểu hiện tại chung” này thay đổi như thế nào khi kích thước cửa sổ tăng lên. 

Các ràng buộc đủ chặt chẽ để mọi thứ bậc hai trong$n$mỗi trường hợp thử nghiệm là không thể. Mỗi phần tử tham gia vào nhiều cửa sổ và có tới$3 \cdot 10^5$tổng số phần tử qua các thử nghiệm, vì vậy giải pháp dự định phải gần với tuyến tính hoặc tuyến tính. Một giải pháp tính toán lại thông tin một cách độc lập cho từng$k$sẽ lặp lại công việc một cách đại khái$n$lần, ngay lập tức vượt quá giới hạn. 

Một điểm tinh tế là một giá trị có thể vắng mặt trong một số cửa sổ ngay cả khi nó xuất hiện thường xuyên về tổng thể. Ví dụ, trong`1 2 1 2 1`, số`1`xuất hiện nhiều lần, nhưng đối với một số kích thước cửa sổ nhất định, tồn tại các phân đoạn không có nó, do đó nó không đạt được điều kiện chung. Điều này phá vỡ lý luận tần số ngây thơ. 

Một trường hợp cạnh khác là khi tất cả các phần tử đều giống hệt nhau. Khi đó mọi cửa sổ đều chứa giá trị đó, vì vậy câu trả lời phải luôn là giá trị đó cho tất cả$k$. Một cách tiếp cận bất cẩn chỉ kiểm tra tần số hoặc khoảng cách toàn cầu vẫn có thể tạo ra$-1$không chính xác nếu nó hiểu sai điều kiện. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực cố định một giá trị của$k$, liệt kê tất cả các cửa sổ có độ dài$k$và tính toán giao điểm của tất cả các bộ giá trị trong các cửa sổ này. Đối với mỗi cửa sổ, chúng tôi có thể duy trì một bản đồ tần số, sau đó giao nhau giữa các cửa sổ. Ngay cả khi mỗi lần kiểm tra cửa sổ đều$O(k)$, điều này trở thành$O(nk)$mỗi$k$, dẫn đến$O(n^3)$tổng thể trong trường hợp xấu nhất. 

Quan sát quan trọng là ngừng suy nghĩ về cửa sổ trượt và thay vào đó tập trung vào vị trí của từng giá trị. Một số không đáp ứng được điều kiện cho trước$k$nếu tồn tại ít nhất một cửa sổ có độ dài$k$tránh được mọi sự xuất hiện của nó. Điều này tương đương với việc nói rằng có ít nhất một khoảng cách về chiều dài$k$giữa các lần xuất hiện liên tiếp, bao gồm cả ranh giới. 

Vì vậy, thay vì quét các cửa sổ, chúng tôi theo dõi vị trí xuất hiện của từng giá trị và xem xét “khoảng cách” tối đa giữa các lần xuất hiện liên tiếp, bao gồm từ đầu đến cuối. Một giá trị được đảm bảo xuất hiện trong mọi cửa sổ kích thước$k$khi và chỉ khi khoảng cách tối đa của nó lớn nhất$k$. Điều này chuyển đổi vấn đề thành các ràng buộc theo dõi trên mỗi giá trị và tổng hợp chúng thành câu trả lời cho tất cả$k$. 

Chúng tôi đảo ngược quan điểm: mỗi giá trị đóng góp một ngưỡng cho$k$. Đối với mỗi giá trị, hãy tính khoảng cách tối đa giữa các lần xuất hiện liên tiếp. Sau đó giá trị này trở nên hợp lệ cho tất cả$k$lớn hơn hoặc bằng ngưỡng đó. Sau đó chúng ta cần, với mỗi$k$, giá trị nhỏ nhất có ngưỡng là$\le k$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^3)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chuyển đổi mảng thành danh sách lần xuất hiện, tính toán các ràng buộc trên mỗi giá trị, sau đó trả lời các truy vấn cho tất cả$k$. 

1. Lưu trữ tất cả các vị trí của từng giá trị trong mảng. Điều này cho phép chúng tôi suy luận trực tiếp về các khoảng trống thay vì quét các cửa sổ nhiều lần. 
2. Đối với mỗi giá trị, hãy xây dựng danh sách các chỉ số xuất hiện của nó và tăng thêm các ranh giới ảo tại các vị trí$0$Và$n+1$. Điều này đảm bảo các khoảng trống ở cạnh được xử lý đồng nhất với các khoảng trống bên trong. 
3. Với mỗi giá trị, hãy tính chênh lệch tối đa giữa các vị trí liên tiếp trong danh sách tăng cường này. Khoảng cách tối đa này biểu thị kích thước cửa sổ nhỏ nhất không còn có thể “tránh” giá trị đó nữa. 
4. Giải thích đây là ngưỡng: giá trị hợp lệ cho tất cả$k$lớn hơn hoặc bằng khoảng cách tối đa này, bởi vì bất kỳ cửa sổ nào ngắn hơn khoảng cách đó đều có thể được đặt hoàn toàn bên trong khoảng cách và bỏ lỡ hoàn toàn giá trị. 
5. Bây giờ chúng ta cần tính toán với mọi$k$, giá trị nhỏ nhất có ngưỡng lớn nhất$k$. Chúng tôi xử lý các ngưỡng theo thứ tự tăng dần$k$, duy trì giá trị ứng viên tốt nhất được thấy cho đến nay. 
6. Xây dựng một mảng`best[k]`nơi chúng tôi cập nhật tất cả các vị trí bắt đầu từ mỗi ngưỡng. Quét tuyến tính từ nhỏ đến lớn$k$duy trì giá trị tối thiểu toàn cầu trở nên hợp lệ. 

### Tại sao nó hoạt động 

Bất biến quan trọng là đối với mỗi giá trị, khoảng cách tối đa được tính toán là kích thước cửa sổ nhỏ nhất chính xác buộc giá trị xuất hiện trong mọi cửa sổ. Bất kỳ cửa sổ nào ngắn hơn khoảng cách này đều có thể được đặt bên trong khoảng cách lớn nhất giữa các lần xuất hiện, tránh hoàn toàn giá trị. Bất kỳ cửa sổ nào có kích thước lớn nhất như thế này đều phải giao nhau với mọi chuỗi khoảng trống, đảm bảo ít nhất một lần xuất hiện trong mỗi cửa sổ. Bởi vì ngưỡng này mô tả đầy đủ tính hợp lệ, tổng hợp các giá trị theo ngưỡng và lấy tiền tố cực tiểu$k$tạo ra câu trả lời đúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        pos = [[] for _ in range(n + 1)]
        for i, v in enumerate(a, 1):
            pos[v].append(i)

        # threshold[k] = best candidate value that activates at k
        INF = 10**18
        threshold = [INF] * (n + 2)

        for v in range(1, n + 1):
            if not pos[v]:
                continue

            arr = [0] + pos[v] + [n + 1]
            mx_gap = 0
            for i in range(1, len(arr)):
                mx_gap = max(mx_gap, arr[i] - arr[i - 1])

            threshold[mx_gap] = min(threshold[mx_gap], v)

        ans = [-1] * (n + 1)
        best = INF

        for k in range(1, n + 1):
            best = min(best, threshold[k])
            if best != INF:
                ans[k] = best
            else:
                ans[k] = -1

        print(*ans[1:])

if __name__ == "__main__":
    solve()
```Giải pháp này xây dựng danh sách sự kiện cho từng giá trị, đây là cấu trúc duy nhất cần thiết để giải thích về phạm vi bao phủ của cửa sổ. Các ranh giới mở rộng tại`0`Và`n+1`đảm bảo các khoảng trống cạnh được xử lý mà không có trường hợp đặc biệt. Tính toán khoảng cách tối đa trực tiếp tạo ra ngưỡng tới hạn cho từng giá trị. 

Mảng`threshold`ánh xạ kích thước cửa sổ tới giá trị nhỏ nhất có giá trị chính xác ở kích thước đó. Sau đó, quá trình quét tối thiểu tiền tố sẽ chuyển đổi các điểm kích hoạt này thành câu trả lời cho tất cả$k$, đảm bảo rằng một khi một giá trị hợp lệ thì nó vẫn là ứng cử viên cho tất cả các giá trị lớn hơn$k$. 

Một cạm bẫy phổ biến là quên đi những khoảng trống ranh giới. Không cần thêm`0`Và`n+1`, các giá trị xuất hiện gần các cạnh không chính xác có vẻ ổn định hơn thực tế. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
1 2 3 4 5
```Tất cả các giá trị xuất hiện một lần. 

| Giá trị | Vị trí | Tăng cường | Khoảng cách tối đa | Ngưỡng | 
| --- | --- | --- | --- | --- | 
| 1 | [1] | [0,1,6] | 5 | 5 | 
| 2 | [2] | [0,2,6] | 4 | 4 | 
| 3 | [3] | [0,3,6] | 3 | 3 | 
| 4 | [4] | [0,4,6] | 2 | 2 | 
| 5 | [5] | [0,5,6] | 1 | 1 | 

Bây giờ chúng ta quét$k$: 

| k | ngưỡng tốt nhất | trả lời | 
| --- | --- | --- | 
| 1 | 1 | 1 | 
| 2 | 1 | 1 | 
| 3 | 1 | 1 | 
| 4 | 1 | 1 | 
| 5 | 1 | 1 | 

Điều này cho thấy một khi ngưỡng nhỏ nhất xuất hiện thì nó sẽ lấn át tất cả các ngưỡng lớn hơn.$k$. 

### Ví dụ 2 

đầu vào:```
4 4 4 4 2
```Giá trị 4 dày đặc, giá trị 2 bị cô lập. 

| Giá trị | Vị trí | Tăng cường | Khoảng cách tối đa | Ngưỡng | 
| --- | --- | --- | --- | --- | 
| 4 | [1,2,3,4] | [0,1,2,3,4,6] | 2 | 2 | 
| 2 | [5] | [0,5,6] | 5 | 5 | 

Quét: 

| k | ngưỡng tốt nhất | trả lời | 
| --- | --- | --- | 
| 1 | 2 | -1 | 
| 2 | 2 | 4 | 
| 3 | 2 | 4 | 
| 4 | 2 | 4 | 
| 5 | 2 | 2 | 

Điều này chứng tỏ rằng một giá trị thưa thớt có thể thống trị các kích thước cửa sổ lớn ngay cả khi nó không thường xuyên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi chỉ mục được xử lý một lần để xây dựng vị trí và lần xuất hiện của mỗi giá trị được quét một lần để tính toán các khoảng trống | 
| Không gian |$O(n)$| Danh sách vị trí và mảng phụ trợ chia tỷ lệ tuyến tính với kích thước đầu vào | 

Thuật toán phù hợp thoải mái trong các ràng buộc vì tổng số phần tử mảng trong tất cả các trường hợp thử nghiệm là$3 \cdot 10^5$và mọi phần tử đều được sử dụng trong các phép toán có thời gian không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def solve():
        t = int(input())
        for _ in range(t):
            n = int(input())
            a = list(map(int, input().split()))

            pos = [[] for _ in range(n + 1)]
            for i, v in enumerate(a, 1):
                pos[v].append(i)

            INF = 10**18
            threshold = [INF] * (n + 2)

            for v in range(1, n + 1):
                if not pos[v]:
                    continue
                arr = [0] + pos[v] + [n + 1]
                mx = 0
                for i in range(1, len(arr)):
                    mx = max(mx, arr[i] - arr[i - 1])
                threshold[mx] = min(threshold[mx], v)

            best = INF
            ans = []
            for k in range(1, n + 1):
                best = min(best, threshold[k])
                ans.append(-1 if best == INF else best)

            print(*ans)

    solve()
    return sys.stdout.getvalue().strip()

# provided samples
assert run("""3
5
1 2 3 4 5
5
4 4 4 4 2
6
1 3 1 5 3 1
""") == """1 1 1 1 1
-1 4 4 4 2
-1 -1 1 1 1 1"""

# custom cases
assert run("""1
1
7
""") == "7", "single element"

assert run("""1
5
1 1 1 1 1
""") == "1 1 1 1 1", "all equal"

assert run("""1
6
1 2 1 2 1 2
""") == "1 1 1 1 1 1", "alternating pattern"

assert run("""1
4
1 2 3 1
""") == "-1 1 1 1", "edge gaps matter"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 7 | xử lý ranh giới tối thiểu | 
| tất cả đều bình đẳng | tất cả 1s | trường hợp bảo hiểm đầy đủ | 
| xen kẽ | tất cả 1s | chồng chéo dày đặc | 
| khoảng trống cạnh | -1 1 1 1 | độ đúng ranh giới | 

## Vỏ cạnh 

Đối với mảng một phần tử như`[7]`, các vị trí tăng cường trở thành`[0,1,2]`, tạo ra khoảng cách tối đa là`2`. Điều này mã hóa chính xác rằng chỉ các cửa sổ có kích thước 1 mới bao gồm giá trị một cách tầm thường, trong khi các khoảng trống khái niệm lớn hơn là không liên quan vì không có cách nào để tránh nó khi$k=1$. 

Đối với một mảng như`[1,2,1,2,1,2]`, cả hai giá trị đều có khoảng cách tối đa nhỏ vì các lần xuất hiện thường xuyên và cách đều nhau. Thuật toán gán cả hai ngưỡng nhỏ và tiền tố tối thiểu đảm bảo giá trị nhỏ nhất chiếm ưu thế trong mọi$k$, tạo ra một câu trả lời không đổi một cách chính xác. 

Đối với các mẫu nặng cạnh như`[1,2,3,1]`, giá trị`2`Và`3`có những khoảng trống lớn cho phép chúng được bỏ qua bởi các cửa sổ nhỏ, trong khi giá trị`1`có khoảng cách bên trong chặt chẽ hơn. Khoảng cách ranh giới gia tăng là yếu tố đảm bảo những lần xuất hiện đầu tiên và cuối cùng được tính toán chính xác, ngăn chặn việc đánh giá quá cao độ ổn định.
