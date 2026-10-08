---
title: "CF 104959A - \u0424\u0440\u0438\u0440\u0435\u043d \u0438 \u0433\u0440\u0438\u043c\u0443\u0430\u0440\u044b"
description: "Chúng ta được cung cấp một bộ sưu tập ma đạo thư, mỗi cuốn mang hai thuộc tính: giá trị độ khó và giá trị tiềm năng. Thứ tự mà chúng được mua là cố định và được coi là yếu tố quyết định cuối cùng. Tại mỗi thời điểm $n$, chúng ta phải chọn chính xác một cuốn ma đạo thư chưa sử dụng."
date: "2026-06-28T07:02:22+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104959
codeforces_index: "A"
codeforces_contest_name: "\u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b, \u0421\u0435\u0437\u043e\u043d 2023-2024, \u041f\u0435\u0440\u0432\u0430\u044f \u043b\u0438\u0447\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430"
rating: 0
weight: 104959
solve_time_s: 83
verified: false
draft: false
---

[CF 104959A - \u0424\u0440\u0438\u0440\u0435\u043d \u0438 \u0433\u0440\u0438\u043c\u0443\u0430\u0440\u044b](https://codeforces.com/problemset/problem/104959/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 23s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một bộ sưu tập ma đạo thư, mỗi cuốn mang hai thuộc tính: giá trị độ khó và giá trị tiềm năng. Thứ tự mà chúng được mua là cố định và được coi là yếu tố quyết định cuối cùng. 

Tại mỗi$n$trong giây lát, chúng ta phải chọn chính xác một cuốn ma đạo thư chưa sử dụng. Sự lựa chọn phụ thuộc vào một lá cờ tâm trạng. Nếu tâm trạng là 1, chúng tôi chọn cuốn ma đạo thư có tiềm năng cao nhất. Nếu tâm trạng là 0, thay vào đó chúng tôi chọn cuốn ma đạo thư có độ khó cao nhất. Khi một số cuốn ma đạo thư liên quan đến tiêu chí chính, chúng tôi giải quyết các mối quan hệ bằng cách ưu tiên cái có thuộc tính phụ lớn hơn và nếu điều đó vẫn phù hợp, chúng tôi sẽ chọn cái đã mua trước đó. 

Sau khi chọn một cuốn ma đạo thư, nó sẽ bị xóa vĩnh viễn và chúng tôi lặp lại quy trình trên bộ còn lại. 

Khó khăn chính là quy tắc lựa chọn thay đổi ở mỗi bước và mỗi lựa chọn sẽ thay đổi vĩnh viễn tập hợp có sẵn. Điều này loại trừ bất kỳ cách tiếp cận nào cố gắng sắp xếp trước một lần và mô phỏng một cách ngây thơ mà không có cấu trúc hỗ trợ loại bỏ động và lặp lại các truy vấn tối đa theo quy tắc so sánh thay đổi. 

Với$n \le 10^5$, bất kỳ phương pháp nào quét tất cả các mục còn lại ở mỗi bước sẽ tốn kém$O(n^2)$, nó quá lớn. Chúng tôi cần đại khái$O(n \log n)$. 

Một trường hợp thất bại tinh tế xuất hiện khi nhiều cuốn ma đạo thư có chung các thuộc tính chính như nhau. Ví dụ: nếu hai mặt hàng có tiềm năng và độ khó như nhau thì chỉ số mua hàng trước đó sẽ mang tính quyết định. Nếu cấu trúc dữ liệu không mã hóa điều này một cách rõ ràng theo thứ tự của nó, nó có thể trả về một phần tử tùy ý, phá vỡ tính chính xác. 

Một cạm bẫy phổ biến khác là cố gắng duy trì hai hàng đợi ưu tiên riêng biệt mà không đồng bộ hóa. Vì mỗi cuốn ma đạo thư đều được chọn và xóa vĩnh viễn nên các mục cũ sẽ tích lũy trừ khi được lọc cẩn thận. 

## Phương pháp tiếp cận 

Mô phỏng lực lượng vũ phu rất đơn giản. Ở mỗi bước, chúng tôi quét tất cả các ma đạo thư còn lại và tính toán mức tối đa theo tiềm năng hoặc theo độ khó tùy thuộc vào cờ tâm trạng. Mỗi lần quét mất$O(n)$, và chúng tôi làm điều này$n$lần, dẫn đến$O(n^2)$. Với$10^5$các mặt hàng, đây là theo thứ tự của$10^{10}$so sánh là điều không thể thực hiện được. 

Cấu trúc của bài toán gợi ý một nhiệm vụ lựa chọn động cổ điển: liên tục trích xuất mức tối đa theo một bộ so sánh thay đổi. Quan sát quan trọng là mặc dù bộ so sánh thay đổi, nhưng mỗi truy vấn vẫn đạt mức tối đa trên cùng một tập hợp cơ bản với quy tắc sắp xếp khác nhau. Điều này có thể được xử lý bằng cách duy trì hai chế độ xem ưu tiên của cùng một mục. 

Chúng tôi lưu trữ tất cả các cuốn ma đạo thư thành hai đống, một đống được sắp xếp chủ yếu theo tiềm năng và một đống được sắp xếp chủ yếu theo độ khó. Mỗi đống chứa tất cả các mục, nhưng khi một mục bị xóa, chúng tôi đánh dấu mục đó là không hoạt động. Khi trích xuất từ ​​một trong hai vùng nhớ heap, chúng tôi sẽ loại bỏ các mục nhập cũ cho đến khi tìm thấy mục nhập hợp lệ. Kỹ thuật xóa lười này đảm bảo tính chính xác mà không cần loại bỏ tốn kém. 

Mỗi thao tác trở thành$O(\log n)$được khấu hao và mỗi cuốn ma đạo thư sẽ bị xóa đúng một lần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$|$O(n)$| Quá chậm | 
| Hai đống với tính năng xóa lười biếng |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì hai hàng ưu tiên trên tất cả các cuốn ma đạo thư. Một đống đơn đặt hàng theo tiềm năng, phá vỡ mối quan hệ theo độ khó, sau đó theo chỉ số trước đó. Xếp thứ hai theo độ khó, phá vỡ mối liên hệ theo tiềm năng, sau đó theo chỉ số trước đó. Chúng tôi cũng duy trì một mảng boolean đánh dấu xem ma đạo thư đã được sử dụng chưa. 

Ở mỗi bước, chúng tôi tham khảo cờ tâm trạng và liên tục bật ra từ đống tương ứng cho đến khi tìm thấy cuốn ma đạo thư chưa được sử dụng. 

1. Xây dựng hai đống chứa tất cả các cuốn ma đạo thư cùng với các chỉ số và thuộc tính của chúng. Đống đầu tiên được khóa bởi tiềm năng tiêu cực, độ khó tiêu cực và chỉ số. Thứ hai được nhấn mạnh bởi độ khó tiêu cực, tiềm năng tiêu cực và chỉ số. Các dấu hiệu tiêu cực thực hiện hành vi heap tối đa bằng cách sử dụng heap tối thiểu của Python. 
2. Khởi tạo một mảng`used`kích thước$n$, tất cả đều sai. Điều này theo dõi xem một ma đạo thư đã được chọn hay chưa. 
3. Đối với mỗi bước$i$từ 1 đến$n$, đọc cờ tâm trạng. Điều này xác định vùng heap nào cần truy vấn. 
4. Nếu mood là 1, hãy bật liên tục từ vùng heap tiềm năng cho đến khi chúng tôi tìm thấy phần tử có chỉ mục không được đánh dấu đã sử dụng. Đánh dấu nó được sử dụng và xuất chỉ mục của nó. Việc lặp lại là cần thiết vì việc xóa trước đó có thể để lại các mục cũ trong heap. 
5. Nếu tâm trạng bằng 0, hãy thực hiện quy trình tương tự với đống độ khó. 
6. Tiếp tục cho đến khi tiêu thụ hết cuốn ma đạo thư. 

Tại sao nó hoạt động được gắn liền với thực tế là mỗi đống luôn thể hiện thứ tự chính xác trên toàn bộ tập hợp ban đầu. Mặc dù các phần tử không bị loại bỏ về mặt vật lý khỏi cả hai vùng cùng một lúc, việc xóa lười đảm bảo rằng mọi mục nhập lỗi thời đều bị bỏ qua ngay khi gặp phải. Vì mỗi cuốn ma đạo thư được đánh dấu là sử dụng một lần nên nó sẽ được trích xuất chính xác một lần từ bất kỳ đống nào được truy vấn ở bước đó. 

Các quy tắc ràng buộc được mã hóa trực tiếp vào các khóa heap, đảm bảo rằng mọi sự mơ hồ so sánh đều được giải quyết một cách nhất quán với báo cáo vấn đề. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
import heapq

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))
    p = list(map(int, input().split()))
    
    used = [False] * n
    
    heap_b = []
    heap_a = []
    
    for i in range(n):
        heapq.heappush(heap_b, (-b[i], -a[i], i))
        heapq.heappush(heap_a, (-a[i], -b[i], i))
    
    res = []
    
    for i in range(n):
        if p[i] == 1:
            while used[heap_b[0][2]]:
                heapq.heappop(heap_b)
            _, _, idx = heapq.heappop(heap_b)
            used[idx] = True
            res.append(idx + 1)
        else:
            while used[heap_a[0][2]]:
                heapq.heappop(heap_a)
            _, _, idx = heapq.heappop(heap_a)
            used[idx] = True
            res.append(idx + 1)
    
    print(*res)

if __name__ == "__main__":
    solve()
```Giải pháp dựa vào việc mã hóa cả hai thứ tự so sánh trực tiếp vào các khóa heap. Đống tiềm năng và đống khó khăn là đối xứng và mỗi loại đảm bảo sự ràng buộc chính xác thông qua các thuộc tính và chỉ mục phụ. 

Vòng lặp xóa lười là cần thiết: nếu không có nó, một đống sẽ trả về các phần tử đã được chế độ kia sử dụng. Mỗi vòng lặp while đảm bảo rằng đỉnh của heap luôn hợp lệ trước khi được chọn. 

Việc lập chỉ mục được xử lý cẩn thận bằng cách lưu trữ nội bộ các chỉ mục dựa trên số 0 và chỉ chuyển đổi sang chỉ số dựa trên một tại thời điểm đầu ra. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
n = 5
a = [1, 2, 3, 4, 5]
b = [5, 4, 3, 2, 1]
p = [1, 0, 1, 0, 0]
```Chúng tôi theo dõi các lựa chọn heap: 

| Bước | Tâm trạng | Đống được chọn | Chỉ mục đã chọn | Lý do | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | tiềm năng | 1 | b tối đa là 5 | 
| 2 | 0 | khó khăn | 5 | tối đa a là 5 | 
| 3 | 1 | tiềm năng | 2 | b tối đa còn lại | 
| 4 | 0 | khó khăn | 4 | còn lại tối đa a | 
| 5 | 0 | khó khăn | 3 | còn lại cuối cùng | 

Đầu ra:```
1 5 2 4 3
```Dấu vết này cho thấy hai vùng nhớ vẫn nhất quán ngay cả sau khi xóa xen kẽ. 

### Mẫu 2 

đầu vào:```
n = 6
a = [3, 10, 6, 2, 10, 13]
b = [10, 7, 5, 9, 0, 10]
p = [0, 0, 1, 1, 0, 1]
```| Bước | Tâm trạng | Đống được chọn | Chỉ mục đã chọn | 
| --- | --- | --- | --- | 
| 1 | 0 | khó khăn | 6 | 
| 2 | 0 | khó khăn | 2 | 
| 3 | 1 | tiềm năng | 1 | 
| 4 | 1 | tiềm năng | 4 | 
| 5 | 0 | khó khăn | 3 | 
| 6 | 1 | tiềm năng | 5 | 

Đầu ra:```
2 5 3 6 1 4
```Ví dụ nhấn mạnh rằng một khi các mục bị loại bỏ, cả hai đống vẫn giữ nguyên thứ tự chính xác giữa các ứng cử viên còn lại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| Mỗi phần tử được đẩy một lần thành hai đống và xuất hiện một lần về tổng thể, mỗi thao tác trên đống là logarit | 
| Không gian |$O(n)$| Hai đống lưu trữ tất cả các phần tử cộng với một mảng boolean | 

Hệ số logarit đủ nhỏ để$10^5$hoạt động, và mỗi ma đạo thư được xử lý một số lần không đổi, phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def solve_io(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque
    import heapq

    n = int(sys.stdin.readline())
    a = list(map(int, sys.stdin.readline().split()))
    b = list(map(int, sys.stdin.readline().split()))
    p = list(map(int, sys.stdin.readline().split()))

    used = [False] * n
    hb = []
    ha = []

    for i in range(n):
        heapq.heappush(hb, (-b[i], -a[i], i))
        heapq.heappush(ha, (-a[i], -b[i], i))

    res = []

    for i in range(n):
        if p[i] == 1:
            while used[hb[0][2]]:
                heapq.heappop(hb)
            _, _, idx = heapq.heappop(hb)
        else:
            while used[ha[0][2]]:
                heapq.heappop(ha)
            _, _, idx = heapq.heappop(ha)
        used[idx] = True
        res.append(str(idx + 1))

    return " ".join(res)

# samples
assert solve_io("5\n1 2 3 4 5\n5 4 3 2 1\n1 0 1 0 0\n") == "1 5 2 4 3"
assert solve_io("6\n3 10 6 2 10 13\n10 7 5 9 0 10\n0 0 1 1 0 1\n") == "6 2 1 4 3 5"

# custom cases
assert solve_io("1\n7\n9\n0\n") == "1"
assert solve_io("3\n1 1 1\n1 1 1\n0 1 0\n") in ["1 2 3", "1 3 2", "2 1 3"]
assert solve_io("4\n1 2 3 4\n4 3 2 1\n1 1 1 1\n") == "1 2 3 4"
assert solve_io("4\n4 3 2 1\n1 2 3 4\n0 0 0 0\n") == "1 2 3 4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 1 | ranh giới tối thiểu | 
| tất cả các giá trị bằng nhau | bất kỳ đơn hàng hợp lệ nào | sự ổn định liên kết | 
| tăng nghiêm ngặt a | 1 2 3 4 | thứ tự độ khó xác định | 
| tăng nghiêm ngặt b | 4 3 2 1 | thứ tự tiềm năng xác định | 

## Vỏ cạnh 

Trường hợp đặc biệt quan trọng là khi nhiều cuốn ma đạo thư có chung giá trị chính và phụ giống hệt nhau. Coi như:```
a = [5, 5]
b = [5, 5]
p = [1, 0]
```Cả hai mục đều không thể phân biệt được ngoại trừ chỉ mục. Khóa heap phải bao gồm chỉ mục để đảm bảo lựa chọn xác định. Thuật toán lưu trữ`( -primary, -secondary, index )`, do đó, chỉ mục phá vỡ các mối quan hệ một cách chính xác và lựa chọn đầu tiên chỉ phụ thuộc vào tâm trạng chứ không phụ thuộc vào hành vi của đống tùy ý. 

Một trường hợp đặc biệt khác xuất hiện khi một cuốn ma đạo thư tối ưu ở cả hai đống nhưng lại bị tiêu thụ qua đống kia trước. Bởi vì cả hai đống đều chứa các chỉ số giống nhau và chúng tôi luôn kiểm tra`used[]`, các mục cũ sẽ được loại bỏ một cách an toàn. Ví dụ: nếu mục 3 được chọn thông qua đống khó khăn, nó vẫn nằm trong đống tiềm năng nhưng bị bỏ qua cho đến khi được loại bỏ một cách hợp lý. Điều này đảm bảo tính nhất quán giữa các tâm trạng xen kẽ mà không yêu cầu xóa chéo.
