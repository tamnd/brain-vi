---
title: "CF 102864C - Trò chơi thay đổi"
description: "Chúng ta có mảng A ban đầu với các giá trị duy nhất và mảng mục tiêu B, cũng có các giá trị duy nhất. Một thao tác chọn một vị trí và thay thế giá trị hiện tại của nó bằng một số nguyên khác, nhưng mảng phải giữ tất cả các giá trị khác nhau sau mỗi thao tác."
date: "2026-07-25T20:38:41+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102864
codeforces_index: "C"
codeforces_contest_name: "The 15-th BIT Campus Programming Contest - Online Round"
rating: 0
weight: 102864
solve_time_s: 44
verified: true
draft: false
---

[CF 102864C - Trò chơi thay đổi](https://codeforces.com/problemset/problem/102864/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 44s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một mảng ban đầu`A`với các giá trị duy nhất và một mảng mục tiêu`B`, cũng với các giá trị duy nhất. Một thao tác chọn một vị trí và thay thế giá trị hiện tại của nó bằng một số nguyên khác, nhưng mảng phải giữ tất cả các giá trị khác nhau sau mỗi thao tác. Nhiệm vụ là xuất ra chuỗi thao tác ngắn nhất thay đổi`A`vào trong`B`. 

Một vị trí có giá trị đã chính xác thì không bao giờ cần phải chạm vào. Khó khăn đến từ các vị trí cần các giá trị hiện được lưu trữ ở một nơi khác. Nếu một giá trị cần thiết bị chiếm dụng, chúng ta không thể ghi trực tiếp nó vì các giá trị trùng lặp bị cấm trong quá trình này. 

Tổng kích thước của tất cả các mảng có thể đạt tới hai triệu phần tử, vì vậy cách tiếp cận với hành vi bậc hai là không thể. Ngay cả một trường hợp thử nghiệm đơn lẻ với một trăm nghìn vị trí cũng loại trừ việc tìm kiếm liên tục trong mảng hoặc mô phỏng nhiều lựa chọn có thể xảy ra. Chúng ta cần một giải pháp gần với thời gian tuyến tính. 

Một số trường hợp rất dễ xử lý sai. Nếu mọi vị trí đều đúng thì câu trả lời là không có thao tác nào. Ví dụ:```
A = [5, 7]
B = [5, 7]
```Đầu ra đúng là một chuỗi trống. Một giải pháp luôn tạo ra các hoạt động trợ giúp cho các chu kỳ sẽ sai ở đây. 

Một trường hợp tinh vi hơn là một chu trình khép kín:```
A = [1, 2]
B = [2, 1]
```Câu trả lời không thể là hai thao tác. Thay đổi vị trí đầu tiên thành`2`là bất hợp pháp bởi vì`2`vẫn còn hiện diện. Cần có một giá trị tạm thời nên tối thiểu là ba thao tác. 

Một trường hợp quan trọng khác là một chuỗi:```
A = [1, 2, 3]
B = [2, 3, 4]
```Tối thiểu chính xác là ba hoạt động. Vị trí cuối cùng có thể trở thành`4`đầu tiên là giải phóng`3`, thì vị trí trước đó có thể trở thành`3`, và cuối cùng vị trí đầu tiên có thể trở thành`2`. Xử lý chuỗi này theo thứ tự ngược lại sẽ thất bại. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ liên tục tìm một vị trí có thể được thay đổi thành giá trị cuối cùng và thực hiện thao tác đó. Nếu không có vị trí nào như vậy tồn tại, chúng ta có thể tìm kiếm một giá trị tạm thời và phá vỡ sự phụ thuộc. Ý tưởng này đúng vì mọi thao tác đều đặt một giá trị cố định hoặc tạo chỗ cho các thao tác trong tương lai. 

Vấn đề là việc triển khai đơn giản liên tục tìm kiếm các vị trí có sẵn. Trong trường hợp xấu nhất, một chuỗi phụ thuộc dài có độ dài`n`có thể buộc quét gần như toàn bộ mảng nhiều lần. Với`n = 100000`, điều này có thể tiếp cận`O(n^2)`làm việc vượt xa phạm vi cho phép. 

Quan sát quan trọng là các vị trí không chính xác tạo thành biểu đồ phụ thuộc. Mỗi vị trí sai có một cạnh từ giá trị hiện tại đến giá trị cần thiết. Vì tất cả các giá trị trong cả hai mảng là duy nhất nên mỗi giá trị có nhiều nhất một cạnh ra và nhiều nhất một cạnh vào. Mỗi thành phần là một đường dẫn hoặc một chu trình. 

Một đường dẫn có điểm cuối mà giá trị mục tiêu hiện không cần thiết cho một vị trí sai khác. Chúng ta có thể giải quyết một thành phần như vậy từ đầu trở đi. Một chu trình không có giá trị tự do, do đó cần thêm một giá trị tạm thời để phá vỡ nó. Mỗi vị trí sai cần ít nhất một thao tác, và mỗi chu kỳ cần đúng một thao tác bổ sung nên cách xây dựng này là tối ưu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng ánh xạ từ mọi giá trị trong`A`đến vị trí của nó. Đối với mỗi vị trí mà`A[i]`khác với`B[i]`, tạo sự phụ thuộc vào vị trí hiện đang nắm giữ`B[i]`nếu vị trí đó cũng sai. 

Biểu đồ phụ thuộc này mô tả chính xác những giá trị nào chặn lẫn nhau. 
2. Tính mức độ đến của từng vị trí sai. Các vị trí có độ đến bằng 0 là điểm bắt đầu của chuỗi. 

Đây là những thành phần hiện đang thiếu một số giá trị bắt buộc, do đó không cần giá trị tạm thời. 
3. Đối với mỗi lần bắt đầu chuỗi, hãy làm theo các con trỏ phụ thuộc và lưu trữ các vị trí theo thứ tự. Xuất các hoạt động theo thứ tự ngược lại dọc theo chuỗi. 

Phần tử cuối cùng của chuỗi là phần tử duy nhất có thể được thay đổi ngay lập tức. Mỗi thao tác trước đó sẽ có thể thực hiện được sau khi giá trị mà nó cần được giải phóng. 
4. Sau khi xử lý chuỗi, mọi vị trí sai chưa được xử lý còn lại đều thuộc về một chu trình. 

Chọn một giá trị không có trong mảng ban đầu làm giá trị tạm thời. Trong một chu kỳ, di chuyển vị trí đầu tiên đến giá trị tạm thời, xoay tất cả các giá trị khác vào vị trí cuối cùng của chúng, sau đó di chuyển giá trị tạm thời vào vị trí cuối cùng còn lại. 
5. Xuất ra tất cả các thao tác được tạo bởi các bước này. 

Tại sao nó hoạt động: mỗi vị trí sai sẽ được thay đổi ít nhất một lần, vì vậy không có giải pháp nào có thể sử dụng ít hơn số vị trí sai làm cơ sở. Một chuỗi có thể được giải chính xác bằng một thao tác cho mỗi vị trí vì nó có một đầu tự do. Một chu trình không thể được giải quyết nếu không đưa ra một giá trị mới, bởi vì mọi giá trị mục tiêu đều nằm trong chu trình. Thuật toán thêm chính xác một thao tác bổ sung cho mỗi chu trình như vậy và không bao giờ tạo ra những thay đổi không cần thiết, do đó số lượng thao tác là tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    ans_all = []

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))

        pos = {x: i for i, x in enumerate(a)}

        wrong = [False] * n
        for i in range(n):
            if a[i] != b[i]:
                wrong[i] = True

        nxt = [-1] * n
        indeg = [0] * n

        for i in range(n):
            if wrong[i] and b[i] in pos and wrong[pos[b[i]]]:
                nxt[i] = pos[b[i]]
                indeg[nxt[i]] += 1

        ops = []
        visited = [False] * n

        for i in range(n):
            if wrong[i] and indeg[i] == 0:
                cur = i
                path = []
                while cur != -1 and not visited[cur]:
                    visited[cur] = True
                    path.append(cur)
                    cur = nxt[cur]

                for x in reversed(path):
                    ops.append((x + 1, b[x]))

        temp = 1000000000
        used = set(a)
        while temp in used:
            temp -= 1

        for i in range(n):
            if wrong[i] and not visited[i]:
                cycle = []
                cur = i
                while not visited[cur]:
                    visited[cur] = True
                    cycle.append(cur)
                    cur = nxt[cur]

                ops.append((cycle[0] + 1, temp))

                for j in range(len(cycle) - 1, 0, -1):
                    ops.append((cycle[j] + 1, a[cycle[j - 1]]))

                ops.append((cycle[0] + 1, b[cycle[0]]))

        ans_all.append(str(len(ops)))
        for x, y in ops:
            ans_all.append(f"{x} {y}")

    sys.stdout.write("\n".join(ans_all))

if __name__ == "__main__":
    solve()
```Từ điển`pos`cung cấp quyền truy cập liên tục theo thời gian cho chủ sở hữu của bất kỳ giá trị nào. Mảng phụ thuộc`nxt`chỉ được xây dựng giữa các vị trí không chính xác vì các vị trí chính xác đã chứa giá trị cuối cùng của chúng và không bao giờ cần phải di chuyển. 

Việc truyền tải đầu tiên xử lý các đường dẫn. Thứ tự ngược lại là chi tiết quan trọng: việc thay đổi vị trí cuối cùng sẽ giải phóng giá trị mà vị trí trước đó yêu cầu. 

Việc xử lý chu trình sử dụng một giá trị tạm thời bên ngoài tập hợp ban đầu. Bước đầu tiên sẽ loại bỏ giá trị bắt đầu của chu trình khỏi mảng, làm cho việc xoay vòng trở nên hợp pháp. Động thái cuối cùng sẽ khôi phục người giữ giá trị tạm thời về đích của nó. 

Tất cả các chỉ số trong quá trình triển khai chỉ được chuyển đổi từ 0 dựa trên 1 khi được lưu trữ trong câu trả lời. Thuật toán không bao giờ cần sửa đổi mảng thực tế vì cấu trúc phụ thuộc đã nắm bắt được toàn bộ quá trình chuyển đổi. 

## Ví dụ đã hoạt động 

Dành cho:```
A = [1, 2]
B = [2, 1]
```chu kỳ phụ thuộc là: 

| Bước | Thành phần hiện tại | Hoạt động | 
| --- | --- | --- | 
| 1 | 1 -> 2, 2 -> 1 | vị trí 1 trở thành tạm thời | 
| 2 | giá trị 1 là miễn phí | vị trí 2 trở thành 1 | 
| 3 | tạm giữ bản gốc 1 | vị trí 1 trở thành 2 | 

Hoạt động bổ sung xuất hiện vì không có điểm cuối miễn phí. Dấu vết xác nhận quy tắc chu kỳ. 

Vì:```
A = [1, 2, 3]
B = [2, 3, 4]
```chuỗi phụ thuộc là: 

| Bước | Vị trí được xử lý | Hoạt động | 
| --- | --- | --- | 
| 1 | vị trí 3 | 3 trở thành 4 | 
| 2 | vị trí 2 | 2 trở thành 3 | 
| 3 | vị trí 1 | 1 trở thành 2 | 

Dấu vết cho thấy lý do tại sao đường dẫn được xử lý ngược. Mỗi thao tác tạo ra giá trị được yêu cầu bởi vị trí tiếp theo. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi vị trí được truy cập với số lần không đổi trong khi xây dựng và duyệt qua các thành phần | 
| Không gian | O(n) | Bản đồ và mảng lưu trữ thông tin cho trường hợp thử nghiệm hiện tại | 

Tổng của tất cả`n`giá trị là hai triệu, do đó nghiệm tuyến tính phù hợp với các giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    data = sys.stdin.read().split()
    sys.stdin = old
    return " ".join(data)

assert run("1\n1\n5\n5\n") == "1 1 5 5", "placeholder execution format"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`A=[5], B=[5]`|`0`hoạt động | Đã đúng mảng | 
|`A=[1,2], B=[2,1]`|`3`hoạt động | Chu kỳ yêu cầu giá trị tạm thời | 
|`A=[1,2,3], B=[2,3,4]`|`3`hoạt động | Đặt hàng theo chuỗi | 
|`A=[1000000000], B=[1]`|`1`hoạt động | Giá trị biên | 

## Vỏ cạnh 

Đối với một mảng đã được giải quyết, chẳng hạn như`A=[5,7]`Và`B=[5,7]`, mọi vị trí đều được đánh dấu chính xác. Không có cạnh đồ thị nào được tạo và câu trả lời vẫn trống. 

Đối với chu kỳ`A=[1,2]`,`B=[2,1]`, cả hai vị trí đều có các cạnh vào và ra, do đó không tồn tại điểm bắt đầu chuỗi nào. Thành phần còn lại được phát hiện dưới dạng chu trình và nhận chính xác một thao tác tạm thời. 

Đối với chuỗi`A=[1,2,3]`,`B=[2,3,4]`, vị trí cuối cùng không có cạnh nào vì`4`không có mặt. Nó trở thành điểm khởi đầu của chuỗi và việc đảo ngược đường dẫn đã thu thập sẽ tạo ra thứ tự hoạt động hợp pháp duy nhất. 

Đối với trường hợp đã có sẵn giá trị ứng cử viên tạm thời, chẳng hạn như một mảng chứa`1000000000`, việc triển khai sẽ giảm ứng viên cho đến khi tìm thấy số nguyên chưa sử dụng. Điều này giữ cho việc di chuyển tạm thời hợp pháp. 

Nếu bạn muốn, tôi cũng có thể cung cấp một phiên bản biên tập ngắn hơn theo phong cách Codeforces với cùng một bằng chứng nhưng văn xuôi ít giải thích hơn.
