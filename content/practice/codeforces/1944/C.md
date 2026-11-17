---
title: "CF 1944C - MEX Trận 1"
description: "Alice và Bob chơi trên nhiều bộ số. Alice và Bob lần lượt loại bỏ các phần tử khỏi mảng, Alice di chuyển trước. Sự khác biệt là bất cứ khi nào Alice xóa một giá trị, cô ấy cũng giữ nó trong mảng c cá nhân của mình, trong khi Bob chỉ xóa một giá trị."
date: "2026-06-09T01:54:26+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "games", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1944
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 934 (Div. 2)"
rating: 1300
weight: 1944
solve_time_s: 209
verified: true
draft: false
---

[CF 1944C - Trò chơi MEX 1](https://codeforces.com/problemset/problem/1944/C) 

**Đánh giá:** 1300 
**Tags:** thuật toán xây dựng, trò chơi, tham lam 
**Thời gian giải:** 3m 29s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Alice và Bob chơi trên nhiều bộ số. Alice và Bob lần lượt loại bỏ các phần tử khỏi mảng, Alice di chuyển trước. Sự khác biệt là bất cứ khi nào Alice xóa một giá trị, cô ấy cũng giữ nó trong mảng cá nhân của mình`c`, trong khi Bob chỉ xóa một giá trị. 

Khi tất cả các yếu tố đã bị loại bỏ, điểm của trò chơi là MEX của`c`, nghĩa là số nguyên không âm nhỏ nhất không xuất hiện trong số các giá trị Alice thu thập được. 

Alice muốn MEX này càng lớn càng tốt. Bob muốn nó càng nhỏ càng tốt. Chúng ta phải xác định tỷ số cuối cùng nếu cả hai người chơi đều chơi hoàn hảo. 

Các giá trị mảng luôn nằm trong khoảng`0`Và`n-1`và tổng số phần tử trong tất cả các trường hợp thử nghiệm nhiều nhất là`2 · 10^5`. Điều này ngay lập tức gợi ý rằng bất cứ thứ gì bậc hai đều quá đắt. Một giải pháp thực hiện công việc tỷ lệ thuận với số phần tử, hoặc có lẽ`n log n`, dễ dàng đủ nhanh. 

Phần khó khăn là đây là một trò chơi. Việc triển khai đơn giản có thể cố gắng mô phỏng từng bước chơi tối ưu, nhưng số lượng trạng thái trò chơi có thể có sẽ tăng theo cấp số nhân. Nhiệm vụ thực sự là hiểu những giá trị nào Alice có thể áp đặt vào bộ sưu tập của mình và những giá trị nào Bob có thể ngăn chặn. 

Một số trường hợp khó khăn rất dễ bị đánh giá sai. 

Coi như:```
2
1 1
```không có`0`không hề. Alice không bao giờ có thể có được một`0`, vậy câu trả lời là`0`. 

Coi như:```
4
0 1 2 3
```Mỗi giá trị bắt buộc xuất hiện chính xác một lần. Alice lấy một trong số chúng, Bob ngay lập tức xóa một cái khác. Bob có thể phá hủy một thành phần còn thiếu của MEX trong tương lai, vì vậy câu trả lời chỉ là`1`, không`4`. 

Coi như:```
4
0 0 1 1
```Mỗi giá trị quan trọng xuất hiện hai lần. Alice có thể lấy một bản trong khi Bob loại bỏ bản còn lại. Cả hai`0`Và`1`sống sót, vì vậy câu trả lời trở thành`2`. 

Một lỗi phổ biến là tính MEX thông thường của mảng ban đầu. Trò chơi nói về những giá trị mà Alice có thể đảm bảo thu thập, chứ không phải những giá trị nào chỉ tồn tại. 

## Phương pháp tiếp cận 

Quan điểm bạo lực là coi trò chơi như một tìm kiếm tối đa. Từ mọi trạng thái, Alice chọn một phần tử để tối đa hóa MEX cuối cùng, trong khi Bob chọn một phần tử để giảm thiểu nó. Điều này đúng vì nó trực tiếp mô hình hóa cách chơi tối ưu. 

Thật không may, ngay cả đối với khiêm tốn`n`, số lượng các trạng thái trở nên rất lớn. Mỗi bước di chuyển đều có nhiều khả năng, khiến cho độ phức tạp về cơ bản trở nên phức tạp theo cấp số nhân. Với`n`lên đến`2 · 10^5`, điều này hoàn toàn không thể thực hiện được. 

Quan sát quan trọng là chỉ có tần số của các giá trị mới quan trọng. 

Giả sử chúng ta tập trung vào một số số`x`. 

Nếu như`x`hoàn toàn không tồn tại, Alice không bao giờ có thể có được nó. 

Nếu như`x`xuất hiện đúng một lần thì người chơi nào nhận được bản sao duy nhất đó sẽ xác định xem`x`sẽ thuộc về bộ sưu tập của Alice. Bob luôn có thể nhắm tới những giá trị dễ bị tổn thương như vậy. 

Nếu như`x`xuất hiện ít nhất hai lần, Alice được bảo vệ nhiều hơn. Ngay cả khi Bob xóa một bản sao thì bản sao khác vẫn có sẵn. 

Để đạt được MEX cuối cùng ít nhất`k`, Alice phải lấy mọi giá trị từ`0`bởi vì`k-1`. 

Chiến lược tốt nhất của Bob là tạo ra giá trị còn thiếu càng sớm càng tốt. Giá trị có tần số`1`là những nơi duy nhất mà anh ta có thể tiêu diệt vĩnh viễn một số bằng cách lấy bản sao duy nhất của nó. 

Điều này dẫn đến một đặc tính đơn giản đáng chú ý. 

Quét các giá trị từ`0`trở lên. 

Nếu một giá trị không bao giờ xuất hiện thì giá trị đó sẽ trở thành câu trả lời ngay lập tức. 

Trong số các giá trị xuất hiện, hãy đếm chính xác có bao nhiêu giá trị có tần số`1`. 

Giá trị đầu tiên mà số lượng này đạt tới`2`cũng là câu trả lời. 

Tại sao? Alice di chuyển trước, vì vậy trong số tất cả các giá trị đơn lẻ, cô ấy có thể đảm bảo nhiều nhất một trong số chúng trước khi Bob phá hủy một đơn vị khác. Đĩa đơn thứ hai trở thành khoảng trống không thể tránh khỏi đầu tiên trong bộ sưu tập của Alice. 

Điều này biến một bài toán trò chơi thành một bài toán đếm tần số. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đếm tần số của mọi giá trị trong mảng. 
2. Khởi tạo bộ đếm`single = 0`. Điều này sẽ theo dõi có bao nhiêu giá trị được thấy cho đến nay xảy ra chính xác một lần. 
3. Quét các giá trị từ`0`trở lên. 
4. Nếu giá trị hiện tại có tần số`0`, trả lại giá trị này ngay lập tức. 

Alice không thể thu thập một giá trị bị thiếu nên nó tự động là MEX. 
5. Nếu giá trị hiện tại có tần số`1`, tăng`single`. 
6. Nếu`single`trở thành`2`, trả về giá trị hiện tại. 

Alice có thể đảm bảo sở hữu nhiều nhất một giá trị đơn lẻ. Singleton thứ hai tạo ra giá trị đầu tiên mà Bob có thể buộc vắng mặt trong bộ sưu tập của Alice. 
7. Quá trình quét luôn kết thúc vì các giá trị bị giới hạn trong phạm vi`[0, n-1]`và cuối cùng một số giá trị vượt quá tần số hiện có sẽ được tính`0`. 

### Tại sao nó hoạt động 

Đối với mọi giá trị có tần số ít nhất là hai, Bob không thể loại bỏ hoàn toàn giá trị đó trước khi Alice có cơ hội lấy được một bản sao. Những giá trị như vậy là an toàn. 

Các giá trị có tần số chính xác bằng một là dễ vỡ. Bob có thể loại bỏ vĩnh viễn một singleton bằng cách lấy bản sao duy nhất của nó. Vì Alice di chuyển trước nên cô ấy có thể bảo vệ tối đa một giá trị đơn lẻ trong chuỗi giá trị tăng dần quan trọng đối với MEX. Khi hai giá trị đơn lẻ xuất hiện, Bob luôn có thể ngăn một trong số chúng xâm nhập vào bộ sưu tập của Alice. Giá trị nhỏ nhất mà điều này trở nên không thể tránh khỏi chính là MEX cuối cùng. 

Giá trị vắng mặt đầu tiên cũng ngay lập tức xác định MEX vì Alice không thể thu thập thứ gì đó không tồn tại. 

Hai điều kiện này hoàn toàn đặc trưng cho việc chơi tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    answers = []

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        freq = [0] * (n + 1)
        for x in a:
            freq[x] += 1

        single = 0

        for mex in range(n + 1):
            if freq[mex] == 0:
                answers.append(str(mex))
                break

            if freq[mex] == 1:
                single += 1

            if single == 2:
                answers.append(str(mex))
                break

    sys.stdout.write("\n".join(answers))

if __name__ == "__main__":
    solve()
```Mảng tần số là cấu trúc dữ liệu cốt lõi. Vì mọi giá trị đều thỏa mãn`0 ≤ a[i] < n`, một mảng có kích thước`n + 1`là đủ. 

Quá trình quét tiến hành theo thứ tự tăng dần vì MEX phụ thuộc vào giá trị có vấn đề đầu tiên. Thời điểm chúng ta gặp một giá trị còn thiếu, câu trả lời đã được xác định và chúng ta dừng lại. 

các`single`truy cập theo dõi có bao nhiêu giá trị dễ bị tổn thương đã xuất hiện. Singleton đầu tiên có thể được bảo vệ bởi Alice. Singleton thứ hai là nơi Bob có thể tạo ra một khoảng trống, vì vậy chúng tôi quay lại ngay lập tức. 

Một chi tiết tinh tế đang quét qua`n`. Mặc dù giá trị đầu vào hoàn toàn nhỏ hơn`n`, câu trả lời có thể là`n`. Ví dụ:```
0 1 2 3
```có MEX thông thường`4`. Chỉ sử dụng một mảng tần số có kích thước`n`sẽ bỏ lỡ khả năng này. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
0 0 1 1
```Bảng tần số: 

| Giá trị | Tần số | Đếm đơn | Hành động | 
| --- | --- | --- | --- | 
| 0 | 2 | 0 | tiếp tục | 
| 1 | 2 | 0 | tiếp tục | 
| 2 | 0 | 0 | đáp án = 2 | 

Đầu ra:```
2
```Cả hai giá trị quan trọng đều xuất hiện hai lần nên Bob không thể chặn hoàn toàn một giá trị nào đó. Giá trị vắng mặt đầu tiên là`2`, trở thành MEX cuối cùng. 

### Ví dụ 2 

đầu vào:```
4
0 1 2 3
```Bảng tần số: 

| Giá trị | Tần số | Đếm đơn | Hành động | 
| --- | --- | --- | --- | 
| 0 | 1 | 1 | tiếp tục | 
| 1 | 1 | 2 | câu trả lời = 1 | 

Đầu ra:```
1
```Mỗi giá trị là một singleton. Alice chỉ có thể bảo đảm một trong số họ. Ngay khi singleton thứ hai xuất hiện, Bob có thể buộc một giá trị còn thiếu trong số`{0,1}`, làm cho MEX bằng`1`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi phần tử được tính một lần và mỗi giá trị được quét một lần | 
| Không gian | O(n) | Dải tần có kích thước`n + 1`| 

Tổng số tiền của`n`trên tất cả các trường hợp thử nghiệm là nhiều nhất`2 · 10^5`. MỘT`O(n)`giải pháp tổng thể chỉ xử lý vài trăm nghìn thao tác, khá thoải mái trong thời gian giới hạn. Việc sử dụng bộ nhớ cũng thấp hơn nhiều so với giới hạn có sẵn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline
    t = int(input())
    ans = []

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        freq = [0] * (n + 1)
        for x in a:
            freq[x] += 1

        single = 0

        for mex in range(n + 1):
            if freq[mex] == 0:
                ans.append(str(mex))
                break

            if freq[mex] == 1:
                single += 1

            if single == 2:
                ans.append(str(mex))
                break

    return "\n".join(ans) + "\n"

# provided samples
assert run(
"""3
4
0 0 1 1
4
0 1 2 3
2
1 1
"""
) == "2\n1\n0\n", "sample 1"

# minimum size
assert run(
"""1
1
0
"""
) == "1\n", "single element 0"

# all equal values
assert run(
"""1
5
0 0 0 0 0
"""
) == "1\n", "all zeros"

# missing zero
assert run(
"""1
4
1 1 2 2
"""
) == "0\n", "mex starts at zero"

# two singleton values
assert run(
"""1
5
0 1 2 2 2
"""
) == "1\n", "second singleton determines answer"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1 / 0`|`1`| Đầu vào hợp lệ nhỏ nhất | 
|`5 / 0 0 0 0 0`|`1`| Tất cả các giá trị giống nhau | 
|`4 / 1 1 2 2`|`0`| Thiếu xử lý số 0 | 
|`5 / 0 1 2 2 2`|`1`| Quy tắc đơn thứ hai | 

## Vỏ cạnh 

Hãy xem xét:```
1
2
1 1
```Tần số của`0`đã bằng không rồi. Trong quá trình quét, thuật toán dừng ngay ở giá trị`0`và trả về`0`. Alice không thể thu thập một giá trị không tồn tại, do đó MEX phải được`0`. 

Coi như:```
1
4
0 1 2 3
```Quá trình quét thấy tần số`1`cho giá trị`0`, làm`single = 1`. Sau đó nó nhìn thấy tần số`1`cho giá trị`1`, làm`single = 2`, và trả về`1`. Điều này cho thấy thực tế là Bob có thể phá hủy một trong các giá trị đơn lẻ trước khi Alice thu thập tất cả chúng. 

Coi như:```
1
6
0 0 1 1 2 2
```Tất cả các tần số lên đến`2`ít nhất là hai. Bộ đếm đơn không bao giờ tăng. Quá trình quét đạt giá trị`3`, có tần số bằng 0 và trả về`3`. Nhiều bản sao bảo vệ mọi giá trị cần thiết khỏi sự can thiệp của Bob. 

Coi như:```
1
5
0 0 1 2 2
```Các quá trình quét: 

| Giá trị | Tần số | Đếm đơn | 
| --- | --- | --- | 
| 0 | 2 | 0 | 
| 1 | 1 | 1 | 
| 2 | 2 | 1 | 
| 3 | 0 | 1 | 

Giá trị còn thiếu đầu tiên là`3`, vậy câu trả lời là`3`. Chỉ có một singleton mà Alice có thể bảo đảm, vì vậy Bob không thể tạo ra khoảng trống sớm hơn.
