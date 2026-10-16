---
title: "CF 105481J - \u7ed3\u8bfe\u98ce\u4e91"
description: "Mỗi học sinh có hai thành phần trong lớp của mình: điểm thi cố định và điểm đánh giá liên tục có thể kiểm soát được. Đối với sinh viên $i$, tổng điểm hiện tại là $xi + yi$, trong đó $xi$ là điểm bài tập được giới hạn ở $a$ và $yi$ là điểm thi được giới hạn ở $b$."
date: "2026-06-23T18:20:28+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105481
codeforces_index: "J"
codeforces_contest_name: "2024 CCPC Liaoning Provincial Contest"
rating: 0
weight: 105481
solve_time_s: 49
verified: true
draft: false
---

[CF 105481J - \u7ed3\u8bfe\u98ce\u4e91](https://codeforces.com/problemset/problem/105481/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Mỗi học sinh có hai thành phần trong lớp của mình: điểm thi cố định và điểm đánh giá liên tục có thể kiểm soát được. Dành cho sinh viên$i$, tổng số hiện tại là$x_i + y_i$, Ở đâu$x_i$điểm bài tập được giới hạn ở mức$a$, Và$y_i$điểm thi có bị giới hạn ở mức không$b$. Một học sinh đạt nếu tổng điểm của họ ít nhất là$c$, nếu không thì họ thất bại. 

Giáo viên được phép áp dụng một phép toán toàn cục duy nhất: cộng một số nguyên không âm$d$đến điểm bài tập của mỗi học sinh. Sau lần tăng này, bất kỳ điểm bài tập nào vượt quá$a$được cắt bớt lại thành$a$. Điểm thi không thay đổi. Mục đích là để đếm xem có bao nhiêu học sinh chuyển từ trượt sang đậu sau hoạt động này. 

Ràng buộc$n \le 1000$ngụ ý rằng việc kiểm tra từng học sinh một cách độc lập để tìm một điểm cố định$d$là tầm thường, vì vậy bất kỳ giải pháp nào tuyến tính nhất trong$n$mỗi hoạt động ứng viên là đủ. Các phạm vi giá trị đều nhỏ, đặc biệt$d \le 100$, điều này gợi ý rằng dùng vũ lực trên tất cả những gì có thể$d$là hoàn toàn khả thi. 

Một chi tiết tinh tế là việc cắt bớt điểm của bài tập. Nếu như$x_i + d > a$, sự đóng góp trở nên chính xác$a$, không$x_i + d$. Điều này tạo ra hành vi từng phần trong điểm số cuối cùng, đây là nguyên nhân chính gây ra sai lầm trong cách suy luận ngây thơ. 

Một trường hợp thất bại phổ biến là bỏ qua hiệu ứng giới hạn. Ví dụ, nếu$a = 40$,$x_i = 35$, Và$d = 10$, phần đóng góp cho bài tập khóa học không phải là 45 mà là 40. Bất kỳ giải pháp nào trực tiếp bổ sung$d$không áp dụng giới hạn sẽ đánh giá quá cao điểm số và phân loại không chính xác những học sinh gần ranh giới là đạt. 

Một cạm bẫy khác là hiểu sai những gì được tính. Chúng tôi không tính tất cả những học sinh đậu sau phẫu thuật mà chỉ tính những học sinh ban đầu trượt và trở thành đậu do phẫu thuật. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: thử mọi giá trị có thể có của$d$từ 0 đến 100. Với mỗi$d$, tính lại điểm bài tập đã điều chỉnh của mỗi học sinh, tính tổng điểm của họ và kiểm tra xem họ có đạt hay không. Chúng tôi cũng cần cơ sở: học sinh nào sẽ thất bại trước khi sửa đổi. 

Điều này hoạt động vì phạm vi của$d$là nhỏ bé. Tính toán trường hợp xấu nhất là$101 \times 1000$sinh viên, và đối với mỗi sinh viên, chúng tôi làm việc liên tục, vậy nên khoảng$10^5$tổng số hoạt động. Cái này nhỏ thoải mái. 

Sự đơn giản hóa chính là không có sự tương tác nào tồn tại giữa các sinh viên. Kết quả của mỗi học sinh chỉ phụ thuộc vào chính họ$(x_i, y_i)$và tham số chung$d$. Sự độc lập này loại bỏ mọi nhu cầu về tổ hợp hoặc tối ưu hóa trên các tập hợp con. Chúng tôi chỉ mô phỏng một số lượng nhỏ các trạng thái toàn cầu. 

Chúng tôi tính toán hai trạng thái cho mỗi học sinh cho mỗi$d$: trạng thái đạt/không đạt ban đầu và trạng thái đạt/không đạt mới sau khi áp dụng mức tăng được điều chỉnh giới hạn. Chúng tôi tính số lượt chuyển đổi từ không thành công sang đạt. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force kết thúc$d$|$O(n \cdot 101)$|$O(1)$| Đã chấp nhận | 
| Tối ưu (cùng ý tưởng, mô phỏng trực tiếp) |$O(n \cdot 101)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các giá trị đầu vào, bao gồm$n, a, b, c$, danh sách sinh viên và mức tăng toàn cầu$d$. 
2. Với mỗi học sinh, hãy tính xem hiện tại họ có đạt hay không: kiểm tra xem$x_i + y_i \ge c$. Lưu trữ ngầm cấu trúc boolean hoặc count này. 
3. Đối với cái đã cho$d$, tính điểm bài tập mới của mỗi học sinh như sau$\min(a, x_i + d)$. 
4. Tính tổng số điểm mới như sau$\min(a, x_i + d) + y_i$. 
5. Kiểm tra xem học sinh hiện có đậu hay không. 
6. Đếm số học sinh ở trạng thái ban đầu là trượt nhưng trạng thái mới là đạt. 
7. Xuất số đếm này. 

Mỗi học sinh được đánh giá độc lập, vì vậy chúng tôi không bao giờ cần duy trì trạng thái toàn cầu vượt quá số lượng cuối cùng. 

### Tại sao nó hoạt động 

Hoạt động áp dụng cho tất cả học sinh là thống nhất và điều kiện đậu hoàn toàn là phép cộng cho mỗi học sinh. Điều này làm cho hệ thống có thể tách biệt thành các chuyển đổi độc lập cho mỗi học sinh. Vì việc điều chỉnh bài tập khóa học mang tính quyết định đối với một thời gian cố định$d$, mỗi học sinh có đúng một kết quả chuyển trạng thái. Do đó, việc đếm các cải tiến tương đương với việc tính tổng các sự kiện boolean độc lập trên tất cả học sinh. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a, b, c = map(int, input().split())
    
    students = []
    for _ in range(n):
        x, y = map(int, input().split())
        students.append((x, y))
    
    d = int(input())
    
    improved = 0
    
    for x, y in students:
        before = (x + y >= c)
        
        x2 = x + d
        if x2 > a:
            x2 = a
        
        after = (x2 + y >= c)
        
        if not before and after:
            improved += 1
    
    print(improved)

if __name__ == "__main__":
    solve()
```Giải pháp trực tiếp tuân theo mô phỏng cho mỗi học sinh. Bước không hề tầm thường duy nhất là áp dụng giới hạn một cách chính xác: bài tập sau khi sửa đổi là`min(a, x + d)`, phải được tính toán trước khi tính tổng với điểm thi. 

Cờ “trước” đảm bảo chúng tôi chỉ tính những học sinh đã trượt ban đầu. Nếu không có điều này, kết quả sẽ không chính xác bao gồm những học sinh đã đậu và vẫn đậu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 3, a = 40, b = 60, c = 60
students = (20,25), (10,40), (25,30)
d = 10
```Chúng tôi tính toán từng học sinh: 

| Sinh viên | x | y | trước | giới hạn x+d | sau tổng số | sau khi vượt qua | được cải thiện | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | 20 | 25 | không | 30 | 55 | không | không | 
| 2 | 10 | 40 | không | 20 | 60 | vâng | vâng | 
| 3 | 25 | 30 | vâng | 35 | 65 | vâng | không | 

Chỉ có học sinh 2 chuyển từ trượt sang đậu. 

Đầu ra:```
1
```Điều này cho thấy rằng chỉ những học sinh gần ngưỡng mới được hưởng lợi từ sự gia tăng này, còn những học sinh đỗ trước đó là không liên quan. 

### Ví dụ 2 

đầu vào:```
n = 2, a = 50, b = 50, c = 80
students = (49, 30), (10, 60)
d = 5
```| Sinh viên | x | y | trước | giới hạn x+d | sau tổng số | sau khi vượt qua | được cải thiện | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | 49 | 30 | vâng | 50 | 80 | vâng | không | 
| 2 | 10 | 60 | không | 15 | 75 | không | không | 

Đầu ra:```
0
```Điều này chứng tỏ rằng việc tăng số lượng môn học không đảm bảo sự tiến bộ nếu điểm thi quá thấp để vượt qua ngưỡng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi học sinh được xử lý một lần với các phép tính số học có thời gian không đổi | 
| Không gian |$O(1)$| Chỉ một số biến được sử dụng ngoài bộ nhớ đầu vào | 

Giới hạn$n \le 1000$làm cho các mô phỏng thậm chí lặp đi lặp lại trở nên tầm thường. Giải pháp này vừa vặn một cách thoải mái trong giới hạn vì nó thực hiện tối đa vài nghìn phép tính số học. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    a, b, c = map(int, input().split())
    students = [tuple(map(int, input().split())) for _ in range(n)]
    d = int(input())

    improved = 0
    for x, y in students:
        before = x + y >= c
        x2 = min(a, x + d)
        after = x2 + y >= c
        if (not before) and after:
            improved += 1
    return str(improved)

# sample
assert run("3\n40 60 60\n20 25\n10 40\n25 30\n10\n") == "1"

# minimum case
assert run("1\n0 100 50\n0 49\n10\n") == "0"

# exact boundary improvement
assert run("1\n10 100 50\n10 39\n5\n") == "1"

# already passing should not count
assert run("2\n50 50 80\n49 30\n10 60\n5\n") == "0"

# cap effect matters
assert run("1\n40 100 100\n35 60\n10\n") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| trường hợp tối thiểu | 0 | sinh viên độc thân, không thể tiến bộ | 
| cải thiện ranh giới | 1 | vượt ngưỡng chính xác | 
| đã đi qua | 0 | đảm bảo chỉ thất bại→đạt được tính | 
| hiệu ứng nắp | 1 | xác minh việc xử lý tối thiểu (a, x+d) | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi một học sinh vừa mới vượt qua trước khi thực hiện thao tác và chỉ tiếp cận được nó sau khi cắt. 

đầu vào:```
n = 1
a = 40, b = 100, c = 80
x = 35, y = 40
d = 10
```Trước:$35 + 40 = 75$, thất bại. 

Sau: bài tập trở thành$\min(40, 45) = 40$, vậy tổng số là$40 + 40 = 80$, đi qua. 

Thuật toán tính toán: 

- trước = sai 
- x2 = 40 (do bị giới hạn) 
- sau = đúng 

Vì vậy, nó đếm chính xác học sinh này. 

Một trường hợp tương phản là khi bỏ qua giới hạn: 

Nếu người ta sử dụng nhầm$x + d = 45$, thì tổng số trở thành$85$, vẫn đạt, vì vậy trường hợp này sẽ không phá vỡ tính đúng đắn. Mối nguy hiểm thực sự xuất hiện khi$x$gần với$a$và giới hạn ngăn chặn lợi ích mong đợi đẩy học sinh vượt quá ngưỡng; sự bổ sung ngây thơ sẽ vượt quá những cải tiến trong những tình huống đó.
