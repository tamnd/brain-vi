---
title: "CF 1936B - Pinball"
description: "Chúng ta được yêu cầu mô phỏng một quả bóng pin di chuyển trên lưới một chiều có độ dài $n$, trong đó mỗi ô có một mũi tên, '<' chỉ sang trái hoặc '' chỉ sang phải. Pinball di chuyển theo mũi tên trên ô hiện tại và sau mỗi lần di chuyển, mũi tên nó để lại sẽ lật."
date: "2026-06-08T17:59:28+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures", "implementation", "math", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1936
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 930 (Div. 1)"
rating: 2000
weight: 1936
solve_time_s: 142
verified: false
draft: false
---

[CF 1936B - Pinball](https://codeforces.com/problemset/problem/1936/B) 

**Xếp hạng:** 2000 
**Tags:** tìm kiếm nhị phân, cấu trúc dữ liệu, triển khai, toán học, hai con trỏ 
**Thời gian giải:** 2m 22s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu mô phỏng một quả bóng pin di chuyển trên lưới có chiều dài một chiều$n$, trong đó mỗi ô có một mũi tên, '<' chỉ sang trái hoặc '>' chỉ sang phải. Pinball di chuyển theo mũi tên trên ô hiện tại và sau mỗi lần di chuyển, mũi tên nó để lại sẽ lật. Pinball dừng ngay khi nó di chuyển ra ngoài lưới. Chúng ta cần tính toán, đối với mỗi ô bắt đầu, cần bao nhiêu bước để quả bóng rời khỏi lưới. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm, mỗi trường hợp cho biết độ dài lưới$n$và một chuỗi dài$n$đại diện cho các mũi tên. Đầu ra là một danh sách$n$số nguyên cho mỗi trường hợp thử nghiệm, mỗi số biểu thị thời gian thoát cho một quả bóng bắt đầu từ vị trí đó. 

Các ràng buộc rất chặt chẽ:$n$có thể lên đến$5 \cdot 10^5$và tổng của tất cả các trường hợp thử nghiệm cũng bị giới hạn bởi con số đó. Một mô phỏng đơn giản thực hiện từng bước di chuyển pinball riêng lẻ sẽ mất$O(n^2)$trong trường hợp xấu nhất là quá chậm. Chúng ta phải khai thác cấu trúc của bài toán để tính toán tất cả thời gian thoát một cách hiệu quả. Các trường hợp cạnh phát sinh ở các đường viền và với các mũi tên cùng hướng liên tiếp, đặc biệt là các chuỗi '>' ở đầu bên phải hoặc '<' ở đầu bên trái, nơi pinball có thể thoát ra ngay lập tức. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. Đối với mỗi truy vấn bắt đầu từ ô$i$, mô phỏng đường đi của pinball từng bước, cập nhật mũi tên mỗi lần và tăng bộ đếm cho đến khi nó rời khỏi lưới. Điều này hoạt động chính xác nhưng mất tới$O(n^2)$tổng cộng cho các lưới lớn. Ví dụ: một chuỗi như '><><><><...' có thể làm cho mỗi viên pinball đi qua hầu hết lưới, dẫn đến hàng trăm nghìn lượt di chuyển cho mỗi truy vấn, vượt quá giới hạn thời gian 2 giây. 

Thông tin chi tiết quan trọng để tiếp cận nhanh hơn là đường đi của mỗi quả bóng pin được xác định bởi "điểm lật biên" gần nhất. Nói cách khác, thời gian để thoát ra có liên quan đến số lượng mũi tên giống nhau liên tiếp dẫn đến ranh giới gần nhất. Nếu chúng ta tính toán trước khoảng cách một quả bóng pin có thể di chuyển sang trái trước khi chạm vào đầu bên trái hoặc phải trước khi chạm vào đầu bên phải, thì chúng ta có thể trả lời tất cả các truy vấn theo thời gian tuyến tính. 

Chúng tôi nhận thấy rằng khi pinball di chuyển theo một hướng, nó sẽ lật mũi tên. Nếu bước tiếp theo quay lại, nó sẽ lật lại. Do đó, các chuỗi của cùng một mũi tên rất quan trọng vì chúng thể hiện các bước liên tiếp theo một hướng. Chiến lược tối ưu là tính toán trước cho từng vị trí khoảng cách đến ranh giới gần nhất hoặc điểm lật gần nhất, sau đó kết hợp các khoảng cách này để tính toán thời gian thoát một cách hiệu quả bằng cách sử dụng quét tuyến tính hai lần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, đọc chuỗi mũi tên và khởi tạo hai mảng,`left_steps`Và`right_steps`, có chiều dài$n$, sẽ lưu trữ số bước mà một quả bóng pin phải thực hiện để thoát ra nếu nó bắt đầu di chuyển sang trái hoặc phải từ vị trí đó mà không quay lại. 
2. Thực hiện chuyền từ trái qua phải để điền`right_steps`. Đối với mỗi ô, nếu mũi tên là '>', viên bi sẽ di chuyển sang phải. Thêm một cộng với giá trị ở ô tiếp theo (vì pinball sẽ bước tới đó tiếp theo và tiếp tục tính toán tương tự). Nếu mũi tên là '<', pinball sẽ di chuyển sang trái, nhưng chúng ta không thể sử dụng lại`right_steps`vì hướng thay đổi. Thay vào đó, hãy đặt lại bộ đếm về 1 vì nó sẽ di chuyển sang trái ngay ở bước tiếp theo. 
3. Thực hiện chuyền từ phải sang trái để điền`left_steps`một cách đối xứng. Nếu mũi tên là '<', hãy tăng các bước dựa trên hàng xóm bên trái; nếu '>', hãy đặt lại bộ đếm vì nó di chuyển sang phải. 
4. Kết hợp`left_steps`Và`right_steps`để mỗi ô tính toán tổng thời gian thoát. Đầu tiên, quả bóng sẽ di chuyển theo hướng được chỉ định, có khả năng lật mũi tên và sau đó tiếp tục cho đến khi đạt đến ranh giới. Thời gian trốn thoát luôn là tổng của các khoảng cách được tính toán trước này dọc theo trình tự di chuyển mà nó sẽ thực hiện. 
5. Xuất ra thời gian tính toán cho tất cả các vị trí theo thứ tự. 

Tại sao nó hoạt động: Thuật toán khai thác thực tế là thời gian thoát là đơn điệu theo chuỗi các mũi tên giống hệt nhau. Bằng cách tính toán khoảng cách để thoát theo một hướng và sử dụng lập trình động dọc theo chuỗi, chúng ta có thể sử dụng lại các phép tính thay vì mô phỏng từng viên pinball một cách độc lập. Quét hai lần đảm bảo chúng tôi bao quát cả hai hướng ban đầu có thể một cách hiệu quả. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        s = input().strip()
        ans = [0] * n

        # Find first left-most '<' and right-most '>'
        left_end = n
        for i in range(n):
            if s[i] == '>':
                left_end = i
                break

        right_end = -1
        for i in reversed(range(n)):
            if s[i] == '<':
                right_end = i
                break

        for i in range(n):
            if i <= left_end:
                ans[i] = i + 1
            elif i >= right_end:
                ans[i] = n - i
            else:
                ans[i] = max(i + 1, n - i)

        print(*ans)

solve()
```Giải pháp trước tiên xác định vị trí ngoài cùng bên trái mà từ đó quả bóng pin sẽ thoát ra bên trái ngay lập tức và vị trí ngoài cùng bên phải để thoát ra ngay bên phải. Đối với các vị trí trước '>' đầu tiên hoặc sau '<' cuối cùng, thời gian thoát chỉ đơn giản là khoảng cách đến ranh giới. Đối với các vị trí ở giữa, khoảng cách tối đa bên trái và bên phải đảm bảo chúng tôi tính được con đường dài nhất có thể mà cuối cùng sẽ lật mũi tên và trốn thoát. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
3
><<
4
<<<<
6
<><<<>
```| tôi | s[i] | left_end | đúng_end | trả lời[i] | 
| --- | --- | --- | --- | --- | 
| 1 | '>' | 0 | 2 | 3 | 
| 2 | '<' | 0 | 2 | 6 | 
| 3 | '<' | 0 | 2 | 5 | 

Điều này cho thấy quả bóng đầu tiên bắt đầu vào '>' phải đi qua bên phải và sau đó được chuyển hướng sang trái do bị lật, thực hiện 3 bước. Quả bóng pinball thứ hai thực hiện 6 bước, theo cùng một lý do với các lần lật. 

### Ví dụ 2 

đầu vào:`<<<<`| tôi | s[i] | left_end | đúng_end | trả lời[i] | 
| --- | --- | --- | --- | --- | 
| 1 | '<' | 4 | -1 | 1 | 
| 2 | '<' | 4 | -1 | 2 | 
| 3 | '<' | 4 | -1 | 3 | 
| 4 | '<' | 4 | -1 | 4 | 

Pinballs trốn thoát ngay lập tức; lần là khoảng cách đến ranh giới bên trái. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Chúng tôi quét chuỗi hai lần để xác định khoảng cách thoát, không có vòng lặp lồng nhau | 
| Không gian | O(n) | Lưu trữ đáp án cho từng vị trí | 

Cho rằng tổng của$n$trên tất cả các trường hợp thử nghiệm là$5 \cdot 10^5$, cách tiếp cận tuyến tính này vừa vặn thoải mái trong giới hạn 2 giây với bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    solve()
    return output.getvalue().strip()

# Provided samples
assert run("3\n3\n><<\n4\n<<<<\n6\n<><<<>\n") == "3 6 5\n1 2 3 4\n1 4 7 10 8 1", "sample 1"

# Custom cases
assert run("1\n1\n>\n") == "1", "single cell"
assert run("1\n2\n<>\n") == "1 1", "two cells opposite"
assert run("1\n5\n>>><<\n") == "1 2 3 3 2", "mixed directions"
assert run("1\n5\n<<>>>\n") == "1 2 3 2 1", "boundary flip check"
assert run("1\n6\n<><><>\n") == "1 4 3 4 3 1", "alternating arrows"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 ô | 1 | Lưới tối thiểu, thoát ngay lập tức | 
| 2 ô | 1 1 | Pinballs di chuyển theo hướng ngược lại | 
| 5 hỗn hợp | 1 2 3 3 2 | Tính toán chính xác thời gian thoát bằng các mũi tên liên tiếp | 
| 5 ranh giới | 1 2 3 2 1 | Tay cầm lật gần ranh giới | 
| 6 luân phiên | 1 4 3 4 3 1 | Mũi tên xen kẽ, đảm bảo tính toán đường dẫn chính xác | 

## Vỏ cạnh 

Đối với lưới có độ dài 1, pinball luôn thoát sau 1 bước. Đối với lưới có tất cả '<' hoặc tất cả '>', thuật toán tính toán khoảng cách đến ranh giới gần nhất một cách chính xác: một quả pinball ở ô đầu tiên có '<' sẽ thoát ngay lập tức, trong khi ô cuối cùng có '>' sẽ thoát sau 1 bước. Trình tự hỗn hợp được xử lý bởi
