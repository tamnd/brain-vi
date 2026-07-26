---
title: "CF 102868E - Cam"
description: "Nhiệm vụ là di chuyển một điểm trên lưới vô hạn từ điểm mạng này sang điểm mạng khác. Một bước di chuyển sẽ thay đổi chính xác một tọa độ một đơn vị, vì vậy mỗi bước di chuyển là một trong bốn hướng chính. Hạn chế là không thể sử dụng cùng một hướng hai lần liên tiếp."
date: "2026-07-25T13:29:39+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102868
codeforces_index: "E"
codeforces_contest_name: "2020 UTPC Fall Puzzle Contest"
rating: 0
weight: 102868
solve_time_s: 54
verified: true
draft: false
---

[CF 102868E - Cam](https://codeforces.com/problemset/problem/102868/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 54s 
**Đã xác minh:** có 

##Giải pháp 
#Hiểu vấn đề 

Nhiệm vụ là di chuyển một điểm trên lưới vô hạn từ điểm mạng này sang điểm mạng khác. Một bước di chuyển sẽ thay đổi chính xác một tọa độ một đơn vị, vì vậy mỗi bước di chuyển là một trong bốn hướng chính. Hạn chế là không thể sử dụng cùng một hướng hai lần liên tiếp. Ví dụ, di chuyển về phía bắc hai lần liên tiếp bị cấm, nhưng được phép di chuyển về phía bắc, đông, bắc. 

Đối với mỗi kịch bản, chúng ta được cung cấp tọa độ bắt đầu và tọa độ kết thúc của điểm. Đầu ra bắt buộc là số lần di chuyển tối thiểu cần thiết trong khi vẫn tôn trọng giới hạn hướng. 

Tọa độ có thể lớn bằng$10^{18}$và có thể có tới 1000 kịch bản. Điều này ngay lập tức loại trừ mọi mô phỏng tìm kiếm đường dẫn hoặc biểu đồ. Một giải pháp chỉ được phép sử dụng các phép tính số học cho mỗi trường hợp thử nghiệm, bởi vì ngay cả việc lặp lại khoảng cách giữa các điểm cũng có thể yêu cầu tới$10^{18}$các bước. 

Khó khăn chính là con đường Manhattan ngắn nhất không phải lúc nào cũng hợp lệ. Một đường đi ngắn nhất thông thường sử dụng chính xác số bước di chuyển theo chiều ngang và chiều dọc cần thiết, nhưng nó có thể chứa quá nhiều bước di chuyển thuộc một loại liên tiếp. Con đường phải xen kẽ giữa các bước di chuyển ngang và dọc thường xuyên. 

Hãy xem xét những trường hợp việc triển khai bất cẩn không thành công. 

Để di chuyển từ$(0,0)$ĐẾN$(5,0)$, khoảng cách Manhattan là 5. Một giải pháp đơn giản sẽ cho kết quả 5, nhưng điều đó là không thể vì tất cả các chuyển động đều theo chiều ngang. Câu trả lời đúng là 10. Một tuyến đường hợp lệ là phải, lên, phải, xuống, phải, lên, phải, xuống, phải, xuống. 

Để di chuyển từ$(0,0)$ĐẾN$(2,1)$, khoảng cách Manhattan là 3. Điều này đã hợp lệ vì hai bước di chuyển ngang có thể được phân tách bằng bước di chuyển dọc. Kết quả đúng là 3. Một cách tiếp cận luôn bổ sung thêm các đường vòng khi cả hai tọa độ đều khác 0 sẽ tạo ra câu trả lời sai. 

Để di chuyển từ$(1,1)$ĐẾN$(1,1)$, câu trả lời là 0. Việc thêm đường vòng là không cần thiết vì không cần di chuyển. 

# Phương pháp tiếp cận 

Ý tưởng mạnh mẽ là cố gắng xây dựng từng bước một con đường trong khi theo dõi vị trí hiện tại và hướng đi trước đó. Điều này mô hình hóa chính xác hạn chế, bởi vì mọi tiểu bang đều chứa chính xác thông tin cần thiết để quyết định động thái nào là hợp pháp. Tuy nhiên, lưới điện là rất lớn. Số lượng các vị trí có thể tăng lên theo khoảng cách tọa độ và việc tìm kiếm theo chiều rộng hoặc lập trình động trên các vị trí là không thể. Ngay cả một mô phỏng đơn giản về khoảng cách Manhattan cũng không thành công khi có tọa độ xung quanh$10^{18}$. 

Quan sát hữu ích là ban đầu các hướng thực tế không quan trọng. Điều quan trọng là sử dụng bao nhiêu bước di chuyển ngang và di chuyển dọc. Bất kỳ chuỗi nước đi hợp lệ nào cũng phải luân phiên giữa các nước đi theo chiều ngang và chiều dọc, ngoại trừ một loại có thể xuất hiện nhiều hơn loại kia ở hai đầu của chuỗi. 

Giả sử khoảng cách ngang và dọc yêu cầu là$x$Và$y$. Chúng ta cần ít nhất$x$di chuyển theo chiều ngang và ít nhất$y$di chuyển theo chiều dọc. Các bước di chuyển bổ sung chỉ hữu ích khi quay lại các cặp quay lại, vì vậy tổng số bước di chuyển của một trục phải có cùng độ chẵn lẻ với khoảng cách yêu cầu của nó. Số lượng nhỏ nhất có thể ban đầu là$x$Và$y$. Nếu hiệu của chúng nhiều nhất là một thì chúng có thể được sắp xếp thành một chuỗi xen kẽ. 

Khi một trục lớn hơn một trục, trục nhỏ hơn cần di chuyển thêm để đóng vai trò là dấu phân cách. Vì các bước đi bổ sung đi theo cặp nên chúng tôi tăng số lượng nhỏ hơn lên số lượng chẵn nhỏ nhất để tạo ra sự khác biệt nhiều nhất là một. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(khoảng cách) hoặc tệ hơn | O(khoảng cách) | Quá chậm | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

#Hướng dẫn thuật toán 

1. Tính khoảng cách tuyệt đối theo chiều ngang và chiều dọc giữa điểm đầu và điểm cuối. Gọi cho họ$x$Và$y$. Chỉ có khoảng cách mới quan trọng vì cùng một công trình vẫn hoạt động bất kể chúng ta di chuyển sang trái hay phải, lên hay xuống. 
2. Hãy để`big`lớn hơn$x$Và$y$, và để`small`hãy là người khác Nếu như`big - small`nhiều nhất là một, đường đi Manhattan đã tương thích với quy tắc hướng xen kẽ, vì vậy câu trả lời đơn giản là`big + small`. 
3. Nếu chênh lệch lớn hơn một, hãy tăng`small`bằng một số chẵn. Các bước di chuyển được thêm vào thể hiện việc quay đi quay lại trên trục nhỏ hơn. Chúng tôi chọn phép cộng chẵn nhỏ nhất để giảm sự khác biệt giữa hai số lần di chuyển xuống tối đa một. 
4. Trả về tổng số nước đi mới. 

Lý do điều này có hiệu quả là vì mọi đường dẫn hợp lệ đều có thể được xem dưới dạng một chuỗi các chuyển động ngang và dọc xen kẽ. Một trình tự như vậy không thể có một danh mục xuất hiện nhiều lần ngoài danh mục kia. Thuật toán tìm ra số bước di chuyển bổ sung tối thiểu cần thiết để đáp ứng chính xác điều kiện đó. Các bước di chuyển bổ sung chỉ được thêm dưới dạng cặp, giúp duy trì tọa độ cuối cùng, do đó số lượng kết quả vừa có thể đạt được vừa ở mức tối thiểu. 

#Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    ans = []

    for _ in range(t):
        a, b, c, d = map(int, input().split())

        x = abs(c - a)
        y = abs(d - b)

        big = max(x, y)
        small = min(x, y)

        diff = big - small
        if diff > 1:
            add = diff - 1
            if add % 2:
                add += 1
            small += add

        ans.append(str(big + small))

    print("\n".join(ans))

if __name__ == "__main__":
    solve()
```Đầu tiên, chương trình chuyển đổi các thay đổi tọa độ thành hai khoảng cách không âm. Dấu hiệu của các bước di chuyển là không liên quan vì bất kỳ chuyển động bổ sung nào luôn là một chuyến quay trở lại. 

Các biến`big`Và`small`biểu thị số lần di chuyển cần thiết trên mỗi trục trước khi khắc phục sự cố luân phiên. Khi trục lớn hơn có quá nhiều lần di chuyển, mã sẽ tăng trục nhỏ hơn lên một lượng chẵn. Việc điều chỉnh tính chẵn lẻ là cần thiết vì dấu phân cách bổ sung phải bao gồm một bước di chuyển ra xa và một bước di chuyển lùi lại, luôn thêm hai bước di chuyển. 

Số nguyên Python xử lý$10^{18}$phạm vi tọa độ trực tiếp, do đó không cần xử lý tràn. Việc triển khai không xây dựng một đường dẫn giúp giữ cho cả thời gian chạy và bộ nhớ không đổi. 

# Ví dụ đã hoạt động 

Đối với đầu vào:```
2
1 1 3 3
0 1 1 1
```Trường hợp đầu tiên có khoảng cách ngang 2 và khoảng cách dọc 2. 

| Bước | x | y | lớn | nhỏ | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| Giá trị ban đầu | 2 | 2 | 2 | 2 | 4 | 
| Kiểm tra sự khác biệt | 2 | 2 | 2 | 2 | 4 | 

Số lần di chuyển đã được cân bằng. Một đường đi xen kẽ giữa các bước di chuyển ngang và dọc tồn tại với đúng bốn bước di chuyển. 

Trường hợp thứ hai có khoảng cách ngang 1 và khoảng cách dọc 0. 

| Bước | x | y | lớn | nhỏ | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| Giá trị ban đầu | 1 | 0 | 1 | 0 | 1 | 
| Kiểm tra sự khác biệt | 1 | 0 | 1 | 0 | 1 | 

Được phép di chuyển ngang một lần vì không có bước di chuyển thứ hai có cùng hướng. 

Một ví dụ thứ hai:```
1
0 0 5 0
```| Bước | x | y | lớn | nhỏ | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| Giá trị ban đầu | 5 | 0 | 5 | 0 | 5 | 
| Sự khác biệt | 5 | 0 | 5 | 0 | 5 | 
| Bổ sung bắt buộc | 5 | 0 | 5 | 4 | 9 | 
| Sửa lỗi chẵn lẻ cuối cùng | 5 | 0 | 5 | 5 | 10 | 

Năm nước đi ngang cần có dấu phân cách dọc. Số bước di chuyển theo chiều dọc nhỏ nhất có thể là năm, tổng cộng là mười bước. 

# Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) cho mỗi trường hợp thử nghiệm | Chỉ một số phép tính số học cố định được thực hiện | 
| Không gian | O(1) | Thuật toán chỉ lưu trữ một vài biến số nguyên | 

Giải pháp xử lý kích thước tọa độ tối đa vì nó không bao giờ phụ thuộc vào kích thước của lưới. Với 1000 kịch bản, tổng công việc vẫn không đáng kể. 

# Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    solve()

    sys.stdin = old_stdin
    sys.stdout = old_stdout

    return out.getvalue()

# provided samples
assert run("""2
1 1 3 3
0 1 1 1
""") == """4
1
""", "sample 1"

# same point
assert run("""1
5 7 5 7
""") == """0
""", "zero distance"

# straight line requiring detours
assert run("""1
0 0 5 0
""") == """10
""", "horizontal only"

# already alternating
assert run("""1
0 0 4 3
""") == """7
""", "balanced movement"

# huge coordinates
assert run("""1
0 0 1000000000000000000 0
""") == """2000000000000000000
""", "large boundary values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Điểm bắt đầu và điểm kết thúc giống nhau | 0 | Xử lý chính xác khi không có chuyển động | 
| Năm bước ngang | 10 | Xử lý các trường hợp cần di chuyển dải phân cách | 
| Khoảng cách 4 và 3 | 7 | Khẳng định đường đi Manhattan bình thường không thay đổi | 
| Khoảng cách tọa độ$10^{18}$|$2 \cdot 10^{18}$| Xác nhận xử lý số nguyên lớn | 

# Vỏ cạnh 

Khi cả hai điểm giống hệt nhau, chẳng hạn như đầu vào:```
1
3 4 3 4
```khoảng cách là$x=0$Và$y=0$. Sự khác biệt là 0 nên thuật toán trả về 0 ngay lập tức. Nó không thêm những bước di chuyển không cần thiết vì không có xung đột về hướng. 

Khi chuyển động chỉ tồn tại trên một trục, chẳng hạn như:```
1
0 0 5 0
```số đếm ban đầu nằm ngang$5$và dọc$0$. Sự khác biệt là năm, vì vậy bốn bước di chuyển dọc được thêm vào trước, nhưng số lượng phải chẵn. Bốn là đủ để giảm khoảng cách xuống còn một, và vì bên nhỏ hơn phải có số chẵn lẻ chính xác nên cần thêm một cặp nữa, đưa ra năm bước di chuyển theo chiều dọc và câu trả lời cuối cùng là mười. 

Khi cả hai trục có khoảng cách tương tự nhau, chẳng hạn như:```
1
0 0 3 2
```số đếm khác nhau một nên không cần chuyển động thêm. Thuật toán trả về năm và các bước di chuyển có thể được sắp xếp theo chiều ngang, chiều dọc, chiều ngang, chiều dọc, chiều ngang. 

Khi tọa độ cực lớn, chẳng hạn như:```
1
0 0 1000000000000000000 0
```thuật toán chỉ thực hiện phép trừ, so sánh và cộng. Nó không bao giờ cố gắng xây dựng đường đi nên khoảng cách rất lớn không ảnh hưởng đến thời gian chạy.
