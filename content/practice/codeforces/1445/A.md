---
title: "CF 1445A - Sắp xếp lại mảng"
description: "Chúng ta được cho hai danh sách các số có độ dài bằng nhau và chúng ta chỉ được phép sắp xếp lại danh sách thứ hai. Sau khi sắp xếp lại, chúng tôi ghép các phần tử theo chỉ mục và kiểm tra xem mọi tổng được ghép có nằm trong giới hạn trên cố định hay không."
date: "2026-06-11T04:01:04+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1445
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 680 (Div. 2, based on Moscow Team Olympiad)"
rating: 800
weight: 1445
solve_time_s: 95
verified: true
draft: false
---

[CF 1445A - Sắp xếp lại mảng](https://codeforces.com/problemset/problem/1445/A) 

**Đánh giá:** 800 
**Tags:** tham lam, phân loại 
**Thời gian giải:** 1m 35s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho hai danh sách các số có độ dài bằng nhau và chúng ta chỉ được phép sắp xếp lại danh sách thứ hai. Sau khi sắp xếp lại, chúng tôi ghép các phần tử theo chỉ mục và kiểm tra xem mọi tổng được ghép có nằm trong giới hạn trên cố định hay không. 

Một cách hữu ích để nghĩ về điều này là mỗi giá trị trong mảng đầu tiên đại diện cho một “chi phí đã cam kết” và mỗi giá trị trong mảng thứ hai đại diện cho một “ngân sách bổ sung” mà chúng ta có thể tự do ấn định cho những chi phí đó. Mục tiêu là phân bổ các ngân sách này để không có vị trí nào vượt quá giới hạn. 

Các ràng buộc rất nhỏ: mỗi trường hợp thử nghiệm có tối đa 50 phần tử và có nhiều nhất 100 trường hợp thử nghiệm. Điều này có nghĩa là ngay cả các thuật toán bậc hai cho mỗi trường hợp thử nghiệm cũng đủ nhanh. Về mặt lý thuyết, mọi thứ có thể lập phương hoặc hàm mũ vẫn có thể được chấp nhận nhưng không cần thiết. 

Một sai lầm ngây thơ thường xuất hiện ở đây là cố gắng khớp các phần tử một cách tùy tiện hoặc tham lam mà không sắp xếp cả hai mảng. Ví dụ: ghép nối phần tử lớn nhất trong mảng đầu tiên với phần tử ngẫu nhiên trong mảng thứ hai có thể không thành công ngay cả khi có sự sắp xếp hợp lệ. Coi như`a = [1, 4]`,`b = [2, 5]`,`x = 6`. Nếu chúng ta ghép đôi một cách tham lam như`(4, 5)`đầu tiên, chúng ta ngay lập tức phá vỡ ràng buộc, mặc dù sắp xếp lại`b`BẰNG`[5, 2]`làm cho cả hai cặp hợp lệ. 

Một thất bại tinh vi khác xuất phát từ việc giả định rằng nếu phần tử lớn nhất trong`a`có thể được ghép nối với một số yếu tố trong`b`, sau đó phần còn lại sẽ tự động hoạt động. Điều đó bỏ qua việc sử dụng một lượng nhỏ`b`quá sớm có thể cản trở tính khả thi sau này. 

## Phương pháp tiếp cận 

Ý tưởng mạnh mẽ là thử mọi hoán vị của`b`và kiểm tra xem việc ghép nối có hoạt động hay không. Điều này đúng vì nó khám phá tất cả các nhiệm vụ có thể, nhưng nó sẽ phát triển khi`n!`. Ngay cả đối với`n = 50`, điều này hoàn toàn không khả thi vì số lượng hoán vị rất lớn về mặt thiên văn. 

Cái nhìn sâu sắc quan trọng là chúng ta đang giải quyết một bài toán gán với một ràng buộc đơn điệu: các giá trị nhỏ hơn trong`a`dễ thỏa mãn hơn, do đó chúng nên được kết hợp với sự linh hoạt lớn hơn trong`b`và các giá trị lớn hơn trong`a`yêu cầu các giá trị ghép nhỏ hơn từ`b`. 

Điều này gợi ý việc sắp xếp cả hai mảng và ghép nối chúng theo cách có cấu trúc thay vì thử tất cả các hoán vị. Chiến lược đúng đắn hóa ra là ghép nối phần tử nhỏ nhất trong`a`với phần tử hợp lệ lớn nhất có thể trong`b`hoặc tương đương, ghép nối từ nhỏ nhất đến lớn nhất theo cách đảo ngược nhất quán. Việc sắp xếp chuyển đổi vấn đề từ kết hợp tổ hợp sang kiểm tra tham lam xác định. 

Lý do điều này có tác dụng là vì nếu có bất kỳ phép gán hợp lệ nào tồn tại thì sẽ có một phép gán duy trì trật tự: việc hoán đổi hai phép gán vi phạm trật tự không bao giờ phá vỡ tính khả thi và thường cải thiện việc phân bổ độ trễ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O (n!) mỗi bài kiểm tra | O(n) | Quá chậm | 
| Tối ưu (sắp xếp + ghép nối tham lam) | O(n log n) mỗi lần kiểm tra | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi muốn kiểm tra xem liệu có tồn tại một cặp phần tử từ`b`sao cho mọi cặp với`a`ở dưới`x`. 

1. Sắp xếp mảng`a`theo thứ tự không giảm và sắp xếp mảng`b`theo thứ tự không giảm. Việc sắp xếp là cần thiết vì nó cho phép chúng ta suy luận về cấu trúc thay vì hoán vị. 
2. Mảng đảo ngược`b`. Sau khi đảo ngược, phần tử lớn nhất của`b`đến trước. Điều này chuẩn bị cho chúng ta một chiến lược ghép đôi tham lam, trong đó chúng ta kiểm soát “ngân sách” nào được chỉ định cho “chi phí” nào. 
3. Đối với mỗi chỉ số`i`từ`0`ĐẾN`n - 1`, kiểm tra xem`a[i] + b[n - 1 - i] <= x`. Điều này ghép phần tử nhỏ nhất của`a`với phần tử lớn nhất của`b`, nhỏ thứ hai với lớn thứ hai, v.v. 

Lý do đằng sau sự ghép đôi này là các giá trị lớn trong`b`là “đắt” về mặt lãng phí. Gán chúng cho các giá trị nhỏ trong`a`ngăn cản chúng cản trở tính khả thi đối với các giá trị lớn hơn trong`a`. 

Nếu bất kỳ cặp nào vi phạm điều kiện, chúng tôi ngay lập tức kết luận rằng không có sự sắp xếp lại hợp lệ nào tồn tại. 

### Tại sao nó hoạt động 

Sau khi sắp xếp cả hai mảng, hãy xem xét mọi phép gán hợp lệ giữa chúng. Nếu tồn tại một phép gán giao nhau trong đó phần tử lớn hơn trong`a`được ghép nối với một phần tử lớn hơn trong`b`trong khi một phần tử nhỏ hơn trong`a`được ghép nối với một phần tử nhỏ hơn trong`b`, việc hoán đổi hai cặp này không làm xấu đi bất kỳ hạn chế nào và thường cải thiện tính khả thi. Việc áp dụng nhiều lần các hoán đổi như vậy sẽ biến đổi bất kỳ giải pháp hợp lệ nào thành một giải pháp khớp với các đầu đối diện của mảng được sắp xếp. Điều này chứng tỏ rằng việc kiểm tra ghép nối ngược là đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        line = input().strip()
        while line == "":
            line = input().strip()
        n, x = map(int, line.split())
        
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))
        
        a.sort()
        b.sort(reverse=True)
        
        ok = True
        for i in range(n):
            if a[i] + b[i] > x:
                ok = False
                break
        
        print("Yes" if ok else "No")

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách đọc cẩn thận từng trường hợp thử nghiệm, bao gồm cả việc xử lý các dòng trống phân tách các trường hợp. Sắp xếp`a`theo thứ tự tăng dần và`b`theo thứ tự giảm dần thiết lập cấu trúc tham lam. 

Vòng lặp lõi kiểm tra việc ghép nối duy nhất mà chúng tôi cần xác minh. Thời điểm vi phạm xuất hiện, chúng tôi có thể dừng sớm vì không việc sắp xếp lại các phần tử còn lại có thể khắc phục được ràng buộc không thành công ở một cặp chỉ mục cố định. 

Một chi tiết triển khai tinh tế đang đảo ngược`b`thay vì sắp xếp cả hai theo cùng một hướng và lập chỉ mục từ các đầu đối diện. Cả hai đều tương đương, nhưng việc đảo ngược làm cho logic ghép nối trở nên rõ ràng hơn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 3, x = 4
a = [1, 2, 3]
b = [1, 1, 2]
```| Bước | một [tôi] | b[i] | tổng hợp | hợp lệ | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 2 | 3 | vâng | 
| 1 | 2 | 1 | 3 | vâng | 
| 2 | 3 | 1 | 4 | vâng | 

Tất cả các cặp đều thỏa mãn ràng buộc, vì vậy câu trả lời là Có. 

Điều này chứng tỏ giá trị lớn nhất trong`b`được dành riêng cho giá trị nhỏ nhất trong`a`, duy trì tính khả thi cho những ràng buộc khó khăn hơn sau này. 

### Ví dụ 2 

đầu vào:```
n = 2, x = 6
a = [1, 4]
b = [2, 5]
```Sau khi sắp xếp và đảo ngược`b`:`a = [1, 4]`,`b = [5, 2]`| Bước | một [tôi] | b[i] | tổng hợp | hợp lệ | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 5 | 6 | vâng | 
| 1 | 4 | 2 | 6 | vâng | 

Điều này cho thấy giải pháp đúng không phải là giữ nguyên trật tự ban đầu mà là kết hợp các thái cực. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t · n log n) | việc sắp xếp thống trị từng trường hợp thử nghiệm | 
| Không gian | O(n) | lưu trữ mảng cho mỗi bài kiểm tra | 

Cho rằng`n ≤ 50`Và`t ≤ 100`, tổng công việc là cực kỳ nhỏ ngay cả với chi phí Python. Sắp xếp có hiệu quả theo thời gian liên tục trong thực tế ở quy mô này. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdin.read()

# We redefine solver for testing
def solve_input(inp):
    import sys
    from io import StringIO
    sys.stdin = StringIO(inp)

    def solve():
        t = int(input())
        out = []
        for _ in range(t):
            line = input().strip()
            while line == "":
                line = input().strip()
            n, x = map(int, line.split())
            a = list(map(int, input().split()))
            b = list(map(int, input().split()))
            a.sort()
            b.sort(reverse=True)
            ok = all(a[i] + b[i] <= x for i in range(n))
            out.append("Yes" if ok else "No")
        return "\n".join(out)

    return solve()

# provided samples
assert solve_input("""4
3 4
1 2 3
1 1 2

2 6
1 4
2 5

4 4
1 2 3 4
1 2 3 4

1 5
5
5
""") == """Yes
Yes
No
No"""

# custom cases
assert solve_input("""1
1 10
5
4
""") == "Yes"

assert solve_input("""1
1 5
5
5
""") == "No"

assert solve_input("""1
3 10
3 3 3
3 3 3
""") == "Yes"

assert solve_input("""1
3 5
1 2 3
3 3 3
""") == "No"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cặp duy nhất khả thi | Có | trường hợp tối thiểu | 
| cặp đơn chặt thất bại | Không | bình đẳng ranh giới | 
| mảng giống hệt nhau hợp lệ | Có | trường hợp đối xứng | 
| phân phối không thể | Không | phát hiện tham lam | 

## Vỏ cạnh 

Trường hợp cạnh tối thiểu là khi`n = 1`. Thuật toán rút gọn thành một so sánh duy nhất và việc sắp xếp không có tác dụng. Việc ghép nối trực tiếp kiểm tra xem tổng duy nhất có thể có phù hợp với`x`. 

Trường hợp ranh giới chặt chẽ xảy ra khi`a[i] + b[j]`bằng chính xác`x`cho tất cả các cặp. Thuật toán chấp nhận điều này vì nó chỉ từ chối các vi phạm nghiêm ngặt và điều này xác nhận tính đúng đắn trong các ràng buộc bình đẳng. 

Tình huống dễ xảy ra lỗi là khi cả hai mảng đều chứa các giá trị lớn lặp lại. Ví dụ,`a = [4, 4, 4]`,`b = [3, 3, 3]`,`x = 6`. Việc kiểm tra ghép nối tham lam`4 + 3 = 7`, từ chối ngay lập tức, điều này phù hợp với việc không thể sắp xếp lại bất kỳ sự sắp xếp nào vì tất cả các cặp đều giống hệt nhau về hoán vị.
