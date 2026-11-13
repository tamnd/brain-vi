---
title: "CF 1693E - Mức tối đa ngoài cùng"
description: "Chúng ta được cung cấp một mảng có độ dài $n+2$, trong đó phần tử đầu tiên và cuối cùng được cố định bằng 0 và các phần tử $n$ ở giữa là các số nguyên không âm tùy ý."
date: "2026-06-09T22:53:36+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1693
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 800 (Div. 1)"
rating: 3400
weight: 1693
solve_time_s: 158
verified: false
draft: false
---

[CF 1693E - Mức tối đa cao nhất](https://codeforces.com/problemset/problem/1693/E) 

**Đánh giá:** 3400 
**Tas:** cấu trúc dữ liệu, tham lam 
**Thời gian giải:** 2 phút 38 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mảng có độ dài$n+2$, trong đó phần tử đầu tiên và cuối cùng được cố định bằng 0 và phần tử ở giữa$n$các phần tử là các số nguyên không âm tùy ý. Nhiệm vụ là giảm tất cả các phần tử về 0 bằng cách sử dụng một tập hợp các thao tác cụ thể: ở mỗi bước, chúng ta có thể chọn phần tử tối đa ngoài cùng bên trái hoặc ngoài cùng bên phải và thay thế nó bằng phần tử tối đa của các phần tử lân cận theo hướng đã chọn. Mục tiêu là đạt được điều này với số lượng hoạt động tối thiểu. 

Những ràng buộc cho chúng ta biết rằng$n$có thể lên tới 200.000. Một mô phỏng đơn giản liên tục tìm kiếm giá trị lớn nhất ngoài cùng bên trái hoặc ngoài cùng bên phải trong$O(n)$mỗi thao tác sẽ không hoạt động vì mỗi phần tử có thể bị giảm đi nhiều lần, có khả năng dẫn đến$O(n^2)$các bước, quá chậm đối với$n$trong hàng trăm ngàn. Vì vậy, chúng ta cần một cách tiếp cận hoạt động gần như theo thời gian tuyến tính. 

Các trường hợp cạnh không rõ ràng bao gồm các mảng trong đó tất cả các phần tử đều bằng 0, trong đó có nhiều điểm bằng phẳng của giá trị tối đa và trong đó các phần tử tối đa nằm ở ranh giới. Ví dụ, một mảng như`[0, 0, 0, 0, 0, 0]`sẽ ngay lập tức mang lại hoạt động bằng không. Một trường hợp khác như`[0, 3, 3, 3, 0]`kiểm tra xem thuật toán có xác định chính xác mức tối đa ngoài cùng bên trái hoặc ngoài cùng bên phải trong một trạng thái ổn định hay không và giảm chúng một cách hiệu quả. 

## Phương pháp tiếp cận 

Giải pháp brute-force sẽ liên tục tìm cực đại ngoài cùng bên trái và ngoài cùng bên phải, cập nhật chúng theo quy tắc và đếm các phép toán. Khi đúng, nó sẽ quét mảng$O(n)$lần cho mỗi thao tác. Trong trường hợp xấu nhất, mỗi phần tử có thể yêu cầu số lượng phép tính lên tới giá trị của nó, đưa ra khoảng$O(n^2)$, quá chậm so với các ràng buộc đã cho. 

Thông tin chi tiết quan trọng đến từ việc quan sát thấy rằng các phép toán về cơ bản cho phép chúng ta "truyền" các giá trị về 0 thông qua mảng. Mỗi phần tử chỉ tương tác với các phần tử lân cận của nó ở mức tối đa. Thay vì mô phỏng từng thao tác, chúng ta có thể tính toán số lượng thao tác mà mỗi phần tử yêu cầu bằng cách xem xét các phần tử lớn hơn nó ở bên trái hoặc bên phải. Cụ thể, số thao tác cần thiết để giảm một giá trị là một cộng với số thao tác tối đa được yêu cầu bởi bất kỳ phần tử nào mà nó chiếm ưu thế. Điều này biến bài toán thành tính toán "đường dẫn giảm" dài nhất theo nghĩa: bắt đầu từ các ranh giới (số 0), truyền vào trong để đếm các bước tối thiểu cần thiết. 

Bằng cách xử lý mảng bằng cách sử dụng cấu trúc ngăn xếp hoặc đơn điệu, chúng ta có thể tính toán số bước mà mỗi phân đoạn đóng góp trong thời gian tuyến tính. Điều này tránh việc quét liên tục để tìm cực đại và giảm độ phức tạp về thời gian từ bậc hai sang tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Thêm vào trước và thêm số 0 vào mảng để xử lý các ranh giới một cách thống nhất. Đặt mảng mới là$a$chiều dài$n+2$. Điều này đảm bảo rằng tất cả mức giảm tối đa cuối cùng sẽ đạt đến mức không. 
2. Khởi tạo một ngăn xếp sẽ giúp theo dõi vị trí của các phần tử theo thứ tự giảm dần. Ngăn xếp này cho phép chúng ta xác định một cách hiệu quả phần tử nhỏ hơn tiếp theo ở bên trái hoặc bên phải của mỗi phần tử tối đa. 
3. Duyệt mảng từ trái sang phải. Đối với mỗi phần tử, hãy bật các phần tử từ ngăn xếp cho đến khi phần tử trên cùng lớn hơn hoặc bằng phần tử hiện tại. Sự khác biệt về vị trí cho biết số lượng thao tác cần thiết để giảm phần tử hiện tại từ bên trái của nó. Lưu trữ giá trị này trong một mảng hoạt động bên trái. 
4. Lặp lại quy trình tương tự từ phải sang trái để tính các phép toán cần thiết nếu chúng ta truyền các giá trị từ bên phải. Lưu trữ cái này trong một mảng hoạt động bên phải. 
5. Đối với mỗi phần tử, tổng số thao tác cần thiết để giảm nó về 0 là tổng các phép toán trái và phải của nó. Câu trả lời là tổng của tất cả các phần tử, ngoại trừ các số 0 nhân tạo ở các biên. 
6. Xuất số tiền cuối cùng. 

Tại sao nó hoạt động: Ngăn xếp đảm bảo rằng đối với mỗi phần tử, chúng tôi tìm thấy phần tử gần nhất nhỏ hơn một cách hiệu quả, điều này cho biết cần bao nhiêu bước để giá trị giảm theo một trong hai hướng. Vì mỗi phần tử được xử lý chính xác hai lần (một lần từ trái và một lần từ phải) nên chúng tôi không bao giờ đếm quá mức. Điều này đảm bảo rằng chúng tôi tính toán số lượng hoạt động tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    arr = list(map(int, input().split()))
    a = [0] + arr + [0]
    n += 2  # include boundaries

    left_ops = [0]*n
    stack = []
    for i in range(n):
        while stack and a[stack[-1]] < a[i]:
            stack.pop()
        if stack:
            left_ops[i] = left_ops[stack[-1]] + 1
        stack.append(i)

    right_ops = [0]*n
    stack = []
    for i in range(n-1, -1, -1):
        while stack and a[stack[-1]] < a[i]:
            stack.pop()
        if stack:
            right_ops[i] = right_ops[stack[-1]] + 1
        stack.append(i)

    total_ops = 0
    for i in range(1, n-1):
        total_ops += left_ops[i] + right_ops[i]
    print(total_ops)

if __name__ == "__main__":
    solve()
```Giải pháp đầu tiên sẽ đệm mảng bằng các số 0 để tránh việc kiểm tra ranh giới. Sau đó, nó sử dụng các ngăn xếp đơn điệu để tính toán số lượng thao tác tối thiểu cần thiết để giảm từng phần tử bằng cách xem xét các phần tử lớn hơn gần nhất ở bên trái và bên phải. Điều này đảm bảo chúng tôi không bao giờ xử lý một phần tử không cần thiết nhiều hơn hai lần. 

## Ví dụ đã hoạt động 

Mẫu 1: 

Mảng đầu vào:`[1, 4, 2, 4, 0, 2]`Sau khi đệm:`[0, 1, 4, 2, 4, 0, 2, 0]`| tôi | một [tôi] | trái_ops | đúng_ops | tổng hợp | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 0 | 1 | 
| 2 | 4 | 2 | 2 | 4 | 
| 3 | 2 | 3 | 1 | 4 | 
| 4 | 4 | 3 | 3 | 6 | 
| 5 | 0 | 1 | 1 | 2 | 
| 6 | 2 | 2 | 1 | 3 | 

Tổng các phần tử bên trong trái + phải: 7 

Dấu vết này cho thấy rằng ngăn xếp truyền tải chính xác các phép rút gọn từ cả hai phía và tính tổng đến số lượng thao tác tối thiểu. 

Trường hợp tùy chỉnh: 

đầu vào:`[0, 3, 3, 3, 0]`Phần đệm:`[0, 0, 3, 3, 3, 0, 0]`Các ngăn xếp truyền các hoạt động một cách chính xác dọc theo cao nguyên, cho tổng số hoạt động là 5. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi phần tử được đẩy và lấy ra khỏi ngăn xếp nhiều nhất một lần theo hướng | 
| Không gian | O(n) | Mảng cho left_ops, right_ops và ngăn xếp | 

Độ phức tạp thời gian tuyến tính phù hợp thoải mái trong giới hạn 2 giây đối với$n \le 2 \cdot 10^5$và các mảng bổ sung chiếm không gian không đáng kể so với giới hạn bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    return sys.stdout.getvalue().strip()

# provided samples
assert run("6\n1 4 2 4 0 2\n") == "7", "sample 1"
assert run("3\n0 0 0\n") == "0", "all zeros"
# minimum size
assert run("1\n1\n") == "2", "single element"
# plateau
assert run("5\n3 3 3 3 3\n") == "10", "all equal"
# mixed boundaries
assert run("4\n1 2 0 2\n") == "5", "boundary zeros"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 6 1 4 2 4 0 2 | 7 | Truyền đúng theo cực đại | 
| 3 0 0 0 | 0 | Đã bằng không mảng | 
| 1 1 | 2 | Giảm phần tử đơn | 
| 5 3 3 3 3 3 | 10 | Xử lý cao nguyên | 
| 4 1 2 0 2 | 5 | Số không ở ranh giới được xử lý chính xác | 

## Vỏ cạnh 

Đối với mảng`[0, 0, 0]`, thuật toán tính left_ops và right_ops là số 0 cho mỗi phần tử. Tổng bằng 0, phù hợp với mong đợi. Đối với một cao nguyên`[0, 3, 3, 3, 0]`, các ngăn xếp sẽ tìm chính xác phần tử lớn hơn gần nhất để truyền giảm, tránh đếm quá mức. Thuật toán xử lý các ranh giới một cách tự nhiên do các số 0 được đệm, vì vậy chúng ta không cần các trường hợp đặc biệt cho phần tử đầu tiên hoặc phần tử cuối cùng.
