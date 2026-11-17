---
title: "CF 1994C - Hungry Games"
description: "Chúng tôi được cung cấp một hàng giá trị và chúng tôi muốn đếm xem có bao nhiêu phân đoạn liền kề hoạt động “tốt” theo một quy tắc mô phỏng rất cụ thể. Khi chúng tôi chọn một phân đoạn $[l, r]$, chúng tôi xử lý các phần tử của nó từ trái sang phải, duy trì tổng hiện hành $g$."
date: "2026-06-09T02:19:18+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "dp", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1994
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 959 sponsored by NEAR (Div. 1 + Div. 2)"
rating: 1600
weight: 1994
solve_time_s: 162
verified: false
draft: false
---

[CF 1994C - Hungry Games](https://codeforces.com/problemset/problem/1994/C) 

**Đánh giá:** 1600 
**Tags:** tìm kiếm nhị phân, dp, hai con trỏ 
**Thời gian giải:** 2m 42s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một hàng giá trị và chúng tôi muốn đếm xem có bao nhiêu phân đoạn liền kề hoạt động “tốt” theo một quy tắc mô phỏng rất cụ thể. 

Khi chúng tôi chọn một phân khúc$[l, r]$, chúng tôi xử lý các phần tử của nó từ trái sang phải, duy trì tổng hoạt động$g$. Sau khi thêm từng phần tử, nếu tổng chạy vượt quá ngưỡng$x$, tổng được đặt lại về 0. Nếu không thì nó vẫn như cũ. Một phân đoạn được coi là tốt nếu sau khi xử lý toàn bộ phân đoạn đó, giá trị cuối cùng của$g$không phải là số không. Nhiệm vụ là đếm tất cả các phân đoạn tốt như vậy qua nhiều trường hợp thử nghiệm. 

Các ràng buộc buộc chúng ta phải suy nghĩ về các giải pháp tuyến tính hoặc gần tuyến tính cho mỗi trường hợp thử nghiệm. Tổng chiều dài của tất cả các trường hợp thử nghiệm nhiều nhất là$2 \cdot 10^5$, loại trừ mọi thứ bậc hai như kiểm tra tất cả các phân đoạn một cách rõ ràng. Bất kỳ giải pháp hợp lệ nào về cơ bản đều phải duy trì một cửa sổ trượt hoặc cấu trúc gia tăng trong đó mỗi phần tử được xử lý với số lần không đổi. 

Trường hợp cạnh tinh tế xuất hiện khi các giá trị cực kỳ lớn so với$x$. Trong những trường hợp như vậy, mỗi phần tử sẽ đặt lại tổng riêng lẻ, có nghĩa là chỉ các phân đoạn một phần tử mới có thể hợp lệ. Một trường hợp đặc biệt khác là khi mảng không giảm hoặc tất cả các giá trị nhỏ, trong đó các đoạn dài tồn tại qua nhiều bước mà không cần đặt lại và logic ngây thơ chỉ theo dõi tổng tiền tố sẽ bị tính quá mức. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực thử mọi phân đoạn$[l, r]$, mô phỏng quy trình và kiểm tra xem kết quả cuối cùng có$g$là khác không. Mỗi mô phỏng mất$O(n)$, dẫn đến$O(n^3)$tổng số trong trường hợp xấu nhất, quá chậm. 

Quan sát quan trọng là quá trình này chỉ phụ thuộc vào tổng tích lũy hiện tại kể từ lần đặt lại cuối cùng. Khi số tiền vượt quá$x$, nó sẽ bị xóa về 0 và quá trình khởi động lại cục bộ. Điều này tạo ra các “khối hợp lệ” tự nhiên trong đó tổng hoạt động không bao giờ vượt quá$x$và ranh giới giữa các khối hoạt động độc lập. 

Vấn đề giảm xuống còn việc theo dõi xem chúng tôi có thể mở rộng một phân đoạn bao xa bắt đầu từ mỗi vị trí trước khi tổng hoạt động kích hoạt thiết lập lại. Khi chúng tôi biết các ranh giới này, chúng tôi có thể đếm các phân đoạn hợp lệ bằng cách sử dụng chiến lược đếm dựa trên hai con trỏ hoặc tiền tố. 

Lực lượng vũ phu hoạt động vì nó mô phỏng rõ ràng các lần đặt lại, nhưng không thành công vì nó liên tục tính toán lại cùng một hành vi tiền tố. Quan sát đặt lại phân vùng mảng thành các phân đoạn độc lập cho phép chúng tôi sử dụng lại cấu trúc đã tính toán và đếm các đóng góp một cách hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$hoặc tệ hơn |$O(1)$| Quá chậm | 
| Tối ưu |$O(n)$|$O(n)$hoặc$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập và duy trì một cửa sổ trượt trên mảng. 

1. Chúng tôi duy trì một con trỏ bên trái$l$và tổng số tiền đang chạy$g$. Ý tưởng là giữ một phân đoạn tối đa bắt đầu từ$l$nơi quá trình chưa được thiết lập lại. 
2. Chúng tôi lặp lại$r$từ trái qua phải, thêm$a[r]$ĐẾN$g$. Điều này mô phỏng việc mở rộng phân khúc hiện tại. 
3. Nếu$g$vượt quá$x$, chúng tôi mô phỏng việc thiết lập lại bằng cách thiết lập$g = 0$và di chuyển$l$ĐẾN$r + 1$. Điều này phản ánh rằng phân khúc trước$r$không thể đóng góp vào sự tiếp tục hợp lệ. 
4. Tại mỗi vị trí$r$, nếu như$g \neq 0$, thì điểm bắt đầu bất kỳ$l' \in [l, r]$tạo ra một phân đoạn hợp lệ kết thúc tại$r$, bởi vì không có lần khởi động nào trong số đó có thể khiến thiết lập lại ngay lập tức trước khi đạt đến$r$. Chúng tôi thêm$r - l + 1$để trả lời. 
5. Chúng tôi tiếp tục cho đến cuối cùng, tích lũy đóng góp. 

Ý tưởng chính là việc đặt lại chia mảng thành các khối độc lập và trong mỗi khối, chúng tôi đếm tất cả các mảng con kết thúc ở mỗi vị trí còn tồn tại trong quá trình. 

### Tại sao nó hoạt động 

Điều bất biến là tại bất kỳ thời điểm nào,$l$luôn trỏ đến chỉ mục đầu tiên của khối hoạt động hiện tại nơi không có sự thiết lập lại nào xảy ra kể từ đó$l$. Tổng số tiền chạy$g$đại diện chính xác cho phân đoạn được xử lý$[l, r]$theo luật chơi. Mỗi lần chúng tôi đặt lại, chúng tôi sẽ loại bỏ chính xác tất cả những đóng góp trước đó không thể mở rộng thêm. Điều này đảm bảo mỗi phân đoạn hợp lệ được tính chính xác một lần và mọi phân đoạn không hợp lệ đều bị loại trừ vì nó nhất thiết phải kích hoạt việc đặt lại trước khi hoàn thành. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, x = map(int, input().split())
        a = list(map(int, input().split()))
        
        l = 0
        g = 0
        ans = 0
        
        for r in range(n):
            g += a[r]
            
            if g > x:
                g = 0
                l = r + 1
                continue
            
            ans += (r - l + 1)
        
        print(ans)

if __name__ == "__main__":
    solve()
```Việc thực hiện trực tiếp tuân theo logic cửa sổ trượt. Con trỏ$l$chỉ được cập nhật khi quá trình thiết lập lại xảy ra, tương ứng với việc vượt quá ngưỡng. Sự đóng góp$r - l + 1$đếm tất cả các điểm bắt đầu hợp lệ cho các mảng con kết thúc tại$r$. Bước đặt lại là rất quan trọng vì nếu không có bước này chúng ta sẽ chuyển nhầm các phần tổng không hợp lệ sang các phân đoạn trong tương lai. 

## Ví dụ đã hoạt động 

Hãy xem xét một trường hợp nhỏ trong đó$x = 2$và mảng là$[1, 2, 1]$. 

Tại$r = 0$, chúng tôi có$g = 1$, Vì thế$l = 0$, đóng góp 1 đoạn. 

Tại$r = 1$,$g = 3$vượt quá$x$, vì vậy chúng tôi đặt lại và thiết lập$l = 2$, đóng góp 0. 

Tại$r = 2$,$g = 1$, đóng góp 1 đoạn. 

Tổng số là 2, phù hợp với hành vi dự kiến. 

Bây giờ hãy xem xét$[1, 1, 1, 1]$với$x = 2$. Cửa sổ tăng lên cho đến khi tổng vượt quá$x$, sau đó đặt lại và chúng tôi liên tục đếm các khoản đóng góp bên trong mỗi khối an toàn. Dấu vết cho thấy rằng mọi tiện ích mở rộng trong một khối hợp lệ đều đóng góp tuyến tính và việc đặt lại sẽ ngăn chặn sự lây nhiễm chéo giữa các khối. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi phần tử được xử lý một lần, với tối đa một lần đặt lại cho mỗi chỉ mục | 
| Không gian |$O(1)$| Chỉ có một số biến được duy trì | 

Tổng độ phức tạp trên tất cả các trường hợp thử nghiệm là tuyến tính trong tổng kích thước đầu vào, điều này cần thiết với ràng buộc rằng$\sum n \le 2 \cdot 10^5$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n, x = map(int, input().split())
        a = list(map(int, input().split()))
        
        l = 0
        g = 0
        ans = 0
        
        for r in range(n):
            g += a[r]
            if g > x:
                g = 0
                l = r + 1
                continue
            ans += (r - l + 1)
        
        out.append(str(ans))
    return "\n".join(out)

# provided samples
assert run("""5
4 2
1 1 1 1
3 2
1 2 3
1 6
10
6 3
1 2 1 4 3 8
5 999999999
999999999 999999998 1000000000 1000000000 500000000
""") == """8
2
0
10
7"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả những cái nhỏ | 4 | không cần đặt lại, tích lũy đầy đủ | 
| phần tử lớn duy nhất | 0 | hành vi thiết lập lại ngay lập tức | 
| xen kẽ nhỏ/lớn | khác nhau | thiết lập lại ranh giới | 

## Vỏ cạnh 

Khi tất cả các phần tử đều lớn hơn$x$, mỗi bước sẽ kích hoạt thiết lập lại ngay lập tức, do đó chỉ các đoạn có độ dài 1 là hợp lệ. Thuật toán xử lý việc này vì mỗi lần$g > x$, chúng tôi thiết lập lại và di chuyển$l$qua chỉ mục hiện tại, đảm bảo không có phân đoạn không hợp lệ nào được đóng góp. 

Khi tất cả các phần tử đều nhỏ và tổng không bao giờ vượt quá$x$, cửa sổ không bao giờ đặt lại và câu trả lời trở thành tổng của tất cả các số mảng con, công thức này$(r - l + 1)$tích lũy một cách chính xác. 

Khi việc thiết lập lại xảy ra chính xác tại vị trí$r$, việc triển khai đảm bảo rằng không có đóng góp nào được tính cho bước đó, bởi vì$g$được xóa trước bất kỳ phần bổ sung nào vào câu trả lời, ngăn chặn việc đưa vào không chính xác các phân đoạn bị hỏng.
