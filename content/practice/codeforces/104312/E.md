---
title: "CF 104312E - Tấn công người khổng lồ"
description: "Chúng ta có ba bức tường độc lập, mỗi bức tường được mô tả như một dãy các chiều cao của từng phần. Mỗi mảng chứa $n$ số nguyên, trong đó mỗi số nguyên biểu thị chiều cao của một đoạn trong bức tường đó."
date: "2026-07-01T19:52:17+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104312
codeforces_index: "E"
codeforces_contest_name: "UTPC Spring 2023 Contest (HS)"
rating: 0
weight: 104312
solve_time_s: 60
verified: true
draft: false
---

[CF 104312E - Tấn công người khổng lồ](https://codeforces.com/problemset/problem/104312/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có ba bức tường độc lập, mỗi bức tường được mô tả như một dãy các chiều cao của từng phần. Mỗi mảng chứa$n$số nguyên, trong đó mỗi số nguyên biểu thị chiều cao của một đoạn trên bức tường đó. Mục tiêu không phải là khớp các vị trí trên các bức tường mà chỉ để tìm giá trị chiều cao xuất hiện trong cả ba mảng. Trong số tất cả các độ cao được chia sẻ như vậy, chúng ta phải trả về độ cao lớn nhất. Nếu cả ba bức tường không có chiều cao chung thì câu trả lời là$-1$. 

Chi tiết quan trọng là chúng tôi đang so sánh các giá trị chứ không phải chỉ số. Chiều cao hợp lệ nếu nó xuất hiện ít nhất một lần ở Wall Maria, ít nhất một lần ở Wall Rose và ít nhất một lần ở Wall Sina. 

Ràng buộc$n \le 10^5$ngụ ý rằng mỗi bức tường có thể chứa tới một trăm nghìn giá trị và có ba mảng như vậy. So sánh bậc hai hoặc bậc ba trực tiếp giữa các mảng sẽ dẫn đến kết quả gần đúng$10^{10}$hoạt động trong trường hợp xấu nhất, vượt xa giới hạn 2 giây có thể xử lý. Điều này ngay lập tức loại trừ việc quét theo cặp giữa tất cả các phần tử của ba mảng. 

Bản thân các giá trị được giới hạn bởi$10^5$, đó là một gợi ý cấu trúc quan trọng. Nó gợi ý rằng việc đếm tần số hoặc đánh dấu sự hiện diện trên một miền cố định có thể khả thi. 

Một vài trường hợp đặc biệt quan trọng trong thực tế. Nếu tất cả các giá trị trong tất cả các mảng là khác nhau thì không có sự trùng lặp và kết quả đầu ra phải là$-1$. Nếu tất cả các mảng đều chứa các giá trị giống nhau thì câu trả lời chỉ đơn giản là giá trị lớn nhất hiện có. Một trường hợp tinh tế khác phát sinh khi có sự trùng lặp trong mảng; sự trùng lặp sẽ không ảnh hưởng đến tính chính xác vì chúng tôi chỉ quan tâm đến sự tồn tại chứ không phải tần suất. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Đối với mọi giá trị ở bức tường đầu tiên, chúng tôi kiểm tra xem nó có xuất hiện ở bức tường thứ hai và bức tường thứ ba hay không. Điều này có thể được thực hiện bằng cách sử dụng các vòng lặp lồng nhau hoặc tìm kiếm tuyến tính. 

Nếu chúng ta làm điều này một cách ngây thơ, đối với mỗi$n$các phần tử trong mảng đầu tiên, chúng tôi có thể quét tới$n$các yếu tố trong thứ hai và khác$n$ở phần thứ ba. Điều này dẫn đến$O(n^3)$trong trường hợp xấu nhất nếu được thực hiện với các vòng lặp thô hoặc$O(n^2)$nếu chúng tôi kiểm tra trước tư cách thành viên bằng cách sử dụng danh sách và quét tuyến tính. Thậm chí$O(n^2)$dịch sang$10^{10}$những hoạt động không khả thi. 

Quan sát quan trọng là chúng ta không cần so sánh các phần tử theo cặp. Chúng ta chỉ cần biết liệu một giá trị có tồn tại trong mỗi mảng hay không. Điều này biến bài toán thành một bài toán giao nhau đã định sẵn. Khi mỗi mảng được chuyển đổi thành cấu trúc thành viên, việc kiểm tra xem một giá trị xuất hiện trong cả ba có phải là thời gian không đổi hay không. 

Vì các giá trị được giới hạn bởi$10^5$, chúng ta cũng có thể sử dụng mảng tần số boolean hoặc bộ băm. Sau khi đánh dấu sự hiện diện cho mỗi bức tường, chúng tôi quét qua phạm vi giá trị có thể có và chọn giá trị lớn nhất có trong cả ba bức tường. 

Điều này làm giảm vấn đề từ việc tìm kiếm lặp đi lặp lại thành một lần quét tuyến tính trên miền giá trị. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$ĐẾN$O(n^3)$|$O(1)$| Quá chậm | 
| Mảng / bộ hiện diện |$O(n + V)$|$O(V)$| Đã chấp nhận | 

Đây$V = 10^5$, giá trị lớn nhất có thể 

## Hướng dẫn thuật toán 

1. Tạo ba mảng (hoặc bộ) boolean biểu thị giá trị nào xuất hiện trên mỗi bức tường. Bước này chuyển đổi danh sách thô thành cấu trúc thành viên nhanh để chúng tôi có thể trả lời các truy vấn tồn tại trong thời gian không đổi. 
2. Đối với mỗi chiều cao trong Tường Maria, hãy đánh dấu nó như hiện diện trong cấu trúc đầu tiên. Các giá trị lặp đi lặp lại không thành vấn đề vì chúng ta chỉ quan tâm đến sự tồn tại. 
3. Lặp lại quy trình đánh dấu tương tự cho Wall Rose và Wall Sina, lần lượt điền vào cấu trúc thứ hai và thứ ba. 
4. Lặp lại tất cả các độ cao có thể từ$1$ĐẾN$10^5$, kiểm tra xem một giá trị có được đánh dấu trong cả ba cấu trúc hay không. Điều này có hiệu quả vì tất cả độ cao hợp lệ phải nằm trong giới hạn nhất định. 
5. Theo dõi giá trị lớn nhất thỏa mãn sự hiện diện trong cả ba mảng. Vì chúng tôi quét theo thứ tự tăng dần nên chúng tôi có thể chỉ cần ghi đè lên câu trả lời bất cứ khi nào chúng tôi tìm thấy giá trị hợp lệ. 
6. Sau khi quét, nếu không tìm thấy giá trị nào, hãy quay lại$-1$, nếu không thì trả về mức tối đa đã ghi. 

### Tại sao nó hoạt động 

Thuật toán dựa trên thực tế là các thành viên trong mỗi bức tường là độc lập và không có trật tự. Bằng cách chuyển đổi mỗi bức tường thành một tập hợp các giá trị hiện tại, chúng tôi lưu giữ chính xác thông tin cần thiết cho vấn đề và loại bỏ mọi thứ không liên quan, chẳng hạn như thứ tự và sự trùng lặp. Lần quét cuối cùng sẽ kiểm tra từng chiều cao ứng cử viên dựa trên sự thể hiện chính xác sự tồn tại trong cả ba bức tường, đảm bảo rằng mọi giá trị được báo cáo thực sự xuất hiện trong tất cả các mảng. Vì chúng tôi đánh giá tất cả các độ cao có thể có trong phạm vi cho phép nên không thể bỏ sót câu trả lời hợp lệ nào và việc chọn mức tối đa trong số đó đảm bảo tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    m = list(map(int, input().split()))
    r = list(map(int, input().split()))
    s = list(map(int, input().split()))
    
    MAXV = 100000
    
    in_m = [False] * (MAXV + 1)
    in_r = [False] * (MAXV + 1)
    in_s = [False] * (MAXV + 1)
    
    for x in m:
        in_m[x] = True
    for x in r:
        in_r[x] = True
    for x in s:
        in_s[x] = True
    
    ans = -1
    for v in range(1, MAXV + 1):
        if in_m[v] and in_r[v] and in_s[v]:
            ans = v
    
    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp đầu tiên là xây dựng ba bảng hiện diện, mỗi bảng một bức tường. Mỗi bảng ghi lại xem bức tường đó có tồn tại chiều cao hay không. Điều này tránh việc quét lặp lại và đảm bảo kiểm tra tư cách thành viên liên tục. 

Vòng lặp cuối cùng quét tất cả các độ cao có thể theo thứ tự tăng dần. Mỗi khi tìm thấy chiều cao trong cả ba mảng, nó sẽ cập nhật câu trả lời. Vì chúng tôi quét lên trên nên giá trị được ghi cuối cùng là chiều cao hợp lệ tối đa. 

Một điểm tinh tế là phạm vi cố định$1 \le h \le 10^5$. Điều này cho phép chúng ta phân bổ các mảng có kích thước một cách an toàn$100001$mà không phải lo lắng về giới hạn động. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 5
Maria = [1, 2, 3, 8, 5]
Rose  = [5, 6, 7, 8, 9]
Sina  = [8, 12, 14, 19, 12]
```| v | ở Maria | ở Hoa hồng | ở Sina | trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | T | F | F | -1 | 
| 2 | T | F | F | -1 | 
| 3 | T | F | F | -1 | 
| 5 | T | T | F | -1 | 
| 8 | T | T | T | 8 | 

Quá trình quét cho thấy chỉ có giá trị 8 xuất hiện trong cả ba mảng nên nó trở thành đáp án cuối cùng. 

### Ví dụ 2 

đầu vào:```
n = 4
Maria = [10, 20, 30, 40]
Rose  = [15, 20, 35, 45]
Sina  = [5, 25, 20, 60]
```| v | ở Maria | ở Hoa hồng | ở Sina | trả lời | 
| --- | --- | --- | --- | --- | 
| 20 | T | T | T | 20 | 

Chỉ có 20 là phổ biến trên tất cả các mảng, vì vậy nó được trả về trực tiếp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n + V)$| Mỗi mảng được xử lý một lần, sau đó chúng tôi quét toàn bộ phạm vi giá trị | 
| Không gian |$O(V)$| Ba mảng boolean có kích thước lên tới$10^5$| 

Các ràng buộc cho phép lên đến$n = 10^5$, Và$V = 10^5$, do đó giải pháp chạy thoải mái trong giới hạn. Các hoạt động là tuyến tính và việc sử dụng bộ nhớ là cố định và nhỏ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    input = sys.stdin.readline
    
    n = int(input())
    m = list(map(int, input().split()))
    r = list(map(int, input().split()))
    s = list(map(int, input().split()))
    
    MAXV = 100000
    in_m = [False] * (MAXV + 1)
    in_r = [False] * (MAXV + 1)
    in_s = [False] * (MAXV + 1)
    
    for x in m:
        in_m[x] = True
    for x in r:
        in_r[x] = True
    for x in s:
        in_s[x] = True
    
    ans = -1
    for v in range(1, MAXV + 1):
        if in_m[v] and in_r[v] and in_s[v]:
            ans = v
    
    return str(ans)

# provided sample
assert run("""5
1 2 3 8 5
5 6 7 8 9
8 12 14 19 12
""") == "8"

# custom 1: no intersection
assert run("""3
1 2 3
4 5 6
7 8 9
""") == "-1"

# custom 2: all equal
assert run("""3
1 2 3
1 2 3
1 2 3
""") == "3"

# custom 3: single element
assert run("""1
5
5
5
""") == "5"

# custom 4: multiple common, choose max
assert run("""6
1 2 3 4 5 6
2 3 4 5 6 7
0 2 4 6 8 10
""") == "6"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| không chồng chéo | -1 | xử lý vắng mặt | 
| mảng giống hệt nhau | giá trị tối đa | lựa chọn tối đa chính xác | 
| phần tử đơn | giá trị | độ chính xác đầu vào tối thiểu | 
| chồng chéo nhiều lần | 6 | đúng nút giao tối đa | 

## Vỏ cạnh 

Trường hợp tất cả các mảng chứa các giá trị rời rạc thể hiện cách xử lý chính xác sự vắng mặt của thuật toán. Ví dụ:```
Maria = [1,2], Rose = [3,4], Sina = [5,6]
```Trong quá trình quét, không có chỉ mục$v$thỏa mãn cả ba kiểm tra boolean, vì vậy`ans`còn lại$-1$, được trả về chính xác. 

Trường hợp thứ hai là khi sự trùng lặp chiếm ưu thế:```
Maria = [7,7,7], Rose = [7,7], Sina = [7]
```Mặc dù số lượng khác nhau nhưng mảng hiện diện chỉ đánh dấu sự tồn tại. Tại$v = 7$, cả ba cờ đều đúng, do đó câu trả lời được xác định chính xác là 7.
