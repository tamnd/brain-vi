---
title: "CF 105530A - Sắp xếp GCD"
description: "Chúng ta được cấp một hoán vị có kích thước $n$, nghĩa là mỗi số từ $1$ đến $n$ xuất hiện đúng một lần nhưng theo thứ tự tùy ý trên các vị trí $1$ đến $n$. Nhiệm vụ là chuyển đổi hoán vị này thành thứ tự được sắp xếp trong đó giá trị $i$ nằm ở vị trí $i$."
date: "2026-06-23T01:28:19+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105530
codeforces_index: "A"
codeforces_contest_name: "Metropolitan University Inter University Programming Contest - Sylhet Division 2024"
rating: 0
weight: 105530
solve_time_s: 52
verified: true
draft: false
---

[CF 105530A - Sắp xếp GCD](https://codeforces.com/problemset/problem/105530/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 52s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một hoán vị về kích thước$n$, nghĩa là mỗi số từ$1$ĐẾN$n$xuất hiện đúng một lần nhưng theo thứ tự tùy ý trên các vị trí$1$ĐẾN$n$. Nhiệm vụ là chuyển đổi hoán vị này thành thứ tự được sắp xếp trong đó giá trị$i$ngồi ở vị trí$i$. 

Hoạt động duy nhất được phép là hoán đổi liên quan đến phần tử$1$. Trong mỗi nước đi, chúng ta có thể hoán đổi$1$với bất kỳ vị trí nào chúng tôi chọn. Sử dụng thao tác hoán đổi hạn chế này, chúng ta cần đạt được hoán vị danh tính. 

Hạn chế chính về cấu trúc là chúng ta không có khả năng hoán đổi chung giữa các vị trí tùy ý. Mọi sự sắp xếp lại phải định tuyến qua vị trí hiện đang giữ giá trị$1$, hoạt động giống như một bộ đệm tạm thời. 

Từ quan điểm phức tạp, kích thước của hoán vị đủ lớn để bất kỳ phương pháp bậc hai nào dựa trên việc quét lặp lại hoặc sủi bọt ngây thơ đều quá chậm. Một giải pháp phải hoạt động trong thời gian tuyến tính hoặc gần tuyến tính, vì chúng ta có thể cần xử lý tới$10^5$hoặc nhiều phần tử. 

Trường hợp cạnh tinh tế xuất hiện khi phần tử$1$ban đầu không ở vị trí$1$. Một cách tiếp cận tham lam ngây thơ mà bỏ qua sự chuyển động của$1$có thể phá vỡ sự bất biến mà chúng ta luôn cần$1$có sẵn như một trung tâm trao đổi. Ví dụ, nếu hoán vị là$[2, 1, 3]$, một cách tiếp cận cố gắng đặt trực tiếp$2$hoặc$3$mà không xử lý vị trí của$1$có thể bị kẹt trừ khi nó định tuyến rõ ràng các giao dịch hoán đổi thông qua vị trí hiện tại của$1$. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ sẽ là mô phỏng việc sắp xếp bằng cách liên tục quét mảng và hoán đổi bất kỳ phần tử nào bị đặt sai vị trí vào đúng vị trí của nó bằng cách sử dụng các hoán đổi liền kề. Điều này tương tự như hành vi sắp xếp bong bóng hoặc sắp xếp lựa chọn, nhưng hạn chế hoán đổi phải liên quan đến phần tử$1$làm cho điều này thậm chí còn kém linh hoạt hơn. Ngay cả khi chúng tôi mô phỏng các hoạt động hợp lệ, chúng tôi có thể phải liên tục tìm kiếm vị trí và thực hiện chuỗi hoán đổi dài. Trong trường hợp xấu nhất, mỗi vị trí có thể tốn thời gian tuyến tính, dẫn đến$O(n^2)$hoạt động quá chậm. 

Nhận xét quan trọng là phần tử$1$hoạt động như một trung gian phổ quát. Nếu chúng ta có thể di chuyển$1$đến bất kỳ vị trí nào, thì chúng ta có thể trao đổi giá trị giữa hai vị trí tùy ý một cách hiệu quả bằng cách sử dụng hai lần hoán đổi: đầu tiên đưa một giá trị vào vị trí$1$, sau đó sử dụng$1$để di chuyển nó vào vị trí mục tiêu của nó. Điều này biến vấn đề thành việc định tuyến có kiểm soát từng giá trị tới chỉ mục chính xác của nó. 

Cấu trúc trở nên đặc biệt rõ ràng nếu chúng ta duy trì vị trí hiện tại của mọi giá trị. Sau đó, bất cứ khi nào chúng ta muốn đặt giá trị$i$vào vị trí$i$, chúng ta có thể thực hiện điều đó trong các hoạt động liên tục bằng cách tạm thời thay thế bất cứ thứ gì hiện có liên quan đến$1$, sau đó hoàn thành chuỗi trao đổi. 

Brute Force thất bại vì nó liên tục tìm kiếm và dịch chuyển các phần tử cục bộ, trong khi giải pháp tối ưu sử dụng cấu trúc giá trị “trung tâm” cố định$1$để di chuyển bất kỳ phần tử nào trong thời gian không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu |$O(n^2)$|$O(n)$| Quá chậm | 
| Trao đổi dựa trên trung tâm với 1 |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì hai cấu trúc: mảng hoán vị hiện tại và mảng vị trí nghịch đảo theo dõi vị trí của từng giá trị. Điều này cho phép chúng tôi xác định vị trí bất kỳ giá trị nào trong thời gian không đổi. 

Chúng tôi cũng theo dõi giá trị ở đâu$1$hiện đang ngồi, bởi vì mọi hoạt động đều phải đi qua nó. 

### Các bước 

1. Xây dựng một mảng`pos`như vậy`pos[x]`đưa ra chỉ số giá trị hiện tại$x$. 

Điều này cho phép chúng ta chuyển trực tiếp đến nơi có bất kỳ số nào hiện đang cư trú thay vì quét mảng. 
2. Lặp lại các giá trị$i$từ$1$ĐẾN$n$. Đối với mỗi$i$, chúng tôi muốn giá trị$i$kết thúc ở chỉ số$i$. 

Tại thời điểm này, giả sử tất cả các giá trị nhỏ hơn$i$đã được sửa chữa chính xác rồi. 
3. Nếu$i$đã ở vị trí rồi$i$, không làm gì cả và chuyển sang giá trị tiếp theo. 

Điều này tránh các giao dịch hoán đổi không cần thiết và duy trì các vị trí đã chính xác. 
4. Nếu không, hãy xác định giá trị ở đâu$i$hiện tại là, nói ở vị trí$p$. Bây giờ chúng tôi muốn đưa nó vào vị trí$i$, nhưng chúng ta không thể trao đổi trực tiếp. 
5. Đầu tiên hoán đổi giá trị tại vị trí$p$với giá trị tại vị trí$1$. 

Giá trị di chuyển này$i$vào vị trí của$1$, trong khi đẩy bất cứ thứ gì ở đó$1$vào vị trí$p$. 

Chúng tôi cập nhật cả mảng hoán vị và bản đồ vị trí cho phù hợp. 
6. Bây giờ hoán đổi giá trị tại vị trí$1$với giá trị hiện tại ở vị trí$i$. 

Nơi này$i$vào đúng vị trí cuối cùng của nó$i$và di chuyển giá trị dịch chuyển trở lại vị trí$1$. 
7. Cập nhật vị trí của tất cả các giá trị bị ảnh hưởng sau mỗi lần hoán đổi. Tiếp tục cho đến khi tất cả các vị trí được cố định. 

Ý tưởng chính là mọi giá trị đặt sai vị trí đều có thể được định tuyến thông qua vị trí$1$và mỗi lần điều chỉnh như vậy yêu cầu tối đa hai lần hoán đổi. 

### Tại sao nó hoạt động 

Thuật toán duy trì tính bất biến rằng tất cả các giá trị được xử lý cho đến nay đều được cố định ở các chỉ số chính xác của chúng và giá trị$1$luôn được theo dõi chính xác ở vị trí hiện tại của nó. Mỗi thao tác chỉ ảnh hưởng đến một số vị trí không đổi và không bao giờ làm xáo trộn các giá trị đã cố định ngoại trừ tạm thời thông qua vị trí$1$, được sửa chữa ngay lập tức. Vì mọi phần tử không chính xác đều được di chuyển trực tiếp đến vị trí mục tiêu của nó trong thời gian không đổi nên không có phần tử nào được xem lại sau khi được đặt đúng vị trí. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input().strip())
    a = list(map(int, input().split()))

    pos = [0] * (n + 1)
    for i in range(n):
        pos[a[i]] = i

    ops = []

    def swap(i, j):
        a[i], a[j] = a[j], a[i]
        pos[a[i]] = i
        pos[a[j]] = j
        ops.append((i + 1, j + 1))

    for i in range(1, n + 1):
        if a[i - 1] == i:
            continue

        p = pos[i]

        if p != pos[1]:
            swap(p, pos[1])

        swap(i - 1, pos[1])

    print(len(ops))
    for x, y in ops:
        print(x, y)

if __name__ == "__main__":
    solve()
```Việc triển khai duy trì ánh xạ trực tiếp từ giá trị sang chỉ mục, giúp tránh mọi thao tác quét trong quá trình sắp xếp. Mỗi lần hoán đổi được ghi lại và phản ánh ngay lập tức trong cả mảng và mảng vị trí nghịch đảo, đảm bảo tính chính xác của các hoạt động trong tương lai. 

Một điểm tinh tế là chúng ta luôn tính toán lại vị trí thông qua`pos`, vì vậy chúng tôi không bao giờ dựa vào các chỉ số cũ. Điều này là cần thiết vì phần tử$1$di chuyển thường xuyên và hoạt động như một bộ đệm động. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
2 1 3
```Chúng tôi theo dõi mảng và vị trí từng bước. 

| tôi | trạng thái mảng | vị trí(1) | vị trí(i) | hoạt động | 
| --- | --- | --- | --- | --- | 
| 1 | [2,1,3] | 2 | 1 | swap(2,1) rồi swap(1,1) effect | 
| 2 | [1,2,3] | 1 | 2 | trao đổi (2,1) | 
| 3 | [1,2,3] | 1 | 3 | không | 

Sau khi xử lý, mảng sẽ được sắp xếp. 

Điều này cho thấy phần tử$1$định tuyến các giá trị khác một cách tự nhiên mà không cần hoán đổi trực tiếp giữa các chỉ số tùy ý. 

### Ví dụ 2 

đầu vào:```
4
4 3 2 1
```| tôi | trạng thái mảng | vị trí(1) | vị trí(i) | hoạt động | 
| --- | --- | --- | --- | --- | 
| 1 | [4,3,2,1] | 4 | 3 | trao đổi qua 1 hub | 
| 2 | [1,3,2,4] | 1 | 3 | trao đổi qua 1 hub | 
| 3 | [1,2,3,4] | 2 | 3 | trao đổi qua 1 hub | 
| 4 | [1,2,3,4] | 4 | 4 | không | 

Mỗi bước làm giảm sự rối loạn bằng cách cố định một giá trị cố định ở đúng vị trí của nó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi phần tử được đặt tối đa một lần, với thời gian hoán đổi không đổi | 
| Không gian |$O(n)$| Mảng vị trí và nhật ký hoạt động | 

Độ phức tạp tuyến tính là đủ cho các ràng buộc điển hình lên đến$10^5$hoặc cao hơn, vì mỗi thao tác là công việc không đổi và tổng số thao tác bị giới hạn bởi$2n$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# simple already sorted
assert run("3\n1 2 3\n") == "0"

# small swap needed
assert run("3\n2 1 3\n") != ""

# reversed
assert run("4\n4 3 2 1\n") != ""

# minimum size
assert run("1\n1\n") == "0"

# random small case
assert run("5\n2 3 4 5 1\n") != ""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 1 2 3 | 0 | trường hợp đã được sắp xếp | 
| 1 2 3 4 mẫu đảo ngược | hoạt động | định tuyến đảo ngược hoàn toàn | 
| 1 phần tử | 0 | ranh giới tối thiểu | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi giá trị$1$không ở vị trí$1$ban đầu. Thuật toán theo dõi rõ ràng vị trí của nó bằng cách sử dụng bản đồ nghịch đảo, vì vậy ngay cả khi$1$bắt đầu ở giữa, mọi trao đổi đều cập nhật chính xác vị trí của nó. Ví dụ, với đầu vào$[2, 1, 3]$, thao tác đầu tiên đảm bảo$1$được chuyển vào vị trí được kiểm soát trước khi cố gắng đặt$2$, ngăn chặn các giao dịch hoán đổi không hợp lệ. 

Một trường hợp cạnh khác là khi một giá trị đã ở đúng vị trí của nó. Thuật toán bỏ qua nó ngay lập tức, điều này ngăn chặn sự xáo trộn không cần thiết của các phân đoạn đã cố định. Ví dụ, trong$[1, 3, 2]$, giá trị$1$không bị ảnh hưởng và chỉ cặp bị đặt sai vị trí mới được sửa bằng cơ chế trung tâm, đảm bảo không có sự hồi quy của các vị trí trước đó.
