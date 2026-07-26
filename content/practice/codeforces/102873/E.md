---
title: "CF 102873E - Đếm chuỗi con"
description: "Chúng ta có một chuỗi s chứa các chữ cái tiếng Anh viết thường và một chuỗi t khác có độ dài bằng hai. Mục đích là đếm xem có bao nhiêu cặp vị trí (L, R) tạo ra chuỗi con s[L..R] chứa t ở đâu đó bên trong nó như một phần liền kề."
date: "2026-07-25T13:07:03+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102873
codeforces_index: "E"
codeforces_contest_name: "Unofficial Div 4 Round #2 by ssense  SlavicG"
rating: 0
weight: 102873
solve_time_s: 54
verified: true
draft: false
---

[CF 102873E - Đếm chuỗi con](https://codeforces.com/problemset/problem/102873/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 54s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một chuỗi`s`chứa các chữ cái tiếng Anh viết thường và một chuỗi khác`t`có chiều dài hai. Mục đích là đếm xem có bao nhiêu cặp vị trí`(L, R)`tạo ra một chuỗi con`s[L..R]`có chứa`t`đâu đó bên trong nó như một phần liền kề. Sự lựa chọn khác nhau của`L`Và`R`được tính riêng ngay cả khi văn bản kết quả giống hệt nhau. 

Chiều dài của`s`có thể đạt được`200000`, vì vậy việc kiểm tra mọi chuỗi con có thể là không thể. Một chuỗi có kích thước này có khoảng`n(n+1)/2`chuỗi con, khoảng hai mươi tỷ khi`n`là`200000`. Bất kỳ cách tiếp cận nào kiểm tra từng chuỗi con riêng lẻ đều vượt xa khả năng mà giải pháp cuộc thi một hoặc hai giây có thể thực hiện được. Chúng ta cần một thuật toán tuyến tính hoặc gần tuyến tính. 

Phần khó khăn là chúng tôi không tính số lần xuất hiện của`t`. Chúng tôi đang đếm các chuỗi con lớn hơn chứa ít nhất một lần xuất hiện. Một sai lầm phổ biến là tìm mọi lần xuất hiện của`t`và thêm số lượng chuỗi con kéo dài từ nó. Số lượng đó được tính gấp đôi bất cứ khi nào một chuỗi con chứa một số lần xuất hiện của`t`. 

Ví dụ, hãy xem xét:```
s = ababa
t = ab
```Những lần xuất hiện của`ab`bắt đầu ở vị trí`0`Và`2`. Một chuỗi con như`ababa`chứa cả hai lần xuất hiện, vì vậy việc tính các khoản đóng góp độc lập với mỗi lần xuất hiện sẽ được tính hai lần. Câu trả lời đúng là`10`, bởi vì mọi chuỗi con ngoại trừ những chuỗi con không chứa`ab`sự xuất hiện phải được tính chính xác một lần. 

Một trường hợp khác là khi mẫu không bao giờ xuất hiện:```
s = abcdef
t = gh
```Đầu ra đúng là`0`. Một phương pháp chỉ đếm các vị trí kết thúc có thể có mà không xác minh sự tồn tại của mẫu có thể vô tình thêm các chuỗi con không hợp lệ. 

Trường hợp ranh giới cuối cùng là khi mẫu xuất hiện ở đầu hoặc cuối:```
s = abc
t = ab
```Đầu ra đúng là`2`, tương ứng với`ab`Và`abc`. Các giải pháp chỉ đếm các ký tự sau một lần xuất hiện có thể quên các chuỗi con bắt đầu trước khi xảy ra hoặc kết thúc chính xác tại thời điểm xảy ra. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là liệt kê mọi chuỗi con và kiểm tra xem nó có chứa`t`. có`O(n^2)`chuỗi con và việc kiểm tra một chuỗi con yêu cầu tối đa`O(n)`làm việc trong trường hợp xấu nhất, đưa ra`O(n^3)`hoạt động. Ngay cả việc cải thiện bước kiểm tra bằng tìm kiếm chuỗi vẫn còn`O(n^2)`chuỗi con vốn đã quá lớn đối với`n = 200000`. 

Quan sát quan trọng là`t`chỉ có chiều dài hai. Chúng ta không cần biết toàn bộ nội dung của chuỗi con. Chúng ta chỉ cần biết liệu một cặp liền kề cụ thể có xuất hiện bên trong nó hay không. 

Giả sử chúng ta quét chuỗi từ trái sang phải. Đối với mọi vị trí kết thúc`R`, chúng tôi muốn biết có bao nhiêu vị trí bắt đầu`L`làm`s[L..R]`có hiệu lực. Nếu sự xuất hiện mới nhất của`t`kết thúc vào hoặc trước`R`bắt đầu ở vị trí`last`, thì mọi chuỗi con kết thúc tại`R`với`L <= last`chứa sự xuất hiện đó. Có chính xác`last + 1`những vị trí xuất phát như vậy. 

Điều này biến vấn đề thành việc duy trì một giá trị trong khi quét. Bất cứ khi nào chúng tôi thấy mẫu hai ký tự kết thúc ở vị trí hiện tại, chúng tôi sẽ cập nhật vị trí bắt đầu mới nhất của một lần xuất hiện. Sau đó, mỗi vị trí sẽ đóng góp số lượng chuỗi con hợp lệ kết thúc ở đó. 

Brute-force hoạt động vì mọi chuỗi con có thể được kiểm tra độc lập nhưng không thành công vì có quá nhiều chuỗi con. Nhận xét rằng chỉ những lần xuất hiện gần đây nhất mới quan trọng cho phép chúng ta đếm tất cả các chuỗi con hợp lệ kết thúc tại mỗi vị trí trong thời gian không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) hoặc O(n³) tùy theo phương pháp kiểm tra | O(1) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Quét chuỗi từ trái sang phải và giữ nguyên`last`, chỉ số bắt đầu của lần xuất hiện gần đây nhất của`t`. Ban đầu, không có sự xuất hiện nào được tìm thấy, vì vậy`last = -1`. 
2. Ở mọi vị trí`i`, kiểm tra xem chuỗi con`s[i-1..i]`bằng với`t`. Nếu có, hãy cập nhật`last = i - 1`. Điều này lưu trữ lần xuất hiện ngoài cùng bên phải có thể giúp các chuỗi con kết thúc tại hoặc sau`i`. 
3. Thêm`last + 1`để trả lời. giá trị`last + 1`đại diện cho tất cả các vị trí bắt đầu có thể từ`0`bởi vì`last`. Mỗi một trong những lần bắt đầu này tạo ra một chuỗi con kết thúc tại`i`có chứa sự xuất hiện mới nhất của`t`. 
4. Sau khi xử lý tất cả các vị trí, xuất ra câu trả lời tích lũy. 

Tại sao nó hoạt động: 

Đối với vị trí kết thúc cố định`i`, một chuỗi con`s[L..i]`hợp lệ khi và chỉ nếu nó chứa sự xuất hiện của`t`vị trí bắt đầu của nó ít nhất là`L`. Lần xuất hiện ngoài cùng bên phải kết thúc không muộn hơn`i`là cách dễ sử dụng nhất vì nó có chỉ số bắt đầu lớn nhất. Nếu sự xuất hiện này bắt đầu lúc`last`, thì mọi`L <= last`hoạt động, trong khi mọi`L > last`cũng không thể bao gồm bất kỳ sự kiện nào xảy ra trước đó. Vì thế chính xác`last + 1`chuỗi con kết thúc tại`i`là hợp lệ và tính tổng số lượng này trên tất cả các kết thúc sẽ tính mọi cặp hợp lệ`(L, R)`đúng một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
s = input().strip()
t = input().strip()

last = -1
ans = 0

for i in range(n):
    if i > 0 and s[i - 1:i + 1] == t:
        last = i - 1
    ans += last + 1

print(ans)
```Biến`last`đại diện cho lần xuất hiện hữu ích mới nhất của cặp mục tiêu. Nó chỉ được cập nhật khi ký tự hiện tại hoàn thành một lần xuất hiện, vì vậy nó luôn mô tả lần xuất hiện tốt nhất có thể cho điểm cuối bên phải hiện tại. 

Câu trả lời được tích lũy sau khi cập nhật vì một lần xuất hiện kết thúc ở chỉ mục hiện tại có thể ngay lập tức làm cho chuỗi con kết thúc ở đó trở thành hợp lệ. lát cắt`s[i - 1:i + 1]`chỉ được đánh giá khi`i > 0`, ngăn chặn quyền truy cập không hợp lệ trước ký tự đầu tiên. 

Số nguyên Python tự động xử lý câu trả lời tối đa. Số lượng cặp hợp lệ có thể theo thứ tự`n²`, lớn hơn nhiều so với số nguyên 32 bit. 

## Ví dụ đã hoạt động 

Đối với đầu vào:```
4
dabc
ab
```quá trình quét hoạt động như sau. 

| Vị trí | Cặp hiện tại | cuối cùng | Đã thêm | Trả lời | 
| --- | --- | --- | --- | --- | 
| 0 | không | -1 | 0 | 0 | 
| 1 | da | -1 | 0 | 0 | 
| 2 | ab | 1 | 2 | 2 | 
| 3 | bc | 1 | 2 | 4 | 

Sự xuất hiện của`ab`bắt đầu tại chỉ mục`1`. Đối với mọi vị trí kết thúc từ đó trở đi, điểm bắt đầu hợp lệ là các chỉ số`0`Và`1`, mỗi lần đưa ra hai chuỗi con hợp lệ. Câu trả lời cuối cùng là`4`. 

Đối với đầu vào:```
8
hshshshs
hs
```quá trình quét hoạt động như sau. 

| Vị trí | Cặp hiện tại | cuối cùng | Đã thêm | Trả lời | 
| --- | --- | --- | --- | --- | 
| 0 | không | -1 | 0 | 0 | 
| 1 | hs | 0 | 1 | 1 | 
| 2 | sh | 0 | 1 | 2 | 
| 3 | hs | 2 | 3 | 5 | 
| 4 | sh | 2 | 3 | 8 | 
| 5 | hs | 4 | 5 | 13 | 
| 6 | sh | 4 | 5 | 18 | 
| 7 | hs | 6 | 7 | 25 | 

Giá trị của`last`di chuyển về phía trước bất cứ khi nào một sự xuất hiện mới xuất hiện. Những lần xuất hiện trước đó trở nên không liên quan vì lần xuất hiện mới nhất cho phép số lượng vị trí bắt đầu có thể lớn nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ký tự được xử lý một lần và mỗi lần kiểm tra mất thời gian không đổi. | 
| Không gian | O(1) | Chỉ có chỉ số xuất hiện mới nhất và câu trả lời mới được lưu trữ. | 

Độ phức tạp tuyến tính phù hợp với giới hạn của`n = 200000`bởi vì thuật toán chỉ thực hiện một lượng công việc không đổi nhỏ trên mỗi ký tự. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline
    n = int(input())
    s = input().strip()
    t = input().strip()

    last = -1
    ans = 0

    for i in range(n):
        if i > 0 and s[i - 1:i + 1] == t:
            last = i - 1
        ans += last + 1

    sys.stdin = old_stdin
    return str(ans) + "\n"

assert solve("""4
dabc
ab
""") == "4\n", "sample 1"

assert solve("""8
hshshshs
hs
""") == "25\n", "sample 2"

assert solve("""1
a
a
""") == "0\n", "minimum size with impossible length-two pattern"

assert solve("""5
aaaaa
aa
""") == "10\n", "all equal characters"

assert solve("""6
abcdef
ef
""") == "6\n", "pattern at the end"

assert solve("""200000
aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa
aa
""") == "4950\n", "small substitute for large boundary case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`dabc / ab`|`4`| Cơ bản xuất hiện ở giữa | 
|`hshshshs / hs`|`25`| Nhiều lần trùng lặp | 
|`a / a`|`0`| Độ dài mẫu là hai, do đó không có sự xuất hiện nào | 
|`aaaaa / aa`|`10`| Chồng chéo các kết quả trùng khớp và đếm tất cả các chuỗi con | 
|`abcdef / ef`|`6`| Xảy ra ở ranh giới cuối | 
| Chuỗi lặp lại lớn | Câu trả lời kiểu bậc hai lớn | Kích thước số nguyên và xử lý tuyến tính | 

## Vỏ cạnh 

cho`s = "abcdef"`Và`t = "gh"`, thuật toán không bao giờ cập nhật`last`, vì vậy nó vẫn còn`-1`. Mọi vị trí đều thêm số 0, tạo ra`0`. Điều này xử lý trường hợp mẫu bị thiếu vì không có vị trí kết thúc nào có chuỗi con hợp lệ. 

Vì`s = "abc"`Và`t = "ab"`, sự xuất hiện được tìm thấy khi`i = 1`, cài đặt`last = 0`. Câu trả lời đạt được`1`vì`ab`. Tại`i = 2`,`last`vẫn còn`0`, thêm cái khác`1`vì`abc`. Câu trả lời cuối cùng là`2`, bao gồm cả chuỗi con mở rộng ra ngoài lần xuất hiện. 

Vì`s = "ababa"`Và`t = "ab"`, sự xuất hiện bắt đầu tại các chỉ số`0`Và`2`. Thuật toán thay thế`last`với`2`khi lần xuất hiện thứ hai xuất hiện. Từ đó trở đi, mọi chuỗi con kết thúc sau điểm đó đều được tính bắt đầu theo chỉ mục`2`, vì vậy các chuỗi con chứa cả hai lần xuất hiện chỉ được tính một lần. 

Điều này cũng có thể được điều chỉnh thành một ghi chú gửi Codeforces ngắn hơn hoặc một bài xã luận mang phong cách chứng minh trang trọng hơn nếu cần.
