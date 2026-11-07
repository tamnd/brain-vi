---
title: "CF 1352A - Tổng các số làm tròn"
description: "Nhiệm vụ là chia một số nguyên nhất định thành các khối xây dựng đơn giản hơn, trong đó mỗi khối xây dựng là một số trông giống như một chữ số khác 0 chỉ theo sau là số 0. Đây là những con số như 7, 40, 900 hoặc 3000."
date: "2026-06-16T10:36:34+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 1352
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 640 (Div. 4)"
rating: 800
weight: 1352
solve_time_s: 376
verified: false
draft: false
---

[CF 1352A - Tổng các số làm tròn](https://codeforces.com/problemset/problem/1352/A) 

**Đánh giá:** 800 
**Tags:** thực hiện, toán học 
**Thời gian giải:** 6 phút 16s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ là chia một số nguyên nhất định thành các khối xây dựng đơn giản hơn, trong đó mỗi khối xây dựng là một số trông giống như một chữ số khác 0 chỉ theo sau là số 0. Đây là các số như 7, 40, 900 hoặc 3000. Mục tiêu là biểu thị số đã cho dưới dạng tổng của các số “làm tròn” như vậy trong khi sử dụng càng ít số triệu càng tốt. 

Mỗi trường hợp thử nghiệm cho một số nguyên lên tới 10.000. Đối với mỗi số, chúng ta phải xuất ra bao nhiêu số tròn mà chúng ta đã sử dụng, sau đó là phân tách thực tế. 

Phạm vi ràng buộc đủ nhỏ để chúng ta có thể xử lý từng số một cách độc lập theo thời gian tuyến tính so với số chữ số. Với tối đa 10.000 trường hợp thử nghiệm, mọi giải pháp kiểm tra từng chữ số một lần đều đủ nhanh, trong khi mọi giải pháp liên quan đến tìm kiếm tổ hợp hoặc thử và sai tham lam sẽ không cần thiết và có nguy cơ phức tạp quá mức. 

Trường hợp cạnh tinh vi xuất hiện khi số chứa các số 0 ở giữa các chữ số khác 0. Ví dụ: 101 hoặc 1001. Một cách tiếp cận đơn giản có thể cố gắng nhóm các chữ số thành số tròn lớn hơn một cách không chính xác, chẳng hạn như coi 1001 là 1000 + 1 nhưng vô tình làm mất tính chính xác về vị trí nếu được thực hiện thông qua việc cắt bớt chuỗi hoặc các phím tắt số học. Một sai lầm tiềm ẩn khác là quên rằng mỗi chữ số đóng góp một số tròn riêng biệt ngay cả khi các chữ số lặp lại. 

Ví dụ: trong đầu vào 707, phân tách chính xác là 700 + 7. Việc triển khai bất cẩn cố gắng tạo thành các khối khác 0 liền kề sẽ thử sai 707 dưới dạng một số đơn hoặc bị chia sai, mặc dù nó không tròn. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là thử xây dựng tất cả các tổng số tròn có thể có và chọn tổ hợp nhỏ nhất. Điều đó có nghĩa là tạo ra các ứng cử viên như 1, 2, ..., 9, 10, 20, ..., 9000 và sau đó cố gắng biểu diễn số mục tiêu bằng cách sử dụng tìm kiếm hoặc lập trình động trên các giá trị này. Mặc dù có thể đạt được độ chính xác nhưng không gian trạng thái trở nên lớn một cách không cần thiết và số lượng kết hợp tăng lên nhanh chóng. Mặc dù phạm vi đầu vào nhỏ, cách tiếp cận này gây ra sự phức tạp có thể tránh được trong cả việc thực hiện và lập luận. 

Quan sát quan trọng là mọi số nguyên đều có sự phân tách tự nhiên được căn chỉnh theo giá trị vị trí. Mỗi chữ số ở vị trí i đóng góp độc lập một số có dạng chữ số × 10^i, vốn đã là một số tròn. Điều này loại bỏ mọi nhu cầu tối ưu hóa hoặc tìm kiếm: cách biểu diễn được xác định duy nhất bởi cấu trúc thập phân của số. 

Cách tiếp cận bạo lực không thành công vì nó coi đây là vấn đề lựa chọn tập hợp con, trong khi cấu trúc ký hiệu thập phân đã thực thi trực tiếp giải pháp tối ưu. Khi chúng ta nhận ra rằng mỗi chữ số có thể được tách biệt bởi giá trị vị trí của nó, vấn đề sẽ giảm xuống việc đọc các chữ số và tái tạo lại các số. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (tìm kiếm theo sự kết hợp) | Số mũ trong chữ số | Cao | Quá chậm/không cần thiết | 
| Phân rã chữ số | O(chữ số) | O(chữ số) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Đối với mỗi trường hợp thử nghiệm, chúng tôi xử lý số một cách độc lập.

1. Chuyển số thành chuỗi để chúng ta có thể truy cập từng chữ số với vị trí được giữ nguyên. Điều này rất quan trọng vì giá trị vị trí xác định lũy thừa của mười liên quan đến mỗi chữ số. 
2. Duyệt các chữ số từ phải sang trái (ít quan trọng nhất đến quan trọng nhất). Lý do cho hướng này là vì nó phù hợp một cách tự nhiên với lũy thừa của mười: đơn vị, hàng chục, hàng trăm, v.v. 
3. Với mỗi chữ số, nếu khác 0, hãy lập số tròn bằng cách nhân chữ số đó với lũy thừa mười tương ứng của nó. Ví dụ: chữ số 7 ở hàng trăm trở thành 700. 
4. Thu thập tất cả các giá trị được xây dựng như vậy vào một danh sách. Mỗi chữ số khác 0 đóng góp chính xác một triệu hồi, do đó số thuật ngữ chỉ đơn giản là số lượng các chữ số khác 0. 
5. In ra số lượng các số hạng được thu thập theo sau là các số hạng theo thứ tự bất kỳ. 

Ý tưởng chính là chúng ta không bao giờ cần hợp nhất hoặc chia các chữ số ngoài mức đóng góp vị trí tự nhiên của chúng. 

### Tại sao nó hoạt động 

Mỗi chữ số trong biểu diễn cơ số 10 đóng góp độc lập vào giá trị của số thông qua giá trị vị trí của nó. Viết số dưới dạng tổng của chữ số × 10^số hạng vị trí không phải là một phép biến đổi mà là một đồng nhất thức. Mỗi số hạng như vậy theo định nghĩa là một số tròn vì tất cả các chữ số ngoại trừ số đứng đầu đều bằng 0. Vì sự phân tách này là chính xác và sử dụng chính xác một số hạng cho mỗi chữ số khác 0, nên không có cách biểu diễn nào có thể sử dụng ít số hạng hơn mà không hợp nhất các chữ số, điều này sẽ vi phạm định nghĩa về số tròn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = input().strip()
    
    parts = []
    length = len(n)
    
    for i in range(length):
        digit = int(n[length - 1 - i])
        if digit != 0:
            parts.append(digit * (10 ** i))
    
    print(len(parts))
    print(*parts)
```Việc triển khai phản ánh trực tiếp logic phân tách chữ số. Vòng lặp kết thúc`i`đảm bảo chúng ta gán đúng lũy ​​thừa của mười bắt đầu từ chữ số có nghĩa nhỏ nhất. điều kiện`digit != 0`lọc ra những khoản đóng góp bằng 0 không cần thiết, nếu không sẽ làm tăng số lượng lệnh triệu tập một cách không chính xác. 

Một lỗi phổ biến là lặp từ trái sang phải mà không điều chỉnh chính xác chỉ số công suất, dẫn đến cường độ không chính xác. Một vấn đề khác là xây dựng chuỗi thay vì số nguyên, điều này không cần thiết nhưng vẫn có thể hoạt động nếu được xử lý cẩn thận. 

## Ví dụ đã hoạt động 

Xem xét đầu vào`5009`. 

| Bước | Vị trí | Chữ số | Đóng góp | 
| --- | --- | --- | --- | 
| 1 | 10^0 | 9 | 9 | 
| 2 | 10^1 | 0 | bỏ qua | 
| 3 | 10^2 | 0 | bỏ qua | 
| 4 | 10^3 | 5 | 5000 | 

Đầu ra trở thành:`5000 9`. 

Điều này xác nhận rằng mỗi chữ số ánh xạ độc lập tới một số tròn và các số 0 không đóng góp. 

Bây giờ hãy xem xét`9876`. 

| Bước | Vị trí | Chữ số | Đóng góp | 
| --- | --- | --- | --- | 
| 1 | 10^0 | 6 | 6 | 
| 2 | 10^1 | 7 | 70 | 
| 3 | 10^2 | 8 | 800 | 
| 4 | 10^3 | 9 | 9000 | 

Đầu ra là bốn lệnh triệu tập, mỗi lệnh một chữ số. 

Ví dụ này cho thấy rằng khi tất cả các chữ số khác 0, việc phân tách sử dụng số lượng tối đa các thành phần tròn được phép, vẫn tối ưu vì mỗi chữ số là độc lập. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(d) cho mỗi trường hợp thử nghiệm | Mỗi chữ số được xử lý một lần để xây dựng đóng góp của nó | 
| Không gian | O(d) | Lưu trữ tối đa một thuật ngữ cho mỗi chữ số khác 0 | 

Tổng độ phức tạp trong tất cả các trường hợp thử nghiệm là tuyến tính theo tổng số chữ số trên các đầu vào, điều này không đáng kể dưới các ràng buộc của tối đa 10.000 số, mỗi số có tối đa 4 chữ số. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n = input().strip()
        parts = []
        length = len(n)
        for i in range(length):
            digit = int(n[length - 1 - i])
            if digit != 0:
                parts.append(digit * (10 ** i))
        out.append(str(len(parts)))
        if parts:
            out.append(" ".join(map(str, parts)))
        else:
            out.append("0")
    return "\n".join(out) + "\n"

# provided samples
assert run("5\n5009\n7\n9876\n10000\n10\n") == "2\n5000 9\n1\n7\n4\n800 70 6 9000\n1\n10000\n1\n10\n", "sample test"

# custom cases
assert run("1\n1\n") == "1\n1\n", "minimum case"
assert run("1\n10\n") == "1\n10\n", "single non-zero digit at tens"
assert run("1\n1010\n") == "2\n1000 10\n", "zeros between digits"
assert run("1\n9999\n") == "4\n9 90 900 9000\n", "max decomposition"
assert run("1\n10000\n") == "1\n10000\n", "single high power"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 1 | xử lý số nhỏ nhất | 
| 10 | 10 | một chữ số khác 0 | 
| 1010 | 1000 10 | số không nội bộ | 
| 9999 | 9 90 900 9000 | phân hủy tối đa | 
| 10000 | 10000 | công suất cao duy nhất | 

## Vỏ cạnh 

Đối với một số như 10000, thuật toán đọc các chữ số từ phải sang trái và chỉ tìm một chữ số khác 0 ở vị trí cao nhất. Điều đó tạo ra một số hạng duy nhất 10000 và tất cả các vị trí thấp hơn đều bị bỏ qua. Điều này xác nhận rằng thuật toán không phân chia lũy thừa 10 một cách giả tạo khi không cần thiết. 

Đối với một số như 1010, các chữ số ở hàng chục và hàng nghìn đóng góp 10 và 1000. Các chữ số hàng đơn vị và hàng trăm bằng 0 và bị bỏ qua. Đầu ra tự nhiên trở thành hai lệnh triệu hồi, khớp với định nghĩa của số tròn mà không có bất kỳ sự mơ hồ hoặc cần điều chỉnh nào.
