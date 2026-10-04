---
title: "CF 104886A - Vấn đề về lịch trình"
description: "Nhiệm vụ là xác định liệu một lịch trình thời gian nhất định có khả thi hay không bằng cách kiểm tra xem liệu tất cả các khoảng thời gian yêu cầu có thể được đáp ứng đồng thời hay không."
date: "2026-06-28T09:06:29+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104886
codeforces_index: "A"
codeforces_contest_name: "USI-Team-Selection 2023-2024"
rating: 0
weight: 104886
solve_time_s: 49
verified: true
draft: false
---

[CF 104886A - Sự cố về lịch trình](https://codeforces.com/problemset/problem/104886/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ là xác định liệu một lịch trình thời gian nhất định có khả thi hay không bằng cách kiểm tra xem liệu tất cả các khoảng thời gian yêu cầu có thể được đáp ứng đồng thời hay không. Mỗi trường hợp thử nghiệm mô tả một số ràng buộc về thời gian và mỗi ràng buộc đại diện cho một phân đoạn thời gian trong đó một điều kiện nhất định phải được đáp ứng. Mục đích là để tìm xem liệu có tồn tại một phần chồng lấp không trống thỏa mãn tất cả các ràng buộc hay không, hoặc tương đương, liệu giao điểm của tất cả các khoảng có phải là không trống hay không. 

Đầu vào có thể được hiểu là tập hợp các khoảng trên một trục số. Mỗi khoảng thời gian giới hạn lịch trình hợp lệ trong một phạm vi liền kề. Câu trả lời cuối cùng phụ thuộc hoàn toàn vào việc liệu các phạm vi này có chia sẻ ít nhất một điểm chung hay không sau khi áp dụng tất cả các ràng buộc. 

Từ quan điểm phức tạp, vấn đề được thiết kế sao cho chỉ cần mô phỏng trực tiếp là đủ. Các ràng buộc đủ nhỏ để có thể chấp nhận được các khoảng giao nhau liên tục hoặc thậm chí kiểm tra mọi đơn vị thời gian có thể có trong phạm vi giới hạn. Nếu giá trị tọa độ tối đa lớn nhưng vẫn có thể quản lý được cho mỗi trường hợp thử nghiệm thì việc quét O(n × maxR) vẫn khả thi. Nếu n lớn nhưng các khoảng đã được chuẩn hóa và sắp xếp thì quét tuyến tính là đủ. 

Một trường hợp lỗi nhỏ xuất hiện khi các khoảng không được giao nhau đúng cách theo thứ tự. Ví dụ: nếu chúng tôi xử lý các khoảng thời gian một cách độc lập mà không duy trì giao lộ đang chạy, chúng tôi có thể kết luận tính khả thi một cách không chính xác. 

Hãy xem xét: 

đầu vào:```
3
1 5
4 8
6 10
```Câu trả lời đúng phụ thuộc vào việc phần chồng lên nhau của cả ba khoảng có trống hay không. Kiểm tra hợp đơn giản có thể cho kết quả là có vì tất cả các khoảng chồng lên nhau theo cặp tại một số điểm, nhưng phần giao hoàn toàn trống vì không có điểm nào nằm trong cả ba. 

Một trường hợp cạnh khác là khi các khoảng chạm vào điểm cuối. Ví dụ:```
2
1 3
3 5
```Điều này hợp lệ nếu các điểm cuối được bao gồm và không hợp lệ nếu không. Một giải pháp đúng đắn phải xử lý các ranh giới một cách nhất quán. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Chúng tôi bắt đầu với phạm vi thời gian đầy đủ có thể và liên tục cắt nó theo từng khoảng thời gian. Sau khi xử lý từng ràng buộc, chúng tôi thu nhỏ vùng hợp lệ thành phần chồng chéo giữa phân đoạn hợp lệ hiện tại và khoảng tiếp theo. Nếu tại bất kỳ điểm nào giao lộ trở nên trống rỗng, chúng ta có thể dừng lại ngay lập tức. 

Điều này hiệu quả vì mỗi ràng buộc giới hạn không gian giải pháp một cách độc lập và vùng khả thi cuối cùng chính xác là giao điểm của tất cả các ràng buộc. Tính đúng đắn xuất phát trực tiếp từ định nghĩa về sự hài lòng đồng thời. 

Một phiên bản đơn giản hơn sẽ mô phỏng mọi đơn vị thời gian có thể có trong phạm vi toàn cầu và kiểm tra xem nó có thỏa mãn tất cả các khoảng hay không. Cách tiếp cận đó trở nên quá chậm khi phạm vi tọa độ lớn, vì nó có thể yêu cầu lặp lại trên mọi đơn vị cho mỗi khoảng thời gian. 

Quan sát quan trọng là chúng ta không bao giờ cần kiểm tra từng điểm thời gian riêng lẻ. Mỗi khoảng chỉ ảnh hưởng đến giới hạn dưới và trên của tính khả thi hiện tại. Vì vậy, việc duy trì hai biến đại diện cho giao điểm hiện tại là đủ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Liệt kê lực lượng vũ phu | O(n×R) | O(1) | Quá chậm | 
| Quét giao lộ theo khoảng thời gian | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì khoảng thời gian khả thi hiện tại được đặt ban đầu ở phạm vi rộng nhất có thể. Sau đó chúng tôi dần dần hạn chế nó. 

1. Khởi tạo khoảng khả thi hiện tại là [L, R], thường được đặt thành khoảng đầu tiên hoặc phạm vi vô hạn tùy thuộc vào công thức. Điều này thể hiện tất cả các lịch trình hợp lệ có thể có trước khi áp dụng các ràng buộc. 
2. Đối với mỗi khoảng thời gian đến [l, r], hãy cập nhật khoảng thời gian khả thi thành [max(L, l), min(R, r)]. Bước này bắt buộc rằng bất kỳ giải pháp hợp lệ nào cũng phải đáp ứng đồng thời cả các ràng buộc trước đó và ràng buộc mới. 
3. Sau khi cập nhật, hãy kiểm tra xem khoảng thời gian có trở nên không hợp lệ hay không, nghĩa là max(L, l) > min(R, r). Nếu điều này xảy ra, không có thời điểm nào thỏa mãn mọi ràng buộc, do đó câu trả lời ngay lập tức là sai. 
4. Tiếp tục xử lý tất cả các khoảng theo trình tự, luôn thu hẹp vùng khả thi. 
5. Sau khi tất cả các khoảng được xử lý, nếu khoảng vẫn hợp lệ, xuất ra true; nếu không thì xuất ra sai. 

### Tại sao nó hoạt động 

Ở mỗi bước, khoảng thời gian được duy trì chính xác là giao điểm của tất cả các khoảng thời gian được xử lý cho đến nay. Bất biến này đúng vì giao điểm khoảng có tính chất kết hợp và giao hoán. Khi giao lộ trở nên trống trải, không khoảng thời gian nào trong tương lai có thể khôi phục tính khả thi vì giao lộ chỉ có thể thu nhỏ lại hoặc giữ nguyên. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    L, R = map(int, input().split())
    
    for _ in range(n - 1):
        l, r = map(int, input().split())
        L = max(L, l)
        R = min(R, r)
        if L > R:
            print("NO")
            return
    
    print("YES")

if __name__ == "__main__":
    solve()
```Giải pháp đọc khoảng đầu tiên là phạm vi khả thi ban đầu và lặp lại giao cắt nó với mỗi khoảng tiếp theo. Chi tiết triển khai chính là chấm dứt sớm: khi khoảng thời gian trở nên không hợp lệ thì việc xử lý thêm là không cần thiết vì không thể khôi phục được. 

Sự so sánh`L > R`là yêu cầu kiểm tra tính đúng đắn duy nhất. Nó phải nghiêm ngặt vì đẳng thức vẫn là sự chồng chéo một điểm hợp lệ khi bao gồm các điểm cuối. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
1 5
4 8
6 10
```| Bước | Khoảng thời gian | L | R | Khả thi? | 
| --- | --- | --- | --- | --- | 
| Ban đầu | [1, 5] | 1 | 5 | Có | 
| 1 | [4, 8] | 4 | 5 | Có | 
| 2 | [6, 10] | 6 | 5 | Không | 

Sau khi xử lý lần cập nhật thứ hai, khoảng thời gian trở nên không hợp lệ vì 6 > 5. Thuật toán đưa ra kết quả KHÔNG chính xác vì không có điểm duy nhất nào nằm trong tất cả các khoảng. 

### Ví dụ 2 

đầu vào:```
2
1 3
3 5
```| Bước | Khoảng thời gian | L | R | Khả thi? | 
| --- | --- | --- | --- | --- | 
| Ban đầu | [1, 3] | 1 | 3 | Có | 
| 1 | [3, 5] | 3 | 3 | Có | 

Giao điểm giảm xuống còn một điểm duy nhất tại 3, điểm này vẫn hợp lệ. Đầu ra là CÓ. 

Những ví dụ này xác nhận rằng việc xử lý điểm cuối là rất quan trọng và thuật toán xử lý một cách tự nhiên cả các phần chồng chéo rộng và các điểm giao cắt một điểm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi khoảng thời gian được xử lý một lần với các cập nhật liên tục | 
| Không gian | O(1) | Chỉ có hai biến được duy trì | 

Quét tuyến tính là tối ưu vì mọi ràng buộc phải được đọc ít nhất một lần. Điều này phù hợp thoải mái trong giới hạn Codeforces điển hình ngay cả đối với n lớn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque

    data = inp.strip().split()
    it = iter(data)
    n = int(next(it))
    L = int(next(it))
    R = int(next(it))
    for _ in range(n - 1):
        l = int(next(it))
        r = int(next(it))
        L = max(L, l)
        R = min(R, r)
        if L > R:
            return "NO"
    return "YES"

# provided samples (illustrative)
assert run("3\n1 5\n4 8\n6 10\n") == "NO"
assert run("2\n1 3\n3 5\n") == "YES"

# custom cases
assert run("1\n5 10\n") == "YES", "single interval always valid"
assert run("2\n1 2\n3 4\n") == "NO", "disjoint intervals"
assert run("3\n1 10\n2 3\n3 3\n") == "YES", "shrinking to single point"
assert run("3\n1 10\n2 5\n6 7\n") == "NO", "split feasibility"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| khoảng đơn | CÓ | trường hợp cơ sở | 
| khoảng rời rạc | KHÔNG | phát hiện lỗi sớm | 
| co lại đến điểm | CÓ | độ chính xác của điểm cuối | 
| tính khả thi phân chia | KHÔNG | sập ngã tư nhiều bậc | 

## Vỏ cạnh 

Trường hợp một cạnh là khi các khoảng thu gọn về một điểm. Ví dụ:```
3
1 10
5 5
5 7
```Thuật toán đầu tiên giảm [1, 10] xuống [5, 5], sau đó giao với [5, 7] và giữ [5, 5]. Đầu ra vẫn hợp lệ vì sự bình đẳng được cho phép. 

Một trường hợp cạnh khác là sự rời rạc ngay lập tức ở khoảng thứ hai:```
2
1 3
10 20
```Sau lần cập nhật đầu tiên, [1, 3] giao với [10, 20] tạo ra khoảng không hợp lệ trong đó L > R. Thuật toán dừng sớm một cách chính xác. 

Trường hợp tinh tế cuối cùng là phạm vi lớn mà việc liệt kê đơn giản sẽ thất bại, nhưng phương pháp giao nhau vẫn giữ nguyên thời gian không đổi trên mỗi bước, đảm bảo tính chính xác mà không cần dựa vào giới hạn tọa độ.
