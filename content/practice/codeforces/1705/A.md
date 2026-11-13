---
title: "CF 1705A - Mark the Nhiếp ảnh gia"
description: "Chúng tôi có tổng cộng $2n$ người có chiều cao đã biết. Mark muốn sắp xếp họ thành hai hàng, mỗi hàng $n$ người: hàng trước và hàng sau."
date: "2026-06-09T21:22:54+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1705
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 807 (Div. 2)"
rating: 800
weight: 1705
solve_time_s: 118
verified: true
draft: false
---

[CF 1705A - Đánh dấu nhiếp ảnh gia](https://codeforces.com/problemset/problem/1705/A) 

**Đánh giá:** 800 
**Tags:** tham lam, phân loại 
**Thời gian giải:** 1 phút 58 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có tổng cộng$2n$những người có chiều cao được biết đến. Mark muốn sắp xếp chúng thành hai hàng$n$mỗi người một hàng trước và một hàng sau. Yêu cầu là đối với mọi vị trí$j$(từ$1$ĐẾN$n$), người ngồi ở hàng sau ít nhất phải$x$đơn vị cao hơn người ngay phía trước. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm đưa ra$n$,$x$, và danh sách$2n$độ cao. Đầu ra là "CÓ" nếu có sự sắp xếp như vậy hoặc "KHÔNG" nếu không có. 

Những hạn chế là nhỏ:$n$nhiều nhất là$100$và chiều cao nhiều nhất là$1000$. Điều này ngụ ý rằng một$O(n \log n)$giải pháp dựa trên sắp xếp sẽ đủ hiệu quả. Các trường hợp cạnh bao gồm các tình huống trong đó tất cả các chiều cao đều bằng nhau,$x$lớn hơn sự khác biệt giữa người cao nhất và người thấp nhất, hoặc khi$n = 1$(cặp đơn). Việc triển khai bất cẩn có thể cố gắng ghép các độ cao một cách tùy ý, dẫn đến giải pháp không thành công trong các trường hợp có chiều cao tối thiểu hoặc bằng nhau này. 

Ví dụ, hãy xem xét đầu vào`1 100`với độ cao`50 50`. Việc ghép đôi đơn giản mà không phân loại có thể khẳng định thành công, nhưng kết quả đầu ra chính xác là "KHÔNG" vì không có người ở hàng sau nào cao hơn người đứng trước 100 đơn vị. 

## Phương pháp tiếp cận 

Giải pháp brute-force xem xét mọi tập hợp con có thể có của$n$người ở hàng sau và mọi hoán vị của những người còn lại ở hàng trước. Đối với mỗi cặp, chúng tôi kiểm tra xem chênh lệch chiều cao ít nhất là$x$cho mọi vị trí. Điều này có độ phức tạp$O(\binom{2n}{n} \cdot n!)$, điều này trở nên không thực tế ngay cả đối với$n=10$. 

Quan sát quan trọng là chúng ta không cần phải xem xét tất cả các hoán vị. Để tối đa hóa cơ hội đạt yêu cầu về chiều cao, chúng ta nên ghép người thấp nhất hàng ghế trước với người thấp nhất hàng sau cao hơn họ. Điều này tự nhiên dẫn đến việc sắp xếp tất cả$2n$người theo chiều cao. Sau đó chúng ta có thể gán cái đầu tiên$n$chiều cao đến hàng đầu và cuối cùng$n$cao tới hàng sau. Kiểm tra$j$-cặp thứ trực tiếp đủ để xác định xem sự sắp xếp có khả thi hay không. 

Cách tiếp cận tham lam này có hiệu quả vì một khi chiều cao đã được sắp xếp, mọi cách sắp xếp khác đều không thể cải thiện được những khác biệt tối thiểu; ghép một người ở hàng sau cao hơn với một người ở hàng trước thấp hơn sẽ làm giảm sự khác biệt còn lại ở hàng sau. Việc sắp xếp đảm bảo rằng nếu điều kiện tối thiểu thất bại đối với bất kỳ cặp nào theo thứ tự đã sắp xếp thì nó không thể thành công theo bất kỳ thứ tự nào khác. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((2n)! / (n!)2) | O(2n) | Quá chậm | 
| Phân loại + Tham lam | O(n log n) | O(2n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case$t$. 
2. Với mỗi test case, hãy đọc$n$,$x$, và danh sách$2n$độ cao. 
3. Sắp xếp toàn bộ danh sách chiều cao theo thứ tự tăng dần. Điều này đảm bảo những người nhỏ nhất sẽ ở đầu và cao nhất ở cuối. 
4. Chỉ định đầu tiên$n$chiều cao đến hàng đầu và cuối cùng$n$cao tới hàng sau. 
5. Lặp lại từng vị trí$j$từ$0$ĐẾN$n-1$. Kiểm tra xem chiều cao của người ngồi hàng sau trừ đi người ngồi hàng trước có nhỏ nhất không$x$. 
6. Nếu tất cả sự khác biệt đáp ứng hoặc vượt quá$x$, in "CÓ"; nếu không thì in "KHÔNG". 

Tại sao nó hoạt động: Việc sắp xếp đảm bảo rằng đối với mỗi người tiềm năng ở hàng đầu, người ở hàng sau tương ứng là người thấp nhất có chiều cao thấp nhất. Nếu cấu hình tối thiểu này không thành công, bất kỳ hoán vị nào cũng sẽ làm giảm một số khác biệt hoặc giữ nguyên chúng, do đó không có sự sắp xếp hợp lệ nào tồn tại. Điều bất biến là$j$-chiều cao hàng sau trong bài tập được sắp xếp luôn lớn hơn hoặc bằng bất kỳ ứng cử viên hàng sau nào khác có thể có cho$j$-người ngồi hàng đầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n, x = map(int, input().split())
    heights = list(map(int, input().split()))
    heights.sort()
    front = heights[:n]
    back = heights[n:]
    possible = True
    for j in range(n):
        if back[j] - front[j] < x:
            possible = False
            break
    print("YES" if possible else "NO")
```Giải pháp này bắt đầu bằng cách đọc dữ liệu đầu vào một cách hiệu quả bằng cách sử dụng`sys.stdin.readline`. Việc sắp xếp đảm bảo các hàng trước và sau được chọn một cách tham lam để tối đa hóa sự khác biệt về chiều cao. Vòng lặp kiểm tra rõ ràng từng vị trí và việc ngắt sớm sẽ tránh những so sánh không cần thiết sau khi phát hiện thấy vi phạm. Một điểm tinh tế là hãy nhớ rằng`heights[n:]`cắt chính xác nửa cao hơn cho hàng sau. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3 6
1 3 9 10 12 16
```Chiều cao được sắp xếp:`[1, 3, 9, 10, 12, 16]`Hàng trước:`[1, 3, 9]`Hàng sau:`[10, 12, 16]`| j | phía trước[j] | trở lại[j] | trở lại[j]-trước[j] | gặp x=6 | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 10 | 9 | Có | 
| 1 | 3 | 12 | 9 | Có | 
| 2 | 9 | 16 | 7 | Có | 

Tất cả sự khác biệt ≥ 6, xuất ra "CÓ". 

### Mẫu 2 

đầu vào:```
3 1
2 5 2 2 2 5
```Chiều cao được sắp xếp:`[2, 2, 2, 2, 5, 5]`Hàng trước:`[2, 2, 2]`Hàng sau:`[2, 5, 5]`| j | phía trước[j] | trở lại[j] | trở lại[j]-trước[j] | gặp x=1 | 
| --- | --- | --- | --- | --- | 
| 0 | 2 | 2 | 0 | Không | 

Thất bại ở vị trí đầu tiên, xuất ra "NO". 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) cho mỗi trường hợp thử nghiệm | Sắp xếp chiếm ưu thế; lặp lại các cặp là O(n) | 
| Không gian | O(n) | Hai lát cho hàng trước và hàng sau; không đáng kể so với đầu vào | 

Với$n \le 100$Và$t \le 100$, giải pháp này thực hiện tối đa$100 * 100 log 100 ≈ 70000$hoạt động, trong giới hạn 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    t = int(input())
    for _ in range(t):
        n, x = map(int, input().split())
        heights = list(map(int, input().split()))
        heights.sort()
        front = heights[:n]
        back = heights[n:]
        possible = True
        for j in range(n):
            if back[j] - front[j] < x:
                possible = False
                break
        print("YES" if possible else "NO")
    return output.getvalue().strip()

# Provided samples
assert run("3\n3 6\n1 3 9 10 12 16\n3 1\n2 5 2 2 2 5\n1 2\n8 6") == "YES\nNO\nYES"

# Custom tests
assert run("1\n1 100\n50 50") == "NO", "equal heights cannot satisfy large x"
assert run("1\n2 0\n1 2 3 4") == "YES", "x=0 always succeeds"
assert run("1\n3 2\n1 2 3 4 5 6") == "YES", "simple increasing sequence"
assert run("1\n3 4\n1 2 3 4 5 6") == "NO", "x too large for minimal difference"
assert run("1\n1 1\n1 3") == "YES", "single pair success"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 100\n50 50`| KHÔNG | chiều cao bằng nhau không thể thỏa mãn x lớn | 
|`2 0\n1 2 3 4`| CÓ | x=0 thành công một cách tầm thường | 
|`3 2\n1 2 3 4 5 6`| CÓ | dãy tăng cơ bản | 
|`3 4\n1 2 3 4 5 6`| KHÔNG | x quá lớn để có sự khác biệt tối thiểu | 
|`1 1\n1 3`| CÓ | trường hợp n cạnh nhỏ nhất | 

## Vỏ cạnh 

Đối với các độ cao bằng nhau có x cao, ví dụ:`n=1`,`x=100`, độ cao`[50,50]`, thuật toán sắp xếp thành`[50,50]`, gán phía trước =`[50]`, quay lại =`[50]`, Và
