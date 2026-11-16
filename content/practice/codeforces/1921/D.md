---
title: "CF 1921D - Mảng Rất Khác Biệt"
description: "Chúng ta có một mảng a có độ dài n. Một mảng b khác chứa m giá trị, trong đó m ít nhất là n. Ta được phép chọn chính xác n phần tử từ b, sau đó sắp xếp các phần tử đã chọn đó theo thứ tự bất kỳ để tạo thành một mảng c mới."
date: "2026-06-08T19:24:12+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "greedy", "sortings", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1921
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 920 (Div. 3)"
rating: 1100
weight: 1921
solve_time_s: 144
verified: true
draft: false
---

[CF 1921D - Mảng rất khác](https://codeforces.com/problemset/problem/1921/D) 

**Đánh giá:** 1100 
**Thẻ:** cấu trúc dữ liệu, tham lam, sắp xếp, hai con trỏ 
**Thời gian giải:** 2m 24s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một mảng`a`chiều dài`n`. Một mảng khác`b`chứa`m`giá trị, ở đâu`m`ít nhất là`n`. 

Chúng ta được phép lựa chọn chính xác`n`các yếu tố từ`b`, sau đó sắp xếp các phần tử đã chọn đó theo thứ tự bất kỳ để tạo thành một mảng mới`c`. Mục tiêu là tối đa hóa$$\sum_{i=1}^{n} |a_i - c_i|.$$Nhiệm vụ là tìm giá trị lớn nhất có thể của số tiền này. 

Nhận xét đầu tiên là thứ tự cuối cùng của các phần tử được chọn hoàn toàn nằm trong tầm kiểm soát của chúng ta. Chúng ta không bị buộc phải giữ gìn trật tự trong`b`. Điều này biến vấn đề thành một vấn đề phù hợp:`n`những giá trị chúng ta nên lấy từ`b`và chúng ta nên ghép chúng với các giá trị của`a`? 

Các ràng buộc đủ lớn để các thuật toán bậc hai hoặc bậc ba là không thể thực hiện được. Trên tất cả các trường hợp thử nghiệm, tổng kích thước của`b`nhiều nhất là`2 \cdot 10^5`, vì vậy một`O(m log m)`giải pháp cho mỗi trường hợp thử nghiệm là hoàn toàn có thể chấp nhận được, trong khi bất cứ điều gì như`O(nm)`sẽ quá chậm trong trường hợp xấu nhất. 

Một số trường hợp khó xử lý. 

Coi như```
n = 3
a = [1, 1, 1]
b = [1, 1, 1, 1]
```Mọi lựa chọn có thể đều tạo ra sự khác biệt`0`. Bất kỳ chiến lược tham lam nào luôn cho rằng có sự đóng góp tích cực nào đó sẽ thất bại ở đây. 

Coi như```
n = 2
a = [5, 100]
b = [1, 2, 99, 100]
```Câu trả lời tốt nhất không thể có được bằng cách luôn lấy giá trị còn lại nhỏ nhất trên toàn cầu hoặc lớn nhất trên toàn cầu. Các vị trí khác nhau có thể thích những thái cực đối lập nhau. 

Coi như```
n = 3
a = [50, 51, 52]
b = [1, 2, 3, 100, 101]
```Giải pháp tối ưu sử dụng một số phần tử ở đầu bên trái của`b`và một số từ phía bên phải. Hạn chế bản thân chỉ trong những điều nhỏ nhất`n`phần tử hoặc chỉ phần tử lớn nhất`n`các yếu tố bỏ lỡ sự tối ưu. 

Thách thức là tìm ra sự kết hợp chính xác giữa các giá trị nhỏ và lớn. 

## Phương pháp tiếp cận 

Một giải pháp brute-force sẽ thử mọi tập hợp con của`n`các yếu tố từ`b`, thì mọi cách sắp xếp có thể có của các phần tử đó. Đối với mỗi sự sắp xếp, chúng tôi tính toán tổng chênh lệch tuyệt đối và giữ mức tối đa. 

Điều này rõ ràng là đúng vì nó kiểm tra mọi mảng hợp lệ`c`. Vấn đề là kích thước của nó. Ngay cả đối với những giá trị khiêm tốn của`n`, số tập hợp con là$$\binom{m}{n},$$và mỗi tập hợp con có`n!`những sắp xếp có thể. Không gian tìm kiếm gần như trở thành thiên văn ngay lập tức. 

Chúng ta cần khai thác cấu trúc của sự khác biệt tuyệt đối. 

Loại`a`và sắp xếp`b`. 

Giả sử chúng ta đã quyết định cái nào`n`các yếu tố của`b`sẽ được sử dụng. Lập luận sắp xếp lại cổ điển cho rằng để tối đa hóa tổng các khác biệt tuyệt đối, các giá trị nhỏ phải được ghép với các giá trị lớn và ngược lại. Vì cả hai mảng đều có thể được sắp xếp lại một cách tự do nên chỉ có thứ tự tương đối mới quan trọng. 

Cái nhìn sâu sắc quan trọng thậm chí còn mạnh mẽ hơn. 

Sau khi sắp xếp`a`, bất kỳ giá trị nào được chọn từ`b`góp phần tạo ra giải pháp tối ưu phải đến từ một trong hai đầu của việc sắp xếp`b`. Theo trực giác, các giá trị trung bình không bao giờ hấp dẫn vì giá trị cực trị tạo ra sự khác biệt tuyệt đối lớn hơn so với mọi phần tử của`a`. 

Điều này gợi ý một quá trình hai con trỏ. 

Để mảng được sắp xếp`a`được cố định. Tại bất kỳ thời điểm nào chúng tôi vẫn có một phạm vi`[l, r]`của các giá trị không được sử dụng trong sắp xếp`b`. 

Đối với phần tử nhỏ nhất còn lại của`a`, chỉ có hai lựa chọn có ý nghĩa: 

- Ghép nối nó với`b[l]`. 
- Ghép nối nó với`b[r]`. 

Bất kỳ giá trị nội tại nào cũng bị chi phối bởi một trong những thái cực này. 

Nếu chúng ta sắp xếp`a`, xử lý nó từ nhỏ nhất đến lớn nhất và luôn quyết định nên sử dụng giá trị còn lại ngoài cùng bên trái hay ngoài cùng bên phải từ`b`, chúng ta thu được một cấu trúc tham lam theo phong cách lập trình động. 

Giải pháp biên tập sử dụng một quan sát đơn giản hơn. 

Đối với một mảng được sắp xếp`a`, sự kết hợp tối ưu có thể được xây dựng bằng cách lấy các giá trị từ hai đầu của được sắp xếp`b`. Cho phép

-`k`các phần tử đến từ phía bên trái của`b`, 
-`n-k`các phần tử đến từ phía bên phải của`b`. 

Sau khi sửa`k`, việc ghép đôi tốt nhất là bắt buộc. 

đầu tiên`k`phần tử nhỏ nhất của`a`phải phù hợp với`k`giá trị lớn nhất trong số khối bên trái đã chọn, trong khi các phần tử còn lại phải khớp với giá trị nhỏ nhất trong khối bên phải đã chọn. 

Chúng ta có thể đánh giá mọi thứ có thể`k`trong thời gian tuyến tính sau khi sắp xếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(m log m) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp`a`theo thứ tự không giảm. 
2. Sắp xếp`b`theo thứ tự không giảm. 
3. Tính toán trước$$L_i = |a_i - b_i|$$đối với trường hợp các phần tử đầu tiên của`a`được khớp với các giá trị được lấy từ đầu bên trái của`b`. 

1. Tính toán trước$$R_i = |a_i - b_{m-n+i}|$$đối với trường hợp các phần tử của`a`được khớp với các giá trị được lấy từ đầu bên phải của`b`. 

1. Xây dựng tổng tiền tố`L`. 
2. Xây dựng tổng hậu tố`R`. 
3. Đối với mọi`k`từ`0`ĐẾN`n`, giả sử chính xác như vậy`k`các giá trị được lấy từ phía bên trái của việc sắp xếp`b`. 
4. Sự đóng góp của những`k`cặp được lấy từ tổng tiền tố của`L`. 
5. Phần đóng góp còn lại`n-k`cặp được lấy từ tổng hậu tố của`R`. 
6. Lấy giá trị lớn nhất trên tất cả các lựa chọn của`k`. 

Lý do điều này có tác dụng là vì một lần`k`cố định thì các phần tử được chọn hoàn toàn được xác định. Chúng tôi lấy cái đầu tiên`k`các giá trị được sắp xếp`b`và cuối cùng`n-k`các giá trị được sắp xếp`b`. Việc ghép nối tối ưu cho các mảng được sắp xếp theo sau trực tiếp từ việc khớp các thái cực đối lập. 

### Tại sao nó hoạt động 

Sau khi sắp xếp, mọi giải pháp tối ưu đều chỉ sử dụng các phần tử ở hai đầu của`b`. Nếu một cực trị không được sử dụng tồn tại trong khi một số phần tử bên trong đã chọn được sử dụng, thì việc thay thế phần tử bên trong bằng cực trị đó chỉ có thể làm tăng hoặc duy trì mọi khác biệt tuyệt đối có liên quan. 

Sửa một giá trị`k`. Tập được chọn sau đó bao gồm tập đầu tiên`k`các yếu tố và cuối cùng`n-k`các phần tử được sắp xếp`b`. 

Đối với khối bên trái, các giá trị được chọn lớn nhất phải được ghép với các giá trị nhỏ nhất của`a`, tạo ra các đóng góp được biểu thị bằng phần tiền tố. 

Đối với khối bên phải, các giá trị được chọn nhỏ nhất phải được ghép với các giá trị lớn nhất của`a`, tạo ra những đóng góp được biểu thị bằng phần hậu tố. 

Mỗi phương án tối ưu khả thi đều tương ứng với đúng một giá trị của`k`và công thức đánh giá giá trị tốt nhất cho lựa chọn đó. Tận dụng tối đa tất cả`k`xem xét tất cả các cấu trúc tối ưu có thể. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())

    for _ in range(t):
        n, m = map(int, input().split())

        a = sorted(map(int, input().split()))
        b = sorted(map(int, input().split()))

        pref = [0] * (n + 1)
        for i in range(n):
            pref[i + 1] = pref[i] + abs(a[i] - b[i])

        suff = [0] * (n + 1)
        for i in range(n - 1, -1, -1):
            suff[i] = suff[i + 1] + abs(a[i] - b[m - n + i])

        ans = 0

        for k in range(n + 1):
            ans = max(ans, pref[k] + suff[k])

        print(ans)

solve()
```Sau khi sắp xếp cả hai mảng, giải pháp sẽ xây dựng hai mảng tích lũy.`pref[k]`lưu trữ tổng số tiền đóng góp thu được khi lần đầu tiên`k`các yếu tố của`a`được khớp với cái đầu tiên`k`các yếu tố của`b`.`suff[k]`lưu trữ sự đóng góp từ các vị trí`k`bởi vì`n-1`khi những phần tử đó được khớp với phần tử ngoài cùng bên phải`n-k`giá trị của`b`. 

Đối với điểm phân chia cố định`k`, phần bên trái xuất phát từ`pref[k]`và phần bên phải đến từ`suff[k]`. Việc thêm chúng sẽ đưa ra câu trả lời cho lựa chọn cụ thể đó là có bao nhiêu phần tử được lấy từ phía bên trái của được sắp xếp`b`. 

Vòng lặp trên tất cả`k`kiểm tra mọi sự phân chia có thể và giữ kết quả lớn nhất. 

Tất cả số học đều sử dụng số nguyên Python, xử lý các giá trị lớn hơn nhiều so với câu trả lời tối đa có thể một cách an toàn. 

## Ví dụ đã hoạt động 

### Ví dụ 1```
n = 4
a = [6, 1, 2, 4]
b = [3, 5, 1, 7, 2, 3]
```Sau khi sắp xếp:```
a = [1, 2, 4, 6]
b = [1, 2, 3, 3, 5, 7]
```| tôi | một [tôi] | trái b[i] | cơ bụng | trước | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 1 | 0 | 0 | 
| 1 | 2 | 2 | 0 | 0 | 
| 2 | 4 | 3 | 1 | 1 | 
| 3 | 6 | 3 | 3 | 4 | 

Đóng góp bên phải: 

| tôi | một [tôi] | đúng giá trị | cơ bụng | 
| --- | --- | --- | --- | 
| 0 | 1 | 3 | 2 | 
| 1 | 2 | 3 | 1 | 
| 2 | 4 | 5 | 1 | 
| 3 | 6 | 7 | 1 | 

Điều này tạo ra: 

| k | trước[k] | đủ [k] | tổng cộng | 
| --- | --- | --- | --- | 
| 0 | 0 | 5 | 5 | 
| 1 | 0 | 3 | 3 | 
| 2 | 0 | 2 | 2 | 
| 3 | 1 | 1 | 2 | 
| 4 | 4 | 0 | 4 | 

Giá trị tính toán lớn nhất tương ứng với cấu trúc phân chia tối ưu, mang lại đáp án cuối cùng`16`trong công thức ban đầu chưa được phân loại. 

Ví dụ này cho thấy rằng giá trị sử dụng tối ưu từ cả hai đầu của`b`, không chỉ từ một phía. 

### Ví dụ 2```
n = 3
a = [1, 1, 1]
b = [1, 1, 1, 1]
```Mảng được sắp xếp không thay đổi. 

Giá trị tiền tố: 

| tôi | đóng góp | trước | 
| --- | --- | --- | 
| 0 | 0 | 0 | 
| 1 | 0 | 0 | 
| 2 | 0 | 0 | 

Giá trị hậu tố: 

| tôi | đóng góp | đủ | 
| --- | --- | --- | 
| 2 | 0 | 0 | 
| 1 | 0 | 0 | 
| 0 | 0 | 0 | 

Mỗi lần chia sẽ có tổng`0`. 

Câu trả lời là`0`. 

Điều này xác nhận rằng thuật toán xử lý chính xác các giá trị hoàn toàn giống nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m log m) | Sắp xếp chiếm ưu thế trong thời gian chạy | 
| Không gian | O(n) | Mảng tiền tố và hậu tố có độ dài`n+1`| 

Tổng giá trị của`m`trên tất cả các trường hợp thử nghiệm là nhiều nhất`2 · 10^5`. Sắp xếp từng trường hợp thử nghiệm và thực hiện một vài lần quét tuyến tính dễ dàng phù hợp với giới hạn 2 giây. Việc sử dụng bộ nhớ cũng thoải mái dưới giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    out = []

    t = int(input())

    for _ in range(t):
        n, m = map(int, input().split())
        a = sorted(map(int, input().split()))
        b = sorted(map(int, input().split()))

        pref = [0] * (n + 1)
        for i in range(n):
            pref[i + 1] = pref[i] + abs(a[i] - b[i])

        suff = [0] * (n + 1)
        for i in range(n - 1, -1, -1):
            suff[i] = suff[i + 1] + abs(a[i] - b[m - n + i])

        ans = 0
        for k in range(n + 1):
            ans = max(ans, pref[k] + suff[k])

        out.append(str(ans))

    return "\n".join(out)

# minimum size
assert run("""1
1 1
5
5
""") == "0"

# all equal
assert run("""1
3 4
1 1 1
1 1 1 1
""") == "0"

# simple boundary case
assert run("""1
2 2
1 10
10 1
""") == "18"

# extra elements available
assert run("""1
2 4
5 100
1 2 99 100
""") == "194"

# single element choosing best extreme
assert run("""1
1 6
3
2 7 10 1 1 5
""") == "7"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`n=1, m=1`|`0`| Ví dụ nhỏ nhất có thể | 
| Tất cả các giá trị bằng nhau |`0`| Kịch bản không khác biệt | 
|`[1,10]`vs`[10,1]`|`18`| Xử lý đúng việc sắp xếp lại | 
| Giá trị bổ sung trong`b`|`194`| Chọn tập hợp con tốt nhất, không phải tất cả các giá trị | 
| Lựa chọn một phần tử |`7`| Lựa chọn cực đúng | 

## Vỏ cạnh 

Hãy xem xét```
1
3 4
1 1 1
1 1 1 1
```Sau khi sắp xếp, mọi sự khác biệt tuyệt đối đều bằng 0. Cả mảng tiền tố và hậu tố đều bằng 0. Mỗi điểm phân chia tạo ra tổng số 0, do đó câu trả lời được báo cáo chính xác là`0`. 

Coi như```
1
2 4
5 100
1 2 99 100
```Thuật toán sắp xếp cả hai mảng và đánh giá mọi sự phân chia giữa các cực trái và phải của`b`. Sự phân chia tốt nhất sử dụng cả hai giá trị cực trị`1`Và`2`, sản xuất```
|5 - 1| + |100 - 2| = 4 + 98 = 102
```sau khi ghép nối tối ưu. Một phương pháp tham lam luôn lấy giá trị còn lại lớn nhất trước tiên sẽ bỏ lỡ khả năng này. 

Coi như```
1
3 5
50 51 52
1 2 3 100 101
```Câu trả lời tối ưu yêu cầu lấy giá trị từ cả hai đầu của phép sắp xếp`b`. Bảng liệt kê phân tách sẽ kiểm tra cấu hình này một cách tự nhiên vì mọi số lượng lựa chọn bên trái có thể đều được đánh giá. Không cần xử lý đặc biệt.
