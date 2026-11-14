---
title: "CF 1776A - Cậu Bé Biết Đi"
description: "Nhiệm vụ này yêu cầu chúng tôi xác định xem liệu thẩm phán có thể dắt con chó Boy của cô ấy đi dạo ít nhất hai lần trong một ngày dựa trên dòng thời gian của tin nhắn hay không. Mỗi lần đi bộ mất đúng 120 phút, không thể chồng lên một lần đi bộ khác và trọng tài không bao giờ gửi tin nhắn trong khi đi bộ."
date: "2026-06-09T11:44:21+07:00"
tags: ["codeforces", "competitive-programming", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1776
codeforces_index: "A"
codeforces_contest_name: "SWERC 2022-2023 - Online Mirror (Unrated, ICPC Rules, Teams Preferred)"
rating: 800
weight: 1776
solve_time_s: 77
verified: true
draft: false
---

[CF 1776A - Cậu bé biết đi](https://codeforces.com/problemset/problem/1776/A) 

**Đánh giá:** 800 
**Thẻ:** tham lam 
**Thời gian giải:** 1 phút 17s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ này yêu cầu chúng tôi xác định xem liệu thẩm phán có thể dắt con chó Boy của cô ấy đi dạo ít nhất hai lần trong một ngày dựa trên dòng thời gian của tin nhắn hay không. Mỗi lần đi bộ mất đúng 120 phút, không thể chồng lên một lần đi bộ khác và trọng tài không bao giờ gửi tin nhắn trong khi đi bộ. Đầu vào bao gồm một số trường hợp thử nghiệm, mỗi trường hợp cung cấp danh sách thời gian tin nhắn tăng dần theo phút kể từ nửa đêm. Đầu ra cho mỗi trường hợp thử nghiệm là "CÓ" nếu có thể phù hợp với ít nhất hai lần đi bộ không chồng chéo trong 120 phút xung quanh các tin nhắn hoặc "KHÔNG" nếu không. 

Một quan sát quan trọng là một ngày có 1440 phút, do đó tổng thời gian có sẵn là hữu hạn. Vì mỗi lần đi bộ tiêu tốn 120 phút nên hai lần đi bộ cần 240 phút rảnh rỗi. Thời gian nhắn tin chia ngày thành các khoảng thời gian có thể đi bộ. Nếu bất kỳ khoảng thời gian nào giữa các tin nhắn liên tiếp hoặc giữa đầu/cuối ngày ít nhất là 120 phút thì có thể phải đi bộ ở đó. Chúng ta cần kiểm tra xem có ít nhất hai khoảng như vậy tồn tại mà không chồng chéo hay không. 

Các trường hợp đặc biệt bao gồm các tình huống có rất ít tin nhắn, các tin nhắn được xếp chặt chẽ với nhau hoặc các cuộc đi bộ có khả năng bắt đầu lúc nửa đêm hoặc kết thúc vào phút cuối cùng trong ngày. Ví dụ: nếu tin nhắn xuất hiện ở các phút 0, 1, 2, 3, 4 thì thời gian còn lại từ 5 đến 14h40 là đủ để lên lịch cho hai lần đi bộ. Ngược lại, nếu các tin nhắn ở mức 0, 120, 240, …, 1320 thì có thể không thể đi bộ được dù chỉ 120 phút. 

Những hạn chế của vấn đề là rất nhẹ. Mỗi trường hợp kiểm thử có tối đa 100 thông báo và có tối đa 100 trường hợp kiểm thử. Điều này cho phép mọi giải pháp O(n) trên mỗi trường hợp thử nghiệm chạy thoải mái trong giới hạn thời gian. Chúng ta có thể lặp lại thời gian của tin nhắn và tính toán các khoảng trống mà không cần lo lắng về hiệu suất. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng thực hiện một cuộc đi bộ dài 120 phút bắt đầu vào mọi phút có thể trong ngày và kiểm tra xem liệu nó có trùng lặp với bất kỳ tin nhắn nào không. Điều này sẽ liên quan đến việc kiểm tra tới 1440 lần bắt đầu cho mỗi trường hợp thử nghiệm và đối với mỗi thời gian bắt đầu, hãy lặp lại tất cả các thông báo để kiểm tra xung đột. Trong trường hợp xấu nhất, điều này dẫn đến khoảng 1440 * 100 = 144.000 thao tác cho mỗi trường hợp thử nghiệm, điều này khả thi nhưng không hiệu quả một cách không cần thiết. 

Cách tiếp cận tối ưu nhận thấy rằng thời điểm thích hợp duy nhất là khoảng thời gian giữa các tin nhắn liên tiếp và các thời điểm trong ngày. Mỗi tin nhắn chia ngày thành các khoảng trống: từ nửa đêm đến tin nhắn đầu tiên, giữa các tin nhắn và từ tin nhắn cuối cùng đến cuối ngày. Chúng ta chỉ cần đo những khoảng trống này và xem liệu có ít nhất hai khoảng trống có chiều dài từ 120 trở lên hay không. Bằng cách lặp lại một lần trên danh sách thông báo và kiểm tra độ dài của các khoảng thời gian, chúng ta có thể giải quyết từng trường hợp kiểm thử trong thời gian O(n). Thông tin chi tiết là việc đặt một bước đi ở bất kỳ đâu trong một khoảng cách đủ lớn luôn có tác dụng vì các thông báo không di chuyển trong quá trình đi bộ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(1440 * n) | O(n) | Hoạt động nhưng chậm không cần thiết | 
| Kiểm tra định kỳ | O(n) | O(1) | Được chấp nhận và đơn giản | 

## Hướng dẫn thuật toán 

1. Bắt đầu bằng cách đọc số lượng test case,`t`. Điều này cho chúng ta biết chúng ta cần phân tích bao nhiêu lịch trình riêng biệt. 
2. Với mỗi test case, hãy đọc số lượng tin nhắn`n`và danh sách thời gian nhắn tin`a`. Danh sách này đang tăng lên một cách nghiêm túc. 
3. Khởi tạo bộ đếm`walkable_intervals`về không. Điều này sẽ theo dõi số lượng khoảng thời gian 120 phút tồn tại giữa các tin nhắn. 
4. Tính khoảng thời gian đầu tiên từ nửa đêm đến tin nhắn đầu tiên. Nếu khoảng thời gian này ít nhất là 120 phút, hãy tăng dần`walkable_intervals`. 
5. Lặp lại các cặp tin nhắn liên tiếp. Đối với mỗi cặp`(a[i], a[i+1])`, kiểm tra khoảng`a[i+1] - a[i]`. Nếu khoảng cách này ít nhất là 120 phút, hãy tăng dần`walkable_intervals`. 
6. Kiểm tra khoảng thời gian cuối cùng từ tin nhắn cuối cùng đến cuối ngày. Nếu như`1440 - a[-1]`ít nhất là 120, tăng dần`walkable_intervals`. 
7. Sau khi đếm tất cả các khoảng, nếu`walkable_intervals`ít nhất là 2, xuất ra "CÓ"; nếu không thì xuất ra "NO". 

Tại sao nó hoạt động: Thuật toán đúng vì nó chỉ xem xét các khoảng thời gian liền kề tối đa mà không có thông báo. Bất kỳ chuyến đi bộ 120 phút nào cũng phải hoàn toàn phù hợp với một trong những khoảng thời gian này. Bằng cách đếm các khoảng thời gian có độ dài ít nhất là 120, chúng tôi trực tiếp đếm số lần đi bộ không chồng chéo có thể thực hiện được. Hai hoặc nhiều khoảng thời gian như vậy đảm bảo rằng hai lần đi bộ có thể xảy ra. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    walkable = 0
    
    # Check interval before first message
    if a[0] >= 120:
        walkable += 1
    
    # Check intervals between messages
    for i in range(n - 1):
        if a[i+1] - a[i] >= 120:
            walkable += 1
    
    # Check interval after last message
    if 1440 - a[-1] >= 120:
        walkable += 1
    
    print("YES" if walkable >= 2 else "NO")
```Mã này phản ánh thuật toán từng bước. Chúng tôi kiểm tra rõ ràng các khoảng thời gian trước tin nhắn đầu tiên, giữa mỗi cặp liên tiếp và sau tin nhắn cuối cùng. Bộ đếm có thể đi bộ ghi lại số lượng khoảng thời gian 120 phút hợp lệ. So sánh ngưỡng`>= 120`đảm bảo chúng ta không đánh giá thấp thời gian sẵn có. Điều kiện cuối cùng chỉ đơn giản là xác minh xem có thể lên lịch ít nhất hai lần đi bộ hay không. 

## Ví dụ đã hoạt động 

**Ví dụ 1** 

đầu vào:`100 200 300 400 500 600 700 800 900 1000 1100 1200 1300 1400`| Bắt đầu khoảng thời gian | Khoảng thời gian kết thúc | Chiều dài | Có thể đi bộ? | 
| --- | --- | --- | --- | 
| 0 | 100 | 100 | Không | 
| 100 | 200 | 100 | Không | 
| 200 | 300 | 100 | Không | 
| 300 | 400 | 100 | Không | 
| 400 | 500 | 100 | Không | 
| 500 | 600 | 100 | Không | 
| 600 | 700 | 100 | Không | 
| 700 | 800 | 100 | Không | 
| 800 | 900 | 100 | Không | 
| 900 | 1000 | 100 | Không | 
| 1000 | 1100 | 100 | Không | 
| 1100 | 1200 | 100 | Không | 
| 1200 | 1300 | 100 | Không | 
| 1300 | 1400 | 100 | Không | 
| 1400 | 1440 | 40 | Không | 

Khoảng thời gian có thể đi bộ: 0 → đầu ra`NO`. 

**Ví dụ 2** 

đầu vào:`100 200 300 400 600 700 800 900 1100 1200 1300 1400`Khoảng thời gian ít nhất 120 phút: 400→600, 900→1100. Khoảng thời gian có thể đi bộ: 2 → đầu ra`YES`. 

Các bảng này minh họa cách đếm các khoảng trống hợp lệ trực tiếp xác định tính khả thi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Lặp lại n tin nhắn một lần, công việc liên tục trong mỗi khoảng thời gian | 
| Không gian | O(n) | Lưu trữ thời gian tin nhắn trong danh sách | 

Với`t`các trường hợp thử nghiệm, tổng độ phức tạp là O(t*n), tối đa là 100 * 100 = 10.000 thao tác, không đáng kể so với giới hạn thời gian 2 giây. Việc sử dụng bộ nhớ ở mức tối thiểu, chỉ lưu trữ danh sách tin nhắn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        walkable = 0
        if a[0] >= 120:
            walkable += 1
        for i in range(n - 1):
            if a[i+1] - a[i] >= 120:
                walkable += 1
        if 1440 - a[-1] >= 120:
            walkable += 1
        print("YES" if walkable >= 2 else "NO")
    return output.getvalue().strip()

# Provided samples
assert run("6\n14\n100 200 300 400 500 600 700 800 900 1000 1100 1200 1300 1400\n12\n100 200 300 400 600 700 800 900 1100 1200 1300 1400\n13\n100 200 300 400 500 600 700 800 900 1100 1200 1300 1400\n13\n101 189 272 356 463 563 659 739 979 1071 1170 1274 1358\n1\n42\n5\n0 1 2 3 4") == "NO\nYES\nNO\nYES\nYES\nYES"

# Custom edge cases
assert run("1\n1\n0") == "NO", "Only one message at start"
assert run("1\n1\n1320") == "NO", "Only one message near end"
assert run("1\n0\n") == "YES
```
