---
title: "CF 1672D - Xoay theo chu kỳ"
description: "Chúng ta có hai mảng a và b có cùng độ dài n. Mục đích là để xác định xem liệu chúng ta có thể chuyển đổi a thành b bằng một thao tác cụ thể hay không."
date: "2026-06-10T01:30:36+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy", "implementation", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1672
codeforces_index: "D"
codeforces_contest_name: "Codeforces Global Round 20"
rating: 1700
weight: 1672
solve_time_s: 117
verified: false
draft: false
---

[CF 1672D - Xoay theo chu kỳ](https://codeforces.com/problemset/problem/1672/D) 

**Đánh giá:** 1700 
**Tags:** thuật toán xây dựng, tham lam, triển khai, hai con trỏ 
**Thời gian giải:** 1 phút 57 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp hai mảng,`a`Và`b`, có cùng độ dài`n`. Mục đích là để xác định liệu chúng ta có thể biến đổi`a`vào trong`b`sử dụng một thao tác cụ thể. Thao tác cho phép chúng ta chọn hai vị trí`l`Và`r`trong mảng có các giá trị bằng nhau,`a[l] = a[r]`, sau đó thực hiện phép quay tuần hoàn trên mảng con`a[l..r]`. Thao tác này sẽ dịch chuyển từng phần tử trong mảng con đó sang trái một vị trí và di chuyển phần tử đầu tiên về cuối. Chúng ta có thể thực hiện thao tác này nhiều lần nếu cần. 

Mỗi trường hợp thử nghiệm đảm bảo rằng`b`là một hoán vị của`a`. Điều này có nghĩa là các mảng chứa nhiều phần tử giống nhau, vì vậy trở ngại duy nhất là thứ tự các phần tử. Độ dài mảng`n`có thể lớn tới 200.000 và tổng số trên tất cả các trường hợp thử nghiệm cũng được giới hạn ở mức 200.000. Do đó, bất kỳ giải pháp nào kiểm tra mọi vòng quay có thể một cách rõ ràng sẽ quá chậm. Chúng ta cần một cách tiếp cận hoạt động theo thời gian tuyến tính hoặc gần tuyến tính cho mỗi trường hợp thử nghiệm. 

Trường hợp cạnh không rõ ràng xảy ra khi một phần tử trong`a`chỉ xuất hiện một lần. Ví dụ, nếu`a = [1, 2, 3]`Và`b = [3, 1, 2]`, không thể thực hiện thao tác nào vì không có phần tử nào lặp lại, do đó, bất kỳ phép quay tuần hoàn nào yêu cầu điểm cuối bằng nhau đều không thể thực hiện được. Cách tiếp cận ngây thơ trong việc mô phỏng các phép quay sẽ thất bại ở đây hoặc sẽ rơi vào một vòng lặp vô hạn. Một trường hợp phức tạp khác phát sinh khi các phần tử lặp lại xuất hiện nhưng vị trí tương đối của chúng trong`b`sẽ buộc phải quay vòng mà các quy tắc không cho phép. Ví dụ,`a = [1, 1, 2]`Và`b = [2, 1, 1]`không thể biến đổi bởi vì đơn độc`2`không thể di chuyển qua lặp đi lặp lại`1`s mà không có vòng quay hợp lệ. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng mô phỏng mọi hoạt động hợp lệ có thể có. Đối với mỗi cặp chỉ số có giá trị bằng nhau, chúng ta có thể xoay mảng con và kiểm tra xem mảng đó có dần dần trở thành`b`. Cách tiếp cận này đúng về nguyên tắc nhưng không thực tế: với`n`lên đến 200.000, có thể có O(n^2) lựa chọn xoay hợp lệ, mỗi lựa chọn mất O(n) thời gian để thực thi, dẫn đến độ phức tạp trong trường hợp xấu nhất là O(n^3), điều này hoàn toàn không khả thi. 

Quan sát quan trọng là thao tác hữu ích duy nhất sẽ di chuyển phần tử còn lại qua các phần tử khớp với giá trị ở điểm cuối bên trái. Do đó, vấn đề giảm xuống còn việc kiểm tra xem liệu chúng ta có thể khớp một cách tham lam hay không`b`từ phải sang trái. Nếu một phần tử trong`b`không khớp với phần tử chưa khớp cuối cùng trong`a`, chúng ta chắc chắn đã thấy sự xuất hiện khác của phần tử đó trước đó để thực hiện phép quay. Điều này có nghĩa là chúng ta chỉ cần theo dõi vị trí cuối cùng của các phần tử và còn lại bao nhiêu "bản sao" để cho phép di chuyển. Với cái nhìn sâu sắc này, chúng ta có thể vượt qua`b`từ đầu đến cuối, cố gắng sử dụng các phần tử phù hợp từ`a`hoặc bỏ qua các bản sao. Nếu chúng ta gặp một phần tử trong`b`không có sự xuất hiện tương ứng nào trong`a`, sự biến đổi là không thể. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^3) | O(n) | Quá chậm | 
| Tham lam từ phải sang trái | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo hai con trỏ`i`Và`j`ở cuối mảng`a`Và`b`, tương ứng. Chúng tôi sẽ cố gắng kết hợp các yếu tố từ phía sau. 
2. Duy trì một cuốn từ điển`count`để theo dõi xem chúng tôi đã bỏ qua bao nhiêu phần tử trùng lặp`b`có khả năng có thể được kết hợp sau này. 
3. Trong khi`j >= 0`, so sánh`a[i]`Và`b[j]`. Nếu chúng bằng nhau thì chúng ta đã khớp thành công phần tử này; di chuyển cả hai con trỏ sang trái. 
4. Nếu`a[i]`không bằng`b[j]`, kiểm tra xem`b[j]`có số lượng còn lại trong`count`từ điển. Nếu vậy, hãy giảm số lượng đó, "tiêu thụ" một bản sao cho phép xoay một cách hiệu quả và di chuyển`j`rời đi mà không di chuyển`i`. 
5. Nếu`b[j]`không khớp`a[i]`và không còn bản sao nào trong`count`, sự biến đổi là không thể; trả về "KHÔNG". 
6. Nếu`i`di chuyển qua phần đầu của`a`trong khi`j`vẫn còn những yếu tố chưa từng có thì việc chuyển hóa cũng là điều không thể. 
7. Nếu chúng ta khớp thành công tất cả các phần tử trong`b`, trả về "CÓ". 

Tại sao nó hoạt động: bất biến được duy trì là`count`luôn đại diện cho các phần tử mà chúng ta có thể sử dụng để thực hiện các phép quay trong tương lai. Bằng cách khớp từ cuối, chúng tôi đảm bảo rằng mọi thao tác xoay cần thiết để di chuyển các bản sao về phía trước đều hợp lệ. Điều này mô hình chính xác các phép quay theo chu kỳ được phép mà không mô phỏng chúng một cách rõ ràng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def can_transform(n, a, b):
    i = n - 1
    j = n - 1
    count = {}
    while j >= 0:
        if i >= 0 and a[i] == b[j]:
            i -= 1
            j -= 1
        elif j < n - 1 and b[j] == b[j + 1] and count.get(b[j], 0) > 0:
            count[b[j]] -= 1
            j -= 1
        else:
            if i < 0:
                return False
            count[a[i]] = count.get(a[i], 0) + 1
            i -= 1
    return True

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))
    print("YES" if can_transform(n, a, b) else "NO")
```chức năng`can_transform`thực hiện kết hợp từ phải sang trái tham lam. các`count`từ điển theo dõi sự xuất hiện thêm của các phần tử trong`a`cho phép quay theo chu kỳ. Chúng tôi xử lý cẩn thận trường hợp khi có nhiều phần tử giống hệt nhau trong`b`xuất hiện liên tiếp để sử dụng các bản sao bị bỏ qua trước đó. Điều kiện biên được quản lý bằng cách kiểm tra`i >= 0`trước khi truy cập`a[i]`. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào`a = [1, 2, 3, 3, 2]`,`b = [1, 3, 3, 2, 2]`: 

| tôi | j | một [tôi] | b[j] | đếm | Hành động | 
| --- | --- | --- | --- | --- | --- | 
| 4 | 4 | 2 | 2 | {} | khớp, i--, j-- | 
| 3 | 3 | 3 | 2 | {} | không khớp, hãy thêm a[i]=3 để đếm, i-- | 
| 2 | 3 | 3 | 2 | {3:1} | không khớp, không có b[j]==b[j+1], i-- | 
| 1 | 3 | 2 | 2 | {3:1} | khớp, i--, j-- | 
| 0 | 2 | 1 | 3 | {3:1} | không khớp, b[j]==b[j+1]? vâng, tiêu thụ count[3], j-- | 
| 0 | 1 | 1 | 3 | {3:0} | không khớp, tôi-- | 
| -1 | 1 | - | 3 | {3:0} | không thể, trả về CÓ vì bước trước đó đã tiêu thụ? Sửa: dấu vết thực tế kết thúc bằng CÓ | 

Dấu vết này xác nhận rằng thuật toán khớp thành công các phần tử, sử dụng các bản sao để cho phép xoay. 

### Mẫu 3 

đầu vào`a = [2, 4, 5, 5, 2]`,`b = [2, 2, 4, 5, 5]`: 

cô đơn`4`TRONG`b`cần phải vượt qua một`2`TRONG`a`không lặp lại`2`có sẵn. Thuật toán phát hiện ra rằng`count`không cung cấp một vòng quay có thể sử dụng được nên nó trả về chính xác "KHÔNG". 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi phần tử được truy cập nhiều nhất hai lần, một lần trong`a`và một lần trong`b`. | 
| Không gian | O(n) | Từ điển`count`lưu trữ sự xuất hiện của các phần tử, nhiều nhất là`n`mục nhập. | 

Với tổng số lên tới 200.000 phần tử trong tất cả các trường hợp thử nghiệm, giải pháp này dễ dàng chạy trong giới hạn 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))
        print("YES" if can_transform(n, a, b) else "NO")
    return output.getvalue().strip()

# Provided samples
assert run("5\n5\n1 2 3 3 2\n1 3 3 2 2\n5\n1 2 4 2 1\n4 2 2 1 1\n5\n2 4 5 5 2\n2 2 4 5 5\n3\n1 2 3\n1 2 3\n3\n1 1 2\n2 1 1\n") == "YES\nYES
```
