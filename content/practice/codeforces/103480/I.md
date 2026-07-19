---
title: "CF 103480I - \u597d\u60f3\u542c\u8086\u5b9d\u5531\u6b4c\u554a"
description: "Chúng tôi được cung cấp một bộ sưu tập các bài hát, trong đó mỗi bài hát có một giá trị phổ biến duy nhất và một tên duy nhất. Giá trị mức độ phổ biến hoạt động giống như một yếu tố xếp hạng nghiêm ngặt: không có hai bài hát nào có cùng số điểm và giá trị cao hơn có nghĩa là một bài hát được mong muốn hơn."
date: "2026-07-03T06:32:23+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103480
codeforces_index: "I"
codeforces_contest_name: "The 4th Hangzhou Normal University Freshman Programming Contest"
rating: 0
weight: 103480
solve_time_s: 46
verified: true
draft: false
---

[CF 103480I - \u597d\u60f3\u542c\u8086\u5b9d\u5531\u6b4c\u554a](https://codeforces.com/problemset/problem/103480/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 46s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một bộ sưu tập các bài hát, trong đó mỗi bài hát có một giá trị phổ biến duy nhất và một tên duy nhất. Giá trị mức độ phổ biến hoạt động giống như một yếu tố xếp hạng nghiêm ngặt: không có hai bài hát nào có cùng số điểm và giá trị cao hơn có nghĩa là một bài hát được mong muốn hơn. 

Nhiệm vụ là tưởng tượng tất cả các bài hát được sắp xếp theo mức độ phổ biến giảm dần. Một số bài hát phổ biến nhất đã được người dùng khác sử dụng. Chúng ta được biết chính xác có bao nhiêu lựa chọn hàng đầu, chẳng hạn như k trong số đó, đã biến mất. Công việc của chúng ta là xác định bài hát hiện có hay nhất tiếp theo, nghĩa là bài hát sẽ xuất hiện ở vị trí k + 1 theo thứ tự được sắp xếp đó. 

Kích thước đầu vào có thể lên tới 100.000 bài hát. Điều này ngay lập tức loại trừ mọi cách tiếp cận liên tục quét danh sách theo kiểu tuyến tính cho mỗi lần xóa hoặc sử dụng các vòng lặp bên trong sắp xếp lặp lại. Giải pháp O(n log n) có thể chấp nhận được, nhưng mọi phương trình bậc hai sẽ không thành công dưới giới hạn 1 giây. 

Trường hợp cạnh tinh vi phát sinh khi k bằng 0. Trong trường hợp này, không có bài hát nào được chọn nên câu trả lời đơn giản là bài hát phổ biến nhất trên toàn cầu. Một trường hợp góc khác là khi k bằng n − 1, trong đó chỉ có một bài hát còn hợp lệ và nó phải được trả về bất kể vị trí. 

Một sai lầm ngây thơ là sắp xếp theo thứ tự tăng dần và vô tình chọn số nhỏ thứ k thay vì số lớn thứ k. Ví dụ: nếu chúng tôi diễn giải không chính xác "mong muốn nhất", chúng tôi sẽ đảo ngược thứ hạng và trả lại bài hát sai một cách nhất quán mặc dù việc sắp xếp vẫn đúng. 

## Phương pháp tiếp cận 

Ý tưởng đơn giản nhất là sắp xếp rõ ràng tất cả các bài hát theo giá trị phổ biến của chúng theo thứ tự giảm dần. Sau khi sắp xếp, chúng tôi trực tiếp lập chỉ mục vị trí thứ k. 

Điều này hiệu quả vì việc sắp xếp cung cấp cho chúng ta thứ tự chung của tất cả các mục theo cùng một quy tắc so sánh xác định mức độ ưu tiên. Sau khi sắp xếp, việc loại bỏ k mục đầu tiên về mặt khái niệm tương đương với việc bỏ qua chúng và lấy mục tiếp theo. 

Một giải pháp thay thế mạnh mẽ là quét liên tục danh sách để tìm mức tối đa hiện tại, xóa nó và lặp lại k + 1 lần này. Mỗi lần quét tốn O(n) và thực hiện k lần sẽ dẫn đến O(nk), trong trường hợp xấu nhất sẽ trở thành O(n²). Với n lên đến 10⁵, điều này vượt xa giới hạn khả thi. 

Quan sát quan trọng là chúng ta không cần loại bỏ động. Chúng tôi chỉ cần xếp hạng cuối cùng một lần. Điều đó biến vấn đề thành một vấn đề sắp xếp tĩnh, đó chính xác là cách sắp xếp giải quyết một cách hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Trích xuất tối đa lặp đi lặp lại | O(n²) | O(1) | Quá chậm | 
| Sắp xếp theo giá trị | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các bài hát vào danh sách theo cặp (mức độ phổ biến, tên). Điều này bảo toàn cả khóa xếp hạng và mã định danh mà chúng tôi phải xuất ra. 
2. Sắp xếp danh sách theo mức độ phổ biến giảm dần. Điều này tạo ra một bảng xếp hạng toàn cầu trong đó yếu tố đầu tiên là bài hát được mong muốn nhất và mỗi yếu tố tiếp theo hoàn toàn ít được mong muốn hơn yếu tố trước đó. Tính phổ biến duy nhất đảm bảo không có mối ràng buộc nào, do đó thứ tự mang tính quyết định. 
3. Đọc số nguyên k, biểu thị số lượng bài hát xếp hạng cao nhất đã được sử dụng. 
4. Truy cập trực tiếp vào phần tử tại chỉ mục k trong danh sách đã sắp xếp. Điều này tương ứng với bài hát phổ biến nhất (k + 1) trong bảng xếp hạng dựa trên 1. 
5. Xuất ra tên liên quan đến phần tử đó. 

### Tại sao nó hoạt động 

Sau khi sắp xếp, danh sách này là thứ tự tổng thể của các bài hát theo mức độ phổ biến giảm dần. Bởi vì tất cả các giá trị phổ biến đều khác biệt nên thứ tự này rất chặt chẽ và ổn định. Việc loại bỏ phần tử k trên cùng không làm thay đổi thứ tự tương đối của các phần tử còn lại, do đó phần tử còn lại đầu tiên chính xác là chỉ mục thứ k trong lập chỉ mục dựa trên 0. Thuật toán không bao giờ phụ thuộc vào các cập nhật hoặc xóa trung gian, do đó không thể xảy ra lỗi trạng thái hoặc sắp xếp lại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    songs = []
    
    for _ in range(n):
        w, s = input().split()
        w = int(w)
        songs.append((w, s))
    
    k = int(input())
    
    songs.sort(reverse=True)
    
    print(songs[k][1])

if __name__ == "__main__":
    solve()
```Cốt lõi của việc triển khai là lệnh gọi sắp xếp, sắp xếp các bộ dữ liệu theo phần tử đầu tiên theo thứ tự giảm dần do`reverse=True`. Vì Python so sánh các bộ dữ liệu theo từ điển nên tên chỉ được sử dụng làm khóa phụ nhưng không thành vấn đề vì trọng số được đảm bảo là duy nhất. 

Lập chỉ mục trực tiếp`songs[k]`an toàn vì k đảm bảo thỏa mãn 0  k < n. Không cần kiểm tra ranh giới ngoài việc tin tưởng vào các ràng buộc đầu vào. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
flos 1
Yellow 3
Starduster 9
1
```Thứ tự sắp xếp trở thành:```
(9, Starduster)
(3, Yellow)
(1, flos)
```| Bước | Hành động | Tiểu bang | 
| --- | --- | --- | 
| Đọc đầu vào | lưu trữ bài hát | [(1, flos), (3, Vàng), (9, Starduster)] | 
| Sắp xếp | giảm dần | [(9, Starduster), (3, Vàng), (1, flos)] | 
| k = 1 | chọn chỉ số 1 | (3, Vàng) | 

Đầu ra:```
Yellow
```Điều này khẳng định rằng sau khi loại bỏ bài hát đứng đầu, bài hát phổ biến thứ hai đã được chọn chính xác. 

### Ví dụ 2 

đầu vào:```
4
A 5
B 2
C 10
D 7
0
```Thứ tự sắp xếp:```
(10, C), (7, D), (5, A), (2, B)
```| Bước | Hành động | Tiểu bang | 
| --- | --- | --- | 
| Đọc đầu vào | lưu trữ bài hát | [(5,A),(2,B),(10,C),(7,D)] | 
| Sắp xếp | giảm dần | [(10,C),(7,D),(5,A),(2,B)] | 
| k = 0 | chọn chỉ số 0 | (10, C) | 

Đầu ra:```
C
```Điều này xác nhận trường hợp đặc biệt trong đó không có bài hát nào bị xóa: chúng tôi trực tiếp trả về mức tối đa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | bị chi phối bởi việc sắp xếp n bài hát theo mức độ phổ biến | 
| Không gian | O(n) | lưu trữ tất cả các cặp bài hát | 

Các ràng buộc cho phép tối đa 100.000 bài hát và việc sắp xếp ở quy mô này cũng nằm trong giới hạn trong Python. Việc sử dụng bộ nhớ là tuyến tính theo số lượng cặp được lưu trữ và thoải mái dưới 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    n = int(input())
    songs = []
    for _ in range(n):
        w, s = input().split()
        songs.append((int(w), s))
    k = int(input())
    
    songs.sort(reverse=True)
    return songs[k][1]

# provided sample style cases
assert run("""3
1 flos
3 Yellow
9 Starduster
1
""") == "Yellow"

assert run("""1
1000000000 Kawakiwoameku
0
""") == "Kawakiwoameku"

# custom cases
assert run("""2
1 A
2 B
0
""") == "B"

assert run("""2
1 A
2 B
1
""") == "A"

assert run("""5
10 A
20 B
30 C
40 D
50 E
4
""") == "A"

assert run("""3
5 X
1 Y
3 Z
2
""") == "Y"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 bài hát, k=0 | bài hát cao nhất | độ chính xác lựa chọn tối đa | 
| 2 bài hát, k=1 | bài hát thấp nhất | tính chính xác của việc lập chỉ mục ranh giới | 
| 5 giá trị ngày càng tăng | phần tử cuối cùng | đặt hàng đầy đủ đúng đắn | 
| giá trị xáo trộn, k=2 | phần tử ở giữa | xếp hạng chung đúng đắn | 

## Vỏ cạnh 

Khi k bằng 0, thuật toán trả về trực tiếp phần tử đầu tiên sau khi sắp xếp. Ví dụ: với đầu vào:```
3
5 A
2 B
9 C
0
```Sau khi sắp xếp, ta được (9, C), (5, A), (2, B). Chỉ số 0 là C, điều này đúng vì không có bài hát nào được coi là đã được chọn. 

Khi k bằng n − 1, chỉ còn lại phần tử nhỏ nhất. Đối với đầu vào:```
3
5 A
2 B
9 C
2
```Danh sách được sắp xếp là (9, C), (5, A), (2, B). Chỉ số 2 là (2,B), là lựa chọn cuối cùng còn lại sau khi loại bỏ 2 bài hát đứng đầu. 

Bởi vì thuật toán không bao giờ sửa đổi danh sách sau khi sắp xếp nên các trường hợp đặc biệt này không yêu cầu xử lý đặc biệt. Quy tắc lập chỉ mục tương tự được áp dụng nhất quán trên tất cả các giá trị hợp lệ của k.
