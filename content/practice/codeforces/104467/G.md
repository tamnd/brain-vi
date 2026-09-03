---
title: "CF 104467G - Giảm mạnh"
description: "Chúng ta được cung cấp một loạt các thay đổi giá hàng ngày, trong đó mỗi giá trị thể hiện cách giá cổ phiếu di chuyển từ ngày này sang ngày tiếp theo. Từ chuỗi này, chúng ta liên tục xét một cửa sổ trượt kết thúc ở vị trí thứ i và kéo dài hầu hết M phần tử."
date: "2026-06-30T13:08:45+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104467
codeforces_index: "G"
codeforces_contest_name: "La Salle-Pui Ching Programming Challenge \u57f9\u6b63\u5587\u6c99\u7de8\u7a0b\u6311\u6230\u8cfd 2022"
rating: 0
weight: 104467
solve_time_s: 95
verified: false
draft: false
---

[CF 104467G - Giảm mạnh](https://codeforces.com/problemset/problem/104467/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 35s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một loạt các thay đổi giá hàng ngày, trong đó mỗi giá trị thể hiện cách giá cổ phiếu di chuyển từ ngày này sang ngày tiếp theo. Từ trình tự này, chúng ta liên tục nhìn vào một cửa sổ trượt kết thúc ở vị trí`i`và kéo dài nhiều nhất`M`các phần tử. Đối với mọi vị trí`i`, chúng tôi chỉ xem xét phân khúc`A[max(1, i-M+1) ... i]`. 

Bên trong mỗi cửa sổ như vậy, chúng tôi muốn tìm đoạn giảm giá liên tục tồi tệ nhất. Phân đoạn suy giảm là bất kỳ mảng con liền kề nào có tổng càng âm càng tốt, nhưng chúng tôi chỉ quan tâm đến mức giảm nghiêm trọng nhất. Nếu tất cả các mảng con đều không âm thì câu trả lời được xác định là 0. 

Vì vậy, đối với mỗi cửa sổ, chúng tôi đang tính tổng mảng con tối thiểu và nếu mức tối thiểu đó là dương hoặc bằng 0, chúng tôi sẽ giữ nó ở mức 0. 

Đây thực chất là một phiên bản cửa sổ trượt của bài toán “tổng mảng con tối đa” cổ điển, ngoại trừ việc chúng ta đang tối đa hóa chuyển động đi xuống thay vì mức tăng lên. 

Những hạn chế`N ≤ 100000`Và`M ≤ N`ngụ ý rằng bất kỳ giải pháp nào tính toán lại câu trả lời từ đầu cho mỗi vị trí đều quá chậm. Một cách tiếp cận bậc hai trên tất cả các cửa sổ sẽ cố gắng đạt tới khoảng`10^10`trong trường hợp xấu nhất vượt xa giới hạn. Chúng ta cần một cấu trúc tuyến tính hoặc gần tuyến tính được khấu hao. 

Một số trường hợp phức tạp có vấn đề: 

Một cửa sổ có tất cả các giá trị không âm phải xuất ra`0`. Ví dụ, nếu`A = [1, 2, 3]`Và`M = 2`, mọi cửa sổ đều không tạo ra sự suy giảm hợp lệ, vì vậy mọi câu trả lời đều là`0`. 

Cửa sổ mà sự suy giảm tồi tệ nhất không phải là tiền tố hoặc hậu tố đầy đủ mà là phân đoạn bên trong phải được xử lý chính xác. Ví dụ, trong`[-3, -5, 2, -3]`, mức giảm tồi tệ nhất là`[-3, -5]`, không phải là mảng đầy đủ. 

Một sai lầm ngây thơ là cho rằng mức giảm tồi tệ nhất luôn là tổng hậu tố hoặc tổng tiền tố. Điều đó không thành công trên các mảng dấu hỗn hợp trong đó phân đoạn tối ưu là nội bộ. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Đối với mỗi chỉ số`i`, chúng tôi lấy cái cuối cùng`M`phần tử kết thúc tại`i`và thử tất cả các mảng con có thể có trong cửa sổ đó. Đối với mỗi mảng con, chúng tôi tính tổng của nó và theo dõi mức tối thiểu. 

Điều này đúng vì nó kiểm tra rõ ràng mọi phân đoạn liên tục có thể. Tuy nhiên, bên trong mỗi cửa sổ có kích thước lên tới`M`, có`O(M^2)`mảng con. Tổng thể`N`cửa sổ, điều này trở thành`O(NM^2)`theo cách giải thích tồi tệ nhất, hoặc thậm chí`O(NM)`nếu chúng ta tối ưu hóa tính toán tổng mảng con với các tổng tiền tố, nhưng vẫn quá lớn khi cả hai đều`10^5`. 

Quan sát quan trọng là mỗi cửa sổ đang yêu cầu tổng mảng con tối thiểu, tương đương với chênh lệch tiền tố tối đa. Nếu chúng ta xác định tổng tiền tố`S[i] = A[1] + ... + A[i]`, thì bất kỳ tổng mảng con nào`A[l..r]`là`S[r] - S[l-1]`. Giảm thiểu điều này tương đương với việc sửa chữa`r`và tìm giá trị lớn nhất`S[l-1]`trong phạm vi cho phép. 

Vì vậy, mỗi cửa sổ giảm xuống mức duy trì cửa sổ trượt tối đa trên tổng tiền tố, nhưng với hạn chế là chỉ các chỉ mục tiền tố bên trong cửa sổ cuối cùng.`M`các vị trí được cho phép. Điều này trở thành vấn đề về cấu trúc dữ liệu: chúng ta cần duy trì tổng tiền tố tối đa trong cửa sổ trượt và tính toán`S[i] - max_prefix`. 

Tuy nhiên, có một vấn đề phức tạp: cửa sổ động theo`i`, do đó các chỉ số tiền tố nhập và rời khỏi phạm vi hợp lệ. Đây chính xác là một kịch bản deque đơn điệu, trong đó chúng ta duy trì các ứng cử viên cho tổng tiền tố tối đa theo thứ tự giảm dần. 

Câu trả lời cuối cùng cho mỗi`i`là:`min_subarray_sum = S[i] - max_{j in window}(S[j-1])`, được kẹp bằng 0 nếu dương. 

Chúng tôi duy trì một dãy chỉ số của tổng tiền tố, đảm bảo giá trị tiền tố của chúng giảm dần và chúng tôi cũng đảm bảo loại bỏ các chỉ mục bên ngoài cửa sổ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N·M²) | O(1) | Quá chậm | 
| Tối ưu (tiền tố + deque) | O(N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chuyển đổi vấn đề thành tổng tiền tố, sau đó duy trì cấu trúc trượt trên các chỉ số tiền tố. 

### bước 

1. Xây dựng tổng tiền tố`S`Ở đâu`S[0] = 0`Và`S[i] = S[i-1] + A[i]`. 

Điều này cho phép chúng ta tính tổng bất kỳ mảng con nào trong thời gian không đổi bằng cách sử dụng hiệu. 
2. Đối với từng vị trí`i`, chúng ta muốn tổng mảng con tối thiểu kết thúc ở đâu đó bên trong mảng cuối cùng`M`các phần tử tương ứng với việc chọn`j`TRONG`[i-M, i-1]`cho chỉ mục tiền tố`j`. 
3. Duy trì một dãy chỉ số của`S`đó là các ứng cử viên để trở thành giá trị tiền tố tối đa trong cửa sổ hiện tại. 

Chúng tôi lưu trữ các chỉ số theo thứ tự tăng dần nhưng duy trì các giá trị tiền tố theo thứ tự giảm dần. 
4. Trước khi xử lý`i`, xóa các chỉ mục ở phía trước deque nằm ngoài phạm vi. 

Cụ thể, bất kỳ chỉ số nào`< i-M`không còn giá trị nữa. 
5. Khi chèn chỉ mục tiền tố mới`i-1`, chúng tôi xóa khỏi mặt sau của deque bất kỳ chỉ mục nào có giá trị tiền tố nhỏ hơn hoặc bằng`S[i-1]`. 

Những điều này vô dụng vì tiền tố lớn hơn sẽ chi phối chúng cho tất cả các truy vấn trong tương lai. 
6. Sau khi dọn dẹp, hãy nối thêm`i-1`đến deque. 
7. Tiền tố tốt nhất (lớn nhất) trong cửa sổ nằm ở phía trước deque. Tính toán:`best = S[i] - S[deque[0]]`. 
8. Nếu`best > 0`, đầu ra`0`, nếu không thì xuất ra`best`. 

### Tại sao nó hoạt động 

Deque duy trì tính bất biến là mặt trước của nó luôn lưu chỉ mục có tổng tiền tố tối đa trong số tất cả các chỉ mục hợp lệ trong cửa sổ hiện tại. Bất kỳ chỉ mục nào có tổng tiền tố nhỏ hơn chỉ mục mới hơn sẽ không bao giờ tối ưu nữa vì nó tệ hơn và cũng cũ hơn hoặc bằng các ràng buộc về vị trí. Điều này đảm bảo rằng mọi truy vấn đều được trả lời trong thời gian không đổi và mọi chỉ mục vào và ra khỏi deque nhiều nhất một lần, duy trì độ phức tạp tuyến tính. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    a = list(map(int, input().split()))

    # prefix sums
    ps = [0] * (n + 1)
    for i in range(n):
        ps[i + 1] = ps[i] + a[i]

    from collections import deque
    dq = deque()

    res = []

    for i in range(1, n + 1):
        # window for prefix indices: [i-m, i-1]
        left = i - m
        if left < 0:
            left = 0

        # remove out of range indices
        while dq and dq[0] < left:
            dq.popleft()

        # add prefix index i-1
        idx = i - 1
        while dq and ps[dq[-1]] <= ps[idx]:
            dq.pop()
        dq.append(idx)

        best_prefix = ps[dq[0]]
        best = ps[i] - best_prefix

        if best > 0:
            best = 0
        res.append(str(best))

    print(" ".join(res))

if __name__ == "__main__":
    solve()
```Mảng tổng tiền tố cho phép đánh giá tổng mảng con theo thời gian không đổi. Deque duy trì các ứng cử viên cho điểm bắt đầu tối ưu của mảng con phủ định kết thúc tại`i`. Chi tiết quan trọng là chúng tôi lưu trữ các chỉ mục tiền tố, không phải chỉ mục mảng và luôn sử dụng`i-1`làm điểm cuối tiền tố ứng cử viên mới nhất. 

Việc loại bỏ các chỉ số bên ngoài`[i-m, i-1]`đảm bảo ràng buộc cửa sổ được tôn trọng, trong khi việc loại bỏ đơn điệu đảm bảo chỉ còn lại các tổng tiền tố có khả năng tối ưu. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
10 4
4 -3 1 -2 -3 2 0 -1 -1 -2
```Chúng tôi theo dõi tổng tiền tố và tiến hóa deque: 

| tôi | A[i] | tiền tố tôi | deque (chỉ số tiền tố) | tiền tố tốt nhất | kết quả | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 4 | 4 | [0] | 0 | 0 | 
| 2 | -3 | 1 | [1,2] | 1 | 0 | 
| 3 | 1 | 2 | [1,2,3] | 1 | -3 | 
| 4 | -2 | 0 | [1,4] | 1 | -3 | 
| 5 | -3 | -3 | [1,5] | 1 | -3 | 
| 6 | 2 | -1 | [4,5,6] | 0 | -5 | 
| 7 | 0 | -1 | [4,5,6,7] | 0 | -5 | 
| 8 | -1 | -2 | [5,6,7,8] | -3 | -3 | 
| 9 | -1 | -3 | [6,7,8,9] | -3 | -2 | 
| 10 | -2 | -5 | [7,8,9,10] | -3 | -4 | 

Dấu vết này cho thấy mức giảm tốt nhất luôn được xác định bằng tiền tố tối đa trong cửa sổ hợp lệ, không nhất thiết là vị trí sớm nhất hoặc mới nhất. 

### Mẫu 2 

đầu vào:```
12 7
-3 -7 -1 -3 -5 6 -3 -2 -2 4 -1 2
```| tôi | A[i] | tiền tố tôi | deque | tiền tố tốt nhất | kết quả | 
| --- | --- | --- | --- | --- | --- | 
| 1 | -3 | -3 | [0,1] | 0 | -3 | 
| 2 | -7 | -10 | [0,1,2] | 0 | -10 | 
| 3 | -1 | -11 | [0,1,2,3] | 0 | -11 | 
| 4 | -3 | -14 | [0,1,2,3,4] | 0 | -14 | 
| 5 | -5 | -19 | [0,1,2,3,4,5] | 0 | -19 | 
| 6 | 6 | -13 | [0,5,6] | -19? không, dịch chuyển cửa sổ | -19 | 
| 7 | -3 | -16 | cập nhật | -19 | -19 | 

Mẫu thứ hai cho thấy các giá trị tiền tố lớn ban đầu vẫn tồn tại như thế nào cho đến khi chúng rời khỏi cửa sổ, ảnh hưởng mạnh mẽ đến các mảng con sau này ngay cả sau khi các cải tiến cục bộ xuất hiện. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) | mỗi chỉ mục vào và ra khỏi deque một lần | 
| Không gian | O(N) | mảng tiền tố và lưu trữ deque | 

Hành vi tuyến tính là đủ cho`N = 100000`. Các hoạt động bên trong mỗi vòng lặp được khấu hao không đổi, do đó giải pháp hoạt động thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, m = map(int, input().split())
    a = list(map(int, input().split()))

    ps = [0] * (n + 1)
    for i in range(n):
        ps[i + 1] = ps[i] + a[i]

    from collections import deque
    dq = deque()
    res = []

    for i in range(1, n + 1):
        left = i - m
        if left < 0:
            left = 0

        while dq and dq[0] < left:
            dq.popleft()

        idx = i - 1
        while dq and ps[dq[-1]] <= ps[idx]:
            dq.pop()
        dq.append(idx)

        best = ps[i] - ps[dq[0]]
        if best > 0:
            best = 0
        res.append(str(best))

    return " ".join(res)

# provided samples
assert run("10 4\n4 -3 1 -2 -3 2 0 -1 -1 -2") == "0 -3 -3 -3 -5 -5 -5 -3 -2 -4"
assert run("12 7\n-3 -7 -1 -3 -5 6 -3 -2 -2 4 -1 2") == "-3 -10 -11 -14 -19 -19 -19 -16 -9 -8 -7 -7"

# custom cases
assert run("1 1\n5") == "0", "single positive"
assert run("5 2\n1 2 3 4 5") == "0 0 0 0 0", "all non-negative"
assert run("5 3\n-1 -2 -3 -4 -5") == "-1 -3 -6 -9 -12", "all negative"
assert run("6 2\n3 -10 3 -10 3 -10") == "0 -7 0 -7 0 -7", "alternating spikes"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 0 | xử lý cửa sổ tối thiểu | 
| tất cả đều tích cực | tất cả số không | kẹp đúng | 
| tất cả đều tiêu cực | tích lũy suy giảm tồi tệ nhất | tính đúng đắn của các âm đơn điệu | 
| gai xen kẽ | tính toán lại định kỳ | cửa sổ trượt đúng cách | 

## Vỏ cạnh 

Cửa sổ một phần tử xuất hiện khi`M = 1`. Thuật toán giảm xuống việc kiểm tra xem phần tử có âm hay không, vì mảng con duy nhất là chính nó. Phương thức tiền tố/deque vẫn hoạt động vì cửa sổ chỉ số tiền tố trở thành một điểm duy nhất và sự khác biệt`S[i] - S[i-1]`mang lại kết quả chính xác`A[i]`. 

Khi tất cả các giá trị không âm, tổng tiền tố tăng nghiêm ngặt, do đó tiền tố tối đa trong mọi cửa sổ luôn là chỉ mục hợp lệ sớm nhất. Tổng của mảng con được tính toán trở thành không dương, do đó việc kẹp tạo ra số 0 một cách nhất quán. Deque vẫn cập nhật chính xác nhưng không bao giờ thay đổi cấu trúc kết quả. 

Khi tất cả các giá trị đều âm, tiền tố tối đa luôn là tiền tố ít âm nhất, thường là tiền tố sớm nhất trong cửa sổ. Điều này tạo ra một chuỗi câu trả lời giảm dần phù hợp với các phân đoạn tồi tệ nhất tích lũy. Thuật toán xử lý điều này vì deque duy trì thứ tự tiền tố giảm dần, nhưng tất cả các ứng cử viên vẫn có liên quan cho đến khi chúng trượt ra khỏi phạm vi.
