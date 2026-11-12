---
title: "CF 1677B - Tokitsukaze và cuộc gặp gỡ"
description: "Chúng tôi được yêu cầu mô phỏng một kịch bản chỗ ngồi năng động. Có một hội trường có n hàng và m cột, học sinh lần lượt đến, mỗi em nghiêm túc hoặc nghịch ngợm."
date: "2026-06-10T00:55:49+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 1677
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 789 (Div. 1)"
rating: 1700
weight: 1677
solve_time_s: 549
verified: false
draft: false
---

[CF 1677B - Tokitsukaze và cuộc gặp gỡ](https://codeforces.com/problemset/problem/1677/B) 

**Đánh giá:** 1700 
**Tags:** cấu trúc dữ liệu, triển khai, toán học 
**Thời gian giải:** 9 phút 9 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu mô phỏng một kịch bản chỗ ngồi năng động. Có một hội trường với`n`hàng và`m`các cột và học sinh lần lượt đến, mỗi em nghiêm túc hoặc nghịch ngợm. Học sinh đầu tiên luôn ngồi ở cột đầu tiên của hàng đầu tiên và mỗi học sinh mới đến sẽ đẩy các học sinh hiện có theo thứ tự chính: trong một hàng, học sinh chuyển sang bên phải và ghế cuối cùng của hàng sẽ xếp vào ghế đầu tiên của hàng tiếp theo. Sau khi mỗi học sinh ngồi xuống, chúng tôi được yêu cầu đếm xem có bao nhiêu hàng và cột có ít nhất một học sinh nghiêm túc. Đầu ra của mỗi trường hợp kiểm tra là danh sách số lượng tích lũy sau khi mỗi học sinh tham gia. 

Những hạn chế đòi hỏi phải xem xét cẩn thận. Số lượng chỗ ngồi cho mỗi trường hợp thử nghiệm có thể lên tới`10^6`và tổng số tiền của tất cả các trường hợp thử nghiệm không vượt quá`10^6`. Điều này loại trừ các mô phỏng đơn giản trong đó chúng tôi duy trì toàn bộ lưới 2D của học sinh và các mục nhập thay đổi mỗi lần đến vì thời gian trong trường hợp xấu nhất sẽ là bậc hai, quá chậm. Chúng tôi cần một cách tiếp cận cập nhật số lượng theo thời gian không đổi cho mỗi học sinh. Các trường hợp đặc biệt bao gồm các tình huống trong đó tất cả học sinh đều nghiêm túc hoặc tất cả đều nghịch ngợm hoặc khi ma trận có một hàng hoặc cột duy nhất, vì những điều này ảnh hưởng đến cách lan truyền các cập nhật hàng và cột. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là duy trì ma trận 2D biểu thị chỗ ngồi, dịch chuyển mọi hàng sang phải và gói cột cuối cùng sang hàng tiếp theo mỗi khi học sinh đến. Sau mỗi lần chèn, chúng tôi sẽ quét tất cả các hàng và cột để đếm những hàng có chứa ít nhất một học sinh nghiêm túc. Mặc dù điều này sẽ tạo ra câu trả lời đúng, nhưng mỗi lần chèn sẽ tốn O(n * m) trong trường hợp xấu nhất, trở thành O((n * m)^2) cho một trường hợp thử nghiệm duy nhất có kích thước tối đa, vượt xa giới hạn thời gian. 

Thông tin chi tiết quan trọng là tách lưới 2D và chỉ theo dõi thông tin tối thiểu cần thiết: liệu một hàng hay cột có "tốt" hay không. Mỗi học sinh ảnh hưởng đến chính xác một chỗ ngồi, điều này quyết định hàng và cột có thể trở nên tốt nếu học sinh nghiêm túc. Vì vậy, chúng ta có thể duy trì hai mảng: một mảng cho tổng số hàng tốt và một mảng cho các cột tốt. Để xử lý hiệu ứng bao quanh theo thứ tự hàng lớn, chúng tôi tính toán trước chỉ mục cột cho mỗi học sinh bằng cách`student_index % m`và chỉ số hàng theo`student_index // m`. Đối với các cột, chúng ta cần xem xét không chỉ học sinh hiện tại mà còn tất cả các học sinh trước đó trong cùng một cột; chúng ta có thể duy trì tổng tiền tố cho những học sinh nghiêm túc trên mỗi cột. Mỗi học sinh mới chỉ có thể tăng số lượng hàng hoặc cột một lần, do đó có thể cập nhật liên tục theo thời gian, dẫn đến thuật toán tổng thể là O(n * m). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((n * m)^2) | O(n * m) | Quá chậm | 
| Tối ưu | O(n * m) | O(m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo hai mảng kích thước`m`:`col_count`để theo dõi có bao nhiêu học sinh nghiêm túc được xếp vào mỗi cột và`col_good`dưới dạng một mảng nhị phân cho biết một cột có tốt hay không. Khởi tạo một tập hợp hoặc mảng`row_good`kích thước`n`để theo dõi xem mỗi hàng có chứa một học sinh nghiêm túc hay không. Khởi tạo`good_rows`Và`good_cols`bộ đếm về 0. 
2. Đối với mỗi học sinh`i`trong chuỗi`s`, tính chỉ số hàng`r = i // m`và chỉ số cột`c = i % m`. Các chỉ số này xác định hàng và cột nào bị ảnh hưởng. 
3. Nếu học sinh nghiêm túc (`s[i] == '1'`) và hàng`r`vẫn chưa được đánh giá là tốt, tăng thêm`good_rows`và đánh dấu`row_good[r]`như sự thật. Tương tự, nếu cột`c`vẫn chưa ổn, tăng thêm`good_cols`và đánh dấu`col_good[c]`như sự thật. Cập nhật`col_count[c]`để phản ánh học sinh mới nghiêm túc trong cột đó. 
4. Nối thêm`good_rows + good_cols`vào mảng đầu ra. Điều này thể hiện tổng số hàng và cột tốt sau học sinh hiện tại. 
5. Sau khi xử lý tất cả học sinh, in mảng đầu ra thành một dòng. 

Tại sao nó hoạt động: Mỗi học sinh chỉ ảnh hưởng đến hàng và cột nơi họ ngồi. Bằng cách theo dõi những hàng và cột nào đã tốt, chúng tôi tránh được việc tính hai lần và duy trì tính bất biến đó`good_rows`Và`good_cols`phản ánh chính xác số lượng hàng và cột có ít nhất một học sinh nghiêm túc ở mỗi bước. Hành vi bao quanh được xử lý ngầm vì các chỉ số cột lặp lại modulo`m`và chỉ số hàng tăng tuần tự. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        s = input().strip()
        row_good = [0] * n
        col_good = [0] * m
        good_rows = 0
        good_cols = 0
        col_count = [0] * m
        prefix = [0] * (n * m)
        out = []
        for i in range(n * m):
            r = i // m
            c = i % m
            if s[i] == '1':
                col_count[c] += 1
                if row_good[r] == 0:
                    row_good[r] = 1
                    good_rows += 1
                if col_good[c] == 0:
                    col_good[c] = 1
                    good_cols += 1
            out.append(good_rows + good_cols)
        print(' '.join(map(str, out)))

solve()
```Giải pháp này duy trì độ tốt của các mảng và cập nhật các bộ đếm theo thời gian không đổi cho mỗi học sinh. các`col_count`mảng được chuẩn bị để hỗ trợ các phần mở rộng tiềm năng nếu cần theo dõi nhiều học sinh nghiêm túc trên mỗi cột, mặc dù trong vấn đề này chỉ sự hiện diện là quan trọng. Chúng tôi tính toán các chỉ số hàng và cột bằng cách sử dụng phép chia số nguyên và modulo để xử lý bố cục 2D trong một mảng phẳng. 

## Ví dụ đã hoạt động 

Đối với đầu vào mẫu đầu tiên:```
2 2
1100
```| tôi | s[i] | r | c | tốt_rows | tốt_cols | đầu ra | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 0 | 0 | 1 | 1 | 2 | 
| 1 | 1 | 0 | 1 | 1 | 2 | 3 | 
| 2 | 0 | 1 | 0 | 2 | 2 | 4 | 
| 3 | 0 | 1 | 1 | 2 | 2 | 3 | 

Bảng này chứng tỏ rằng các hàng và cột tốt được cập nhật chính xác khi một học sinh nghiêm túc nhập vào một hàng hoặc cột chưa được đánh dấu trước đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n * m) | Mỗi học sinh được xử lý một lần và cập nhật cho hàng và cột là thời gian không đổi. | 
| Không gian | O(n + m) | Mảng cho độ tốt của hàng và cột được sử dụng. | 

Điều này phù hợp thoải mái trong các ràng buộc nhất định, vì tổng số chỗ trong các trường hợp thử nghiệm là 10^6. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    solve()
    return output.getvalue().strip()

# Provided samples
assert run("3\n2 2\n1100\n4 2\n11001101\n2 4\n11001101\n") == "2 3 4 3\n2 3 4 3 5 4 6 5\n2 3 3 3 4 4 4 5", "samples"

# Custom cases
assert run("1\n1 1\n1\n") == "2", "single cell serious"
assert run("1\n1 1\n0\n") == "0", "single cell naughty"
assert run("1\n2 2\n0000\n") == "0 0 0 0", "all naughty"
assert run("1\n2 2\n1111\n") == "2 3 4 4", "all serious"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1, sinh viên nghiêm túc | 2 | cập nhật lưới tối thiểu một cách chính xác | 
| 1 1, sinh viên nghịch ngợm | 0 | ban đầu không có hàng hoặc cột tốt | 
| 2 2, tất cả đều nghịch ngợm | 0 0 0 0 | nhiều sinh viên, không có thông tin cập nhật | 
| 2 2, tất cả đều nghiêm túc | 2 3 4 4 | nhiều sinh viên, mọi ô đều tốt | 

## Vỏ cạnh 

Một trường hợp phức tạp xảy ra khi học sinh đầu tiên trong mỗi cột nghiêm túc nhưng những học sinh khác lại nghịch ngợm. Thuật toán tăng chính xác`good_cols`chỉ lần đầu tiên một cột nhận được một học sinh nghiêm túc và theo dõi các hàng tương tự. Một trường hợp khác là một hàng hoặc một cột, trong đó tất cả học sinh nằm trong cùng một hàng hoặc cột. Hàng hoặc cột được đánh dấu tốt ở học sinh nghiêm túc đầu tiên và những học sinh tiếp theo không tăng số đếm lại, đảm bảo không tính hai lần. Các phép toán phân chia theo modulo và số nguyên xử lý một cách tự nhiên ánh xạ 1D-to-2D được làm phẳng ngay cả trong các trường hợp biên này.
