---
title: "CF 137A - Bưu thiếp và ảnh"
description: "Chúng ta được cho một chuỗi gồm hai ký tự C và P. Mỗi ký tự đại diện cho một vật thể treo trên tường. C có nghĩa là bưu thiếp, P có nghĩa là ảnh. Polycarpus loại bỏ các đối tượng từ trái sang phải. Anh ta không thể bỏ qua các vị trí và tại bất kỳ thời điểm nào anh ta chỉ có thể mang theo một loại đồ vật."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 137
codeforces_index: "A"
codeforces_contest_name: "Codeforces Beta Round 98 (Div. 2)"
rating: 900
weight: 137
solve_time_s: 88
verified: true
draft: false
---

[CF 137A - Bưu thiếp và ảnh](https://codeforces.com/problemset/problem/137/A) 

**Xếp hạng:** 900 
**Thẻ:** triển khai 
**Thời gian giải:** 1 phút 28s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một chuỗi gồm hai ký tự,`C`Và`P`. Mỗi ký tự đại diện cho một đồ vật treo trên tường.`C`có nghĩa là bưu thiếp,`P`có nghĩa là ảnh. 

Polycarpus loại bỏ các đối tượng từ trái sang phải. Anh ta không thể bỏ qua các vị trí và tại bất kỳ thời điểm nào anh ta chỉ có thể mang theo một loại đồ vật. Anh ta cũng không thể mang nhiều hơn 5 đồ vật cùng một lúc. 

Nhiệm vụ là tính toán số lần đi đến tủ tối thiểu cần thiết để loại bỏ mọi đồ vật. 

Chi tiết quan trọng là một khi loại đối tượng thay đổi, anh ta phải làm trống tay trước khi tiếp tục. Ví dụ, trong trình tự`CCCPP`, anh ấy có thể mang ba tấm bưu thiếp cùng nhau, nhưng khi bức ảnh đầu tiên xuất hiện, trước tiên anh ấy phải vào tủ. 

Độ dài chuỗi tối đa là 100, rất nhỏ. Ngay cả giải pháp bậc hai cũng dễ dàng vượt qua. Tuy nhiên, cấu trúc của bài toán cho phép quét tuyến tính trực tiếp. 

Một sai lầm phổ biến là suy nghĩ toàn cầu thay vì cục bộ. Vì Polycarpus đi hoàn toàn từ trái sang phải và không thể bỏ qua các mục nên mỗi khối liền kề tối đa gồm các ký tự bằng nhau sẽ độc lập với các khối khác. 

Hãy xem xét ví dụ này:```
CCCCCC
```Câu trả lời đúng là`2`, không`6`. Anh ta có thể mang theo tối đa 5 món đồ nên khối 6 tấm bưu thiếp cần phải đi hai chuyến. 

Một sai lầm dễ mắc phải khác là quên phân chia theo thay đổi loại.```
CPPPP
```Câu trả lời đúng là`2`. Một chuyến đi cho một tấm bưu thiếp, một chuyến đi cho bốn bức ảnh. Xử lý toàn bộ chiều dài chuỗi như một lô sẽ cho kết quả không chính xác`1`. 

Ngoài ra còn có một cạm bẫy khi kích thước khối chia hết cho 5.```
CCCCCCCCCC
```Câu trả lời đúng là`2`, không`3`. Mười mục hoàn toàn phù hợp với hai chuyến đi cỡ 5. Sử dụng công thức như`len // 5 + 1`không kiểm tra khả năng chia hết sẽ thất bại ở đây. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là mô phỏng quá trình theo đúng nghĩa đen. Chúng tôi đi qua sợi dây trong khi theo dõi loại mang theo hiện tại và số lượng đồ vật hiện có trong tay. Bất cứ khi nào loại thay đổi hoặc trên tay đạt tới 5 món đồ, chúng ta sẽ đi đến tủ và tiếp tục. 

Mô phỏng này đã đủ nhanh vì chuỗi cực kỳ ngắn. Tổng công việc tỷ lệ thuận với chiều dài chuỗi. 

Có một cách thậm chí còn rõ ràng hơn để nghĩ về cùng một quá trình. 

Bức tường tự nhiên chia thành các nhóm liền kề tối đa có các ký tự bằng nhau. Ví dụ:```
CCCPPCCCC
```trở thành:```
CCC | PP | CCCC
```Mỗi nhóm hoàn toàn độc lập vì Polycarpus không thể trộn lẫn các loại trong tay mình. Bên trong một nhóm kích thước`k`, số chuyến đi tối thiểu chỉ đơn giản là:```
ceil(k / 5)
```vì mỗi chuyến đi có thể chở tối đa 5 đồ vật. 

Toàn bộ vấn đề giảm xuống còn việc quét chuỗi, tìm độ dài của các khối ký tự bằng nhau liên tiếp và tính tổng các chuyến đi cần thiết của chúng. 

Mô phỏng lực lượng vũ phu hoạt động vì nó mô hình hóa trực tiếp quá trình thực, nhưng việc quan sát nhóm biến vấn đề thành một nhiệm vụ đếm đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n) | O(1) | Đã chấp nhận | 
| Đếm nhóm tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc chuỗi đầu vào. 
2. Khởi tạo biến trả lời`ans = 0`. 
3. Quét chuỗi từ trái sang phải đồng thời nhóm các ký tự bằng nhau liên tiếp lại với nhau. 
4. Với mỗi nhóm, hãy đếm độ dài của nó`cnt`. 
5. Tính toán nhóm này cần bao nhiêu chuyến đi bằng cách sử dụng:```
(cnt + 4) // 5
```Đây là số học số nguyên cho`ceil(cnt / 5)`. 

1. Thêm giá trị này vào câu trả lời cuối cùng. 
2. Tiếp tục cho đến khi toàn bộ chuỗi được xử lý. 
3. In câu trả lời. 

Tại sao nó hoạt động: 

Mỗi chuyến đi có thể chỉ chứa một loại đối tượng, do đó các đối tượng từ các nhóm khác nhau không bao giờ có thể được kết hợp thành một chuyến đi. Trong một nhóm, việc mang theo càng nhiều đồ vật càng tốt trong mỗi chuyến đi luôn là điều tối ưu vì mọi chuyến đi đều có chi phí như nhau. Việc tự nguyện mang theo ít hơn 5 món đồ sẽ chỉ làm tăng số chuyến đi. Vì mỗi nhóm được xử lý một cách tối ưu và độc lập nên tổng số tiền là tối ưu toàn cục. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

s = input().strip()

n = len(s)
ans = 0
i = 0

while i < n:
    j = i

    while j < n and s[j] == s[i]:
        j += 1

    cnt = j - i
    ans += (cnt + 4) // 5

    i = j

print(ans)
```Giải pháp sử dụng hai con trỏ,`i`Và`j`, để xác định các khối liền kề có ký tự bằng nhau.`i`đánh dấu sự bắt đầu của nhóm hiện tại.`j`di chuyển về phía trước cho đến khi nhân vật thay đổi. Độ dài nhóm là`j - i`. 

biểu thức`(cnt + 4) // 5`là một cách tiêu chuẩn chỉ dùng số nguyên để tính mức chia trần cho 5. Nó xử lý chính xác tất cả các trường hợp, kể cả khi kích thước nhóm chia hết chính xác cho 5. 

Đang cập nhật`i = j`chuyển trực tiếp tới nhóm chưa được xử lý tiếp theo. 

Việc triển khai sẽ tránh được các mảng không cần thiết hoặc bộ nhớ thừa vì các nhóm được xử lý nhanh chóng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
CPCPCPC
```| Bước | Nhóm | Chiều dài | Chuyến đi cho nhóm | Tổng số câu trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | C | 1 | 1 | 1 | 
| 2 | P | 1 | 1 | 2 | 
| 3 | C | 1 | 1 | 3 | 
| 4 | P | 1 | 1 | 4 | 
| 5 | C | 1 | 1 | 5 | 
| 6 | P | 1 | 1 | 6 | 
| 7 | C | 1 | 1 | 7 | 

Đầu ra cuối cùng:```
7
```Ví dụ này cho thấy sự phân mảnh tồi tệ nhất có thể xảy ra. Vì loại thay đổi sau mỗi đối tượng nên không có gì có thể được nhóm lại với nhau. 

### Ví dụ 2 

đầu vào:```
CCCCCCPPPPPP
```| Bước | Nhóm | Chiều dài | Chuyến đi cho nhóm | Tổng số câu trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | CCCCCC | 6 | 2 | 2 | 
| 2 | PPPPPP | 6 | 2 | 4 | 

Đầu ra cuối cùng:```
4
```Điều này chứng tỏ các nhóm lớn được chia thành các lô có tối đa 5 mặt hàng như thế nào. Mỗi khối 6 cần chính xác hai chuyến đi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ký tự được xử lý một lần | 
| Không gian | O(1) | Chỉ có một vài biến được sử dụng | 

Kích thước đầu vào rất nhỏ nên giải pháp dễ dàng nằm gọn trong giới hạn. Ngay cả với độ dài tối đa là 100, thuật toán chỉ thực hiện một lần quét tuyến tính duy nhất. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    s = input().strip()

    n = len(s)
    ans = 0
    i = 0

    while i < n:
        j = i

        while j < n and s[j] == s[i]:
            j += 1

        cnt = j - i
        ans += (cnt + 4) // 5

        i = j

    print(ans)

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    out = sys.stdout.getvalue()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out

# provided sample
assert run("CPCPCPC\n") == "7\n", "sample 1"

# custom cases
assert run("C\n") == "1\n", "single item"
assert run("CCCCCCCCCC\n") == "2\n", "exact multiple of 5"
assert run("PPPPPP\n") == "2\n", "single group larger than 5"
assert run("CCCPPCCCC\n") == "3\n", "multiple groups"
assert run(("C" * 100) + "\n") == "20\n", "maximum length all same"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`C`|`1`| Đầu vào kích thước tối thiểu | 
|`CCCCCCCCCC`|`2`| Chia hết chính xác cho 5 | 
|`PPPPPP`|`2`| Thêm một chuyến đi sau khi hết công suất | 
|`CCCPPCCCC`|`3`| Phân tách chính xác giữa các nhóm | 
|`C`lặp lại 100 lần |`20`| Đầu vào kích thước tối đa | 

## Vỏ cạnh 

Hãy xem xét đầu vào:```
CCCCCCCCCC
```Thuật toán tìm thấy một nhóm có độ dài 10.```
(10 + 4) // 5 = 14 // 5 = 2
```Vậy câu trả lời là`2`. Điều này xử lý chính xác trường hợp chia hết chính xác. 

Bây giờ hãy xem xét:```
CPPPP
```Các nhóm là:```
C
PPPP
```Đóng góp đầu tiên`1`chuyến đi, đóng góp thứ hai`1`chuyến đi, tổng cộng là`2`. 

Thuật toán không bao giờ cố gắng kết hợp các đối tượng theo một thay đổi về loại, khớp chính xác với các quy tắc. 

Cuối cùng, hãy xem xét trường hợp xen kẽ cao:```
PCPCPC
```Mỗi nhóm có kích thước`1`, vì vậy mọi đối tượng đều cần hành trình riêng của mình. 

Thuật toán xử lý sáu nhóm riêng biệt:```
1 + 1 + 1 + 1 + 1 + 1 = 6
```Điều này xác nhận logic nhóm cũng xử lý phân mảnh tối đa có thể một cách chính xác.
