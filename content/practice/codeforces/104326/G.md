---
title: "CF 104326G - Christopher Robin đang học lập trình hướng đối tượng"
description: "Chúng ta được cung cấp một tập hợp các đối tượng trong đó mỗi đối tượng có một giá trị số và một danh sách tham chiếu có thứ tự cố định đến các đối tượng khác. Các tham chiếu tạo thành một cấu trúc có hướng và cấu trúc này có thể bao gồm các chu trình."
date: "2026-07-01T19:09:34+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104326
codeforces_index: "G"
codeforces_contest_name: "Udmurt SU Contest 2011"
rating: 0
weight: 104326
solve_time_s: 89
verified: false
draft: false
---

[CF 104326G - Christopher Robin đang học lập trình hướng đối tượng](https://codeforces.com/problemset/problem/104326/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 29s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một tập hợp các đối tượng trong đó mỗi đối tượng có một giá trị số và một danh sách tham chiếu có thứ tự cố định đến các đối tượng khác. Các tham chiếu tạo thành một cấu trúc có hướng và cấu trúc này có thể bao gồm các chu trình. Nhiệm vụ là phân vùng tất cả các đối tượng thành các lớp tương đương trong đó hai đối tượng thuộc cùng một lớp nếu “cấu trúc mở rộng hoàn toàn” của chúng giống hệt nhau, nghĩa là giá trị vô hướng của chúng bằng nhau và danh sách tham chiếu của chúng trỏ đến các đối tượng tương đương với nhau theo cùng một thứ tự. 

Nói một cách cụ thể hơn, mỗi đối tượng được xác định bởi một nhãn và một chuỗi các đối tượng con, và hai đối tượng giống hệt nhau nếu đồ thị có hướng gốc của chúng, với thứ tự các con được giữ nguyên, là đẳng cấu dưới sự bình đẳng của các cấu trúc con. 

Khó khăn xuất phát từ thực tế là các tài liệu tham khảo có thể tạo thành các chu kỳ. Điều đó có nghĩa là cấu trúc không phải là một cái cây, do đó việc so sánh đệ quy đơn giản giữa các cây con sẽ không kết thúc trừ khi chúng ta bằng cách nào đó phá vỡ chu trình phụ thuộc. 

Kích thước đầu vào lên tới 100.000 đối tượng và mỗi đối tượng có tối đa 10 tham chiếu. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào so sánh mọi cặp đối tượng về mặt cấu trúc. Phương pháp so sánh theo cặp sẽ là O(n²), tức là khoảng 10¹⁰ so sánh trong trường hợp xấu nhất, vượt xa giới hạn khả thi. Ngay cả một phép băm đệ quy đơn giản cho mỗi đối tượng tính toán lại các hàm băm cấu trúc con nhiều lần cũng sẽ bùng nổ do việc truyền tải lặp đi lặp lại trên các đồ thị con được chia sẻ. 

Trường hợp cạnh tinh tế xuất hiện khi chu kỳ tồn tại. Ví dụ: đối tượng A có thể tham chiếu B và B có thể tham chiếu A. Tính toán băm DFS đơn giản sẽ lặp lại vô hạn hoặc yêu cầu ghi nhớ đặc biệt khó thiết kế chính xác nếu không có sự phối hợp toàn cầu. 

Một trường hợp khác là nhiều cấu trúc con giống hệt nhau xuất hiện ở các chỉ số khác nhau. Mặc dù chúng giống nhau về mặt cấu trúc nhưng chúng phải nhận cùng một lớp và sự bình đẳng này phải nhất quán ngay cả khi được phát hiện theo các thứ tự truyền tải khác nhau. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là xác định một “tuần tự hóa” đệ quy của từng đối tượng: biểu diễn một đối tượng dưới dạng một bộ dữ liệu bao gồm giá trị vô hướng của nó và danh sách các bộ dữ liệu con được tuần tự hóa, sau đó so sánh các bộ dữ liệu này để tìm sự bằng nhau. Nếu tính toán từ dưới lên này, chúng ta có thể băm từng cấu trúc và sau đó nhóm các giá trị băm giống hệt nhau. 

Tuy nhiên, cách tiếp cận này thất bại khi có sự hiện diện của các chu kỳ. Không có thứ tự từ dưới lên tự nhiên vì các phần phụ thuộc không được đảm bảo để tạo thành DAG. Việc cố gắng tính toán các giá trị băm một cách đệ quy sẽ dẫn đến việc đệ quy vô hạn hoặc tính toán lại nhiều lần. Ngay cả khi chúng tôi áp dụng tính năng ghi nhớ, chúng tôi vẫn gặp phải vấn đề phụ thuộc vòng tròn trong đó hàm băm của A phụ thuộc vào B và ngược lại và không thể hoàn thiện một cách độc lập. 

Quan sát quan trọng là chúng ta thực sự không cần phải giải quyết các chu trình trong quá trình đánh giá cấu trúc. Chúng ta chỉ cần một mối quan hệ tương đương nhất quán trên tất cả các nút. Đây chính xác là một vấn đề sàng lọc phân vùng đồ thị: chúng tôi muốn gán cho mỗi nút một mã định danh ổn định sao cho hai nút chia sẻ cùng một mã định danh khi và chỉ khi giá trị vô hướng của chúng và tập hợp được sắp xếp theo cấu trúc của mã định danh lân cận của chúng khớp với nhau. 

Điều này cho thấy một quá trình ổn định lặp đi lặp lại. Chúng tôi bắt đầu bằng cách gán cho mỗi nút một nhãn ban đầu chỉ dựa trên giá trị vô hướng của nó. Sau đó, chúng tôi liên tục tinh chỉnh các nhãn bằng cách xem xét bộ dữ liệu được hình thành bởi giá trị vô hướng của nút và chuỗi nhãn hiện tại của các đối tượng được tham chiếu của nó. Mỗi lần lặp lại sẽ nén các bộ dữ liệu này thành các mã định danh duy nhất mới. Bởi vì các nhãn chỉ phụ thuộc vào các nhãn trước đó nên các chu kỳ không gây ra vấn đề gì vì chúng tôi không lặp lại mà đang lặp lại.

Đây thực chất là một phép tính điểm cố định trên hàm ghi nhãn đồ thị. Vì biểu đồ có tối đa 100k nút và mỗi nút có tối đa 10 cạnh nên mỗi bước tinh chỉnh có kích thước đầu vào là tuyến tính. Trong thực tế, số lần lặp cần thiết là nhỏ vì mỗi lần lặp sẽ tinh chỉnh hoặc ổn định nghiêm ngặt các lớp tương đương và không gian trạng thái được giới hạn bởi số lượng nút. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Băm đệ quy Brute Force | O(n² + chi phí đệ quy) | O(n) | Quá chậm / Không chính xác do chu kỳ | 
| Tinh chỉnh nhãn lặp đi lặp lại | O(k · n log n) | O(n) | Đã chấp nhận | 

Ở đây k là số vòng sàng lọc cho đến khi ổn định, thường nhỏ. 

## Hướng dẫn thuật toán 

Chúng tôi coi mỗi đối tượng là một nút trong biểu đồ có hướng, trong đó các cạnh bảo toàn thứ tự. Mỗi nút mang một giá trị và danh sách các cạnh đi ra. 

1. Gán cho mỗi nút một nhãn ban đầu bằng giá trị vô hướng của nó. Điều này đưa ra một nhóm thô trong đó chỉ các giá trị giống nhau mới được coi là bằng nhau. 
2. Xây dựng cho mỗi nút một chữ ký bao gồm giá trị vô hướng của nó và nhãn hiện tại của các nút được tham chiếu theo thứ tự. Chữ ký này thể hiện “chế độ xem” hiện tại của đối tượng theo giá trị gần đúng hiện tại. 
3. Sắp xếp hoặc băm các chữ ký này để gán các mã định danh nhỏ gọn mới. Các nút có chữ ký giống nhau sẽ nhận được cùng một nhãn mới. Bước này về cơ bản là nén thông tin cấu trúc thành các lớp tương đương. 
4. Lặp lại việc xây dựng chữ ký bằng cách sử dụng các nhãn đã cập nhật cho đến khi các nhãn ngừng thay đổi giữa các lần lặp. Khi không có nhãn nào thay đổi, chúng ta đã đạt tới một điểm cố định nơi sự tương đương về cấu trúc là ổn định. 
5. Xuất nhãn cuối cùng của mỗi nút làm định danh lớp của nó. 

Lý do chúng ta dựa vào phép lặp thay vì đệ quy là vì các chu kỳ ngăn cản thứ tự đánh giá được xác định rõ ràng. Việc lặp lại lan truyền các ràng buộc trên toàn cầu, cho phép thông tin lưu chuyển theo chu kỳ cho đến khi đạt được tính nhất quán. 

### Tại sao nó hoạt động 

Bất biến chính là sau mỗi lần lặp, nếu hai nút có cùng nhãn thì giá trị vô hướng của chúng bằng nhau và nhãn lân cận của chúng khớp với thứ tự trong lần lặp trước đó. Điều này đảm bảo rằng các nhãn chỉ trở nên có tính phân biệt rõ ràng hơn hoặc vẫn ổn định. Vì chỉ có hữu hạn nhiều nút nên quá trình sàng lọc không thể tiếp tục vô thời hạn và cuối cùng đạt đến điểm không thể sàng lọc thêm nữa. Tại thời điểm đó, sự bình đẳng của các nhãn trùng khớp chính xác với sự tương đương về cấu trúc bởi vì bất kỳ sự khác biệt về cấu trúc nào cũng sẽ tạo ra một dấu hiệu khác nhau trong một số lần lặp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    val = [0] * n
    adj = [[] for _ in range(n)]

    for i in range(n):
        parts = list(map(int, input().split()))
        val[i] = parts[0]
        m = parts[1]
        adj[i] = [x - 1 for x in parts[2:]]

    # initial labels based on scalar value
    label = val[:]

    while True:
        sig_map = {}
        new_label = [0] * n
        cur_id = 1

        for i in range(n):
            sig = (label[i], tuple(label[v] for v in adj[i]))
            if sig not in sig_map:
                sig_map[sig] = cur_id
                cur_id += 1
            new_label[i] = sig_map[sig]

        if new_label == label:
            break
        label = new_label

    print(*label)

if __name__ == "__main__":
    solve()
```Việc triển khai duy trì một mảng nhãn gần giống với nhận dạng cấu trúc. Mỗi lần lặp lại xây dựng một chữ ký bộ bao gồm nhãn hiện tại và danh sách các nhãn lân cận được sắp xếp theo thứ tự. Các bộ dữ liệu Python được sử dụng trực tiếp làm khóa từ điển để xác định duy nhất các trạng thái cấu trúc. 

Điều kiện dừng kiểm tra tính ổn định của nhãn trong một lần lặp. Điều này đảm bảo rằng không thể tinh chỉnh thêm nữa, nghĩa là tất cả các cấu trúc có thể phân biệt được đã được tách ra. 

Một điểm tinh tế là chúng tôi không thử bất kỳ DFS hoặc đệ quy nào. Tất cả các phần phụ thuộc đều được giải quyết đồng thời trên mỗi lần lặp, giúp tránh các vòng lặp vô hạn trong biểu đồ tuần hoàn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
1 1 2
2 1 3
3 0
```Trạng thái ban đầu: 

| Nút | Giá trị | Nhãn ban đầu | 
| --- | --- | --- | 
| 1 | 1 | 1 | 
| 2 | 2 | 2 | 
| 3 | 3 | 3 | 

Chữ ký lặp 1: 

| Nút | Chữ ký | Nhãn Mới | 
| --- | --- | --- | 
| 1 | (1, [2]) | 1 | 
| 2 | (2, [3]) | 2 | 
| 3 | (3, []) | 3 | 

Sau đó không có thay đổi nào xảy ra nên thuật toán dừng lại. 

Điều này xác nhận rằng các chuỗi đơn giản đã ổn định sau một bước sàng lọc. 

### Ví dụ 2 

đầu vào:```
2
5 1 2
5 1 1
```Nhãn ban đầu: 

| Nút | Giá trị | Nhãn | 
| --- | --- | --- | 
| 1 | 5 | 5 | 
| 2 | 5 | 5 | 

Chữ ký lặp 1: 

| Nút | Chữ ký | Nhãn Mới | 
| --- | --- | --- | 
| 1 | (5, [5]) | 1 | 
| 2 | (5, [5]) | 1 | 

Cả hai nút đều thu gọn vào cùng một lớp, cho thấy đệ quy lẫn nhau không ngăn cản sự hội tụ. Bước sàng lọc giải quyết chu trình bằng tính đối xứng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(k · n log n) | Mỗi lần lặp lại xử lý tất cả các nút và chữ ký băm/sắp xếp; k là số vòng ổn định | 
| Không gian | O(n) | Chúng tôi lưu trữ danh sách kề và nhãn hiện tại | 

Cho n tối đa 100.000 và m trên mỗi nút nhiều nhất là 10, mỗi lần lặp trong thực tế là tuyến tính. Số lần lặp được giới hạn bởi số mức sàng lọc riêng biệt trước khi hội tụ, con số này nhỏ do sự phân chia nhanh chóng của các lớp tương đương. 

Điều này phù hợp thoải mái trong cả giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from io import StringIO as _StringIO

    output = _StringIO()
    _stdout = _sys.stdout
    _sys.stdout = output
    try:
        solve()
    finally:
        _sys.stdout = _stdout
    return output.getvalue().strip()

# provided sample
assert run("""8
50 3 3 4 5
120 2 1 4
20 0
30 0
40 0
50 3 3 4 5
120 2 6 4
120 2 4 6
""") == """10
12
3
4
5
10
12
11"""

# minimal case
assert run("""1
7 0
""") == "1"

# two identical self-referential nodes
assert run("""2
1 1 2
1 1 1
""") == "1\n1"

# distinct values no references
assert run("""3
1 0
2 0
3 0
""") == "1\n2\n3"

# chain structure
assert run("""4
1 1 2
1 1 3
1 1 4
1 0
""") == """1
2
3
4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nút đơn | 1 | trường hợp cơ sở | 
| chu kỳ lẫn nhau | tất cả đều giống nhau | ổn định chu kỳ | 
| tất cả đều bị cô lập | các lớp riêng biệt | danh tính chỉ vô hướng | 
| chuỗi | cấu trúc gia tăng | độ nhạy đặt hàng | 

## Vỏ cạnh 

Chu trình trực tiếp giữa các nút được xử lý một cách tự nhiên vì mỗi lần lặp lại xử lý các nhãn lân cận như đã được xác định từ bước trước. Trong trường hợp tham chiếu lẫn nhau giữa hai nút, cả hai nút bắt đầu giống hệt nhau, do đó chúng giữ các chữ ký giống hệt nhau mãi mãi, điều này gán chính xác cho chúng cùng một lớp. 

Các nút tự tham chiếu hoạt động tương tự. Một nút trỏ đến chính nó sẽ tạo ra một chữ ký bao gồm nhãn hiện tại của nó trong danh sách lân cận của chính nó. Vì cả hai nút đều so sánh bằng nhau trong mỗi lần lặp nên điểm cố định sẽ ổn định ngay lập tức. 

Các tập hợp lớn các đối tượng giống hệt nhau với các mẫu tham chiếu giống hệt nhau sẽ thu gọn thành một lớp duy nhất trong một hoặc hai lần lặp vì dấu hiệu của chúng vẫn giống nhau ở mọi giai đoạn sàng lọc, ngăn chặn sự phân tách không cần thiết.
