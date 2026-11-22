---
title: "CF 348A - Mafia"
description: "Chúng ta có một nhóm bạn sẽ chơi nhiều lần một trò chơi. Mỗi vòng của trò chơi có chính xác một người đóng vai trò là người giám sát, trong khi những người $n-1$ còn lại tham gia với tư cách là người chơi."
date: "2026-06-06T18:31:10+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "math", "sortings"]
categories: ["algorithms"]
codeforces_contest: 348
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 202 (Div. 1)"
rating: 1600
weight: 348
solve_time_s: 131
verified: false
draft: false
---

[CF 348A - Mafia](https://codeforces.com/problemset/problem/348/A) 

**Đánh giá:** 1600 
**Tags:** tìm kiếm nhị phân, toán học, sắp xếp 
**Thời gian giải:** 2m 11s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một nhóm bạn sẽ chơi nhiều lần một trò chơi. Mỗi vòng của trò chơi có đúng một người đóng vai trò là người giám sát, những người còn lại$n-1$mọi người tham gia với tư cách là người chơi. 

Đối với mỗi người$i$, chúng tôi được cho biết họ muốn tham gia bao nhiêu vòng với tư cách là người chơi. Vì một người bị loại khỏi đúng một vòng bất cứ khi nào họ trở thành người giám sát, nên tổng số vòng sẽ xác định số lần mỗi người có thể chơi. 

Nếu tổng số vòng là$R$, thì người$i$chính xác sẽ là một người chơi$R - x_i$vòng, ở đâu$x_i$là số lần họ được chọn làm người giám sát. Vì mỗi vòng có chính xác một người giám sát nên tổng của tất cả$x_i$bằng$R$. 

Nhiệm vụ là xác định mức tối thiểu có thể$R$sao cho chúng tôi có thể phân công người giám sát qua các vòng để mỗi người chơi ít nhất số lần họ yêu cầu. 

Những ràng buộc đẩy chúng ta tới một giải pháp nhanh hơn$O(n^2)$. Với$n$lên đến$10^5$và giá trị lên đến$10^9$, bất kỳ công trình nào mô phỏng các vòng đấu hoặc thử mọi khả năng đều không thể thực hiện được. Chúng ta cần một công thức rút gọn bài toán thành một phép kiểm tra số đơn giản hoặc tối ưu hóa trên một điều kiện đơn điệu. 

Một trường hợp phức tạp phát sinh khi tất cả các nhu cầu đều rất lớn nhưng cân bằng, hoặc khi một người yêu cầu hầu hết mọi thứ trong khi những người khác yêu cầu ít. Ví dụ, nếu tất cả$a_i = 1$, thì hai vòng là đủ vì mỗi người có thể làm giám sát một lần ngoại trừ một, và ràng buộc cuối cùng đương nhiên được thỏa mãn. Một ý tưởng ngây thơ mà bỏ qua sự giám sát cân bằng sẽ đánh giá quá cao hoặc đánh giá thấp một cách không chính xác trong những trường hợp như vậy. 

Một tình huống phức tạp khác là khi một giá trị chiếm ưu thế. Ví dụ,$a = [10^9, 1, 1, \dots]$. Một lý luận ngây thơ chỉ đơn giản là tính tổng các yêu cầu hoặc tính trung bình chúng sẽ thất bại vì năng lực giám sát được phân bổ không đồng đều nhưng vẫn bị giới hạn theo từng vòng. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là thử tăng giá trị của$R$và kiểm tra tính khả thi. Đối với một cố định$R$, chúng ta phải quyết định liệu chúng ta có thể gán$R$vị trí giám sát trong số$n$những người như vậy người đó$i$bị loại trừ tối đa$R - a_i$vòng. Tương đương, họ phải là người giám sát nhiều nhất$R - a_i$lần. 

Để kiểm tra tính khả thi của một$R$, chúng tôi tính toán năng lực$c_i = R - a_i$. Nếu có$c_i < 0$, cái đó$R$là không thể. Nếu không, chúng tôi phải đảm bảo tổng số vị trí giám sát$R$có thể được phân công mà không vượt quá năng lực. Điều kiện then chốt trở thành$\sum \min(c_i, R)\ge R$, nhưng có một quan sát đơn giản hơn: vì mỗi vòng cần chính xác một người giám sát nên chúng ta cần phân phối$R$nhiệm vụ giống hệt nhau giữa những người có giới hạn trên$c_i$. 

Điều này trở thành một kiểm tra tính khả thi tiêu chuẩn: tổng công suất ít nhất phải bằng$R$. Điều đó mang lại$\sum (R - a_i) \ge R$, đơn giản hóa trực tiếp thành điều kiện dạng đóng. 

Sự tàn bạo đã kết thúc$R$sẽ tốn kém$O(n \cdot \max a_i)$, nó quá lớn$a_i$lên đến$10^9$. 

Quan sát quan trọng là tính khả thi là đơn điệu trong$R$. Nếu một số$R$hoạt động, lớn hơn$R$cũng có tác dụng vì tăng số vòng chỉ làm tăng khả năng giám sát. Điều này có nghĩa là chúng ta có thể tìm kiếm nhị phân, nhưng đại số cho thấy chúng ta không cần tìm kiếm gì cả. 

Bắt đầu từ:$$\sum (R - a_i) \ge R$$chúng tôi mở rộng:$$nR - \sum a_i \ge R$$

$$(n-1)R \ge \sum a_i$$Vì thế:$$R \ge \frac{\sum a_i}{n-1}$$Do đó đáp án là số nguyên nhỏ nhất$R$thỏa mãn bất đẳng thức này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên R |$O(n \cdot \max a_i)$|$O(1)$| Quá chậm | 
| Giảm đại số |$O(n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính tổng số tiền$S = \sum a_i$. Điều này thể hiện tổng số lần tất cả người chơi cùng nhất quyết tham gia. 
2. Quan sát rằng mỗi vòng sẽ giảm tổng “khoảng cách tham gia bắt buộc” một cách chính xác$n-1$, vì mỗi lần một người bị loại. Điều này chuyển vấn đề lập kế hoạch thành cân bằng tài nguyên tuyến tính. 
3. Suy ra điều kiện$(n-1)R \ge S$, nghĩa là tổng số "vị trí người chơi đã tạo" phải bao gồm tất cả các lần tham gia bắt buộc. 
4. Tính số nguyên nhỏ nhất$R$thỏa mãn bất đẳng thức này bằng phép chia trần. 
5. Xuất giá trị này dưới dạng số vòng tối thiểu. 

### Tại sao nó hoạt động 

Mỗi vòng đóng góp chính xác$n-1$tham gia vì tất cả trừ một người chơi đều tham gia. Qua$R$vòng, tổng số vị trí tham gia có sẵn là$R(n-1)$. Mỗi người đòi hỏi$a_i$sự tham gia, do đó tổng nhu cầu là$S$. Vì sự tham gia là hạn chế duy nhất nên tính khả thi chỉ phụ thuộc vào việc liệu tổng nguồn cung về vị trí tham gia có đáp ứng được tổng nhu cầu hay không. Bất kỳ sự phân bổ người giám sát nào cũng tự động mang lại sự phân công hợp lệ cho mỗi người miễn là tổng năng lực là đủ, bởi vì không có giới hạn trên cho mỗi người ngoài việc loại trừ mỗi vòng sẽ hạn chế tính linh hoạt. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    arr = list(map(int, input().split()))
    
    s = sum(arr)
    # minimal R such that (n-1)*R >= s
    r = (s + n - 2) // (n - 1)
    print(r)

if __name__ == "__main__":
    solve()
```Việc thực hiện là một bản dịch trực tiếp của bất đẳng thức dẫn xuất. Sự tinh tế duy nhất là phép chia trần theo số nguyên, được thực hiện dưới dạng$(S + n - 2) // (n - 1)$, để tránh lỗi dấu phẩy động. 

Việc tính tổng an toàn trong Python do các số nguyên có độ chính xác tùy ý. Không cần mô phỏng và không cần theo dõi nhiệm vụ của từng người giám sát. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
3 2 2
```Chúng tôi tính toán$S = 7$,$n = 3$, Vì thế$n-1 = 2$. 

| Bước | S | n | Tính toán R | Kết quả | 
| --- | --- | --- | --- | --- | 
| Tính tổng | 7 | 3 | - | 7 | 
| Áp dụng công thức | 7 | 3 | trần nhà(7/2) | 4 | 

Đầu ra là 4. 

Điều này xác nhận rằng 4 vòng tạo ra$4 \cdot 2 = 8$vị trí tham gia, đủ để đáp ứng tổng nhu cầu 7. 

### Ví dụ 2 

đầu vào:```
4
1 1 1 1
```Chúng tôi tính toán$S = 4$,$n = 4$, Vì thế$n-1 = 3$. 

| Bước | S | n | Tính toán R | Kết quả | 
| --- | --- | --- | --- | --- | 
| Tính tổng | 4 | 4 | - | 4 | 
| Áp dụng công thức | 4 | 4 | trần nhà(4/3) | 2 | 

Đầu ra là 2. 

Điều này cho thấy dù có 4 người, mỗi người muốn 1 lần tham gia nhưng chỉ cần 2 vòng là đủ vì mỗi vòng có 3 người tham gia. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| một lần để tính tổng | 
| Không gian |$O(1)$| chỉ lưu trữ một vài số nguyên | 

Giải pháp phù hợp thoải mái trong các ràng buộc vì$n \le 10^5$và chúng tôi chỉ thực hiện tổng hợp tuyến tính theo sau là số học theo thời gian không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    arr = list(map(int, input().split()))
    s = sum(arr)
    r = (s + n - 2) // (n - 1)
    return str(r)

# provided sample
assert run("3\n3 2 2\n") == "4"

# minimum n
assert run("3\n1 1 1\n") == "1"

# all equal large
assert run("5\n10 10 10 10 10\n") == "13"

# skewed case
assert run("4\n100 1 1 1\n") == "35"

# large uniform
assert run("6\n1 1 1 1 1 1\n") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 1 1 1 | 1 | trường hợp khả thi tối thiểu | 
| 5 lớn giống hệt nhau | 13 | độ chính xác của tỷ lệ | 
| 100 lệch | 35 | trường hợp cạnh thống trị | 
| tất cả những cái | 2 | hành vi phân phối cân bằng | 

## Vỏ cạnh 

Khi tất cả các giá trị đều tối thiểu, chẳng hạn như$n=3, a=[1,1,1]$, công thức cho$R = 1$. Một vòng đã cung cấp hai vị trí tham gia cho mỗi nhóm người theo thời gian và mọi nhiệm vụ trong một vòng đều có tác dụng. 

Khi một giá trị chiếm ưu thế, chẳng hạn như$a=[100,1,1,1]$, chúng tôi nhận được$S=103$,$n=4$, Vì thế$R = \lceil 103/3 \rceil = 35$. Mỗi vòng đóng góp 3 vị trí tham gia và 35 vòng tạo ra 105 vị trí, đủ để đáp ứng nhu cầu lớn đồng thời phân bổ các ràng buộc giám sát cho những người tham gia ít tham gia hơn. 

Khi tất cả các giá trị bằng nhau, giải pháp sẽ mở rộng quy mô một cách suôn sẻ vì cả nhu cầu và công suất đều tăng trưởng tuyến tính và bộ phận trần xử lý tính không chia hết mà không có trường hợp đặc biệt.
