---
title: "CF 2164A - Trò chơi tuần tự"
description: "Chúng ta được cung cấp một chuỗi số và chúng ta nén nó liên tục cho đến khi chỉ còn lại một giá trị. Mỗi thao tác lấy hai phần tử liền kề, loại bỏ chúng và thay thế chúng bằng bất kỳ số nguyên nào nằm giữa hai giá trị ban đầu, bao gồm cả hai."
date: "2026-06-07T23:36:29+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "sortings"]
categories: ["algorithms"]
codeforces_contest: 2164
codeforces_index: "A"
codeforces_contest_name: "Codeforces Global Round 30 (Div. 1 + Div. 2)"
rating: 800
weight: 2164
solve_time_s: 74
verified: true
draft: false
---

[CF 2164A - Trò chơi tuần tự](https://codeforces.com/problemset/problem/2164/A) 

**Đánh giá:** 800 
**Tags:** vũ lực, phân loại 
**Thời gian giải:** 1 phút 14s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi số và chúng ta nén nó liên tục cho đến khi chỉ còn lại một giá trị. Mỗi thao tác lấy hai phần tử liền kề, loại bỏ chúng và thay thế chúng bằng bất kỳ số nguyên nào nằm giữa hai giá trị ban đầu, bao gồm cả hai. Quyền tự do chính là chúng ta không bị buộc phải chọn quy tắc trung bình hoặc quy tắc cố định, nhưng chúng ta có thể chọn bất kỳ giá trị nào trong khoảng được hình thành bởi cặp. 

Sau chính xác$n-1$sự hợp nhất như vậy, mảng sẽ trở thành một số duy nhất. Câu hỏi đặt ra là liệu có tồn tại một số chuỗi kết hợp và lựa chọn các giá trị trung gian sao cho số còn lại cuối cùng chính xác là$x$. 

Các ràng buộc rất nhỏ: mỗi thử nghiệm có tối đa 100 phần tử và tối đa 500 trường hợp thử nghiệm. Điều này ngay lập tức gợi ý rằng ngay cả lý luận bậc hai hoặc bậc ba cho mỗi bài kiểm tra cũng có thể chấp nhận được, nhưng bất cứ điều gì theo cấp số nhân trên tất cả các chuỗi hợp nhất thì không. Một mô phỏng đơn giản thử tất cả các thứ tự hợp nhất có thể và tất cả các giá trị có thể là không thể bởi vì mỗi lần hợp nhất đưa ra một phạm vi khả năng liên tục và số lượng cây hợp nhất đã theo cấp số nhân trong$n$. 

Trường hợp cạnh tinh tế xuất hiện khi tất cả các giá trị bằng nhau hoặc khi$x$nằm ngoài phạm vi của mảng. Ví dụ, nếu tất cả các phần tử đều$[3,3,3]$, giá trị cuối cùng phải luôn là 3, vì vậy bất kỳ$x \neq 3$là không thể. Tương tự, nếu chúng ta bắt đầu với$[1, 5]$, chúng ta luôn có thể chọn bất kỳ giá trị cuối cùng nào trong$[1,5]$, vậy bất kỳ$x$bên ngoài khoảng thời gian này là không thể ngay lập tức. Phần thú vị là liệu mọi giá trị trong phạm vi toàn cầu có luôn đạt được hay không. 

Một sai lầm ngây thơ là cho rằng vì chúng ta có thể chọn bất kỳ giá trị nào giữa hai số nên chúng ta có thể “điều khiển” kết quả cuối cùng một cách tùy ý. Trực giác đó sẽ thất bại nếu chúng ta nghĩ về sự lan truyền những ràng buộc hơn là sự tự do cục bộ. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ sẽ cố gắng mô phỏng tất cả các chuỗi hợp nhất có thể có. Ở mỗi bước, chúng tôi chọn một cặp liền kề và thay thế nó bằng mọi số nguyên trong phạm vi của nó. Điều này tạo ra một yếu tố phân nhánh của cả vị trí và giá trị. Ngay cả khi bỏ qua các giá trị, vẫn có các cấu trúc hợp nhất số Catalan và với các lựa chọn giá trị, không gian trạng thái trở nên vô hạn. Ngay cả việc rời rạc hóa các giá trị cũng không giúp ích gì, bởi vì sự kết hợp vượt quá tính khả thi. 

Cái nhìn sâu sắc quan trọng là chúng tôi không thực sự quan tâm đến thứ tự hợp nhất. Mọi thao tác chỉ hợp nhất hai phân đoạn liền kề thành một phân đoạn có các giá trị có thể đạt được tạo thành một khoảng. Điều này cho thấy rằng mỗi phân đoạn$[l, r]$có thể được biểu diễn bằng một phạm vi liên tục$[min(l,r), max(l,r)]$và việc hợp nhất hai phân đoạn chỉ đơn giản là hợp nhất các phạm vi có thể có của chúng thành khoảng đầy đủ giữa tất cả các phần tử trong phân đoạn đó. 

Vì vậy, vấn đề trở thành một bất biến đơn giản hơn nhiều: sau bất kỳ chuỗi phép tính nào, giá trị cuối cùng có thể là bất kỳ số nguyên nào giữa mức tối thiểu toàn cục và mức tối đa toàn cục của mảng ban đầu. Điều này là do chúng ta luôn có thể truyền bá các cực trị vào bên trong bằng cách sử dụng các phép hợp nhất và ở mỗi bước, chúng ta có thể tự do chọn các giá trị trung gian để tránh đưa ra các ràng buộc mới. 

Vì vậy, câu trả lời cuối cùng chỉ đơn giản là liệu$x$nằm bên trong$[\min(a), \max(a)]$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(n) mỗi lần kiểm tra | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giảm từng trường hợp thử nghiệm thành một truy vấn phạm vi trên mảng. 

1. Tính giá trị nhỏ nhất của dãy. Giá trị này thể hiện giá trị thấp nhất có thể được bảo toàn thông qua việc hợp nhất, vì không có thao tác nào có thể tạo ra thứ gì đó bên ngoài bao lồi của đầu vào của nó. 
2. Tính giá trị lớn nhất của dãy. Đây là giới hạn trên đối xứng vì lý do tương tự. 
3. Kiểm tra xem$x$nằm giữa hai giá trị này, bao gồm. 
4. Nếu có thì xuất ra "YES", nếu không thì xuất ra "NO". 

Phần không rõ ràng duy nhất là tại sao toàn bộ khoảng thời gian có thể đạt được. Quy tắc hợp nhất cho phép chúng tôi chọn bất kỳ giá trị nào giữa hai phân đoạn liền kề. Bằng cách liên tục hợp nhất và chọn các giá trị dần dần tiến tới$x$, chúng ta luôn có thể xây dựng một đường dẫn không “bỏ qua” bất kỳ số nguyên nào trong phạm vi. 

### Tại sao nó hoạt động 

Mỗi lần hợp nhất sẽ thay thế hai phân đoạn liền kề bằng một phân đoạn duy nhất có các giá trị có thể đạt được tạo thành một khoảng kết nối bao gồm cả hai đầu vào. Vì các khoảng được đóng trong thao tác hợp nhất này nên đoạn cuối cùng cũng phải là một khoảng. Các đoạn ban đầu là các điểm đơn lẻ, do đó khoảng cuối cùng chính xác là bao lồi của tất cả các giá trị ban đầu, tức là$[\min(a), \max(a)]$. Vì vậy, bất kỳ$x$bên trong khoảng thời gian này có thể truy cập được. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        x = int(input())

        mn = min(a)
        mx = max(a)

        if mn <= x <= mx:
            print("YES")
        else:
            print("NO")

if __name__ == "__main__":
    solve()
```Mã trực tiếp tính toán tối thiểu và tối đa của mỗi mảng và kiểm tra xem$x$nằm trong khoảng đó. Việc thực hiện được cố ý tối thiểu vì cấu trúc của vấn đề loại bỏ mọi nhu cầu mô phỏng. 

Một sai lầm phổ biến ở đây là cố gắng hợp nhất hoặc mô phỏng các hoạt động một cách tham lam, nhưng điều đó là không cần thiết. Một cạm bẫy tiềm tàng khác là quên rằng các giá trị trung gian có thể là bất kỳ số nguyên nào trong phạm vi, điều này làm cho tập hợp có thể truy cập liên tục thay vì rời rạc. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:$$[2, 7, 5],\ x = 4$$Chúng tôi tính toán phạm vi: 

| Bước | Mảng | phút | tối đa | x trong phạm vi? | 
| --- | --- | --- | --- | --- | 
| ban đầu | [2,7,5] | 2 | 7 | vâng | 

Vì 4 nằm giữa 2 và 7 nên câu trả lời là CÓ. 

Điều này chứng tỏ rằng mặc dù 4 không có trong mảng, nhưng các phép hợp nhất trung gian cho phép xây dựng nó. 

### Ví dụ 2 

đầu vào:$$[-1, 3, 7, -9, -2],\ x = 8$$| Bước | Mảng | phút | tối đa | x trong phạm vi? | 
| --- | --- | --- | --- | --- | 
| ban đầu | [-1,3,7,-9,-2] | -9 | 7 | không | 

Ở đây, giá trị cuối cùng tối đa có thể không được vượt quá 7, vì vậy không thể truy cập được 8 bất kể lựa chọn hợp nhất. Điều này xác nhận rằng ranh giới phạm vi là chặt chẽ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) mỗi lần kiểm tra | Chúng tôi tính toán tối thiểu và tối đa trên mảng một lần | 
| Không gian | O(1) | Chỉ có một vài biến được lưu trữ | 

Các ràng buộc cho phép tối đa 500 trường hợp thử nghiệm với các mảng có kích thước 100, do đó quá trình quét tuyến tính này dễ dàng đủ nhanh. Giải pháp chạy tính bằng micro giây cho mỗi lần kiểm tra trong thực tế. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    out = []

    input = sys.stdin.readline
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        x = int(input())
        mn, mx = min(a), max(a)
        out.append("YES" if mn <= x <= mx else "NO")

    return "\n".join(out)

# provided samples
assert run("""3
3
2 7 5
4
5
-1 3 7 -9 -2
8
6
1 -1 -4 5 1 -4
-2
""") == """YES
NO
YES"""

# custom cases
assert run("""1
1
5
5
""") == "YES"

assert run("""1
1
5
6
""") == "NO"

assert run("""1
4
1 2 3 4
10
""") == "NO"

assert run("""1
4
1 2 3 4
1
""") == "YES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử bằng nhau duy nhất | CÓ | trường hợp cơ sở đúng đắn | 
| không khớp đơn | KHÔNG | ranh giới một phần tử | 
| ngoài phạm vi lớn x | KHÔNG | xử lý giới hạn trên | 
| x bằng phút | CÓ | tính đúng đắn của ranh giới bao gồm | 

## Vỏ cạnh 

Trường hợp một cạnh là khi mảng có một phần tử duy nhất. Đối với đầu vào$[5]$, giá trị cuối cùng duy nhất có thể là 5 vì không tồn tại phép toán nào. Thuật toán tính toán$mn = mx = 5$, vậy chỉ$x = 5$trả về CÓ, khớp chính xác với quy trình. 

Một trường hợp cạnh khác là khi tất cả các giá trị giống hệt nhau, chẳng hạn như$[3,3,3,3]$. Mỗi lần hợp nhất đều giữ nguyên giá trị 3 bất kể lựa chọn nào, do đó kết quả cuối cùng là cố định. Phạm vi tính toán là$[3,3]$, điều này hạn chế chính xác câu trả lời. 

Một trường hợp tế nhị hơn là khi$x$nằm hoàn toàn giữa hai giá trị xa nhau, chẳng hạn như$[1, 100]$với$x = 50$. Mặc dù ban đầu không có giá trị trung gian nào tồn tại, việc hợp nhất trực tiếp hai phần tử cho phép chọn bất kỳ số nguyên nào trong khoảng đó, tạo ra 50 có thể đạt được. Việc kiểm tra phạm vi sẽ nắm bắt được điều này ngay lập tức mà không cần mô phỏng các trạng thái trung gian.
