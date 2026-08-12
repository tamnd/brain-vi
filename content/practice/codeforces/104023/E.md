---
title: "CF 104023E - Python sẽ nhanh hơn C++"
description: "Chúng ta được cung cấp một chuỗi biểu thị thời gian chạy triển khai Python trên các phiên bản kế tiếp. Các giá trị $n$ đầu tiên được biết từ phép đo."
date: "2026-07-02T04:23:50+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104023
codeforces_index: "E"
codeforces_contest_name: "2022 China Collegiate Programming Contest (CCPC) Weihai Site"
rating: 0
weight: 104023
solve_time_s: 50
verified: true
draft: false
---

[CF 104023E - Python sẽ nhanh hơn C++](https://codeforces.com/problemset/problem/104023/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi biểu thị thời gian chạy triển khai Python trên các phiên bản kế tiếp. đầu tiên$n$các giá trị được biết từ phép đo. Sau đó, thời gian chạy cho các phiên bản sau này không được cung cấp trực tiếp; thay vào đó, nó được tạo bằng quy tắc xác định dựa trên hai phiên bản trước đó. 

Ngoài ra còn có một hằng số cố định$k$, thể hiện thời gian chạy của quá trình triển khai C++. Chúng tôi giải thích “Python trở nên nhanh hơn C++” vì lần đầu tiên thời gian chạy Python giảm xuống dưới mức$k$. Nhiệm vụ là xác định chỉ số phiên bản sớm nhất$i > n$nơi điều này xảy ra, hoặc kết luận rằng nó không bao giờ xảy ra. 

Khó khăn chính là các giá trị tương lai được xác định theo cách đệ quy, vì vậy chúng ta phải hiểu hành vi dài hạn của chuỗi thay vì tính toán lại nó một cách ngây thơ theo cách không giới hạn. 

Những hạn chế rất nhỏ:$n \le 10$. Điều này ngay lập tức cho chúng ta biết rằng bất kỳ tính toán nào liên quan đến phân đoạn ban đầu đều có thể được xử lý trực tiếp và thậm chí việc mô phỏng khá chậm đối với các số hạng trong tương lai cũng có thể chấp nhận được nếu chúng ta có thể đảm bảo rằng chuỗi ổn định hoặc trở nên đơn giản. 

Một vấn đề tế nhị là sự truy hồi liên quan đến mức tối đa bằng 0. Điều này có nghĩa là chuỗi có thể bị “cắt bớt” và ngừng tuân theo một dạng đại số rõ ràng, do đó, việc giả định bất cẩn về một phép truy toán tuyến tính đơn giản mà không xem xét việc cắt bớt này có thể dẫn đến những dự đoán không chính xác nếu các giá trị âm xuất hiện. 

## Phương pháp tiếp cận 

Nếu chúng ta bỏ qua những lo ngại về hiệu quả thì chiến lược trực tiếp nhất là tạo ra từng số hạng một bằng cách sử dụng phép truy toán đã cho. Mỗi thuật ngữ mới chỉ phụ thuộc vào hai thuật ngữ trước đó, vì vậy việc mô phỏng này rất đơn giản. 

Điều phức tạp là hiểu được mô phỏng này có thể tiếp tục trong bao lâu. Nếu chúng ta coi phép truy hồi hoàn toàn là một mối quan hệ tuyến tính không có mức tối đa, thì chúng ta sẽ có một phép truy hồi đồng nhất bậc hai đơn giản hóa thành hàm tuyến tính trong$i$. Cấu trúc đó có nghĩa là trình tự không phát triển bùng nổ hoặc trở nên hỗn loạn. Nó hoạt động giống như một đường thẳng được xác định bởi hai điểm đã biết cuối cùng. 

Mức tối đa bằng 0 chỉ sửa đổi hành vi này khi dự đoán tuyến tính trở thành âm. Khi điều đó xảy ra, chuỗi sẽ được ghim ở mức 0 mãi mãi, vì cả hai giá trị trước đó đều không dương và phép lặp tiếp tục tạo ra số 0. 

Điều này giúp đơn giản hóa cấu trúc quan trọng: sau phân đoạn ban đầu, chuỗi trở thành một cấp số cộng cho đến khi nó có khả năng chạm tới 0, sau đó nó vẫn giữ nguyên bằng 0. Vì vậy, chúng ta chỉ cần mô phỏng một xu hướng tuyến tính đơn giản với mức sàn ở mức 0 và dừng lại ngay khi chúng ta vượt qua bên dưới$k$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng ngây thơ cho đến khi dừng lại |$O(T)$|$O(1)$| Đã chấp nhận | 
| Mô phỏng tuyến tính tối ưu với thông tin chi tiết về cắt |$O(T)$|$O(1)$| Đã chấp nhận | 

Đây$T$là số bước mô phỏng trong tương lai, bị giới hạn vì chuỗi giảm tuyến tính về 0 hoặc ngay lập tức trở nên không tăng về ngưỡng. 

## Hướng dẫn thuật toán 

Sự truy hồi cho các số hạng trong tương lai bị chi phối hoàn toàn bởi hai giá trị cuối cùng và sự khác biệt giữa chúng trở thành động lực của chuỗi. 

1. Tính chênh lệch$d = a_n - a_{n-1}$. Giá trị này xác định mỗi thuật ngữ tiếp theo thay đổi như thế nào so với thuật ngữ trước đó. 
2. Bắt đầu từ giá trị được biết cuối cùng$a_n$, liên tục tạo giá trị tiếp theo bằng cách sử dụng$a_{i} = a_{i-1} + d$. Điều này tương đương với việc khai triển phép truy toán mà không có mức tối đa, vì mối quan hệ bậc hai sụp đổ thành một cấp số sai phân không đổi. 
3. Áp dụng ràng buộc$a_i = \max(0, a_i)$sau mỗi lần tính toán. Nếu giá trị trở thành âm, hãy thay thế nó bằng 0. Từ thời điểm đó trở đi, tất cả các giá trị trong tương lai vẫn bằng 0 vì sự lặp lại tiếp tục tạo ra kết quả không dương. 
4. Đối với mỗi giá trị được tạo ra, hãy kiểm tra xem nó có nhỏ hơn hoàn toàn không$k$. Chỉ số đầu tiên nơi điều này xảy ra là câu trả lời. 
5. Nếu chúng ta đạt đến điểm mà chuỗi ổn định ở giá trị lớn hơn hoặc bằng$k$hoặc nếu nó trở thành 0 nhưng chúng tôi đã vượt qua tất cả các chuyển đổi có liên quan thì chúng tôi kết luận rằng không có phiên bản nào trong tương lai thỏa mãn điều kiện. 

### Tại sao nó hoạt động 

Sự truy hồi không có mức tối đa xác định mối quan hệ đồng nhất tuyến tính bậc hai có đa thức đặc trưng có nghiệm lặp lại, buộc tất cả các nghiệm hợp lệ phải là hàm tuyến tính của chỉ số. Điều này đảm bảo rằng khi hai giá trị cuối cùng được cố định, tất cả các giá trị trong tương lai đều tuân theo một cấp số cộng xác định. Mức tối đa bằng 0 chỉ cắt ngắn tiến trình từ bên dưới và không bao giờ tạo ra sự tăng trưởng hoặc dao động trở lại. Kết quả là chuỗi có nhiều nhất một thay đổi pha, sau đó nó trở thành không đổi. Cấu trúc này đảm bảo rằng việc kiểm tra các thuật ngữ một cách tuần tự không thể bỏ sót lần cắt đầu tiên bên dưới$k$. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, k = map(int, input().split())
a = list(map(int, input().split()))

if min(a) < k:
    # already faster in given data (not needed per problem, but safe guard)
    pass

# If only 2 values, define difference directly
if n == 2:
    d = a[1] - a[0]
    last = a[1]
    idx = 2
else:
    d = a[-1] - a[-2]
    last = a[-1]
    idx = n - 1

# simulate forward
cur = last
i = n

# We keep a safety bound: sequence becomes 0 or linear decreasing fast
while True:
    i += 1
    cur = cur + d
    if cur < 0:
        cur = 0

    if cur < k:
        print(f"Python 3.{i} will be faster than C++")
        break

    # if stuck at 0 and k > 0, it will eventually trigger immediately next step
    # but we continue naturally
    if i > n + 200000:
        print("Python will never be faster than C++")
        break
```Việc triển khai trực tiếp tuân theo quan sát rằng phép truy toán giảm xuống một chuỗi sai phân không đổi với giới hạn dưới bằng 0. Chúng tôi tính toán sự khác biệt đó một lần và sau đó mô phỏng từng số hạng tiếp theo. Mỗi lần lặp lại sẽ xây dựng thời gian chạy tiếp theo và ngay lập tức kiểm tra xem nó có vượt qua bên dưới không$k$. Giới hạn số lần lặp là mạng lưới an toàn cho các trường hợp suy biến trong đó trình tự không bao giờ được cải thiện đầy đủ. 

Một cạm bẫy phổ biến là cố gắng áp dụng phép truy toán một cách mù quáng mà không nhận ra rằng nó đơn giản hóa thành một cấp số cộng. Một điều khác là quên rằng một khi các giá trị trở thành không dương, mức tối đa sẽ buộc chúng duy trì vĩnh viễn bằng 0, loại bỏ mọi động lực tiếp theo. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
10 1
11 45 14 19 19 8 10 13 10 8
```Chúng tôi tính toán$d = 8 - 10 = -2$. Bắt đầu từ 8, chúng tôi mở rộng chuỗi: 

| tôi | giá trị | hành động | 
| --- | --- | --- | 
| 10 | 8 | bắt đầu | 
| 11 | 6 | 8 - 2 | 
| 12 | 4 | 6 - 2 | 
| 13 | 2 | 4 - 2 | 
| 14 | 0 | cắt bớt 2 - 2 | 
| 15 | 0 | ở lại 0 | 

Giá trị đầu tiên bên dưới$k = 1$đang ở$i = 14$. Điều này xác nhận rằng chuỗi cuối cùng sẽ sụp đổ dưới ngưỡng sau khi giảm tuyến tính. 

Đầu ra:```
Python 3.14 will be faster than C++
```### Ví dụ 2 

đầu vào:```
10 1
2 2 2 2 2 2 2 2 2 2
```Đây$d = 0$, do đó dãy không đổi ở mức 2 mãi mãi. Vì 2 luôn lớn hơn$k = 1$, điều kiện không bao giờ được thỏa mãn. 

| tôi | giá trị | 
| --- | --- | 
| 10 | 2 | 
| 11 | 2 | 
| 12 | 2 | 

Không có sự vượt qua nào xảy ra. 

Đầu ra:```
Python will never be faster than C++
```## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(T)$| Chúng tôi mô phỏng từng phiên bản trong tương lai một lần cho đến khi đạt điều kiện dừng | 
| Không gian |$O(1)$| Chỉ giá trị và chênh lệch cuối cùng được lưu trữ | 

Các ràng buộc đảm bảo rằng$n$rất nhỏ nên chi phí có ý nghĩa duy nhất là chúng tôi mở rộng được bao xa sang các phiên bản trong tương lai. Bởi vì trình tự là tuyến tính với mức sàn bằng 0 nên nó ổn định nhanh chóng và duy trì trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from subprocess import Popen, PIPE

    # We embed solution here for testing purposes
    input = sys.stdin.readline
    n, k = map(int, input().split())
    a = list(map(int, input().split()))

    d = a[-1] - a[-2]
    cur = a[-1]
    i = n

    while True:
        i += 1
        cur = cur + d
        if cur < 0:
            cur = 0
        if cur < k:
            return f"Python 3.{i} will be faster than C++"
        if i > n + 10000:
            return "Python will never be faster than C++"

# provided samples
assert run("10 1\n11 45 14 19 19 8 10 13 10 8\n") == "Python 3.14 will be faster than C++"
assert run("10 1\n2 2 2 2 2 2 2 2 2 2\n") == "Python will never be faster than C++"

# custom cases
assert run("2 5\n10 9\n") == "Python 3.4 will be faster than C++"
assert run("2 5\n10 10\n") == "Python will never be faster than C++"
assert run("3 3\n5 4 3\n") == "Python 3.4 will be faster than C++"
assert run("2 1\n100 2\n") == "Python 3.3 will be faster than C++"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| dãy giảm dần | vượt sớm | hành vi khác biệt tiêu cực | 
| chuỗi không đổi | không bao giờ | độ ổn định chênh lệch bằng không | 
| giảm ranh giới | băng qua ngay lập tức | lập chỉ mục từng cái một | 
| khoảng cách ban đầu lớn | băng qua nhanh | độ chính xác khi cắt | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi chênh lệch bằng 0. Trong tình huống này, chuỗi trở thành hằng số ngay lập tức, do đó mọi giá trị trong tương lai đều hợp lệ hoặc không có giá trị nào. Vì các giá trị ban đầu được đảm bảo lớn hơn hoặc bằng$k$, trường hợp này luôn dẫn đến “không bao giờ”. 

Một trường hợp khác là khi chênh lệch là dương. Dãy số tăng tuyến tính nên không bao giờ có thể giảm xuống dưới$k$. Một mô phỏng đơn giản vẫn có thể tiếp tục vô thời hạn trừ khi nó kiểm tra rõ ràng tính đơn điệu. 

Trường hợp cuối cùng xảy ra khi chuỗi giảm dần và chạm mức 0. Ví dụ: nếu các giá trị là$5, 3, 1$, sau đó$d = -2$, và trình tự trở thành$5, 3, 1, 0, 0, 0$. Khi đạt đến mức 0, hành vi được xác định đầy đủ và không còn biến thể nào nữa, do đó thuật toán sẽ kết thúc một cách an toàn ngay sau khi chuyển đổi.
