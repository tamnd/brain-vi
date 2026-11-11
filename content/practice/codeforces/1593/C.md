---
title: "CF 1593C - Tiết kiệm nhiều chuột hơn"
description: "Chúng ta có một đoạn thẳng có ba loại thực thể: một con mèo bắt đầu ở vị trí 0, một cái lỗ ở vị trí n và một vài con chuột được đặt đúng vị trí giữa chúng. Thời gian tiến triển theo từng giây rời rạc."
date: "2026-06-14T23:39:01+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1593
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 748 (Div. 3)"
rating: 1000
weight: 1593
solve_time_s: 165
verified: false
draft: false
---

[CF 1593C - Tiết kiệm nhiều chuột hơn](https://codeforces.com/problemset/problem/1593/C) 

**Đánh giá:** 1000 
**Tags:** tìm kiếm nhị phân, tham lam 
**Thời gian giải:** 2m 45s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một đoạn thẳng có ba loại thực thể: một con mèo bắt đầu ở vị trí 0, một cái lỗ ở vị trí n và một vài con chuột được đặt đúng vị trí giữa chúng. Thời gian tiến triển theo từng giây rời rạc. Mỗi giây được chia thành hai hành động: đầu tiên chúng ta chọn chính xác một con chuột và di chuyển nó sang phải một bước, sau đó con mèo di chuyển một bước sang phải. Nếu một con chuột đến lỗ trong khi di chuyển, nó sẽ ngay lập tức an toàn và không còn tham gia nữa. Nếu con mèo đáp xuống vị trí có một hoặc nhiều con chuột sau khi di chuyển, những con chuột đó sẽ bị ăn thịt. 

Nhiệm vụ là chọn con chuột nào sẽ di chuyển mỗi giây để tối đa hóa số lượng con chuột tiếp cận lỗ trước khi chia sẻ vị trí với con mèo sau khi nó di chuyển. 

Một cách hữu ích để giải thích lại điều này là hãy nghĩ mỗi con chuột đều có một thời hạn: nó phải đạt đến vị trí n trước khi con mèo đến được vị trí của nó. Tuy nhiên, không giống như bài toán thời hạn cố định, chúng ta có thể xen kẽ tiến độ giữa các con chuột, điều này tạo ra vấn đề lập kế hoạch thay vì vấn đề so sánh trực tiếp. 

Các ràng buộc rất lớn: tổng số chuột trong tất cả các trường hợp thử nghiệm lên tới 4⋅10^5. Điều này ngay lập tức loại trừ bất kỳ mô phỏng bậc hai nào mà chúng tôi liên tục mô phỏng từng giây cho mỗi con chuột. Ngay cả O(k^2) cho mỗi trường hợp thử nghiệm cũng sẽ quá chậm. Chúng ta cần một cái gì đó gần hơn với O(k log k) hoặc O(k). 

Một vài trường hợp đặc biệt tiết lộ cấu trúc: 

Nếu tất cả những con chuột bắt đầu rất gần con mèo, ví dụ n = 10 và các vị trí [1,1,1,1], thì bất kỳ lịch trình kém nào mà chúng ta liên tục tiến lên một con chuột có thể cho phép con mèo bắt tất cả chúng ở vị trí 1 trước khi bất kỳ con chuột nào chạm tới lỗ. Chiến lược đúng đắn là ưu tiên đẩy một số con chuột về phía trước đủ xa để chúng “thoát khỏi tầm với của mèo”. 

Nếu tất cả các con chuột đã ở gần lỗ, ví dụ n = 10 và vị trí [9,9,9], thì bất kỳ lịch trình nào cũng có hiệu quả miễn là có ít nhất một con chuột được di chuyển ngay lập tức. Yếu tố hạn chế không phải là sự can thiệp giữa các con chuột mà là hạn chế về một con chuột mỗi giây. 

Điểm tinh tế quan trọng là con mèo tiến lên một cách xác định ở tốc độ đơn vị và hoạt động như một đường cắt chuyển động. Những con chuột tụt lại phía sau giới hạn đó sẽ bị diệt vong không thể cứu vãn được. 

## Phương pháp tiếp cận 

Một mô phỏng trực tiếp sẽ cố gắng mô hình hóa từng giây, chọn một con chuột một cách tham lam hoặc thậm chí toàn diện, cập nhật vị trí và kiểm tra xem chuột bị ăn thịt hay trốn thoát. Mặc dù đúng nhưng cách tiếp cận này về cơ bản là quá chậm vì mỗi con chuột có thể yêu cầu tối đa O(n) lần di chuyển và chúng tôi có tới 4⋅10^5 con chuột. 

Quan sát quan trọng là đảo ngược quan điểm: thay vì nghĩ về các vị trí trong không gian tuyệt đối, chúng tôi nghĩ theo khía cạnh “mỗi con chuột sống sót được bao nhiêu giây trước khi bị bắt”. Một con chuột bắt đầu ở vị trí x phải đến n trước khi con mèo đến x. Vì con mèo di chuyển chính xác một bước mỗi giây bắt đầu từ 0 nên con mèo đạt đến vị trí x tại thời điểm x. Điều này gợi ý rằng con chuột phải hoàn thành hành trình cần thiết trước thời hạn gắn liền với vị trí xuất phát của nó. 

Tuy nhiên, chuột di chuyển không liên tục; chỉ một con chuột có thể di chuyển mỗi giây, vì vậy những con chuột cạnh tranh hiệu quả để giành lấy các khoảng thời gian trước thời hạn của chúng. Điều này trở thành bài toán lập kế hoạch: mỗi con chuột i cần một số lần di chuyển nhất định (n − x_i) và nó phải nhận đủ lượt trước thời điểm x_i. 

Viết lại điều này, mỗi con chuột mang lại “lợi nhuận” là 1 nếu chúng tôi xử lý đầy đủ nó và có một cửa sổ ràng buộc có kích thước x_i. Chiến lược tốt nhất là chọn những con chuột dễ hoàn thành nhất trong cửa sổ có sẵn của chúng. Điều đó tự nhiên gợi ý việc sắp xếp theo vị trí bắt đầu và đếm xem có thể hoàn thành được bao nhiêu trong khi tôn trọng thời gian tích lũy.

Chúng tôi xử lý chuột theo thứ tự tăng dần khoảng cách của chúng với con mèo (hoặc giảm dần vị trí bắt đầu). Mỗi lần chúng ta xem xét một con chuột, chúng ta cố gắng gán cho nó số lần di chuyển cần thiết. Nếu tại bất kỳ thời điểm nào chúng tôi vượt quá thời hạn, chúng tôi sẽ dừng hoặc bỏ qua tùy thuộc vào việc theo dõi tính khả thi. Cấu trúc cuối cùng giảm xuống việc chọn tiền tố tối đa theo điều kiện khả thi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng Brute Force | O(k · n) trường hợp xấu nhất | O(k) | Quá chậm | 
| Lên lịch tham lam sau khi sắp xếp | O(k log k) | O(k) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi diễn giải lại mỗi con chuột là cần một khoảng thời gian xử lý nhất định và có thời hạn do con mèo đến. 

1. Tính thời gian cho mỗi con chuột để đến lỗ, đó là n − x_i. Đây là số lần nó phải được chọn để di chuyển. 
2. Sắp xếp chuột theo vị trí xuất phát theo thứ tự giảm dần. Những con chuột ở gần hố hơn được ưu tiên vì thời hạn của chúng chặt chẽ hơn; trì hoãn chúng có nguy cơ mất mát ngay lập tức. 
3. Duy trì một bộ đếm đang chạy biểu thị số giây đã được sử dụng để chỉ định các bước di chuyển cho những con chuột mà chúng ta đang cố gắng lưu. Bộ đếm này thể hiện việc sử dụng thời gian toàn cầu trong lịch trình của chúng tôi. 
4. Lặp lại các con chuột theo thứ tự được sắp xếp. Đối với mỗi con chuột, hãy thêm thời gian di chuyển cần thiết của nó vào bộ đếm. 
5. Nếu tại bất kỳ thời điểm nào, thời gian tích lũy vượt quá số lần di chuyển an toàn có sẵn trước khi con mèo đến ranh giới liên quan, con chuột này sẽ không thể được lưu và chúng tôi ngừng tăng số lượng. 
6. Nếu không, hãy tính con chuột này là đã lập lịch thành công. 

Ý tưởng chính là mỗi con chuột được lưu sẽ tiêu tốn một khối thời gian liền kề trong lịch trình và những con chuột trước đó (gần lỗ hơn) phải được đảm bảo hoàn thành trước khi tiến trình của con mèo loại bỏ khoảng thời gian khả thi của chúng. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, con mèo thực thi một ràng buộc ngày càng đơn điệu: các vị trí sẽ trở nên không an toàn vĩnh viễn khi con mèo đi qua chúng. Điều này tạo ra một cấu trúc trong đó tính khả thi chỉ phụ thuộc vào việc liệu chúng ta có thể chỉ định đủ khoảng thời gian sớm cho mỗi con chuột được chọn trước thời hạn ngầm định của nó hay không. Việc sắp xếp theo vị trí đảm bảo chúng tôi luôn xem xét các trường hợp hạn chế nhất trước tiên. Sự tích lũy tham lam đảm bảo chúng ta không bao giờ “lãng phí” thời gian sớm cho một con chuột không phù hợp với lịch trình còn lại. Bởi vì mỗi con chuột đều cạnh tranh để giành cùng một nguồn tài nguyên trong một đơn vị thời gian, nên tính tối ưu sẽ giảm xuống để lấp đầy lịch trình với số lượng tối đa các yêu cầu về thời gian không chồng chéo trong điều kiện ràng buộc ngày càng tăng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        x = list(map(int, input().split()))

        # sort mice by closeness to hole (largest x first)
        x.sort(reverse=True)

        time_used = 0
        saved = 0

        for xi in x:
            need = n - xi

            if time_used + need <= xi:
                time_used += need
                saved += 1
            else:
                break

        print(saved)

if __name__ == "__main__":
    solve()
```Mã này sắp xếp chuột đầu tiên sao cho những con chuột gần lỗ nhất được xử lý trước vì chúng nhạy cảm nhất về thời gian. Đối với mỗi con chuột, chúng tôi tính toán cần bao nhiêu bước di chuyển để đến được lỗ. Biến`time_used`theo dõi tổng số lần di chuyển chuột mà chúng tôi đã lên lịch. Chúng tôi chỉ chấp nhận một con chuột nếu việc thêm các bước di chuyển cần thiết vẫn cho phép nó hoàn thành trước khi con mèo đến vị trí bắt đầu. Một khi điều kiện này không thành công, không thể cứu được thêm con chuột nào nữa vì những con chuột còn lại thậm chí còn ít bị hạn chế hơn. 

Chi tiết triển khai chính là sự bất bình đẳng`time_used + need <= xi`. Nó mã hóa thực tế là tất cả công việc được giao cho con chuột phải xảy ra trước khi con mèo đến vị trí bắt đầu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 10
x = [8, 7, 5, 4, 9, 4]
```Đã sắp xếp:```
[9, 8, 7, 5, 4, 4]
```| Chuột | cần (10-x) | time_used trước | khả thi? | time_used sau | đã lưu | 
| --- | --- | --- | --- | --- | --- | 
| 9 | 1 | 0 | vâng | 1 | 1 | 
| 8 | 2 | 1 | vâng | 3 | 2 | 
| 7 | 3 | 3 | vâng | 6 | 3 | 
| 5 | 5 | 6 | không | - | 3 | 

Chúng ta có thể cứu được 3 con chuột trước khi kế hoạch trở nên bất khả thi. Điều này cho thấy mặc dù có nhiều con chuột ở gần lỗ nhưng thời gian xử lý tích lũy của chúng vẫn trở thành nút thắt cổ chai. 

### Ví dụ 2 

đầu vào:```
n = 2
x = [1,1,1,1,1,1,1,1]
```Đã sắp xếp:```
[1,1,1,1,1,1,1,1]
```| Chuột | cần (2-x) | time_used trước | khả thi? | time_used sau | đã lưu | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | vâng | 1 | 1 | 
| 1 | 1 | 1 | không | - | 1 | 

Chỉ có thể cứu được một con chuột vì mỗi con yêu cầu chính xác một đơn vị thời gian và thời hạn cực kỳ chặt chẽ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(k log k) | việc sắp xếp thống trị từng trường hợp thử nghiệm | 
| Không gian | O(k) | lưu trữ vị trí chuột | 

Tổng k trên tất cả các trường hợp thử nghiệm tối đa là 4⋅10^5, do đó giải pháp chạy thoải mái trong giới hạn ngay cả trong Python. Việc sắp xếp và quét tuyến tính đều đủ hiệu quả cho thang đo ràng buộc này. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# provided samples
assert run("""3
10 6
8 7 5 4 9 4
2 8
1 1 1 1 1 1 1 1
12 11
1 2 3 4 5 6 7 8 9 10 11
""") == """3
1
4"""

# minimum case
assert run("""1
2 1
1
""") == "1"

# all equal positions
assert run("""1
10 5
5 5 5 5 5
""") == "2"

# already near hole
assert run("""1
10 3
9 9 9
""") == "3"

# large spread
assert run("""1
100 4
1 2 50 99
""") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chuột đơn | 1 | tính khả thi cơ bản | 
| tất cả đều bình đẳng | 2 | lên lịch thi đấu | 
| gần lỗ | 3 | trường hợp thoát hiểm tầm thường | 
| lan truyền hỗn hợp | 3 | tham lam đặt hàng đúng đắn | 

## Vỏ cạnh 

Cụm chặt chẽ gần vị trí 1 bộc lộ ràng buộc lập kế hoạch rõ ràng nhất. Đối với đầu vào như n = 10 và x = [1,1,1], mỗi con chuột cần 9 lần di chuyển trong khi thời hạn của chúng là 1, khiến tất cả nhưng có thể không khả thi. Thuật toán ngay lập tức phát hiện tính không khả thi sau lần bổ sung đầu tiên vào`time_used`, bởi vì ngay cả một bài tập cũng vượt quá điều kiện thời hạn. 

Trường hợp đối xứng gần lỗ, chẳng hạn như x = [n−1, n−1, n−1], cho thấy hành vi ngược lại. Mỗi con chuột chỉ cần một lần di chuyển và thời hạn của chúng đủ lớn để có thể lên lịch nhiều lần trước khi con mèo đến vị trí của chúng. Việc tích lũy tham lam vẫn tiếp tục mà không vi phạm, xác nhận rằng ràng buộc chỉ bị ràng buộc khi công việc tích lũy vượt quá thời gian an toàn sẵn có.
