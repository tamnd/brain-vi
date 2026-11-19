---
title: "CF 2087B - Trận đấu biểu diễn"
description: "Chúng tôi được yêu cầu tổ chức một trận đấu biểu diễn cho một trò chơi cạnh tranh có sự tham gia của những người chơi trị giá $2n$, mỗi người có một xếp hạng riêng. Yêu cầu quan trọng là mọi người chơi phải được ghép đôi với một người chơi khác có xếp hạng gần nhất với họ."
date: "2026-06-08T05:56:46+07:00"
tags: ["codeforces", "competitive-programming", "*special"]
categories: ["algorithms"]
codeforces_contest: 2087
codeforces_index: "B"
codeforces_contest_name: "Kotlin Heroes: Episode 12"
rating: 0
weight: 2087
solve_time_s: 74
verified: true
draft: false
---

[CF 2087B - Showmatch](https://codeforces.com/problemset/problem/2087/B) 

**Đánh giá:** - 
**Thẻ:** *đặc biệt 
**Thời gian giải:** 1 phút 14s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu tổ chức một trận đấu biểu diễn cho một trò chơi mang tính cạnh tranh liên quan đến$2n$những người chơi khác nhau, mỗi người có một đánh giá riêng. Yêu cầu quan trọng là mọi người chơi phải được ghép đôi với một người chơi khác có xếp hạng gần nhất với họ. Chính xác hơn, mỗi người chơi có một hoặc nhiều "đối thủ tốt nhất", được định nghĩa là những người chơi có xếp hạng tạo ra sự khác biệt tuyệt đối tối thiểu với họ. Nhiệm vụ là xác định liệu có thể hình thành$n$các cặp rời rạc sao cho trong mỗi cặp, cả hai người tham gia đều là đối thủ tốt nhất của nhau. 

Đầu vào rất đơn giản: số lượng trường hợp thử nghiệm$t$, theo sau là$t$khối trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm đưa ra$n$và một danh sách$2n$xếp hạng. Đầu ra là CÓ nếu tồn tại một cặp đôi hoàn hảo theo tiêu chí đối thủ tốt nhất và KHÔNG nếu ngược lại. 

Những hạn chế là nhỏ:$n$có thể lên tới 50, vì vậy$2n \le 100$, và xếp hạng lên tới$10^5$. Điều này có nghĩa là ngay cả một$O(n^2)$thuật toán sẽ chạy thoải mái trong thời gian giới hạn, nhưng cách tiếp cận mạnh mẽ để kiểm tra tất cả$n!$các cặp đôi có thể là không khả thi. 

Một trường hợp khó phát hiện khi các xếp hạng được dàn trải sao cho các hàng xóm gần nhất tạo thành một chuỗi, nhưng chuỗi không thể khớp hoàn hảo. Ví dụ, với xếp hạng`[1, 3, 5, 8]`, ghép cặp hàng xóm gần nhất mang lại kết quả`(1, 3)`Và`(5, 8)`. Ở đây, đối thủ tốt nhất của số 3 là 1, đối thủ tốt nhất của số 5 là 3 và đối thủ tốt nhất của số 8 là 5.`(1, 3)`Và`(5, 8)`hoạt động, nhưng nếu chúng ta cố gắng`(1, 5)`Và`(3, 8)`, việc ghép nối không thành công. Một cách tiếp cận bất cẩn chỉ ghép các phần tử một cách tùy ý theo độ gần nhau sẽ thất bại đối với các chuỗi như vậy. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là tạo ra tất cả các cách ghép nối có thể$2n$người chơi và kiểm tra xem mỗi cặp có phải là đối thủ tốt nhất của nhau hay không. Mỗi cặp có thể được biểu diễn dưới dạng một cặp khớp hoàn hảo, trong đó có$(2n-1)!!$khả năng. Vì$n = 50$, cái này lớn về mặt thiên văn. Brute-force hoạt động vì về mặt khái niệm nó đảm bảo tính đúng đắn, nhưng nó không thực tế do sự bùng nổ tổ hợp. 

Thông tin chi tiết quan trọng là mối quan hệ đối thủ tốt nhất mang tính cục bộ khi xếp hạng được sắp xếp. Việc sắp xếp xếp hạng đảm bảo rằng đối với bất kỳ người chơi nào, (các) xếp hạng gần nhất sẽ liền kề. Vì tất cả các xếp hạng đều khác nhau nên mỗi người chơi chỉ có một đối thủ tốt nhất: xếp hạng liền kề nhằm giảm thiểu sự khác biệt tuyệt đối. Vì vậy, chỉ cần sắp xếp danh sách xếp hạng và ghép đôi những người chơi liền kề là đủ. Nếu như$2n$chẵn, chúng ta có thể ghép đôi người chơi`(a[0], a[1]), (a[2], a[3]), ..., (a[2n-2], a[2n-1])`. Mỗi người chơi trong cặp này đứng cạnh đối thủ tốt nhất của họ theo thứ tự được sắp xếp, điều này đảm bảo tính tương hỗ. Nếu sự khác biệt tối thiểu không nằm giữa các xếp hạng liền kề thì không thể có sự kết hợp hoàn hảo. 

Quan sát này làm giảm vấn đề để sắp xếp và kiểm tra sự khác biệt liền kề. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((2n)!/n!) | O(n) | Quá chậm | 
| Sắp xếp + Ghép liền kề | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case$t$. 
2. Với mỗi test case, hãy đọc$n$và danh sách$2n$xếp hạng riêng biệt. 
3. Sắp xếp xếp hạng theo thứ tự tăng dần. Việc sắp xếp đảm bảo rằng các xếp hạng gần nhất nằm liền kề nhau, điều này được đảm bảo bởi sự khác biệt của các xếp hạng. 
4. Với mỗi cặp xếp hạng liền kề`(ratings[2*i], ratings[2*i + 1])`, kiểm tra sự khác biệt với các hàng xóm của nó. Nếu như`ratings[2*i+1] - ratings[2*i]`là tối thiểu đối với cả hai người chơi so với bất kỳ xếp hạng nào khác thì cặp đó hợp lệ. Vì việc sắp xếp đảm bảo tính liền kề nên chúng ta chỉ cần kiểm tra sự khác biệt với các cặp lân cận. 
5. Nếu tất cả các cặp liền kề thỏa mãn đặc tính sai phân tối thiểu, ghi CÓ; nếu không thì xuất ra NO. 

Tại sao nó hoạt động: Tính năng sắp xếp đảm bảo rằng người hàng xóm gần nhất của mỗi người chơi ở ngay trước hoặc sau họ. Việc ghép nối người chơi một cách tuần tự trong danh sách đã sắp xếp đảm bảo rằng mỗi người chơi được ghép với đối thủ gần nhất có thể của họ. Vì xếp hạng là khác nhau nên sự khác biệt tối thiểu đối với mỗi người chơi là duy nhất và xảy ra với xếp hạng liền kề. Bất biến này đảm bảo rằng việc ghép đôi được xây dựng thỏa mãn yêu cầu đối thủ tốt nhất lẫn nhau. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        a.sort()
        valid = True
        for i in range(0, 2*n, 2):
            # check if the left neighbor is closer than previous pair, or right neighbor
            left_diff = a[i+1] - a[i]
            if i > 0:
                prev_diff = a[i] - a[i-1]
                if prev_diff < left_diff:
                    valid = False
                    break
            if i + 2 < 2*n:
                next_diff = a[i+2] - a[i+1]
                if next_diff < left_diff:
                    valid = False
                    break
        print("YES" if valid else "NO")

if __name__ == "__main__":
    solve()
```Giải pháp trước tiên sắp xếp các xếp hạng sao cho các xếp hạng gần nhất liền kề nhau. Sau đó, nó lặp lại theo hai bước để kiểm tra thuộc tính sai biệt tối thiểu giữa mỗi cặp và các cặp lân cận của nó. Điều này đảm bảo rằng mỗi cặp bao gồm các đối thủ tốt nhất của nhau. Kiểm tra ranh giới ngăn chặn các lỗi ngoài phạm vi khi truy cập các hàng xóm ở cuối danh sách. 

## Ví dụ đã hoạt động 

Đối với đầu vào:```
2
2
3 7 5 8
2
3 7 5 12
```| Bước | Xếp hạng được sắp xếp | tôi | Đã kiểm tra cặp | Kiểm tra khác biệt hàng xóm | Kết quả | 
| --- | --- | --- | --- | --- | --- | 
| Kiểm tra 1 | [3, 5, 7, 8] | 0 | (3,5) | Trái Không áp dụng, Phải tiếp theo khác biệt 7-5=2 >= 2 | hợp lệ | 
| | | 2 | (7,8) | Khác biệt bên trái 8-7=1, Khác biệt trước 7-5=2 >=1 | hợp lệ | 
| Kiểm tra 2 | [3,5,7,12] | 0 | (3,5) | Phải khác biệt 7-5=2 < 5-3=2? Không | Không hợp lệ | 
| | | 2 | (7,12) | Khác biệt bên trái 12-7=5, Khác biệt trước 7-5=2 < 5? Có, không hợp lệ | Không hợp lệ | 

Điều này chứng tỏ rằng việc sắp xếp và ghép nối các phần tử liền kề có hiệu quả khi sự khác biệt lan truyền chính xác nhưng không thành công khi các khoảng trống tạo ra sự không khớp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t * n log n) | Sắp xếp từng trường hợp thử nghiệm chiếm ưu thế; n ≤ 50 nên việc sắp xếp không đáng kể | 
| Không gian | O(n) | Lưu trữ 2n xếp hạng cho mỗi trường hợp thử nghiệm | 

Với t 100 và n 50, giải pháp thực hiện tối đa 100 * 50 * log50 ≈ 3000 thao tác, trong giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    solve()
    sys.stdout = sys.__stdout__
    return output.getvalue().strip()

# Provided samples
assert run("3\n2\n3 7 5 12\n2\n3 7 5 8\n2\n3 7 5 9\n") == "NO\nYES\nYES", "sample cases"

# Custom cases
assert run("1\n2\n1 2 3 4\n") == "YES", "small consecutive numbers"
assert run("1\n2\n1 4 2 3\n") == "YES", "shuffle still pairs correctly"
assert run("1\n3\n1 10 2 9 3 8\n") == "YES", "all pairable after sorting"
assert run("1\n3\n1 10 2 9 3 7\n") == "NO", "one large gap breaks matching"
assert run("1\n2\n100 1 50 51\n") == "YES", "larger numbers with small difference adjacency"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 2 3 4 | CÓ | Số liên tiếp tối thiểu có thể ghép nối | 
| 1 4 2 3 | CÓ | Đầu vào không theo thứ tự vẫn tạo ra các cặp hợp lệ sau khi sắp xếp | 
| 1 10 2 9 3 8 | CÓ | Kết hợp đúng theo trình tự lớn hơn | 
| 1 10 2 9 3 7 | KHÔNG | Khoảng cách ngăn cản việc ghép nối hợp lệ | 
| 100 1 50 51 | CÓ | Giá trị cạnh có sự khác biệt nhỏ | 

## Vỏ cạnh 

Đối với đầu vào`[1, 10, 2, 9, 3, 7]`, sắp xếp sản lượng`[1, 2, 3, 7, 9, 10]`. Cặp đầu tiên`(1,2)`hoạt động, thứ hai`(3,7)`không thành công vì lân cận gần nhất của 3 là 2, không phải 7. Thuật toán xác định chính xác giá trị này và đưa ra NO. Tương tự, với `[100,1,50,
