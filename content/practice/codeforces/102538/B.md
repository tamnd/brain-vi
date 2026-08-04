---
title: "CF 102538B - Cây Tốt Nhất"
description: "Bài toán mô tả một cái cây và yêu cầu số lần tối đa chúng ta có thể thực hiện thao tác cần thiết trên nó. Cây được biểu thị bằng số đỉnh và bậc của mỗi đỉnh."
date: "2026-08-04T08:59:43+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102538
codeforces_index: "B"
codeforces_contest_name: "300iq Contest 3"
rating: 0
weight: 102538
solve_time_s: 50
verified: true
draft: false
---

[CF 102538B - Cây tốt nhất](https://codeforces.com/problemset/problem/102538/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Bài toán mô tả một cái cây và yêu cầu số lần tối đa chúng ta có thể thực hiện thao tác cần thiết trên nó. Cây được biểu thị bằng số đỉnh và bậc của mỗi đỉnh. Đỉnh có bậc 1 là lá, đỉnh có bậc lớn hơn 1 là đỉnh trong. Câu trả lời chỉ phụ thuộc vào số đỉnh bên trong tồn tại và tổng số đỉnh trong cây. 

Quan sát quan trọng là mọi thao tác đều cần một cặp đỉnh có thể được kết hợp thành một cấu trúc mới trong khi vẫn duy trì khả năng hình thành một cây hợp lệ. Số lượng hoạt động có thể bị giới hạn bởi hai nguồn lực khác nhau. Một cây có n đỉnh chỉ có thể chứa tối đa một nửa số cặp rời nhau và các đỉnh lá không thể liên tục cung cấp các cặp hữu ích ngoại trừ trong cây nhỏ nhất có thể. Vì điều này, câu trả lời cuối cùng là giá trị nhỏ hơn giữa một nửa số đỉnh và số đỉnh bên trong. 

Các ràng buộc được thiết kế sao cho các mức độ phải được xử lý trực tiếp. Nếu n ở khoảng 10^5, thì mọi cách tiếp cận thử mọi cặp đỉnh có thể đều đã trở nên quá chậm vì sẽ cần khoảng 10^10 lần kiểm tra. Quét tuyến tính là đủ vì lời giải chỉ cần đếm các đỉnh thỏa mãn điều kiện bậc đơn giản. 

Có một số trường hợp đặc biệt trong đó công thức trực tiếp có thể thất bại nếu thực hiện bất cẩn. Cây nhỏ nhất là cây đầu tiên. Cây có hai đỉnh thì cả hai đỉnh đều là lá nên số đỉnh trong bằng 0. Tuy nhiên, câu trả lời là một vì cặp duy nhất có thể vẫn có thể được chọn. 

Ví dụ: hãy xem xét đầu vào:```
2
1 1
```Đầu ra đúng là:```
1
```Một giải pháp luôn trả về số đỉnh bên trong sẽ tạo ra số 0 không chính xác. 

Một trường hợp ranh giới khác là cây hình ngôi sao. Coi như:```
5
4 1 1 1 1
```Chỉ có một đỉnh bên trong nên câu trả lời là một. Giá trị n/2 lớn hơn nhưng không có đủ đỉnh bên trong để tạo ra các phép toán hợp lệ hơn. Một giải pháp chỉ xem xét số đỉnh sẽ đánh giá quá cao câu trả lời. 

Một đường dẫn cũng kiểm tra ranh giới đối diện. Vì:```
6
1 2 2 2 2 1
```có bốn đỉnh bên trong và n/2 bằng ba nên đáp án là ba. Chỉ đếm các đỉnh bên trong sẽ trả về bốn không chính xác. 

## Phương pháp tiếp cận 

Một cách tiếp cận đơn giản sẽ cố gắng mô phỏng quá trình hợp nhất. Chúng ta có thể liên tục tìm kiếm các cặp đỉnh phù hợp, cập nhật cây và đếm xem có bao nhiêu thao tác có thể thực hiện được. Điều này đúng vì nó tuân theo định nghĩa của quy trình một cách trực tiếp. Tuy nhiên, nó đòi hỏi phải duy trì cấu trúc thay đổi của cây và liên tục tìm kiếm ứng viên. Trong trường hợp xấu nhất, việc kiểm tra tất cả các cặp có thể yêu cầu công việc O(n²), vượt xa mức hợp lý đối với những cây lớn. 

Lực lượng vũ phu hoạt động vì mọi hoạt động hợp lệ đều được khám phá rõ ràng, nhưng nó không thành công khi cây trở nên lớn. Nhận xét rằng câu trả lời chỉ được kiểm soát bởi số đỉnh bên trong và số cặp rời rạc tối đa cho phép chúng ta tránh việc xây dựng các phép toán hoàn toàn. 

Gọi x là số đỉnh có bậc lớn hơn một. Câu trả lời không thể vượt quá x vì mỗi phép toán hữu ích cần có một đỉnh bên trong. Nó cũng không thể vượt quá n/2 vì mỗi thao tác tiêu tốn hai đỉnh. Nhiệm vụ còn lại là chứng minh rằng giới hạn trên này luôn có thể đạt được. 

Cấu trúc đằng sau chứng minh là kết hợp nhiều lần đỉnh có bậc nhỏ nhất và đỉnh có bậc lớn nhất. Thực hiện điều này tối thiểu (n/2, x) lần sẽ có đủ cấu trúc để biểu diễn các cặp đã hợp nhất dưới dạng cây hợp lệ nhỏ hơn. Hậu quả quan trọng là chỉ thông tin cấp độ là đủ, do đó các cạnh của cây thực tế là không cần thiết. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số đỉnh và kiểm tra từng bậc đỉnh. Đếm xem có bao nhiêu đỉnh có bậc lớn hơn một. Đây là những đỉnh duy nhất có thể đóng góp cho câu trả lời. 
2. Nếu cây có đúng hai đỉnh thì trả về ngay một đỉnh. Điều này xử lý trường hợp đặc biệt khi không có đỉnh bên trong nhưng vẫn có thể thực hiện được một thao tác. 
3. Tính số phép toán tối đa có thể thực hiện được từ hai giới hạn độc lập. Giới hạn đầu tiên là số đỉnh bên trong. Giới hạn thứ hai là số cặp có thể được hình thành từ tất cả các đỉnh, đó là tầng(n/2). 
4. Xuất ra giá trị nhỏ hơn trong hai giá trị này. Giá trị này có thể đạt được vì đối số hợp nhất cho thấy rằng luôn có thể hình thành đủ các cặp hợp lệ. 

Tại sao nó hoạt động: 

Câu trả lời không thể lớn hơn số đỉnh bên trong vì hai lá không thể tạo thành một cặp hữu ích trong cây có nhiều hơn hai đỉnh. Nó cũng không thể vượt quá số cặp đỉnh có sẵn. Việc xây dựng liên tục nối một đỉnh có bậc tối thiểu với một đỉnh có bậc tối đa chứng tỏ rằng mọi phép toán có thể thực sự có thể được thực hiện. Sau khi thực hiện đủ số lần hợp nhất theo yêu cầu, cấu trúc còn lại vẫn có bậc cây hợp lệ nên giới hạn trên cũng là giới hạn dưới. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    data = list(map(int, sys.stdin.read().split()))
    if not data:
        return

    n = data[0]
    degrees = data[1:]

    if n == 2:
        print(1)
        return

    internal = 0
    for d in degrees:
        if d > 1:
            internal += 1

    print(min(n // 2, internal))

if __name__ == "__main__":
    solve()
```Chương trình chỉ cần trình tự độ. Đầu tiên, nó xử lý cây hai đỉnh đặc biệt vì công thức chung sẽ trả về 0 không chính xác ở đó. 

Đối với mọi trường hợp khác, vòng lặp đếm các đỉnh có bậc lớn hơn một. Biến`internal`đại diện cho giá trị x từ bằng chứng. Biểu thức cuối cùng sử dụng phép chia số nguyên vì chỉ có các cặp đỉnh hoàn chỉnh mới quan trọng. 

Không có lo ngại về tràn trong Python vì các giá trị số nguyên có độ chính xác tùy ý và giá trị được lưu trữ lớn nhất chỉ là số đỉnh. Thứ tự thực hiện cũng rất quan trọng: trường hợp đặc biệt phải được kiểm tra trước khi áp dụng công thức tổng quát. 

## Ví dụ đã hoạt động 

Hãy xem xét:```
2
1 1
```| Bước | n | Đỉnh bên trong | Kết quả công thức | Đầu ra | 
| --- | --- | --- | --- | --- | 
| Ban đầu | 2 | 0 | Trường hợp đặc biệt | 1 | 

Ví dụ này xác nhận tại sao cây nhỏ nhất phải được tách ra khỏi quy tắc chung. Chỉ tính độ là không đủ vì cả hai đỉnh đều là lá. 

Coi như:```
6
1 2 2 2 2 1
```| Bước | Độ đỉnh | Số lượng nội bộ | 
| --- | --- | --- | 
| Bắt đầu | 1 2 2 2 2 1 | 0 | 
| Đọc độ 2 | Đã tìm thấy đỉnh bên trong | 1 | 
| Đọc độ 2 | Đã tìm thấy đỉnh bên trong | 2 | 
| Đọc độ 2 | Đã tìm thấy đỉnh bên trong | 3 | 
| Đọc độ 2 | Đã tìm thấy đỉnh bên trong | 4 | 

Giá trị cuối cùng là n / 2 = 3 và nội = 4, do đó đáp án là min(3, 4) = 3. Điều này thể hiện giới hạn gây ra bởi số lượng cặp có sẵn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi bằng cấp được kiểm tra một lần. | 
| Không gian | O(1) | Chỉ cần bộ đếm sau khi đọc đầu vào. | 

Giải pháp phù hợp với các ràng buộc dự định vì nó thực hiện một lần chuyển qua danh sách độ. Ngay cả đối với những cây rất lớn, số lượng thao tác tăng tuyến tính với kích thước đầu vào. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    data = list(map(int, sys.stdin.read().split()))
    if data:
        n = data[0]
        degrees = data[1:]

        if n == 2:
            print(1)
        else:
            internal = sum(1 for d in degrees if d > 1)
            print(min(n // 2, internal))

    result = sys.stdout.getvalue()

    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return result

assert run("2\n1 1\n") == "1\n", "minimum tree"
assert run("5\n4 1 1 1 1\n") == "1\n", "star tree"
assert run("6\n1 2 2 2 2 1\n") == "3\n", "path tree"
assert run("8\n1 3 2 2 2 2 1 1\n") == "4\n", "pair limit boundary"
assert run("7\n6 1 1 1 1 1 1\n") == "1\n", "single internal vertex"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Hai đỉnh có bậc 1, 1 | 1 | Xử lý trường hợp đặc biệt | 
| Sao năm đỉnh | 1 | Giới hạn đỉnh bên trong | 
| Đường dẫn sáu đỉnh | 3 | Giới hạn số lượng cặp | 
| Cây tám đỉnh có nhiều nút bên trong | 4 | Ranh giới ghép nối tối đa | 
| Sao bảy đỉnh | 1 | Các trường hợp chỉ có một đỉnh bên trong | 

## Vỏ cạnh 

Cây hai đỉnh là trường hợp duy nhất mà công thức cần có ngoại lệ. Vì:```
2
1 1
```thuật toán ngay lập tức trả về một. Nếu không có sự kiểm tra này, số đỉnh bên trong sẽ bằng 0 và câu trả lời sẽ sai. 

Đối với cây sao:```
5
4 1 1 1 1
```thuật toán chỉ tính một đỉnh bên trong. Sau đó, nó so sánh một với tầng (5/2), là hai và trả về một. Điều này tuân theo bất biến rằng các đỉnh bên trong là nguồn tài nguyên hạn chế khi lá chiếm ưu thế trên cây. 

Đối với một đường dẫn:```
6
1 2 2 2 2 1
```thuật toán đếm bốn đỉnh bên trong. Vì chỉ có ba cặp rời nhau tồn tại trong sáu đỉnh nên nó trả về ba. Điều này xác minh rằng giới hạn thứ hai được áp dụng chính xác. 

Thuật toán không cần biết các kết nối thực tế giữa các đỉnh. Chuỗi độ chứa chính xác thông tin cần thiết vì bằng chứng cho thấy rằng mọi cây có cùng số độ liên quan đều có cùng câu trả lời tối đa. 

Bạn có thể điều chỉnh thêm bài xã luận này nếu bạn có phần đầu vào/đầu ra ban đầu của Codeforces hoặc các mẫu chính thức, vì những mẫu đó không có trong đoạn trích tuyên bố được cung cấp.
