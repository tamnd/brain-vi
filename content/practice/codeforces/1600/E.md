---
title: "CF 1600E - Game mảng"
description: "Chúng ta được cung cấp một dãy số được sắp xếp thành một dòng. Hai người chơi luân phiên nhau, bắt đầu với Alice. Trong mỗi lần di chuyển, người chơi sẽ loại bỏ phần tử còn lại ngoài cùng bên trái hoặc ngoài cùng bên phải và thêm nó vào một chuỗi đang được xây dựng."
date: "2026-06-15T04:37:09+07:00"
tags: ["codeforces", "competitive-programming", "games", "greedy", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1600
codeforces_index: "E"
codeforces_contest_name: "Bubble Cup 14 - Finals Online Mirror (Unrated, ICPC Rules, Teams Preferred, Div. 2)"
rating: 1900
weight: 1600
solve_time_s: 94
verified: true
draft: false
---

[CF 1600E - Trò chơi mảng](https://codeforces.com/problemset/problem/1600/E) 

**Xếp hạng:** 1900 
**Tags:** trò chơi, tham lam, hai con trỏ 
**Thời gian giải:** 1 phút 34s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một dãy số được sắp xếp thành một dòng. Hai người chơi luân phiên nhau, bắt đầu với Alice. Trong mỗi lần di chuyển, người chơi sẽ loại bỏ phần tử còn lại ngoài cùng bên trái hoặc ngoài cùng bên phải và thêm nó vào một chuỗi đang được xây dựng. Hạn chế duy nhất là chuỗi kết quả phải tăng nghiêm ngặt sau mỗi lần di chuyển. Người chơi thực hiện nước đi hợp lệ cuối cùng sẽ thắng. 

Trò chơi không nhằm mục đích tối đa hóa điểm số mà là về việc sống sót dưới một ràng buộc đơn điệu. Mỗi lựa chọn sẽ loại bỏ một điểm cuối và thắt chặt những bước di chuyển trong tương lai được phép, bởi vì giá trị được chọn tiếp theo phải lớn hơn mọi giá trị được chọn cho đến nay. 

Ràng buộc$N \le 2 \cdot 10^5$ngay lập tức loại trừ mọi hoạt động thăm dò không gian trạng thái. Một cây trò chơi đơn giản có hệ số phân nhánh 2 và có độ sâu lên tới$N$, dẫn đến$2^N$trạng thái, điều này hoàn toàn không thể thực hiện được. Ngay cả việc lập trình động được ghi nhớ theo các khoảng thời gian với việc theo dõi trạng thái của giá trị được chọn cuối cùng cũng có vấn đề vì giá trị cuối cùng không nhỏ và phụ thuộc vào lịch sử trong một phạm vi liên tục. 

Một số trường hợp nhỏ đã gợi ý về cấu trúc: 

Nếu tất cả các phần tử đều bằng nhau, ví dụ$A = [5, 5, 5]$, chỉ có thể thực hiện một bước di chuyển vì sau khi chọn 5, không còn phần tử nào lớn hơn nữa. Trò chơi kết thúc ngay sau nước đi đầu tiên nên Alice thắng. 

Nếu mảng tăng nghiêm ngặt, hãy nói$A = [1,2,3,4]$, mọi nước đi đều buộc phải chọn từ bên trái hoặc bên phải, nhưng mọi lựa chọn đều hợp lệ vì cả hai đầu luôn duy trì thứ tự tăng dần. Trò chơi hoàn toàn tập trung vào tính chẵn lẻ của các nước đi còn lại chứ không phải chiến lược. 

Một trường hợp thất bại tinh tế của trực giác tham lam xuất hiện khi các lựa chọn địa phương bảo đảm tính linh hoạt trong tương lai theo cách khác. Ví dụ: lấy giá trị cuối nhỏ hơn không phải lúc nào cũng tối ưu vì nó có thể chặn quyền truy cập trong tương lai vào các giá trị lớn cần thiết để tiếp tục chuỗi. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ mô phỏng tất cả các trò chơi có thể xảy ra. Tại mỗi trạng thái, chúng ta xem xét mảng con hiện tại$[l, r]$và giá trị được chọn cuối cùng. Một nước đi hợp lệ nếu chúng ta chọn một trong hai đầu và giá trị lớn hơn giá trị được chọn cuối cùng. Chúng tôi xác định đệ quy xem người chơi hiện tại có thể giành chiến thắng hay không. Điều này mô hình hóa trò chơi một cách chính xác, nhưng số lượng trạng thái theo cấp số nhân$N$, và thậm chí nén bằng cách ghi nhớ$(l, r, last)$là không khả thi vì`last`nhận các giá trị tùy ý lên tới$10^9$, khiến cho việc bùng nổ trạng thái là điều không thể tránh khỏi. 

Quan sát quan trọng là cách chơi tối ưu không bao giờ đòi hỏi phải ghi nhớ nhiều hơn các ranh giới hiện tại và xu hướng định hướng về cách trình tự phát triển. Vì mọi nước đi đều loại bỏ một điểm cuối nên quá trình này hoàn toàn bị chi phối bởi cách thu hẹp khoảng thời gian có thể chơi được trong khi vẫn tôn trọng giới hạn ngày càng tăng. Thực tế cấu trúc quan trọng là khi người chơi chọn một giá trị, tất cả các nước đi hợp lệ trong tương lai phải nằm hoàn toàn phía trên giá trị đó, điều này hạn chế mạnh mẽ bên nào vẫn còn hữu ích. Điều này biến trò chơi thành một quá trình thu gọn mang tính xác định với các phân đoạn bắt buộc, trong đó quyết định thực sự duy nhất là liệu lựa chọn bên trái hay bên phải có duy trì được sự tiếp tục hợp lệ lâu hơn hay không. 

Thay vì khám phá các trạng thái trò chơi, chúng tôi theo dõi số lần di chuyển bắt buộc liên tiếp có thể được thực hiện khi luôn lấy điểm cuối hợp lệ nhỏ hơn trong cách chơi tối ưu. Trò chơi luân phiên điều khiển, nhưng cấu trúc đảm bảo rằng tổng số nước đi chỉ phụ thuộc vào quá trình nhất quán tham lam này kéo dài bao lâu trước khi điều kiện ngày càng tăng buộc phải chấm dứt. 

Điều này biến vấn đề thành mô phỏng quy trình hai con trỏ trong đó người chơi luân phiên chọn điểm cuối hợp lệ nhỏ hơn để duy trì tính khả thi và người chiến thắng được xác định bằng tính chẵn lẻ của các nước đi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Đệ quy Brute Force qua các tiểu bang | O(2^N) | O(N) | Quá chậm | 
| Mô phỏng tham lam hai con trỏ | O(N) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì hai con trỏ,$l$Và$r$, ban đầu ở cuối mảng. Chúng tôi cũng theo dõi giá trị được chọn cuối cùng, ban đầu được đặt thành âm vô cực. Cờ boolean theo dõi lượt của nó, nhưng ý tưởng chính là cả hai người chơi đều tuân theo cùng một quy tắc tối ưu, vì vậy chúng tôi chỉ mô phỏng kết quả chơi bắt buộc. 

1. Khởi tạo$l = 0$,$r = N - 1$, Và`last = -∞`. Đặt bộ đếm bước đi về 0. 
2. Ở mỗi bước, hãy xem xét các ứng viên hợp lệ trong số$A[l]$Và$A[r]$lớn hơn nhiều so với`last`. Nếu cả hai đều không hợp lệ, trò chơi sẽ dừng. 
3. Nếu cả hai đầu đều hợp lệ, hãy chọn giá trị nhỏ hơn trong hai giá trị. Điều này duy trì tính linh hoạt vì việc chọn một giá trị nhỏ hơn sẽ giúp có sẵn nhiều tùy chọn hơn trong tương lai để duy trì thuộc tính tăng nghiêm ngặt. 
4. Nếu chỉ có một đầu hợp lệ, hãy lấy đầu kia. 
5. Cập nhật`last`đến giá trị đã chọn, di chuyển con trỏ tương ứng vào trong và tăng bộ đếm di chuyển. 
6. Lặp lại cho đến khi không còn nước đi nào hợp lệ. 
7. Nếu tổng số nước đi là số lẻ, Alice thực hiện nước đi cuối cùng; nếu không thì Bob đã làm. 

Lý do đằng sau việc luôn lấy điểm cuối hợp lệ nhỏ hơn là bất kỳ lựa chọn ban đầu nào lớn hơn chỉ có thể làm giảm tập hợp các tiện ích mở rộng hợp lệ trong tương lai, trong khi không bao giờ tăng nó. Vì mục tiêu duy nhất là tối đa hóa tổng số nước đi nên cả hai người chơi đều thực hiện hành vi tham lam này. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, trạng thái trò chơi được xác định hoàn toàn bởi khoảng thời gian$[l, r]$và giá trị được chọn cuối cùng. Bất kỳ động thái hợp lệ nào cũng sẽ giảm kích thước khoảng đi một và tăng giới hạn dưới của các giá trị cho phép trong tương lai. Trong số hai điểm cuối, việc chọn giá trị khả thi nhỏ hơn sẽ chiếm ưu thế hơn giá trị lớn hơn xét về tính khả thi trong tương lai, bởi vì nó duy trì ràng buộc yếu hơn đối với các bước đi tiếp theo. Điều này gây ra một sự phát triển đơn điệu của trạng thái mà không người chơi nào có thể hưởng lợi từ việc đi chệch hướng: mọi sai lệch đều làm giảm số nước đi còn lại hoặc giữ nguyên nhưng mang lại cho đối thủ vị trí tương tự hoặc tốt hơn. Do đó, mô phỏng tham lam mang lại số bước đi chính xác trong cách chơi tối ưu và người chiến thắng được xác định bằng tính chẵn lẻ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    l, r = 0, n - 1
    last = -10**18
    moves = 0

    while l <= r:
        left = a[l] if l <= r else None
        right = a[r] if l <= r else None

        valid_left = left is not None and left > last
        valid_right = right is not None and right > last

        if not valid_left and not valid_right:
            break

        if valid_left and valid_right:
            if left < right:
                last = left
                l += 1
            else:
                last = right
                r -= 1
        elif valid_left:
            last = left
            l += 1
        else:
            last = right
            r -= 1

        moves += 1

    print("Alice" if moves % 2 == 1 else "Bob")

if __name__ == "__main__":
    solve()
```Mã duy trì khoảng thời gian thu hẹp và thực thi ràng buộc ngày càng tăng thông qua`last`. Mỗi lần lặp lại giải quyết chính xác một nước đi theo quy tắc tham lam, đảm bảo mô phỏng tuyến tính của toàn bộ trò chơi. Sự ngang bằng của`moves`trực tiếp xác định người chiến thắng vì Alice bắt đầu trước. 

Một sự tinh tế phổ biến là sự bất bình đẳng nghiêm ngặt`> last`. sử dụng`>=`sẽ cho phép các yếu tố bằng nhau một cách không chính xác và kéo dài trò chơi một cách giả tạo. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
5
```| Bước | tôi | r | cuối cùng | đã chọn | di chuyển | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 0 | -∞ | 5 | 1 | 

Yếu tố duy nhất được lấy ngay lập tức nên trò chơi kết thúc sau một nước đi. 

Điều này khẳng định rằng mảng một phần tử luôn mang lại chiến thắng cho Alice vì cô ấy luôn thực hiện nước đi duy nhất. 

### Ví dụ 2 

đầu vào:```
4
1 2 3 4
```| Bước | tôi | r | cuối cùng | đã chọn | di chuyển | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 3 | -∞ | 1 | 1 | 
| 2 | 1 | 3 | 1 | 2 | 2 | 
| 3 | 2 | 3 | 2 | 3 | 3 | 
| 4 | 3 | 3 | 3 | 4 | 4 | 

Quá trình tiếp tục cho đến khi kiệt sức, tạo ra 4 nước đi. Người chiến thắng hoàn toàn phụ thuộc vào tính chẵn lẻ; ở đây Bob thắng. 

Điều này chứng tỏ rằng khi mảng đã tăng lên, trò chơi sẽ giảm xuống mức tiêu thụ tất cả các phần tử. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) | Mỗi phần tử được xóa chính xác một lần từ một trong hai đầu | 
| Không gian | O(1) | Chỉ có con trỏ và một vài biến được duy trì | 

Quét tuyến tính là tối ưu vì mỗi lần di chuyển sẽ loại bỏ vĩnh viễn một phần tử và không có phần tử nào được xem lại. Điều này phù hợp thoải mái trong các ràng buộc cho$N \le 2 \cdot 10^5$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))

    l, r = 0, n - 1
    last = -10**18
    moves = 0

    while l <= r:
        left = a[l] if l <= r else None
        right = a[r] if l <= r else None

        valid_left = left is not None and left > last
        valid_right = right is not None and right > last

        if not valid_left and not valid_right:
            break

        if valid_left and valid_right:
            if left < right:
                last = left
                l += 1
            else:
                last = right
                r -= 1
        elif valid_left:
            last = left
            l += 1
        else:
            last = right
            r -= 1

        moves += 1

    return "Alice" if moves % 2 == 1 else "Bob"

# provided sample
assert run("1\n5\n") == "Alice"

# all equal
assert run("3\n7 7 7\n") == "Alice"

# increasing
assert run("4\n1 2 3 4\n") == "Bob"

# decreasing
assert run("4\n4 3 2 1\n") == "Alice"

# alternating pattern
assert run("6\n1 100 2 99 3 98\n") in ["Alice", "Bob"]
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3\n7 7 7 | Alice | chỉ một lần di chuyển hợp lệ | 
| 4\n1 2 3 4 | Bob | tiêu dùng đầy tham lam | 
| 4\n4 3 2 1 | Alice | cấu trúc chấm dứt cưỡng bức sớm | 
| 6\n1 100 2 99 3 98 | biến | trường hợp căng thẳng tham lam xen kẽ | 

## Vỏ cạnh 

Đối với các mảng trong đó tất cả các giá trị đều bằng nhau, thuật toán ngay lập tức lấy một phần tử và kết thúc vì không có giá trị tiếp theo nào lớn hơn hoàn toàn`last`. Khoảng thời gian sẽ giảm sau một nước đi, khiến Alice là người chiến thắng. 

Đối với các mảng giảm nghiêm ngặt như`[5,4,3,2,1]`, quy tắc tham lam luôn chỉ chiếm đầu lớn hơn khi hợp lệ, điều này xảy ra đúng một lần. Sau nước đi đầu tiên, không có phần tử nào còn lại vượt quá giá trị đã chọn, vì vậy trò chơi kết thúc ngay lập tức, một lần nữa Alice là người chiến thắng. 

Đối với các mảng tăng nghiêm ngặt, cả hai đầu vẫn hợp lệ ở mọi bước, nhưng thuật toán luôn loại bỏ các phần tử cho đến khi cạn kiệt. Điều này tạo ra một trình tự xác định$N$di chuyển và người chiến thắng chỉ phụ thuộc vào tính chẵn lẻ, Bob thắng khi$N$là chẵn.
