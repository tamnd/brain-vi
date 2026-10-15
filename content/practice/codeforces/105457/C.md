---
title: "CF 105457C - Lịch trình"
description: "Chúng ta được cung cấp một danh sách các lịch học, mỗi lịch được biểu thị bằng thời gian bắt đầu và thời gian kết thúc trong một ngày tròn có m giờ riêng biệt."
date: "2026-06-23T17:46:10+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105457
codeforces_index: "C"
codeforces_contest_name: "XXIII Spain Olympiad in Informatics, Online Qualifier 1"
rating: 0
weight: 105457
solve_time_s: 71
verified: true
draft: false
---

[CF 105457C - Lịch trình](https://codeforces.com/problemset/problem/105457/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 11 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một danh sách các lịch học, mỗi lịch được biểu thị bằng thời gian bắt đầu và thời gian kết thúc vào một ngày tròn duy nhất có`m`giờ rời rạc. Mỗi lớp chiếm một khoảng thời gian nửa mở từ giờ bắt đầu đến giờ kết thúc và nhiệm vụ là chọn càng nhiều lớp càng tốt sao cho không có hai lớp được chọn trùng nhau về mặt thời gian. 

Thời gian là tuyến tính từ`0`ĐẾN`m`, vì vậy mặc dù cách giải thích là “giờ trong một ngày” nhưng không có sự bao hàm. Hạn chế duy nhất là các khoảng được chọn phải rời rạc theo từng cặp và được phép chạm vào điểm cuối vì lớp học kết thúc vào thời điểm đó.`t`không trùng lặp với một bắt đầu từ`t`. 

Đầu vào có thể chứa nhiều trường hợp kiểm thử độc lập, mỗi trường hợp có tối đa`5 · 10^5`khoảng thời gian trong phiên bản khó nhất. Thang đo đó ngay lập tức loại trừ mọi giải pháp so sánh các cặp khoảng hoặc khám phá các tập hợp con. Một bậc hai hoặc thậm chí`O(n log n)`giải pháp cho mỗi trường hợp thử nghiệm vẫn ổn, nhưng bất cứ điều gì tệ hơn`O(n log n)`tổng thể sẽ thất bại do thời gian chạy chiếm ưu thế sắp xếp lặp đi lặp lại. 

Trường hợp cạnh tinh tế xuất hiện khi nhiều khoảng thời gian có cùng thời gian bắt đầu hoặc kết thúc. Ví dụ, hãy xem xét`m = 2`với khoảng thời gian`[0, 2]`,`[0, 1]`,`[1, 2]`. Câu trả lời tối ưu là`2`, nhưng một chiến lược ngây thơ luôn chọn khoảng thời gian bắt đầu sớm nhất có thể chọn`[0, 2]`và chặn mọi thứ khác. Một cạm bẫy khác phát sinh khi các khoảng được lồng vào nhau:`[0, 10]`,`[1, 2]`,`[2, 3]`, trong đó câu trả lời đúng là 2 chứ không phải 1. 

Những trường hợp này cho thấy các quyết định tham lam của địa phương chỉ dựa vào thời gian bắt đầu là không đủ. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử tất cả các tập hợp con của các khoảng và kiểm tra xem tập hợp con được chọn có chồng chéo hay không, theo dõi kích thước tối đa. Kiểm tra tính hợp lệ của một tập hợp con chi phí`O(n)`nếu được thực hiện một cách ngây thơ bằng cách sắp xếp hoặc quét, và có`2^n`tập hợp con, làm cho điều này không khả thi ngay cả đối với`n = 30`. Một ý tưởng khác tốt hơn một chút là quay lui bằng việc cắt tỉa, nhưng hành vi trong trường hợp xấu nhất vẫn suy biến thành thời gian theo cấp số nhân vì các khoảng thời gian có thể được sắp xếp để tránh việc cắt tỉa sớm. 

Quan sát quan trọng là bài toán có cấu trúc con tối ưu và thuộc tính lựa chọn tham lam nếu chúng ta sắp xếp các khoảng thời gian theo thời gian hoàn thành. Khi chúng tôi cam kết sử dụng khoảng thời gian kết thúc sớm nhất trong số các lựa chọn có sẵn, chúng tôi sẽ tối đa hóa không gian trống còn lại cho các khoảng thời gian trong tương lai. Bất kỳ lựa chọn thay thế nào kết thúc sau đó chỉ có thể hạn chế hoặc trì hoãn các lựa chọn trong tương lai mà không làm tăng số khoảng thời gian mà chúng ta có thể điều chỉnh. 

Điều này biến vấn đề thành một vấn đề tối đa hóa lịch trình khoảng thời gian cổ điển. Việc sắp xếp theo thời gian kết thúc đảm bảo rằng mỗi bước đều đưa ra lựa chọn tối ưu cục bộ mang lại sự linh hoạt tối đa. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n · n) | O(n) | Quá chậm | 
| Tối Ưu (Tham Lam) | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập. 

1. Đọc tất cả các khoảng thời gian và lưu trữ chúng theo cặp`(a, b)`. Chúng đại diện cho các khối thời gian cố định phải được chọn mà không bị trùng lặp. 
2. Sắp xếp tất cả các khoảng thời gian theo thời gian kết thúc`b`. Lý do cho việc đặt hàng này là việc chọn khoảng thời gian hoàn thiện sớm hơn sẽ duy trì được tính khả dụng cao nhất trong tương lai. 
3. Khởi tạo một biến`current_time = 0`, theo dõi thời gian sớm nhất mà chúng ta có thể rảnh để lên lịch cho khoảng thời gian tiếp theo. 
4. Lặp lại các khoảng thời gian theo thứ tự được sắp xếp. 
5. Đối với mỗi khoảng thời gian`(a, b)`, nếu như`a >= current_time`, chọn nó và đặt`current_time = b`. Điều này đảm bảo chúng tôi không bao giờ chồng chéo và luôn cam kết khoảng thời gian tương thích hoàn thiện sớm nhất. 
6. Đếm xem chúng tôi đã chọn bao nhiêu khoảng thời gian và xuất giá trị này. 

### Tại sao nó hoạt động 

Ở mỗi bước, chúng tôi duy trì bất biến rằng`current_time`là phần cuối của khoảng được chọn cuối cùng và tất cả các khoảng được chọn đều rời rạc và kết thúc càng sớm càng tốt theo thứ tự sắp xếp. Khi chúng tôi xem xét khoảng thời gian kết thúc sớm nhất trong số tất cả các ứng cử viên còn lại, bất kỳ khoảng thời gian thay thế nào trùng lặp với khoảng thời gian đó nhưng kết thúc sau chỉ có thể làm giảm không gian lập kế hoạch trong tương lai mà không làm tăng số lượng khoảng thời gian tương thích. Đối số trao đổi này đảm bảo rằng việc thay thế lựa chọn khác biệt đầu tiên của bất kỳ giải pháp tối ưu nào bằng lựa chọn tham lam sẽ không làm giảm tổng số khoảng, do đó việc áp dụng lặp lại sẽ mang lại một giải pháp tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    it = iter(sys.stdin.read().strip().split())
    out = []
    
    while True:
        try:
            n = int(next(it))
        except StopIteration:
            break
        m = int(next(it))  # unused, but part of format
        
        intervals = []
        for _ in range(n):
            a = int(next(it))
            b = int(next(it))
            intervals.append((b, a))  # sort by end time
        
        intervals.sort()
        
        current = 0
        ans = 0
        
        for b, a in intervals:
            if a >= current:
                ans += 1
                current = b
        
        out.append(str(ans))
    
    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Việc triển khai đọc tất cả đầu vào cùng một lúc để tránh phí tổn từ các lệnh gọi I/O lặp lại, điều quan trọng là tổng kích thước có thể đạt tới vài trăm nghìn khoảng thời gian. 

Khoảng thời gian được lưu trữ dưới dạng`(end, start)`đặc biệt là cho phép sắp xếp bộ dữ liệu mặc định của Python sắp xếp chúng theo thời gian kết thúc trước mà không cần thêm logic so sánh. Điều này vừa đơn giản vừa nhanh hơn các phím sắp xếp tùy chỉnh trong các vòng lặp chặt chẽ. 

Cuộc càn quét tham lam vẫn duy trì`current`, đại diện cho sự kết thúc của khoảng thời gian được chọn cuối cùng. điều kiện`a >= current`thực thi không chồng chéo trong khi vẫn cho phép lập lịch quay lại, phù hợp với định nghĩa của vấn đề. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 2
0 1
0 2
1 2
```Sắp xếp theo thời gian kết thúc:`(0,1)`,`(1,2)`,`(2,0)`| Bước | Khoảng (a,b) | hiện tại | Hành động | trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | (0,1) | 0 | lấy | 1 | 
| 2 | (1,2) | 1 | lấy | 2 | 
| 3 | (0,2) | 2 | bỏ qua | 2 | 

Dấu vết cho thấy rằng việc chọn khoảng thời gian hoàn thiện sớm sẽ duy trì khả năng thực hiện những khoảng thời gian tương thích sau này. 

### Ví dụ 2 

đầu vào:```
4 8
0 1
1 2
3 7
4 7
```Đã sắp xếp:`(0,1)`,`(1,2)`,`(3,7)`,`(4,7)`| Bước | Khoảng (a,b) | hiện tại | Hành động | trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | (0,1) | 0 | lấy | 1 | 
| 2 | (1,2) | 1 | lấy | 2 | 
| 3 | (3,7) | 2 | lấy | 3 | 
| 4 | (4,7) | 7 | bỏ qua | 3 | 

Điều này chứng tỏ rằng ngay cả khi các khoảng thời gian trùng nhau một phần, việc sắp xếp theo thời gian kết thúc đảm bảo chúng tôi luôn giữ lịch trình linh hoạt nhất có thể. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) cho mỗi trường hợp thử nghiệm | Sắp xếp chiếm ưu thế, quét tham lam là tuyến tính | 
| Không gian | O(n) | Khoảng thời gian lưu trữ | 

Các ràng buộc cho phép lên đến`5 · 10^5`khoảng thời gian, vì vậy một toàn cầu duy nhất`O(n log n)`vượt qua là hiệu quả. Dấu chân bộ nhớ là tuyến tính và nằm trong giới hạn vì chúng tôi chỉ lưu trữ các cặp số nguyên. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    from collections import deque
    
    input = sys.stdin.readline
    
    data = inp.strip().split()
    it = iter(data)
    out = []
    
    while True:
        try:
            n = int(next(it))
        except StopIteration:
            break
        m = int(next(it))
        intervals = []
        for _ in range(n):
            a = int(next(it))
            b = int(next(it))
            intervals.append((b, a))
        intervals.sort()
        
        cur = 0
        ans = 0
        for b, a in intervals:
            if a >= cur:
                ans += 1
                cur = b
        
        out.append(str(ans))
    
    return "\n".join(out)

# provided samples
assert run("""3 2
0 1
0 2
1 2
2 2
0 1
0 2
4 8
0 1
1 2
3 7
4 7
""") == "2\n1\n3"

# minimum size
assert run("1 10\n0 5\n") == "1"

# all overlapping
assert run("3 10\n0 10\n1 9\n2 8\n") == "1"

# disjoint chain
assert run("3 10\n0 2\n2 4\n4 6\n") == "3"

# identical end times
assert run("3 10\n0 5\n1 5\n2 5\n") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| khoảng đơn | 1 | trường hợp tối thiểu | 
| khoảng lồng nhau | 1 | sự thống trị của khoảng cách lớn | 
| khoảng thời gian chuỗi | 3 | xử lý bình đẳng ranh giới | 
| đầu giống hệt nhau | 1 | bẻ dây đúng cách | 

## Vỏ cạnh 

Trường hợp góc đầu tiên là khi các khoảng được lồng hoàn toàn. Xem xét đầu vào:```
3 10
0 10
1 9
2 8
```Thuật toán sắp xếp theo thời gian kết thúc và chọn`(2,8)`Đầu tiên? Không, nó thực sự chọn`(8,2)`là kết thúc sớm nhất, sau đó từ chối những kết thúc khác vì tất cả đều bắt đầu trước khi khoảng thời gian đã chọn kết thúc. Câu trả lời cuối cùng là`1`, phù hợp với hành vi tối ưu vì chỉ có thể chọn một khoảng mà không bị trùng lặp trong cấu hình này. 

Một trường hợp cạnh khác là một chuỗi chặt chẽ các khoảng thời gian liên tiếp:```
3 10
0 2
2 4
4 6
```Ở đây mỗi khoảng thời gian bắt đầu chính xác khi khoảng thời gian trước đó kết thúc. Tình trạng tham lam`a >= current`cho phép tất cả chúng được chọn. Thuật toán xử lý chúng theo thứ tự thời gian kết thúc tăng dần, cập nhật`current`từng bước và kết quả đầu ra`3`, đó là tối ưu. 

Trường hợp thứ ba liên quan đến thời gian kết thúc giống hệt nhau:```
3 10
0 5
1 5
2 5
```Sau khi sắp xếp, tất cả các khoảng đều có chung một đầu. Quá trình tham lam chọn cái đầu tiên nó gặp có khởi đầu hợp lệ, sau đó từ chối phần còn lại vì`current`trở thành`5`. Kết quả là`1`, điều này đúng vì tất cả các khoảng trùng nhau trong toàn bộ khoảng thời gian của chúng cho đến thời điểm 5.
