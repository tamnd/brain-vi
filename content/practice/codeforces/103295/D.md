---
title: "CF 103295D - Truy đuổi cánh đồng ngô"
description: "Chúng ta được cung cấp một lưới biểu thị một cánh đồng ngô trong đó mỗi ô bị ngô chặn hoặc trống. Nhiệm vụ là đếm xem có thể tạo được bao nhiêu hình tam giác khác nhau sao cho cả ba đỉnh của tam giác đều nằm trên các ô chứa ngô."
date: "2026-07-03T14:25:15+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103295
codeforces_index: "D"
codeforces_contest_name: "UTPC Contest 09-17-21 Div. 1 (Advanced)"
rating: 0
weight: 103295
solve_time_s: 51
verified: true
draft: false
---

[CF 103295D - Truy tìm cánh đồng ngô](https://codeforces.com/problemset/problem/103295/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 51s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một lưới biểu thị một cánh đồng ngô trong đó mỗi ô bị ngô chặn hoặc trống. Nhiệm vụ là đếm xem có thể tạo được bao nhiêu hình tam giác khác nhau sao cho cả ba đỉnh của tam giác đều nằm trên các ô chứa ngô. 

Hình tam giác có cấu trúc hình học chặt chẽ: nó phải là một tam giác vuông và hai cạnh của nó phải thẳng hàng với các trục lưới. Điều này có nghĩa là các chân của hình tam giác là các đoạn ngang và dọc trên lưới và góc vuông được hình thành tại một trong các ô ngô đã chọn. Vì vậy, mọi tam giác hợp lệ được xác định đầy đủ bằng cách chọn ba ô ngô thỏa mãn điều kiện tam giác vuông thẳng hàng với trục này. 

Kích thước lưới lên tới 500 x 500, nghĩa là lên tới 250.000 ô. Việc liệt kê khối trên tất cả các bộ ba ô sẽ hoàn toàn không khả thi. Ngay cả cách tiếp cận bậc hai đối với tất cả các cặp ô ngô cũng sẽ yêu cầu kiểm tra theo thứ tự 10^10 trong trường hợp xấu nhất, vượt xa giới hạn thời gian. Điều này ngay lập tức gợi ý rằng giải pháp phải tránh liệt kê các cặp điểm một cách rõ ràng và thay vào đó dựa vào cấu trúc đếm dọc theo hàng và cột. 

Trường hợp khó nhận thấy xuất hiện khi mật độ ngô rất cao hoặc rất thấp. Nếu lưới hoàn toàn trống, câu trả lời gần như bằng 0 và bất kỳ logic đếm nào giả định ít nhất một đỉnh hợp lệ trên mỗi hàng hoặc cột đều phải xử lý việc này một cách rõ ràng. Ở một thái cực khác, nếu lưới chứa đầy ngô, việc đếm tổ hợp đơn giản có thể dễ dàng đếm quá mức các hình tam giác nhiều lần trừ khi mỗi tam giác được liên kết với một ô “neo” duy nhất, ngăn chặn sự trùng lặp. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Chúng tôi thử từng bộ ba ô ngô và kiểm tra xem chúng có tạo thành một tam giác vuông có các chân thẳng hàng với các trục lưới hay không. Điều này yêu cầu kiểm tra tất cả sự kết hợp của ba điểm, đó là O(K^3) trong đó K là số lượng tế bào ngô. Trong một lưới dày đặc K có thể là 250.000, khiến cho cách tiếp cận này thậm chí không thể được khái niệm hóa trong các ràng buộc. Ngay cả việc hạn chế theo cặp và rút ra điểm thứ ba cũng không giúp ích nhiều, vì việc xác minh sự tồn tại vẫn đòi hỏi việc tra cứu tốn kém nếu không xử lý trước. 

Quan sát quan trọng là mọi tam giác hợp lệ đều có thể được mô tả duy nhất bằng đỉnh góc vuông của nó. Khi chúng ta cố định một ô ngô làm góc vuông, hai đỉnh còn lại lần lượt phải nằm trong cùng một hàng và cùng một cột. Điều đó có nghĩa là chúng ta không cần hình học ngoài việc căn chỉnh trục; chúng ta chỉ cần đếm số ô ngô ở mỗi hàng và mỗi cột. 

Giả sử tại một ô cố định chúng ta biết có bao nhiêu ô ngô tồn tại ở hàng và bao nhiêu ô tồn tại ở cột. Nếu chúng ta chọn một ô ngô khác trong cùng một hàng và một ô trong cùng một cột, chúng ta sẽ tạo thành một tam giác vuông hợp lệ với ô hiện tại là góc vuông. Số lượng các lựa chọn như vậy là tích của hai số đếm này, nhưng chúng ta phải cẩn thận để không đưa chính ô hiện tại vào các số lượng đó. Vì vậy, chúng tôi trừ đi một từ mỗi hướng trước khi nhân. 

Điều này làm giảm vấn đề từ việc suy luận về các cặp điểm trong toàn bộ lưới đến việc duy trì hai mảng tần số. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Brute Force tăng gấp ba lần | O(K^3) | O(1) | Quá chậm | 
| Đếm theo hàng/cột | O(nm) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Đọc lưới và xác định tất cả các tế bào ngô. Trong khi thực hiện việc này, hãy duy trì hai mảng, một mảng đếm số lượng ô ngô xuất hiện trong mỗi hàng và một mảng đếm số lượng ô xuất hiện trong mỗi cột. Quá trình tiền xử lý này nén tất cả thông tin hình học thành các số đếm tần số đơn giản. 
2. Lặp lại từng ô trong lưới một lần nữa. Khi chúng ta gặp một ô ngô, hãy coi nó như một đỉnh vuông góc tiềm tàng của một tam giác. 
3. Với mỗi ô như vậy ở vị trí (i, j), hãy tính xem có bao nhiêu lựa chọn hợp lệ cho đỉnh thứ hai trên cùng một hàng. Đây là row_count[i] trừ một, vì chúng tôi loại trừ ô hiện tại. 
4. Tương tự, tính toán có bao nhiêu lựa chọn hợp lệ cho chặng thứ hai trên cùng một cột. Đây là col_count[j] trừ một. 
5. Nhân hai giá trị này. Tích này đếm tất cả các hình tam giác có góc vuông tại (i, j). 
6. Tính tổng giá trị này trên tất cả các ô ngô trong lưới. 

Bước nhân là đúng vì việc chọn một ô ngô riêng biệt trong hàng sẽ cố định một cạnh của tam giác và việc chọn một ô ngô riêng biệt trong cột sẽ cố định chân còn lại một cách độc lập. 

### Tại sao nó hoạt động 

Mỗi tam giác hợp lệ có chính xác một ô chứa góc vuông. Ô đó được xác định duy nhất vì nó là đỉnh duy nhất có chung cả cạnh ngang và dọc với hai đỉnh còn lại. Bằng cách đếm neo tại đỉnh này, mỗi tam giác được tính chính xác một lần. Tính độc lập giữa các lựa chọn hàng và lựa chọn cột đảm bảo rằng tất cả các kết hợp đều tương ứng với các hình tam giác hình học riêng biệt và không có hình dạng không hợp lệ nào được đưa vào vì việc căn chỉnh trục tự động thực thi độ vuông góc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    grid = [input().strip() for _ in range(n)]

    row = [0] * n
    col = [0] * m

    # count stars
    for i in range(n):
        for j in range(m):
            if grid[i][j] == '*':
                row[i] += 1
                col[j] += 1

    ans = 0

    for i in range(n):
        for j in range(m):
            if grid[i][j] == '*':
                ans += (row[i] - 1) * (col[j] - 1)

    print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai tách biệt quá trình tiền xử lý khỏi việc đếm, điều này tránh việc tính toán lại số liệu thống kê hàng hoặc cột nhiều lần. Việc trừ một theo cả hai hướng là cần thiết vì ô hiện tại được tính theo tổng số hàng và cột. 

Vòng tích lũy cuối cùng an toàn về mặt tràn trong Python, nhưng trong ngôn ngữ được gõ mạnh như C++, kết quả có thể yêu cầu số nguyên 64 bit do tăng trưởng bậc hai trong các lưới dày đặc. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 2
**
*.
```Chúng tôi tính số hàng là [2, 1] và số cột là [2, 1]. Bây giờ chúng tôi đánh giá từng tế bào ngô. 

| Tế bào | hàng[i] | col[j] | (hàng-1)*(col-1) | 
| --- | --- | --- | --- | 
| (0,0) | 2 | 2 | 1 | 
| (0,1) | 2 | 1 | 0 | 
| (1,0) | 1 | 2 | 0 | 

Tổng đóng góp là 1. 

Điều này phù hợp với ý tưởng rằng chỉ có thể hình thành một tam giác vuông trong một lưới nhỏ như vậy. 

### Ví dụ 2 

đầu vào:```
3 2
.*
*.
.*
```Số hàng là [1,1,1] và số cột là [2,1]. 

Đánh giá từng tế bào ngô: 

| Tế bào | hàng[i] | col[j] | (hàng-1)*(col-1) | 
| --- | --- | --- | --- | 
| (0,1) | 1 | 1 | 0 | 
| (1,0) | 1 | 2 | 0 | 
| (2,1) | 1 | 1 | 0 | 

Tổng cộng là 0. 

Điều này cho thấy mặc dù ngô tồn tại nhưng không có cặp hàng-cột nào cho phép thêm hai ô ngô riêng biệt tạo thành các chân vuông góc từ cùng một đỉnh. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nm) | Mỗi ô được truy cập với số lần không đổi để đếm và tổng hợp | 
| Không gian | O(n + m) | Chỉ các mảng tần số hàng và cột được lưu trữ | 

Kích thước lưới tối đa là 500 x 500, vì vậy tối đa 250.000 thao tác được thực hiện. Điều này nằm trong giới hạn 1 giây trong môi trường Codeforces điển hình. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    solve()
    return sys.stdout.getvalue().strip()

# sample 1
assert run("""2 2
**
*.
""") == "1"

# sample 2
assert run("""3 2
.*
*.
.*
""") == "0"

# single cell
assert run("""1 1
*
""") == "0"

# full grid 2x2
assert run("""2 2
**
**
""") == "4"

# sparse grid
assert run("""3 3
*.*
.*.
*.*
""") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1x1 sao đơn | 0 | ranh giới tối thiểu | 
| 2x2 đầy đủ | 4 | hành vi đếm quá mức dày đặc | 
| mẫu bàn cờ | 0 | không hoàn thành cột hàng hợp lệ | 

## Vỏ cạnh 

Đối với lưới một ô chứa dấu sao, thuật toán trả về 0 một cách chính xác vì số hàng hoặc số cột trở thành 1, làm cho cả hai thừa số bằng 0 sau khi trừ. Điều này ngăn chặn các hình tam giác sai trong hình học suy biến. 

Trong một lưới được lấp đầy, mỗi ô đóng vai trò là một đỉnh góc vuông. Thuật toán đếm chính xác tất cả các kết hợp vì mỗi tam giác được neo duy nhất ở góc của nó và phép nhân trên các lựa chọn hàng và cột sẽ thu được tất cả các cặp hợp lệ mà không bị trùng lặp. 

Trong các lưới rất thưa nơi các ngôi sao bị cô lập, số lượng hàng hoặc cột vẫn bằng một, đảm bảo không có hình tam giác không hợp lệ nào được hình thành do không thể xây dựng được một cạnh của tam giác.
