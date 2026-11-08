---
title: "CF 1452A - Chương trình Robot"
description: "Chúng ta có một robot đứng ở điểm gốc của lưới 2D vô hạn và chúng ta cần hướng dẫn nó đến ô đích có tọa độ $(x, y)$. Robot có thể di chuyển về phía bắc, phía đông, phía nam, phía tây hoặc giữ nguyên vị trí nhưng không thể lặp lại cùng một lệnh liên tiếp."
date: "2026-06-11T03:13:28+07:00"
tags: ["codeforces", "competitive-programming", "math"]
categories: ["algorithms"]
codeforces_contest: 1452
codeforces_index: "A"
codeforces_contest_name: "Educational Codeforces Round 98 (Rated for Div. 2)"
rating: 800
weight: 1452
solve_time_s: 82
verified: true
draft: false
---

[CF 1452A - Chương trình Robot](https://codeforces.com/problemset/problem/1452/A) 

**Đánh giá:** 800 
**Thẻ:** toán 
**Thời gian giải:** 1m 22s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một robot đứng ở điểm gốc của lưới 2D vô hạn và chúng ta cần hướng dẫn nó đến ô mục tiêu có tọa độ$(x, y)$. Robot có thể di chuyển về phía bắc, phía đông, phía nam, phía tây hoặc giữ nguyên vị trí nhưng không thể lặp lại cùng một lệnh liên tiếp. Đầu vào cung cấp cho chúng tôi nhiều trường hợp kiểm thử, mỗi trường hợp chỉ định một đích đến$(x, y)$. Đối với mỗi điểm đến, chúng ta phải xác định số bước di chuyển tối thiểu để robot có thể đi$(0, 0)$ĐẾN$(x, y)$đồng thời tôn trọng hạn chế lệnh không liên tiếp. 

Những hạn chế là khiêm tốn. Với tối đa 100 trường hợp thử nghiệm và tọa độ lên tới$10^4$, chúng ta cần một thuật toán tính toán kết quả theo thời gian tuyến tính cho mỗi trường hợp thử nghiệm. Bất cứ điều gì cố gắng mô phỏng tất cả các chuỗi di chuyển có thể sẽ quá chậm vì số lượng các chuỗi tiềm năng tăng theo cấp số nhân với$x + y$. 

Các trường hợp cạnh phát sinh khi một trong hai$x$hoặc$y$bằng 0, vì các lệnh xen kẽ đòi hỏi phải xử lý cẩn thận. Ví dụ, để chuyển từ$(0, 0)$ĐẾN$(2, 0)$, một chiến lược ngây thơ có thể gợi ý hai bước di chuyển về phía đông, nhưng robot không thể thực hiện được`E E`liên tiếp, do đó, trình tự tối thiểu thực sự sử dụng lệnh "ở lại" ở giữa:`E 0 E`. Tương tự, khi cả hai tọa độ đều bằng 0, robot đã đến đích và không cần lệnh nào. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng tạo ra tất cả các chuỗi di chuyển tôn trọng quy tắc và phạm vi tiếp cận "không lặp lại".$(x, y)$. Phương pháp này đúng về nguyên tắc, nhưng không khả thi vì đối với tọa độ lớn, số chuỗi tăng theo cấp số nhân, theo thứ tự$2^{x+y}$trong trường hợp xấu nhất. 

Cái nhìn sâu sắc quan trọng là hạn chế chỉ quan trọng khi chúng ta có những bước đi liên tiếp theo cùng một hướng. Để tránh lặp lại cùng một mệnh lệnh, chúng ta có thể luân phiên giữa hai hướng cần di chuyển: bắc-nam và đông-tây. Thực tế, độ dài chuỗi tối thiểu chỉ phụ thuộc vào độ dài lớn hơn của$x$Và$y$. Nếu cả hai tọa độ đều bằng nhau, chúng ta có thể luân phiên di chuyển mà không cần thêm lệnh "ở lại", tạo ra kết quả chính xác$x + y$di chuyển. Nếu tọa độ không bằng nhau, sau khi luân phiên dọc theo chiều nhỏ hơn, robot phải thực hiện thêm một bước di chuyển cho mỗi bước còn lại ở chiều lớn hơn chiều nhỏ hơn. Điều này dẫn đến công thức`2 * max(x, y) - 1`nếu như`x != y`, Và`x + y`nếu như`x == y`. 

Bảng dưới đây tóm tắt các phương pháp tiếp cận: 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^(x+y)) | O(x+y) | Quá chậm | 
| Tối ưu | O(1) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case$t$. 
2. Đối với mỗi trường hợp thử nghiệm, hãy đọc tọa độ đích$x$Và$y$. 
3. Nếu cả hai$x$Và$y$bằng 0, xuất 0 và tiếp tục, vì không cần di chuyển. 
4. Xác định tọa độ nhỏ hơn$a = \min(x, y)$và tọa độ lớn hơn$b = \max(x, y)$. 
5. Nếu$x$Và$y$bằng nhau, đầu ra$x + y$, bởi vì chúng ta có thể luân phiên di chuyển một cách hoàn hảo mà không cần thêm bất kỳ bước nào. 
6. Nếu$x$Và$y$không bằng nhau, đầu ra`2 * b - 1`. Điều này tính đến các bước di chuyển xen kẽ lên tọa độ nhỏ hơn, cộng với các bước bổ sung cần thiết để hoàn thành các bước di chuyển còn lại ở tọa độ lớn hơn mà không lặp lại lệnh cuối cùng. 
7. Lặp lại cho tất cả các trường hợp thử nghiệm. 

Điều bất biến là robot luôn luân phiên các hướng cho đến khi hết một tọa độ. Bất kỳ bước di chuyển nào còn lại trong tọa độ lớn hơn đều yêu cầu xen kẽ một bước dừng hoặc chuyển hướng ngược lại, thêm chính xác một lệnh bổ sung cho mỗi bước còn lại. Logic này đảm bảo số lượng lệnh tối thiểu trong khi vẫn tôn trọng quy tắc "không lặp lại liên tiếp". 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    x, y = map(int, input().split())
    if x == 0 and y == 0:
        print(0)
        continue
    a, b = min(x, y), max(x, y)
    if x == y:
        print(x + y)
    else:
        print(2 * b - 1)
```Đoạn mã đầu tiên xử lý trường hợp tầm thường của`(0, 0)`để tránh những tính toán không cần thiết. Sau đó, nó tính toán tọa độ tối thiểu và tối đa để quyết định xem đường dẫn có thể được thay thế hoàn hảo hay không. Nếu tọa độ bằng nhau thì tổng sẽ cho số bước di chuyển tối thiểu. Ngược lại ta áp dụng công thức dẫn xuất`2 * max(x, y) - 1`. 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 1:**`(5, 5)`| x | y | phút | tối đa | x==y | Đầu ra | 
| --- | --- | --- | --- | --- | --- | 
| 5 | 5 | 5 | 5 | Đúng | 10 | 

Di chuyển xen kẽ hoàn hảo dọc theo cả hai trục mang lại`N E N E ...`10 động tác. 

**Đầu vào mẫu 2:**`(3, 4)`| x | y | phút | tối đa | x==y | Đầu ra | 
| --- | --- | --- | --- | --- | --- | 
| 3 | 4 | 3 | 4 | Sai | 7 | 

Chúng ta xen kẽ 3 bước di chuyển dọc theo cả hai trục:`N E N E N E`(6 nước đi), sau đó thêm một nước đi dọc theo trục lớn hơn (phía đông) mà không lặp lại, tổng cộng là 7 nước đi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi trường hợp thử nghiệm được xử lý trong thời gian không đổi. | 
| Không gian | O(1) | Chỉ một vài biến được lưu trữ cho mỗi trường hợp thử nghiệm. | 

Cho mức tối đa$t = 100$, thuật toán thực hiện tối đa vài trăm thao tác, trong giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    t = int(input())
    for _ in range(t):
        x, y = map(int, input().split())
        if x == 0 and y == 0:
            print(0)
            continue
        a, b = min(x, y), max(x, y)
        if x == y:
            print(x + y)
        else:
            print(2 * b - 1)
    return output.getvalue().strip()

# provided samples
assert run("5\n5 5\n3 4\n7 1\n0 0\n2 0\n") == "10\n7\n13\n0\n3", "sample 1"

# custom cases
assert run("3\n0 1\n1 0\n10000 10000\n") == "1\n1\n20000", "edge moves and max equal"
assert run("2\n0 0\n1 1\n") == "0\n2", "origin and minimal equal"
assert run("2\n10000 0\n0 10000\n") == "19999\n19999", "max single axis moves"
assert run("1\n2 3\n") == "5", "unequal small coordinates"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 0 1 | 1 | di chuyển tối thiểu một trục | 
| 1 0 | 1 | di chuyển tối thiểu một trục | 
| 10000 10000 | 20000 | tọa độ lớn bằng nhau | 
| 0 0 | 0 | đã có mặt tại nơi xuất xứ | 
| 1 1 | 2 | tọa độ bằng nhau tối thiểu | 
| 10000 0 | 19999 | chuyển động trục đơn lớn | 
| 0 10000 | 19999 | chuyển động trục đơn lớn | 
| 2 3 | 5 | tọa độ nhỏ không bằng nhau | 

## Vỏ cạnh 

Khi robot chỉ phải di chuyển dọc theo một trục, quy tắc xen kẽ buộc phải chèn lệnh dừng giữa các lần di chuyển lặp lại. Đối với đầu vào`(2, 0)`,`a = 0`Và`b = 2`. Công thức cho`2 * 2 - 1 = 3`, tương ứng với trình tự`E 0 E`. Vì`(0, 0)`, đầu ra là 0 vì không cần lệnh. Đối với tọa độ lớn bằng nhau như`(10000, 10000)`, các chuyển động xen kẽ dọc theo cả hai trục mang lại kết quả chính xác`x + y = 20000`, hiển thị thang đo thuật toán một cách chính xác. Mỗi trường hợp cạnh tôn trọng sự bất biến rằng robot không bao giờ lặp lại lệnh liên tiếp trong khi sử dụng số bước tối thiểu.
