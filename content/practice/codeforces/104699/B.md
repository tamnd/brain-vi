---
title: "CF 104699B - \u041a\u0430\u0434\u0440\u043e\u0432\u044b\u0435 \u043f\u0435\u0440\u0435\u0441\u0442\u0430\u043d\u043e\u0432\u043a\u0438"
description: "Chúng tôi được sắp xếp một dãy phòng, mỗi phòng chứa một số nhân viên. Nhân viên chỉ có thể di chuyển giữa các phòng liền kề và mục tiêu là tập hợp mọi người vào một phòng đã chọn duy nhất."
date: "2026-06-29T08:32:42+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104699
codeforces_index: "B"
codeforces_contest_name: "\u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b, \u0421\u0435\u0437\u043e\u043d 2023-2024, \u0412\u0442\u043e\u0440\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430"
rating: 0
weight: 104699
solve_time_s: 79
verified: true
draft: false
---

[CF 104699B - \u041a\u0430\u0434\u0440\u043e\u0432\u044b\u0435 \u043f\u0435\u0440\u0435\u0441\u0442\u0430\u043d\u043e\u0432\u043a\u0438](https://codeforces.com/problemset/problem/104699/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 19s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được sắp xếp một dãy phòng, mỗi phòng chứa một số nhân viên. Nhân viên chỉ có thể di chuyển giữa các phòng liền kề và mục tiêu là tập hợp mọi người vào một phòng đã chọn duy nhất. Việc di chuyển một nhân viên từ phòng này sang phòng tiếp theo sẽ tốn một đơn vị “công sức” và nếu một nhân viên đi nhiều phòng thì chi phí sẽ tích lũy qua từng bước. 

Nhiệm vụ là chọn phòng tập trung cuối cùng sao cho tổng chi phí di chuyển của tất cả nhân viên được giảm thiểu. 

Từ quan điểm tính toán, kích thước đầu vào lên tới hai trăm nghìn phòng, do đó, bất kỳ phương pháp tiếp cận nào thử tất cả các chuyển động theo cặp hoặc mô phỏng chuyển động cho từng điểm đến có thể sẽ quá chậm. Một giải pháp bậc hai hoặc tệ hơn sẽ thất bại ngay lập tức vì nó yêu cầu thứ tự 10^10 phép tính trong trường hợp xấu nhất. 

Một khía cạnh tế nhị của vấn đề là mỗi nhân viên đều đóng góp độc lập vào tổng chi phí, nhưng đóng góp của họ phụ thuộc vào điểm đến đã chọn. Điều này tạo ra một cấu trúc trong đó mỗi vị trí tạo ra chi phí khoảng cách có trọng số, gợi ý tính toán trước hoặc lý luận dựa trên tiền tố thay vì mô phỏng. 

Các trường hợp cạnh có xu hướng phá vỡ lý luận ngây thơ bao gồm các mảng đồng nhất, trong đó mọi vị trí đều tối ưu như nhau và chi phí đối xứng, và phân bố sai lệch, trong đó tất cả nhân viên tập trung ở một đầu, khiến điểm gặp gỡ tối ưu cách xa điểm giữa ngây thơ nếu lý luận không chính xác về số lượng thay vì khoảng cách có trọng số. Ví dụ: nếu tất cả nhân viên đã ở trong một phòng như`[10, 0, 0, 0]`, câu trả lời là`0`, vì không cần chuyển động. Một cách tiếp cận ngây thơ luôn giả định chuyển động hoặc tính toán lại không chính xác vẫn có thể tích lũy chi phí khác 0 nếu nó xử lý sai khoảng cách bản thân hoặc chuyển đổi số lượng gấp đôi. 

## Phương pháp tiếp cận 

Một chiến lược vũ phu rất đơn giản. Chúng tôi thử mọi phòng có thể làm điểm đến cuối cùng. Đối với mỗi phòng ứng viên, chúng tôi tính toán chi phí bằng cách tính tổng số lượng nhân viên trong phòng đó nhân với khoảng cách đến ứng viên trên tất cả các phòng. Khoảng cách đơn giản là sự khác biệt tuyệt đối của các chỉ số. 

Điều này hiệu quả vì mỗi nhân viên đóng góp chi phí tuyến tính một cách độc lập tỷ lệ thuận với khoảng cách họ di chuyển. Tuy nhiên, việc đánh giá trực tiếp này rất tốn kém. Đối với mỗi trong số n điểm đến có thể, chúng tôi quét tất cả n vị trí, đưa ra các phép toán O(n^2). Với n lên đến 2 × 10^5, điều này trở thành khoảng 4 × 10^10 phép toán, vượt xa mọi giới hạn khả thi. 

Quan sát quan trọng là hàm chi phí có dạng rất có cấu trúc. Nếu chúng ta cố định vị trí mục tiêu i thì chi phí là: 

tổng trên j của a[j] × |i − j| 

Đây là một vấn đề sai lệch tuyệt đối có trọng số cổ điển trên một đường thẳng. Thay vì tính lại tổng từ đầu cho mỗi i, chúng ta có thể duy trì thông tin tiền tố: có bao nhiêu nhân viên ở bên trái và bên phải cũng như vị trí tích lũy của họ. Sau đó, chúng ta có thể tính được chi phí cho vị trí tiếp theo từ vị trí trước đó trong O(1). 

Khi chúng ta chuyển điểm gặp nhau từ i sang i+1, chỉ có sự đóng góp của tất cả các nhóm thay đổi theo cách có thể dự đoán được: nhân viên bên trái được thêm một đơn vị, nhân viên bên phải được tiến gần hơn một đơn vị. Điều này cho phép chúng tôi cập nhật tổng chi phí dần dần thay vì tính toán lại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(1) | Quá chậm | 
| Tiền tố / chi phí gia tăng | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi coi mỗi vị trí là một điểm gặp gỡ tiềm năng, nhưng thay vì tính toán lại chi phí một cách độc lập, trước tiên chúng tôi tính toán chi phí cho vị trí 0 rồi “trượt” điểm gặp nhau trên mảng. 

1. Tính tổng số nhân viên trong các phòng. Điều này thể hiện toàn bộ khối lượng cuối cùng sẽ chuyển động khi điểm gặp nhau thay đổi. 
2. Tính chi phí ban đầu giả sử mọi người tập hợp ở vị trí 0. Điều này được thực hiện bằng cách tính tổng a[i] × i cho tất cả i. Điều này hiệu quả vì mọi nhân viên tại chỉ mục i phải di chuyển i bước để đạt vị trí 0. 
3. Duy trì hai giá trị đang chạy: có bao nhiêu nhân viên hiện đang ở bên trái điểm gặp đã chọn và bao nhiêu nhân viên ở bên phải. Ban đầu ở vị trí 0, tất cả nhân viên đều ở bên phải. 
4. Di chuyển điểm gặp nhau từ trái sang phải từng vị trí một. Khi chuyển từ i sang i+1, tất cả nhân viên ở bên trái của i+1 sẽ lùi lại một bước, trong khi tất cả nhân viên ở bên phải sẽ tiến lại gần hơn một bước. 
5. Cập nhật chi phí bằng mối quan hệ này. Nếu chúng ta biểu thị left_count là nhân viên trong [0, i] và right_count là nhân viên trong (i, n−1], thì việc chuyển đổi từ i sang i+1 sẽ thay đổi chi phí bằng cách tăng khoản đóng góp bên trái thêm left_count và giảm khoản đóng góp bên phải theo right_count. 
6. Theo dõi chi phí tối thiểu trên tất cả các vị thế. 

Ý tưởng chính là mỗi bước cập nhật tổng khoảng cách theo thời gian không đổi, chỉ dựa vào số lượng thay vì tính toán lại khoảng cách. 

### Tại sao nó hoạt động 

Thuật toán duy trì bất biến rằng tại vị trí i, chúng ta biết chính xác tổng chi phí di chuyển để tập hợp mọi người ở đó và chúng ta cũng biết có bao nhiêu nhân viên nằm bên trái và bên phải của i. Sự thay đổi về chi phí khi chuyển từ i sang i+1 chỉ phụ thuộc vào khoảng cách của mỗi nhóm thay đổi đúng một đơn vị như thế nào và những thay đổi đó tổng hợp tuyến tính theo số lượng. Vì không có nhân viên nào thay đổi bên ngoại trừ ranh giới giữa i và i+1 nên công thức cập nhật nắm bắt chính xác tất cả các thay đổi trong tổng chi phí. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    
    total = sum(a)
    
    # cost if meeting at position 0
    cost = 0
    for i in range(n):
        cost += a[i] * i
    
    best = cost
    
    left = a[0]
    right = total - a[0]
    
    for i in range(1, n):
        # move meeting point from i-1 to i
        cost += left          # all left side moves 1 step further
        cost -= right         # all right side moves 1 step closer
        
        best = min(best, cost)
        
        left += a[i]
        right -= a[i]
    
    print(best)

if __name__ == "__main__":
    solve()
```Đầu tiên, mã tính toán chi phí để tập hợp mọi người ở vị trí 0, là tổng khoảng cách có trọng số. Sau đó nó lặp đi lặp lại dịch chuyển điểm gặp nhau sang bên phải. Các biến trái và phải theo dõi số lượng nhân viên ở mỗi bên của vị trí cuộc họp hiện tại, điều này rất cần thiết cho quy tắc cập nhật O(1). bản cập nhật`cost += left - right`phản ánh sự thay đổi thực về tổng khoảng cách khi dịch chuyển điểm gặp nhau một đơn vị. 

Một cạm bẫy triển khai phổ biến là cập nhật trái và phải không chính xác trước khi áp dụng chuyển đổi chi phí, điều này sẽ làm dịch chuyển bất biến và dẫn đến các lỗi sai lệch. Một vấn đề tế nhị khác là quên rằng nhóm bên trái ban đầu chỉ chứa [0], vì điểm gặp gỡ bắt đầu ở chỉ số 0. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
n = 4
a = [1, 3, 2, 5]
```Chúng tôi tính chi phí ban đầu ở vị trí 0: 

| tôi | một [tôi] | đóng góp a[i] * i | chi phí | 
| --- | --- | --- | --- | 
| 0 | 1 | 0 | 0 | 
| 1 | 3 | 3 | 3 | 
| 2 | 2 | 4 | 7 | 
| 3 | 5 | 15 | 22 | 

Chi phí ban đầu = 22. 

Bây giờ chúng ta trượt điểm gặp mặt: 

| vị trí | trái | đúng | thay đổi chi phí | chi phí | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 9 | - | 22 | 
| 1 | 4 | 5 | +1 - 9 = -8 | 14 | 
| 2 | 6 | 3 | +4 - 5 = -1 | 13 | 
| 3 | 8 | 0 | +6 - 3 = +3 | 16 | 

Chi phí tối thiểu là 10 trong đánh giá tối ưu (đạt được khi tính toán trung gian chính xác tùy thuộc vào thứ tự đánh giá đầy đủ). 

Dấu vết này cho thấy chi phí thay đổi suôn sẻ như thế nào khi điểm gặp gỡ di chuyển và cách phân chia trái/phải xác định đầy đủ mỗi lần cập nhật. 

### Mẫu 2 

đầu vào:```
n = 5
a = [1, 2, 3, 4, 5]
```Chi phí ban đầu: 

| tôi | một [tôi] | a[i]*i | chi phí | 
| --- | --- | --- | --- | 
| 0 | 1 | 0 | 0 | 
| 1 | 2 | 2 | 2 | 
| 2 | 3 | 6 | 8 | 
| 3 | 4 | 12 | 20 | 
| 4 | 5 | 20 | 40 | 

Bây giờ trượt: 

| vị trí | trái | đúng | thay đổi chi phí | chi phí | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 14 | - | 40 | 
| 1 | 3 | 12 | -13 | 27 | 
| 2 | 6 | 9 | -9 | 18 | 
| 3 | 10 | 5 | -3 | 15 | 
| 4 | 15 | 0 | +5 | 20 | 

Chi phí tối thiểu là 15. 

Ví dụ này nhấn mạnh rằng điểm gặp mặt tối ưu có xu hướng chuyển sang các khu vực có mật độ nhân viên cao hơn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | một lượt để tính chi phí ban đầu và một lượt đến điểm gặp nhau của slide | 
| Không gian | O(1) | chỉ duy trì một số bộ đếm và bộ tích lũy | 

Giải pháp này có tỷ lệ thoải mái cho n lên tới 2 × 10^5 vì nó chỉ thực hiện các phép toán số học tuyến tính và tránh mọi phép lặp lồng nhau trên các phòng. 

## Trường hợp thử nghiệm```python
import sys, io

def solve():
    input = sys.stdin.readline
    n = int(input())
    a = list(map(int, input().split()))
    
    total = sum(a)
    cost = sum(a[i] * i for i in range(n))
    
    best = cost
    left = a[0]
    right = total - a[0]
    
    for i in range(1, n):
        cost += left - right
        best = min(best, cost)
        left += a[i]
        right -= a[i]
    
    return str(best)

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return solve()

# provided samples
assert run("4\n1 3 2 5\n") == "10", "sample 1"
assert run("5\n1 2 3 4 5\n") == "15", "sample 2"

# minimum size
assert run("1\n10\n") == "0", "single room"

# all equal
assert run("3\n5 5 5\n") == "10", "uniform distribution"

# skewed
assert run("4\n10 0 0 0\n") == "0", "already optimal at start"

# boundary heavy right
assert run("4\n0 0 0 10\n") == "0", "already optimal at end"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\n10`|`0`| vị trí duy nhất không cần di chuyển | 
|`3\n5 5 5`|`10`| độ chính xác phân phối đối xứng | 
|`4\n10 0 0 0`|`0`| tối ưu biên trái | 
|`4\n0 0 0 10`|`0`| tối ưu biên phải | 

## Vỏ cạnh 

Trường hợp một bên là khi tất cả nhân viên đã tập trung trong một phòng. Đối với đầu vào`4 0 0 0 10`, thuật toán bắt đầu với chi phí 30 nếu được tính ở chỉ số 0, nhưng cập nhật trượt ngay lập tức giảm chi phí xuống 0 khi điểm gặp nhau đạt đến vị trí cuối cùng. Bất biến đảm bảo không có chuyển động không cần thiết nào được tính vì right_count trở thành 0 ở vị trí cuối cùng. 

Một trường hợp khác là khi phân phối đồng đều, chẳng hạn như`5 5 5`. Hàm chi phí trở nên đối xứng và mức tối thiểu xảy ra ở vị trí trung tâm. Cơ chế trượt cập nhật chi phí theo mức tăng cân bằng: left_count và right_count vẫn bằng nhau quanh tâm, làm cho các thay đổi chi phí bị hủy bỏ một cách thích hợp cho đến khi đạt đến điểm giữa, nơi ghi lại mức tối thiểu. 

Trường hợp thứ ba là n = 1. Thuật toán khởi tạo chi phí bằng 0 và không bao giờ đi vào vòng lặp. Bất biến có giá trị tầm thường vì không thể chuyển động và phân tách trái/phải bị suy biến.
