---
title: "CF 102680F - Phép tính loại bỏ"
description: "Bài toán mô tả một dòng vị trí được đánh số từ 1 đến n. Một số phạm vi vị trí bị loại bỏ, nghĩa là chúng không thể chứa đối tượng bị thiếu. Sau khi loại bỏ tất cả các phạm vi nhất định, vẫn còn chính xác một vị trí. Nhiệm vụ là tìm ra vị trí còn lại đó."
date: "2026-08-01T23:34:48+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102680
codeforces_index: "F"
codeforces_contest_name: "Brookfield Computer Programming Challenge 1"
rating: 0
weight: 102680
solve_time_s: 105
verified: true
draft: false
---

[CF 102680F - Phép tính loại bỏ](https://codeforces.com/problemset/problem/102680/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 45s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Vấn đề mô tả một dòng vị trí được đánh số từ`1`ĐẾN`n`. Một số phạm vi vị trí bị loại bỏ, nghĩa là chúng không thể chứa đối tượng bị thiếu. Sau khi loại bỏ tất cả các phạm vi nhất định, vẫn còn chính xác một vị trí. Nhiệm vụ là tìm ra vị trí còn lại đó. 

Đầu vào đưa ra tổng số vị trí và số phạm vi bị loại bỏ, theo sau là điểm bắt đầu và kết thúc của mỗi khoảng bị loại bỏ. Các khoảng có thể trùng nhau nên có thể xóa cùng một vị trí nhiều lần. Đầu ra là một vị trí duy nhất không bao giờ được đưa vào bất kỳ khoảng thời gian nào. Những ràng buộc ban đầu cho phép`n`lớn như`2,000,000,010`và số khoảng thời gian tối đa là`1000`, vì vậy việc lặp qua mọi vị trí là không thể. Một vòng lặp trên tất cả các vị trí sẽ yêu cầu hàng tỷ thao tác, trong khi số lượng khoảng thời gian đủ nhỏ để thuật toán phụ thuộc chủ yếu vào các khoảng thời gian là hướng dự định. 

Mô phỏng trực tiếp cũng có vấn đề về bộ nhớ ẩn. Tạo một mảng có độ dài`n`để đánh dấu các vị trí đã loại bỏ là không thể bởi vì`n`có thể vào khoảng hai tỷ. Giải pháp chỉ phải hoạt động với các khoảng thời gian nhất định và khoảng cách giữa chúng. 

Trường hợp cạnh đầu tiên là khi câu trả lời nằm trước mỗi phạm vi bị loại bỏ. Ví dụ:```
10 1
3 10
```Đầu ra đúng là:```
1
```Cách tiếp cận bất cẩn chỉ kiểm tra khoảng cách giữa các cặp khoảng sẽ bỏ lỡ tiền tố trước khoảng đầu tiên. 

Trường hợp cạnh thứ hai là khi câu trả lời nằm sau mỗi phạm vi bị xóa:```
10 1
1 9
```Đầu ra đúng là:```
10
```Việc triển khai quên kiểm tra hậu tố sau khoảng thời gian cuối cùng sẽ không thành công ở đây. 

Trường hợp cạnh thứ ba là các khoảng chồng chéo:```
10 3
3 5
5 8
7 10
```Đầu ra đúng là:```
1
```Nếu các khoảng được xử lý độc lập mà không hợp nhất hoặc theo dõi vị trí được bao phủ xa nhất thì các phạm vi chồng chéo có thể tạo ra các khoảng trống giả không thực sự tồn tại. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Tạo một đại diện của tất cả các vị trí từ`1`ĐẾN`n`, đánh dấu mọi vị trí bên trong mỗi khoảng là đã bị xóa và quét tìm vị trí duy nhất còn lại. Điều này đúng vì nó trực tiếp tuân theo định nghĩa của câu trả lời. 

Vấn đề là giá trị của`n`. Trong trường hợp lớn nhất, chỉ riêng việc quét sẽ cần khoảng hai tỷ thao tác và việc lưu trữ mảng sẽ cần hàng tỷ mục nhập. Ngay cả trước khi xem xét cập nhật theo khoảng thời gian, bản thân biểu diễn này đã quá lớn. 

Quan sát hữu ích là số khoảng rất nhỏ so với phạm vi tọa độ. Các vị trí bị loại bỏ chỉ được mô tả bởi`u`khoảng thời gian, ở đâu`u`nhiều nhất là`1000`. Thay vì nhìn vào mọi vị trí, chúng ta có thể nhìn vào những nơi mà phạm vi phủ sóng thay đổi. 

Nếu chúng ta sắp xếp các khoảng theo vị trí bắt đầu của chúng và hợp nhất các khoảng chồng chéo thì mỗi khoảng được hợp nhất sẽ biểu thị một khối liên tục các vị trí bị loại bỏ. Đối tượng bị thiếu phải nằm trong khoảng trống giữa hai khoảng được hợp nhất hoặc trước khoảng thời gian đầu tiên hoặc sau khoảng thời gian cuối cùng. Vì đảm bảo có chính xác một vị trí không được che chắn nên chỉ cần tìm khoảng trống đó là đủ. 

Brute-force hoạt động vì mọi vị trí riêng lẻ đều được kiểm tra, nhưng không thành công vì không gian tọa độ quá lớn. Nhận xét rằng chỉ có ranh giới khoảng mới quan trọng cho phép chúng ta rút gọn bài toán từ kích thước của`n`đến số khoảng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n + tổng chiều dài được che phủ) | O(n) | Quá chậm | 
| Tối ưu | O(bạn đăng nhập bạn) | O(u) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các khoảng và sắp xếp chúng theo điểm cuối bên trái của chúng. Sắp xếp các khoảng thời gian gần nhau, giúp phát hiện sự chồng chéo chỉ bằng một lần thực hiện. 
2. Hợp nhất các khoảng đã sắp xếp. Giữ phân đoạn được bảo hiểm hiện tại. Nếu khoảng tiếp theo bắt đầu trước hoặc ở cuối phân đoạn hiện tại, hãy mở rộng phân đoạn hiện tại nếu cần. Ngược lại, có một khoảng cách giữa hai phân đoạn. 
3. Trước khi chấp nhận khoảng thời gian hợp nhất đầu tiên, hãy kiểm tra xem nó có bắt đầu sau vị trí không`1`. Nếu đúng như vậy thì mọi vị trí trước nó đều bị phát hiện và vì chỉ có một vị trí hợp lệ nên vị trí đó chính là câu trả lời. 
4. Sau khi xử lý tất cả các khoảng thời gian, hãy kiểm tra khoảng cách giữa các khoảng thời gian được hợp nhất liên tiếp. Nếu một khoảng hợp nhất kết thúc tại`a`và lần tiếp theo bắt đầu lúc`b`, thì mọi vị trí từ`a + 1`ĐẾN`b - 1`được khám phá. Vì chỉ còn lại một vị trí,`a + 1`là câu trả lời. 
5. Nếu không tìm thấy khoảng trống trước đó, khả năng còn lại là hậu tố sau khoảng thời gian hợp nhất cuối cùng. Câu trả lời là một vị trí sau khi kết thúc. 

Tại sao nó hoạt động: 

Sau khi hợp nhất, mọi vị trí đã xóa sẽ thuộc về chính xác một phân đoạn được hợp nhất và không có hai phân đoạn được hợp nhất nào trùng nhau hoặc chạm vào nhau. Điều này có nghĩa là mọi vị trí không nằm trong các phân khúc đó đều thực sự có sẵn. Thuật toán kiểm tra mọi vùng có thể tồn tại vị trí như vậy: tiền tố, mọi khoảng cách giữa các phân đoạn và hậu tố. Vì đầu vào đảm bảo chính xác một vị trí sẵn có nên vị trí chưa được khám phá đầu tiên được tìm thấy phải là câu trả lời bắt buộc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, u = map(int, input().split())
    intervals = []

    for _ in range(u):
        l, r = map(int, input().split())
        intervals.append((l, r))

    if u == 0:
        print(1)
        return

    intervals.sort()

    merged = []
    cur_l, cur_r = intervals[0]

    for l, r in intervals[1:]:
        if l <= cur_r + 1:
            if r > cur_r:
                cur_r = r
        else:
            merged.append((cur_l, cur_r))
            cur_l, cur_r = l, r

    merged.append((cur_l, cur_r))

    if merged[0][0] > 1:
        print(1)
        return

    for i in range(len(merged) - 1):
        left_end = merged[i][1]
        right_start = merged[i + 1][0]
        if right_start - left_end > 1:
            print(left_end + 1)
            return

    print(merged[-1][1] + 1)

if __name__ == "__main__":
    solve()
```Mã đầu tiên chỉ lưu trữ các khoảng thời gian, không bao giờ lưu trữ các vị trí riêng lẻ. Điều này giữ cho bộ nhớ tỷ lệ thuận với số lượng phạm vi. 

Vòng lặp hợp nhất duy trì một phân đoạn`[cur_l, cur_r]`đại diện cho tất cả các vị trí đã loại bỏ hiện được kết nối. Khi khoảng tiếp theo bắt đầu bên trong hoặc ngay sau đoạn này, việc mở rộng điểm cuối bên phải sẽ giữ cho biểu diễn được thu gọn. các`+1`TRONG`l <= cur_r + 1`là có chủ ý vì các khoảng chạm không để lại vị trí nào bị che khuất giữa chúng. 

Sau khi hợp nhất, việc kiểm tra được thực hiện theo thứ tự tọa độ. Kiểm tra tiền tố xử lý các câu trả lời trước phân đoạn bị xóa đầu tiên. Vòng lặp kiểm tra các khoảng trống bên trong. Bản in cuối cùng xử lý hậu tố sau đoạn bị xóa cuối cùng. Vì câu trả lời được đảm bảo tồn tại nên dòng cuối cùng luôn in ra vị trí hợp lệ. 

Số nguyên Python không bị tràn, do đó giá trị lớn của`n`không yêu cầu xử lý đặc biệt. Việc triển khai cũng tránh được sự đệ quy và chỉ sử dụng một danh sách nhỏ các khoảng thời gian. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
10 3
7 10
1 1
3 8
```Sau khi sắp xếp: 

| Bước | Khoảng thời gian hiện tại | Khoảng hợp nhất | Hành động | 
| --- | --- | --- | --- | 
| 1 | (1, 1) | [] | Bắt đầu đoạn đầu tiên | 
| 2 | (3, 8) | [] | Đã tạo phân đoạn riêng biệt | 
| 3 | (7, 10) | [(1,1),(3,10)] | Hợp nhất chồng chéo | 

Khoảng cách giữa`(1,1)`Và`(3,10)`là vị trí`2`, vậy đáp án là:```
2
```Dấu vết này cho thấy lý do tại sao các khoảng chồng chéo phải được hợp nhất. Các khoảng`(3,8)`Và`(7,10)`không phải là các khu vực bị loại bỏ riêng biệt. 

### Ví dụ 2 

đầu vào:```
2000000010 2
1 8
10 2000000010
```Các khoảng thời gian hợp nhất đã có: 

| Bước | Khoảng thời gian hiện tại | Khoảng hợp nhất | Hành động | 
| --- | --- | --- | --- | 
| 1 | (1,8) | [] | Bắt đầu đoạn đầu tiên | 
| 2 | (10,2000000010) | [(1,8)] | Tạo phân khúc thứ hai | 

Khoảng cách giữa các vị trí`9`Và`9`, vậy đáp án là:```
9
```Điều này chứng tỏ thuật toán xử lý tọa độ rất lớn mà không cấp phát bộ nhớ dựa trên`n`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(bạn đăng nhập bạn) | Sắp xếp các khoảng chiếm ưu thế trong một lần hợp nhất | 
| Không gian | O(u) | Chỉ các khoảng đầu vào và khoảng thời gian hợp nhất mới được lưu trữ | 

Với nhiều nhất`1000`khoảng thời gian, sắp xếp và quét là tầm thường. Kích thước tọa độ không ảnh hưởng đến thời gian chạy, vì vậy ngay cả giá trị tối đa của`n`được xử lý dễ dàng. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve_case(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    data = sys.stdin.readline

    n, u = map(int, data().split())
    intervals = []

    for _ in range(u):
        intervals.append(tuple(map(int, data().split())))

    if u == 0:
        ans = 1
    else:
        intervals.sort()
        merged = []
        l, r = intervals[0]

        for nl, nr in intervals[1:]:
            if nl <= r + 1:
                r = max(r, nr)
            else:
                merged.append((l, r))
                l, r = nl, nr

        merged.append((l, r))

        if merged[0][0] > 1:
            ans = 1
        else:
            ans = merged[-1][1] + 1
            for i in range(len(merged) - 1):
                if merged[i + 1][0] - merged[i][1] > 1:
                    ans = merged[i][1] + 1
                    break

    sys.stdin = old_stdin
    return str(ans)

assert solve_case("""10 3
7 10
1 1
3 8
""") == "2"

assert solve_case("""2000000010 2
1 8
10 2000000010
""") == "9"

assert solve_case("""10 1
3 10
""") == "1"

assert solve_case("""10 1
1 9
""") == "10"

assert solve_case("""10 3
3 5
5 8
7 10
""") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`10 1 / 3 10`|`1`| Xử lý khoảng cách tiền tố | 
|`10 1 / 1 9`|`10`| Xử lý khoảng cách hậu tố | 
|`10 3 / 3 5 / 5 8 / 7 10`|`1`| Khoảng chồng chéo | 
|`2000000010 2 / 1 8 / 10 2000000010`|`9`| Tọa độ rất lớn | 

## Vỏ cạnh 

Đối với trường hợp tiền tố:```
10 1
3 10
```Danh sách hợp nhất chỉ chứa`(3,10)`. Khoảng thời gian hợp nhất đầu tiên không bắt đầu ở vị trí`1`, do đó thuật toán trả về ngay`1`. Giải pháp dựa trên quá trình quét sẽ hoạt động ở đây nhưng giải pháp dựa trên khoảng thời gian phải xử lý rõ ràng ranh giới này. 

Đối với trường hợp hậu tố:```
10 1
1 9
```Danh sách được hợp nhất là`(1,9)`. Không có khoảng trống bên trong nên lần kiểm tra cuối cùng sẽ trả về`9 + 1 = 10`. Điều này nắm bắt các giải pháp chỉ kiểm tra không gian trước hoặc giữa các khoảng thời gian. 

Đối với các khoảng chồng chéo:```
10 3
3 5
5 8
7 10
```Các khoảng đã sắp xếp hợp nhất thành`(3,10)`. Thuật toán không bao giờ coi phạm vi phủ sóng lặp đi lặp lại là tạo thêm các vị trí miễn phí. Vì tiền tố không được phát hiện nên nó trả về`1`, đó là câu trả lời hợp lệ duy nhất.
