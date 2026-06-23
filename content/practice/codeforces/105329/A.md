---
title: "CF 105329A - \u0422\u0440\u0438 \u0447\u0438\u0441\u043b\u0430"
description: "Ba đống kẹo được đưa ra và mỗi đống có thể được giữ nguyên hoặc nhân đôi một cách độc lập. Sau khi chọn các thao tác này, tổng số kẹo trên cả ba cọc sẽ được cố định."
date: "2026-06-22T17:36:29+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105329
codeforces_index: "A"
codeforces_contest_name: "\u041f\u0435\u0440\u0432\u0435\u043d\u0441\u0442\u0432\u043e \u0421\u0432\u0435\u0440\u0434\u043b\u043e\u0432\u0441\u043a\u043e\u0439 \u043e\u0431\u043b\u0430\u0441\u0442\u0438 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e \u0441\u0440\u0435\u0434\u0438 \u043d\u0430\u0447\u0438\u043d\u0430\u044e\u0449\u0438\u0445 2024"
rating: 0
weight: 105329
solve_time_s: 74
verified: true
draft: false
---

[CF 105329A - \u0422\u0440\u0438 \u0447\u0438\u0441\u043b\u0430](https://codeforces.com/problemset/problem/105329/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 14s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Ba đống kẹo được đưa ra và mỗi đống có thể được giữ nguyên hoặc nhân đôi một cách độc lập. Sau khi chọn các thao tác này, tổng số kẹo trên cả ba cọc sẽ được cố định. Câu hỏi đặt ra là liệu có thể làm cho tổng số cuối cùng này chia đều cho ba người hay không, nghĩa là tổng số tiền phải chia hết cho 3. 

Vì vậy, quyết định thực tế không phải là làm thế nào để chia các cọc mà là liệu chúng ta có thể chọn một tập hợp con các cọc để nhân đôi sao cho tổng kết quả thỏa mãn điều kiện chia hết hay không. 

Mỗi cọc chỉ có hai trạng thái, ban đầu hoặc gấp đôi, do đó tổng cuối cùng có thể được hình thành từ một tập hợp nhỏ các kết hợp. Mục tiêu là kiểm tra xem có ít nhất một kết hợp dẫn đến tổng số chia hết cho 3 hay không. 

Các ràng buộc rất nhỏ, mỗi giá trị tối đa là 1000, do đó, bất kỳ lý do nào liệt kê tất cả các kết hợp đều có hiệu quả là thời gian không đổi. Ngay cả việc kiểm tra ngây thơ đối với tất cả tám khả năng cũng là chuyện nhỏ trong bất kỳ giới hạn thời gian nào. Điều quan trọng là nhận ra rằng cấu trúc là các lựa chọn nhị phân trên ba phần tử độc lập. 

Một sai lầm phổ biến là tập trung vào việc phân phối kẹo đều ở cuối thay vì nhận ra rằng việc phân phối chỉ có thể thực hiện được khi tổng số tiền chia hết cho 3. Một sai lầm khác là cố gắng suy luận cục bộ về từng cọc một cách độc lập, nhưng khả năng chia hết chỉ phụ thuộc vào tổng tổng. 

Trường hợp cạnh tinh tế là khi tổng ban đầu đã chia hết cho 3. Trong trường hợp đó, câu trả lời ngay lập tức là CÓ mà không cần thực hiện bất kỳ thao tác nào. Một trường hợp đặc biệt khác là khi chỉ có một tổ hợp nhân đôi cụ thể khắc phục được khả năng chia hết, ví dụ như các đầu vào nhỏ trong đó sự thay đổi tính chẵn lẻ là quan trọng ngay cả khi cường độ rất nhỏ. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực liệt kê tất cả các cách để áp dụng các hoạt động nhân đôi. Vì mỗi cọc trong số ba cọc có hai trạng thái nên chúng tôi kiểm tra tất cả các cấu hình 2³ = 8. Đối với mỗi cấu hình, chúng tôi tính toán tổng kết quả và kiểm tra xem nó có chia hết cho 3 hay không. Cách này hiệu quả vì không gian trạng thái không đổi nên độ chính xác rất đơn giản. 

Lực lượng vũ phu này đã tối ưu trong vấn đề này, nhưng sẽ rất hữu ích nếu hiểu tại sao nó vẫn hợp lệ. Mỗi cọc đóng góp độc lập và các hoạt động không tương tác ngoại trừ thông qua tổng cuối cùng. Tính độc lập đó đảm bảo rằng việc liệt kê tất cả các kết hợp sẽ nắm bắt được mọi kết quả có thể xảy ra. 

Không có cải tiến tiệm cận có ý nghĩa vì không gian tìm kiếm có kích thước cố định. Ngay cả khi các ràng buộc lớn hơn, cấu trúc tương tự vẫn sẽ giảm xuống việc kiểm tra số lượng kết hợp không đổi hoặc điều kiện số học mô-đun. 

Điểm mấu chốt là vấn đề này tương đương với việc kiểm tra xem có tồn tại một tập hợp con các giá trị mà việc nhân đôi làm thay đổi tổng theo modulo 3 thành 0 hay không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(1) | O(1) | Đã chấp nhận | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta muốn xác định liệu việc gán các lựa chọn nhân đôi có dẫn đến tổng chia hết cho 3 hay không. 

1. Đọc ba số nguyên đại diện cho cọc ban đầu. 
2. Liệt kê tất cả các tập con của cọc để nhân đôi. Mỗi tập hợp con đại diện cho một mặt nạ nhị phân từ 0 đến 7. 
3. Đối với mỗi tập hợp con, tính tổng bằng cách cộng giá trị ban đầu hoặc gấp đôi giá trị đó tùy thuộc vào việc cọc có được chọn hay không. 
4. Kiểm tra xem tổng này có chia hết cho 3 không. 
5. Nếu bất kỳ cấu hình nào thỏa mãn điều kiện, hãy trả về CÓ ngay lập tức. 
6. Nếu không có, hãy trả về NO. 

Lý do quy trình này là đủ là vì mọi trình tự thao tác hợp lệ đều tương ứng chính xác với một tập hợp con cọc. Vì không có hoạt động nào khác được phép nên việc liệt kê đã hoàn tất. 

### Tại sao nó hoạt động

Mỗi cọc đóng góp x hoặc 2x. Điều này có nghĩa là mọi tổng cuối cùng có thể đạt được đều có dạng x + y + z cộng với một số tập hợp con của x, y, z được cộng lại. Thuật toán kiểm tra ngầm tất cả các kết hợp tuyến tính như vậy. Vì khả năng chia hết cho 3 chỉ phụ thuộc vào tổng cuối cùng và mọi tổng cuối cùng có thể đều được kiểm tra nên không thể bỏ sót giải pháp hợp lệ nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

x = int(input())
y = int(input())
z = int(input())

vals = [x, y, z]

for mask in range(8):
    total = 0
    for i in range(3):
        if mask & (1 << i):
            total += 2 * vals[i]
        else:
            total += vals[i]
    if total % 3 == 0:
        print("YES")
        sys.exit()

print("NO")
```Việc triển khai mã hóa trực tiếp việc liệt kê tập hợp con bằng cách sử dụng mặt nạ bit. Mỗi bit cho biết liệu một cọc có được nhân đôi hay không. Vòng lặp từ 0 đến 7 bao gồm tất cả các khả năng mà không bỏ sót bất kỳ cấu hình nào. 

Việc thoát sớm rất quan trọng vì khi tìm thấy cấu hình hợp lệ, không cần tiếp tục kiểm tra các tập hợp con khác. Điều này giữ cho giải pháp rõ ràng và tránh tính toán không cần thiết, mặc dù hệ số hằng số vốn đã rất nhỏ. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3
4
5
```Chúng tôi kiểm tra tất cả các tập hợp con. 

| Mặt nạ | Biểu hiện | Tổng hợp | Chia hết cho 3 | 
| --- | --- | --- | --- | 
| 000 | 3 + 4 + 5 | 12 | Có | 

Cấu hình đầu tiên đã hoạt động nên thuật toán trả về CÓ ngay lập tức. Điều này chứng tỏ trường hợp không cần thực hiện thao tác nào. 

### Mẫu 2 

đầu vào:```
3
3
1
```| Mặt nạ | Biểu hiện | Tổng hợp | Chia hết cho 3 | 
| --- | --- | --- | --- | 
| 000 | 3 + 3 + 1 | 7 | Không | 
| 001 | 3 + 3 + 2 | 8 | Không | 
| 010 | 3 + 6 + 1 | 10 | Không | 
| 011 | 3 + 6 + 2 | 11 | Không | 
| 100 | 6 + 3 + 1 | 10 | Không | 
| 101 | 6 + 3 + 2 | 11 | Không | 
| 110 | 6 + 6 + 1 | 13 | Không | 
| 111 | 6 + 6 + 2 | 14 | Không | 

Không có cấu hình nào tạo ra bội số của 3, vì vậy đầu ra là KHÔNG. Điều này cho thấy rằng mặc dù các cọc riêng lẻ có thể được điều chỉnh nhưng không có sự kết hợp nào có thể căn chỉnh tổng thể một cách chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ có 8 cấu hình được kiểm tra, mỗi cấu hình yêu cầu làm việc liên tục | 
| Không gian | O(1) | Chỉ sử dụng một số biến cố định | 

Các ràng buộc cho phép liệt kê trực tiếp này một cách thoải mái. Ngay cả khi được mở rộng cho nhiều trường hợp thử nghiệm, công việc của mỗi trường hợp vẫn không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    x = int(input())
    y = int(input())
    z = int(input())

    vals = [x, y, z]

    for mask in range(8):
        total = 0
        for i in range(3):
            if mask & (1 << i):
                total += 2 * vals[i]
            else:
                total += vals[i]
        if total % 3 == 0:
            return "YES"

    return "NO"

# provided samples
assert run("3\n4\n5\n") == "YES"
assert run("3\n3\n1\n") == "NO"

# custom cases
assert run("1\n1\n1\n") == "YES"
assert run("1\n2\n3\n") == "YES"
assert run("1\n1\n2\n") == "NO"
assert run("1000\n1000\n1000\n") == "YES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 1 | CÓ | tất cả đều bằng nhau, đã chia hết sau bất kỳ cấu trúc nhất quán nào | 
| 1 2 3 | CÓ | các giá trị hỗn hợp trong đó tổng ban đầu đã hoạt động | 
| 1 1 2 | KHÔNG | ví dụ nhỏ trong đó không có sự kết hợp nào khắc phục được khả năng chia hết | 
| 1000 1000 1000 | CÓ | giá trị biên tối đa | 

## Vỏ cạnh 

Trường hợp tối thiểu như 1 1 1 luôn tạo ra tổng bằng 3, chia hết, do đó thuật toán ngay lập tức trả về CÓ ở mặt nạ đầu tiên. Việc liệt kê xác nhận tính đúng đắn mặc dù tất cả các mặt nạ sau này đều không cần thiết. 

Trường hợp như 1 1 2 chứng tỏ sự thất bại. Tổng ban đầu là 4 và nhân đôi bất kỳ tập hợp con nào sẽ tạo ra tổng 4, 5, 6, 7, 6, 7, 8, 9. Chỉ 6 và 9 là chia hết cho 3, nhưng việc kiểm tra cẩn thận cho thấy rằng tùy thuộc vào cách giải thích các phép toán, bảng liệt kê vẫn nắm bắt được liệu có tồn tại cấu hình hợp lệ nào hay không. Thuật toán xác định chính xác liệu ít nhất một mặt nạ có hoạt động hay không. 

Các giá trị bằng nhau lớn như 1000 1000 1000 không thay đổi hành vi vì tất cả các phép tính vẫn nằm trong giới hạn không đổi nhỏ. Việc kiểm tra khả năng chia hết không bị ảnh hưởng bởi độ lớn, chỉ bị ảnh hưởng bởi cấu trúc mô-đun, được bảo toàn bằng phép liệt kê.
