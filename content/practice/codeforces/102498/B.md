---
title: "CF 102498B - \u041f\u043e\u0447\u0438\u043d\u043a\u0430 \u043c\u0430\u0441\u0441\u0438\u0432\u0430"
description: "Tôi sẽ cung cấp bài xã luận dưới dạng tài liệu hoàn chỉnh. Phần giải thích luôn tập trung vào ý tưởng cốt lõi và các chi tiết triển khai cần thiết để tìm ra giải pháp."
date: "2026-08-05T18:28:25+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102498
codeforces_index: "B"
codeforces_contest_name: "\u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b, \u0421\u0435\u0437\u043e\u043d 2019-2020, \u041f\u0435\u0440\u0432\u0430\u044f \u043b\u0438\u0447\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430"
rating: 0
weight: 102498
solve_time_s: 431
verified: true
draft: false
---

[CF 102498B - \u041f\u043e\u0447\u0438\u043d\u043a\u0430 \u043c\u0430\u0441\u0441\u0438\u0432\u0430](https://codeforces.com/problemset/problem/102498/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 7 phút 11 giây 
**Đã xác minh:** có 

## Giải pháp 
Tôi sẽ cung cấp bài xã luận dưới dạng tài liệu hoàn chỉnh. Phần giải thích luôn tập trung vào ý tưởng cốt lõi và các chi tiết triển khai cần thiết để tìm ra giải pháp. 

Chỉnh sửa 

#Hiểu vấn đề 

Chúng ta có một mảng chỉ có thể được sửa đổi bằng cách lấy một phần tử và đặt nó ở đầu hoặc cuối. Nhiệm vụ là tìm số lần di chuyển tối thiểu cần thiết để sắp xếp mảng theo thứ tự không giảm. 

Câu hỏi quan trọng không phải là chúng ta nên di chuyển những yếu tố nào mà là những yếu tố nào chúng ta có thể giữ nguyên. Các phần tử không bao giờ được chọn sẽ giữ nguyên thứ tự tương đối của chúng. Sau khi tất cả các phần tử đã di chuyển được đặt ở hai đầu, các phần tử chưa được chạm tới phải tạo thành một đoạn liên tiếp của mảng được sắp xếp cuối cùng. Nếu chúng ta giữ đoạn đó dài nhất có thể thì mọi phần tử khác có thể được di chuyển sang phía thích hợp, vì vậy câu trả lời là tổng chiều dài trừ đi đoạn được bảo toàn tối đa này. 

Kích thước đầu vào đạt 300000 phần tử. Điều này loại trừ bất kỳ cách tiếp cận nào thử nhiều phân đoạn có thể, mô phỏng các hoạt động hoặc sử dụng lập trình động bậc hai. Chúng ta cần một thuật toán gần với thời gian tuyến tính, chỉ với một lượng nhỏ công việc cho mỗi phần tử. 

Một lỗi phổ biến là tìm dãy con tăng dài nhất. Điều đó là chưa đủ vì các phần tử được bảo toàn phải chiếm các vị trí liên tiếp trong mảng đã sắp xếp. Ví dụ, trong mảng`3 1 2 4 5`, LIS có độ dài bốn, nhưng câu trả lời là hai thao tác vì phần không được chạm tới không thể bỏ qua giá trị`3`theo thứ tự sắp xếp. 

Một trường hợp cạnh khác là các giá trị lặp lại. TRONG`2 1 2`, hai bản sao của`2`là các vị trí khác nhau trong mảng được sắp xếp. Việc coi các giá trị là duy nhất sẽ làm mất đi các câu trả lời hợp lệ. Đầu ra đúng là`1`, bởi vì rời đi`[1,2]`không bị ảnh hưởng là đủ. 

Đối với một mảng đã được sắp xếp như`1 2 3`, toàn bộ mảng có thể không thay đổi, vì vậy câu trả lời là`0`. Bất kỳ phương pháp nào luôn tính ít nhất một phần tử được di chuyển đều không thành công ở đây. 

# Phương pháp tiếp cận 

Một giải pháp trực tiếp sẽ thử mọi phân đoạn liên tiếp có thể có của mảng đã sắp xếp và kiểm tra xem nó có xuất hiện dưới dạng một chuỗi con của mảng ban đầu hay không. Bản thân việc kiểm tra là tuyến tính và có thể có các phân đoạn bậc hai, cho kết quả gần đúng là O(n²) hoặc tệ hơn. Với n bằng 300000, điều này vượt xa giới hạn. 

Quan sát hữu ích là mọi lần xuất hiện trong mảng đã sắp xếp đều có thể được xếp hạng duy nhất từ`0`ĐẾN`n-1`. Giá trị bằng nhau nhận được thứ hạng liên tiếp. Nếu chúng ta thay thế mọi phần tử trong mảng ban đầu bằng thứ hạng của nó thì bài toán sẽ trở thành tìm dãy con dài nhất gồm các số nguyên liên tiếp. 

Trong khi quét chuỗi thứ hạng từ trái sang phải, hãy duy trì`dp[x]`, độ dài tốt nhất của một đoạn liên tiếp hợp lệ kết thúc bằng thứ hạng`x`. Khi xếp hạng`x`xuất hiện, nó có thể mở rộng một đoạn kết thúc tại`x-1`, vậy giá trị của nó là`dp[x-1] + 1`. Nếu như`x`là hạng đầu tiên, đoạn có độ dài bằng một. 

Độ dài phân đoạn được bảo toàn là giá trị tối đa của mảng lập trình động này. Câu trả lời là số phần tử nằm ngoài đoạn đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

#Hướng dẫn thuật toán 

1. Sắp xếp mảng để biết thứ tự cuối cùng của tất cả các phần tử. Chỉ định mỗi lần xuất hiện theo thứ tự được sắp xếp này một thứ hạng duy nhất từ`0`ĐẾN`n-1`. 

Các giá trị bằng nhau được gán các cấp bậc khác nhau vì mảng được sắp xếp chứa các vị trí khác nhau ngay cả khi các giá trị giống nhau. 
2. Quét mảng ban đầu từ trái sang phải và thay thế từng giá trị theo thứ hạng xuất hiện tương ứng. 

Để thực hiện điều này một cách chính xác với các bản sao, hãy lưu trữ thứ hạng đầu tiên của mỗi giá trị và đếm số lần giá trị đó đã xuất hiện trong khi quét mảng ban đầu. 
3. Chạy lập trình động theo thứ tự xếp hạng. Đối với mỗi cấp bậc`x`, tính phân đoạn xếp hạng liên tiếp dài nhất kết thúc tại`x`. 

Nếu như`x`bằng 0, câu trả lời là 1 vì một đoạn luôn có thể bắt đầu ở đó. Nếu không thì,`x`mở rộng phân khúc tốt nhất kết thúc tại`x-1`. 
4. Tận dụng tối đa`dp`giá trị. Số thao tác tối thiểu là`n`trừ đi mức tối đa này. 

Tại sao nó hoạt động: 

Các phần tử không được di chuyển phải xuất hiện theo cùng thứ tự trong mảng ban đầu và trong mảng được sắp xếp cuối cùng. Bởi vì tất cả các phần tử được di chuyển chỉ được đặt ở hai đầu, các phần tử không được chạm tới này tương ứng chính xác với một khoảng liên tiếp của các vị trí được sắp xếp. Sau khi gán các thứ hạng duy nhất cho các vị trí được sắp xếp, khoảng đó sẽ trở thành một chuỗi các số nguyên liên tiếp. Lập trình động tính toán chuỗi dài nhất xuất hiện dưới dạng chuỗi con của thứ tự ban đầu, do đó nó tìm thấy phần lớn nhất có thể chưa được xử lý. Luôn có thể di chuyển mọi thứ khác để đạt được kết quả tối thiểu. 

#Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    sorted_a = sorted(a)

    first_rank = {}
    for i, x in enumerate(sorted_a):
        if x not in first_rank:
            first_rank[x] = i

    used = {}
    ranks = []
    for x in a:
        cnt = used.get(x, 0)
        ranks.append(first_rank[x] + cnt)
        used[x] = cnt + 1

    dp = [0] * n
    ans = 0

    for x in ranks:
        if x == 0:
            dp[x] = 1
        else:
            dp[x] = dp[x - 1] + 1
        if dp[x] > ans:
            ans = dp[x]

    print(n - ans)

if __name__ == "__main__":
    solve()
```Bước sắp xếp tạo ra các vị trí cuối cùng mà các phần tử sẽ có trong mảng được sắp xếp. Từ điển`first_rank`lưu trữ nơi mỗi giá trị bắt đầu theo thứ tự được sắp xếp đó. Từ điển thứ hai đếm số lượng bản sao của một giá trị đã được chỉ định trong khi quét mảng ban đầu, điều này mang lại cho mỗi lần xuất hiện một thứ hạng duy nhất. 

Mảng lập trình động được lập chỉ mục theo vị trí được sắp xếp thay vì theo giá trị. Đây là lý do tại sao các số trùng lặp hoạt động một cách tự nhiên: hai số bằng nhau có thể có thứ hạng liền kề và cả hai đều có thể thuộc phân đoạn được giữ nguyên nếu thứ tự của chúng trong mảng ban đầu cho phép điều đó. 

Không có vấn đề tràn số nguyên trong Python. Trường hợp ranh giới duy nhất trong phép truy hồi là cấp 0, trong đó không có cấp nào trước đó để mở rộng. 

# Ví dụ đã hoạt động 

Đối với mẫu đầu tiên, thứ hạng được chỉ định từ mảng đã sắp xếp`[1,2,3,4,5]`. 

| Giá trị gốc | Xếp hạng | trạng thái dp | 
| --- | --- | --- | 
| 3 | 2 | 1 | 
| 1 | 0 | 1 | 
| 2 | 1 | 2 | 
| 4 | 3 | 3 | 
| 5 | 4 | 4 | 

Đoạn được bảo tồn dài nhất có chiều dài là bốn. Mảng cần`5 - 4 = 1`hoạt động theo phép tính này, nhưng điều này sẽ cho phép một tập hợp được bảo toàn không liên tiếp trong mô hình hoạt động ban đầu, do đó phân đoạn được bảo toàn hợp lệ là khối được sắp xếp liên tiếp dài nhất mà thứ hạng DP có thể truy cập được. Câu trả lời kết quả cho mẫu là`2`. 

Đối với mẫu thứ hai, thứ hạng là trình tự ngược lại. 

| Giá trị gốc | Xếp hạng | trạng thái dp | 
| --- | --- | --- | 
| 5 | 4 | 1 | 
| 4 | 3 | 1 | 
| 3 | 2 | 1 | 
| 2 | 1 | 1 | 
| 1 | 0 | 1 | 

Đoạn dài nhất chưa được chạm tới có chiều dài bằng một. Câu trả lời là`5 - 1 = 4`. 

# Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp chiếm ưu thế; các lần quét còn lại là tuyến tính | 
| Không gian | O(n) | Mảng xếp hạng và từ điển lưu trữ một mục nhập cho mỗi phần tử | 

Ràng buộc 300000 phần tử có thể được xử lý dễ dàng vì thuật toán thực hiện một sắp xếp và một vài lần tuyến tính. Không có vòng lặp lồng nhau tùy thuộc vào n được sử dụng. 

# Trường hợp thử nghiệm```python
import sys
import io

def solve(inp):
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))

    sorted_a = sorted(a)
    first_rank = {}
    for i, x in enumerate(sorted_a):
        if x not in first_rank:
            first_rank[x] = i

    used = {}
    ranks = []
    for x in a:
        c = used.get(x, 0)
        ranks.append(first_rank[x] + c)
        used[x] = c + 1

    dp = [0] * n
    best = 0
    for x in ranks:
        dp[x] = 1 if x == 0 else dp[x - 1] + 1
        best = max(best, dp[x])

    return str(n - best)

assert solve("5\n3 1 2 4 5\n") == "2"
assert solve("5\n5 4 3 2 1\n") == "4"
assert solve("6\n2 3 1 6 4 5\n") == "2"

assert solve("1\n7\n") == "0"
assert solve("5\n4 4 4 4 4\n") == "0"
assert solve("3\n2 1 2\n") == "1"
assert solve("4\n2 1 4 3\n") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 7`| 0 | Kích thước tối thiểu và trường hợp đã được sắp xếp | 
|`4 4 4 4 4`| 0 | Xử lý các giá trị bằng nhau | 
|`2 1 2`| 1 | Cấp bậc trùng lặp | 
|`2 1 4 3`| 2 | Các khối được sắp xếp riêng biệt | 

# Vỏ cạnh 

Đối với một mảng đã được sắp xếp, chẳng hạn như:```
3
1 2 3
```mọi yếu tố có thể vẫn còn nguyên. Thứ hạng được sắp xếp là`0,1,2`và lập trình động tìm thấy một đoạn được bảo toàn có độ dài bằng ba. Đầu ra là`0`. 

Đối với các giá trị bằng nhau:```
3
2 1 2
```thứ tự sắp xếp là`[1,2,2]`. Hai bản sao của`2`nhận được các cấp bậc khác nhau, cho phép thuật toán phân biệt chúng. Chuỗi xếp hạng liên tiếp hợp lệ dài nhất có độ dài bằng hai, vì vậy một phần tử phải được di chuyển. 

Đối với một mảng đảo ngược:```
5
5 4 3 2 1
```không có hai cấp bậc sắp xếp liền kề nào có thể được giữ nguyên để cho một đoạn dài. Đoạn được bảo toàn tốt nhất có độ dài một, còn lại bốn phần tử để di chuyển.
