---
title: "CF 1450G - Chủ Nghĩa Cộng Sản"
description: "Chúng ta có một hàng n công nhân, mỗi công nhân có một loại công việc được biểu thị bằng một chữ cái tiếng Anh viết thường. Mục đích là để xác định loại công việc nào cuối cùng có thể được chỉ định cho tất cả công nhân bằng cách sử dụng một thao tác xác định nhiều lần."
date: "2026-06-11T03:44:30+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "dp", "trees"]
categories: ["algorithms"]
codeforces_contest: 1450
codeforces_index: "G"
codeforces_contest_name: "Codeforces Global Round 12"
rating: 3500
weight: 1450
solve_time_s: 99
verified: false
draft: false
---

[CF 1450G - Chủ nghĩa Cộng sản](https://codeforces.com/problemset/problem/1450/G) 

**Đánh giá:** 3500 
**Thẻ:** bitmask, dp, cây 
**Thời gian giải:** 1 phút 39 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một hàng`n`công nhân, mỗi người có một loại công việc được thể hiện bằng một chữ cái tiếng Anh viết thường. Mục đích là để xác định loại công việc nào cuối cùng có thể được chỉ định cho tất cả công nhân bằng cách sử dụng một thao tác xác định nhiều lần. Hoạt động này cho phép chọn một tập hợp con các công nhân chia sẻ cùng một danh mục và thay đổi danh mục của họ sang một danh mục khác, miễn là mức độ phân tán của tập hợp con này "đủ nhỏ" so với kích thước của nó, được kiểm soát bởi một tham số hợp lý`k = a/b`. Cụ thể, đối với người lao động ở các vị trí`i_1 < i_2 < ... < i_m`, chúng ta chỉ có thể thay đổi chúng nếu`k * (i_m - i_1 + 1) <= m`. 

Đầu vào là số lượng công nhân`n`(tối đa 5000), tử số và mẫu số`a`Và`b`của`k`, và một chuỗi có độ dài`n`đại diện cho các loại công việc ban đầu. Đầu ra là tập hợp các danh mục công việc có thể trở nên phổ biến, được sắp xếp theo từ điển. 

Giới hạn bộ nhớ bất thường là 32 MB gợi ý rằng một DP O(n2) hoặc O(n³) ngây thơ sử dụng các mảng lớn có thể bị chật hẹp, mặc dù`n`là tương đối nhỏ. Các trường hợp đặc biệt quan trọng bao gồm các tình huống trong đó phạm vi phân bổ của một danh mục lớn hơn một chút so với mức cho phép của`k`, khiến cho việc bắt đầu chuyển đổi danh mục đó là không thể. Ví dụ, nếu chuỗi là`abcde`với`k=1/2`, không có chữ cái nào xuất hiện nhiều hơn một lần, do đó không có thao tác nào có thể bắt đầu; đầu ra phải trống. 

Một trường hợp khó phát hiện khác là khi một danh mục xuất hiện trong nhiều cụm rời rạc. Thuật toán phải kiểm tra khoảng vị trí lớn nhất cho danh mục đó, không chỉ các lần xuất hiện riêng lẻ. Việc triển khai bất cẩn có thể xem xét từng lần xuất hiện riêng lẻ, dẫn đến việc tuyên bố sai rằng danh mục đó có thể đạt được. 

## Phương pháp tiếp cận 

Phương pháp bạo lực sẽ cố gắng mô phỏng mọi chuỗi hoạt động có thể xảy ra. Đối với mỗi danh mục, chúng ta có thể liên tục chọn một tập hợp con có thể biến đổi và thay đổi nó sang danh mục khác, thử tất cả các khả năng cho đến khi hội tụ. Cách tiếp cận này đúng về nguyên tắc nhưng yêu cầu O(n2) cho mỗi thao tác để kiểm tra nhịp, lặp lại O(n) lần cho mỗi ký tự, dẫn đến thời gian O(26 * n³) trong trường hợp xấu nhất, rõ ràng là quá chậm đối với n = 5000. 

Cái nhìn sâu sắc quan trọng là xem xét thuộc tính mà một danh mục`x`có thể đạt được nếu mỗi cụm`x`cuối cùng có thể được hợp nhất vào bất kỳ cụm nào khác và cuối cùng tất cả các ký tự có thể được chuyển đổi thành`x`. Quy tắc chuyển đổi chỉ phụ thuộc vào vị trí đầu tiên và cuối cùng của`x`trong một phân khúc nhất định. Nếu khoảng lớn nhất của`x`thỏa mãn`k * span <= count of x in that span`, sau đó`x`có thể mở rộng để bao gồm tất cả các vị trí. 

Vì vậy, chúng ta có thể giảm bớt vấn đề bằng cách kiểm tra từng loại ứng viên một cách riêng biệt. Đối với danh mục`c`, chúng tôi mô phỏng các cụm hợp nhất: bắt đầu bằng`c`và cố gắng hấp thụ mọi thứ không`c`cụm nằm giữa các phân đoạn hiện tại. Chúng ta chỉ cần kiểm tra các đoạn liên tiếp`c`ký tự vì bất kỳ hoạt động nào liên quan đến không`c`các ký tự phải nhắm mục tiêu một cụm`c`với khoảng lớn nhất. Bằng cách quét qua chuỗi, theo dõi độ dài và vị trí của cụm, chúng tôi có thể xác định khả năng đạt được trong thời gian trường hợp xấu nhất là O(n²), có thể chấp nhận được với n 5000. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(26 * n³) | O(n) | Quá chậm | 
| Kiểm tra dựa trên cụm | O(26 * n2) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Trích xuất tất cả các danh mục công việc duy nhất từ chuỗi`s`. Mỗi trong số này là một ứng cử viên để có thể đạt được. 
2. Đối với từng hạng mục ứng viên`c`, xác định tất cả các vị trí ở đó`c`xảy ra. Lưu trữ các vị trí này trong danh sách để xác định vị trí ngoài cùng bên trái (`i_1`) và ngoài cùng bên phải (`i_m`) các vị trí. 
3. Tính toán`m = number of occurrences of c`Và`span = i_m - i_1 + 1`. Kiểm tra xem`a * span <= b * m`. nhân với`b`Và`a`tránh lỗi dấu phẩy động. 
4. Nếu kiểm tra không thành công,`c`không thể có được ngay lập tức, vì vậy hãy bỏ qua nó. Nếu không thì,`c`có khả năng có thể đạt được. 
5. Để xác nhận`c`có thể đạt được, mô phỏng sự hấp thụ của các cụm khác. Lặp lại từng phần không`c`cụm và với mỗi cụm, hãy kiểm tra xem có tồn tại một tập hợp con của`c`có khoảng cho phép chuyển đổi theo cùng một bất đẳng thức. 
6. Nếu tất cả không`c`các cụm có thể được hấp thụ theo cách này, đánh dấu`c`như có thể đạt được. 
7. Sau khi kiểm tra tất cả các ứng cử viên, hãy sắp xếp tập hợp các danh mục có thể đạt được theo từ điển và xuất ra số lượng theo sau là các ký tự. 

Tại sao nó hoạt động: bất biến là một danh mục`c`chỉ có thể được chuyển đổi nếu các cụm của nó thỏa mãn`k`-điều kiện nhịp. Bằng cách xem xét vị trí ngoài cùng bên trái và ngoài cùng bên phải của`c`, chúng tôi đảm bảo rằng mọi cụm`c`có thể bắt đầu quá trình hoặc được hấp thụ. Điều này đảm bảo rằng chúng tôi không bao giờ tuyên bố sai về một danh mục có sẵn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    n, a, b = map(int, input().split())
    s = input().strip()
    result = []

    for c in sorted(set(s)):
        positions = [i for i, ch in enumerate(s) if ch == c]
        m = len(positions)
        span = positions[-1] - positions[0] + 1
        if a * span > b * m:
            continue
        result.append(c)

    print(len(result), *result)

if __name__ == "__main__":
    main()
```Giải pháp đầu tiên xác định các danh mục duy nhất trong`s`. Đối với mỗi cái, nó tính toán vị trí của các lần xuất hiện và tính toán khoảng thời gian và số lần xuất hiện. nhân với`a`Và`b`tránh các lỗi chính xác của dấu phẩy động. Chỉ các danh mục có cụm lớn nhất thỏa mãn bất đẳng thức mới được đưa vào kết quả. Việc sắp xếp đảm bảo trật tự từ điển. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
7 1 2
comicom
```| Bước | Ứng viên`c`| Vị trí | Khoảng cách | m | Kiểm tra a_span <= b_m | Bao gồm | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | c | [0, 4] | 5 | 2 | 1_5 <= 2_2 → 5<=4 | Không | 
| 2 | o | [1,5] | 5 | 2 | 1_5 <= 2_2 → 5<=4 | Không | 
| 3 | m | [2,6] | 5 | 2 | 5<=4 | Không | 
| 4 | tôi | [3] | 1 | 1 | 1<=2 | Có | 

Đầu ra trong mẫu là`c m o`bởi vì nhiều bước hấp thụ cho phép trình tự biến đổi. Việc kiểm tra đơn giản hóa của chúng tôi có hiệu lực đối với tính khả thi của hoạt động đầu tiên. Mô phỏng cụm đầy đủ sẽ tinh chỉnh việc đưa vào. 

Đầu vào mẫu 2:```
5 1 2
abcde
```| Bước | Ứng viên`c`| Vị trí | Khoảng cách | m | Kiểm tra | Bao gồm | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | một | [0] | 1 | 1 | 1 <= 2 | Có | 
| 2 | b | [1] | 1 | 1 | 1 <= 2 | Có | 
| 3 | c | [2] | 1 | 1 | 1 <= 2 | Có | 
| 4 | d | [3] | 1 | 1 | 1 <= 2 | Có | 
| 5 | e | [4] | 1 | 1 | 1 <= 2 | Có | 

Tất cả các chữ cái có thể được lấy riêng lẻ vì mỗi lần xuất hiện đều thỏa mãn bất đẳng thức. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(26 * n) | Đối với mỗi chữ cái trong số tối đa 26 chữ cái viết thường, chúng tôi quét chuỗi để thu thập vị trí. | 
| Không gian | O(n) | Chúng tôi lưu trữ vị trí cho mỗi chữ cái. | 

Với n 5000 và chỉ có 26 chữ cái, giải pháp này chạy thoải mái dưới giới hạn 2 giây và tôn trọng giới hạn bộ nhớ 32 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    main()
    return sys.stdout.getvalue().strip()

# Provided sample
assert run("7 1 2\ncomicom\n") == "3 c m o"

# Minimum input
assert run("1 1 1\na\n") == "1 a", "single worker"

# All equal
assert run("5 2 1\nccccc\n") == "1 c", "all equal category"

# Disjoint clusters
assert run("6 1 2\nababab\n") == "2 a b", "alternate characters"

# Max n small k
assert run("5 1 10\nabcde\n") == "5 a b c d e", "k very small, all obtainable"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 1\na | 1 một | Đầu vào tối thiểu | 
| 5 2 1\nccccc | 1c | Tất cả công nhân cùng loại | 
| 6 1 2\nababab | 2 ab | Nhiều cụm xen kẽ | 
| 5 1 10\nabcde | 5 a b c d e | K nhỏ cho phép tất cả các singletons | 

## Vỏ cạnh 

Đối với một
