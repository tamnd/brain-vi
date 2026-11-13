---
title: "CF 174C - Tăng phạm vi"
description: "Chúng ta bắt đầu với một mảng có độ dài n chứa đầy các số 0. Một thao tác chọn một đoạn [l, r] và thêm 1 vào mọi phần tử bên trong đoạn đó. Mảng cuối cùng được đưa ra."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "greedy"]
categories: ["algorithms"]
codeforces_contest: 174
codeforces_index: "C"
codeforces_contest_name: "VK Cup 2012 Round 3 (Unofficial Div. 2 Edition)"
rating: 1800
weight: 174
solve_time_s: 103
verified: true
draft: false
---

[CF 174C - Tăng phạm vi](https://codeforces.com/problemset/problem/174/C) 

**Đánh giá:** 1800 
**Tas:** cấu trúc dữ liệu, tham lam 
**Thời gian giải:** 1m 43s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi bắt đầu với một mảng có độ dài`n`chứa đầy số không. Một thao tác chọn một phân đoạn`[l, r]`và thêm vào`1`tới mọi phần tử bên trong phân khúc đó. 

Mảng cuối cùng được đưa ra. Chúng ta phải xây dựng lại một chuỗi các thao tác tạo ra chính xác mảng đó, đồng thời sử dụng càng ít thao tác càng tốt. 

Chi tiết quan trọng là các hoạt động có thể chồng chéo một cách tự do. Nếu một vị trí kết thúc bằng giá trị`5`, thì chính xác năm đoạn được chọn phải bao phủ vị trí đó. Các vị trí khác nhau có thể chia sẻ hoạt động nếu mức tăng yêu cầu của chúng trùng nhau. 

Các ràng buộc ngay lập tức loại trừ mọi thứ bậc hai. Chiều dài mảng đạt`10^5`, và các giá trị cũng đạt tới`10^5`. Một giải pháp quét liên tục các khoảng thời gian hoặc mô phỏng từng ô tăng dần sẽ dễ dàng thực hiện xung quanh`10^10`hoạt động trong trường hợp xấu nhất. Chúng ta cần một cái gì đó tuyến tính hoặc gần tuyến tính. 

Ngoài ra còn có một yêu cầu xây dựng. Chúng ta không chỉ được yêu cầu về số lượng tối thiểu mà còn phải xuất ra các khoảng thời gian một cách rõ ràng. Vì phát biểu đảm bảo câu trả lời tối ưu chứa nhiều nhất`10^5`hoạt động, một`O(answer)`xây dựng có thể chấp nhận được. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Hãy xem xét một mảng tăng nghiêm ngặt:```
1 2 3 4
```Câu trả lời đúng là bốn đoạn:```
[1,4]
[2,4]
[3,4]
[4,4]
```Một kẻ tham lam bất cẩn cố gắng kéo dài những khoảng thời gian cũ càng xa càng tốt mà không mở những khoảng thời gian mới sẽ thất bại, bởi vì mọi sự gia tăng từ`a[i-1]`ĐẾN`a[i]`buộc các khoảng thời gian bổ sung bắt đầu tại`i`. 

Bây giờ hãy xem xét các giá trị giảm dần:```
4 3 2 1
```Cách xây dựng tối ưu là:```
[1,1]
[1,2]
[1,3]
[1,4]
```Ở đây không cần có khoảng thời gian mới sau vị trí đầu tiên. Thay vào đó, một số khoảng thời gian chỉ đơn giản là kết thúc sớm hơn. Việc triển khai luôn giữ tất cả các khoảng thời gian tồn tại cho đến khi kết thúc sẽ vượt quá các vị trí sau đó. 

Một trường hợp tinh vi khác chứa số 0 bên trong mảng:```
2 0 2
```Số 0 ở giữa ngăn cách hoàn toàn phần bên trái và bên phải. Không có khoảng thời gian có thể vượt qua chỉ số`2`, nếu không thì vị trí đó sẽ trở nên tích cực. Giải pháp tối ưu sử dụng bốn khoảng độc lập:```
[1,1]
[1,1]
[3,3]
[3,3]
```Bất kỳ cách tiếp cận nào xử lý các khoảng có thể tái sử dụng trên các số 0 sẽ tạo ra một mảng không hợp lệ. 

## Phương pháp tiếp cận 

Quan điểm bạo lực giúp phơi bày cấu trúc của vấn đề. 

Giả sử chúng ta xử lý mảng từ trái sang phải. Tại vị trí`i`, chúng tôi biết có bao nhiêu khoảng thời gian hiện đang hoạt động bao trùm nó. Nếu ít hơn`a[i]`các khoảng thời gian đang hoạt động, chúng tôi mở các khoảng thời gian mới bắt đầu từ`i`. Nếu có quá nhiều khoảng thời gian được kích hoạt, chúng tôi sẽ đóng một số khoảng thời gian trước khi đạt được`i`. 

Một cách triển khai đơn giản sẽ theo dõi rõ ràng mọi phân đoạn đang hoạt động và liên tục tìm kiếm những khoảng thời gian cần đóng. Trong trường hợp xấu nhất, điều này trở thành bậc hai. Ví dụ: một mẫu xen kẽ như:```
1 0 1 0 1 0 ...
```buộc tạo ra và phá hủy khoảng thời gian thường xuyên. Quét tuyến tính lặp đi lặp lại trong các khoảng thời gian hoạt động sẽ quá chậm để`10^5`các phần tử. 

Quan sát quan trọng là số lượng thao tác tối thiểu hoàn toàn được xác định bằng cách tăng mảng. 

Bất cứ khi nào:```
a[i] > a[i-1]
```chúng ta phải bắt đầu chính xác:```
a[i] - a[i-1]
```khoảng thời gian mới tại vị trí`i`. 

Không có cách nào xung quanh điều này. Khoảng thời gian hiện tại chỉ đóng góp`a[i-1]`bảo hiểm tại vị trí`i-1`, vì vậy để nâng cao mức độ bao phủ hơn nữa tại`i`, các khoảng bổ sung phải bắt đầu từ đó. 

Ngược lại, nếu giá trị giảm, một số khoảng sẽ kết thúc trước vị trí hiện tại. 

Điều này biến vấn đề thành việc ghi sổ theo khoảng thời gian. Chúng tôi duy trì tất cả các khoảng thời gian hiện đang hoạt động. Khi chiều cao yêu cầu tăng lên, chúng tôi mở các khoảng mới. Khi nó giảm, chúng ta đóng đủ khoảng cách ở vị trí trước đó. 

Một ngăn xếp hoạt động hoàn hảo ở đây. Mỗi khoảng thời gian hoạt động chỉ lưu trữ chỉ số bắt đầu của nó. Khi một khoảng phải kết thúc, chúng tôi sẽ bật nó và xuất ra`[start, current_position - 1]`. 

Mỗi khoảng được đẩy một lần và xuất hiện một lần, vì vậy toàn bộ quá trình là tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo một ngăn xếp trống. Mỗi phần tử đại diện cho một khoảng hoạt động và chỉ lưu trữ vị trí bắt đầu của nó. 
2. Lặp lại mảng từ trái sang phải. 
3. Tại vị trí`i`, so sánh`a[i]`với số khoảng thời gian hoạt động, đơn giản là`len(stack)`. 
4. Nếu có quá nhiều khoảng thời gian hoạt động, hãy bật liên tục các khoảng thời gian từ ngăn xếp cho đến khi kích thước ngăn xếp trở thành`a[i]`. 

Mỗi khoảng thời gian bật lên kết thúc lúc`i - 1`, bởi vì nó không thể tiếp tục ở vị trí hiện tại. 
5. Nếu có quá ít khoảng thời gian hoạt động, hãy đẩy liên tục vị trí hiện tại`i`vào ngăn xếp cho đến khi kích thước ngăn xếp trở thành`a[i]`. 

Mỗi khoảng thời gian mới bắt đầu lúc`i`. 
6. Tiếp tục quá trình này cho tất cả các vị trí. 
7. Sau khi xử lý toàn bộ mảng, tất cả các khoảng hoạt động còn lại phải kết thúc tại vị trí`n`. Bật tất cả chúng và xuất các phân đoạn của chúng. 

### Tại sao nó hoạt động 

Bất biến rất đơn giản: 

Sau khi xử lý vị trí`i`, ngăn xếp chứa chính xác các khoảng bao phủ vị trí`i`. 

Do đó kích thước của nó chính xác là`a[i]`. 

Bất cứ khi nào giá trị mục tiêu tăng lên, vùng phủ sóng bổ sung phải xuất hiện lần đầu tiên ở vị trí hiện tại, do đó các khoảng thời gian mới là không thể tránh khỏi. Việc bắt đầu chúng muộn hơn sẽ không bao phủ được ô hiện tại, trong khi bắt đầu chúng sớm hơn sẽ làm tăng các ô trước đó một cách không chính xác. 

Bất cứ khi nào giá trị mục tiêu giảm, một số khoảng thời gian phải dừng trước vị trí hiện tại. Việc kết thúc bất kỳ tập hợp con nào của khoảng thời gian hoạt động đều hoạt động tốt như nhau, vì vậy chỉ cần sử dụng ngăn xếp là đủ. 

Vì các khoảng mới chỉ được tạo khi cần thiết về mặt toán học nên tổng số khoảng là tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    stack = []
    ans = []

    for i in range(n):
        pos = i + 1

        while len(stack) > a[i]:
            start = stack.pop()
            ans.append((start, pos - 1))

        while len(stack) < a[i]:
            stack.append(pos)

    while stack:
        start = stack.pop()
        ans.append((start, n))

    print(len(ans))
    print("\n".join(f"{l} {r}" for l, r in ans))

solve()
```Ngăn xếp lưu trữ các vị trí bắt đầu của tất cả các khoảng hiện bao gồm chỉ mục hiện tại. 

Khi chiều cao yêu cầu giảm xuống, các khoảng thời gian phải kết thúc trước vị trí hiện tại. Vì tất cả các khoảng thời gian hoạt động đều tương đương nhau nên việc bật bất kỳ khoảng thời gian nào cũng có tác dụng. Việc sử dụng ngăn xếp sẽ giúp chèn và xóa theo thời gian liên tục. 

Khi độ cao yêu cầu tăng lên, chúng tôi bắt đầu các khoảng thời gian mới ở vị trí hiện tại. Mỗi lần đẩy tương ứng với một thao tác mới. 

Một lỗi thường gặp xuất hiện khi kết thúc các khoảng thời gian. Nếu chúng ta hiện đang ở vị trí`i`, thì các khoảng bị loại bỏ phải kết thúc tại`i - 1`, vì chúng không nên đóng góp vào ô hiện tại. 

Một chi tiết tinh tế khác là việc dọn dẹp cuối cùng. Các khoảng thời gian còn lại hoạt động sau vòng lặp phải kéo dài đến hết`n`. 

Giải pháp không bao giờ sửa đổi mảng trực tiếp. Nó chỉ duy trì số lượng khoảng thời gian hoạt động ở mỗi vị trí, đủ để xây dựng lại câu trả lời tối ưu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
6
1 2 1 1 4 1
```| Vị trí | Giá trị mục tiêu | Hoạt động trước | Hành động | Hoạt động sau | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | bắt đầu [1,...] | 1 | 
| 2 | 2 | 1 | bắt đầu [2,...] | 2 | 
| 3 | 1 | 2 | đóng [2,2] | 1 | 
| 4 | 1 | 1 | không có gì | 1 | 
| 5 | 4 | 1 | bắt đầu 3 khoảng lúc 5 | 4 | 
| 6 | 1 | 4 | đóng ba [5,5] | 1 | 

Sau khi quá trình quét kết thúc, một khoảng thời gian vẫn hoạt động nên nó sẽ trở thành`[1,6]`. 

Khoảng thời gian cuối cùng:```
[2,2]
[5,5]
[5,5]
[5,5]
[1,6]
```Dấu vết này thể hiện sự bất biến trung tâm. Kích thước ngăn xếp luôn bằng chiều cao yêu cầu ở vị trí hiện tại. 

### Ví dụ 2 

đầu vào:```
5
3 3 1 2 0
```| Vị trí | Giá trị mục tiêu | Hoạt động trước | Hành động | Hoạt động sau | 
| --- | --- | --- | --- | --- | 
| 1 | 3 | 0 | bắt đầu 3 khoảng thời gian tại 1 | 3 | 
| 2 | 3 | 3 | không có gì | 3 | 
| 3 | 1 | 3 | đóng hai lúc 2 | 1 | 
| 4 | 2 | 1 | bắt đầu lúc 4 giờ | 2 | 
| 5 | 0 | 2 | đóng hai lúc 4 | 0 | 

Khoảng thời gian được tạo:```
[1,2]
[1,2]
[4,4]
[1,4]
```Ví dụ này cho thấy các khoảng được phân chia một cách tự nhiên khi chiều cao giảm. Số 0 ở cuối buộc mọi khoảng thời gian hoạt động phải chấm dứt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + k) | mỗi khoảng được đẩy và bật một lần | 
| Không gian | O(k) | lưu trữ ngăn xếp và trả lời | 

Đây`k`là số thao tác trong câu trả lời tối ưu. 

Tuyên bố đảm bảo`k ≤ 10^5`, do đó cả mức sử dụng bộ nhớ và thời gian chạy đều nằm trong giới hạn một cách thoải mái. Thuật toán chỉ thực hiện công việc tuyến tính và sử dụng các thao tác ngăn xếp đơn giản. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve_io(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))

    stack = []
    ans = []

    for i in range(n):
        pos = i + 1

        while len(stack) > a[i]:
            start = stack.pop()
            ans.append((start, pos - 1))

        while len(stack) < a[i]:
            stack.append(pos)

    while stack:
        start = stack.pop()
        ans.append((start, n))

    out = [str(len(ans))]
    out.extend(f"{l} {r}" for l, r in ans)

    return "\n".join(out)

# provided sample
assert solve_io(
    "6\n1 2 1 1 4 1\n"
).startswith("5")

# minimum size
assert solve_io(
    "1\n1\n"
).startswith("1")

# all equal
assert solve_io(
    "4\n3 3 3 3\n"
).startswith("3")

# strictly increasing
assert solve_io(
    "5\n1 2 3 4 5\n"
).startswith("5")

# strictly decreasing
assert solve_io(
    "5\n5 4 3 2 1\n"
).startswith("5")

# zeros separating regions
assert solve_io(
    "3\n2 0 2\n"
).startswith("4")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1`| một khoảng | mảng kích thước tối thiểu | 
|`3 3 3 3`| ba khoảng | khoảng có thể kéo dài toàn bộ mảng | 
|`1 2 3 4 5`| năm khoảng | mỗi lần tăng bắt đầu những khoảng thời gian mới | 
|`5 4 3 2 1`| năm khoảng | khoảng thời gian chấm dứt dần dần | 
|`2 0 2`| bốn khoảng | số không phân chia các vùng độc lập | 

## Vỏ cạnh 

Hãy xem xét đầu vào:```
3
2 0 2
```Tại vị trí`1`, hai khoảng thời gian bắt đầu.```
stack = [1, 1]
```Tại vị trí`2`, giá trị được yêu cầu trở thành 0. Cả hai khoảng phải chấm dứt ở vị trí`1`.```
[1,1]
[1,1]
```Tại vị trí`3`, hai khoảng hoàn toàn mới bắt đầu:```
[3,3]
[3,3]
```Thuật toán ngăn chặn chính xác các khoảng vượt qua số 0. 

Bây giờ hãy xem xét:```
4
1 2 3 4
```Kích thước ngăn xếp phát triển như sau:```
1 -> 2 -> 3 -> 4
```Mỗi lần tăng buộc phải bắt đầu đúng một khoảng thời gian mới. Không có khoảng thời gian nào kết thúc trước khi kết thúc, tạo ra:```
[1,4]
[2,4]
[3,4]
[4,4]
```Điều này xác nhận quy tắc tăng xác định số lượng hoạt động tối thiểu. 

Cuối cùng, hãy xem xét:```
5
5 4 3 2 1
```Ban đầu năm khoảng thời gian bắt đầu ở vị trí`1`. 

Khi mục tiêu giảm, các khoảng thời gian chấm dứt từng cái một:```
[1,1]
[1,2]
[1,3]
[1,4]
[1,5]
```Thuật toán xử lý việc thu hẹp phạm vi một cách chính xác bằng cách đóng các khoảng thời gian chính xác khi số lượng hoạt động trở nên quá lớn.
