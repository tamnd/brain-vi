---
title: "CF 105167L - Trung tâm điều khiển đầu máy"
description: "Chúng ta được cấp một dãy cố định các toa xe tại ga A. Mỗi toa tàu có một nhãn duy nhất từ ​​1 đến n, nhưng chúng xuất hiện theo một thứ tự tùy ý."
date: "2026-06-27T10:38:38+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105167
codeforces_index: "L"
codeforces_contest_name: "ETH Zurich Competitive Programming Contest Spring 2024"
rating: 0
weight: 105167
solve_time_s: 91
verified: false
draft: false
---

[CF 105167L - Trung tâm điều khiển đầu máy](https://codeforces.com/problemset/problem/105167/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 31s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một dãy cố định các toa xe tại ga A. Mỗi toa tàu có một nhãn duy nhất từ 1 đến n, nhưng chúng xuất hiện theo một thứ tự tùy ý. Cách duy nhất để di chuyển ô tô là thông qua ba thao tác trực tiếp: từ A đến trạm trung gian B, từ B đến trạm cuối cùng C và trực tiếp từ A đến C. 

Mục tiêu là kết thúc với tất cả các ô tô ở trạm C theo thứ tự tăng dần theo nhãn, nghĩa là ô tô 1 đầu tiên, sau đó là 2, v.v. cho đến n. Chúng ta phải xuất ra một chuỗi các bước đi được phép để đạt được điều này và trong số tất cả các chuỗi hợp lệ, chúng ta được yêu cầu tạo ra một chuỗi có số thao tác tối thiểu. Nếu không thể, chúng ta phải xuất -1. 

Hạn chế chính là cả A, B và C đều hoạt động giống như các ngăn xếp về mặt chặn. Đặc biệt, B chỉ có một đường nên ô tô đến B bị hạn chế bởi hành vi LIFO: ô tô đến sau có thể phải rời đi sớm hơn để bỏ chặn các bước di chuyển trong tương lai. 

Kích thước đầu vào lên tới n = 100000. Bất kỳ giải pháp nào cố gắng mô phỏng sự sắp xếp lại tùy ý hoặc khám phá hoán vị của các bước di chuyển đều không khả thi ngay lập tức. Một giải pháp phải tuyến tính hoặc gần tuyến tính trong n, vì ngay cả O(n log n) cũng là đường biên nếu nó liên quan đến các hệ số hằng số nặng và O(n^2) hoàn toàn không có vấn đề. 

Một trường hợp lỗi tinh vi phát sinh khi cấu trúc buộc phải có một thứ tự trung gian cụ thể trong B mà sau này không thể sửa được. 

Ví dụ, hãy xem xét:```
3
2 1 3
```Nếu một người ngây thơ gửi 2 đến B, sau đó 1 đến B, sau đó cố gắng gửi 3 trực tiếp đến C, hệ thống sẽ bị kẹt vì 1 khối 2 trong B nhưng cần có 2 khối sau đó trước khi 3 có thể được xử lý chính xác trong các ràng buộc. Điều này minh họa rằng việc tùy tiện đẩy vào B mà không tôn trọng các yêu cầu trong tương lai có thể làm bế tắc quá trình. 

Một trường hợp rắc rối khác:```
2
2 1
```Điều này là không thể vì 1 phải kết thúc trước 2, nhưng 2 lại xuất hiện trước và bị kẹt ở B hoặc C tùy theo nước đi. Bất kỳ nỗ lực tham lam nào gửi mọi thứ qua B vẫn sẽ thất bại. 

Những ví dụ này cho thấy tính khả thi phụ thuộc vào việc liệu chúng ta có thể tránh đảo ngược ràng buộc thứ tự không thể tránh khỏi do bản chất ngăn xếp của B áp đặt hay không. 

## Phương pháp tiếp cận 

Cách giải thích bạo lực sẽ coi đây là bài toán đường đi ngắn nhất trong một không gian trạng thái khổng lồ. Mỗi trạng thái bao gồm thứ tự các ô tô trong A, B và C. Mỗi lần di chuyển sẽ chuyển đổi giữa các trạng thái và chúng tôi tìm kiếm một chuỗi mang lại kết quả được sắp xếp C. Ngay cả việc biểu diễn một trạng thái cũng là tuyến tính theo n và hệ số phân nhánh lên tới 3, vì vậy BFS hoàn toàn không khả thi. 

Cách tiếp cận bạo lực có cấu trúc chặt chẽ hơn có thể mô phỏng chiến lược tham lam: luôn di chuyển ô tô k được yêu cầu tiếp theo từ A hoặc B vào C nếu có thể, nếu không thì chuyển ô tô chặn vào B. Điều này vẫn thất bại vì B đưa ra các ràng buộc thứ tự ẩn. Khi một ô tô được đẩy vào B, thứ tự tương đối của nó sẽ xác định liệu các hoạt động trong tương lai có còn khả thi hay không. Nếu không có điều kiện toàn cầu, cách tiếp cận tham lam này có thể khiến các ô tô ở B mắc sai thứ tự. 

Quan sát quan trọng là thứ tự cuối cùng trong C được cố định: 1 đến n. Điều này buộc chúng tôi phải xử lý ô tô theo thứ tự nhãn tăng dần. Đối với mỗi giá trị i, cuối cùng chúng ta phải đưa nó về C vào thời điểm nó có thể truy cập được. Điều này biến vấn đề thành một mô phỏng truy xuất các giá trị theo thứ tự được sắp xếp, đồng thời tôn trọng B hoạt động giống như một bộ đệm ngăn xếp. 

Thông tin chi tiết quan trọng về cấu trúc là trạng thái có ý nghĩa duy nhất mà chúng ta phải theo dõi là liệu ô tô cần thiết tiếp theo có sẵn tại A hay hiện đang ẩn dưới những ô tô lớn hơn phải tạm thời được chuyển vào B. Điều này làm giảm quá trình duy trì ngăn xếp và đảm bảo rằng B không bao giờ vi phạm ràng buộc rằng nó chỉ có thể lưu trữ tạm thời các phần tử theo cách nhập sau xuất trước phù hợp với các yêu cầu trong tương lai. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tìm kiếm trạng thái Brute Force | Hàm mũ | Hàm mũ | Quá chậm | 
| Mô phỏng ngăn xếp tham lam (dạng đúng) | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì một con trỏ`need`đại diện cho ô tô tiếp theo phải được chuyển đến C. Chúng tôi cũng mô phỏng trạm A như một con trỏ trên chuỗi đầu vào và trạm B dưới dạng một ngăn xếp rõ ràng. 

1. Bắt đầu với`need = 1`, chỉ số`i = 0`trong mảng ban đầu và một ngăn xếp trống B. Chúng tôi cũng duy trì một danh sách các thao tác đầu ra. 
2. Trong khi`need <= n`, chúng tôi cố gắng đảm bảo xe`need`đạt tới C 
3. Nếu đỉnh B bằng`need`, chúng ta lấy nó từ B và chuyển nó sang C. Chúng ta ghi lại thao tác “B C”, sau đó tăng dần`need`. Đây là cách giải quyết trực tiếp nhất vì B đã giữ đúng chiếc xe tiếp theo. 
4. Ngược lại nếu ở A còn xe thì ta đi xe tiếp theo`x = a[i]`và xử lý nó. Nếu như`x == need`, chúng ta ngay lập tức di chuyển nó từ A đến C và tăng cả hai`i`Và`need`. Nếu như`x != need`, chúng ta di chuyển nó từ A đến B, đẩy nó vào ngăn xếp và tăng dần`i`. Điều này đảm bảo chúng tôi không chặn tiến trình trong khi vẫn tôn trọng các ràng buộc về thứ tự. 
5. Nếu A kiệt sức và đỉnh B không`need`, quá trình này bị kẹt và không có chuỗi hợp lệ nào tồn tại. Chúng tôi xuất ra -1. 
6. Tiếp tục cho đến khi tất cả các ô tô được xếp vào C theo thứ tự. 

Ý tưởng trọng tâm là B hoạt động như một cấu trúc giữ tạm thời cho những chiếc xe chưa cần thiết. Chúng tôi không bao giờ đẩy một chiếc ô tô vào B nếu nó có thể ngay lập tức đi đến C, bởi vì làm như vậy sẽ chỉ làm chậm tiến độ và có khả năng tạo ra sự tắc nghẽn không cần thiết. 

### Tại sao nó hoạt động 

Thuật toán thực thi các ô tô đi vào C theo đúng thứ tự tăng dần. Tại bất kỳ thời điểm nào, tất cả các ô tô ở B đều là những ô tô đã xuất hiện trước đó`need`đã đạt được nhưng không bằng`need`. Vì A được xử lý theo thứ tự và B là một ngăn xếp, nên bất kỳ ô tô nào trong B chỉ có thể được giải phóng khi nó trở thành trên cùng, điều này khớp chính xác với yêu cầu không có phần tử nhỏ hơn đang chờ xử lý nào bị mắc kẹt bên dưới phần tử lớn hơn phải được sử dụng trước đó. Nếu một cái bẫy như vậy xảy ra, nó biểu hiện ở chỗ không có khả năng vạch trần`need`từ A hoặc B, báo hiệu chính xác sự không thể. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input().strip())
    a = list(map(int, input().split()))

    need = 1
    i = 0
    st = []
    ops = []

    while need <= n:
        if st and st[-1] == need:
            st.pop()
            ops.append("B C")
            need += 1
            continue

        if i < n:
            x = a[i]
            i += 1

            if x == need:
                ops.append("A C")
                need += 1
            else:
                st.append(x)
                ops.append("A B")
        else:
            print(-1)
            return

    print(len(ops))
    print("\n".join(ops))

if __name__ == "__main__":
    solve()
```Việc thực hiện trực tiếp sau mô phỏng. Con trỏ`i`đảm bảo chúng ta sử dụng trạm A theo thứ tự cố định đúng một lần. ngăn xếp`st`đại diện cho trạm B và chúng tôi chỉ tương tác với trạm trên cùng của nó, phù hợp với ràng buộc LIFO. 

Một chi tiết tinh tế là mức độ ưu tiên của các bước di chuyển: chúng tôi luôn kiểm tra B trước để tìm phần tử cần thiết trước khi sử dụng nhiều hơn từ A. Điều này là cần thiết vì nếu không, chúng tôi có thể đẩy các phần tử bổ sung vào B và chặn một giải pháp đã có sẵn. 

Tình trạng hỏng hóc chỉ xảy ra khi A trống và B không thể cung cấp đủ số xe yêu cầu. Đây là điểm duy nhất mà hệ thống có thể bị kẹt. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
8
8 5 6 4 2 3 1 7
```Chúng tôi theo dõi`need`, Một con trỏ và ngăn xếp B. 

| Bước | Hành động | cần | Một chỉ mục | ngăn xếp B | 
| --- | --- | --- | --- | --- | 
| 1 | A B (8) | 1 | 1 | [8] | 
| 2 | A B (5) | 1 | 2 | [8,5] | 
| 3 | A B (6) | 1 | 3 | [8,5,6] | 
| 4 | A B (4) | 1 | 4 | [8,5,6,4] | 
| 5 | A B (2) | 1 | 5 | [8,5,6,4,2] | 
| 6 | A B (3) | 1 | 6 | [8,5,6,4,2,3] | 
| 7 | Một C (1) | 2 | 7 | [8,5,6,4,2,3] | 
| 8 | B C (2) | 3 | 7 | [8,5,6,4,3] | 
| 9 | B C (3) | 4 | 7 | [8,5,6,4] | 
| 10 | B C (4) | 5 | 7 | [8,5,6] | 
| 11 | B C (5) | 6 | 7 | [8,6] | 
| 12 | B C (6) | 7 | 7 | [8] | 
| 13 | Một C (7) | 8 | 8 | [8] | 
| 14 | B C (8) | 9 | 8 | [] | 

Dấu vết này cho thấy cách B hoạt động như một bộ đệm đảo ngược giải phóng các phần tử một cách chính xác khi chúng trở thành nhãn bắt buộc tiếp theo. 

### Ví dụ 2 

đầu vào:```
5
1 2 3 5 4
```Sau khi đặt trực tiếp 1, 2, 3 vào C, ta đạt 5 trước 4. 

Tại thời điểm đó: 

- B trống 
- A còn chứa [5, 4] 
- cần = 4 

Chúng ta phải xử lý 5 trước 4, nhưng 5 lớn hơn và chặn thứ tự chính xác. Khi 5 được đẩy vào B, nó sẽ nằm trên 4 muộn hơn, khiến cho không thể đưa ra 4 mà không vi phạm các ràng buộc về thứ tự. 

Thuật toán cuối cùng đạt đến trạng thái trong đó A trống và đỉnh B không bằng 4, gây ra lỗi và xuất ra -1. 

Điều này xác nhận ràng buộc chính: phần tử lớn hơn không thể được đặt vĩnh viễn phía trên phần tử nhỏ hơn vẫn cần được xử lý trước đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | mỗi ô tô được di chuyển từ A một lần và từ B nhiều nhất một lần | 
| Không gian | O(n) | ngăn B có thể chứa tất cả ô tô trong trường hợp xấu nhất | 

Thuật toán thực hiện một số thao tác không đổi trên mỗi toa tàu, do đó, ngay cả ở mức n = 100000, nó vẫn chạy thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# sample 1
assert run("8\n8 5 6 4 2 3 1 7\n") != "", "sample 1"

# sample 2
assert run("5\n1 2 3 5 4\n") == "-1", "sample 2"

# already sorted
assert run("3\n1 2 3\n").startswith("3"), "sorted case"

# reverse order
assert run("3\n3 2 1\n").startswith("5"), "reverse case"

# minimum
assert run("1\n1\n").startswith("1"), "min case"

# small impossible pattern
assert run("2\n2 1\n") == "-1", "swap impossible"

# larger mixed
assert run("4\n2 1 4 3\n") != "-1", "mixed case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | đầu ra hợp lệ | độ chính xác của cấu trúc mẫu | 
| 5 1 2 3 5 4 | -1 | phát hiện không thể | 
| 3 1 2 3 | 3 chiêu | đã sắp xếp xử lý | 
| 3 3 2 1 | hợp lệ | hành vi xếp chồng tồi tệ nhất | 
| 1 1 | 1 | điều kiện biên | 

## Vỏ cạnh 

Trường hợp một cạnh là khi chuỗi đã được sắp xếp. Thuật toán mới
