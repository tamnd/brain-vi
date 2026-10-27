---
title: "CF 106142J - \u0420\u0430\u0437\u043c\u0435\u0449\u0435\u043d\u0438\u0435 \u043f\u043e\u0441\u0442\u0440\u043e\u0439\u043a\u0438"
description: "Chúng ta có một lưới hình chữ nhật rất lớn có n hàng và m cột. Một số ô bị chặn bởi đá, và tất cả các ô còn lại đều tự do."
date: "2026-06-21T09:35:28+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106142
codeforces_index: "J"
codeforces_contest_name: "2025-2026 \u0412\u0441\u0435\u0440\u043e\u0441\u0441\u0438\u0439\u0441\u043a\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u0448\u043a\u043e\u043b\u044c\u043d\u0438\u043a\u043e\u0432 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e, \u0440\u0435\u0433\u0438\u043e\u043d\u0430\u043b\u044c\u043d\u044b\u0439 \u044d\u0442\u0430\u043f \u0421\u0430\u0440\u0430\u0442\u043e\u0432\u0441\u043a\u043e\u0439 \u043e\u0431\u043b\u0430\u0441\u0442\u0438 (\u0412\u041a\u041e\u0428\u041f 25, \u0421\u0430\u0440\u0430\u0442\u043e\u0432\u0441\u043a\u0438\u0439 \u043e\u0442\u0431\u043e\u0440\u043e\u0447\u043d\u044b\u0439 \u044d\u0442\u0430\u043f)"
rating: 0
weight: 106142
solve_time_s: 62
verified: true
draft: false
---

[CF 106142J - \u0420\u0430\u0437\u043c\u0435\u0449\u0435\u043d\u0438\u0435 \u043f\u043e\u0441\u0442\u0440\u043e\u0439\u043a\u0438](https://codeforces.com/problemset/problem/106142/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 2s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một lưới hình chữ nhật rất lớn với`n`hàng và`m`cột. Một số ô bị chặn bởi đá, và tất cả các ô còn lại đều tự do. Nhiệm vụ là đếm xem có bao nhiêu vị trí riêng biệt của một`2 × 2`hình vuông tồn tại sao cho cả bốn ô của hình vuông đều nằm bên trong lưới và không ô nào chứa đá. 

Một vị trí của`2 × 2`hình vuông được xác định đầy đủ bởi ô trên cùng bên trái của nó. Nếu góc trên bên trái là`(i, j)`, thì hình vuông bao phủ`(i, j)`,`(i, j+1)`,`(i+1, j)`,`(i+1, j+1)`. Do đó, các vị trí hợp lệ tương ứng chính xác với các góc trên cùng bên trái trong phạm vi`1 ≤ i ≤ n-1`Và`1 ≤ j ≤ m-1`sao cho tất cả bốn ô được che đều trống. 

Các ràng buộc là cực kỳ lớn đối với kích thước lưới, với`n`Và`m`lên tới một tỷ. Điều này làm cho bất kỳ cách tiếp cận nào lặp lại trên lưới đều không thể thực hiện được. Số lượng ô bị chặn tối đa là 200.000, đủ nhỏ để lặp lại trực tiếp. Sự mất cân bằng này là gợi ý về cấu trúc trung tâm: giải pháp phải chỉ phụ thuộc vào các ô bị chặn chứ không phụ thuộc vào kích thước lưới. 

Một nỗ lực ngây thơ sẽ liệt kê tất cả`(n-1)(m-1)`khả thi`2 × 2`vị trí và kiểm tra xem mỗi vị trí có hợp lệ hay không. Điều này là không khả thi vì lưới có thể chứa tới`10^18`các vị trí ứng cử viên. 

Ý tưởng ngây thơ thứ hai là lặp lại từng ô bị chặn và đánh dấu tất cả các ô bị ảnh hưởng`2 × 2`hình vuông. Điều này gần đúng hơn nhưng cần phải cẩn thận vì nhiều viên đá có thể làm mất hiệu lực của cùng một hình vuông, do đó, các bản sao phải được xử lý đúng cách. 

Các trường hợp cạnh thường phá vỡ các giải pháp không chính xác bao gồm lưới không có đá, lưới có đá nằm trên đường viền (ảnh hưởng đến ít ô vuông hơn) và trường hợp nhiều đá góp phần vào cùng một giải pháp không hợp lệ.`2 × 2`vùng đất. Ví dụ, nếu hai hòn đá nằm trong cùng một`2 × 2`diện tích, hình vuông đó vẫn chỉ được tính một lần là không hợp lệ. 

## Phương pháp tiếp cận 

Phương pháp đếm trực tiếp bắt đầu từ việc quan sát rằng mọi vị trí hợp lệ đều tương ứng với góc trên bên trái của`2 × 2`khối. Không có viên đá nào, câu trả lời đơn giản là`(n - 1) × (m - 1)`. 

Việc đưa đá vào chỉ loại bỏ một số vị trí này. Vị trí sẽ không hợp lệ nếu ít nhất một trong bốn ô của nó chứa đá. Thay vì kiểm tra tất cả các vị trí, chúng tôi đảo ngược quan điểm: bắt đầu từ tất cả các vị trí có thể và trừ đi những vị trí bị chặn. 

Khó khăn chính là tránh tính hai lần. Mỗi viên đá có thể ảnh hưởng đến bốn góc trên bên trái của một`2 × 2`hình vuông bao gồm nó. Nếu chúng tôi thu thập tất cả các vị trí bị ảnh hưởng như vậy và coi chúng là các vị trí không hợp lệ, thì các bản sao sẽ tự nhiên xuất hiện khi nhiều viên đá ảnh hưởng đến cùng một hình vuông. Điều này gợi ý sử dụng một bộ để lưu trữ tất cả các góc trên cùng bên trái không hợp lệ. 

Câu trả lời cuối cùng là tổng số vị trí trừ đi số vị trí không hợp lệ riêng biệt được thu thập từ tất cả các viên đá. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên tất cả các ô vuông | O(nm) | O(1) | Quá chậm | 
| Liệt kê các ô vuông bị ảnh hưởng từ đá | O(k) trung bình | O(k) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính tổng số có thể`2 × 2`vị trí trong một lưới trống như`(n - 1) × (m - 1)`. Điều này thể hiện tất cả các góc trên bên trái có khả năng chứa một hình vuông. 
2. Duy trì một bộ sẽ lưu trữ tất cả các góc trên bên trái của`2 × 2`các ô vuông không hợp lệ do có ít nhất một viên đá. 
3. Đối với mỗi viên đá nằm ở`(x, y)`, xác định xem`2 × 2`hình vuông mà nó có thể thuộc về. Một hòn đá có thể nằm trong một hình vuông có góc trên bên trái là một trong`(x, y)`,`(x-1, y)`,`(x, y-1)`,`(x-1, y-1)`miễn là các tọa độ đó nằm trong giới hạn góc trên bên trái hợp lệ. 
4. Đối với mỗi ứng cử viên như vậy, góc trên bên trái`(i, j)`, kiểm tra xem`1 ≤ i ≤ n-1`Và`1 ≤ j ≤ m-1`. Nếu hợp lệ thì chèn`(i, j)`vào tập hợp các vị trí không hợp lệ. 
5. Sau khi xử lý tất cả các viên đá, trừ đi kích thước của bộ không hợp lệ khỏi tổng số vị trí. 

Lý do điều này có tác dụng là vì mọi`2 × 2`hình vuông phải chứa ít nhất một viên đá và do đó phải được phát hiện khi xử lý viên đá đó. Ngược lại, mỗi hình vuông được thêm tối đa một lần vào bộ bất kể nó chứa bao nhiêu viên đá, bởi vì các bộ loại bỏ các bản sao. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m, k = map(int, input().split())
    
    total = (n - 1) * (m - 1)
    
    bad = set()
    
    for _ in range(k):
        x, y = map(int, input().split())
        
        # possible top-left corners of 2x2 blocks containing (x, y)
        candidates = [
            (x, y),
            (x - 1, y),
            (x, y - 1),
            (x - 1, y - 1)
        ]
        
        for i, j in candidates:
            if 1 <= i <= n - 1 and 1 <= j <= m - 1:
                bad.add((i, j))
    
    print(total - len(bad))

if __name__ == "__main__":
    solve()
```Giải pháp dựa vào việc tính toán toàn bộ trước, sau đó loại bỏ các cấu hình không hợp lệ. Bước tạo ứng cử viên là phần quan trọng: mỗi viên đá chỉ ảnh hưởng đến nhiều vị trí tiềm năng không đổi, do đó tổng công việc là tuyến tính theo`k`. Bộ này đảm bảo rằng ảnh hưởng chồng chéo từ nhiều viên đá không dẫn đến việc trừ quá mức. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 3 2
1 3
3 3
```Tổng số có thể`2 × 2`vị trí là`(2 × 2) = 4`. 

Chúng tôi xử lý đá`(1, 3)`Đầu tiên. 

| Đá | Ứng viên góc trên bên trái | Các góc hợp lệ được thêm vào tập hợp xấu | 
| --- | --- | --- | 
| (1,3) | (1,3), (0,3), (1,2), (0,2) | (1,2) | 
| (3,3) | (3,3), (2,3), (3,2), (2,2) | (2,2) | 

Hiện nay`bad = {(1,2), (2,2)}`. 

Câu trả lời trở thành`4 - 2 = 2`. 

Dấu vết này cho thấy rằng mỗi ô vuông không hợp lệ được chụp chính xác một lần mặc dù nhiều viên đá có thể đóng góp các ứng cử viên nói chung. 

### Ví dụ 2 

đầu vào:```
3 5 3
2 4
2 2
2 3
```Tổng số vị trí là`(2 × 4) = 8`. 

Tất cả các viên đá đều nằm ở hàng 2 theo cột liên tiếp, gây ô nhiễm nặng nề cho toàn bộ`2 × 2`các ô vuông cắt qua hàng đó. 

Sau khi xử lý tất cả các ứng cử viên, mọi góc trên bên trái có thể sẽ trở thành tập hợp không hợp lệ. 

Kết quả cuối cùng là`0`, phù hợp với thực tế là mọi khả năng`2 × 2`hình vuông bao gồm ít nhất một trong các ô bị chặn. 

Ví dụ này chứng minh rằng thuật toán xử lý một cách tự nhiên các cụm đá dày đặc mà không cần vỏ bọc đặc biệt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(k) | Mỗi viên đá tạo ra tối đa bốn ô vuông ứng cử viên, mỗi viên được chèn vào một bộ băm trong thời gian trung bình không đổi | 
| Không gian | O(k) | Bộ lưu trữ tối đa bốn mục trên mỗi viên đá, nhưng các mục trùng lặp được hợp nhất | 

Các ràng buộc cho phép lên tới 200.000 viên đá, do đó cả thời gian và mức sử dụng bộ nhớ vẫn ở mức thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, m, k = map(int, input().split())
    total = (n - 1) * (m - 1)
    bad = set()

    for _ in range(k):
        x, y = map(int, input().split())
        for i, j in [(x, y), (x-1, y), (x, y-1), (x-1, y-1)]:
            if 1 <= i <= n - 1 and 1 <= j <= m - 1:
                bad.add((i, j))

    return str(total - len(bad))

assert run("3 3 2\n1 3\n3 3\n") == "2", "sample 1"
assert run("3 5 3\n2 4\n2 2\n2 3\n") == "0", "sample 2"

assert run("2 2 0\n") == "1", "minimum grid no stones"
assert run("2 2 1\n1 1\n") == "0", "single square fully blocked"
assert run("1000000000 1000000000 0\n") == str((10**9-1)*(10**9-1)), "max grid empty"
assert run("3 3 4\n1 1\n1 2\n2 1\n2 2\n") == "0", "all cells of one square blocked"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 2 0 | 1 | lưới nhỏ nhất với một vị trí | 
| 2 2 1, (1,1) | 0 | đá duy nhất phá hủy hình vuông | 
| Lưới 10^9, 0 viên đá | (n-1)(m-1) | độ chính xác của lưới trống lớn | 
| chặn đầy đủ 2x2 | 0 | phủ sóng toàn bộ một hình vuông | 

## Vỏ cạnh 

Lưới không có đá được xử lý trực tiếp theo công thức`(n - 1) × (m - 1)`bởi vì tập hợp các ô vuông không hợp lệ vẫn trống. Thuật toán không tạo ra các phép chèn ứng cử viên, do đó phép trừ không làm gì cả. 

Một hòn đá ở ranh giới như`(1, m)`chỉ đóng góp vào các góc trên cùng bên trái hợp lệ nếu chúng vẫn nằm trong giới hạn. Thế hệ ứng cử viên tự nhiên lọc ra các tọa độ không hợp lệ như`(0, m)`hoặc`(1, m)`khi`m`vượt quá giới hạn cho góc trên cùng bên trái. 

Khi nhiều viên đá nằm trong cùng một`2 × 2`vùng, tất cả đều cố gắng chèn cùng một góc trên bên trái vào tập hợp. Bộ này đảm bảo rằng hình vuông này chỉ được tính một lần trong bộ không hợp lệ, ngăn chặn việc trừ quá mức. 

Bị chặn hoàn toàn`2 × 2`vùng được xử lý chính xác vì mọi viên đá bên trong nó đều tạo ra cùng một ứng cử viên ở trên cùng bên trái, đảm bảo hình vuông được đưa vào tập hợp không hợp lệ đúng một lần.
