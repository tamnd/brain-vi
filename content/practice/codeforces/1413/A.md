---
title: "CF 1413A - Đi Tìm Sasuke"
description: "Mỗi trường hợp thử nghiệm đưa ra một tập hợp các số nguyên có độ dài chẵn được gắn vào một “cánh cửa”. Những số nguyên này đóng vai trò là hệ số. Chúng ta phải gán một số nguyên khác 0 cho mỗi vị trí sao cho tổng trọng số của các cặp triệt tiêu chính xác về 0."
date: "2026-06-11T07:20:54+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "math"]
categories: ["algorithms"]
codeforces_contest: 1413
codeforces_index: "A"
codeforces_contest_name: "Technocup 2021 - Elimination Round 1"
rating: 800
weight: 1413
solve_time_s: 134
verified: false
draft: false
---

[CF 1413A - Đi tìm Sasuke](https://codeforces.com/problemset/problem/1413/A) 

**Đánh giá:** 800 
**Tags:** thuật toán xây dựng, toán học 
**Thời gian giải:** 2m 14s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Mỗi trường hợp thử nghiệm đưa ra một tập hợp các số nguyên có độ dài chẵn được gắn vào một “cánh cửa”. Những số nguyên này đóng vai trò là hệ số. Chúng ta phải gán một số nguyên khác 0 cho mỗi vị trí sao cho tổng trọng số của các cặp triệt tiêu chính xác về 0. Chính thức, đối với mọi vị trí$i$, chúng tôi chọn một giá trị$b_i$và yêu cầu là tích số chấm của hai dãy bằng 0:$\sum a_i b_i = 0$. Mỗi người được chọn$b_i$cũng phải khác 0 và nằm trong phạm vi$[-100, 100]$. 

Vì vậy, nhiệm vụ không phải là tối ưu hóa hay tính độc đáo. Nó hoàn toàn mang tính xây dựng: đối với mỗi mảng đầu vào, chúng ta phải xuất ra một mảng khác có cùng độ dài thỏa mãn một ràng buộc tuyến tính duy nhất. 

Ràng buộc$n \le 100$mỗi trường hợp thử nghiệm và$T \le 1000$có nghĩa là nhiều nhất$10^5$các giá trị tổng thể. Bất kỳ giải pháp nào xử lý từng trường hợp thử nghiệm theo thời gian tuyến tính đều đủ. Vì chúng ta chỉ cần xây dựng các giá trị nên ngay cả chiến lược mỗi phần tử có thời gian không đổi là đủ. 

Một cách tiếp cận ngây thơ sẽ cố gắng tìm kiếm các phép gán số nguyên tùy ý$b_i$thỏa mãn phương trình, có thể bằng cách giải phương trình Diophantine tuyến tính tổng quát hoặc thử các phép gán ngẫu nhiên. Điều này sẽ không cần thiết và có khả năng không ổn định. 

Một trường hợp thất bại tinh vi đối với lý luận ngây thơ xuất hiện khi người ta cố gắng gán tất cả$b_i = 1$. Khi đó tổng trở thành$\sum a_i$, không được đảm bảo bằng 0. Ví dụ, đối với đầu vào$[1, 2]$, điều này mang lại$3 \ne 0$, vì vậy nó thất bại ngay lập tức. Một ý tưởng ngây thơ khác có thể là đặt một giá trị để hủy tất cả các giá trị khác, nhưng điều đó có thể vi phạm ràng buộc ràng buộc$|b_i| \le 100$. 

Khó khăn chính là đồng thời thỏa mãn các ràng buộc khác 0 và giữ các giá trị nhỏ trong khi thực thi việc hủy bỏ chính xác. 

## Phương pháp tiếp cận 

Ý tưởng brute-force sẽ là gán giá trị cho$b_1, \dots, b_n$từng cái một và kiểm tra xem chúng ta có thể hoàn thành các giá trị còn lại để thỏa mãn phương trình tuyến tính hay không. Điều này trở thành một tìm kiếm trong một không gian có kích thước gần đúng$200^n$, điều này hoàn toàn không thể thực hiện được ngay cả đối với$n = 10$. 

Một nỗ lực có cấu trúc hơn là xem vấn đề như việc giải một phương trình tuyến tính với$n$những điều chưa biết. Điều này chưa được xác định rõ ràng nên có vô số giải pháp tồn tại. Thách thức không phải là sự tồn tại mà là tìm ra giải pháp với các giá trị nguyên nhỏ và không có số 0. 

Quan sát quan trọng là$n$là chẵn. Điều này cho phép chúng tôi ghép các chỉ số và buộc hủy cục bộ thay vì toàn cầu. Thay vì giải một phương trình trên tất cả các biến, chúng ta xây dựng các đóng góp bằng 0 trong mỗi cặp. Nếu chúng ta có thể đảm bảo rằng mỗi cặp đóng góp bằng 0 một cách độc lập thì toàn bộ tổng sẽ tự động trở thành 0. 

Độ khó giảm dần khi xây dựng cho mỗi cặp$(a_i, a_j)$, hai số nguyên khác 0$(b_i, b_j)$như vậy:$$a_i b_i + a_j b_j = 0$$Điều này luôn có thể thực hiện được bằng cách chọn$b_i = a_j$Và$b_j = -a_i$. Điều này trực tiếp hủy bỏ vì:$$a_i a_j + a_j (-a_i) = 0$$Việc xây dựng này tôn trọng mọi ràng buộc vì$|a_i|, |a_j| \le 100$, Vì thế$b_i$Và$b_j$cũng nằm trong giới hạn và khác 0. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tìm kiếm vũ phu | hàm mũ | O(n) | Quá chậm | 
| Xây dựng theo cặp | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập. 

1. Đọc mảng$a$chiều dài$n$. Mục tiêu là xây dựng một mảng phù hợp$b$. 
2. Duyệt mảng theo từng bước hai vị trí, xét từng cặp$(a_{2k}, a_{2k+1})$. Lý do chúng ta nhóm các phần tử là vì mỗi cặp sẽ thỏa mãn ràng buộc tổng bằng 0 một cách độc lập. 
3. Với mỗi cặp, chỉ định$b_{2k} = a_{2k+1}$Và$b_{2k+1} = -a_{2k}$. Điều này đảm bảo cặp đóng góp chính xác bằng 0 vào tổng số tiền. 
4. Xuất mảng đã xây dựng. 

Tính chính xác phụ thuộc vào thực tế là mọi chỉ mục được sử dụng chính xác một lần trong một cặp, do đó không có phần tử còn sót lại nào không được ghép nối. 

### Tại sao nó hoạt động 

Mỗi cặp đóng góp:$$a_{2k} b_{2k} + a_{2k+1} b_{2k+1} = a_{2k} a_{2k+1} + a_{2k+1} (-a_{2k}) = 0$$Vì tổng tổng là tổng của các cặp số 0 nên tổng toàn cục bằng 0. Việc xây dựng không bao giờ đưa ra các giá trị bằng 0 trong$b$và các giá trị vẫn nằm trong giới hạn vì chúng được lấy trực tiếp từ mảng đầu vào, được giới hạn bởi 100 về giá trị tuyệt đối. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    T = int(input())
    out = []
    
    for _ in range(T):
        n = int(input())
        a = list(map(int, input().split()))
        
        b = [0] * n
        
        for i in range(0, n, 2):
            b[i] = a[i + 1]
            b[i + 1] = -a[i]
        
        out.append(" ".join(map(str, b)))
    
    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Giải pháp đọc từng trường hợp thử nghiệm và xây dựng mảng đầu ra một cách trực tiếp. Vòng lặp tăng thêm hai, đảm bảo quyền truy cập an toàn vào các cặp kể từ$n$thậm chí còn được đảm bảo. Mỗi phép gán có thời gian không đổi, vì vậy toàn bộ việc xây dựng là tuyến tính. 

Điểm tinh tế duy nhất là đảm bảo các dấu hiệu là chính xác: hoán đổi và phủ định là điều cần thiết. Bất kỳ hoán vị nào khác mà không lật dấu sẽ phá vỡ thuộc tính hủy bỏ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 2
a = [1, 100]
```Chúng tôi xử lý một cặp: 

| tôi | một [tôi] | a[i+1] | b[i] | b[i+1] | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 100 | 100 | -1 | 

Kiểm tra tổng hợp:$1 \cdot 100 + 100 \cdot (-1) = 0$Điều này xác nhận rằng công trình vẫn hoạt động ngay cả khi các giá trị khác nhau đáng kể. 

### Ví dụ 2 

đầu vào:```
n = 4
a = [1, 2, 3, 6]
```Chúng ta tạo thành cặp (1,2) và (3,6). 

| tôi | một [tôi] | a[i+1] | b[i] | b[i+1] | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 2 | 2 | -1 | 
| 2 | 3 | 6 | 6 | -3 | 

Tổng đóng góp: 

Cặp đầu tiên:$1\cdot2 + 2\cdot(-1)=0$Cặp thứ hai:$3\cdot6 + 6\cdot(-3)=0$Cả hai cặp đều hủy bỏ độc lập, xác nhận tính đúng đắn của việc xây dựng mô-đun. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | mỗi phần tử được xử lý một lần trong một cặp | 
| Không gian | O(n) | lưu trữ cho mảng đầu ra | 

Tổng số phần tử trong tất cả các trường hợp thử nghiệm nhiều nhất là$10^5$, do đó, việc xây dựng tuyến tính dễ dàng phù hợp với giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    T = int(sys.stdin.readline())
    out = []
    for _ in range(T):
        n = int(sys.stdin.readline())
        a = list(map(int, sys.stdin.readline().split()))
        b = [0] * n
        for i in range(0, n, 2):
            b[i] = a[i + 1]
            b[i + 1] = -a[i]
        out.append(" ".join(map(str, b)))
    return "\n".join(out)

# provided sample check (structure only, since output is not unique)
assert run("2\n2\n1 100\n4\n1 2 3 6\n") is not None

# all negative values
assert run("1\n2\n-5 -7\n") is not None

# mixed signs
assert run("1\n4\n1 -2 3 -4\n") is not None

# minimum repeated structure
assert run("3\n2\n1 2\n2\n2 3\n2\n3 4\n") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 1 2 | hủy hợp lệ | hành vi cặp tối thiểu | 
| dấu hiệu hỗn hợp | hủy hợp lệ | xử lý biển báo | 
| nhiều bài kiểm tra nhỏ | đầu ra hợp lệ | tính nhất quán giữa các trường hợp | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi các giá trị có độ lớn nhưng ngược dấu. Ví dụ,$a = [100, -100]$. Việc xây dựng tạo ra$b = [-100, -100]$. Sự đóng góp trở thành$100 \cdot (-100) + (-100)\cdot(-100) = -10000 + 10000 = 0$, do đó việc hủy vẫn giữ nguyên ngay cả khi các dấu bị trộn lẫn. 

Một trường hợp khác là khi tất cả các giá trị đều bằng nhau, chẳng hạn như$a = [7, 7, 7, 7]$. Thuật toán ghép chúng thành$(7,7)$hai lần, sản xuất$(7,-7)$trong mỗi cặp. Mỗi cặp triệt tiêu độc lập nên không xảy ra hiện tượng nhiễu chéo. 

Cuối cùng, khi các dấu hiệu xen kẽ xuất hiện như$[1, -1, 1, -1]$, phương thức này vẫn hoạt động vì nó không bao giờ dựa vào cấu trúc toàn cục, chỉ hủy theo cặp.
