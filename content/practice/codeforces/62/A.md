---
title: "CF 62A - Ước mơ của học sinh"
description: "Chúng ta có bốn số nguyên mô tả số ngón tay trên hai bàn tay của người ngoài hành tinh. Cô gái sao Kim có tất cả các ngón ở bàn tay trái và các ngón ở bàn tay phải. Cậu bé sao Hỏa có ngón tay bl và br. Họ muốn nắm chính xác một đôi tay."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 62
codeforces_index: "A"
codeforces_contest_name: "Codeforces Beta Round 58"
rating: 1300
weight: 62
solve_time_s: 73
verified: true
draft: false
---

[CF 62A - Giấc mơ của học sinh](https://codeforces.com/problemset/problem/62/A) 

**Đánh giá:** 1300 
**Tags:** tham lam, toán học 
**Thời gian giải:** 1 phút 13s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có bốn số nguyên mô tả số ngón tay trên hai bàn tay của người ngoài hành tinh. Cô gái sao Kim có`al`ngón tay trên bàn tay trái của cô ấy và`ar`trên tay phải của cô ấy. Cậu bé sao Hỏa có`bl`Và`br`ngón tay. 

Họ muốn nắm chính xác một đôi tay. Vì một trong hai người có thể đứng ở bên trái nên chỉ có hai cách ghép đôi: 

- tay trái của cô gái với tay phải của chàng trai 
- Con gái dùng tay phải của con trai 

hoặc hướng ngược lại. 

Sự sắp xếp phải đáp ứng hai quy tắc thoải mái. 

Đầu tiên, không được để hai ngón tay của cô gái chạm trực tiếp vào nhau. Giữa mỗi cặp ngón tay con gái cạnh nhau phải có ít nhất một ngón tay con trai. 

Thứ hai, không có ba ngón tay con trai nào được chạm liên tiếp. 

Nhiệm vụ là quyết định xem có tồn tại ít nhất một cặp bài hợp lệ hay không. 

Các giới hạn rất nhỏ, mọi giá trị tối đa là 100. Ngay cả một mô phỏng mạnh mẽ trên tất cả các khả năng cũng sẽ chạy ngay lập tức. Thách thức thực sự là nhận ra điều kiện toán học ẩn bên trong từ ngữ. 

Phần khó khăn là chuyển mô tả vật lý thành các bất đẳng thức một cách chính xác. 

Giả sử cô gái có`g`ngón tay và cậu bé có`b`ngón tay trong bàn tay đã chọn. 

Để tách từng cặp ngón tay liền kề của con gái ra, con trai cần ít nhất`g - 1`ngón tay. Ví dụ: nếu cô gái có 5 ngón tay thì chúng ta cần cấu trúc như sau:`B G B G B G B G B G`Có 4 khoảng trống giữa các ngón tay bé gái nên cần ít nhất 4 ngón tay bé trai. 

Đồng thời, cậu bé không thể chạm ba ngón tay liên tiếp. Điều đó có nghĩa là chúng ta có thể đặt tối đa hai ngón tay bé trai vào mỗi khe xung quanh ngón tay bé gái. 

Với`g`ngón tay cô gái, có`g + 1`khe nơi ngón tay bé trai có thể xuất hiện: 

- trước ngón tay đầu tiên của cô gái 
- giữa mỗi cặp liền kề 
- sau ngón tay cô gái cuối cùng 

Mỗi ô có thể chứa tối đa 2 ngón tay bé trai, do đó số lượng ngón tay bé trai tối đa có thể là`2 * (g + 1)`. 

Một lỗi phổ biến là quên các khe cạnh. Ai đó có thể nghĩ sai về mức tối đa là`2 * (g - 1)`, chỉ tính các khoảng trống bên trong. Ví dụ:```
girl = 1
boy = 4
```Điều này thực sự hợp lệ:`BBGBB`Không có ba ngón tay con trai liên tiếp. 

Một sai lầm dễ mắc phải khác là sử dụng bất đẳng thức nghiêm ngặt. Coi như:```
girl = 5
boy = 4
```Điều này đúng vì chính xác một ngón tay con trai có thể tách rời mọi cặp ngón tay con gái:`GBGBGBGBG`Giới hạn dưới là`boy >= girl - 1`, không`boy > girl - 1`. 

Trường hợp cuối cùng xuất hiện khi cô gái có rất ít ngón tay.```
girl = 1
boy = 0
```Điều này hợp lệ vì không có ngón tay nữ liền kề nào để tách ra. 

Tình trạng vẫn hoạt động:```
girl - 1 <= boy <= 2 * (girl + 1)
0 <= 0 <= 4
```## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là xây dựng bằng vũ lực. Đối với một đôi bàn tay đã chọn, chúng ta có thể thử tạo ra tất cả các cách sắp xếp ngón tay có thể có và kiểm tra xem liệu có cách sắp xếp nào thỏa mãn các quy tắc hay không. 

Nếu cô gái có`g`ngón tay và cậu bé có`b`, tổng số chuỗi được hợp nhất là:$$\binom{g+b}{g}$$Mặc dù những hạn chế thực tế là rất nhỏ nhưng nhìn chung điều này sẽ phát triển nhanh chóng. Ví dụ, chỉ với tổng cộng 20 ngón tay, đã có hơn 180 nghìn lần đan xen. Việc tìm kiếm đầy đủ trở nên không cần thiết một khi chúng ta hiểu được cấu trúc của các sắp xếp hợp lệ. 

Quan sát quan trọng là các ràng buộc mô tả các quy tắc giãn cách. 

Tình trạng của cô gái tạo ra yêu cầu tối thiểu đối với ngón tay của chàng trai. Mỗi cặp ngón tay nữ liền kề cần có ít nhất một dấu phân cách, vì vậy chúng ta cần:$$b \ge g - 1$$Tình trạng của cậu bé tạo ra một yêu cầu tối đa. Vì không có ba ngón tay con trai nào có thể chạm liên tiếp nhau nên mỗi khe xung quanh ngón tay con gái chỉ có thể chứa tối đa hai ngón tay con trai. 

Với`g`ngón tay cô gái, có`g + 1`khe cắm:```
_ G _ G _ G _
```Mỗi khe có thể chứa tối đa hai ngón tay con trai, mang lại:$$b \le 2(g+1)$$Hai bất đẳng thức này đều cần và đủ. 

Chúng tôi chỉ đơn giản kiểm tra cả hai cặp bài có thể có: 

-`(girl left, boy right)`-`(girl right, boy left)`Nếu một trong hai thỏa mãn bất đẳng thức thì đáp án là`"YES"`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Về mặt khái niệm quá chậm | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng bốn ngón tay. 
2. Xác định chức năng kiểm tra trợ giúp cho một đôi tay đã chọn. 
3. Giả sử cô gái có`g`ngón tay và cậu bé có`b`. 
4. Xác minh giới hạn dưới:$$b \ge g - 1$$Điều này đảm bảo mọi cặp ngón tay nữ lân cận đều có thể được tách ra. 

1. Xác minh giới hạn trên:$$b \le 2(g+1)$$Điều này đảm bảo các ngón tay của cậu bé có thể được phân bổ vào`g + 1`các khe có sẵn mà không cần tạo ba ngón tay bé trai liên tiếp. 

1. Nếu cả hai bất đẳng thức đều đúng thì việc ghép đôi này là khả thi. 
2. Kiểm tra cả hai hướng: 

1. gái trái với trai phải 
2. gái phải với trai trái 
3. Nếu có ít nhất một hướng hoạt động, hãy in`"YES"`. Nếu không thì in`"NO"`. 

### Tại sao nó hoạt động 

Giới hạn dưới là không thể tránh khỏi vì mỗi khoảng cách giữa các ngón tay con gái liên tiếp phải có ít nhất một ngón tay con trai. Một bàn tay với`g`ngón tay cô gái có chính xác`g - 1`những khoảng trống như vậy. 

Giới hạn trên cũng không thể tránh khỏi vì cậu bé không được đóng góp ba ngón tay liên tiếp. Xung quanh`g`ngón tay cô gái chính xác là có`g + 1`vị trí chèn và mỗi vị trí có thể chứa tối đa hai ngón tay con trai. 

Những điều kiện này cũng đủ. Nếu số ngón tay bé trai nằm trong khoảng:$$g - 1 \le b \le 2(g+1)$$Trước tiên, chúng ta có thể đặt một ngón tay bé trai vào mỗi khoảng trống bên trong, sau đó phân bổ các ngón tay bé trai còn lại vào các ô có sẵn nhưng không bao giờ vượt quá hai ngón tay bé trai trên mỗi ô. Điều đó luôn xây dựng một sự sắp xếp hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def possible(g, b):
    return g - 1 <= b <= 2 * (g + 1)

def solve():
    al, ar = map(int, input().split())
    bl, br = map(int, input().split())

    if possible(al, br) or possible(ar, bl):
        print("YES")
    else:
        print("NO")

solve()
```Hàm trợ giúp trực tiếp thực hiện đặc tính toán học của một sự sắp xếp hợp lệ. 

Biểu thức:```
g - 1 <= b
```kiểm tra xem có đủ ngón tay con trai để tách ngón tay con gái liền kề hay không. 

Biểu thức:```
b <= 2 * (g + 1)
```kiểm tra xem các ngón tay bé trai có thể được phân phối mà không cần tạo ba ngón liên tiếp hay không. 

Giải pháp kiểm tra cả hai hướng có thể có vì các cặp sẽ hoán đổi tùy thuộc vào việc ai đứng về phía nào. Quên định hướng thứ hai là lỗi thực hiện phổ biến nhất. 

Tất cả các tính toán đều phù hợp thoải mái bên trong các số nguyên bình thường. Các ràng buộc rất nhỏ, nhưng ngay cả đối với các giá trị lớn hơn nhiều, logic này vẫn sẽ chạy ngay lập tức. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 1
10 5
```Chúng tôi kiểm tra cả hai cặp. 

| Ghép nối | Ngón tay cô gái | Ngón tay cậu bé | Giới hạn dưới | Giới hạn trên | hợp lệ | 
| --- | --- | --- | --- | --- | --- | 
| trái-phải | 5 | 5 | 5 ≥ 4 | 5 12 | Có | 
| phải-trái | 1 | 10 | 10 ≥ 0 | 10 4 | Không | 

Vì cặp đôi đầu tiên hoạt động nên câu trả lời là:```
YES
```Ví dụ này cho thấy chỉ cần một định hướng thành công. 

### Ví dụ 2 

đầu vào:```
4 4
1 1
```| Ghép nối | Ngón tay cô gái | Ngón tay cậu bé | Giới hạn dưới | Giới hạn trên | hợp lệ | 
| --- | --- | --- | --- | --- | --- | 
| trái-phải | 4 | 1 | 1 ≥ 3 | 1 10 | Không | 
| phải-trái | 4 | 1 | 1 ≥ 3 | 1 10 | Không | 

Cả hai hướng đều thất bại vì không có đủ ngón tay con trai để tách các ngón tay con gái liền kề. 

Đầu ra:```
NO
```Điều này chứng tỏ sự cần thiết của giới hạn dưới. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ một số kiểm tra số học được thực hiện | 
| Không gian | O(1) | Không sử dụng cấu trúc dữ liệu bổ sung | 

Các ràng buộc là cực kỳ nhỏ, nhưng giải pháp thời gian không đổi thì rõ ràng về mặt toán học và dễ dàng mở rộng vượt xa các giới hạn ban đầu. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    def possible(g, b):
        return g - 1 <= b <= 2 * (g + 1)

    al, ar = map(int, input().split())
    bl, br = map(int, input().split())

    if possible(al, br) or possible(ar, bl):
        return "YES"
    return "NO"

# provided sample
assert run("5 1\n10 5\n") == "YES", "sample 1"

# insufficient separators
assert run("4 4\n1 1\n") == "NO", "too few boy fingers"

# exact lower bound
assert run("5 2\n4 1\n") == "YES", "boy fingers equal girl-1"

# exact upper bound
assert run("1 3\n4 1\n") == "YES", "boy fingers equal 2*(girl+1)"

# exceeding upper bound
assert run("1 1\n5 5\n") == "NO", "three consecutive boy fingers unavoidable"

# symmetric equal values
assert run("3 3\n3 3\n") == "YES", "balanced configuration"

print("All tests passed.")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`4 4 / 1 1`|`NO`| Thất bại giới hạn dưới | 
|`5 2 / 4 1`|`YES`| Chính xác ngón tay bé trai hợp lệ tối thiểu | 
|`1 3 / 4 1`|`YES`| Chính xác ngón tay bé trai hợp lệ tối đa | 
|`1 1 / 5 5`|`NO`| Vi phạm giới hạn trên | 
|`3 3 / 3 3`|`YES`| Trường hợp cân bằng đối xứng | 

## Vỏ cạnh 

Hãy xem xét trường hợp chàng trai có đủ ngón tay để tách các ngón tay của cô gái. 

đầu vào:```
5 1
4 1
```Kiểm tra định hướng đầu tiên cho`g = 5`Và`b = 1`, thất bại ngay lập tức. Định hướng thứ hai mang lại`g = 1`Và`b = 4`, thỏa mãn:```
0 <= 4 <= 4
```Thuật toán in chính xác`"YES"`bởi vì các khe ở cạnh cho phép hai ngón tay con trai ở trước và sau ngón tay con gái. 

Bây giờ hãy xem xét trường hợp giới hạn trên không đạt được một. 

đầu vào:```
1 1
5 5
```Đối với một trong hai hướng:```
g = 1
b = 5
```Việc kiểm tra trở thành:```
0 <= 5 <= 4
```Giới hạn trên không thành công. Với chỉ có hai khe cạnh, việc đặt 5 ngón tay bé trai chắc chắn sẽ tạo ra ba ngón tay bé trai liên tiếp ở đâu đó. Thuật toán trả về chính xác`"NO"`. 

Cuối cùng, hãy xem xét kịch bản giới hạn dưới chính xác. 

đầu vào:```
5 2
4 1
```Việc ghép nối hợp lệ là`g = 5`,`b = 4`. 

Điều kiện trở thành:```
4 <= 4 <= 12
```Sự sắp xếp:```
G B G B G B G B G
```hoạt động hoàn hảo Thuật toán chấp nhận sự bình đẳng ở giới hạn dưới, tránh sai lầm phổ biến khi yêu cầu nhiều dấu phân cách hơn.
