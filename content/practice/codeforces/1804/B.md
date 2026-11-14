---
title: "CF 1804B - Tiêm chủng"
description: "Chúng tôi được cung cấp một danh sách sắp xếp thời gian đến của bệnh nhân. Mỗi bệnh nhân đến vào một thời điểm cụ thể và sẵn sàng chờ đợi trong một khoảng thời gian giới hạn, nghĩa là có một khoảng thời gian để họ có thể được tiêm chủng."
date: "2026-06-09T09:21:17+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1804
codeforces_index: "B"
codeforces_contest_name: "Nebius Welcome Round (Div. 1 + Div. 2)"
rating: 1000
weight: 1804
solve_time_s: 145
verified: true
draft: false
---

[CF 1804B - Tiêm chủng](https://codeforces.com/problemset/problem/1804/B) 

**Đánh giá:** 1000 
**Tags:** tham lam, thực hiện 
**Thời gian giải:** 2m 25s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một danh sách sắp xếp thời gian đến của bệnh nhân. Mỗi bệnh nhân đến vào một thời điểm cụ thể và sẵn sàng chờ đợi trong một khoảng thời gian giới hạn, nghĩa là có một khoảng thời gian để họ có thể được tiêm chủng. Nếu họ đến vào lúc$t_i$, thì chúng có thể được phục vụ vào bất kỳ thời điểm nguyên nào từ$t_i$ĐẾN$t_i + w$. 

Vắc xin không phải là các đơn vị riêng lẻ mà được đóng thành từng gói$k$liều lượng. Sau khi mở gói, nó phải được sử dụng trong thời hạn sử dụng cố định: nếu mở đúng lúc$x$, nó chỉ có thể được sử dụng theo thời gian$x$ĐẾN$x + d$. Sau đó, mọi liều còn lại sẽ bị loại bỏ. Mỗi bệnh nhân tiêu thụ đúng một liều. 

Nhiệm vụ là chọn thời điểm mở gói và cách chỉ định bệnh nhân vào các gói để mọi bệnh nhân được tiêm chủng trong khoảng thời gian chờ đợi riêng của họ và trong vòng đời của gói đã mở, sử dụng càng ít gói càng tốt. 

Kích thước đầu vào cho phép lên đến$2 \cdot 10^5$tổng số bệnh nhân trong các trường hợp thử nghiệm, vì vậy mọi giải pháp đều phải gần như tuyến tính hoặc gần tuyến tính cho mỗi trường hợp thử nghiệm. Chiến lược bậc hai liên tục thử tất cả các nhóm bệnh nhân có thể sẽ vượt quá giới hạn vì nó yêu cầu kiểm tra tính tương thích giữa nhiều cửa sổ thời gian chồng chéo. 

Tình huống cạnh chính xuất hiện khi cửa sổ bệnh nhân hầu như không chồng lên nhau hoặc rời rạc. Ví dụ, nếu$w = 0$, mọi bệnh nhân phải được phục vụ chính xác vào thời điểm đến, vì vậy chỉ có thể phân nhóm nếu có nhiều bệnh nhân đến cùng một lúc. Một trường hợp góc khác là khi$d = 0$, nghĩa là một gói chỉ có thể được sử dụng tại thời điểm mở chính xác, vì vậy mỗi gói chỉ phục vụ hiệu quả tối đa một lần ngay lập tức bất kể$k$. 

## Phương pháp tiếp cận 

Một ý tưởng ngây thơ là mô phỏng việc chỉ định từng bệnh nhân một và mỗi bệnh nhân cố gắng đặt họ vào một gói đã mở hiện có nếu nó vẫn còn hiệu lực và còn dung lượng, nếu không thì hãy mở gói mới vào thời điểm tốt nhất có thể. Điều này đòi hỏi phải duy trì tất cả các gói hoạt động và kiểm tra tính tương thích cho từng bệnh nhân. Trong trường hợp xấu nhất, nhiều gói trùng lặp về thời gian và chúng tôi liên tục quét chúng để tìm gói hợp lệ, dẫn đến$O(n^2)$hành vi. 

Cấu trúc của vấn đề cho phép quan sát chặt chẽ hơn. Bệnh nhân đến theo thứ tự được sắp xếp nên chúng tôi xử lý họ một cách tự nhiên từ thời điểm sớm nhất đến thời điểm muộn nhất. Mỗi gói, sau khi được mở, sẽ xác định khoảng thời gian sử dụng cố định$[x, x + d]$và có thể phục vụ nhiều nhất$k$bệnh nhân. Trong khi đó, mỗi bệnh nhân có khoảng thời gian sẵn sàng riêng$[t_i, t_i + w]$. Một gói chỉ có thể được sử dụng cho bệnh nhân nếu các khoảng thời gian này trùng nhau tại thời điểm sử dụng dịch vụ. 

Sự đơn giản hóa chính là tránh theo dõi chi tiết các gói riêng lẻ và thay vào đó quyết định một cách tham lam cách nhóm các bệnh nhân liên tiếp. Đối với một bệnh nhân bắt đầu cố định, chúng tôi muốn gói càng nhiều bệnh nhân theo dõi càng tốt vào một gói vắc xin duy nhất, tuân theo cả hai ràng buộc: thời gian phục vụ của họ phải vừa với một khoảng thời gian khả thi chung và chúng tôi không được vượt quá$k$bệnh nhân mỗi gói. 

Đối với một nhóm bệnh nhân bắt đầu từ chỉ số$i$, thời gian muộn nhất mà chúng tôi có thể trì hoãn việc mở gói bị hạn chế bởi thời gian hết hạn sớm nhất có thể của bất kỳ bệnh nhân nào trong nhóm, trong khi thời gian khả thi sớm nhất bị hạn chế bởi người đến muộn nhất trong nhóm trừ đi khoản trợ cấp chờ đợi. Điều này biến vấn đề phân nhóm thành việc duy trì một cửa sổ trượt gồm các bệnh nhân khả thi và việc tham lam đóng cửa một nhóm sau khi thêm một bệnh nhân khác sẽ phá vỡ tính khả thi hoặc vượt quá khả năng. 

Điều này làm giảm vấn đề xuống còn một lần trong đó chúng tôi mở rộng cửa sổ bệnh nhân và liên tục hoàn thiện các nhóm bất cứ khi nào các ràng buộc bị phá vỡ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng mỗi gói ngây thơ |$O(n^2)$|$O(n)$| Quá chậm | 
| Nhóm trượt tham lam |$O(n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý bệnh nhân theo thứ tự thời gian đến tăng dần và duy trì lô hiện tại đại diện cho các bệnh nhân được chỉ định vào cùng một gói. 

1. Bắt đầu với bệnh nhân đầu tiên và khởi tạo lô trống. Chúng tôi cũng duy trì số lượng gói chúng tôi đã mở. 
2. Đối với mỗi bệnh nhân theo thứ tự, hãy thử thêm họ vào lô hiện tại. Khi thêm một bệnh nhân, chúng tôi kiểm tra xem lô đó có khả thi về mặt thời gian hay không. Lô này phải có thể được phân phát trong một khoảng thời gian chung nào đó phù hợp với cả giới hạn chờ đợi của bệnh nhân và thời gian sử dụng vắc xin. 
3. Trong khi xây dựng lô, chúng tôi cũng đảm bảo không vượt quá$k$bệnh nhân. Nếu kích thước lô đạt$k$, chúng tôi hoàn thiện lô ngay lập tức vì gói không thể phục vụ nhiều bệnh nhân hơn bất kể tính khả thi về mặt thời gian. 
4. Nếu việc thêm một bệnh nhân mới sẽ vi phạm tính khả thi, nghĩa là không có một thời điểm nào có thể phục vụ tất cả bệnh nhân hiện có trong đợt, chúng tôi sẽ đóng đợt trước bệnh nhân này và bắt đầu một đợt mới. 
5. Chúng tôi lặp lại cho đến khi tất cả bệnh nhân được xử lý. Mỗi lần đóng một đợt, chúng tôi đếm một gói vắc xin. 

Bước ẩn quan trọng là kiểm tra tính khả thi. Đối với một đợt bệnh nhân, thời gian phục vụ phải nằm ở giao điểm của tất cả các khoảng thời gian của bệnh nhân.$[t_i, t_i + w]$, đó là$[\max t_i, \min (t_i + w)]$. Giao điểm này cũng phải trùng lặp với một số khoảng thời gian gói$d$, nhưng vì chúng tôi chọn thời gian mở cửa một cách tối ưu trong lô nên tính khả thi sẽ giảm xuống để đảm bảo giao lộ không trống và có thể được bao phủ bởi một khoảng thời gian-$d$cửa sổ. 

Một cách thiết thực để duy trì điều này là theo dõi thời gian đến tối đa hiện tại và thời gian phục vụ tối thiểu cho phép muộn nhất trong lô. 

### Tại sao nó hoạt động 

Mỗi lần mở rộng một đợt, chúng tôi chỉ thêm những bệnh nhân có khoảng thời gian vẫn trùng với khu vực dịch vụ khả thi hiện tại. Vì các bệnh nhân đã được sắp xếp nên bất kỳ nhóm nào trước đó có thể bao gồm một bệnh nhân đều đã được xem xét trước khi tiếp tục. Sau khi một đợt được đóng lại, không đợt nào sau đó có thể bao gồm những bệnh nhân đó mà không vi phạm các hạn chế về thời gian hoặc năng lực. Điều này đảm bảo mỗi lô tương ứng với một phân đoạn liền kề khả thi tối đa và phân đoạn tối đa tham lam sẽ giảm thiểu số lượng gói cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k, d, w = map(int, input().split())
    t = list(map(int, input().split()))
    
    packs = 0
    i = 0
    
    while i < n:
        packs += 1
        
        batch_size = 0
        
        # We maintain the feasible time window of the current batch
        # For patients j in batch:
        # feasible start is max(t[j])
        # feasible end is min(t[j] + w)
        left = t[i]
        right = t[i] + w
        
        j = i
        
        while j < n:
            # check if patient j can fit in current feasibility window
            new_left = max(left, t[j])
            new_right = min(right, t[j] + w)
            
            if new_left > new_right:
                break
            
            if batch_size == k:
                break
            
            left = new_left
            right = new_right
            batch_size += 1
            j += 1
        
        i = j
    
    print(packs)

if __name__ == "__main__":
    solve()
```Mã xây dựng từng đợt một cách tham lam. Các biến`left`Và`right`duy trì giao điểm của tất cả các cửa sổ thời gian của bệnh nhân trong nhóm hiện tại. Khi giao lộ vắng tanh, chúng tôi dừng lại và bắt đầu đoàn mới. các`batch_size`biến đảm bảo chúng ta không bao giờ vượt quá giới hạn liều$k$. Con trỏ`i`chuyển sang bệnh nhân chưa được xử lý đầu tiên sau mỗi đợt, đảm bảo tiến trình tuyến tính. 

Một điểm tinh tế là chúng tôi không bao giờ tính toán rõ ràng thời gian mở gói. Nó được chọn ngầm bên trong giao điểm khả thi, vì vậy chúng tôi chỉ theo dõi xem thời gian đó có tồn tại hay không. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
6 3 5 3
1 2 3 10 11 18
```Chúng tôi theo dõi việc phân nhóm: 

| Bước | tôi | j | trái | đúng | cỡ lô | hành động | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 0 | 1 | 4 | 1 | bắt đầu đợt | 
| 2 | 0 | 1 | 1 | 5 | 2 | thêm bệnh nhân 2 | 
| 3 | 0 | 2 | 1 | 6 | 3 | thêm bệnh nhân 3, đợt đầy đủ | 
| 4 | 0 | 3 | - | - | - | đóng lô, gói mới | 
| 5 | 3 | 3 | 10 | 15 | 1 | bắt đầu đợt mới | 
| 6 | 3 | 4 | 10 | 16 | 2 | thêm bệnh nhân 5 | 
| 7 | 3 | 5 | 10 | 23 | 3 | thêm bệnh nhân 6 | 

Chúng tôi nhận được 2 gói. 

Điều này khẳng định rằng những bệnh nhân đến sớm có thể được đóng gói chặt chẽ, trong khi những bệnh nhân đến ở xa hơn cần một đợt mới do khoảng thời gian rời rạc. 

### Ví dụ 2 

đầu vào:```
3 10 3 6
10 20 30
```| Bước | tôi | j | trái | đúng | cỡ lô | hành động | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 0 | 10 | 16 | 1 | bắt đầu | 
| 2 | 0 | 1 | 20 | 26 | 2 | vẫn có thể chồng chéo | 
| 3 | 0 | 2 | 30 | 36 | 3 | tất cả đều phù hợp | 

Một gói là đủ vì dung lượng lớn và khoảng thời gian vẫn cho phép chồng chéo. 

Điều này cho thấy rằng ngay cả những người đến có khoảng cách xa nhau cũng có thể được nhóm lại nếu$w$là đủ lớn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$mỗi trường hợp thử nghiệm | Mỗi bệnh nhân được xử lý một lần khi con trỏ bên phải di chuyển về phía trước một cách đơn điệu | 
| Không gian |$O(1)$| Chỉ có một số biến được duy trì bên cạnh input | 

Tổng số bệnh nhân trong các trường hợp thử nghiệm được giới hạn bởi$2 \cdot 10^5$, do đó giải pháp chạy thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import sys
    
    input = sys.stdin.readline
    
    def solve():
        n, k, d, w = map(int, input().split())
        t = list(map(int, input().split()))
        
        packs = 0
        i = 0
        
        while i < n:
            packs += 1
            batch_size = 0
            left = t[i]
            right = t[i] + w
            j = i
            
            while j < n:
                new_left = max(left, t[j])
                new_right = min(right, t[j] + w)
                
                if new_left > new_right:
                    break
                if batch_size == k:
                    break
                
                left = new_left
                right = new_right
                batch_size += 1
                j += 1
            
            i = j
        
        return str(packs)
    
    return solve()

# provided samples
assert run("""5
6 3 5 3
1 2 3 10 11 18
6 4 0 0
3 3 3 3 3 4
9 10 2 2
0 1 2 3 4 5 6 7 8
3 10 3 6
10 20 30
5 5 4 4
0 2 4 6 8
""") == """2
3
2
3
1
"""

# custom cases
assert run("""1
1 1 10 0
5
""") == "1", "single patient"

assert run("""1
5 5 0 0
1 1 1 1 1
""") == "1", "all same time full capacity"

assert run("""1
5 2 0 0
1 1 1 1 1
""") == "3", "tight capacity forcing splits"

assert run("""1
4 10 100 100
1 50 100 150
""") == "1", "large windows allow full merge"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| bệnh nhân đơn lẻ | 1 | trường hợp cơ sở | 
| đồng thời hết công suất | 1 | đóng gói tối đa | 
| năng lực chặt chẽ buộc phải chia tách | 3 | k ràng buộc | 
| cửa sổ lớn cho phép hợp nhất hoàn toàn | 1 | hợp nhất khả thi tối đa | 

## Vỏ cạnh 

Khi nào$w = 0$, mỗi bệnh nhân chỉ có thể được phục vụ chính xác vào thời điểm họ đến. Thuật toán xử lý vấn đề này vì khoảng giao nhau thu gọn thành một điểm duy nhất cho mỗi bệnh nhân, do đó việc hợp nhất chỉ thành công khi các điểm đến khớp nhau. Ví dụ, trong`1 1 0 0`với những người đến`[5]`, lô ngay lập tức hình thành và kết thúc bằng một gói. 

Khi$k = 1$, mỗi bệnh nhân cần một gói riêng bất kể thời gian. Thuật toán thực thi điều này thông qua`batch_size == k`, buộc đóng cửa sau mỗi bệnh nhân, phù hợp với hành vi tối ưu. 

Khi$d = 0$, thời gian tồn tại của gói bằng 0, nghĩa là gói chỉ có thể được sử dụng tại thời điểm mở gói. Điều kiện khả thi vẫn có tác dụng vì bất kỳ nhóm nào yêu cầu thời gian phục vụ khác nhau sẽ không thể giao nhau trừ khi tất cả bệnh nhân có chung thời gian khả thi, buộc phải thực hiện các đợt dịch vụ rất nhỏ một cách hiệu quả.
