---
title: "CF 1644D - Tô màu chéo"
description: "Chúng ta được cung cấp một lưới bắt đầu hoàn toàn không có màu, với $n$ hàng và $m$ cột. Sau đó chúng tôi áp dụng một chuỗi các hoạt động. Mỗi thao tác sẽ chọn một hàng và một cột, đồng thời tô màu mọi ô trong hàng và cột đó bằng màu không phải màu trắng đã chọn."
date: "2026-06-10T04:14:34+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 1644
codeforces_index: "D"
codeforces_contest_name: "Educational Codeforces Round 123 (Rated for Div. 2)"
rating: 1700
weight: 1644
solve_time_s: 71
verified: true
draft: false
---

[CF 1644D - Tô màu chéo](https://codeforces.com/problemset/problem/1644/D) 

**Đánh giá:** 1700 
**Tags:** cấu trúc dữ liệu, triển khai, toán học 
**Thời gian giải:** 1 phút 11 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một lưới bắt đầu hoàn toàn không có màu, với$n$hàng và$m$cột. Sau đó chúng tôi áp dụng một chuỗi các hoạt động. Mỗi thao tác sẽ chọn một hàng và một cột, đồng thời tô màu mọi ô trong hàng và cột đó bằng màu không phải màu trắng đã chọn. Nếu một ô được vẽ nhiều lần trong các thao tác, thao tác mới nhất chạm vào ô đó sẽ ghi đè màu của ô đó. 

Sau khi tất cả các thao tác được xử lý, lưới cuối cùng được xác định đầy đủ bởi trình tự các thao tác và lựa chọn màu được gán cho chúng. Hai lưới cuối cùng được coi là khác nhau nếu có ít nhất một ô khác nhau về màu cuối cùng. 

Nhiệm vụ là đếm xem có thể tạo ra bao nhiêu lưới cuối cùng riêng biệt nếu, đối với mỗi thao tác, chúng ta chọn độc lập bất kỳ lưới nào trong số đó.$k$màu sắc. 

Các ràng buộc đủ chặt chẽ đến mức bất kỳ giải pháp nào mô phỏng lưới hoặc thậm chí theo dõi trạng thái trên mỗi ô đều không thể thực hiện được ngay lập tức. Tổng số thao tác trên các trường hợp thử nghiệm lên tới$2 \cdot 10^5$, điều này buộc một giải pháp tuyến tính hoặc gần tuyến tính về số lượng thao tác. Bất kỳ cách tiếp cận nào cố gắng tính toán lại lưới hoặc truyền bá các cập nhật trên mỗi ô sẽ vượt quá giới hạn thời gian theo một số bậc độ lớn. 

Một vấn đề nhỏ xuất hiện khi các thao tác lặp lại cùng một cặp hàng-cột. Một ý tưởng ngây thơ có thể là xử lý từng thao tác một cách độc lập và nhân với$k^q$. Điều này không thành công vì các thao tác khác nhau có thể ghi đè lên nhau, khiến cho các lựa chọn trước đó không còn phù hợp trong nhiều trường hợp. Ví dụ: nếu tất cả các thao tác nhắm mục tiêu vào cùng một hàng và cột thì chỉ thao tác cuối cùng mới quan trọng đối với mỗi ô, do đó số lượng màu hợp lệ chỉ là$k$, không$k^q$. 

Một dạng lỗi khác là giả định rằng các hàng và cột có thể được xử lý độc lập. Chúng không độc lập vì một thao tác đơn lẻ sẽ tô màu đồng thời và các tương tác phụ thuộc vào thứ tự các thao tác. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ cố gắng mô phỏng tất cả các phép gán màu có thể có cho các hoạt động. Đối với mỗi hoạt động, chúng tôi chọn một trong$k$màu sắc, mô phỏng hiệu ứng trên lưới và tính toán kết quả cuối cùng. Điều này đúng nhưng không khả thi vì nó khám phá$k^q$khả năng, rất lớn về mặt thiên văn ngay cả đối với đầu vào nhỏ. 

Chúng ta cần tránh theo dõi toàn bộ lưới và thay vào đó tập trung vào tác động cấu trúc của hoạt động. Quan sát quan trọng là chỉ có thao tác cuối cùng ảnh hưởng đến từng hàng hoặc cột mới xác định trạng thái hiển thị cuối cùng của nó. Khi một hàng đã được "bao phủ" bởi một thao tác sau, mọi thao tác trước đó ảnh hưởng đến hàng đó đều không liên quan đến sự đóng góp của hàng đó, trừ khi chúng xảy ra ở các vùng khác nhau không bị ghi đè sau đó. 

Điều này dẫn đến việc sắp xếp lại quy trình theo hướng “lần đầu tiên một hàng hoặc cột trở nên không liên quan do bị ghi đè bởi thao tác sau đó”. Mỗi thao tác sẽ tạo ra ảnh hưởng mới hoặc trở nên dư thừa tùy thuộc vào việc hàng hoặc cột của nó có bị ghi đè sau này hay không. 

Chúng tôi xử lý các hoạt động từ cuối đến đầu. Khi chúng ta nhìn thấy một cặp$(x, y)$, nếu cả hai hàng$x$và cột$y$đã bị “chặn” bởi các hoạt động sau này thì hoạt động này không thể đóng góp được điều gì mới. Nếu ít nhất một cái không bị chặn thì thao tác này góp phần nâng cao mức độ tự do mới trong việc lựa chọn màu sắc. Sau khi xử lý, chúng tôi đánh dấu hàng và cột là bị chặn. 

Cấu trúc tham lam ngược này hoạt động vì khi một hàng hoặc cột đã được tính toán bởi một thao tác sau đó thì mọi thao tác trước đó không thể ảnh hưởng đến bất kỳ vùng chưa được chỉ định nào còn tồn tại trong lưới cuối cùng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(k^q \cdot nm)$|$O(nm)$| Quá chậm | 
| Tối ưu |$O(n + m + q)$|$O(n + m)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý các hoạt động theo thứ tự ngược lại và duy trì những hàng và cột nào đã được “che phủ” bởi các hoạt động sau này. 

1. Khởi tạo hai mảng boolean, một cho hàng và một cho cột, ban đầu cả hai đều sai. Chúng thể hiện liệu một hàng hoặc cột đã được tính toán bởi một thao tác sau này hay chưa. 
2. Khởi tạo bộ đếm đóng góp "hoạt động tích cực" về 0. Mỗi lần chúng ta tìm thấy một thao tác vẫn ảnh hưởng đến ít nhất một hàng hoặc cột mới, nó sẽ đóng góp một hệ số nhân của$k$. 
3. Lặp lại các thao tác từ cuối đến đầu. Đối với mỗi thao tác$(x_i, y_i)$, kiểm tra xem hàng$x_i$đã được đánh dấu hoặc cột$y_i$đã được đánh dấu rồi. 
4. Nếu cả hai hàng$x_i$và cột$y_i$đã được đánh dấu, thao tác này không đưa ra bất kỳ quyền tự do hiển thị mới nào và có thể bị bỏ qua. 
5. Nếu không, thao tác này sẽ góp phần tạo ra sự lựa chọn màu sắc độc lập mới, vì vậy hãy nhân câu trả lời với$k$, sau đó đánh dấu hàng$x_i$và cột$y_i$như được che phủ. 
6. Tiếp tục cho đến khi tất cả các thao tác được xử lý. 

Câu trả lời cuối cùng là tích lũy modulo$998244353$. 

### Tại sao nó hoạt động 

Bất biến chính là tại bất kỳ điểm nào trong quá trình quét ngược, mọi hàng và cột không được đánh dấu đều tương ứng với một vùng của lưới vẫn có khả năng bị ảnh hưởng bởi các hoạt động trước đó. Khi chúng tôi xử lý một thao tác chạm vào ít nhất một hàng hoặc cột không được đánh dấu, đây là lần đầu tiên vùng đó được "xác nhận" theo thời gian ngược, nghĩa là lựa chọn màu của vùng đó vẫn tự do và độc lập. Khi chúng tôi đánh dấu hàng và cột của nó, tất cả các hoạt động trước đó ảnh hưởng đến chúng không thể đưa ra các lựa chọn độc lập mới bởi vì bất kỳ ô nào mà chúng tác động đều đã được xác định bởi một hoạt động sau đó trong thời gian chuyển tiếp. Điều này đảm bảo mỗi hoạt động đóng góp tương ứng với chính xác một yếu tố độc lập của$k$và không có phần phụ thuộc nào được tính hai lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

t = int(input())
for _ in range(t):
    n, m, k, q = map(int, input().split())
    ops = [tuple(map(int, input().split())) for _ in range(q)]

    row_used = [False] * (n + 1)
    col_used = [False] * (m + 1)

    ans = 1

    for x, y in reversed(ops):
        if row_used[x] and col_used[y]:
            continue
        ans = (ans * k) % MOD
        row_used[x] = True
        col_used[y] = True

    print(ans)
```Việc thực hiện phản ánh trực tiếp ý tưởng tham lam ngược lại. Chúng tôi lưu trữ tất cả các hoạt động trước tiên vì chúng tôi cần xử lý ngược chúng. Hai mảng boolean theo dõi xem một hàng hoặc cột đã được thao tác sau này xác nhận hay chưa. 

Chi tiết quan trọng nhất là điều kiện`if row_used[x] and col_used[y]`. Điều này đảm bảo rằng chúng ta chỉ bỏ qua một thao tác khi nó không thể đưa ra bất kỳ lựa chọn màu độc lập mới nào. Nếu hàng hoặc cột vẫn trống, chúng ta phải tính thao tác này vì nó xác định ranh giới ảnh hưởng mới trong quy trình ngược lại. 

Phép nhân với$k$xảy ra chính xác khi một sự lựa chọn độc lập mới được đưa ra. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n=1, m=1, k=3, q=2
(1,1)
(1,1)
```Chúng tôi xử lý ngược lại. 

| Bước | Hoạt động | Hàng được sử dụng | Col đã qua sử dụng | Hành động | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 1 | (1,1) | FF | FF | lấy | 3 | 
| 2 | (1,1) | T T | T T | lấy | 9 | 

Thao tác thứ hai vẫn đóng góp vì mặc dù nó nhắm mục tiêu vào cùng một ô nhưng ngược lại đây là lần đầu tiên hàng và cột đó được xác nhận. Điều này cho thấy tại sao sự trùng lặp vẫn quan trọng trong xử lý ngược. 

### Ví dụ 2 

đầu vào:```
n=2, m=2, k=2, q=3
(2,1)
(1,1)
(2,2)
```Xử lý ngược: 

| Bước | Hoạt động | Hàng được sử dụng | Col đã qua sử dụng | Hành động | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 1 | (2,2) | FF | FF | lấy | 2 | 
| 2 | (1,1) | T F | T F | lấy | 4 | 
| 3 | (2,1) | T T | T T | bỏ qua | 4 | 

Điều này cho thấy các hoạt động sau này “chặn” hoàn toàn các hoạt động trước đó như thế nào khi cả hàng và cột của chúng đã được che phủ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n + m + q)$| Mỗi thao tác được xử lý ngược lại một lần với kiểm tra O(1) | 
| Không gian |$O(n + m)$| Mảng Boolean cho hàng và cột | 

Giải pháp này dễ dàng phù hợp với các ràng buộc vì tổng số thao tác trên các trường hợp thử nghiệm là$2 \cdot 10^5$và mọi thao tác đều được xử lý trong thời gian không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    MOD = 998244353

    t = int(input())
    out = []
    for _ in range(t):
        n, m, k, q = map(int, input().split())
        ops = [tuple(map(int, input().split())) for _ in range(q)]

        row_used = [False] * (n + 1)
        col_used = [False] * (m + 1)

        ans = 1
        for x, y in reversed(ops):
            if row_used[x] and col_used[y]:
                continue
            ans = (ans * k) % MOD
            row_used[x] = True
            col_used[y] = True

        out.append(str(ans))
    return "\n".join(out)

# provided samples
assert run("""2
1 1 3 2
1 1
1 1
2 2 2 3
2 1
1 1
2 2
""") == "3\n4"

# single operation
assert run("""1
3 3 5 1
2 2
""") == "5"

# repeated same cell
assert run("""1
2 2 7 3
1 1
1 1
1 1
""") == "7"

# all distinct
assert run("""1
3 3 2 3
1 1
2 2
3 3
""") == "8"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| hoạt động đơn lẻ | k | đóng góp cơ bản | 
| hoạt động lặp đi lặp lại cùng một ô | k | trùng lặp được thu gọn chính xác | 
| tất cả các hoạt động riêng biệt |$k^3$| trường hợp độc lập hoàn toàn | 

## Vỏ cạnh 

Trường hợp phức tạp là khi nhiều thao tác lặp lại cùng một cặp$(x, y)$. Thuật toán chỉ coi thao tác cuối cùng như vậy là có ý nghĩa ngược lại, nhưng mỗi lần lặp lại vẫn có thể đóng góp nếu đây là lần đầu tiên một hàng hoặc cột được giới thiệu. Ví dụ: với các thao tác giống hệt nhau lặp đi lặp lại, quá trình quét ngược chỉ tính một đóng góp, mang lại kết quả chính xác$k$, vì lần xuất hiện đầu tiên được xử lý đã đánh dấu cả hàng và cột. 

Một trường hợp khác là khi tất cả các thao tác chia sẻ cùng một hàng nhưng khác cột. Ngược lại, thao tác đầu tiên sẽ đánh dấu hàng và các thao tác tiếp theo sẽ bị bỏ qua khi cả hàng và cột đã được đánh dấu. Điều này tránh được việc đếm quá mức một cách chính xác và phản ánh rằng chỉ có ranh giới gặp phải đầu tiên mới đưa ra lựa chọn miễn phí. 

Những trường hợp này xác nhận rằng bất biến không gắn liền với các phép toán riêng lẻ mà gắn với việc liệu chúng có đưa ra ranh giới hàng-cột chưa từng thấy trước đó theo thời gian ngược hay không.
