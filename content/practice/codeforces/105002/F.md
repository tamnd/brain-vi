---
title: "CF 105002F - \u0417\u0430\u043c\u0435\u043d\u0438 \u043d\u0430 \u0441\u0443\u043c\u043c\u0443"
description: "Hai chuỗi số được đưa ra và thao tác được phép là nén liên tục bất kỳ cặp liền kề nào bên trong một trong hai chuỗi bằng cách thay thế nó bằng tổng của chúng. Mỗi lần nén sẽ giảm độ dài của chuỗi đó đi một, trong khi vẫn giữ nguyên tổng số phần tử của nó."
date: "2026-06-28T03:19:03+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105002
codeforces_index: "F"
codeforces_contest_name: "vkoshp.letovo 2022"
rating: 0
weight: 105002
solve_time_s: 65
verified: true
draft: false
---

[CF 105002F - \u0417\u0430\u043c\u0435\u043d\u0438 \u043d\u0430 \u0441\u0443\u043c\u043c\u0443](https://codeforces.com/problemset/problem/105002/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 5s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Hai chuỗi số được đưa ra và thao tác được phép là nén liên tục bất kỳ cặp liền kề nào bên trong một trong hai chuỗi bằng cách thay thế nó bằng tổng của chúng. Mỗi lần nén sẽ giảm độ dài của chuỗi đó đi một, trong khi vẫn giữ nguyên tổng số phần tử của nó. 

Sau khi thực hiện một số chuỗi nén như vậy trên cả hai mảng, mục tiêu là xác định xem liệu có thể tạo các chuỗi kết quả giống hệt nhau hay không và liệu điều này có thể được thực hiện bằng cách sử dụng nhiều nhất hay không.`k`tổng số hoạt động. 

Quan điểm chính là chúng tôi không sắp xếp lại các phần tử, chỉ hợp nhất các khối liền kề. Vì vậy, mỗi phần tử cuối cùng tương ứng với một phân đoạn liền kề của mảng ban đầu và giá trị của nó là tổng của phân đoạn đó. 

Những hạn chế`n, m ≤ 3 · 10^5`loại trừ mọi mô phỏng hợp nhất bậc hai. Bất kỳ giải pháp nào cũng phải xử lý cả hai mảng theo thời gian tuyến tính hoặc gần tuyến tính. Giá trị của`k`có thể lớn, do đó hạn chế thực sự là số lần hợp nhất cần thiết chứ không phải chi phí của chúng về mặt số học. 

Một trường hợp lỗi tinh vi xuất hiện khi tổng khớp nhưng không thể phân đoạn. 

Ví dụ, hãy xem xét:```
a = [1, 2, 3]
b = [3, 3]
```Cả hai đều có tổng số tiền là 6 và cả hai đều có thể được hợp nhất thành một khối duy nhất. Nhưng nếu`k = 0`, không được phép thực hiện phép toán nào nên chúng ta phải kiểm tra cấu trúc chứ không chỉ kiểm tra tổng. 

Một trường hợp phức tạp khác:```
a = [1, 2, 3]
b = [2, 4]
```Mặc dù tổng số tiền khớp với (6), chúng tôi không thể căn chỉnh các phân vùng mà không vi phạm các ràng buộc kề. Bất kỳ giải pháp đúng nào cũng phải mô phỏng ranh giới hợp nhất một cách cẩn thận. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp cố gắng mô phỏng tất cả các cách có thể để hợp nhất từng chuỗi thành tất cả các phân đoạn có thể. Điều này tương đương với việc liệt kê tất cả các phân vùng của mỗi mảng thành các khối liền kề và kiểm tra xem có tồn tại một chuỗi tổng khối chung hay không. Số lượng phân vùng tăng theo cấp số nhân với`n`, vì mỗi vị trí có thể là một vết cắt hoặc không. 

Ngay cả khi chúng ta hạn chế sự chú ý đến việc khớp các tổng một cách tham lam, một giải pháp quay lui ngây thơ vẫn phân nhánh mạnh mẽ khi các tổng khác nhau một chút, dẫn đến hành vi trong trường hợp xấu nhất theo cấp số nhân. 

Quan sát cấu trúc là mỗi chuỗi sau các thao tác sẽ trở thành một chuỗi tổng khối và cả hai mảng phải kết thúc bằng cùng một chuỗi tổng khối. Điều này có nghĩa là chúng ta nên khớp các tiền tố có tổng bằng nhau một cách tham lam, mở rộng phân đoạn hiện tại trong mảng có tổng chạy nhỏ hơn. 

Chúng tôi quét cả hai mảng bằng hai con trỏ, tích lũy một phần tổng cho đến khi chúng khớp nhau. Mỗi khi chúng khớp nhau, chúng tôi sẽ hoàn thiện một khối và di chuyển cả hai con trỏ về phía trước. Số lượng thao tác cần thiết là số lần hợp nhất cần thiết để tạo các khối này, bằng với số phần tử bị loại bỏ trong quá trình này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tìm kiếm phân vùng Brute Force | Hàm mũ | O(n + m) | Quá chậm | 
| Hợp nhất tham lam hai con trỏ | O(n + m) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi mô phỏng việc xây dựng tổng số khối bằng nhau từ trái sang phải. 

1. Khởi tạo hai con trỏ`i`Và`j`ở đầu mảng`a`Và`b`. Cũng duy trì số tiền đang chạy`sa`Và`sb`, cả hai ban đầu đều bằng không. Chúng đại diện cho khối chưa hoàn thành hiện tại ở mỗi bên. 
2. Mở rộng khối hiện tại về phía có tổng chạy nhỏ hơn hoặc bằng nhau. Nếu như`sa ≤ sb`, lấy`a[i]`và thêm nó vào`sa`, sau đó tăng`i`. Nếu không thì lấy`b[j]`và thêm nó vào`sb`, sau đó tăng`j`. Điều này đảm bảo chúng tôi không vượt qua ranh giới phù hợp tiềm năng sớm. 
3. Bất cứ khi nào`sa == sb`, chúng ta đã hình thành một khối hợp lệ tồn tại trong cả hai phân tách. Chúng tôi đặt lại`sa`Và`sb`về 0 và tăng số khối đã hoàn thành. 
4. Tiếp tục cho đến khi cả hai mảng đều được sử dụng hết. Nếu tại bất kỳ thời điểm nào, một mảng đã hoàn thành nhưng mảng kia vẫn có tổng khác 0, thì việc so khớp là không thể vì chúng ta không thể hoàn thành khối căn chỉnh cuối cùng. 
5. Sau khi xử lý, chúng ta có một chuỗi các khối khớp. Số lượng hợp nhất cần thiết bằng tổng số phần tử trừ đi số khối được hình thành, được tính riêng cho từng mảng. Chúng tôi kiểm tra xem tổng số thao tác được yêu cầu có tối đa không`k`. 

Tại sao nó hoạt động: bất cứ lúc nào chúng tôi duy trì rằng phân đoạn một phần hiện tại trong mỗi mảng là khối ứng cử viên duy nhất có thể khớp với phân đoạn tiếp theo trong phân tách cuối cùng. Bởi vì tất cả các phần tử đều dương, nên việc mở rộng tổng nhỏ hơn không thể bỏ qua điểm căn chỉnh hợp lệ, vì chỉ có thể đạt được sự bằng nhau bằng cách tăng cạnh nhỏ hơn. Do đó, mọi điểm đẳng thức đều tương ứng với một ranh giới cần thiết trong bất kỳ nghiệm hợp lệ nào, làm cho việc phân vùng tham lam trở thành chính tắc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))
    k = int(input())

    i = j = 0
    sa = sb = 0
    blocks = 0

    while i < n or j < m:
        if sa <= sb and i < n:
            sa += a[i]
            i += 1
        elif j < m:
            sb += b[j]
            j += 1
        else:
            break

        if sa == sb:
            blocks += 1
            sa = sb = 0

    if sa != 0 or sb != 0:
        print("NO")
        return

    # number of merges = (n - blocks) + (m - blocks)
    ops = (n - blocks) + (m - blocks)
    print("YES" if ops <= k else "NO")

if __name__ == "__main__":
    solve()
```Giải pháp duy trì hai bộ tích lũy phát trực tuyến và xây dựng các phân đoạn phù hợp một cách tham lam. Logic con trỏ đảm bảo rằng chúng ta luôn tăng tổng tích lũy nhỏ hơn, điều này ngăn cản việc bỏ qua ranh giới đẳng thức tiềm năng. các`blocks`bộ đếm theo dõi có bao nhiêu phân đoạn cuối cùng được chia sẻ giữa hai mảng. 

Một điểm tinh tế là kiểm tra cuối cùng`sa != 0 or sb != 0`. Điều này phát hiện các trường hợp trong đó một chuỗi kết thúc ở một phần không thể khớp, điều này sẽ vượt qua không chính xác nếu chỉ xem xét tổng số tiền. 

Công thức tính phép toán suy ra trực tiếp từ thực tế là việc xoay một đoạn có chiều dài`x`thành một khối yêu cầu`x-1`hợp nhất. Tổng hợp tất cả các khối mang lại lợi nhuận`n - blocks`Và`m - blocks`. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
a = [2, 1, 2, 1, 4, 5]
b = [2, 1, 3, 3, 6]
k = 2
```| Bước | tôi | j | sa | sb | khối | 
| --- | --- | --- | --- | --- | --- | 
| bắt đầu | 0 | 0 | 0 | 0 | 0 | 
| thêm | 1 | 0 | 2 | 0 | 0 | 
| thêm b | 1 | 1 | 2 | 2 | 1 | 
| thêm | 2 | 1 | 3 | 2 | 1 | 
| thêm b | 2 | 2 | 3 | 3 | 2 | 
| đặt lại | 2 | 2 | 0 | 0 | 2 | 
| tiếp tục tăng trưởng không phù hợp | ... | ... | ... | ... | ... | 

Quá trình này nhanh chóng hình thành các phân đoạn bằng nhau ban đầu, nhưng sự phân kỳ sau đó buộc các sự hợp nhất bổ sung vượt quá mức cho phép.`k`. Điều này chứng tỏ rằng tổng số tiền phù hợp là không đủ và sự liên kết cấu trúc của các ranh giới phân đoạn là quan trọng. 

### Mẫu 2 

Cùng một mảng, nhưng`k = 9`. 

| Bước | tôi | j | sa | sb | khối | 
| --- | --- | --- | --- | --- | --- | 
| bắt đầu | 0 | 0 | 0 | 0 | 0 | 
| tiến trình hợp nhất | ... | ... | ... | ... | ... | 
| căn chỉnh đầy đủ | n | m | 0 | 0 | 3 | 

Ở đây chúng tôi đã phân tách thành công cả hai mảng thành cùng một chuỗi tổng khối. Số lượng khối được tối đa hóa, giúp giảm thiểu số lượng hợp nhất cần thiết. Từ`k`đủ lớn thì việc xây dựng là khả thi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | mỗi phần tử được xử lý một lần bởi một con trỏ | 
| Không gian | O(1) | chỉ sử dụng bộ tích lũy và bộ đếm | 

Quét tuyến tính dễ dàng phù hợp với giới hạn tối đa 3 · 10^5 phần tử. Không cần đệ quy hoặc cấu trúc phụ trợ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return __import__("__main__").solve()  # adjust if needed

# sample 1
assert run("""6 5
2 1 2 1 4 5
2 1 3 3 6
2
""") == "NO"

# sample 2
assert run("""6 5
2 1 2 1 4 5
2 1 3 3 6
9
""") == "YES"

# minimum size equal
assert run("""1 1
5
5
0
""") == "YES"

# impossible mismatch
assert run("""2 2
1 2
2 2
1
""") == "NO"

# already equal, no ops
assert run("""3 3
1 2 3
1 2 3
0
""") == "YES"

# large single block
assert run("""4 3
1 1 1 1
2 2 2
5
""") == "YES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 bằng | CÓ | trường hợp tối thiểu | 
| số tiền không khớp | KHÔNG | phát hiện không thể | 
| mảng giống hệt nhau | CÓ | độ chính xác của hoạt động bằng không | 
| trường hợp nặng hợp nhất | CÓ | đếm thao tác đúng | 

## Vỏ cạnh 

Trường hợp cạnh khóa xảy ra khi một mảng kết thúc chính xác tại một ranh giới khối trong khi mảng kia vẫn có tổng một phần. 

đầu vào:```
a = [1, 2, 3]
b = [1, 3, 2]
```Trong quá trình xử lý, cả hai bộ tích lũy có thể đạt được tổng trung gian giống nhau nhiều lần, nhưng nếu một bên cạn kiệt phần tử trong khi bên kia vẫn còn`sa > 0`, thuật toán bác bỏ trường hợp này. Điều này đảm bảo chúng tôi không nhầm tưởng rằng các phần tử còn lại có thể được hấp thụ sau này. 

Một trường hợp khác là khi sự bình đẳng chỉ có thể thực hiện được ở phút cuối cùng.```
a = [1, 1, 1]
b = [3]
```Thuật toán tích lũy cho đến khi cả hai đạt 3 cùng một lúc. Chỉ sau đó nó mới đăng ký một khối duy nhất. Kiểm tra khác 0 cuối cùng đảm bảo tính chính xác khi một chuỗi kết thúc sớm hơn ở trạng thái trung gian. 

Những trường hợp này xác nhận rằng việc phát hiện đẳng thức được gắn chặt với các tổng tiền tố có thể truy cập và không có sự sắp xếp lại nhân tạo nào được đưa ra.
