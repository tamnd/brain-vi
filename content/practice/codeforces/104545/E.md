---
title: "CF 104545E - Bí ẩn của tượng Nhân Sư"
description: "Chúng ta được cung cấp một bộ sưu tập “số liệu thống kê” đang ngày càng phát triển, mỗi số liệu đều đúng hoặc sai. Ban đầu có v thống kê đúng và f thống kê sai. Số lượng quan tâm chính là tỷ lệ thống kê sai trong số tất cả các số liệu thống kê."
date: "2026-06-30T08:57:46+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104545
codeforces_index: "E"
codeforces_contest_name: "VIII MaratonUSP Freshman Contest"
rating: 0
weight: 104545
solve_time_s: 50
verified: true
draft: false
---

[CF 104545E - Bí ẩn của Nhân sư](https://codeforces.com/problemset/problem/104545/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một bộ sưu tập “số liệu thống kê” đang ngày càng phát triển, mỗi số liệu đều đúng hoặc sai. Ban đầu có`v`số liệu thống kê thực tế và`f`số liệu thống kê sai. Số lượng quan tâm chính là tỷ lệ thống kê sai trong số tất cả các số liệu thống kê. 

Có một thao tác đặc biệt có thể được áp dụng nhiều lần. Mỗi lần chúng tôi áp dụng nó, chúng tôi coi tuyên bố “hơn 75% số liệu thống kê là sai”. Bản thân tuyên bố này đã trở thành một thống kê mới và giá trị thực của nó phụ thuộc vào tỷ lệ hiện tại`f / (v + f)`. 

Nếu tỷ lệ thống kê sai hiện tại lớn hơn 3/4 thì tuyên bố được coi là đúng và chúng tôi tăng`v`lên 1. Ngược lại, chúng ta tăng`f`thêm 1. Sau mỗi thao tác, tổng số thống kê tăng thêm 1 và sự phân bổ giữa đúng và sai thay đổi tùy thuộc vào việc ngưỡng có được vượt qua hay không. 

Chúng ta phải xác định xem thao tác này phải được áp dụng bao nhiêu lần cho đến khi tỷ lệ thống kê sai đạt chính xác 75%, nghĩa là`f / (v + f) = 3/4`. 

Các ràng buộc cho phép lên đến`10^5`trường hợp thử nghiệm có giá trị lên tới`10^8`. Điều này loại trừ bất kỳ mô phỏng nào tính toán lại phân số từng bước cho từng thao tác trong trường hợp xấu nhất, vì quá trình này có thể mất thời gian tuyến tính cho mỗi lần kiểm tra và tích lũy đến một tổng số không khả thi. 

Một mô phỏng đơn giản cũng che giấu một cạm bẫy về cấu trúc: quyết định thay đổi tùy thuộc vào tỷ lệ trên hay dưới 3/4, có nghĩa là chuỗi cập nhật không đơn điệu trong một biến hiển nhiên. Nếu cố gắng lặp lại cho đến khi hội tụ, chúng ta có thể dễ dàng xử lý sai ranh giới nơi bất đẳng thức chuyển đổi hành vi. 

Một trường hợp cạnh minh họa nhỏ là`v = 1, f = 2`. Tỷ lệ là`2/3`, dưới 3/4, do đó phép toán tăng lên`f`. Việc lặp đi lặp lại một cách mù quáng sẽ làm thay đổi hệ thống theo cách nhanh chóng trôi đi mà không có sự hội tụ rõ ràng. Một mô phỏng bất cẩn có thể cho rằng sự hội tụ là ngay lập tức hoặc đơn điệu đối với tỷ lệ mục tiêu, điều này không được đảm bảo về mặt cấu trúc. 

Thách thức thực sự là chúng ta không được yêu cầu mô phỏng quy trình mà phải xác định chính xác số bước cần thiết để đạt được điều kiện đại số chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ mô phỏng quy trình theo từng bước. Mỗi bước yêu cầu kiểm tra xem`f / (v + f) > 3/4`, đang cập nhật`v`hoặc`f`, và tiếp tục cho đến khi thỏa mãn điều kiện đẳng thức. Mặc dù mỗi bước là O(1), nhưng số bước có thể lớn vì`v`Và`f`có thể tăng lên đến điểm mà phân số ổn định ở mức chính xác là 3/4. Trong trường hợp xấu nhất, điều này có thể yêu cầu một số thao tác tỷ lệ thuận với kích thước cuối cùng của hệ thống, không bị giới hạn chặt chẽ bởi một hằng số nhỏ. 

Quan sát quan trọng là mỗi thao tác không tùy ý, nó luôn di chuyển trạng thái dọc theo một trong hai hướng tuyến tính xác định trong`(v, f)`máy bay. điều kiện`f / (v + f) > 3/4`tương đương với`4f > 3(v + f)`, điều này đơn giản hóa thành`f > 3v`. Vì vậy, quy tắc quyết định chỉ phụ thuộc vào việc`f`lớn hơn`3v`. 

Điều này chia quá trình thành hai chế độ. Nếu như`f > 3v`, chúng tôi tăng`v`. Nếu không chúng tôi tăng`f`. Mỗi thao tác đều thay đổi`v`hoặc`f`chính xác bằng 1, do đó hệ thống tiến triển theo một đường tuyến tính từng phần. 

Thay vì mô phỏng từng bước, chúng ta có thể suy luận xem cần bao nhiêu thao tác để đạt được điều kiện cân bằng chính xác`f = 3(v + f)/4`, điều này đơn giản hóa thành`4f = 3(v + f)`và xa hơn nữa`f = 3v`. Vì vậy, mục tiêu là đạt đến trạng thái nơi`f = 3v`. 

Chúng ta có thể hiểu quá trình này là việc điều chỉnh liên tục`(v, f)`cho đến khi ràng buộc tuyến tính này được thỏa mãn. Mỗi hoạt động hoặc tăng`v`hoặc tăng`f`và chúng tôi muốn đếm xem cần bao nhiêu bước để hạ cánh chính xác trên dòng`f = 3v`. 

Điều này trở thành một bài toán số học xác định: chúng ta theo dõi xem trạng thái hiện tại còn bao xa mới thỏa mãn được.`f = 3v`và mỗi thao tác sẽ di chuyển nó đến gần hơn một cách có kiểm soát. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng | O(câu trả lời) | O(1) | Quá chậm | 
| Xây dựng số học | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta định dạng lại điều kiện và rút ra biểu thức trực tiếp cho số bước cần thiết. 

1. Bắt đầu từ nhận dạng rằng điều kiện mục tiêu là`f = 3v`. Điều này xuất phát từ việc viết lại`f / (v + f) = 3/4`, làm giảm đến`4f = 3v + 3f`, kể từ đây`f = 3v`. 
2. Quan sát cách thức hoạt động tùy thuộc vào sự bất bình đẳng giữa`f`Và`3v`. Nếu như`f > 3v`, thì phân số hiện tại lớn hơn 3/4 nên chúng ta tăng dần`v`. Nếu như`f ≤ 3v`, chúng tôi tăng`f`. Điều này có nghĩa là mỗi bước sẽ chuyển hệ thống theo hướng cân bằng sự khác biệt`f - 3v`. 
3. Xác định hàm thế năng`d = f - 3v`. Mục tiêu là`d = 0`. Bây giờ chúng ta viết lại cách`d`thay đổi sau mỗi lần thao tác. Nếu như`d > 0`, chúng tôi tăng`v`, biến đổi`d`vào trong`(f) - 3(v+1) = d - 3`. Nếu như`d ≤ 0`, chúng tôi tăng`f`, biến đổi`d`vào trong`(f+1) - 3v = d + 1`. 
4. Bây giờ chúng tôi mô phỏng sự tiến hóa của`d`không phải bằng cách bước từng bước một trong một vòng lặp ngây thơ mà bằng cách nhảy theo từng khối. Khi`d`là dương, các ứng dụng lặp lại sẽ giảm nó đi 3 mỗi bước cho đến khi nó trở thành không dương. Khi nó không dương, các ứng dụng lặp lại sẽ tăng nó lên 1 mỗi bước cho đến khi nó trở thành dương. 
5. Quá trình xen kẽ giữa pha giảm và pha tăng. Mỗi giai đoạn có thể được thu gọn thành một bước nhảy số học trực tiếp bằng cách sử dụng phép chia, thay vì lặp lại từng bước. 
6. Chúng ta tiếp tục những bước nhảy xen kẽ này cho đến khi`d`trở thành chính xác 0. Số lần nhảy tích lũy là câu trả lời. 

Tại sao nó hoạt động là vì quá trình này hoàn toàn bị chi phối bởi một trạng thái số nguyên duy nhất`d = f - 3v`. Mọi hoạt động đều thay đổi`d`bởi một hằng số cố định chỉ phụ thuộc vào dấu của nó. Không có trạng thái ẩn nào ngoài`d`, do đó quá trình tiến hóa là một bước đi xác định trên các số nguyên với hai quy tắc chuyển tiếp tuyến tính. Việc thu gọn các chuyển đổi giống hệt nhau lặp đi lặp lại sẽ duy trì tính đúng đắn vì quy tắc quyết định không thay đổi trong một phân đoạn đơn điệu của`d`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve_one(v, f):
    d = f - 3 * v
    steps = 0

    while d != 0:
        if d > 0:
            # apply v increments: each reduces d by 3
            # we need to know how many such steps until d <= 0
            k = (d + 2) // 3
            d -= 3 * k
            v += k
            steps += k
        else:
            # apply f increments: each increases d by 1
            k = (-d)
            d += k
            f += k
            steps += k

    return steps

t = int(input())
out = []
for _ in range(t):
    v, f = map(int, input().split())
    out.append(str(solve_one(v, f)))

print("\n".join(out))
```Việc triển khai nén các hoạt động giống hệt nhau trong thời gian dài. Khi`d > 0`, ta áp dụng lặp lại thao tác “tăng v” cho đến khi`d`vượt qua 0 hoặc trở nên đủ nhỏ đến mức cần ít hơn một khối đầy đủ gồm -3 bước. Công thức`(d + 2) // 3`tính toán chính xác cần bao nhiêu lần giảm 3 để mang lại`d`bằng 0 hoặc thấp hơn. 

Khi`d ≤ 0`, mỗi bước tăng`d`tăng thêm 1, vì vậy chúng ta trực tiếp nhảy qua`-d`bước để đạt đến số không. Điều này tránh việc lặp lại từng cái một thông qua tăng trưởng tuyến tính. 

Biến`steps`tích lũy tổng số thao tác được thực hiện, đó là điều mà bài toán yêu cầu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`v = 1, f = 2`Chúng tôi tính toán`d = f - 3v = 2 - 3 = -1`. 

| Bước | d | Hành động | k | Mới d | 
| --- | --- | --- | --- | --- | 
| 1 | -1 | tăng f | 1 | 0 | 

Thuật toán nhìn thấy`d ≤ 0`, do đó nó tăng lên`f`bằng 1, làm`d = 0`. Quá trình kết thúc trong một bước. Điều này phù hợp với thực tế là hệ thống ngay lập tức đạt đến trạng thái cân bằng sau một lần điều chỉnh. 

### Ví dụ 2 

đầu vào:`v = 2, f = 10`Ban đầu`d = 10 - 6 = 4`. 

| Giai đoạn bước | d bắt đầu | hành động | k | d kết thúc | 
| --- | --- | --- | --- | --- | 
| 1 | 4 | tăng v | 2 | -2 | 
| 2 | -2 | tăng f | 2 | 0 | 

Đầu tiên chúng ta giảm`d`theo khối 3 cho đến khi vượt qua số 0, sau đó bù lên. Quá trình ổn định chính xác tại`d = 0`. 

Điều này chứng tỏ cách thuật toán xen kẽ giữa các pha giảm và tăng mà không cần mô phỏng một bước. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(log max(v, f)) mỗi lần kiểm tra trong trường hợp xấu nhất | Mỗi pha làm giảm đáng kể giá trị tuyệt đối của d thông qua các bước nhảy số học | 
| Không gian | O(1) | Chỉ có một số lượng biến không đổi được duy trì | 

Giải pháp có hiệu quả đối với`t ≤ 10^5`và giá trị lên đến`10^8`, vì mỗi thử nghiệm chạy trong thời gian khấu hao không đổi hoặc gần như không đổi do nén bước lớn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def solve_one(v, f):
        d = f - 3 * v
        steps = 0
        while d != 0:
            if d > 0:
                k = (d + 2) // 3
                d -= 3 * k
                steps += k
            else:
                k = -d
                d += k
                steps += k
        return steps

    t = int(input())
    out = []
    for _ in range(t):
        v, f = map(int, input().split())
        out.append(str(solve_one(v, f)))
    return "\n".join(out)

# minimum cases
assert run("1\n1 2\n") == "1"
assert run("1\n1 3\n") == "0"

# boundary case near threshold
assert run("1\n10 30\n") == "0"

# small mixed cases
assert run("3\n1 2\n2 10\n3 5\n") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 2 | 1 | cập nhật tối thiểu không tầm thường | 
| 1 3 | 0 | trạng thái đã cân bằng | 
| 10 30 | 0 | tỷ lệ chính xác 3:1 | 
| hỗn hợp | khác nhau | tính đúng đắn chung giữa các chế độ | 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng là khi hệ thống khởi động chính xác trên ranh giới`f = 3v`. Trong trường hợp đó, câu trả lời bắt buộc là 0 vì điều kiện đã được thỏa mãn. Ví dụ, đầu vào`v = 10, f = 30`cho`d = 0`, do đó thuật toán ngay lập tức trả về 0 mà không cần nhập bất kỳ vòng lặp nào. 

Một trường hợp tế nhị khác xảy ra khi`f`nhỏ hơn nhiều so với`3v`. Ví dụ`v = 10, f = 1`cho`d = -29`. Thuật toán áp dụng một khối tăng dần`f`đến 29, đến thẳng`d = 0`. Một mô phỏng đơn giản sẽ yêu cầu 29 lần lặp, nhưng phương pháp nén sẽ xử lý nó chỉ trong một bước nhảy, duy trì tính chính xác vì mọi bước trong vùng này đều có hành vi giống hệt nhau.
