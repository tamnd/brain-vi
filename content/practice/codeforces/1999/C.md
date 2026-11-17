---
title: "CF 1999C - Đang Tắm"
description: "Chúng ta có một ngày dài m phút và một tập hợp các nhiệm vụ không chồng chéo, mỗi nhiệm vụ chiếm một khoảng thời gian liên tục."
date: "2026-06-08T14:20:25+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1999
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 964 (Div. 4)"
rating: 800
weight: 1999
solve_time_s: 139
verified: true
draft: false
---

[CF 1999C - Đang tắm](https://codeforces.com/problemset/problem/1999/C) 

**Đánh giá:** 800 
**Tags:** tham lam, thực hiện 
**Thời gian giải:** 2m 19s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được ban cho một ngày dài`m`phút và một tập hợp các nhiệm vụ không chồng chéo, mỗi nhiệm vụ chiếm một khoảng thời gian liên tục. Alex muốn tắm cho`s`phút và chúng ta cần xác định xem có tồn tại ít nhất một khối thời gian liền kề có độ dài hay không`s`không giao nhau với bất kỳ nhiệm vụ đã lên lịch nào. Đầu vào bao gồm nhiều trường hợp thử nghiệm, mỗi trường hợp có danh sách nhiệm vụ và tham số riêng`n`,`s`, Và`m`. Đầu ra là "CÓ" đơn giản nếu khối như vậy tồn tại và "KHÔNG" nếu không. 

Các ràng buộc cho phép lên đến`2 * 10^5`nhiệm vụ trên tất cả các trường hợp thử nghiệm và mỗi ngày có thể lên tới`10^9`phút dài. Vì lý do này, bất kỳ giải pháp nào lặp đi lặp lại từng phút trong ngày sẽ là quá chậm. Chúng ta cần một cách tiếp cận hoạt động tuyến tính theo số lượng nhiệm vụ cho từng trường hợp thử nghiệm. 

Một trường hợp đặc biệt cần xem xét là khi một khối trống tồn tại vào đầu ngày trước nhiệm vụ đầu tiên hoặc ở cuối ngày sau nhiệm vụ cuối cùng. Ví dụ, nếu`s = 3`,`m = 10`, và nhiệm vụ duy nhất là`(5, 7)`, khoảng`(0, 5)`đủ lâu để tắm, và cũng vậy`(7, 10)`. Việc triển khai bất cẩn chỉ kiểm tra khoảng trống giữa các nhiệm vụ liên tiếp có thể bỏ lỡ các khoảng thời gian bắt đầu và kết thúc này. Một trường hợp tế nhị khác là khi`s`chính xác bằng độ dài khoảng cách; lỗi từng cái một thường xuất hiện ở đây nên bất đẳng thức phải là`>= s`. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ xem xét từng phút trong ngày và kiểm tra xem một khối có chiều dài`s`là miễn phí. Cho mỗi phút bắt đầu`t`, chúng tôi sẽ xác minh nếu`[t, t+s)`chồng chéo bất kỳ khoảng thời gian nhiệm vụ. Điều này đúng về mặt logic nhưng có thể yêu cầu tới`10^9`kiểm tra trong trường hợp xấu nhất, vượt xa giới hạn khả thi. 

Quan sát chính là các nhiệm vụ không chồng chéo và được sắp xếp theo thời gian bắt đầu. Do đó, chỉ cần kiểm tra khoảng cách giữa các nhiệm vụ liên tiếp, cũng như thời điểm bắt đầu ngày trước nhiệm vụ đầu tiên và thời điểm cuối ngày sau nhiệm vụ cuối cùng là đủ. Mỗi khoảng trống được xác định khi kết thúc một nhiệm vụ và bắt đầu nhiệm vụ tiếp theo. Nếu có khoảng cách ít nhất`s`vài phút nữa Alex có thể tắm được. Điều này làm giảm vấn đề xuống còn quét tuyến tính trên`n`khoảng thời gian, làm cho sự phức tạp`O(n)`mỗi trường hợp thử nghiệm. Cách tiếp cận này đơn giản, nhanh chóng và xử lý tất cả các trường hợp khó khăn một cách tự nhiên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(m * n) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) cho mỗi trường hợp thử nghiệm | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case`t`. Đối với mỗi trường hợp thử nghiệm, hãy đọc`n`,`s`, Và`m`. 
2. Đọc tất cả`n`nhiệm vụ và lưu trữ chúng theo cặp`(l_i, r_i)`. 
3. Kiểm tra khoảng thời gian trước nhiệm vụ đầu tiên. Nếu nhiệm vụ đầu tiên bắt đầu lúc`l_1`, Và`l_1 >= s`, xuất ra "CÓ". 
4. Lặp lại các nhiệm vụ liên tiếp. Đối với nhiệm vụ`i`Và`i+1`, tính khoảng cách`l_{i+1} - r_i`. Nếu khoảng cách này là`>= s`, xuất ra "CÓ". 
5. Kiểm tra khoảng thời gian sau nhiệm vụ cuối cùng. Nếu nhiệm vụ cuối cùng kết thúc vào lúc`r_n`, Và`m - r_n >= s`, xuất ra "CÓ". 
6. Nếu không có lần kiểm tra nào thành công, xuất ra "NO". 

Tại sao nó hoạt động: Các nhiệm vụ không chồng chéo và được sắp xếp. Tất cả các vị trí có thể để tắm đều là trước nhiệm vụ đầu tiên, giữa hai nhiệm vụ hoặc sau nhiệm vụ cuối cùng. Bằng cách kiểm tra từng khoảng thời gian này để biết độ dài`>= s`, chúng tôi bao gồm tất cả các khả năng mà không bỏ lỡ bất kỳ cửa sổ hợp lệ nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def can_shower(n, s, m, tasks):
    if tasks[0][0] >= s:
        return True
    for i in range(n - 1):
        if tasks[i+1][0] - tasks[i][1] >= s:
            return True
    if m - tasks[-1][1] >= s:
        return True
    return False

t = int(input())
for _ in range(t):
    n, s, m = map(int, input().split())
    tasks = [tuple(map(int, input().split())) for _ in range(n)]
    print("YES" if can_shower(n, s, m, tasks) else "NO")
```chức năng`can_shower`xử lý logic cốt lõi. Trước tiên, chúng tôi kiểm tra khoảng cách đầu ngày, sau đó là khoảng cách giữa các nhiệm vụ và cuối cùng là khoảng cách cuối ngày. Thứ tự này đảm bảo chúng tôi trả về "CÓ" ở khoảng thời gian đầu tiên có thể, tránh các tính toán không cần thiết. Việc sử dụng bộ dữ liệu và số học trực tiếp giúp giải pháp trở nên đơn giản và hiệu quả. Chúng tôi tránh từng lỗi một bằng cách sử dụng cẩn thận`>= s`. 

## Ví dụ đã hoạt động 

Đầu vào mẫu:```
3 3 10
3 5
6 8
9 10
```| Bước | Đã kiểm tra khoảng thời gian | Chiều dài khoảng cách | Kết quả | 
| --- | --- | --- | --- | 
| Trước nhiệm vụ đầu tiên | 0-3 | 3 | CÓ | 

Chúng tôi thấy nhiệm vụ đầu tiên bắt đầu lúc 3 giờ và`3 >= s`, để Alex có thể tắm ngay. Không cần kiểm tra thêm. 

Đầu vào thứ hai:```
3 3 10
1 2
3 5
6 8
```| Bước | Đã kiểm tra khoảng thời gian | Chiều dài khoảng cách | Kết quả | 
| --- | --- | --- | --- | 
| Trước nhiệm vụ đầu tiên | 0-1 | 1 | KHÔNG | 
| Giữa nhiệm vụ 1 & 2 | 3-2 | -1 | KHÔNG | 
| Giữa nhiệm vụ 2 & 3 | 6-5 | 1 | KHÔNG | 
| Sau nhiệm vụ cuối cùng | 8-10 | 2 | KHÔNG | 

Ở đây, tất cả các khoảng trống đều ngắn hơn 3 phút nên đầu ra là KHÔNG. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Chúng tôi quét các nhiệm vụ một lần và tính toán khoảng cách giữa các nhiệm vụ liên tiếp. | 
| Không gian | O(n) cho mỗi trường hợp thử nghiệm | Chúng tôi lưu trữ các khoảng thời gian thực hiện nhiệm vụ dưới dạng danh sách các bộ dữ liệu. | 

Với tổng số`n`trên tất cả các trường hợp thử nghiệm 2*10^5, điều này nằm trong giới hạn thời gian 2 giây. Việc sử dụng bộ nhớ là tối thiểu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    # solution
    t = int(input())
    for _ in range(t):
        n, s, m = map(int, input().split())
        tasks = [tuple(map(int, input().split())) for _ in range(n)]
        if tasks[0][0] >= s:
            print("YES")
            continue
        for i in range(n - 1):
            if tasks[i+1][0] - tasks[i][1] >= s:
                print("YES")
                break
        else:
            if m - tasks[-1][1] >= s:
                print("YES")
            else:
                print("NO")
    return output.getvalue().strip()

# provided samples
assert run("4\n3 3 10\n3 5\n6 8\n9 10\n3 3 10\n1 2\n3 5\n6 7\n3 3 10\n1 2\n3 5\n6 8\n3 4 10\n1 2\n6 7\n8 9\n") == "YES\nYES\nNO\nYES"

# custom tests
assert run("1\n1 5 10\n0 4\n") == "YES", "start-of-day gap"
assert run("1\n2 2 5\n0 2\n3 5\n") == "NO", "no valid gaps"
assert run("1\n3 1 10\n0 1\n2 3\n4 5\n") == "YES", "tiny gaps suffice"
assert run("1\n2 3 6\n0 2\n3 4\n") == "NO", "end-of-day insufficient"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 nhiệm vụ khi bắt đầu | CÓ | Xử lý chính xác khoảng trống trước nhiệm vụ đầu tiên | 
| Hai nhiệm vụ có phạm vi bảo hiểm đầy đủ | KHÔNG | Phát hiện chính xác không có khoảng trống | 
| Nhiều khoảng trống nhỏ | CÓ | Xử lý chính xác những khoảng trống tối thiểu đủ để tắm | 
| Khoảng cách cuối ngày quá ngắn | KHÔNG | Đánh giá chính xác khoảng cách sau nhiệm vụ cuối cùng | 

## Vỏ cạnh 

Nếu nhiệm vụ đầu tiên bắt đầu vào đúng thời điểm`s`, thuật toán sẽ trả về "CÓ" ngay lập tức vì khoảng`[0, l_1)`có chiều dài`l_1 >= s`. Nếu khoảng trống duy nhất xảy ra sau nhiệm vụ cuối cùng, bước kiểm tra cuối cùng`m - r_n >= s`nắm bắt nó. Khoảng cách âm hoặc độ dài bằng 0 không thể xảy ra do đảm bảo đầu vào rằng`l_i > r_{i-1}`, nhưng chúng tôi vẫn sử dụng`>= s`để tránh xảy ra lỗi lẻ tẻ. Điều này đảm bảo tính chính xác ngay cả ở ranh giới.
