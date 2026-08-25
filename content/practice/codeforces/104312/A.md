---
title: "CF 104312A - Đấu võ đường"
description: "Chúng tôi được cung cấp một danh sách học sinh, trong đó mỗi học sinh có một tên và bốn thuộc tính số: kỹ năng đá, kỹ năng phép thuật, tốc độ và kỹ năng diệt quỷ. Nhiệm vụ là tạo ra thứ hạng của tất cả học sinh dựa trên hệ thống ưu tiên đa cấp độ nghiêm ngặt."
date: "2026-07-01T19:51:16+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104312
codeforces_index: "A"
codeforces_contest_name: "UTPC Spring 2023 Contest (HS)"
rating: 0
weight: 104312
solve_time_s: 59
verified: true
draft: false
---

[CF 104312A - Trận đấu võ đường](https://codeforces.com/problemset/problem/104312/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 59s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một danh sách học sinh, trong đó mỗi học sinh có một tên và bốn thuộc tính số: kỹ năng đá, kỹ năng phép thuật, tốc độ và kỹ năng diệt quỷ. Nhiệm vụ là tạo ra thứ hạng của tất cả học sinh dựa trên hệ thống ưu tiên đa cấp độ nghiêm ngặt. 

Thứ tự chủ yếu được xác định bởi kỹ năng diệt quỷ, trong đó giá trị cao hơn sẽ được ưu tiên. Nếu hai học sinh có cùng kỹ năng tiêu diệt quỷ, chúng tôi sẽ so sánh kỹ năng phép thuật của họ và một lần nữa ưu tiên giá trị cao hơn. Nếu cũng bằng nhau thì chúng ta so sánh kỹ năng đá theo thứ tự giảm dần. Nếu cả ba thuộc tính liên quan đến chiến đấu đều giống hệt nhau, chúng tôi sẽ so sánh tốc độ, vẫn theo thứ tự giảm dần. Chỉ khi tất cả các thuộc tính số giống hệt nhau thì chúng ta mới quay lại sắp xếp tên, nhưng trong trường hợp này là tăng dần theo từ điển. 

Vì vậy, đầu ra chỉ đơn giản là tên của tất cả học sinh được in theo thứ tự sắp xếp được xác định bởi chuỗi so sánh này. 

Kích thước đầu vào nhỏ, tối đa 100 sinh viên. Điều này ngay lập tức gợi ý rằng mọi cách tiếp cận lên đến khoảng O(N log N) hoặc thậm chí O(N^2) sẽ trôi qua một cách thoải mái. Việc sắp xếp thống trị tất cả các giải pháp hợp lý ở đây và không cần cấu trúc dữ liệu nâng cao hoặc xây dựng tham lam. 

Một trường hợp cạnh tranh tinh tế phát sinh từ sự ràng buộc. Bởi vì nhiều trường được so sánh theo thứ tự ưu tiên nghiêm ngặt nên rất dễ vô tình sắp xếp sai hướng cho một trường hoặc áp dụng thứ tự từ điển quá sớm. Ví dụ, hãy xem xét:```
2
a 10 10 10 10
b 10 10 10 10
```Đầu ra đúng phải là:```
a
b
```Việc triển khai bất cẩn có thể quên tên đó chỉ được sử dụng khi tất cả các giá trị số khớp nhau và thay vào đó đưa nó vào khóa sắp xếp sớm hơn, tạo ra thứ tự không chính xác ngay cả khi các kỹ năng hơi khác nhau. 

Một trường hợp cạnh khác là trộn lẫn các tiêu chí tăng dần và giảm dần. Vì hầu hết các trường đều giảm dần nhưng tên lại tăng dần nên cách tiếp cận ngây thơ “sắp xếp mọi thứ giảm dần” hoặc “đảo ngược ở cuối” sẽ phá vỡ tính đúng đắn. 

## Phương pháp tiếp cận 

Ý tưởng mạnh mẽ là quét liên tục danh sách và chọn học sinh giỏi nhất còn lại theo các quy tắc so sánh, thêm chúng vào kết quả và loại bỏ chúng khỏi nhóm. Mỗi lựa chọn yêu cầu quét tất cả các phần tử còn lại, tốn O(N) cho mỗi vị trí đầu ra, dẫn đến tổng công việc là O(N^2). Với N lên tới 100, điều này vẫn tầm thường về mặt tuyệt đối, nhưng về mặt khái niệm thì không cần thiết. 

Quan sát rõ ràng hơn là toàn bộ thứ hạng được xác định theo thứ tự từ điển trên một bộ gồm năm giá trị. Khi chúng tôi chuyển đổi mỗi học sinh thành một khóa có thể so sánh được, bài toán sẽ trở thành một nhiệm vụ sắp xếp trực tiếp. 

Thông tin chi tiết quan trọng là tính năng sắp xếp của Python đã hỗ trợ sắp xếp từ điển trên các bộ dữ liệu và chúng ta có thể mã hóa thứ tự giảm dần cần thiết bằng cách phủ định các trường số. Ngoại lệ duy nhất là trường tên, vẫn tăng dần một cách tự nhiên. Vì vậy, thay vì logic so sánh tùy chỉnh, chúng tôi xây dựng một bộ dữ liệu như: 

(tiêu diệt quỷ, ma thuật, cú đá, tốc độ, tên) với sự phủ định được áp dụng cho các phần số. 

Điều này biến một bài toán xếp hạng đa quy tắc thành một thao tác sắp xếp ổn định duy nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lựa chọn vũ phu | O(N2) | O(N) | Đã chấp nhận | 
| Sắp xếp Tuple | O(N log N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả hồ sơ học sinh vào một danh sách, lưu trữ tên và bốn thuộc tính. Điều này cung cấp cho chúng tôi một tập dữ liệu có cấu trúc mà chúng tôi có thể sắp xếp trực tiếp thay vì phân tích cú pháp đầu vào nhiều lần. 
2. Đối với mỗi học sinh, hãy tạo một khóa sắp xếp trong đó tất cả các thuộc tính số bị phủ định, trong khi tên vẫn được giữ nguyên. Phủ định là thứ chuyển đổi các yêu cầu về thứ tự giảm dần thành hành vi sắp xếp tăng dần. 
3. Sắp xếp danh sách sinh viên sử dụng các phím này. Thuật toán sắp xếp trước tiên sẽ so sánh sức mạnh diệt quỷ (lớn nhất trước tiên do phủ định), sau đó là phép thuật, sau đó là cú đá, sau đó là tốc độ và cuối cùng là đặt tên theo thứ tự tăng dần. 
4. Lặp lại danh sách đã sắp xếp và chỉ in các tên theo thứ tự. Điều này tách biệt logic xếp hạng khỏi định dạng đầu ra một cách rõ ràng. 

### Tại sao nó hoạt động 

Tính chính xác xuất phát từ việc biểu diễn quy tắc xếp hạng dưới dạng thứ tự từ điển trên một bộ. Việc sắp xếp từ điển đảm bảo rằng các phần tử bộ dữ liệu trước đó sẽ chiếm ưu thế so với các phần tử sau, phù hợp với chuỗi ưu tiên trong bài toán. Việc phủ định các giá trị số sẽ chuyển đổi yêu cầu giảm dần thành thứ tự tăng dần mà không làm thay đổi các so sánh tương đối. Vì mọi điều kiện tie-break đều được mã hóa rõ ràng theo thứ tự nên không còn sự so sánh mơ hồ nào, do đó thứ tự sắp xếp cuối cùng phải khớp chính xác với thứ hạng được yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    arr = []
    
    for _ in range(n):
        parts = input().split()
        name = parts[0]
        k = int(parts[1])
        m = int(parts[2])
        s = int(parts[3])
        d = int(parts[4])
        
        arr.append(( -d, -m, -k, -s, name ))
    
    arr.sort()
    
    for item in arr:
        print(item[4])

if __name__ == "__main__":
    solve()
```Giải pháp này đọc tất cả học sinh và chuyển đổi từng học sinh thành một bộ mã hóa các quy tắc xếp hạng. Thứ tự của các trường tuple rất quan trọng: tiêu diệt quỷ trước, sau đó là phép thuật, sau đó đá, sau đó là tốc độ, sau đó là tên. Phủ định đảm bảo hành vi giảm dần cho các thuộc tính số mà không cần bộ so sánh tùy chỉnh. 

Một lỗi phổ biến là quên phủ nhận một trong các thuộc tính, điều này âm thầm phá vỡ sự ràng buộc ở cấp độ đó. Một sai lầm khác là đặt tên sớm hơn trong bộ dữ liệu, điều này sẽ khiến thứ tự từ điển ảnh hưởng đến việc so sánh kỹ năng một cách không chính xác. 

Vòng lặp cuối cùng chỉ in tên vì cấu trúc bộ dữ liệu hoàn toàn là để sắp xếp và không được rò rỉ vào logic đầu ra. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
ryomen 20 20 20 20
suguru 30 10 40 50
maki 10 10 90 90
nobara 70 60 50 40
```Chúng tôi tính toán các phím: 

| Tên | d | m | k | s | Chìa khóa | 
| --- | --- | --- | --- | --- | --- | 
| nhà trọ | 20 | 20 | 20 | 20 | (-20, -20, -20, -20, ryomen) | 
| suguru | 50 | 10 | 30 | 40 | (-50, -10, -30, -40, suguru) | 
| maki | 90 | 10 | 10 | 90 | (-90, -10, -10, -90, maki) | 
| nobara | 40 | 60 | 70 | 50 | (-40, -60, -70, -50, nobara) | 

Thứ tự sắp xếp theo so sánh tuple: 

| Bước | Được chọn | 
| --- | --- | 
| 1 | maki | 
| 2 | suguru | 
| 3 | nobara | 
| 4 | nhà trọ | 

Đầu ra:```
maki
suguru
nobara
ryomen
```Điều này xác nhận rằng thứ tự bộ dữ liệu ưu tiên chính xác kỹ năng tiêu diệt quỷ trước tiên, sau đó xếp tầng chính xác qua tất cả các thuộc tính khác. 

### Ví dụ 2 

đầu vào:```
3
a 10 10 10 10
b 10 10 10 10
c 10 10 9  10
```Phím: 

| Tên | Chìa khóa | 
| --- | --- | 
| một | (-10, -10, -10, -10, a) | 
| b | (-10, -10, -10, -10, b) | 
| c | (-10, -10, -9, -10, c) | 

Thứ tự sắp xếp: 

| Bước | Còn lại | Được chọn | 
| --- | --- | --- | 
| 1 | a, b, c | c | 
| 2 | một, b | một | 
| 3 | b | b | 

Đầu ra:```
c
a
b
```Điều này cho thấy rằng ngay cả một cải tiến nhỏ trong thuộc tính có mức độ ưu tiên cao hơn (đá so với các thuộc tính khác tùy theo thứ tự) sẽ ngay lập tức chiếm ưu thế so với các so sánh có mức độ ưu tiên thấp hơn và tên đó chỉ quan trọng khi tất cả các giá trị số giống hệt nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N log N) | Sắp xếp N sinh viên bằng cách sử dụng so sánh bộ chiếm ưu thế trong thời gian chạy | 
| Không gian | O(N) | Lưu trữ danh sách sinh viên và sắp xếp khóa | 

Với N nhiều nhất là 100, điều này thực sự là tức thời. Dung lượng bộ nhớ không đáng kể và thuật toán phù hợp thoải mái trong cả hai giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    arr = []
    for _ in range(n):
        parts = input().split()
        name = parts[0]
        k = int(parts[1])
        m = int(parts[2])
        s = int(parts[3])
        d = int(parts[4])
        arr.append(( -d, -m, -k, -s, name ))
    arr.sort()
    return "\n".join(x[4] for x in arr)

# provided sample
assert run("""4
ryomen 20 20 20 20
suguru 30 10 40 50
maki 10 10 90 90
nobara 70 60 50 40
""") == "maki\nsuguru\nnobara\nryomen"

# minimum size
assert run("1\na 1 1 1 1\n") == "a"

# all equal stats (lexicographic tie-break)
assert run("""3
b 10 10 10 10
a 10 10 10 10
c 10 10 10 10
""") == "a\nb\nc"

# one dominating attribute
assert run("""3
x 1 1 1 100
y 1 1 1 99
z 1 1 1 98
""") == "x\ny\nz"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| sinh viên độc thân | tên đó | tính đúng đắn của trường hợp cơ sở | 
| tất cả các số liệu thống kê như nhau | thứ tự từ điển | luật ràng buộc | 
| trường hợp thống trị giảm dần | sắp xếp theo thuộc tính hàng đầu | tính chính xác của việc sắp xếp chính | 

## Vỏ cạnh 

Khi tất cả các thuộc tính số giống hệt nhau, thuật toán dựa hoàn toàn vào trường tên. Vì tên không được sửa đổi trong khóa sắp xếp nên tính năng so sánh chuỗi mặc định của Python sẽ thực thi chính xác thứ tự từ điển tăng dần. Đối với một đầu vào như:```
2
ryu 10 10 10 10
ace 10 10 10 10
```các khóa trở nên giống hệt nhau ngoại trừ trường cuối cùng, do đó việc sắp xếp chỉ giải quyết hoàn toàn theo tên và tạo ra`ace`trước`ryu`. 

Khi chỉ có một thuộc tính khác nhau, ví dụ như kỹ năng diệt quỷ, thuộc tính đó sẽ chiếm ưu thế hơn tất cả các thuộc tính khác vì nó xuất hiện đầu tiên trong bộ dữ liệu. Ngay cả khi các thuộc tính thấp hơn sẽ có lợi cho sinh viên khác, việc sắp xếp bộ dữ liệu sẽ ngăn chặn ảnh hưởng đó, duy trì tính chính xác của hệ thống phân cấp xếp hạng.
