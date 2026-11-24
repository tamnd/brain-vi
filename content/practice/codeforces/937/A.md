---
title: "CF 937A - Olympic"
description: "Chúng ta được cung cấp một danh sách điểm của những người tham gia và chúng ta cần đếm xem có bao nhiêu cách khác nhau để chúng ta có thể chọn một nhóm người tham gia nhận bằng theo một quy tắc rất cụ thể. Một nhóm hợp lệ được xác định bằng cách chọn một ngưỡng điểm."
date: "2026-06-17T02:45:18+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "sortings"]
categories: ["algorithms"]
codeforces_contest: 937
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 467 (Div. 2)"
rating: 800
weight: 937
solve_time_s: 62
verified: true
draft: false
---

[CF 937A - Olympic](https://codeforces.com/problemset/problem/937/A) 

**Đánh giá:** 800 
**Tags:** triển khai, sắp xếp 
**Thời gian giải:** 1m 2s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một danh sách điểm của những người tham gia và chúng ta cần đếm xem có bao nhiêu cách khác nhau để chúng ta có thể chọn một nhóm người tham gia nhận bằng theo một quy tắc rất cụ thể. 

Một nhóm hợp lệ được xác định bằng cách chọn một ngưỡng điểm. Khi một người tham gia có số điểm nào đó được đưa vào, mọi người tham gia có điểm lớn hơn hoặc bằng số điểm đó cũng phải được đưa vào. Điều này có nghĩa là tập hợp đã chọn hoàn toàn được xác định bởi một giá trị giới hạn: mọi thứ ở trên hoặc bằng nó đều được chọn, mọi thứ ở dưới nó đều bị loại trừ. Có một hạn chế bổ sung là những người tham gia có điểm 0 không bao giờ được đưa vào bất kỳ nhóm hợp lệ nào. Ngoài ra, nhóm đã chọn không được để trống. 

Đầu vào chỉ đơn giản là danh sách điểm số. Đầu ra là số nhóm hợp lệ riêng biệt có thể được hình thành. 

Các ràng buộc rất nhỏ, với tối đa 100 người tham gia, do đó, ngay cả các giải pháp dựa trên phương pháp bậc hai hoặc sắp xếp cũng dễ dàng đủ nhanh. Đây là tín hiệu cho thấy chúng ta nên tập trung vào lý luận về cấu trúc hơn là các thủ thuật tối ưu hóa. 

Trường hợp cạnh tinh tế xuất hiện khi có số 0. Một cách tiếp cận đơn giản đếm tất cả các ngưỡng điểm riêng biệt mà không tách các số 0 sẽ bao gồm không chính xác các cấu hình trong đó những người tham gia có điểm 0 trở thành một phần của hậu tố đã chọn. Một trường hợp khác là khi tất cả các điểm khác 0 đều bằng nhau. Trong trường hợp đó, chỉ tồn tại một lựa chọn hợp lệ, mặc dù có nhiều cách để chọn ngưỡng nếu chúng ta suy nghĩ một cách máy móc về việc chia tách các giá trị được sắp xếp. 

Ví dụ: hãy xem xét đầu vào:```
5
0 1 2 2 3
```Việc giải thích bất cẩn có thể đếm các ngưỡng ở 0, 1, 2, 3 và cho rằng tất cả đều tạo ra các nhóm hợp lệ. Nhưng ngưỡng 0 sẽ bao gồm những người tham gia có điểm 0, điều này bị cấm, vì vậy lựa chọn đó phải bị loại trừ. 

Một ví dụ khác:```
3
0 5 5
```Chỉ tồn tại một nhóm hợp lệ: chọn điểm 5 bao gồm cả 5 điểm và chọn bất kỳ điểm nào khác vi phạm quy tắc hoặc tạo ra cùng một tập hợp. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là thử mọi tập hợp con có thể có của những người tham gia và kiểm tra xem nó có thỏa mãn quy tắc hay không. Đối với mỗi tập hợp con, chúng tôi sẽ xác minh hai điều kiện: không bao gồm người tham gia có điểm 0 và nếu bao gồm một người tham gia thì tất cả những người tham gia có điểm cao hơn hoặc bằng nhau cũng được bao gồm. Với n người tham gia, có 2^n tập hợp con và việc kiểm tra mỗi tập hợp con mất O(n), cho ra O(n·2^n), điều này trở nên không khả thi ngay cả đối với n vừa phải. 

Quan sát quan trọng là cấu trúc của các tập hợp con hợp lệ là cực kỳ cứng nhắc. Khi chúng tôi chọn một người tham gia có số điểm x nào đó, chúng tôi buộc phải bao gồm những người có số điểm ít nhất là x. Vì vậy, thay vì nghĩ về các tập hợp con của chỉ số, chúng ta nên nghĩ về ngưỡng điểm. 

Sau khi sắp xếp điểm, mỗi nhóm hợp lệ sẽ tương ứng với việc chọn một giá trị điểm riêng biệt làm điểm giới hạn, nhưng chỉ trong số các điểm dương. Mỗi giá trị điểm dương riêng biệt xác định chính xác một tập hợp lệ: tất cả những người tham gia có điểm ít nhất là giá trị đó. Nếu chúng tôi chuyển ngưỡng từ điểm cao hơn sang điểm thấp hơn, chúng tôi chỉ thêm nhiều người tham gia hơn chứ không bao giờ xóa những người tham gia trước đó theo cách tạo ra cấu hình mới vượt quá các mức điểm riêng biệt. 

Do đó, câu trả lời quy về việc đếm xem có bao nhiêu giá trị điểm dương riêng biệt tồn tại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n·2^n) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(1) hoặc O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các điểm vào một mảng. Điều này là cần thiết vì chúng ta cần phân tích sự phân bổ các giá trị hơn là danh tính cá nhân. 
2. Loại bỏ hoặc bỏ qua tất cả các giá trị 0. Những người tham gia này không thể xuất hiện trong bất kỳ nhóm hợp lệ nào, vì vậy họ không thể ảnh hưởng đến nhóm nào tồn tại. 
3. Sắp xếp các điểm dương còn lại. Sắp xếp các nhóm có giá trị bằng nhau, giúp dễ dàng xác định các mức điểm khác nhau. 
4. Quét qua danh sách đã sắp xếp và đếm số lần giá trị thay đổi. Mỗi giá trị riêng biệt đại diện cho một ngưỡng mới có thể có. 
5. Xuất ra số đếm này là số bộ bài tập bằng tốt nghiệp hợp lệ. 

Ý tưởng chính là mỗi điểm dương riêng biệt sẽ xác định chính xác một “điểm cắt” trong đó tập hợp đã chọn thay đổi một cách có ý nghĩa. 

### Tại sao nó hoạt động 

Sau khi sắp xếp, tất cả những người tham gia được sắp xếp theo điểm số. Mọi lựa chọn hợp lệ đều phải tương ứng với việc chọn điểm x tối thiểu. Khi x được cố định, tập hợp được xác định duy nhất là tất cả các phần tử có điểm ≥ x. Do đó, số lượng tập hợp hợp lệ chính xác là số lượng giá trị phân biệt x có thể dùng ở mức tối thiểu như vậy, chính xác là số điểm dương khác biệt. 

Không có hai ngưỡng khác nhau trong cùng một giá trị tạo ra các tập hợp khác nhau và số 0 không thể dùng làm ngưỡng vì nó vi phạm hạn chế. Điều này làm cho việc ánh xạ giữa các tập hợp hợp lệ và các điểm dương riêng biệt trở thành một đối một. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    a = [x for x in a if x > 0]
    a.sort()

    if not a:
        print(0)
        return

    cnt = 1
    for i in range(1, len(a)):
        if a[i] != a[i - 1]:
            cnt += 1

    print(cnt)

if __name__ == "__main__":
    solve()
```Việc triển khai bắt đầu bằng cách lọc ra các số 0 vì chúng rõ ràng không được phép trong bất kỳ lựa chọn hợp lệ nào. Sắp xếp được sử dụng để nhóm các điểm bằng nhau lại với nhau để có thể tính các giá trị riêng biệt trong một lần vượt qua. 

Bộ đếm bắt đầu từ một vì danh sách điểm dương không trống luôn đóng góp ít nhất một giá trị riêng biệt. Sau đó, chúng tôi chỉ tăng bộ đếm khi quan sát thấy sự chuyển đổi giữa các giá trị khác nhau. 

Một lỗi phổ biến là quên loại bỏ số 0 trước khi đếm các giá trị riêng biệt, điều này làm tăng câu trả lời không chính xác bằng cách coi số 0 là ngưỡng hợp lệ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
1 3 3 2
```Sắp xếp điểm tích cực:```
[1, 2, 3, 3]
```| Bước | Chỉ mục | Giá trị | Mới khác biệt? | Đếm | 
| --- | --- | --- | --- | --- | 
| ban đầu | - | - | - | 1 | 
| 1 | 1 | 2 | vâng | 2 | 
| 2 | 2 | 3 | vâng | 3 | 
| 3 | 3 | 3 | không | 3 | 

Đầu ra là 3. 

Điều này xác nhận rằng mỗi mức điểm riêng biệt tương ứng với một ngưỡng hợp lệ: 3, 2 và 1. 

### Ví dụ 2 

đầu vào:```
5
0 0 5 5 5
```Lọc và sắp xếp:```
[5, 5, 5]
```| Bước | Chỉ mục | Giá trị | Mới khác biệt? | Đếm | 
| --- | --- | --- | --- | --- | 
| ban đầu | - | - | - | 1 | 
| 1 | 1 | 5 | không | 1 | 
| 2 | 2 | 5 | không | 1 | 

Đầu ra là 1. 

Điều này chứng tỏ rằng các số 0 không đóng góp vào bất kỳ nhóm hợp lệ nào và tất cả các điểm dương giống hệt nhau sẽ chuyển thành một lựa chọn hợp lệ duy nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp chiếm ưu thế; quét tuyến tính sau đó | 
| Không gian | O(n) | Lưu trữ điểm đã lọc | 

Các ràng buộc cho phép tối đa 100 phần tử, do đó, ngay cả một giải pháp sắp xếp đơn giản cũng thấp hơn nhiều so với giới hạn. Việc sử dụng bộ nhớ là không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return main(inp)

def main(inp=None):
    import sys
    input = sys.stdin.readline
    n = int(input())
    a = list(map(int, input().split()))
    a = [x for x in a if x > 0]
    a.sort()
    if not a:
        return "0"
    cnt = 1
    for i in range(1, len(a)):
        if a[i] != a[i - 1]:
            cnt += 1
    return str(cnt)

# provided sample
assert run("4\n1 3 3 2\n") == "3"

# custom cases
assert run("1\n0\n") == "0"
assert run("3\n5 5 5\n") == "1"
assert run("5\n0 1 2 3 4\n") == "4"
assert run("6\n0 0 0 1 1 2\n") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 0 | 0 | chỉ có số 0 mới tạo ra nhóm không hợp lệ | 
| 5 5 5 | 1 | trùng lặp được thu gọn chính xác | 
| 0 1 2 3 4 | 4 | số không bị bỏ qua, nhiều ngưỡng | 
| 0 0 0 1 1 2 | 2 | số không hỗn hợp và trùng lặp | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi tất cả các phần tử đều bằng 0 ngoại trừ ràng buộc đảm bảo có ít nhất một phần tử khác 0, nhưng chúng ta vẫn phải xử lý các trường hợp trong đó quá trình lọc loại bỏ mọi thứ. Đối với đầu vào như:```
3
0 0 5
```Sau khi lọc chúng ta nhận được`[5]`. Sắp xếp không làm gì cả. Thuật toán khởi tạo được tính là 1 và trả về 1, tương ứng với lựa chọn hợp lệ duy nhất: lấy điểm 5 và mọi điểm cao hơn nó (chỉ những người tham gia). 

Một trường hợp khác là khi tất cả các giá trị dương giống hệt nhau:```
4
0 7 7 7
```Danh sách đã lọc là`[7, 7, 7]`. Vòng lặp không bao giờ tìm thấy sự thay đổi về giá trị, vì vậy số đếm vẫn bằng 1. Điều này phản ánh chính xác rằng chỉ tồn tại một ngưỡng. 

Cuối cùng, tăng điểm nghiêm ngặt:```
4
1 2 3 4
```Danh sách đã lọc và sắp xếp không thay đổi. Mỗi lần chuyển đổi sẽ tăng số lượng, tạo ra 4 bộ hợp lệ riêng biệt, phù hợp với ý tưởng rằng mỗi mức điểm sẽ xác định một ngưỡng khác nhau.
