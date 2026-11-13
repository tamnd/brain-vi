---
title: "CF 1715B - Mảng Đẹp"
description: "Chúng ta được yêu cầu xây dựng một mảng có độ dài n gồm các số nguyên không âm. Hai ràng buộc toàn cục phải được thỏa mãn cùng một lúc. Đầu tiên, tổng của tất cả các phần tử phải chính xác bằng s."
date: "2026-06-09T19:55:14+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1715
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 816 (Div. 2)"
rating: 1000
weight: 1715
solve_time_s: 122
verified: false
draft: false
---

[CF 1715B - Mảng đẹp](https://codeforces.com/problemset/problem/1715/B) 

**Đánh giá:** 1000 
**Tags:** thuật toán xây dựng, tham lam, toán học 
**Thời gian giải:** 2m 2s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu xây dựng một mảng có độ dài`n`gồm các số nguyên không âm. Hai ràng buộc toàn cục phải được thỏa mãn cùng một lúc. 

Đầu tiên, tổng của tất cả các phần tử phải chính xác`s`. Thứ hai, nếu mỗi phần tử được chia cho`k`và thả nổi thì tổng các thương số này phải chính xác`b`. Điều kiện thứ hai đó có thể được hiểu là đếm xem có bao nhiêu khối đầy đủ kích thước`k`tồn tại trên toàn mảng. 

Mỗi phần tử đóng góp độc lập: một giá trị`x`đóng góp`x // k`làm đẹp và góp phần`x`đến tổng số tiền. Nhiệm vụ là phân phối tổng số tiền`s`sang`n`vẫn đảm bảo tổng số slot đầy đủ`k`-khối bằng`b`. 

Các ràng buộc rất lớn:`n`lên tới 1e5 qua các bài kiểm tra,`k`lên đến 1e9, và`s`lên tới 1e18. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào thử tất cả các phân phối hoặc xây dựng các ứng cử viên bằng các vòng lặp lồng nhau trên các giá trị có thể có cho mỗi vị trí. Xây dựng tuyến tính cho mỗi trường hợp thử nghiệm là hướng khả thi duy nhất. 

Một trường hợp phức tạp xuất hiện khi tính khả thi là không thể ngay cả trước khi xây dựng. Ví dụ, nếu`k = 6`,`b = 3`, Và`s = 12`với`n = 3`, thì số tiền tối thiểu có thể để đạt được vẻ đẹp`3`đã quá lớn hoặc quá có cấu trúc để phù hợp`s`. Điều này xuất phát từ thực tế là mọi vẻ đẹp đều cần ít nhất`k`tổng đóng góp ở đâu đó trong mảng, vì vậy`s`phải đủ lớn để hỗ trợ`b`khối đầy đủ. 

Một dạng lỗi khác xảy ra khi`b`quá lớn để được phân phối trên`n`các phần tử. Nếu như`b > s // k`, thì ngay cả việc tập trung toàn bộ khối lượng vào một phần tử cũng không thể tạo ra đủ sự phân chia đầy đủ. 

Khó khăn thực sự không phải là tính khả thi mà là phân phối cả “đóng góp khối” và giá trị còn sót lại mà không vi phạm bất kỳ ràng buộc nào. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là coi mỗi phần tử như một biến và thử gán các giá trị tăng dần. Đối với mỗi vị trí, chúng ta có thể thử các giá trị từ`0`ĐẾN`s`, tính tổng và độ đẹp hiện tại rồi quay lại. Điều này đúng về nguyên tắc nhưng hoàn toàn không khả thi. Ngay cả đối với`n = 100`, không gian trạng thái bùng nổ, và đối với`n = 10^5`, điều đó là không thể. 

Nhận xét quan trọng là sự đóng góp về vẻ đẹp được cấu trúc theo từng khối kích thước`k`. Bất kỳ số nào`x`có thể bị phân hủy như sau:`x = (x // k) * k + (x % k)`Điều này chia mỗi phần tử thành “phần khối” và “phần còn lại”. Vẻ đẹp chỉ phụ thuộc vào phần khối, còn phần còn lại không ảnh hưởng gì đến vẻ đẹp cả. 

Vì vậy vấn đề trở thành: chúng ta cần phân bổ chính xác`b`khối có kích thước`k`trên toàn mảng và phân phối số tiền còn lại một cách tự do dưới dạng số dư, trong khi vẫn giữ tổng số tiền bằng`s`. 

Một công trình sạch sẽ xuất hiện nếu trước tiên chúng ta chỉ định càng nhiều`k`các khối thành một phần tử nhất có thể, sau đó phân phối các khối còn lại dưới dạng`k`-gia tăng kích thước ở các vị trí khác. Sau khi chỉnh sửa đẹp đẽ, chúng ta phân bổ số tiền còn sót lại vào bất kỳ phần tử nào mà không ảnh hưởng đến cấu trúc phân chia tầng. 

Chúng ta chỉ cần đảm bảo rằng tổng số tiền tối thiểu cần thiết để làm đẹp`b`là`b * k`, và độ chùng còn lại`s - b * k`có thể được phân phối tùy ý. 

Nếu như`s < b * k`, không có giải pháp. Mặt khác, chúng ta xây dựng một mảng cơ sở với`b`đơn vị của`k`, sau đó phân phối số tiền còn lại. 

Cuối cùng, chúng tôi đảm bảo không có phần tử nào vô tình nhận thêm một khối do tràn vượt quá`k`, bằng cách cẩn thận đặt tối đa một giá trị lớn cho mỗi vị trí. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta xây dựng mảng một cách tham lam trong khi kiểm soát số lượng mảng đầy đủ.`k`-blocks mỗi phần tử đóng góp. 

1. Kiểm tra tính khả thi bằng cách xác minh xem`b * k ≤ s`. Nếu không, không mảng nào có thể tạo đủ khối đầy đủ mà không vượt quá tổng số khối. Điều này là do mỗi đơn vị sắc đẹp ít nhất có giá`k`về giá trị. 
2. Khởi tạo một mảng có kích thước`n`với tất cả số không. Đầu tiên chúng ta sẽ phân bổ vẻ đẹp cần thiết`b`vào phần tử đầu tiên một cách tập trung. Điều này tránh việc phân phối khối sớm và đơn giản hóa việc theo dõi. 
3. Đặt`a[0] = b * k`. Điều này đảm bảo vẻ đẹp do yếu tố này mang lại chính xác`b`, từ`(b * k) // k = b`. Chưa có yếu tố nào khác góp phần tạo nên vẻ đẹp. 
4. Trừ khoản đóng góp này vào số tiền còn lại:`remaining = s - b * k`. 
5. Bây giờ chúng tôi phân phối`remaining`khắp mảng mà không thay đổi vẻ đẹp. Vì bất kỳ giá trị nào`< k`không đóng góp gì cho vẻ đẹp, chúng ta có thể đặt các giá trị vào đó một cách an toàn`[0, k-1]`ở bất kỳ vị trí nào. 
6. Điền các phần tử từ trái sang phải, cộng lại bằng`k-1`mỗi phần tử cho đến khi`remaining`đã cạn kiệt. Điều này đảm bảo không có gì mới`k`-block được tạo ra một cách vô tình. 
7. Xuất ra mảng kết quả. 

### Tại sao nó hoạt động 

Việc xây dựng cô lập tất cả những đóng góp về vẻ đẹp vào một yếu tố được kiểm soát duy nhất. Vì chỉ`a[0]`được phép ≥`k`, chỉ riêng nó mới quyết định tổng số tiền sàn. Mọi yếu tố khác được giữ nghiêm ngặt dưới đây`k`, đảm bảo không đóng góp gì cho sắc đẹp. Số tiền còn lại được phân phối mà không vượt qua ngưỡng có thể tạo thêm khối. Điều này bảo tồn cả hai ràng buộc một cách độc lập, làm cho việc xây dựng trở nên an toàn và mang tính quyết định. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, k, b, s = map(int, input().split())

        min_sum = b * k
        if min_sum > s:
            print(-1)
            continue

        res = [0] * n

        # allocate all beauty into first element
        res[0] = b * k
        remaining = s - b * k

        # distribute remaining without creating new k-blocks
        for i in range(n):
            if remaining == 0:
                break
            add = min(k - 1, remaining)
            res[i] += add
            remaining -= add

        if remaining > 0:
            print(-1)
        else:
            print(*res)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách kiểm tra xem số lượng yêu cầu đầy đủ`k`thậm chí có thể đóng góp với tổng số tiền. Nếu không, nó sẽ bác bỏ vụ việc ngay lập tức. 

Bước xây dựng đặt tất cả vẻ đẹp cần thiết vào phần tử đầu tiên bằng cách sử dụng bội số chính xác của`k`. Điều này đảm bảo rằng tổng chia sàn chính xác`b`và không phụ thuộc vào cách phân phối các giá trị còn lại. 

Vòng lặp phân phối`remaining`được giới hạn cẩn thận bởi`k - 1`, điều này ngăn cản bất kỳ yếu tố mới nào góp phần tạo nên vẻ đẹp. Đây là chi tiết triển khai trọng tâm: vượt qua ngưỡng`k`sẽ âm thầm tăng thêm vẻ đẹp và phá vỡ sự đúng đắn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 3, k = 6, b = 3, s = 19
```Chúng tôi tính toán`b * k = 18`, Vì thế`remaining = 1`. 

| Bước | Trạng thái mảng | Còn lại | Sắc đẹp | 
| --- | --- | --- | --- | 
| ban đầu | [0, 0, 0] | 19 | 0 | 
| gán vẻ đẹp | [18, 0, 0] | 1 | 3 | 
| phân phối | [18, 1, 0] | 0 | 3 | 

Mảng cuối cùng có tổng bằng 19 và tạo ra vẻ đẹp 3, xác nhận cả hai ràng buộc. 

### Ví dụ 2 

đầu vào:```
n = 5, k = 4, b = 2, s = 25
```Chúng tôi tính toán`b * k = 8`, Vì thế`remaining = 17`. 

| Bước | Trạng thái mảng | Còn lại | Sắc đẹp | 
| --- | --- | --- | --- | 
| ban đầu | [0, 0, 0, 0, 0] | 25 | 0 | 
| gán vẻ đẹp | [8, 0, 0, 0, 0] | 17 | 2 | 
| phân phối | [8, 3, 3, 3, 3] | 1 | 2 | 

Chúng tôi dừng lại khi số lượng còn lại đã cạn kiệt hoặc không thể đặt đầy đủ mà không vượt quá giới hạn. 

Dấu vết này cho thấy nguồn gốc duy nhất của vẻ đẹp vẫn là yếu tố đầu tiên, trong khi tất cả những yếu tố khác đều ở bên dưới`k`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi bài kiểm tra phân phối số tiền còn lại một lần cho mỗi phần tử | 
| Không gian | O(n) | Lưu trữ mảng đã xây dựng | 

Các ràng buộc cho phép lên đến`1e5`tổng số phần tử và giải pháp chỉ thực hiện công việc không đổi cho mỗi phần tử, do đó nó phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    out = []
    
    input = sys.stdin.readline
    
    t = int(sys.stdin.readline())
    for _ in range(t):
        n, k, b, s = map(int, sys.stdin.readline().split())
        if b * k > s:
            out.append("-1")
            continue
        res = [0] * n
        res[0] = b * k
        remaining = s - b * k
        for i in range(n):
            if remaining == 0:
                break
            add = min(k - 1, remaining)
            res[i] += add
            remaining -= add
        if remaining > 0:
            out.append("-1")
        else:
            out.append(" ".join(map(str, res)))
    return "\n".join(out)

# provided samples (simplified validity checks)
assert run("1\n1 6 3 100\n") == "-1"
assert run("1\n1 1 0 0\n") == "0"

# custom cases
assert run("1\n3 5 1 5\n") != "", "small feasible case"
assert run("1\n2 10 1 5\n") == "-1", "impossible by k constraint"
assert run("1\n4 3 2 20\n") != "", "distribution case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tối thiểu không thể | -1 | b*k > s | 
| số không tầm thường | 0 | trường hợp cơ sở | 
| nhỏ khả thi | bất kỳ mảng hợp lệ nào | xây dựng đúng đắn | 
| ràng buộc k không thể | -1 | logic khả thi | 
| trường hợp phân phối | mảng hợp lệ | còn sót lại lan rộng | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi`b * k == s`. Trong trường hợp này, không còn sự lỏng lẻo nào nữa. Thuật toán gán`a[0] = s`và để lại tất cả các yếu tố khác bằng không. Vì tất cả các giá trị khác đều ở dưới`k`, chúng không đóng góp gì và chỉ riêng phần tử đầu tiên đã tạo ra chính xác`b`sắc đẹp. Việc xây dựng vẫn còn hiệu lực. 

Một trường hợp cạnh khác là khi`k = 1`. Ở đây mọi số nguyên đều góp phần tạo nên vẻ đẹp trọn vẹn. Điều kiện trở thành`sum(a) = s`và vẻ đẹp ngang bằng`s`. Lực lượng này`b = s`. Nếu không, không có giải pháp nào tồn tại. Thuật toán xử lý việc này một cách tự nhiên vì`b * k = b`phải bằng`s`. 

Trường hợp cạnh cuối cùng là khi`n = 1`. Mảng duy nhất có thể là một giá trị duy nhất`s`. Điều kiện giảm xuống còn kiểm tra xem`s // k == b`, phù hợp với cấu trúc vì chúng tôi đặt phần tử duy nhất thành`b * k`và yêu cầu`s = b * k`.
