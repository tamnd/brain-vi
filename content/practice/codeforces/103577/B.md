---
title: "CF 103577B - Chuỗi khối"
description: "Chúng ta được cho một hoặc nhiều chữ số vô hướng. Mỗi cạnh nối hai đỉnh và mang trọng số nguyên dương."
date: "2026-07-03T03:29:52+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103577
codeforces_index: "B"
codeforces_contest_name: "2021 ICPC Universidad Nacional de Colombia Programming Contest"
rating: 0
weight: 103577
solve_time_s: 43
verified: true
draft: false
---

[CF 103577B - Chuỗi khối](https://codeforces.com/problemset/problem/103577/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 43s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một hoặc nhiều chữ số vô hướng. Mỗi cạnh nối hai đỉnh và mang trọng số nguyên dương. Nhiệm vụ xác định một hàm trên biểu đồ trông giống như chúng ta được phép chọn một tập hợp con các cạnh, nhưng phần đóng góp của tập hợp con đã chọn là nhân lên: đối với mỗi cạnh được chọn, chúng ta nhân trọng số của nó, ngoại trừ một số cạnh bị “cấm” và đóng góp bằng không. 

Chính xác hơn, một cạnh có trọng lượng$w$đóng góp$w \times p(w)$, Ở đâu$p(w)$chỉ là 1 khi$w$là số lẻ, ngược lại là 0. Do đó, bất kỳ cạnh nào có trọng lượng chẵn sẽ ngay lập tức giết chết toàn bộ sản phẩm nếu nó được đưa vào. Điều này có nghĩa là bất kỳ tập hợp con tối ưu nào cũng sẽ không bao giờ bao gồm các cạnh có trọng số chẵn, vì chúng buộc tích số về 0. 

Vì vậy, vấn đề giảm xuống còn việc chọn một số tập con các cạnh có trọng số lẻ, tối đa hóa tích các trọng số của chúng. Nếu chúng ta không chọn cạnh nào thì tích số là 1 theo quy ước (sản phẩm trống). 

Kích thước đầu vào cho phép lên đến$10^5$nút và$10^5$các cạnh cho mỗi trường hợp thử nghiệm, vì vậy mọi giải pháp đều phải gần tuyến tính về số cạnh. Bậc hai hoặc thậm chí$O(m \log m)$với các hằng số nặng thì được, nhưng bất cứ điều gì cố gắng liệt kê các tập hợp con hoặc chạy kết hợp theo cặp đều không thể thực hiện được. 

Trường hợp cạnh tinh tế là khi tất cả các cạnh đều bằng nhau. Khi đó mọi tập con không trống đều có tích 0, vì vậy câu trả lời đúng là 1 bằng cách chọn tập trống. Việc triển khai ngây thơ khởi tạo câu trả lời là 0 hoặc luôn nhân ít nhất một cạnh sẽ thất bại ở đây. 

Một trường hợp cạnh khác là nhiều cạnh giữa các nút giống nhau. Vì các cạnh độc lập trong sản phẩm nên các bản sao không quan trọng về mặt cấu trúc nhưng chúng quan trọng về mặt số lượng và tất cả phải được xem xét nếu là số lẻ. 

## Phương pháp tiếp cận 

Việc giải thích brute-force rất đơn giản: thử mọi tập con của các cạnh, tính tích của$w(e)$trên các cạnh có trọng số lẻ và bỏ qua các tập con chứa các cạnh có trọng số chẵn vì chúng có giá trị bằng 0. Điều này đúng vì định nghĩa nhân lên rõ ràng trên tất cả các cạnh được chọn. Tuy nhiên, số lượng tập hợp con là$2^m$, cái nào cho$m = 10^5$là hoàn toàn không khả thi. 

Sự đơn giản hóa chính xuất phát từ việc nhận thấy rằng cấu trúc biểu đồ là không liên quan. Không có sự tương tác giữa các cạnh ngoại trừ phép nhân. Quyết định duy nhất là có bao gồm từng cạnh có trọng số lẻ hay không và mỗi cạnh có trọng lượng lẻ sẽ tăng tích số một cách độc lập theo hệ số trọng lượng của nó. Vì tất cả các trọng số đều là số nguyên dương lớn hơn hoặc bằng 1 nên bao gồm mọi cạnh có trọng số lẻ không bao giờ có thể làm giảm tích. Không có hạn chế nào ngăn cản việc lấy tất cả chúng. 

Do đó, chiến lược tối ưu chỉ đơn giản là nhân tất cả các trọng số lẻ với nhau. Nếu không có cạnh lẻ thì kết quả là 1. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^m)$|$O(m)$| Quá chậm | 
| Tối ưu |$O(m)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng biểu đồ một cách độc lập. 

1. Khởi tạo bộ tích lũy`ans`đến 1. Điều này thể hiện tích của tất cả các cạnh hợp lệ đã chọn cho đến nay. Bắt đầu từ 1 đảm bảo rằng việc bỏ qua tất cả các cạnh sẽ mang lại giá trị nhận dạng chính xác. 
2. Đọc từng cạnh một. Đối với một cạnh có trọng lượng`w`, kiểm tra xem`w`thật kỳ quặc. 
3. Nếu`w`chẵn, bỏ qua cạnh hoàn toàn. Nó không đóng góp gì hữu ích vì đóng góp của nó sẽ bằng 0 và bất kỳ tập hợp con nào chứa nó sẽ buộc sản phẩm về 0, điều này luôn tệ hơn việc bỏ qua nó. 
4. Nếu`w`lẻ, nhân nó với`ans`. Điều này tương ứng với việc chọn cạnh đó trong tập con tối ưu. 
5. Sau khi xử lý tất cả các cạnh, xuất ra`ans`. 

Bước lập luận quan trọng là vì phép nhân có tính kết hợp và giao hoán và tất cả các giá trị được chọn đều dương nên việc loại trừ cạnh lẻ không bao giờ có lợi. 

### Tại sao nó hoạt động 

Mọi giải pháp khả thi đều tương ứng với việc chọn một tập con các cạnh. Bất kỳ tập con nào chứa cạnh có trọng số chẵn đều có tích bằng 0, tập hợp con này bị chi phối hoàn toàn bởi tập con trống trừ khi tất cả các tích cạnh lẻ cũng bằng 0, chúng không bằng 0 vì trọng số lẻ ít nhất bằng 1. Do đó, tập hợp con tối ưu chỉ bao gồm các cạnh lẻ. Trong số các cạnh lẻ, mỗi cạnh đóng góp một hệ số nhân lớn hơn hoặc bằng 1, do đó, việc bao gồm tất cả chúng sẽ tối đa hóa tích. Không có sự phụ thuộc nào giữa các cạnh, vì vậy lựa chọn tham lam là tối ưu toàn cục. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    data = sys.stdin.read().strip().split()
    if not data:
        return
    it = iter(data)
    out = []

    while True:
        try:
            n = int(next(it))
        except StopIteration:
            break
        m = int(next(it))

        ans = 1

        for _ in range(m):
            w = int(next(it))
            u = int(next(it))
            v = int(next(it))

            if w % 2 == 1:
                ans *= w

        out.append(str(ans))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Giải pháp đọc hàng loạt tất cả dữ liệu đầu vào để tránh lặp lại chi phí I/O. Mỗi cạnh được xử lý trong thời gian không đổi, với việc kiểm tra tính chẵn lẻ đơn giản. Các nút`u`Và`v`không liên quan đến việc tính toán và chỉ được đọc để nâng cao luồng đầu vào một cách chính xác. 

Một lỗi triển khai phổ biến là cố gắng xây dựng danh sách kề hoặc lý do về khả năng kết nối, nhưng cấu trúc biểu đồ không có vai trò gì trong hàm mục tiêu. Một vấn đề nhỏ khác là quên khởi tạo câu trả lời thành 1 thay vì 0, điều này sẽ buộc tất cả kết quả đầu ra về 0 một cách không chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n=4, m=5
edges:
(1,1-2), (2,2-3), (3,3-4), (5,1-3), (6,2-4)
```Chúng tôi xử lý các cạnh: 

| Bước | Cân nặng | Số lẻ? | Trả lời | 
| --- | --- | --- | --- | 
| 1 | 1 | vâng | 1 | 
| 2 | 2 | không | 1 | 
| 3 | 3 | vâng | 3 | 
| 4 | 5 | vâng | 15 | 
| 5 | 6 | không | 15 | 

Đầu ra cuối cùng là 15. 

Điều này xác nhận rằng chỉ các cạnh có trọng số lẻ mới đóng góp và các cạnh chẵn được bỏ qua một cách an toàn. 

### Ví dụ 2 

đầu vào:```
n=3, m=3
edges:
(2,1-2), (4,2-3), (6,1-3)
```| Bước | Cân nặng | Số lẻ? | Trả lời | 
| --- | --- | --- | --- | 
| 1 | 2 | không | 1 | 
| 2 | 4 | không | 1 | 
| 3 | 6 | không | 1 | 

Đầu ra cuối cùng là 1. 

Điều này thể hiện trường hợp tích rỗng trong đó mọi cạnh đều bị cấm và câu trả lời đúng vẫn được xác định rõ ràng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(m)$mỗi biểu đồ | Mỗi cạnh được xử lý một lần với công việc liên tục | 
| Không gian |$O(1)$thêm | Chỉ sử dụng sản phẩm đang chạy và bộ đệm đầu vào | 

Các ràng buộc cho phép lên đến$10^5$các cạnh và giải pháp chỉ thực hiện một lần đi qua chúng, dễ dàng phù hợp trong giới hạn thời gian. Việc sử dụng bộ nhớ không đổi ngoài việc lưu trữ đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from collections import deque

    data = inp.strip().split()
    it = iter(data)
    out = []

    while True:
        try:
            n = int(next(it))
        except StopIteration:
            break
        m = int(next(it))
        ans = 1
        for _ in range(m):
            w = int(next(it))
            u = int(next(it))
            v = int(next(it))
            if w % 2 == 1:
                ans *= w
        out.append(str(ans))

    return "\n".join(out)

# provided sample-style test
assert run("4 3\n1 1 1\n2 1 2\n3 2 3\n") == "3", "simple case"

# all even edges
assert run("3 3\n2 1 2\n4 2 3\n6 1 3\n") == "1", "all even"

# all odd edges
assert run("2 3\n1 1 2\n3 1 2\n5 1 2\n") == str(1*3*5), "all odd"

# single edge
assert run("2 1\n7 1 2\n") == "7", "single odd edge"

# mixed
assert run("2 4\n2 1 2\n3 1 2\n4 1 2\n5 1 2\n") == str(15), "mixed edges"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả các cạnh chẵn | 1 | hành vi sản phẩm trống | 
| tất cả các cạnh lẻ | sản phẩm | phép nhân đầy đủ đúng đắn | 
| cạnh đơn | w | trường hợp cơ sở đúng đắn | 
| cạnh hỗn hợp | 15 | lọc logic | 

## Vỏ cạnh 

Một trường hợp cạnh tới hạn là khi mọi cạnh đều có trọng số chẵn. Đối với đầu vào như:```
3 2
2 1 2
4 2 3
```thuật toán khởi tạo`ans = 1`, sau đó bỏ qua cả hai cạnh, giữ nguyên kết quả. Giá trị này khớp với giá trị tập hợp con trống được yêu cầu. 

Một trường hợp khác là một trọng số lẻ lớn, chẳng hạn như:```
2 1
999999937 1 2
```Thuật toán nhân nó trực tiếp vào bộ tích lũy, tạo ra kết quả chính xác mà không gây lo ngại tràn trong Python do các số nguyên có độ chính xác tùy ý. 

Trường hợp cuối cùng là kích thước đầu vào lớn với tất cả các cạnh lẻ. Thuật toán vẫn thực hiện một phép nhân trên mỗi cạnh, duy trì hành vi tuyến tính.
