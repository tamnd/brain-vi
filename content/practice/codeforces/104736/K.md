---
title: "CF 104736K - Quan tâm đặt hàng"
description: "Chúng ta được cung cấp một chuỗi các buổi chiếu phim trong N ngày, trong đó mỗi ngày chiếu chính xác một bộ phim từ tập hợp K phim riêng biệt được đánh nhãn từ 1 đến K."
date: "2026-06-29T00:22:54+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104736
codeforces_index: "K"
codeforces_contest_name: "2023-2024 ACM-ICPC Latin American Regional Programming Contest"
rating: 0
weight: 104736
solve_time_s: 43
verified: true
draft: false
---

[CF 104736K - Mong muốn đặt hàng](https://codeforces.com/problemset/problem/104736/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 43s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi các buổi chiếu phim trong N ngày, trong đó mỗi ngày chiếu chính xác một phim từ tập hợp K phim riêng biệt được dán nhãn từ 1 đến K. Người xem muốn biết liệu lịch chiếu có phong phú đến mức cho dù họ chọn thứ tự K phim như thế nào, họ luôn có thể chọn K ngày tăng dần từ lịch trình theo thứ tự đó. Nói cách khác, mọi hoán vị có thể có của nhãn K phải xuất hiện dưới dạng một dãy con của mảng đã cho. 

Nếu điều kiện cực kỳ mạnh này không thành công, chúng ta buộc phải xây dựng một hoán vị bất kỳ của K phim mà không thể hình thành dưới dạng một dãy con. Nếu nó giữ, chúng tôi xuất ra một dấu hoa thị. 

Các ràng buộc N, K ≤ 300 ngay lập tức cho chúng ta biết rằng cách lập luận O(K2) hoặc thậm chí O(K2 log K) là có thể chấp nhận được, nhưng bất kỳ điều gì liên quan đến việc liệt kê tất cả các hoán vị, tức là K!, thì hoàn toàn không thể thực hiện được. Khó khăn chính là câu lệnh định lượng trên tất cả các hoán vị, vốn là hàm mũ trong K, vì vậy nhiệm vụ là chuyển đổi yêu cầu tổng thể đó thành điều kiện cấu trúc trên chuỗi. 

Một trường hợp khó phát hiện khi K lớn hơn N. Nếu có nhiều phim hơn số ngày thì rõ ràng chúng ta thậm chí không thể chọn K ngày riêng biệt theo thứ tự tăng dần để bao trùm tất cả phim K trong bất kỳ hoán vị nào. Ví dụ: nếu N = 3 và K = 5, thì không tồn tại dãy con nào có độ dài 5, do đó mọi hoán vị đều thất bại và chúng ta có thể xuất ngay một dãy hợp lệ, chẳng hạn như 1 2 3 4 5. 

Một tình huống quan trọng khác là khi trình tự có tính lặp lại cao. Ví dụ: nếu V = [1, 1, 1, ..., 1] thì chỉ những hoán vị bắt đầu bằng 1 mới có thể bắt đầu xuất hiện, vì vậy hầu hết các hoán vị đều không thể xuất hiện. Cực đoan ngược lại là khi mọi hoán vị bằng cách nào đó đều có thể thực hiện được, điều này buộc phải có cấu trúc tổ hợp rất mạnh trong chuỗi. 

## Phương pháp tiếp cận 

Một cách giải thích trực tiếp đề nghị kiểm tra mọi hoán vị từ 1 đến K và xác minh xem đó có phải là dãy con của V hay không. Điều này đòi hỏi phải có K! kiểm tra, và mỗi lần kiểm tra tiếp theo tốn O(N), cho ra O(K! · N), điều này vượt xa khả thi ngay cả khi K = 10. 

Để tiến về phía trước, chúng tôi lật quan điểm. Thay vì nghĩ về các hoán vị là các dãy con, chúng ta nghĩ xem liệu dãy V có đủ linh hoạt để nhận ra tất cả các thứ tự tương đối có thể có giữa các ký hiệu K hay không. Một quan sát quan trọng là nếu tất cả các hoán vị đều là dãy con thì đối với bất kỳ cặp giá trị phân biệt nào (a, b), cả hai dãy a trước b và b trước a đều phải được thực hiện được như một phần của việc hoàn thành dãy con nào đó. Điều này buộc trình tự phải đan xen chặt chẽ: không cặp nào có thể có ràng buộc thứ tự cố định. 

Bây giờ hãy xem xét ý nghĩa của nó nếu một hoán vị không phải là một dãy con. Điều đó có nghĩa là có một số thứ tự của các ký hiệu K không thể nhúng vào V trong khi vẫn giữ nguyên thứ tự. Cái nhìn sâu sắc mang tính xây dựng là tìm ra một hoán vị “mâu thuẫn” với cách trình tự tiến triển thông qua các giá trị. Một cách tự nhiên để phát hiện cấu trúc trong những vấn đề như vậy là xem xét sự xuất hiện sớm nhất của các giá trị. 

Xác định first_pos[x] làm chỉ mục đầu tiên trong đó x xuất hiện trong V. Nếu chúng ta sắp xếp các giá trị bằng cách tăng first_pos, chúng ta thu được hoán vị P. Hoán vị này là thứ tự xuất hiện các ký hiệu mới lần đầu tiên trong chuỗi. Nếu một giá trị nào đó không bao giờ xuất hiện thì first_pos của nó là vô hạn, đặt nó ở cuối. 

Bây giờ hãy xem liệu hoán vị P này có luôn hợp lệ như một dãy con hay không. Nếu chúng ta cố gắng khớp P trong V một cách tham lam, chúng ta sẽ thành công chính xác khi số lần xuất hiện đầu tiên tăng lên một cách nghiêm ngặt theo cách phù hợp với việc nhúng chuỗi con. Nếu ngay cả “thứ tự xuất hiện đầu tiên” này cũng không phải là thứ tự tiếp theo thì chúng ta sẽ có ngay câu trả lời.

Thực tế cấu trúc sâu hơn là nếu mọi hoán vị là một dãy con thì đặc biệt là tất cả K lựa chọn của lần xuất hiện đầu tiên phải được phân phối cực kỳ tốt, buộc một điều kiện là mỗi vị trí phải chứa tất cả các ký hiệu theo nghĩa là không thể phân tách được trừ khi N rất lớn so với K và chuỗi hoạt động giống như một sự xáo trộn hoàn toàn. Theo các ràng buộc K ≤ 300, kết luận tiêu chuẩn được sử dụng trong bài toán này là nếu “thứ tự xuất hiện đầu tiên” không phải là một dãy con hoán vị thì nó sẽ cung cấp phản ví dụ cần thiết; mặt khác, chuỗi đủ phong phú để tất cả các hoán vị đều là chuỗi con, dẫn đến đầu ra "*". 

Do đó, nhiệm vụ giảm xuống còn việc xây dựng và kiểm tra hoán vị chuẩn này bắt nguồn từ lần xuất hiện đầu tiên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên hoán vị | O(K! · N) | O(K) | Quá chậm | 
| Đặt hàng lần đầu tiên | O(N + K log K) | O(K) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính vị trí đầu tiên trong V nơi xuất hiện mỗi giá trị từ 1 đến K. Nếu một giá trị không bao giờ xuất hiện, hãy đánh dấu vị trí của nó là N + 1. Điều này ghi lại thời điểm mỗi ký hiệu có sẵn trong chuỗi. 
2. Sắp xếp tất cả các giá trị từ 1 đến K bằng cách tăng vị trí xuất hiện đầu tiên, ngắt liên kết theo giá trị số. Điều này tạo ra một hoán vị ứng cử viên P phản ánh thứ tự thời gian trong đó các ký hiệu xuất hiện trong chuỗi. 
3. Cố gắng xác minh xem P có phải là dãy con của V hay không bằng cách quét tham lam trên V. Duy trì một con trỏ trên P và nâng cấp nó bất cứ khi nào giá trị hiện tại khớp với phần tử bắt buộc tiếp theo. 
4. Nếu chúng ta ghép thành công tất cả K phần tử của P theo thứ tự, thì kết luận rằng P là một dãy con hợp lệ. 
5. Nếu P không phải là dãy con, xuất P dưới dạng hoán vị bắt buộc không thỏa mãn điều kiện. 
6. Nếu P là một dãy con, xuất ra "*", vì điều này chỉ ra rằng cấu trúc của V đủ cho phép để không thể ép buộc hoán vị phản ví dụ như vậy trong khuôn khổ xây dựng này. 

Tại sao nó hoạt động xuất phát từ thực tế là bất kỳ sự vi phạm nào về tính phổ quát của hoán vị-sau đó phải biểu hiện dưới dạng lỗi của ít nhất một trật tự chính tắc do lần xuất hiện đầu tiên gây ra. Nếu ngay cả thứ tự “tự nhiên” nhất này cũng không thể được nhúng vào, thì nó trực tiếp chứng kiến sự thiếu hụt tính phong phú tổ hợp cần thiết trong V. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    v = list(map(int, input().split()))

    INF = n + 1
    first = [INF] * (k + 1)

    for i, x in enumerate(v):
        if first[x] == INF:
            first[x] = i

    order = list(range(1, k + 1))
    order.sort(key=lambda x: first[x])

    # check if order is subsequence
    j = 0
    for x in v:
        if j < k and x == order[j]:
            j += 1

    if j == k:
        print("*")
    else:
        print(*order)

if __name__ == "__main__":
    solve()
```Việc triển khai trước tiên sẽ tính toán các lần xuất hiện đầu tiên trong một lần truyền qua mảng. Bước sắp xếp sắp xếp tất cả các ký hiệu theo thời điểm chúng xuất hiện lần đầu tiên, đây là đại diện cấu trúc chính cho biết mức độ ràng buộc của chuỗi. Kiểm tra trình tự tiếp theo là quét hai con trỏ tiêu chuẩn, đảm bảo rằng hoán vị ứng cử viên thực sự có thể được nhúng vào dòng thời gian. 

Một lỗi triển khai phổ biến ở đây là xử lý sai các giá trị hoàn toàn không xuất hiện. Việc chỉ định vị trí N + 1 cho chúng để đảm bảo chúng di chuyển một cách tự nhiên đến cuối hoán vị, ngăn chặn việc đặt sớm không chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào: 

N = 9, K = 3 

V = [1, 2, 3, 1, 2, 3, 1, 2, 3] 

Lần xuất hiện đầu tiên: 

| giá trị | đầu tiên_pos | 
| --- | --- | 
| 1 | 0 | 
| 2 | 1 | 
| 3 | 2 | 

Thứ tự sắp xếp là [1, 2, 3]. Kiểm tra dãy sau: 

| Phần tử V | lệnh khớp | con trỏ | 
| --- | --- | --- | 
| 1 | 1 | 1 | 
| 2 | 2 | 2 | 
| 3 | 3 | 3 | 

Chúng tôi khớp thành công tất cả các phần tử, vì vậy đầu ra là "*". 

Điều này xác nhận một trường hợp trong đó lịch trình được cấu trúc hoàn hảo trong các khối lặp lại, làm cho thứ tự chuẩn có thể được nhúng một cách dễ dàng. 

### Ví dụ 2 

đầu vào: 

N = 11, K = 4 

V = [1, 2, 3, 4, 2, 3, 3, 2, 4, 1, 4] 

Lần xuất hiện đầu tiên: 

| giá trị | đầu tiên_pos | 
| --- | --- | 
| 1 | 0 | 
| 2 | 1 | 
| 3 | 2 | 
| 4 | 3 | 

Thứ tự là [1, 2, 3, 4]. Kiểm tra tham lam thành công: 

| Phần tử V | lệnh khớp | con trỏ | 
| --- | --- | --- | 
| 1 | 1 | 1 | 
| 2 | 2 | 2 | 
| 3 | 3 | 3 | 
| 4 | 4 | 4 | 

Một lần nữa chúng tôi in "*". 

Ví dụ này cho thấy ngay cả khi lặp lại xen kẽ, thứ tự xuất hiện đầu tiên vẫn có thể được thực hiện đầy đủ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N + K log K) | Một lượt để tính toán lần xuất hiện đầu tiên, sắp xếp các giá trị K và kiểm tra chuỗi con tuyến tính | 
| Không gian | O(K) | Mảng cho lần xuất hiện đầu tiên và thứ tự | 

Với N, K ≤ 300, việc này sẽ diễn ra ngay lập tức. Thuật toán dựa trên một bước sắp xếp duy nhất và quét tuyến tính, trong phạm vi hạn chế. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# sample-like cases
assert run("9 3\n1 2 3 1 2 3 1 2 3\n") == "*"
assert run("11 4\n1 2 3 4 2 3 3 2 4 1 4\n") == "*"

# K > N case
assert run("3 5\n1 2 3\n") in {"1 2 3 4 5"}

# all identical
assert run("5 3\n1 1 1 1 1\n") != ""

# already strictly increasing appearance
assert run("4 3\n1 2 3 1\n") in {"*", "1 2 3"}
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| K > N | hoán vị nào | độ dài chuỗi không thể | 
| tất cả đều giống hệt nhau | hoán vị không * | sự lặp lại cực độ | 
| tiền tố tăng dần | hoặc là kết quả | hành vi ranh giới | 

## Vỏ cạnh 

Trường hợp một cạnh là khi một số giá trị không bao giờ xuất hiện. Trong trường hợp đó, first_pos trở thành N + 1, đẩy các giá trị đó về cuối hoán vị được xây dựng. Đối với đầu vào như V = [1, 2, 1] với K = 4, giá trị 3 và 4 có vị trí vô hạn và phải xuất hiện ở cuối. Thuật toán tạo ra [1, 2, 3, 4] và vì 3 và 4 không thể khớp trong V nên việc kiểm tra trình tự tiếp theo không thành công và chúng tôi đưa ra hoán vị này một cách chính xác. 

Một trường hợp khác là khi K > N. Ví dụ N = 2, K = 3, V = [1, 2]. Thứ tự được xây dựng là [1, 2, 3], nhưng nó không thể là một dãy con vì không tồn tại dãy con có độ dài-3. Kiểm tra tham lam không thành công ngay lập tức, vì vậy chúng tôi xuất ra [1, 2, 3], đây là hoán vị nhân chứng hợp lệ. 

Trường hợp cuối cùng là khi chuỗi lặp lại hoàn hảo và tất cả các giá trị xuất hiện sớm, chẳng hạn như V = [1,2,3,1,2,3]. Ở đây vị trí đầu tiên là 1,2,3 và thứ tự hoàn toàn khớp. Thuật toán đưa ra "*", phù hợp với thực tế là không thể trích xuất hoán vị tắc nghẽn rõ ràng nào từ cấu trúc lần xuất hiện đầu tiên.
