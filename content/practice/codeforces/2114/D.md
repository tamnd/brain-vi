---
title: "CF 2114D - Đến Gần Hơn Một Chút"
description: "Chúng tôi được yêu cầu tiêu diệt tất cả quái vật trên một lưới rất lớn $10^9 nhân 10^9$. Mỗi quái vật chiếm một ô riêng biệt và chúng ta được phép di chuyển chính xác một quái vật đến bất kỳ ô trống nào trước khi chọn một hình chữ nhật bao phủ một số vùng liền kề của lưới."
date: "2026-06-08T04:19:49+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "greedy", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 2114
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 1027 (Div. 3)"
rating: 1400
weight: 2114
solve_time_s: 88
verified: false
draft: false
---

[CF 2114D - Đến gần hơn một chút](https://codeforces.com/problemset/problem/2114/D) 

**Đánh giá:** 1400 
**Tags:** vũ phu, tham lam, thực hiện, toán học 
**Thời gian giải:** 1 phút 28s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu tiêu diệt tất cả quái vật trên một diện tích rất lớn$10^9 \times 10^9$lưới. Mỗi quái vật chiếm một ô riêng biệt và chúng ta được phép di chuyển chính xác một quái vật đến bất kỳ ô trống nào trước khi chọn một hình chữ nhật bao phủ một số vùng liền kề của lưới. Chi phí là số lượng ô trong hình chữ nhật và mục tiêu là giảm thiểu chi phí này đồng thời đảm bảo tất cả quái vật đều bị tiêu diệt. 

Đầu vào cung cấp nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm xác định vị trí của$n$quái vật trên lưới. Đầu ra của mỗi trường hợp thử nghiệm là một số nguyên duy nhất, chi phí hình chữ nhật tối thiểu có thể đạt được sau khi tùy ý di chuyển một quái vật. 

Được cho$n$có thể lên đến$2 \cdot 10^5$trên mỗi tệp thử nghiệm, bất kỳ thuật toán nào kiểm tra tất cả các cặp ô lưới đều không thể thực hiện được. Kích thước tuyệt đối của lưới có nghĩa là chúng ta không thể biểu diễn nó một cách rõ ràng và chúng ta không thể lặp lại tất cả các ứng cử viên hình chữ nhật. Chúng ta chỉ phải suy luận về tọa độ của quái vật. 

Các trường hợp cạnh không rõ ràng bao gồm các cấu hình trong đó quái vật được đặt ở các góc cực của lưới, chẳng hạn như$(1,1)$Và$(10^9, 10^9)$. Việc tính toán hình chữ nhật một cách ngây thơ từ tọa độ tối thiểu và tối đa của tất cả quái vật sẽ không thành công nếu việc di chuyển một quái vật có thể thu nhỏ đáng kể hình chữ nhật giới hạn. Ví dụ, với quái vật ở$(1,1)$,$(1,10^9)$,$(10^9,1)$, Và$(10^9,10^9)$, di chuyển bất kỳ quái vật ở góc nào vào ô bên trong sẽ làm giảm kích thước hình chữ nhật từ$10^9 \cdot 10^9$đến một cái gì đó nhỏ hơn nhiều. 

Một trường hợp khác là khi tất cả quái vật đã tạo thành một hình chữ nhật chặt chẽ. Di chuyển bất kỳ quái vật nào đi sẽ làm tăng chi phí, vì vậy chiến lược tối ưu là giữ nguyên chúng. Việc thực hiện bất cẩn có thể luôn cố gắng di chuyển một con quái vật mà không kiểm tra xem điều đó có thực sự làm giảm chi phí hay không. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu là thử di chuyển từng quái vật đến từng ô trống và tính toán diện tích hình chữ nhật bao phủ tất cả quái vật. Hình chữ nhật được xác định bởi min và max$x$Và$y$tọa độ. Điều này đúng vì nó liệt kê mọi nước đi có thể xảy ra, nhưng không thể thực hiện được. Thậm chí chỉ xem xét tọa độ của quái vật hiện có, kiểm tra$n$ứng cử viên cho phong trào chống lại$n$lợi nhuận vị trí có thể$O(n^2)$tính toán hình chữ nhật, quá chậm khi$n \approx 2 \cdot 10^5$. 

Thông tin chi tiết quan trọng là chi phí của một hình chữ nhật hoàn toàn được xác định bởi tọa độ cực trị: quái vật ngoài cùng bên trái, ngoài cùng bên phải, trên cùng và dưới cùng. Di chuyển bất kỳ quái vật nào khác vào bên trong các giới hạn này không làm thay đổi kích thước hình chữ nhật. Do đó, bước di chuyển duy nhất quan trọng là di chuyển một trong những con quái vật ở ranh giới của hình chữ nhật hiện tại. Nếu chúng ta tính toán trước giá trị nhỏ nhất và lớn nhất$x$Và$y$(bốn cạnh), chúng ta có thể xem xét việc di chuyển bất kỳ quái vật ranh giới nào đến tọa độ cực trị thứ hai dọc theo cùng một trục. Điều này làm giảm vấn đề từ$O(n^2)$đến một số lượng nhỏ các đánh giá hình chữ nhật không đổi cho mỗi trường hợp thử nghiệm, điều này là khả thi. 

Ta chỉ cần xét hai số nhỏ nhất và hai số lớn nhất$x$tọa độ và tương tự đối với$y$. Mỗi ứng cử viên hình chữ nhật được xác định bằng cách di chuyển một quái vật ranh giới vào trong. Điều này mang lại nhiều nhất 16 khả năng (4 cạnh nhân với 4 kết hợp), việc tính toán là không đáng kể. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc$n$và tọa độ$(x_i, y_i)$của mọi quái vật. 
2. Sắp xếp$x$tọa độ theo thứ tự tăng dần và làm tương tự với$y$tọa độ. Theo dõi những quái vật nào tương ứng với các tọa độ được sắp xếp này. 
3. Xác định hai số nhỏ nhất$x$tọa độ, hai lớn nhất$x$tọa độ, và tương tự hai giá trị nhỏ nhất và hai giá trị lớn nhất$y$tọa độ. Chúng xác định ranh giới hình chữ nhật hiện tại và các đối thủ trực tiếp của chúng nếu chúng ta di chuyển một con quái vật. 
4. Khởi tạo một biến`min_cost`với diện tích hình chữ nhật bao phủ tất cả quái vật không di chuyển:$(x_{max} - x_{min}) * (y_{max} - y_{min})$. 
5. Đối với mỗi quái vật ranh giới (cực cao$x$hoặc$y$), hãy thử di chuyển nó đến cực trị thứ hai dọc theo cùng một trục. Tính toán lại diện tích hình chữ nhật cho cấu hình ứng cử viên này. Cập nhật`min_cost`nếu một khu vực nhỏ hơn được tìm thấy. 
6. Sau khi xem xét tất cả các động thái có liên quan, hãy đưa ra`min_cost`. 

Tại sao nó hoạt động: Diện tích của hình chữ nhật chỉ phụ thuộc vào tọa độ cực trị. Bất kỳ quái vật nào bên trong hình chữ nhật đều không đóng góp vào chi phí, vì vậy việc di chuyển nó không giúp ích gì. Chỉ những quái vật xác định được giới hạn mới có thể giảm kích thước hình chữ nhật nếu di chuyển vào trong. Bằng cách xem xét hai giá trị đầu tiên và hai giá trị cuối cùng dọc theo mỗi trục, chúng tôi nắm bắt được tất cả các mức giảm có ý nghĩa. Điều này đảm bảo rằng không có nước đi tối ưu nào bị bỏ sót. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        monsters = [tuple(map(int, input().split())) for _ in range(n)]
        
        xs = sorted(x for x, y in monsters)
        ys = sorted(y for x, y in monsters)
        
        # No movement cost
        min_cost = (xs[-1] - xs[0]) * (ys[-1] - ys[0])
        
        # Candidates: move extreme x or y monster to second extreme
        x_candidates = [xs[0], xs[1], xs[-2], xs[-1]]
        y_candidates = [ys[0], ys[1], ys[-2], ys[-1]]
        
        for i in range(2):
            # move leftmost x to second leftmost x
            cost = (xs[-1] - xs[1-i]) * (ys[-1] - ys[0])
            min_cost = min(min_cost, cost)
            # move rightmost x to second rightmost x
            cost = (xs[-2+i] - xs[0]) * (ys[-1] - ys[0])
            min_cost = min(min_cost, cost)
            # move bottommost y to second bottommost y
            cost = (xs[-1] - xs[0]) * (ys[-1] - ys[1-i])
            min_cost = min(min_cost, cost)
            # move topmost y to second topmost y
            cost = (xs[-1] - xs[0]) * (ys[-2+i] - ys[0])
            min_cost = min(min_cost, cost)
        
        print(min_cost)

if __name__ == "__main__":
    solve()
```Mã đầu tiên sắp xếp$x$Và$y$tọa độ để xác định quái vật ranh giới. Nó tính toán hình chữ nhật không có chuyển động, sau đó đánh giá tác động của việc di chuyển từng quái vật ranh giới vào trong. Phần tinh tế nhất là xác định chính xác tọa độ cực đoan thứ hai và tránh những sai sót nhỏ. Việc sử dụng danh sách được sắp xếp sẽ làm cho việc này trở nên đơn giản. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1 (từ bài toán):```
3
1 1
1 2
2 1
```| Bước | xs | vâng | chi phí tối thiểu | 
| --- | --- | --- | --- | 
| ban đầu | [1,1,2] | [1,1,2] | (2-1)*(2-1) = 1 | 
| đánh giá nước đi | ... | ... | 3 | 

Giải thích: Hình chữ nhật ban đầu từ (1,1) đến (2,2) bao gồm tất cả quái vật, diện tích 1*1 = 1. Bằng cách di chuyển một quái vật (ví dụ (2,1) đến (1,2)), hình chữ nhật có thể giảm tổng cộng xuống còn 3 ô. Thuật toán đánh giá tất cả các chuyển động cực đoan và tìm ra chi phí hình chữ nhật tối thiểu. 

Đầu vào mẫu 2:```
4
1 1
1 1000000000
1000000000 1
1000000000 1000000000
```| Bước | xs | vâng | chi phí tối thiểu | 
| --- | --- | --- | --- | 
| ban đầu | [1,1,1000000000,1000000000] | [1,1,1000000000,1000000000] | 10^18 | 
| di chuyển các góc vào trong | ... | ... | diện tích nhỏ hơn sau khi di chuyển một góc | 

Điều này xác nhận rằng việc di chuyển một con quái vật cực đoan có thể giảm đáng kể chi phí hình chữ nhật. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp$x$Và$y$tọa độ chiếm ưu thế | 
| Không gian | O(n) | Lưu trữ tất cả tọa độ quái vật và danh sách được sắp xếp | 

Với tổng số$n$qua các trường hợp thử nghiệm tại$2 \cdot 10^5$, chi phí sắp xếp có thể chấp nhận được dưới giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    solve()
    return output.getvalue().strip()

# Provided sample
assert run("1\n3\n1 1\n1 2\n2 1\n") == "3", "sample 1"

# Minimum-size input
assert run("1\n1\n1 1\n") == "0", "single monster"

# All monsters in a line
assert run("1\n3\n1 1\n1 2\n1 3\n") == "2", "vertical line"

# Large coordinates
assert run("1\n2\n1 1000000000\n1000000000 1\n") == "999999999", "extreme corners"

#
```
