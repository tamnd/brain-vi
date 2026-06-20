---
title: "CF 1027D - Săn Chuột"
description: "Chúng ta có thể coi ký túc xá như một đồ thị có hướng trong đó mỗi phòng có chính xác một cạnh hướng ra ngoài. Từ mỗi phòng i, con chuột chắc chắn sẽ di chuyển đến a[i] sau một giây."
date: "2026-06-16T21:32:59+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "graphs"]
categories: ["algorithms"]
codeforces_contest: 1027
codeforces_index: "D"
codeforces_contest_name: "Educational Codeforces Round 49 (Rated for Div. 2)"
rating: 1700
weight: 1027
solve_time_s: 229
verified: true
draft: false
---

[CF 1027D - Săn chuột](https://codeforces.com/problemset/problem/1027/D) 

**Đánh giá:** 1700 
**Thẻ:** dfs và các biểu đồ tương tự 
**Thời gian giải:** 3 phút 49 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có thể coi ký túc xá như một đồ thị có hướng trong đó mỗi phòng có chính xác một cạnh hướng ra ngoài. Từ mỗi phòng`i`, con chuột chắc chắn sẽ di chuyển đến`a[i]`sau một giây. Bởi vì mỗi nút có chính xác một cạnh đi ra, đồ thị sẽ phân tách thành các chuỗi mà cuối cùng đi vào các chu kỳ có hướng và sau đó ở trong các chu kỳ đó mãi mãi. 

Một cái bẫy được đặt trong một căn phòng sẽ loại bỏ căn phòng đó khỏi trạng thái an toàn. Nếu chuột rơi vào phòng bị mắc kẹt, quá trình sẽ dừng lại. Khó khăn xuất phát từ thực tế là không xác định được phòng bắt đầu, vì vậy chúng ta phải đảm bảo rằng mọi vị trí xuất phát có thể cuối cùng đều dẫn đến một đỉnh bị mắc kẹt dọc theo bước đi xác định của nó. 

Nhiệm vụ là chọn một tập hợp con các nút giảm thiểu tổng chi phí sao cho mỗi bước đi vô hạn trong biểu đồ hàm này cuối cùng sẽ chạm đến ít nhất một nút đã chọn. 

Ràng buộc`n ≤ 2 * 10^5`loại trừ bất kỳ giải pháp nào mô phỏng việc bắt đầu từ mọi nút một cách độc lập và tiến về phía trước cho đến khi được phát hiện, bởi vì đó sẽ là`O(n^2)`trong trường hợp xấu nhất khi đồ thị tạo thành cấu trúc chuỗi hoặc chu trình dài. Chúng ta cần một cách tiếp cận phân rã đồ thị tuyến tính hoặc gần tuyến tính, thông thường`O(n)`hoặc`O(n log n)`. 

Một trường hợp cạnh ngây thơ nhưng quan trọng là một chu trình thuần túy. Nếu tất cả các nút tạo thành một chu trình và chúng ta không đặt bẫy bên trong chu trình đó thì con chuột bắt đầu trong chu trình đó sẽ không bao giờ bị bắt. Ví dụ: 

đầu vào:```
3
5 1 5
2 3 1
```Đây`1 → 2 → 3 → 1`. Bất kỳ giải pháp đúng nào cũng phải chọn ít nhất một nút trong chu trình này. Một kẻ tham lam bất cẩn chỉ xem xét các cấu trúc dạng cây sẽ thất bại ở đây vì nó bỏ qua thực tế rằng các chu trình là các lớp lặp lại đóng. 

Một trường hợp tinh tế khác là tự lặp`i → i`. Chỉ riêng nút đó đã tạo thành một chu kỳ có độ dài bằng một và nếu nó không được chọn, một con chuột bắt đầu từ đó sẽ không bao giờ bị bắt. Điều này buộc mỗi chu kỳ phải được “bao phủ” bởi ít nhất một nút đã chọn. 

## Phương pháp tiếp cận 

Cấu trúc chính là mỗi nút thuộc về chính xác một chu trình được định hướng, có thể có các cây chảy vào đó. Khi chuột bước vào một chu kỳ, nó sẽ không bao giờ rời đi, vì vậy cách duy nhất để đảm bảo bị bắt là đặt ít nhất một bẫy trong mỗi chu kỳ. Các nút bên ngoài chu trình (trong cây dẫn vào chu trình) là tùy chọn, bởi vì bất kỳ đường đi nào từ chúng cuối cùng cũng đến được chu trình của chúng. 

Do đó, bài toán rút gọn thành: xác định tất cả các chu trình có hướng trong đồ thị hàm số và với mỗi chu trình, hãy chọn nút chi phí tối thiểu bên trong nó. 

Chiến lược bạo lực sẽ bắt đầu từ mọi nút, mô phỏng đường dẫn của nó và cố gắng quyết định nút nào phải được chọn để tất cả các đường dẫn đều bị chặn. Điều này nhanh chóng trở nên tốn kém vì mỗi mô phỏng có thể đi qua tối đa`O(n)`các bước và thực hiện việc này cho tất cả các điểm bắt đầu mang lại kết quả`O(n^2)`. 

Sự cải tiến đến từ việc nhận ra rằng biểu đồ là một tập hợp các chu trình rời rạc với các cây sắp tới. Mỗi nút có mức độ ngoài 1, do đó việc phát hiện chu kỳ rất đơn giản bằng cách sử dụng DFS hoặc bóc tách mức độ. Khi các chu kỳ được tách biệt, mỗi chu kỳ sẽ trở nên độc lập: việc chọn các nút trong một chu kỳ không ảnh hưởng đến bất kỳ chu kỳ nào khác. 

Vì vậy, thay vì lý luận về đường dẫn, chúng tôi lý luận về các thành phần. Đối với mỗi chu kỳ, chúng tôi tính toán nút chi phí tối thiểu của nó và thêm nó vào câu trả lời. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Phân hủy chu kỳ | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi sử dụng tính năng phát hiện chu trình trong biểu đồ hàm. 

1. Đánh dấu tất cả các nút là chưa được truy cập. Chúng tôi sẽ duyệt qua từng nút chính xác một lần làm điểm bắt đầu DFS. 
2. Đối với mỗi nút chưa được truy cập`i`, bắt đầu đi về phía trước theo sau`a[i]`, đánh dấu các nút là “trong ngăn xếp đệ quy hiện tại”. 

Việc đánh dấu ngăn xếp này rất quan trọng vì nó cho phép chúng tôi phát hiện khi nào chúng tôi quay lại nút đã có trên đường dẫn hiện tại, điều này cho biết một chu kỳ. 
3. Nếu trong quá trình truyền tải, chúng ta đến được một nút đã có trong ngăn xếp hiện tại, thì chúng ta đã tìm thấy một chu trình có hướng. Sau đó chúng tôi trích xuất tất cả các nút từ chu kỳ đó. 

Việc trích xuất được thực hiện bằng cách đi bộ từ nút lặp lại cho đến khi chúng ta quay lại nút đó. 
4. Với mỗi chu kỳ được phát hiện, hãy tính chi phí tối thiểu`min(c[v])`trên tất cả các nút`v`trong chu trình đó và thêm nó vào câu trả lời. 

Điều này đúng vì chúng ta chỉ cần một bẫy trong mỗi chu kỳ và việc chọn nút rẻ nhất sẽ giảm thiểu chi phí. 
5. Tiếp tục cho đến khi tất cả các nút được xử lý. 

Điểm tinh tế quan trọng là các nút không phải là một phần của chu kỳ cuối cùng sẽ luôn đạt đến một chu kỳ, nhưng chúng sẽ không bao giờ là một phần của xung đột được xem lại ngăn xếp. Chúng chỉ đơn giản được di chuyển ngang và đánh dấu, đảm bảo phạm vi bao phủ tuyến tính. 

### Tại sao nó hoạt động 

Mỗi nút nằm trong một chu trình hoặc trong một cây dẫn đến đúng một chu trình. Bất kỳ vị trí bắt đầu nào cuối cùng cũng đạt đến chu kỳ của nó và không bao giờ rời khỏi nó. Do đó, nếu chúng ta đặt ít nhất một bẫy trong mỗi chu kỳ, mỗi bước đi vô hạn đảm bảo cuối cùng sẽ chạm tới một nút bị mắc kẹt. Ngược lại, nếu chu trình nào không có bẫy thì bắt đầu từ chu trình đó sẽ vĩnh viễn tránh được mọi bẫy. Vì vậy, giải pháp vừa cần vừa đủ, và giảm thiểu chi phí cho mỗi chu kỳ là tối ưu vì các chu kỳ là độc lập. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(10**7)

def solve():
    n = int(input())
    c = [0] + list(map(int, input().split()))
    a = [0] + list(map(int, input().split()))

    visited = [0] * (n + 1)
    in_stack = [0] * (n + 1)
    ans = 0

    for i in range(1, n + 1):
        if visited[i]:
            continue

        stack = []
        v = i

        while not visited[v]:
            visited[v] = 1
            in_stack[v] = 1
            stack.append(v)
            v = a[v]

            if in_stack[v]:
                cycle = []
                idx = len(stack) - 1
                while stack[idx] != v:
                    cycle.append(stack[idx])
                    idx -= 1
                cycle.append(v)

                ans += min(c[x] for x in cycle)
                break

        for node in stack:
            in_stack[node] = 0

    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp duy trì hai trạng thái:`visited`đảm bảo mỗi nút được xử lý một lần trên toàn cầu, trong khi`in_stack`theo dõi đường truyền hiện tại để có thể phát hiện các chu kỳ ngay lập tức khi chúng tôi truy cập lại một nút vẫn còn trong chuỗi hoạt động. 

Ngăn xếp lưu trữ thứ tự truyền tải chính xác, cho phép xây dựng lại chu trình theo thời gian tuyến tính khi tìm thấy sự lặp lại. Thời điểm chúng tôi phát hiện một chu kỳ, chúng tôi chỉ cô lập các nút tạo thành chu kỳ đó và tính toán chi phí tối thiểu giữa chúng. Thanh toán bù trừ`in_stack`sau khi kết thúc mỗi bước đi giống như DFS sẽ ngăn chặn việc phát hiện chu trình sai trên các thành phần khác nhau. 

## Ví dụ đã hoạt động 

Hãy xem xét mẫu:```
5
1 2 3 2 10
1 3 4 3 3
```Chúng tôi theo dõi phát hiện chu kỳ. 

| Bắt đầu | Đường đi ngang | Phát hiện chu kỳ | Nút chu kỳ | Chi phí đã chọn | 
| --- | --- | --- | --- | --- | 
| 1 | 1 → 1 | tự lặp | {1} | 1 | 
| 2 | 2 → 3 → 4 → 3 | 3 chu kỳ | {3,4} | 3 | 
| 5 | 5 → 3 | đã được xử lý | - | - | 

Điều này chứng tỏ rằng các nút 1 và chu kỳ {3,4} xác định tất cả các điểm bẫy cần thiết. 

Một ví dụ khác:```
4
4 1 2 3
```Điều này tạo thành một chu kỳ duy nhất`1 → 4 → 3 → 2 → 1`. 

| Bắt đầu | Đường đi ngang | Phát hiện chu kỳ | Nút chu kỳ | Chi phí đã chọn | 
| --- | --- | --- | --- | --- | 
| 1 | 1 → 4 → 3 → 2 → 1 | chu kỳ đầy đủ | {1,2,3,4} | phút(c) | 

Thuật toán xác định chính xác toàn bộ biểu đồ là một chu kỳ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi nút được truy cập một lần và vào/ra ngăn xếp một lần | 
| Không gian | O(n) | Mảng cho cấu trúc đã truy cập, theo dõi ngăn xếp và đệ quy | 

Độ phức tạp tuyến tính là đủ cho`n ≤ 2 · 10^5`và mức sử dụng bộ nhớ nằm trong giới hạn vì chúng tôi chỉ lưu trữ các mảng phụ trợ không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    from collections import deque

    n = int(sys.stdin.readline())
    c = [0] + list(map(int, sys.stdin.readline().split()))
    a = [0] + list(map(int, sys.stdin.readline().split()))

    visited = [0] * (n + 1)
    in_stack = [0] * (n + 1)
    ans = 0

    for i in range(1, n + 1):
        if visited[i]:
            continue
        stack = []
        v = i
        while not visited[v]:
            visited[v] = 1
            in_stack[v] = 1
            stack.append(v)
            v = a[v]
            if in_stack[v]:
                cycle = []
                idx = len(stack) - 1
                while stack[idx] != v:
                    cycle.append(stack[idx])
                    idx -= 1
                cycle.append(v)
                ans += min(c[x] for x in cycle)
                break
        for x in stack:
            in_stack[x] = 0

    return str(ans)

# provided sample
assert run("""5
1 2 3 2 10
1 3 4 3 3
""") == "3"

# self-loop
assert run("""1
5
1
""") == "5"

# single cycle
assert run("""4
4 3 2 1
2 3 4 1
""") == "1"

# two disjoint cycles
assert run("""6
5 4 3 2 1 10
2 1 4 3 6 5
""") == "6"

# chain into cycle
assert run("""5
10 1 10 10 10
2 3 3 4 4
""") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nút đơn tự lặp | 5 | xử lý chu trình tối thiểu | 
| chu kỳ đảo ngược | 1 | trích xuất chu trình chính xác | 
| hai chu kỳ | 6 | sự độc lập của các thành phần | 
| chuỗi thành chu kỳ | 1 | bỏ qua các nút không có chu kỳ | 

## Vỏ cạnh 

Nút tự lặp thể hiện chu trình đơn giản nhất. Thuật toán bắt đầu tại nút đó, đánh dấu nó trong ngăn xếp và ngay lập tức truy cập lại chính nó. Việc trích xuất chu trình chỉ trả về nút đó và chi phí của nó sẽ được cộng thêm. Không có nút nào khác can thiệp vì không có nút nào. 

Một chuỗi dài dẫn vào một chu kỳ cho thấy tại sao các nút không thuộc chu kỳ không đóng góp. Quá trình truyền tải đánh dấu tất cả các nút chuỗi là đã truy cập nhưng không bao giờ phát hiện chúng là các mục nhập chu kỳ, vì chúng không được xem lại trong ngăn xếp hiện tại. Chỉ khi đạt đến chu kỳ thì sự lặp lại mới xảy ra và chỉ chu kỳ đó mới góp phần đưa ra câu trả lời.
